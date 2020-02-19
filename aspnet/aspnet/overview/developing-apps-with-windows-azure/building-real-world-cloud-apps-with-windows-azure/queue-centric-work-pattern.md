---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Wzorzec pracy skoncentrowanej na kolejkach (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 1177336b25479c06706227e5c8ff4d027cdaebb8
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456988"
---
# <a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Wzorzec pracy skoncentrowanej na kolejkach (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Wcześniej dodaliśmy, że korzystanie z wielu usług może skutkować umową SLA "złożona", w której obowiązująca umowa SLA aplikacji jest *produktem* poszczególnych umowy SLA. Na przykład aplikacja do rozwiązywania problemów używa witryn sieci Web, magazynu i SQL Database. Jeśli jakakolwiek z tych usług ulegnie awarii, aplikacja zwróci błąd do użytkownika.

Buforowanie jest dobrym sposobem obsługi błędów przejściowych dla zawartości tylko do odczytu. Ale co zrobić, jeśli aplikacja wymaga wykonania pracy? Na przykład gdy użytkownik prześle nowe zadanie poprawki IT, aplikacja nie może po prostu umieścić zadania w pamięci podręcznej. Aplikacja musi napisać to zadanie Fix it do trwałego magazynu danych, aby można było je przetworzyć.

Jest to miejsce, w którym znajduje się wzorzec pracy skoncentrowanej na kolejce. Ten wzorzec umożliwia swobodne sprzęganie między warstwą sieci Web i usługą zaplecza.

Oto jak działa wzorzec. Gdy aplikacja uzyskuje żądanie, umieszcza element roboczy w kolejce i natychmiast zwraca odpowiedź. Następnie oddzielny proces zaplecza Pobiera elementy robocze z kolejki i wykonuje prace.

Wzorzec pracy skoncentrowanej na kolejce jest przydatny dla:

- Pracy, która jest czasochłonna (duże opóźnienie).
- Działanie, które wymaga usługi zewnętrznej, która może być niezawsze dostępna.
- Pracuj intensywnie korzystające z zasobów (High CPU).
- Działa, które byłyby korzystne w przypadku bilansowania (z uwzględnieniem nagłych obciążeń obciążenia).

## <a name="reduced-latency"></a>Ograniczone opóźnienie

Kolejki są przydatne podczas czasochłonnych zadań. Jeśli zadanie trwa kilka sekund lub dłużej, zamiast blokować użytkownika końcowego, umieść element roboczy w kolejce. Poinformuj użytkownika o tym, że pracujemy nad nim, a następnie przetwórz zadanie w tle za pomocą odbiornika kolejki.

Jeśli na przykład kupisz coś w sprzedaży detalicznej, witryna sieci Web natychmiast potwierdzi zamówienie. Ale nie oznacza to, że Twoje rzeczy są już w trakcie dostarczania samochodów. Umieszczają zadania w kolejce, a w tle przeprowadzają kontrolę kredytową, przygotowując elementy do wysyłki i tak dalej.

W przypadku scenariuszy z krótkim opóźnieniem całkowity czas od końca do końca może już być używany w kolejce w porównaniu z wykonywaniem zadania synchronicznie. Jednak nawet inne korzyści mogą przeważyć tę wadą.

## <a name="increased-reliability"></a>Zwiększona niezawodność

W wersji poprawki, która została już przeszukana, fronton sieci Web jest ściśle połączony z zaplecem SQL Database. Jeśli usługa SQL Database jest niedostępna, użytkownik otrzymuje błąd. Jeśli ponawianie próby nie zadziała (oznacza to, że awaria jest większa niż przejściowa), jedyną czynnością, którą można zrobić, jest wyświetlenie błędu i poproszenie użytkownika o próbę ponowienie próby później.

![Diagram przedstawiający niepowodzenie frontonu sieci Web w przypadku niepowodzenia SQL Database zaplecza](queue-centric-work-pattern/_static/image1.png)

Przy użyciu kolejek, gdy użytkownik przesyła zadanie poprawki IT, aplikacja zapisuje komunikat do kolejki. Ładunek wiadomości jest reprezentacją [JSON](http://json.org/) zadania. Natychmiast po zapisaniu komunikatu w kolejce aplikacja zwraca i natychmiast wyświetla komunikat o powodzeniu dla użytkownika.

Jeśli którykolwiek z usług zaplecza, takich jak baza danych SQL lub odbiornik kolejki — przejdź do trybu offline, użytkownicy nadal będą mogli przesłać nowe zadania poprawki. Komunikaty będą po prostu umieszczane w kolejce do momentu ponownego udostępnienia usług zaplecza. W tym momencie usługi zaplecza będą przechwycić w zaległości.

![Diagram przedstawiający funkcję frontonu internetowego kontynuuje działanie w przypadku wystąpienia błędu SQL Database](queue-centric-work-pattern/_static/image2.png)

Ponadto teraz można dodać więcej logiki zaplecza bez konieczności odporności frontonu. Na przykład możesz chcieć wysłać wiadomość e-mail lub wiadomości SMS do właściciela po każdym przypisaniu nowej poprawki. Jeśli poczta e-mail lub usługa programu SMS stanie się niedostępna, możesz przetwarzać wszystko inne, a następnie umieścić komunikat w osobnej kolejce do wysyłania wiadomości e-mail/SMS.

Wcześniej nasza obowiązująca umowa SLA była Web Apps &times; magazynu &times; SQL Database = 99,7%. (Zobacz [projekt, aby przetrwać awarie](design-to-survive-failures.md)).

Gdy zmienimy aplikację w taki sposób, aby korzystała z kolejki, fronton sieci Web będzie zależny od Web Apps i magazynu dla złożonej umowy SLA 99,8%. (Należy zauważyć, że kolejki są częścią usługi Azure Storage, dlatego są uwzględnione w tej samej umowie SLA co magazyn obiektów BLOB).

Jeśli potrzebujesz jeszcze lepszego niż 99,8%, możesz utworzyć dwie kolejki w dwóch różnych regionach. Wyznacz jeden jako podstawowy, a drugi jako pomocniczy. Jeśli kolejka główna jest niedostępna w aplikacji, przełącz się do kolejki pomocniczej. Prawdopodobieństwo, że oba są niedostępne w tym samym czasie, jest bardzo mały.

## <a name="rate-leveling-and-independent-scaling"></a>Skalowanie na poziomie i niezależność

Kolejki są również przydatne w przypadku czegoś *o nazwie lub* *wyrównywania obciążeń*.

Aplikacje sieci Web są często podatne na nagłe rozerwania ruchu. Funkcja automatycznego skalowania umożliwia automatyczne dodawanie serwerów sieci Web w celu obsługi zwiększonego ruchu w sieci Web, jednak skalowanie automatyczna może nie być w stanie szybko reagować w celu obsługi nieoczekiwanych obciążeń. Jeśli serwery sieci Web mogą odciążać część pracy, którą trzeba wykonać, pisząc komunikat do kolejki, może obsłużyć więcej ruchu. Usługa zaplecza może następnie odczytywać komunikaty z kolejki i przetwarzać je. Głębokość kolejki zostanie powiększana lub zmniejszana, gdy obciążenie przychodzące będzie się różnić.

W przypadku wielu czasochłonnych zadań załadowanych do usługi zaplecza warstwa sieci Web może łatwiej reagować na nagłe skoki ruchu. Można zaoszczędzić pieniądze, ponieważ każdy ruch może być obsługiwany przez mniejszą liczbę serwerów sieci Web.

Warstwę sieci Web i usługę zaplecza można skalować niezależnie. Na przykład mogą być potrzebne trzy serwery sieci Web, ale tylko jeden komunikat kolejki przetwarzania serwera. Lub jeśli uruchamiasz zadanie intensywnie korzystające z obliczeń w tle, może być konieczne zwiększenie liczby serwerów zaplecza.

![](queue-centric-work-pattern/_static/image3.png)

Skalowanie automatyczne współpracuje z usługami zaplecza oraz z warstwą sieci Web. Można skalować w górę lub w dół liczbę maszyn wirtualnych, które przetwarzają zadania w kolejce, w oparciu o użycie procesora na maszynach wirtualnych zaplecza. Możesz też automatycznie skalować w zależności od liczby elementów znajdujących się w kolejce. Na przykład możesz powiedzieć automatyczne skalowanie, aby nie przechowywać więcej niż 10 elementów w kolejce. Jeśli kolejka ma więcej niż 10 elementów, automatyczne skalowanie doda maszyny wirtualne. Po przechwyceniu automatyczne skalowanie spowoduje rozbicie dodatkowych maszyn wirtualnych.

## <a name="adding-queues-to-the-fix-it-application"></a>Dodawanie kolejek do aplikacji poprawki IT

Aby zaimplementować wzorzec kolejki, musimy wprowadzić dwie zmiany w aplikacji poprawki IT.

- Gdy użytkownik przesyła nowe zadanie poprawki IT, należy umieścić zadanie w kolejce, zamiast zapisywać je w bazie danych.
- Utwórz usługę zaplecza, która przetwarza wiadomości w kolejce.

W przypadku kolejki będziemy używać [usługi Azure queue storage](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Innym rozwiązaniem jest użycie [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Aby określić, która usługa kolejki ma być używana, należy wziąć pod uwagę, w jaki sposób aplikacja musi wysyłać i odbierać komunikaty w kolejce:

- Jeśli masz producentów współpracujących i konkurujących klientów, rozważ użycie usługi Azure Queue Storage. "Producenci współpracująci" to wiele procesów, które dodają komunikaty do kolejki. "Konkurujący konsumenci" to wiele procesów ściągających komunikaty z kolejki w celu ich przetworzenia, ale każdy komunikat może być przetwarzany tylko przez jeden "konsument". Jeśli potrzebujesz większej przepływności niż możesz uzyskać dostęp do jednej kolejki, użyj dodatkowych kolejek i/lub dodatkowych kont magazynu.
- Jeśli potrzebujesz [modelu publikowania/subskrybowania](http://en.wikipedia.org/wiki/Publish/subscribe), rozważ użycie kolejek Azure Service Bus.

Poprawka aplikacji IT pasuje do współpracujących producentów i modelu konkurujących odbiorców.

Inną kwestią jest dostępność aplikacji. Usługa Queue Storage jest częścią tej samej usługi, która jest używana w usłudze BLOB Storage, dzięki czemu nie ma wpływu na naszą umowę SLA. Azure Service Bus jest oddzielną usługą z własną umową SLA. Jeśli korzystamy z kolejek Service Bus, będziemy musieli wziąć udział w dodatkowej umowie SLA, a nasza umowa SLA będzie niższa. Po wybraniu usługi kolejek upewnij się, że rozumiesz wpływ wyboru na dostępność aplikacji. Aby uzyskać więcej informacji, zobacz sekcję [resources](#resources) .

## <a name="creating-queue-messages"></a>Tworzenie komunikatów w kolejce

Aby można było wykonać zadanie poprawki IT w kolejce, fronton sieci Web wykonuje następujące czynności:

1. Utwórz wystąpienie [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) . Wystąpienie `CloudQueueClient` jest używane do wykonywania żądań względem usługi kolejki.
2. Utwórz kolejkę, jeśli jeszcze nie istnieje.
3. Serializacja zadania Fix it.
4. Wywołaj [CloudQueue. AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) , aby umieścić komunikat w kolejce.

Wykonamy tę prace w konstruktorze i `SendMessageAsync` metodzie nowej klasy `FixItQueueManager`.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

W tym miejscu korzystamy z biblioteki [JSON.NET](https://github.com/JamesNK/Newtonsoft.Json) do serializacji fixit do formatu JSON. Możesz użyć dowolnego preferowanego podejścia serializacji. Usługa JSON ma zalety odczytywania danych przez człowieka, ale jest mniej pełnych niż język XML.

Kod jakości produkcji mógłby dodać logikę obsługi błędów, wstrzymać, jeśli baza danych stanie się niedostępna, obsłużyć odzyskiwanie bardziej czyste, utworzyć kolejkę przy uruchamianiu aplikacji i zarządzać[komunikatami "trujące"](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Trująca wiadomość jest komunikatem, którego nie można przetworzyć z jakiegoś powodu. Nie chcesz, aby trujące komunikaty były umieszczane w kolejce, w której rola proces roboczy będzie stale próbować je przetworzyć, Niepowodzenie, spróbuj ponownie, zakończył się niepowodzeniem itd.)

W aplikacji MVC frontonu musimy zaktualizować kod, który tworzy nowe zadanie. Zamiast przełączać zadanie do repozytorium, wywołaj metodę `SendMessageAsync` pokazaną powyżej.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Przetwarzanie komunikatów w kolejce

Aby przetwarzać komunikaty w kolejce, utworzymy usługę zaplecza. Usługa zaplecza uruchomi nieskończoną pętlę, która wykonuje następujące czynności:

1. Pobierz następną wiadomość z kolejki.
2. Deserializacja komunikatu do zadania poprawki IT.
3. Napisz zadanie Fix it do bazy danych.

Aby hostować usługę zaplecza, utworzymy usługę w chmurze platformy Azure, która zawiera *rolę procesu roboczego*. Rola procesu roboczego składa się z co najmniej jednej maszyny wirtualnej, która może przeprowadzić przetwarzanie zaplecza. Kod, który jest uruchamiany w tych maszynach wirtualnych, będzie ściągał komunikaty z kolejki, gdy staną się dostępne. Dla każdego komunikatu będziemy deserializować ładunek JSON i napisać wystąpienie obiektu Fix it do bazy danych przy użyciu tego samego repozytorium, które zostało użyte wcześniej w warstwie sieci Web.

Poniższe kroki pokazują, jak dodać projekt roli procesu roboczego do rozwiązania, które ma standardowy projekt sieci Web. Te kroki zostały już wykonane w projekcie poprawki IT, który można pobrać.

Najpierw Dodaj projekt usługi w chmurze do rozwiązania programu Visual Studio. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz polecenie **Dodaj**, a następnie pozycję **Nowy projekt**. W okienku po lewej stronie rozwiń **pozycję C# Wizualizacja** i wybierz pozycję **chmura**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

W oknie dialogowym **Nowa usługa w chmurze Azure** rozwiń węzeł **wizualizacji C#**  w okienku po lewej stronie. Wybierz **rolę proces roboczy** , a następnie kliknij ikonę strzałki w prawo.

![](queue-centric-work-pattern/_static/image6.png)

(Należy zauważyć, że można również dodać *rolę sieci Web*. W tej samej usłudze w chmurze zamiast uruchamiania jej w witrynie sieci Web systemu Azure można uruchomić ten sam fronton. Ma pewne zalety, aby ułatwić koordynację połączeń między frontonem a zapleczem. Aby jednak zapewnić prostotę tego pokazu, utrzymujemy fronton w aplikacji internetowej Azure App Service i uruchomiono tylko zaplecze w usłudze w chmurze.)

Nazwa domyślna jest przypisana do roli procesu roboczego. Aby zmienić nazwę, umieść kursor myszy nad rolą proces roboczy w prawym okienku, a następnie kliknij ikonę ołówka.

![](queue-centric-work-pattern/_static/image7.png)

Kliknij przycisk **OK** , aby zakończyć okno dialogowe. Spowoduje to dodanie dwóch projektów do rozwiązania programu Visual Studio.

- projekt platformy Azure, który definiuje usługę w chmurze, w tym informacje o konfiguracji.
- Projekt roli procesu roboczego, który definiuje rolę procesu roboczego.

![](queue-centric-work-pattern/_static/image8.png)

Aby uzyskać więcej informacji, zobacz [Tworzenie projektu platformy Azure za pomocą programu Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

W ramach roli procesu roboczego sonduje komunikaty, wywołując metodę `ProcessMessageAsync` klasy `FixItQueueManager`, która została wcześniej wykorzystana.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Metoda `ProcessMessagesAsync` sprawdza, czy istnieje komunikat oczekujący. Jeśli istnieje, deserializacji komunikat do jednostki `FixItTask` i zapisuje jednostkę w bazie danych. Pętle do momentu, gdy kolejka jest pusta.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Sondowanie w poszukiwaniu komunikatów w kolejce powoduje niewielką opłatą za transakcje, więc gdy nie ma oczekujących na przetworzenie komunikatu, Metoda `RunAsync` roli procesu roboczego czeka sekundę przed ponownym sondowaniem, wywołując `Task.Delay(1000)`.

W projekcie sieci Web Dodawanie kodu asynchronicznego może automatycznie zwiększyć wydajność, ponieważ program IIS zarządza ograniczoną pulą wątków. To nie jest przypadek w projekcie roli procesu roboczego. Aby zwiększyć skalowalność roli proces roboczy, można napisać kod wielowątkowy lub użyć kodu asynchronicznego do wdrożenia [programowania równoległego](https://msdn.microsoft.com/library/ff963553.aspx). Przykład nie implementuje programowania równoległego, ale pokazuje, w jaki sposób kod powinien być asynchroniczny, aby można było zaimplementować programowanie równoległe.

## <a name="summary"></a>Podsumowanie

W tym rozdziale pokazano, jak poprawić czas reakcji, niezawodność i skalowalność aplikacji, implementując wzorzec pracy skoncentrowanej na kolejkach.

Jest to ostatnie 13 wzorców objętych tą książką elektroniczną, ale istnieje wiele innych wzorców i rozwiązań, które mogą pomóc w tworzeniu udanych aplikacji w chmurze. [Rozdział końcowy](more-patterns-and-guidance.md) zawiera linki do zasobów dla tematów, które nie zostały omówione w tych 13 wzorcach.

<a id="resources"></a>
## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji o kolejkach, zobacz następujące zasoby.

Łączoną

- [Kolejki Microsoft Azure Storage część 1: wprowadzenie](https://www.red-gate.com/simple-talk/cloud/platform-as-a-service/microsoft-azure-storage-queues-part-1-getting-started/). Artykuł Schacherl Roman.
- [Wykonywanie zadań w tle](https://msdn.microsoft.com/library/ff803365.aspx), rozdział 5 [przenoszonych aplikacji do chmury, trzecia wersja](https://msdn.microsoft.com/library/ff728592.aspx) z wzorców i praktyk firmy Microsoft. (W szczególności Sekcja ["Korzystanie z kolejek usługi Azure Storage"](https://msdn.microsoft.com/library/ff803365.aspx#sec7)).
- [Najlepsze rozwiązania dotyczące maksymalizowania skalowalności i opłacalności rozwiązań do obsługi komunikatów opartych na kolejkach na platformie Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Oficjalny dokument przez Valery Mizonov.
- [Porównywanie kolejek platformy Azure i kolejek Service Bus](https://msdn.microsoft.com/magazine/jj159884.aspx). Artykuł Magazyn MSDN zawiera dodatkowe informacje, które mogą pomóc wybrać usługę kolejki do użycia. Artykuł zawiera informacje o tym, że Service Bus jest zależne od usługi ACS do uwierzytelniania, co oznacza, że kolejki SB byłyby niedostępne, gdy Usługa ACS jest niedostępna. Jednak ze względu na to, że artykuł został zapisany, SB został zmieniony, aby umożliwić korzystanie z [tokenów SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) jako alternatywy dla usługi ACS.
- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zapoznaj się z wzorcem obsługi komunikatów asynchronicznych, wzorca potoków i filtrów, wzorcem transakcji kompensacyjnej, wzorcem konkurujących odbiorców, wzorcem CQRS.
- [CQRS](https://msdn.microsoft.com/library/jj554200). Książka elektroniczna dotycząca CQRS według wzorców i praktyk firmy Microsoft.

Wideo:

- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria wideo dziewięć części przez Ulrich Homann, Marc Mercuri i marking SIMM. Prezentuje koncepcje wysokiego poziomu i zasady architektury w bardzo dostępnym i interesującym scenariuszu, w tym scenariusze opracowane przez firmę Microsoft Customer Advisory Team (CAT) z rzeczywistymi klientami. Aby zapoznać się z wprowadzeniem do usługi Azure Storage i kolejek, zobacz epizod 5 od 35:13.

> [!div class="step-by-step"]
> [Poprzednie](distributed-caching.md)
> [dalej](more-patterns-and-guidance.md)

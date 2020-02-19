---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Najlepsze praktyki dotyczące programowania w sieci Web (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: dfd8a3ac2328d3f17dfbe36e68b37d181177b0f4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457092"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Najlepsze praktyki dotyczące programowania w sieci Web (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Pierwsze trzy wzorce miały na celu skonfigurowanie procesu programowania Agile. Pozostałe informacje dotyczą architektury i kodu. Jest to zbiór najlepszych rozwiązań dotyczących programowania w sieci Web:

- [Bezstanowe serwery sieci Web](#stateless) za pomocą inteligentnego modułu równoważenia obciążenia.
- [Należy unikać stanu sesji](#sessionstate) (lub jeśli nie można tego uniknąć, należy użyć rozproszonej pamięci podręcznej, a nie bazy danych).
- [Użyj sieci CDN](#cdn) do statycznych zasobów plików (obrazów, skryptów) w pamięci podręcznej usługi Edge.
- [Używaj asynchronicznej obsługi platformy .NET 4.5](#async) , aby uniknąć blokowania wywołań.

Te praktyki są prawidłowe dla całego programowania w sieci Web, a nie tylko dla aplikacji w chmurze, ale są szczególnie ważne w przypadku aplikacji w chmurze. Współpracują ze sobą, aby ułatwić optymalne wykorzystanie wysoce elastycznego skalowania oferowanego przez środowisko chmury. Jeśli nie korzystasz z tych zasad, podczas próby skalowania aplikacji będziesz mieć ograniczenia.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Bezstanowa warstwa internetowa za pomocą inteligentnego modułu równoważenia obciążenia

*Bezstanowa warstwa sieci Web* oznacza, że nie są przechowywane żadne dane aplikacji w pamięci serwera sieci Web lub w systemie plików. Pozostawienie warstwy sieci Web bezstanowej umożliwia zarówno lepsze środowisko pracy klienta, jak i oszczędność pieniędzy:

- Jeśli warstwa sieci Web jest bezstanowa i znajduje się za modułem równoważenia obciążenia, możesz szybko reagować na zmiany w ruchu aplikacji przez dynamiczne dodawanie lub usuwanie serwerów. W środowisku chmury, w przypadku których płacisz tylko za zasoby serwera, tak długo, jak faktycznie są używane, możliwość reagowania na zmiany popytu może przełożyć na ogromne oszczędności.
- Bezstanowa warstwa internetowa jest znacznie prostsza do skalowania aplikacji. Pozwala to na szybsze reagowanie na skalowanie, a także poświęcanie mniejszej nakładu na rozwój i testowanie w procesie.
- Serwery w chmurze, takie jak serwery lokalne, muszą być poprawiane i uruchamiane ponownie sporadycznie; a jeśli warstwa sieci Web jest bezstanowa, ponowne kierowanie ruchem po tymczasowym przekroczeniu serwera nie spowoduje błędów lub nieoczekiwane zachowanie.

Większość aplikacji w świecie musi przechowywać stan dla sesji sieci Web; głównym punktem nie jest przechowywanie go na serwerze sieci Web. Można przechowywać stan w inny sposób, na przykład na kliencie w plikach cookie lub poza procesami w ASP.NET stan sesji przy użyciu dostawcy pamięci podręcznej. Pliki można przechowywać w [magazynie obiektów BLOB systemu Windows Azure](unstructured-blob-storage.md) zamiast w lokalnym systemie plików.

Przykładowo, jak łatwo jest skalować aplikację w witrynach sieci Web systemu Windows Azure, jeśli warstwa sieci Web jest bezstanowa, zobacz kartę **Skala** dla witryny sieci Web systemu Windows Azure w portalu zarządzania:

![Karta Skala](web-development-best-practices/_static/image1.png)

Aby dodać serwery sieci Web, można po prostu przeciągnąć suwak liczby wystąpień w prawo. Ustaw ją na 5 i kliknij przycisk **Zapisz**, a w ciągu kilku sekund masz 5 serwerów sieci Web w systemie Windows Azure obsługujących ruch witryny sieci Web.

![Pięć wystąpień](web-development-best-practices/_static/image2.png)

Można równie łatwo ustawić liczbę wystąpień w dół do 3 lub z powrotem na 1. Po skalowaniu z powrotem należy natychmiast zacząć zaoszczędzić pieniądze, ponieważ system Windows Azure obciąża minutę, a nie godzinę.

Możesz również poinformować platformę Microsoft Azure, aby automatycznie zwiększać lub zmniejszać liczbę serwerów sieci Web na podstawie użycia procesora CPU. W poniższym przykładzie, gdy użycie procesora CPU spadnie poniżej 60%, liczba serwerów sieci Web obniży się do co najmniej 2. Jeśli użycie procesora CPU spadnie powyżej 80%, liczba serwerów sieci Web zostanie zwiększona do maksymalnie 4.

![Skalowanie według użycia procesora CPU](web-development-best-practices/_static/image3.png)

Lub co zrobić, Jeśli wiesz, że Twoja witryna będzie zajęta tylko w godzinach pracy? Możesz powiedzieć, że platforma Microsoft Azure będzie uruchamiać wiele serwerów w godzinach dziennych i zmniejszać się do jednego serwera nawet, nocy i weekendów. Poniższa seria zrzutów ekranu pokazuje, jak skonfigurować witrynę sieci Web tak, aby uruchamiała jeden serwer w godzinach poza godzinami i 4 serwerami w godzinach pracy od 8 do 5 PM.

![Skalowanie według harmonogramu](web-development-best-practices/_static/image4.png)

![Ustawianie czasów harmonogramu](web-development-best-practices/_static/image5.png)

![Harmonogram dzienny](web-development-best-practices/_static/image6.png)

![Harmonogram weeknight](web-development-best-practices/_static/image7.png)

![Harmonogram weekendowy](web-development-best-practices/_static/image8.png)

I oczywiście wszystkie te czynności można wykonać w skryptach, a także w portalu.

Możliwość skalowania w poziomie aplikacji jest niemal nieograniczona w systemie Windows Azure, o ile pozwala uniknąć przeszkód w dynamicznym dodawaniu lub usuwaniu maszyn wirtualnych na serwerze, utrzymując warstwę sieci Web bez ograniczeń.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Unikaj stanu sesji

Często nie jest to praktyczne w aplikacji w chmurze w rzeczywistości, aby uniknąć przechowywania niektórych form stanu dla sesji użytkownika, ale niektóre podejścia mają wpływ na wydajność i skalowalność więcej niż inne. Jeśli musisz przechowywać stan, najlepszym rozwiązaniem jest zachowanie małego stanu i zapisanie go w plikach cookie. Jeśli to nie jest możliwe, następnym najlepszym rozwiązaniem jest użycie ASP.NET stanu sesji z dostawcą dla [rozproszonej pamięci podręcznej w pamięci](distributed-caching.md#sessionstate). Najgorszym rozwiązaniem z punktu widzenia wydajności i skalowalności jest użycie dostawcy stanu sesji bazy danych.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Używanie sieci CDN do buforowania statycznych zasobów plików

Usługa CDN jest akronimem dla Content Delivery Network. Udostępniasz statyczne zasoby plików, takie jak obrazy i pliki skryptów, do dostawcy usługi CDN, a dostawca buforuje te pliki w centrach danych na całym świecie, dzięki czemu wszędzie tam, gdzie użytkownicy uzyskują dostęp do aplikacji, uzyskują stosunkowo szybką odpowiedź i małe opóźnienia w przypadku pamięci podręcznej stanu. Skraca to całkowity czas ładowania lokacji i zmniejsza obciążenie serwerów sieci Web. Sieci CDN są szczególnie ważne, jeśli docierasz do odbiorców, które są szeroko rozdystrybuowane geograficznie.

System Windows Azure ma sieć CDN i można używać innych sieci CDN w aplikacji działającej w systemie Windows Azure lub dowolnym środowisku hostingu w sieci Web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Używaj asynchronicznej obsługi platformy .NET 4.5, aby uniknąć blokowania wywołań

.NET 4,5 udoskonalone C# Języki programowania i VB w celu znacznie prostszej obsługi zadań. Korzystanie z programowania asynchronicznego nie dotyczy tylko sytuacji przetwarzania równoległego, takich jak w przypadku, gdy chcesz uruchomić jednocześnie wiele wywołań usługi sieci Web. Umożliwia ona również wydajniejsze i niezawodne działanie serwera sieci Web w warunkach dużego obciążenia. Serwer sieci Web ma ograniczoną liczbę dostępnych wątków i w warunkach wysokiego obciążenia, gdy wszystkie wątki są używane, żądania przychodzące muszą oczekiwać do momentu zwolnienia wątków. Jeśli kod aplikacji nie obsługuje zadań, takich jak zapytania bazy danych i wywołania usługi sieci Web asynchroniczne, wiele wątków jest niepotrzebnie powiązane, gdy serwer oczekuje na odpowiedź we/wy. Ogranicza to ilość ruchu, jaki serwer może obsłużyć w warunkach dużego obciążenia. Dzięki programowaniu asynchronicznym wątki, które oczekują na zwrot danych przez usługę sieci Web lub bazę danych, są zwalniane do obsługi nowych żądań do momentu odebrania danych. W przypadku zajętego serwera sieci Web setki lub tysiące żądań można następnie przetwarzać w ten sposób, co w przeciwnym razie będzie oczekiwać na zwolnienie wątków.

Jak już wcześniej się znajdują, można łatwo zmniejszyć liczbę serwerów sieci Web obsługujących witrynę sieci Web w miarę ich zwiększania. Tak więc, jeśli serwer może uzyskać większą przepływność, nie musisz być tyle z nich i możesz obniżyć koszty, ponieważ potrzebujesz mniejszej liczby serwerów dla danego natężenia ruchu sieciowego niż w przeciwnym razie.

Obsługa asynchronicznego modelu programowania .NET 4,5 jest uwzględniona w ASP.NET 4,5 dla formularzy Web Forms, MVC i Web API; w Entity Framework 6 i w [interfejsie API usługi Windows Azure Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Obsługa asynchroniczna w ASP.NET 4,5

W programie ASP.NET 4,5 zostało dodane wsparcie dla programowania asynchronicznego, które nie jest przeznaczone tylko do języka, ale również do struktur interfejsu API MVC, sieci Web i sieci Web. Na przykład Metoda akcji kontrolera MVC ASP.NET odbiera dane z żądania sieci Web i przekazuje dane do widoku, który następnie tworzy kod HTML do wysłania do przeglądarki. Często Metoda działania wymaga pobrania danych z bazy danych lub usługi sieci Web w celu wyświetlenia jej na stronie sieci Web lub zapisania danych wprowadzonych na stronie sieci Web. W tych scenariuszach łatwo jest utworzyć metodę akcji asynchronicznie: zamiast zwracać obiekt *ActionResult* , zwracasz *zadanie&lt;ActionResult&gt;* i oznacz metodę za pomocą słowa kluczowego *Async* . Wewnątrz metody, gdy wiersz kodu zaczyna się na operacji, która obejmuje czas oczekiwania, oznacz go za pomocą słowa kluczowego await.

Oto prosta metoda działania, która wywołuje metodę repozytorium dla zapytania bazy danych:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

A oto ta sama metoda, która obsługuje asynchroniczne wywołanie bazy danych:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

W obszarze okładki kompilator generuje odpowiedni kod asynchroniczny. Gdy aplikacja wywołuje `FindTaskByIdAsync`, ASP.NET wykonuje żądanie `FindTask`, a następnie cofa wątek roboczy i udostępnia go do przetwarzania innego żądania. Po wykonaniu żądania `FindTask` wątek jest uruchamiany ponownie w celu kontynuowania przetwarzania kodu, który jest dostępny po wywołaniu. W okresie przejściowym między zainicjowaniem żądania `FindTask`, gdy dane są zwracane, dostępny jest wątek, który umożliwia wykonywanie użytecznych zadań, które w przeciwnym razie byłyby związane z oczekiwaniem na odpowiedź.

Istnieją pewne koszty związane z kodem asynchronicznym, ale w przypadku warunków o niskim obciążeniu obciążenie jest nieznaczne, a w warunkach dużego obciążenia można przetwarzać żądania, które w przeciwnym razie byłyby w stanie oczekiwać na dostępne wątki.

Możliwe jest wykonanie tego rodzaju programowania asynchronicznego od ASP.NET 1,1, ale trudno było pisać, podatne na błędy i trudne do debugowania. Teraz upraszczamy kodowanie dla IT w ASP.NET 4,5, dlatego nie jest to konieczne.

### <a name="async-support-in-entity-framework-6"></a>Obsługa asynchroniczna w Entity Framework 6

W ramach obsługi asynchronicznej w 4,5 Firma Microsoft otrzymała asynchroniczne wsparcie dla wywołań usługi sieci Web, gniazd i systemu plików we/wy, ale najpopularniejszym wzorcem dla aplikacji sieci Web jest osiągnięcie bazy danych, a nasze biblioteki danych nie obsługują Async. Teraz Entity Framework 6 dodaje obsługę asynchroniczną dostępu do bazy danych.

W Entity Framework 6 wszystkie metody, które powodują wysłanie zapytania lub polecenia do bazy danych, mają wersje asynchroniczne. W tym przykładzie przedstawiono asynchroniczne wersje metody *Find* .

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Ta obsługa asynchronicznych działa nie tylko w przypadku operacji INSERT, usunięć, Update i Simple, ale działa również z kwerendami LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Istnieje `Async` wersja metody `ToList`, ponieważ w tym kodzie jest to metoda, która powoduje wysłanie zapytania do bazy danych. Metody `Where` i `OrderByDescending` konfigurują tylko zapytanie, podczas gdy metoda `ToListAsync` wykonuje zapytanie i zapisuje odpowiedź w zmiennej `result`.

## <a name="summary"></a>Podsumowanie

Najlepsze rozwiązania związane z programowaniem w sieci Web można zaimplementować w dowolnym miejscu w dowolnym środowisku programowania sieci Web i w każdym ze środowisk chmurowych, ale mamy narzędzia w ASP.NET i platformie Microsoft Azure, które ułatwią jego łatwą obsługę. Jeśli skorzystasz z tych wzorców, możesz łatwo skalować warstwę sieci Web i zminimalizować wydatki, ponieważ każdy serwer będzie mógł obsłużyć więcej ruchu.

W [następnym rozdziale](single-sign-on.md) przedstawiono sposób, w jaki chmura włącza scenariusze rejestracji jednokrotnej.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji, zobacz następujące zasoby.

Bezstanowe serwery sieci Web:

- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące skalowania](https://msdn.microsoft.com/library/dn589774.aspx)automatycznego.
- [Wyłączanie koligacji wystąpienia ARR w witrynach sieci Web systemu Windows Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Wpis w blogu według Ereza Benariego, objaśnia koligację sesji w witrynach sieci Web systemu Windows Azure.

CDN:

- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria wideo dziewięć części przez Ulrich Homann, Marc Mercuri i marking SIMM. Zapoznaj się z dyskusjami w usłudze CDN w epizod 3, rozpoczynając od 1:34:00.
- [Wzorzec hostingu zawartości dla wzorców i praktyk firmy Microsoft](https://msdn.microsoft.com/library/dn589776.aspx)
- [Przeglądy sieci CDN](http://www.cdnreviews.com/). Przegląd wielu sieci CDN.

Programowanie asynchroniczne:

- [Używanie metod asynchronicznych w ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Samouczek według Rick Anderson.
- [Programowanie asynchroniczne z Async i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). Oficjalny dokument MSDN, który wyjaśnia uzasadnienie dla programowania asynchronicznego, jak działa w programie ASP.NET 4,5 i jak napisać kod w celu jego wdrożenia.
- [Entity Framework asynchroniczne zapytanie i Zapisz](https://msdn.microsoft.com/data/jj819165)
- [Jak kompilować ASP.NET aplikacje sieci Web przy użyciu usługi asynchronicznej](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Prezentacja wideo według Rowan Miller. Zawiera ilustracja przedstawiająca sposób, w jaki programowanie asynchroniczne może ułatwić znaczący wzrost przepływności serwera sieci Web w warunkach dużego obciążenia.
- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria wideo dziewięć części przez Ulrich Homann, Marc Mercuri i marking SIMM. Aby poznać dyskusje na temat wpływu programowania asynchronicznego na skalowalność, zobacz część epizod 4 i odcinek 8.
- [Magiczna Metoda używania metod asynchronicznych w ASP.NET 4,5 i ważna Gotcha](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Wpis w blogu przez Scott Hanselman, przede wszystkim o używanie Async w aplikacjach formularzy sieci Web ASP.NET.

Aby uzyskać dodatkowe najlepsze rozwiązania dotyczące programowania w sieci Web, zobacz następujące zasoby:

- [Naprawa przykładowej aplikacji — najlepsze rozwiązania](the-fix-it-sample-application.md#bestpractices). Dodatek do tej książki elektronicznej zawiera kilka najlepszych rozwiązań, które zostały zaimplementowane w aplikacji IT.
- [Lista kontrolna dewelopera sieci Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Poprzednie](continuous-integration-and-continuous-delivery.md)
> [dalej](single-sign-on.md)

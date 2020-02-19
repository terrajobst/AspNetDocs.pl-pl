---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Monitorowanie i telemetria (Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f61810ea7b486b2fa0bbb234edea7541eedde835
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457066"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Monitorowanie i telemetria (Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Wiele osób jest zależnych od klientów, którzy wiedzą, kiedy ich aplikacja nie działa. To nie jest najlepszym rozwiązaniem w dowolnym miejscu i w chmurze. Nie ma gwarancji na szybkie powiadamianie, a gdy użytkownik otrzymuje powiadomienia, często otrzymujesz minimalne lub mylące dane dotyczące tego, co się stało. Dobre systemy telemetrii i rejestrowania mogą być świadome tego, co się dzieje z aplikacją, a kiedy coś się nie dzieje, możesz od razu sprawdzić i uzyskać pomocne informacje dotyczące rozwiązywania problemów.

## <a name="buy-or-rent-a-telemetry-solution"></a>Kupowanie lub wypożyczanie rozwiązania telemetrii

> [!NOTE]
> Ten artykuł został zapisany przed udostępnieniem [Application Insights](/azure/application-insights/app-insights-overview) . Application Insights jest preferowanym podejściem do rozwiązań telemetrycznych na platformie Azure. Aby uzyskać więcej informacji, zobacz [konfigurowanie Application Insights witryny sieci web ASP.NET](/azure/application-insights/app-insights-asp-net) .

Jednym z rzeczy, które doskonale nadaje się w środowisku chmury, jest to, że jest to naprawdę łatwe kupowanie lub wypożyczanie sposobu Victory. Przykładem danych telemetrycznych jest. Bez dużego nakładu pracy możesz uzyskać naprawdę dobry system telemetrii. Istnieje wiele doskonałych partnerów, którzy integrują się z platformą Azure, a niektóre z nich mają bezpłatne warstwy, dzięki czemu możesz uzyskać podstawowe dane telemetryczne dla niczego. Oto kilka z tych, które są obecnie dostępne na platformie Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [DynaTrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Program Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) zawiera również funkcje monitorowania.

Szybko przeprowadzimy Cię przez proces konfigurowania nowego Relic, aby pokazać, jak łatwo można korzystać z systemu telemetrii.

W portalu zarządzania systemu Azure Zarejestruj się w usłudze. Kliknij pozycję **Nowy**, a następnie kliknij pozycję **Magazyn**. Zostanie wyświetlone okno dialogowe **Wybieranie dodatku** . Przewiń w dół i kliknij pozycję **Nowy Relic**.

![Wybierz dodatek](monitoring-and-telemetry/_static/image1.png)

Kliknij strzałkę w prawo i wybierz żądaną warstwę usług. Dla tej wersji demonstracyjnej będziemy korzystać z warstwy Bezpłatna.

![Personalizacja dodatku](monitoring-and-telemetry/_static/image2.png)

Kliknij strzałkę w prawo, Potwierdź "zakup", a nowa Relic teraz jest wyświetlana jako dodatek w portalu.

![Przegląd zakupu](monitoring-and-telemetry/_static/image3.png)

![Nowy dodatek Relic w portalu zarządzania](monitoring-and-telemetry/_static/image4.png)

Kliknij pozycję **Informacje o połączeniu**i skopiuj klucz licencji.

![Informacje o połączeniu](monitoring-and-telemetry/_static/image5.png)

Przejdź do karty **Konfiguracja** aplikacji sieci Web w portalu, ustaw pozycję **monitorowanie wydajności** na **dodatek**, a następnie ustaw listę rozwijaną **Wybierz dodatek na** **New Relic**. Następnie kliknij przycisk **Save** (Zapisz).

![Nowy Relic na karcie Konfigurowanie](monitoring-and-telemetry/_static/image6.png)

W programie Visual Studio Zainstaluj nowy pakiet NuGet Relic w aplikacji.

![Analiza deweloperów na karcie Konfigurowanie](monitoring-and-telemetry/_static/image7.png)

Wdróż aplikację na platformie Azure i zacznij jej używać. Utwórz kilka poprawek, które należy rozwiązać, aby udostępnić działanie nowej Relic do monitorowania.

Następnie wróć do strony **Nowa Relic** na karcie **Dodatki** w portalu, a następnie kliknij pozycję **Zarządzaj**. Portal wyśle do nowego portalu zarządzania Relic przy użyciu logowania jednokrotnego w celu uwierzytelnienia, aby nie trzeba było ponownie wprowadzać poświadczeń. Na stronie Przegląd przedstawiono różne dane statystyczne dotyczące wydajności. (Kliknij obraz, aby zobaczyć pełny rozmiar strony przegląd).

[![nową kartę monitorowania Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Poniżej przedstawiono zaledwie kilka statystyk, które można zobaczyć:

- Średni czas odpowiedzi w różnych porach dnia.

    ![Czas odpowiedzi](monitoring-and-telemetry/_static/image10.png)
- Stawki za przepływność (w żądaniach na minutę) o różnych porach dnia.

    ![Przepływność](monitoring-and-telemetry/_static/image11.png)
- Czas procesora CPU na serwerze poświęcony na obsługę różnych żądań HTTP.

    ![Czasy transakcji sieci Web](monitoring-and-telemetry/_static/image12.png)
- Czas procesora CPU w różnych częściach kodu aplikacji:

    ![Szczegóły śledzenia](monitoring-and-telemetry/_static/image13.png)
- Historyczne statystyki wydajności.

    ![Wydajność historyczna](monitoring-and-telemetry/_static/image14.png)
- Wywołania usług zewnętrznych, takich jak Blob service i statystyka dotyczące sposobu niezawodnego i reagowania usługi.

    ![Usługi zewnętrzne](monitoring-and-telemetry/_static/image15.png)

    ![Usługi zewnętrzne](monitoring-and-telemetry/_static/image16.png)

    ![Usługa zewnętrzna](monitoring-and-telemetry/_static/image17.png)
- Informacje o tym, gdzie znajdują się na świecie lub w którym pochodzi ruch z aplikacji sieci Web w Stanach Zjednoczonych.

    ![Geograficzne](monitoring-and-telemetry/_static/image18.png)

Możesz również skonfigurować raporty i zdarzenia. Na przykład możesz powiedzieć, kiedy zaczniesz wyświetlać Błędy, Wyślij wiadomość e-mail do personelu pomocy technicznej dotyczącej alertu.

![Reports](monitoring-and-telemetry/_static/image19.png)

Nowy Relic to tylko jeden przykład systemu telemetrii; wszystkie te możliwości można uzyskać również z innych usług. Estetyki w chmurze polega na tym, że bez konieczności pisania kodu i w przypadku minimalnych lub niezwiązanych z nim wydatków, nagle można uzyskać więcej informacji na temat sposobu użycia aplikacji oraz tego, jakie są Twoje klienci.

<a id="log"></a>
## <a name="log-for-insight"></a>Zarejestruj się, aby uzyskać szczegółowe informacje

Pakiet telemetrii to dobry pierwszy krok, ale nadal trzeba instrumentować własny kod. Usługa telemetrii informuje o tym, kiedy wystąpił problem, i informuje o tym, co napotyka klient, ale może nie dawać Ci dużo szczegółowych informacji na temat tego, co się dzieje w kodzie.

Nie chcesz, aby użytkownik był zdalny na serwerze produkcyjnym, aby zobaczyć, jak działa aplikacja. Co może być przydatne w przypadku istnienia jednego serwera, ale co w przypadku skalowania do setek serwerów i nie wiesz, które z nich należy zdalnie? Rejestrowanie powinno zapewnić wystarczającą ilość informacji, które nigdy nie mają być zdalne do serwerów produkcyjnych w celu analizowania i debugowania problemów. Należy rejestrować wystarczające informacje, aby można było izolować problemy wyłącznie za pomocą dzienników.

### <a name="log-in-production"></a>Logowanie w środowisku produkcyjnym

Wiele osób włącza śledzenie w środowisku produkcyjnym tylko wtedy, gdy wystąpi problem i chcą debugować. Może to spowodować znaczne opóźnienie między czasem wystąpienia problemu i czasem uzyskania użytecznych informacji dotyczących rozwiązywania problemów. Informacje, które można uzyskać, mogą nie być pomocne w przypadku sporadycznych błędów.

Co zalecamy w środowisku chmury, w którym magazyn jest tani, zawsze pozostawiasz logowanie do produkcji. W ten sposób w przypadku wystąpienia błędów masz już zarejestrowane i masz historyczne dane, które mogą pomóc w analizie problemów, które opracowują się z upływem czasu lub regularnie, w różnym czasie. Można zautomatyzować proces przeczyszczania w celu usunięcia starych dzienników, ale może się okazać, że jest droższe, aby skonfigurować taki proces niż w przypadku przechowywania dzienników.

Dodanie wydatków związanych z rejestrowaniem jest bardzo proste w porównaniu z ilością czasu, jaki jest czas potrzebny do rozwiązywania problemów, dzięki czemu wszystkie informacje, które są potrzebne, są już dostępne, gdy coś się nie stało. Gdy ktoś poinformuje, że wystąpił błąd losowy w ciągu około 8:00 ostatnich nocy, ale nie pamięta błędu, można łatwo sprawdzić, co to jest problem.

Przez czas krótszy niż $4 miesięcznie można przechowywać do 50 gigabajty dzienników i wpływać na wydajność rejestrowania, tak długo, jak należy pamiętać o tym, aby uniknąć wąskich gardeł w zakresie wydajności, upewnij się, że biblioteka rejestrowania jest asynchroniczna.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Odróżnij dzienniki, które informują o dziennikach, które wymagają akcji

Dzienniki są przeznaczone do informowania (chcę wiedzieć coś) lub działania (chcę zrobić coś). Należy zachować ostrożność, aby zapisywać dzienniki działań tylko w przypadku problemów, które oryginalnie wymagają od osoby lub zautomatyzowany proces podjęcia działania. Zbyt wiele dzienników ACT spowoduje utworzenie szumu, co wymaga zbyt dużo pracy, aby przesiany przez niego wszystkie, aby znaleźć oryginalne problemy. A jeśli Dzienniki czynności automatycznie wyzwalają pewne działania, takie jak wysyłanie wiadomości e-mail do personelu pomocy technicznej, Unikaj, aby tysiące takich akcji były wyzwalane przez pojedynczy problem.

W programie .NET system. Diagnostics śledzenia można przypisać do dzienników błędy, ostrzeżenia, informacje i debugowanie/poziom szczegółowości. Można odróżnić działania od dzienników POWIADAMIAnia, przez zachowanie poziomu błędów dla dzienników czynności i używanie niższych poziomów dla dzienników INFORM.

![Poziomy rejestrowania](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Konfigurowanie poziomów rejestrowania w czasie wykonywania

Chociaż wartościowa się, że rejestrowanie jest zawsze włączone w środowisku produkcyjnym, inne najlepsze rozwiązanie polega na zaimplementowaniu struktury rejestrowania, która pozwala na dostosowanie w czasie wykonywania szczegółowego poziomu szczegółowości rejestrowania, bez ponownego wdrażania lub ponownej instalacji aplikacji. Na przykład w przypadku korzystania z funkcji śledzenia w `System.Diagnostics` można utworzyć dzienniki błędów, ostrzeżeń, informacji i debugowania/pełne. Zalecamy, aby zawsze rejestrować dzienniki błędów, ostrzeżeń i informacji w środowisku produkcyjnym, a użytkownik chce mieć możliwość dynamicznego dodawania debugowania/pełnego rejestrowania do rozwiązywania problemów na podstawie wielkości liter.

Web Apps w Azure App Service mają wbudowaną obsługę pisania dzienników `System.Diagnostics` do systemu plików, magazynu tabel lub magazynu obiektów BLOB. Dla każdego miejsca docelowego magazynu można wybrać różne poziomy rejestrowania i można zmienić poziom rejestrowania na bieżąco bez ponownego uruchamiania aplikacji. Obsługa usługi BLOB Storage ułatwia uruchamianie zadań analizy usługi [HDInsight](https://docs.microsoft.com/azure/hdinsight/) w dziennikach aplikacji, ponieważ Usługa HDInsight wie, jak pracować bezpośrednio z usługą BLOB Storage.

### <a name="log-exceptions"></a>rejestrowanie wyjątków

Nie należy tylko umieszczać *wyjątku. ToString ()* w kodzie rejestrowania. Spowoduje to pozostawienie informacji kontekstowych. W przypadku błędów SQL nie został zwrócony numer błędu SQL. W przypadku wszystkich wyjątków Uwzględnij informacje kontekstu, sam wyjątek i wyjątki wewnętrzne, aby upewnić się, że dostarczasz wszystko, co będzie potrzebne do rozwiązywania problemów. Na przykład informacje kontekstu mogą zawierać nazwę serwera, identyfikator transakcji i nazwę użytkownika (ale nie hasło ani żadne wpisy tajne).

Jeśli korzystasz z każdego dewelopera do wykonywania odpowiednich czynności z rejestrowaniem wyjątków, niektóre z nich nie będą. Aby upewnić się, że jest ona gotowa do wykonania w każdym czasie, należy kompilować obsługę wyjątków bezpośrednio do interfejsu rejestratora: Przekaż sam obiekt wyjątku do klasy rejestratora i prawidłowo rejestruje dane wyjątków w klasie rejestratora.

### <a name="log-calls-to-services"></a>Rejestruj wywołania usług

Zdecydowanie zalecamy zapisanie dziennika za każdym razem, gdy aplikacja jest wywoływana do usługi, niezależnie od tego, czy jest to baza danych, czy interfejs API REST czy dowolna usługa zewnętrzna. Dołącz do dzienników nie tylko oznaczenie sukcesu lub niepowodzenia, ale czas trwania każdego żądania. W środowisku chmury często widzisz problemy związane z spowolnieniem pracy, a nie cały czas przestoju. Coś, co zwykle trwa 10 milisekund, może chwilę potrwać. Gdy ktoś poinformuje, że Twoja aplikacja jest powolna, chcesz mieć możliwość przeglądania nowych Relic lub dowolnej usługi telemetrii i sprawdzania ich środowiska, a następnie chcesz mieć możliwość wyszukiwania własnych dzienników, aby szczegółowe szczegółowe informacje o tym, dlaczego jest to powolne.

### <a name="use-an-ilogger-interface"></a>Korzystanie z interfejsu ILogger

To, co zalecamy podczas tworzenia aplikacji produkcyjnej, jest utworzenie prostego interfejsu *ILogger* i dołożenie do niego metod. Dzięki temu można łatwo zmienić implementację rejestrowania w późniejszym czasie, a nie musimy przejść przez cały kod, aby to zrobić. Możemy korzystać z klasy `System.Diagnostics.Trace` w całej aplikacji IT, ale zamiast tego używamy jej w ramach omawianych w klasie rejestrowania, która implementuje *ILogger*, i my *ILogger* metodę wywołuje wywołania metody.

W ten sposób, jeśli kiedykolwiek chcesz, aby rejestrowanie było bogatsze, możesz zamienić [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) z dowolnym żądanym mechanizmem rejestrowania. Na przykład po rozwojem aplikacji możesz zdecydować, że chcesz użyć bardziej kompleksowego pakietu rejestrowania, takiego jak [nLOG](http://nlog-project.org/) lub [blok aplikacji rejestrowania biblioteki korporacyjnej](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) jest kolejną popularną strukturą rejestrowania, ale nie wykonuje rejestrowania asynchronicznego).

Jedną z możliwych przyczyn użycia struktury, takiej jak NLog, jest ułatwienie podziału danych wyjściowych rejestrowania na oddzielne magazyny z dużymi ilościami i wysokiej wartości. Pozwala to efektywnie przechowywać duże ilości danych INFORM, które nie są potrzebne do wykonywania szybkich zapytań, przy jednoczesnym zachowaniu szybkiego dostępu do danych działania.

### <a name="semantic-logging"></a>Rejestrowanie semantyczne

Aby uzyskać stosunkowo nowy sposób rejestrowania, który może generować bardziej przydatne informacje diagnostyczne, zobacz [blok aplikacji rejestrowania semantycznego biblioteki korporacyjnej (płycie)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Strefa korzysta ze [śledzenia zdarzeń systemu Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) i usługi [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) w programie .NET 4,5, aby umożliwić tworzenie bardziej strukturalnych i Queryable dzienników. Należy zdefiniować inną metodę dla każdego typu zdarzenia, które jest rejestrowane, co pozwala na dostosowanie zapisanych informacji. Na przykład, aby rejestrować SQL Database błąd, można wywołać metodę `LogSQLDatabaseError`. Dla tego rodzaju wyjątku wiadomo, że kluczowym elementem informacji jest numer błędu, dlatego można dołączyć parametr numeru błędu w podpisie metody i zarejestrować numer błędu jako osobne pole w rekordzie dziennika, który napiszesz. Ponieważ liczba znajduje się w osobnym polu, można łatwiej i niezawodnie otrzymywać raporty oparte na numerach błędów SQL niż można to zrobić, jeśli po prostu nałączysz numer błędu w ciąg komunikatu.

## <a name="logging-in-the-fix-it-app"></a>Rejestrowanie w aplikacji Fix it

### <a name="the-ilogger-interface"></a>Interfejs ILogger

Oto Interfejs *ILogger* w aplikacji Fix it.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Te metody umożliwiają pisanie dzienników na tych samych czterech poziomach obsługiwanych przez *System. Diagnostics*. Metody TraceApi są przeznaczone do rejestrowania wywołań usług zewnętrznych z informacjami o opóźnieniu. Można również dodać zestaw metod dla poziomu Debuguj/verbose.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>Implementacja rejestratora interfejsu ILogger

Implementacja interfejsu jest naprawdę prosta. Zasadniczo po prostu wywołuje standardowe metody *System. Diagnostics* . Poniższy fragment kodu przedstawia wszystkie trzy metody informacyjne i jeden z nich.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Wywoływanie metod ILogger

Za każdym razem, gdy kod w aplikacji poprawki IT przechwytuje wyjątek, wywołuje metodę *ILogger* , aby zarejestrować szczegóły wyjątku. Za każdym razem, gdy wykonuje wywołanie do bazy danych, Blob service lub interfejsu API REST, uruchamia stopera przed wywołaniem, kończy stopera, gdy usługa zwraca, i rejestruje czas, który upłynął, wraz z informacjami o powodzeniu lub niepowodzeniu.

Zwróć uwagę, że komunikat dziennika zawiera nazwę klasy i nazwę metody. Dobrym sposobem jest upewnienie się, że komunikaty dziennika identyfikują, która część kodu aplikacji została zapisana.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Teraz za każdym razem, gdy nastąpi wywołanie do SQL Database, można zobaczyć wywołanie, metodę, która go wywołała, i dokładnie czas trwania.

![Zapytanie SQL Database w dziennikach](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Jeśli przejdziesz do przeglądania dzienników, możesz zobaczyć, że czas wykonywania wywołań bazy danych jest zmienny. Te informacje mogą być przydatne: ponieważ każda z nich rejestruje, można analizować trendy historyczne w sposób, w jaki usługa bazy danych działa z upływem czasu. Na przykład usługa może być szybko większa od czasu, ale żądania mogą się nie powieść, a odpowiedzi mogą spowalniać się w określonych porach dnia.

Tę samą czynność można wykonać dla Blob service — za każdym razem, gdy aplikacja przekazuje nowy plik, istnieje dziennik i można sprawdzić, jak długo zajęło każdy plik.

![Dziennik przekazywania obiektów BLOB](monitoring-and-telemetry/_static/image23.png)

Jest to tylko kilka dodatkowych wierszy kodu, które można napisać za każdym razem, gdy wywołasz usługę, a teraz za każdym razem, gdy ktoś wie, że wystąpiły problemy, zobaczysz dokładnie, co się stało, czy wystąpił błąd, lub nawet wtedy, gdy działa wolno. Możesz wskazać źródło problemu bez konieczności zdalnego działania serwera lub włączyć rejestrowanie po wystąpieniu błędu i nadzieję, że zostanie on ponownie utworzony.

## <a name="dependency-injection-in-the-fix-it-app"></a>Iniekcja zależności w aplikacji rozwiązania IT

Możesz się zastanawiać, jak Konstruktor repozytorium w powyższym przykładzie pobiera implementację interfejsu rejestratora:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Aby połączyć interfejs z implementacją, aplikacja używa [iniekcji zależności](http://en.wikipedia.org/wiki/Dependency_injection)(di) z [AutoFac](http://autofac.org/). Program DI umożliwia użycie obiektu opartego na interfejsie w wielu miejscach w kodzie i wystarczy określić w jednym miejscu implementację, która jest używana podczas tworzenia wystąpienia interfejsu. Ułatwia to zmianę implementacji: na przykład możesz chcieć zastąpić rejestratora system. Diagnostics z rejestratorem NLog. Lub na potrzeby automatycznego testowania możesz chcieć zastąpić wersję makiety rejestratora.

Poprawka aplikacji IT używa DI we wszystkich repozytoriach i wszystkich kontrolerach. Konstruktory klas kontrolera uzyskują Interfejs *ITaskRepository* w taki sam sposób, jak repozytorium pobiera interfejs rejestratora:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

Aplikacja używa biblioteki AutoFac DI, aby automatycznie dostarczać *TaskRepository* i wystąpienia *rejestratora* dla tych konstruktorów.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

W tym kodzie należy się zastanowić, że każdy Konstruktor potrzebuje interfejsu *ILogger* , przekazać wystąpienie klasy *rejestratora* i zawsze, gdy wymaga interfejsu *IFixItTaskRepository* , należy przekazać wystąpienie klasy *FixItTaskRepository* .

[AutoFac](http://autofac.org/) jest jednym z wielu platform iniekcji zależności, których można użyć. Innym popularnym jest aparat [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), który jest zalecany i wspierany przez wzorce i praktyki firmy Microsoft.

## <a name="built-in-logging-support-in-azure"></a>Wbudowana obsługa rejestrowania na platformie Azure

Platforma Azure obsługuje następujące rodzaje [rejestrowania dla Web Apps w Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Śledzenie system. Diagnostics (można włączać i wyłączać i ustawiać poziom na bieżąco bez ponownego uruchamiania lokacji).
- Zdarzenia systemu Windows.
- Dzienniki usług IIS (HTTP/FREB).

Platforma Azure obsługuje następujące rodzaje [rejestrowania w Cloud Services](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Śledzenie system. Diagnostics.
- Liczniki wydajności.
- Zdarzenia systemu Windows.
- Dzienniki usług IIS (HTTP/FREB).
- Niestandardowe monitorowanie katalogów.

Aplikacja do rozwiązywania problemów używa śledzenia system. Diagnostics. Wszystko, co musisz zrobić, aby włączyć rejestrowanie system. Diagnostics w aplikacji internetowej, przerzucić przełącznik w portalu lub wywołać interfejs API REST. W portalu kliknij kartę **Konfiguracja** swojej lokacji i przewiń w dół, aby wyświetlić sekcję **Diagnostyka aplikacji** . Możesz włączyć lub wyłączyć rejestrowanie i wybrać żądany poziom rejestrowania. Na platformie Azure Możesz zapisywać dzienniki w systemie plików lub na koncie magazynu.

![Diagnostyka aplikacji i Diagnostyka lokacji na karcie Konfigurowanie](monitoring-and-telemetry/_static/image24.png)

Po włączeniu rejestrowania na platformie Azure można wyświetlić dzienniki w oknie danych wyjściowych programu Visual Studio w miarę ich tworzenia.

![Menu dzienniki przesyłania strumieniowego](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menu dzienniki przesyłania strumieniowego](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

Możesz również mieć zapisane na koncie magazynu dzienniki i wyświetlać je za pomocą dowolnego narzędzia, które może uzyskać dostęp do Table service usługi Azure Storage, takich jak **Eksplorator serwera** w programie Visual Studio lub [Eksplorator usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Dzienniki w Eksplorator serwera](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Podsumowanie

Jest to naprawdę proste do implementowania wbudowanego systemu telemetrii, rejestrowania instrumentów w własnym kodzie i konfigurowania rejestrowania na platformie Azure. A w przypadku problemów produkcyjnych połączenie systemu telemetrii i dzienników niestandardowych pomoże szybko rozwiązać problemy, zanim staną się głównymi problemami dla klientów.

W [następnym rozdziale](transient-fault-handling.md) zawarto informacje na temat obsługi błędów przejściowych, dzięki czemu nie staną się problemami produkcyjnymi, które należy zbadać.

## <a name="resources"></a>Zasoby

Więcej informacji zawierają poniższe zasoby.

Dokumentacja głównie dotyczącej telemetrii:

- [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące Instrumentacji i telemetrii, wskazówki dotyczące pomiaru punktu końcowego kondycji oraz wzorzec ponownej konfiguracji środowiska uruchomieniowego.
- [Uszczypnięć groszowe w chmurze: włączanie nowych funkcji monitorowania wydajności Relic w usłudze Azure Websites](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usłudze Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument ze znakami SIMM i Michael Thomassy. Zapoznaj się z sekcją telemetrię i diagnostyka.
- [Opracowywanie nowej generacji przy użyciu Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artykuł magazynu MSDN.

Dokumentacja głównie dotycząca rejestrowania:

- [Blok aplikacji rejestrowania semantycznego (płycie)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neila Mackenzie przedstawia przypadek rejestrowania semantycznego na PŁYCIe.
- [Tworzenie strukturalnych i znaczących dzienników z rejestrowaniem semantycznym](https://channel9.msdn.com/Events/Build/2013/3-336). Plików Wartość juliańskim Dominguez przedstawia przypadek rejestrowania semantycznego z PŁYCIą.
- [Rejestrowanie SQL Ef6 — część 1: proste rejestrowanie](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers pokazuje, jak rejestrować zapytania wykonane Entity Framework w EF 6.
- [Odporność połączeń i przechwycenie poleceń z Entity Framework w aplikacji ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Czwarta część serii samouczków z dziewięcioma częścią pokazuje, jak używać funkcji przechwycenia polecenia Dr 6 do rejestrowania poleceń SQL wysyłanych do bazy danych przez Entity Framework.
- [Ulepszanie rejestrowania C# przy użyciu atrybutów informacji o wywołaniu 5,0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Jak łatwo rejestrować nazwę metody wywołującej bez konieczności kodowania jej do literałów lub przy użyciu odbicia, aby pobrać ją ręcznie.

Dokumentacja głównie dotycząca rozwiązywania problemów:

- [Blog dotyczący rozwiązywania problemów z platformą Azure &amp;](https://blogs.msdn.com/b/kwill/).
- [AzureTools — narzędzie diagnostyczne używane przez zespół pomoc techniczna Developer platformy Azure](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Wprowadza i udostępnia link do pobierania dla narzędzia, którego można użyć na maszynie wirtualnej platformy Azure w celu pobrania i uruchomienia różnorodnych narzędzi diagnostycznych i monitorowania. Przydatne w przypadku konieczności zdiagnozowania problemu dotyczącego określonej maszyny wirtualnej.
- [Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Samouczek krok po kroku dotyczący rozpoczynania pracy z programem System. Diagnostics — śledzenie i zdalne debugowanie.

Filmy wideo:

- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria dziewięciu części przez Ulrich Homann, Marc Mercuri i marking SIMM. Prezentuje koncepcje wysokiego poziomu i zasady architektury w bardzo dostępnym i interesującym scenariuszu, w tym scenariusze opracowane przez firmę Microsoft Customer Advisory Team (CAT) z rzeczywistymi klientami. Odcinki 4 i 9 dotyczą monitorowania i telemetrii. Odcinek 9 zawiera przegląd usług monitorowania MetricsHub, AppDynamics, New Relic i usługi PagerDuty.
- [Tworzenie dużych: lekcje uzyskane od klientów platformy Azure — część II](https://channel9.msdn.com/Events/Build/2012/3-030). Oznacz moduły SIMM rozmowy dotyczące projektowania pod kątem awarii i Instrumentacji wszystkich elementów. Podobnie jak w przypadku serii failsafe, ale więcej szczegółów.

Przykład kodu:

- [Podstawy usługi w chmurze na platformie Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja utworzona przez zespół doradczy klientów Microsoft Azure. Pokazuje zasady dotyczące telemetrii i rejestrowania, jak wyjaśniono w poniższych artykułach. Przykład implementuje rejestrowanie aplikacji przy użyciu [nLOG](http://nlog-project.org/). Aby zapoznać się z dokumentacją, zobacz [artykuł cztery artykuły w witrynie TechNet wiki dotyczące telemetrii i rejestrowania](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Poprzednie](design-to-survive-failures.md)
> [dalej](transient-fault-handling.md)

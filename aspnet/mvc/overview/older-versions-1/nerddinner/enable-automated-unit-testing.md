---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Włącz automatyczne testowanie jednostek | Microsoft Docs
author: microsoft
description: Krok 12 pokazuje, jak opracować pakiet zautomatyzowanych testów jednostkowych, które weryfikują funkcje NerdDinner, co zapewni nam zaufanie do wprowadzania zmian...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 09a7aa186605a6cce48ee94028425ded957c00d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541679"
---
# <a name="enable-automated-unit-testing"></a>Włączanie automatycznych testów jednostkowych

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 12 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 12 pokazuje, jak opracować pakiet zautomatyzowanych testów jednostkowych, które weryfikują nasze funkcje NerdDinner, dzięki czemu firma Microsoft będzie mieć pewność, że zmiany i usprawnienia aplikacji w przyszłości zostaną wprowadzone.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner Krok 12: testowanie jednostkowe

Opracowujmy pakiet zautomatyzowanych testów jednostkowych, które weryfikują nasze funkcje NerdDinner, co zapewni nam pewność, że zmienią się i ulepszają aplikacje w przyszłości.

### <a name="why-unit-test"></a>Dlaczego test jednostkowy?

Na dysku do pracy o jeden dzień rano masz nagłe błyskanie inspiracji dotyczącej aplikacji, nad którą pracujesz. Istnieje zmiana, którą można zaimplementować, aby zwiększyć jej wydajność. Może to być Refaktoryzacja, która czyści kod, dodaje nową funkcję lub naprawia usterkę.

Pytanie, które ponosisz, gdy przytrzesz do komputera, to "jak bezpiecznie jest to ulepszenie?" Co zrobić, jeśli wprowadzanie zmian ma efekty uboczne lub przerywa coś? Ta zmiana może być prosta i trwa tylko kilka minut, ale co zrobić, aby ręcznie przetestować wszystkie scenariusze aplikacji? Co należy zrobić, jeśli zapomnisz o tym scenariuszu, a uszkodzona aplikacja zacznie działać w środowisku produkcyjnym? Czy zwiększa to nakład pracy?

Zautomatyzowane testy jednostkowe mogą zapewniać bezpieczeństwo sieci, które umożliwiają ciągłe ulepszanie aplikacji oraz uniknięcie boisz kodu, nad którym pracujesz. Dzięki automatycznym testom, które szybko weryfikują funkcje, umożliwiają łatwe zakodowanie i umożliwienie wprowadzenia ulepszeń. Ułatwiają one również tworzenie rozwiązań, które są bardziej utrzymywane i mają dłuższy okres istnienia, co prowadzi do znacznie większego zwrotu z inwestycji.

Platforma ASP.NET MVC umożliwia łatwe i naturalne działanie aplikacji testów jednostkowych. Umożliwia także przepływ pracy programowania testowego (TDD), który umożliwia tworzenie na podstawie testów w pierwszej kolejności.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

Po utworzeniu naszej aplikacji NerdDinner na początku tego samouczka zostanie wyświetlony monit z oknem dialogowym z pytaniem, czy chcemy utworzyć projekt testu jednostkowego, aby przejść do projektu aplikacji:

![](enable-automated-unit-testing/_static/image1.png)

Pozostawiono przycisk radiowy "tak, Utwórz projekt testu jednostkowego", który spowodował dodanie projektu "NerdDinner. Tests" do naszego rozwiązania:

![](enable-automated-unit-testing/_static/image2.png)

Projekt NerdDinner. Tests odwołuje się do zestawu projektu aplikacji NerdDinner i umożliwia łatwe dodawanie do niego zautomatyzowanych testów, które weryfikują funkcjonalność aplikacji.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Tworzenie testów jednostkowych dla naszej klasy modelu obiadu

Dodajmy kilka testów do naszego projektu NerdDinner. Tests, który weryfikuje klasę obiadu utworzoną podczas tworzenia naszej warstwy modelu.

Zaczniemy od utworzenia nowego folderu w naszym projekcie testowym o nazwie "models", gdzie umieścimy testy związane z modelem. Następnie kliknij prawym przyciskiem myszy folder i wybierz polecenie **dodaj&gt;nowego testu** . Spowoduje to wyświetlenie okna dialogowego "Dodawanie nowego testu".

Utworzymy "test jednostkowy" i nadaj mu nazwę "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Po kliknięciu przycisku "OK" program Visual Studio doda (i otworzy) plik DinnerTest.cs do projektu:

![](enable-automated-unit-testing/_static/image4.png)

Domyślny szablon testu jednostkowego programu Visual Studio zawiera wiele kodów płyt kotłów w tym samym czasie. Wyczyśćmy go, aby dokładnie zawierał Poniższy kod:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Atrybut [TestClass] w powyższej klasie DinnerTest identyfikuje go jako klasę, która będzie zawierać testy, a także opcjonalne inicjalizacje testu i kod usuwania. Możemy definiować testy w ramach tego programu, dodając do nich metody publiczne, które mają atrybut [TestMethod].

Poniżej znajdują się pierwsze dwa testy, które dodamy do naszej klasy obiadu. Pierwszy test sprawdza, czy nasz obiad jest nieprawidłowy, jeśli zostanie utworzony nowy obiad bez poprawnego ustawiania wszystkich właściwości. Drugi test sprawdza, czy nasz obiad jest ważny, gdy kolacja ma wszystkie właściwości ustawione z prawidłowymi wartościami:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Należy zauważyć, że nasze nazwy testów są bardzo jawne (i nieco pełne). Możemy to zrobić, ponieważ możemy utworzyć setki lub tysiące małych testów i chcemy ułatwić szybkie ustalenie założeń i zachowań dla każdego z nich (zwłaszcza w przypadku przeszukiwania listy błędów w module uruchamiającego testy). Nazwy testów należy nazwać po testowaniu funkcji. W powyższym przypadku użyto wzorca nazewnictwa "rzeczownik\_powinien\_".

Prowadzimy do tworzenia struktury testów przy użyciu wzorca testowania "AAA" — który oznacza "porządkowanie, działanie, potwierdzenie":

- Rozmieść: konfiguruje testowaną jednostkę
- Działanie: wykonywanie testu i wyników przechwytywania
- Potwierdzenie: sprawdzenie zachowania

Podczas pisania testów, które chcemy uniknąć, że poszczególne testy są zbyt duże. Zamiast tego każdy test powinien weryfikować tylko pojedyncze koncepcje (co znacznie ułatwia identyfikowanie przyczyn błędów). Dobrym warunkiem jest wypróbowanie i posiadanie tylko jednej instrukcji Assert dla każdego testu. Jeśli masz więcej niż jedną instrukcję Assert w metodzie testowej, upewnij się, że są one używane do testowania tego samego pojęcia. W razie wątpliwości wykonaj inny test.

### <a name="running-tests"></a>Uruchamianie testów

Program Visual Studio 2008 Professional (i nowsze wersje) zawiera wbudowany moduł uruchamiający testy, który może służyć do uruchamiania projektów testów jednostkowych programu Visual Studio w środowisku IDE. Aby uruchomić wszystkie testy jednostkowe, możemy wybrać polecenie **test-&gt;Run-&gt;wszystkie testy w rozwiązaniu** . Alternatywnie możemy umieścić kursor w ramach konkretnej klasy testowej lub metody testowej, a następnie użyć **test-&gt;Run-&gt;testy w bieżącym menu kontekstowym** (lub wpisać Ctrl R, t), aby uruchomić podzestaw testów jednostkowych.

Umieść kursor w klasie DinnerTest i wpisz ciąg "Ctrl R, T", aby uruchomić dwa testy, które właśnie zostały zdefiniowane. Gdy to zrobisz, okno "Wyniki testów" pojawi się w programie Visual Studio i zobaczymy wyniki naszego przebiegu testu w ramach tego elementu:

![](enable-automated-unit-testing/_static/image5.png)

*Uwaga: w oknie wyników testów programu VS nie jest domyślnie wyświetlana kolumna Nazwa klasy. Możesz to zrobić, klikając prawym przyciskiem myszy w oknie Wyniki testów i korzystając z menu Dodaj/Usuń kolumny.*

Nasze dwa testy miały tylko część sekundową do uruchomienia — i są widoczne w obu przypadkach. Możemy teraz przejść i rozszerzyć je przez utworzenie dodatkowych testów, które weryfikują określone walidacje reguł, a także obejmują dwie metody pomocnika-IsUserHost () i IsUserRegistered () — dodawane do klasy obiadu. Wszystkie te testy dla klasy obiadu znacznie ułatwiają i bezpiecznie dodają do nich nowe reguły biznesowe oraz ich walidacje. Możemy dodać nową logikę reguł do obiadu, a następnie w ciągu kilku sekund Sprawdź, czy nie zainstalowano żadnej z poprzednich funkcji logiki.

Zwróć uwagę, jak za pomocą opisowej nazwy testu ułatwić szybkie zapoznanie się z każdym testem. Zaleca się użycie polecenia **Narzędzia-&gt;opcje** menu, otwarcie ekranu narzędzia testowe —&gt;konfiguracja wykonywania testu, a zaznaczenie pola wyboru "podwójna kliknięcie wyniku testu jednostkowego zakończonego niepowodzeniem lub niejednoznacznego spowoduje wyświetlenie punktu awarii w teście". Umożliwi to dwukrotne kliknięcie błędu w oknie wyników testu i natychmiastowe przechodzenie do błędu potwierdzenia.

### <a name="creating-dinnerscontroller-unit-tests"></a>Tworzenie testów jednostkowych DinnersController

Utwórzmy teraz testy jednostkowe, które weryfikują funkcje DinnersController. Rozpocznie się po kliknięciu prawym przyciskiem myszy folderu "controllers" w naszym projekcie testowym, a następnie wybierz polecenie **Add-&gt;New test** menu. Utworzymy "test jednostkowy" i nadaj mu nazwę "DinnersControllerTest.cs".

Utworzymy dwie metody testowe, które weryfikują metodę Action () na DinnersController. Pierwsza z nich sprawdzi, czy widok jest zwracany po zażądaniu istniejącego obiadu. Drugi sprawdzi, czy widok "NotFound" jest zwracany, gdy zażądano nieistniejącego obiadu:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Powyższy kod kompiluje czysty. Gdy uruchamiamy testy, to oba z nich kończą się niepowodzeniem:

![](enable-automated-unit-testing/_static/image6.png)

Jeśli przejdziesz do komunikatów o błędach, zobaczymy, że przyczyna niepowodzenia testów była spowodowana tym, że nasze klasy DinnersRepository nie mogły nawiązać połączenia z bazą danych. Nasza aplikacja NerdDinner korzysta z parametrów połączenia do lokalnego pliku SQL Server Express, który znajduje się w katalogu \app\_danych projektu aplikacji NerdDinner. Ponieważ nasze projekty NerdDinner. Tests kompilują i uruchamiają się w innym katalogu, projekt aplikacji, lokalizacja ścieżki względnej naszego ciągu połączenia jest niepoprawna.

Możemy *rozwiązać* ten problem, kopiując plik bazy danych SQL Express do naszego projektu testowego, a następnie Dodaj do niego odpowiednie parametry połączenia testowego w pliku App. config naszego projektu testowego. Powyższe testy zostaną odblokowane i uruchomione.

Kod testu jednostkowego korzystający z rzeczywistej bazy danych, mimo że oferuje mu wiele wyzwań. Są to:

- Znacznie spowalnia czas wykonywania testów jednostkowych. Im dłużej trwa uruchamianie testów, tym mniej najprawdopodobniej są one wykonywane często. Najlepiej, aby testy jednostkowe mogły być uruchamiane w ciągu kilku sekund — i mieć coś, co w przypadku kompilowania projektu.
- Powoduje to skomplikowanie logiki instalacji i czyszczenia w ramach testów. Każdy test jednostkowy ma być odizolowany i niezależny od innych (bez efektów ubocznych ani zależności). Podczas pracy z rzeczywistą bazą danych należy mieć na uwadze stan i zresetować ją między testami.

Przyjrzyjmy się wzorowi projektu o nazwie "iniekcja zależności", który może pomóc nam w obejść te problemy i uniknąć konieczności używania rzeczywistej bazy danych z naszymi testami.

### <a name="dependency-injection"></a>Wstrzykiwanie zależności

Teraz DinnersController jest ściśle "sprzężone" z klasą DinnerRepository. "Sprzęganie" odnosi się do sytuacji, w której Klasa jawnie opiera się na innej klasie w celu pracy:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Ponieważ Klasa DinnerRepository wymaga dostępu do bazy danych, ściśle sprzężona zależność klasy DinnersController ma na DinnerRepository, co pozwala na przetestowanie metod akcji DinnersController.

Można to zrobić, wykorzystując Wzorzec projektowy nazywany "iniekcją zależności", który jest podejściem, w którym zależności (takie jak klasy repozytorium, które zapewniają dostęp do danych), nie są już niejawnie tworzone w ramach klas, które z nich korzystają. Zamiast tego zależności mogą być jawnie przekazane do klasy, która używa ich przy użyciu argumentów konstruktora. Jeśli zależności są zdefiniowane przy użyciu interfejsów, zapewniamy elastyczność przekazywania "fałszywych" implementacji zależności dla scenariuszy testów jednostkowych. Dzięki temu możemy tworzyć specyficzne dla testów implementacje zależne, które nie wymagają dostępu do bazy danych.

Aby zobaczyć to w działaniu, zaimplementujmy iniekcję zależności z naszymi DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Wyodrębnianie interfejsu IDinnerRepository

Pierwszym krokiem jest utworzenie nowego interfejsu IDinnerRepository, który hermetyzuje kontrakt repozytorium wymagane przez naszych kontrolerów do pobierania i aktualizowania obiadów.

Możemy zdefiniować ten kontrakt interfejsu ręcznie, klikając prawym przyciskiem myszy folder \Models, a następnie wybierając polecenie menu **&gt;Dodaj nowy element** i tworząc nowy interfejs o nazwie IDinnerRepository.cs.

Alternatywnie możemy użyć narzędzi refaktoryzacji wbudowanych Visual Studio Professional (i wyższych) do automatycznego wyodrębniania i tworzenia interfejsu dla nas z istniejącej klasy DinnerRepository. Aby wyodrębnić ten interfejs przy użyciu programu VS, po prostu umieść kursor w edytorze tekstu na klasie DinnerRepository, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie **&gt;"Wyodrębnij interfejs menu interfejsu** :

![](enable-automated-unit-testing/_static/image7.png)

Spowoduje to uruchomienie okna dialogowego "wyodrębnienie interfejsu" i wyświetlenie monitu o podanie nazwy interfejsu do utworzenia. Zostanie ona domyślnie IDinnerRepository i automatycznie wybierze wszystkie metody publiczne z istniejącej klasy DinnerRepository w celu dodania do interfejsu:

![](enable-automated-unit-testing/_static/image8.png)

Po kliknięciu przycisku "OK" program Visual Studio doda do naszej aplikacji nowy interfejs IDinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

I istniejąca Klasa DinnerRepository zostanie zaktualizowana w celu zaimplementowania interfejsu:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Aktualizowanie DinnersController na potrzeby obsługi iniekcji konstruktorów

Teraz zaktualizujemy klasę DinnersController, aby korzystała z nowego interfejsu.

Obecnie DinnersController jest zakodowana w taki sposób, że pole "dinnerRepository" zawsze jest klasą DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Zmienimy ją tak, aby pole "dinnerRepository" było typu IDinnerRepository zamiast DinnerRepository. Następnie dodamy dwa publiczne konstruktory DinnersController. Jeden z konstruktorów umożliwia przekazanie IDinnerRepository jako argumentu. Drugi jest konstruktorem domyślnym, który używa naszej istniejącej implementacji DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Ponieważ ASP.NET MVC domyślnie tworzy klasy kontrolerów przy użyciu konstruktorów domyślnych, nasze DinnersController w środowisku uruchomieniowym nadal będzie używać klasy DinnerRepository do wykonywania dostępu do danych.

Teraz możemy zaktualizować nasze testy jednostkowe, aby przekazać "fałszywą" implementację repozytorium obiadu przy użyciu konstruktora parametrów. To "fałszywe" repozytorium obiadu nie będzie wymagało dostępu do rzeczywistej bazy danych, a zamiast tego będzie używać danych przykładowych w pamięci.

#### <a name="creating-the-fakedinnerrepository-class"></a>Tworzenie klasy FakeDinnerRepository

Utwórzmy klasę FakeDinnerRepository.

Zaczniemy od utworzenia katalogu "sztuczne" w projekcie NerdDinner. Tests, a następnie dodania do niego nowej klasy FakeDinnerRepository (kliknij prawym przyciskiem myszy folder i wybierz polecenie **Dodaj-&gt;nową klasę**):

![](enable-automated-unit-testing/_static/image9.png)

Zaktualizujemy kod, tak aby Klasa FakeDinnerRepository implementuje interfejs IDinnerRepository. Następnie możemy kliknąć prawym przyciskiem myszy i wybrać polecenie menu kontekstowego "Implementuj interfejs IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Spowoduje to, że program Visual Studio automatycznie doda wszystkie elementy członkowskie interfejsu IDinnerRepository do klasy FakeDinnerRepository z domyślnymi implementacjami "szczątkowego":

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Następnie możemy zaktualizować implementację FakeDinnerRepository, aby mogła zostać wykorzystana z listy znajdującej się w pamięci&lt;obiad&gt; przekazanej do niego jako argument konstruktora:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Mamy teraz fałszywą implementację IDinnerRepository, która nie wymaga bazy danych, i można w zamian wprowadzić ją na liście obiektów obiadu znajdujących się w pamięci.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Używanie FakeDinnerRepository z testami jednostkowymi

Wróćmy do testów jednostkowych DinnersController, które nie powiodły się wcześniej, ponieważ baza danych nie była dostępna. Możemy zaktualizować metody testowe, aby użyć FakeDinnerRepository wypełnionego przykładowymi danymi obiadu w pamięci do DinnersController przy użyciu poniższego kodu:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Teraz, gdy uruchomimy te testy w obu przebiegach:

![](enable-automated-unit-testing/_static/image11.png)

Najlepszym rozwiązaniem jest użycie tylko części sekundy do uruchomienia i nie wymaga skomplikowanej logiki instalacji/oczyszczania. Możemy teraz testować jednostkowo cały kod metody akcji DinnersController (w tym wyświetlanie, stronicowanie, szczegóły, tworzenie, aktualizowanie i usuwanie) bez konieczności nawiązywania połączenia z rzeczywistą bazą danych.

| **Temat boczny: struktury iniekcji zależności** |
| --- |
| Ręczne wstrzyknięcie zależności (na przykład powyżej) działa prawidłowo, ale staje się trudniejsze do utrzymania w miarę zwiększania liczby zależności i składników w aplikacji. Istnieje kilka struktur iniekcji zależności dla platformy .NET, które mogą pomóc zapewnić jeszcze większą elastyczność zarządzania zależnościami. Te struktury, czasami nazywane kontenerami "Inversion of Control" (IoC), zapewniają mechanizmy, które umożliwiają obsługę dodatkowego poziomu konfiguracji do określania i przekazywania zależności do obiektów w czasie wykonywania (najczęściej przy użyciu iniekcji konstruktorów ). Niektóre z najpopularniejszych struktur iniekcji/IOC w programie .NET obejmują: AutoFac, Ninject, Spring.NET, StructureMap i Windsor. ASP.NET MVC uwidacznia interfejsy API rozszerzalności, które umożliwiają deweloperom uczestnictwo w rozwiązywaniu i tworzeniu wystąpień kontrolerów, co pozwala na czystą integrację funkcji iniekcji/IoC w ramach tego procesu. Przy użyciu platformy DI/IOC można również usunąć konstruktora domyślnego z naszego DinnersController — spowoduje to całkowite usunięcie sprzężenia między nim i DinnerRepository. Nie będziemy używać platformy iniekcji/IOC z naszą aplikacją NerdDinner. Jest to jednak coś, co możemy wziąć pod uwagę w przyszłości, jeśli NerdDinner kod — podstawowy i możliwości. |

### <a name="creating-edit-action-unit-tests"></a>Tworzenie testów jednostkowych akcji edycji

Utwórz teraz testy jednostkowe, które weryfikują funkcje edycji DinnersController. Zaczniemy od przetestowania wersji HTTP-GET operacji edycji:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Utworzymy test, który sprawdza, czy widok, którego kopia zapasowa jest obiektem DinnerFormViewModel, jest renderowany z powrotem po zażądaniu prawidłowego obiadu:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Gdy uruchamiamy test, okaże się, że nie powiedzie się, ponieważ wyjątek odwołania o wartości null jest generowany, gdy metoda Edytuj uzyskuje dostęp do właściwości User.Identity.Name w celu wykonania sprawdzenia obiadu. IsHostedBy ().

Obiekt User w klasie bazowej kontrolera hermetyzuje szczegóły dotyczące zalogowanego użytkownika i jest wypełniany przez ASP.NET MVC, gdy tworzy kontroler w czasie wykonywania. Ponieważ testujemy DinnersController poza środowiskiem serwera sieci Web, obiekt User nie jest ustawiony (w związku z tym wyjątek odwołania o wartości null).

### <a name="mocking-the-useridentityname-property"></a>Imitacja właściwości User.Identity.Name

Tworzenie struktur ułatwia testowanie dzięki umożliwieniu nam dynamicznego tworzenia fałszywych wersji obiektów zależnych, które obsługują nasze testy. Na przykład możemy użyć struktury imitacji w naszym teście edycji akcji, aby dynamicznie utworzyć obiekt użytkownika, którego nasza DinnersController może użyć do wyszukiwania symulowanej nazwy użytkownika. Spowoduje to uniknięcie zgłoszenia pustego odwołania podczas wykonywania testu.

Istnieje wiele struktur szkieletowych platformy .NET, które mogą być używane z ASP.NET MVC (w tym miejscu można zobaczyć listę poniżej: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). W celu przetestowania naszej aplikacji NerdDinner będziemy używać struktury "MOQ" o nazwie "open source", którą można pobrać bezpłatnie z [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Po pobraniu należy dodać odwołanie w naszym projekcie NerdDinner. Tests do zestawu MOQ. dll:

![](enable-automated-unit-testing/_static/image12.png)

Następnie dodamy metodę pomocnika "CreateDinnersControllerAs (username)" do klasy testowej, która przyjmuje nazwę użytkownika jako parametr, a następnie "imitacje" właściwości User.Identity.Name w wystąpieniu DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Powyżej korzystamy z MOQ do utworzenia obiektu przypominającego, który jest obiektem ControllerContext (który jest przekazywanym przez ASP.NET MVC do klas kontrolera, aby uwidocznić obiekty środowiska uruchomieniowego, takie jak użytkownik, żądanie, odpowiedź i sesja). Wywołujemy metodę "SetupGet" na potrzeby makiety, aby wskazać, że Właściwość HttpContext.User.Identity.Name w ControllerContext powinna zwracać ciąg username, który został przesłany do metody pomocnika.

Możemy zasymulować dowolną liczbę właściwości i metod ControllerContext. Aby zilustrować to, dodaliśmy również wywołanie SetupGet () dla właściwości Request. IsAuthenticated (która nie jest w rzeczywistości wymagana dla testów poniżej, ale które ułatwiają zilustrowanie sposobu tworzenia właściwości żądania). Po wykonaniu tych czynności przypiszemy wystąpienie makiety ControllerContext do DinnersController metody pomocnika.

Teraz możemy napisać testy jednostkowe, które używają tej metody pomocnika do testowania scenariuszy edycji obejmujących różnych użytkowników:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Teraz, gdy uruchamiamy testy, które przechodzą:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Testowanie scenariuszy UpdateModel ()

Utworzyliśmy testy, które obejmują wersję HTTP-GET akcji edycji. Teraz Utwórz kilka testów, które weryfikują wersję HTTP-POST akcji Edytuj:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Interesujący nowy scenariusz testowania dla nas do obsługi tej metody działania jest jego użycie metody pomocnika UpdateModel () w klasie podstawowej kontrolera. Używamy tej metody pomocnika do powiązania wartości formularza post z naszym wystąpieniem obiektu obiadu.

Poniżej znajdują się dwa testy, które pokazują, w jaki sposób możemy dostarczyć wartości z formularza dla metody pomocnika UpdateModel () do użycia. W tym celu należy utworzyć i wypełnić obiekt FormCollection, a następnie przypisać go do właściwości "ValueProvider" na kontrolerze.

Pierwszy test weryfikuje, że po pomyślnym zapisaniu przeglądarka zostanie przekierowana do akcji szczegóły. Drugi test weryfikuje, że po opublikowaniu nieprawidłowych danych wejściowych Akcja ponownie wyświetla widok edycji z komunikatem o błędzie.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Zawinięcie testów

Zostały omówione podstawowe koncepcje związane z klasami kontrolera testów jednostkowych. Możemy używać tych technik do łatwego tworzenia setek prostych testów, które weryfikują zachowanie naszej aplikacji.

Ponieważ nasze testy kontrolera i modelu nie wymagają rzeczywistej bazy danych, są one niezwykle szybkie i łatwe do uruchomienia. Będziemy mogli wykonywać setki zautomatyzowanych testów w ciągu kilku sekund i natychmiast uzyskać informacje o tym, czy wprowadzono zmianę. Dzięki temu firma Microsoft zapewnia zaufanie do ciągłego ulepszania, refaktoryzacji i udoskonalania naszej aplikacji.

Omawiamy testy jako ostatni temat w tym rozdziale, ale nie ponieważ testowanie jest coś, co należy zrobić na końcu procesu opracowywania. W przeciwieństwie do tego należy pisać zautomatyzowane testy tak szybko, jak to możliwe w procesie tworzenia oprogramowania. Dzięki temu można uzyskać natychmiastową opinię podczas opracowywania aplikacji, a tym samym Thoughtfully się na temat scenariuszy przypadków użycia i przeprowadzisz projektowanie aplikacji przy użyciu czystych warstw i sprzężeń.

W dalszej części książki w książce omówiono projektowanie na podstawie testów (TDD) i sposób korzystania z niego z ASP.NET MVC. TDD to iteracyjna metoda kodowania, w której należy najpierw napisać testy, które będą spełniały otrzymany kod. Przy użyciu funkcji TDD Rozpocznij każdą funkcję, tworząc test, który weryfikuje funkcjonalność, która ma zostać zaimplementowana. Pisząc test jednostkowy, należy zadbać o to, aby dobrze zrozumieć funkcję i jak powinna ona zostać zadziałała. Dopiero po zapisaniu testu (i sprawdzeniu, że nie powiodło się), można zaimplementować rzeczywiste funkcje sprawdzane przez test. Ponieważ już poświęcasz sobie nad przypadkiem użycia, w jaki sposób funkcja powinna być działała, będziesz mieć lepszy wgląd w wymagania i jak najlepiej wdrożyć je. Po wykonaniu tej czynności możesz ponownie uruchomić test — i natychmiast uzyskać opinię na temat tego, czy funkcja działa prawidłowo. Zajmiemy się tym, że będziemy więcej w rozdziale 10.

### <a name="next-step"></a>Następny krok

Niektóre końcowe zawinięte Komentarze.

> [!div class="step-by-step"]
> [Poprzednie](use-ajax-to-implement-mapping-scenarios.md)
> [dalej](nerddinner-wrap-up.md)

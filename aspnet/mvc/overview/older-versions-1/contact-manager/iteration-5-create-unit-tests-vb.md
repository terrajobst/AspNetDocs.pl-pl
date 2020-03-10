---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: 'Iteracja #5 — Tworzenie testów jednostkowych (VB) | Microsoft Docs'
author: microsoft
description: W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy nasze klasy modelu danych i Kompiluj testy jednostkowe dla o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ce1c6224a7e9203ff62f136f4f3a43e4561a904
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544297"
---
# <a name="iteration-5--create-unit-tests-vb"></a>Iteracja #5 — Tworzenie testów jednostkowych (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji Contact Management ASP.NET MVC (VB)

W tej serii samouczków tworzymy całą aplikację do zarządzania kontaktami od początku do końca. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazw, numerów telefonów i adresów e-mail — w celu uzyskania listy osób.

Aplikacja została utworzona przez wiele iteracji. W przypadku każdej iteracji stopniowo ulepszamy aplikację. Celem tej wielu iteracji jest umożliwienie zrozumienia przyczyny każdej zmiany.

- #1 iteracji — Utwórz aplikację. W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

- Iteracja #2 — Zwiększ wygląd aplikacji. W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

- Iteracja #4 — możliwość swobodnego łączenia aplikacji. W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

- #5 iteracji — Utwórz testy jednostkowe. W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

- Iteracja #6 — Użyj programowania opartego na testach. W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

- Iteracja #7 — Dodawanie funkcji AJAX. W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Ta iteracja

W poprzedniej iteracji aplikacji Contact Manager firma Microsoft Refaktoryzacja aplikację, aby była bardziej luźno powiązana. Aplikacja została oddzielona do odrębnych warstw kontrolera, usług i repozytorium. Każda warstwa współdziała z warstwą znajdującą się poniżej za pomocą interfejsów.

Aplikacja została Refaktoryzacja, aby ułatwić jej zachowanie i modyfikację. Jeśli na przykład będziemy musieli użyć nowej technologii dostępu do danych, można po prostu zmienić warstwę repozytorium bez dotknięcia kontrolera lub warstwy usług. Dzięki połączeniu z Menedżerem kontaktów luźno powiązana aplikacja została bardziej odporna na zmianę.

Ale co się stanie, gdy będziemy musiał dodać nową funkcję do aplikacji Contact Manager? Lub co się stanie, gdy naprawimy usterkę? Dokument SAD, ale dobrze sprawdzono, prawdziwie pisania kodu polega na tym, że po każdym dotknięciu kodu utworzysz ryzyko wprowadzenia nowych usterek.

Na przykład jeden dzień, Menedżer może zażądać dodania nowej funkcji do Menedżera kontaktów. Chce dodać obsługę grup kontaktów. Chce umożliwić użytkownikom organizowanie swoich kontaktów w grupy, takie jak znajomi, firma i tak dalej.

W celu zaimplementowania tej nowej funkcji należy zmodyfikować wszystkie trzy warstwy aplikacji Contact Manager. Należy dodać nowe funkcje do kontrolerów, warstwy usług i repozytorium. Od razu po rozpoczęciu modyfikowania kodu zostanie podwyższone ryzyko naruszenia funkcjonalności, która działała wcześniej.

Jest to dobry efekt refaktoryzacji naszej aplikacji do osobnych warstw. Było to dobry efekt, ponieważ umożliwia nam wprowadzanie zmian w całych warstwach bez dotykania reszty aplikacji. Jeśli jednak chcesz, aby kod w ramach warstwy był łatwiejszy do utrzymania i modyfikacji, musisz utworzyć testy jednostkowe dla kodu.

Do testowania pojedynczej jednostki kodu służy test jednostkowy. Te jednostki kodu są mniejsze niż w przypadku całej warstwy aplikacji. Zazwyczaj należy użyć testu jednostkowego, aby sprawdzić, czy dana metoda w kodzie działa w oczekiwany sposób. Na przykład można utworzyć test jednostkowy dla metody "ContactManagerService" () uwidocznionej przez klasę usługi.

Testy jednostkowe aplikacji działają podobnie jak w przypadku zabezpieczeń sieci. Za każdym razem, gdy modyfikujesz kod w aplikacji, możesz uruchomić zestaw testów jednostkowych, aby sprawdzić, czy modyfikacja przerywa istniejące funkcje. Testy jednostkowe sprawiają, że kod można bezpiecznie modyfikować. Testy jednostkowe sprawiają, że cały kod w aplikacji jest bardziej odporny na zmianę.

W tej iteracji dodamy testy jednostkowe do naszej aplikacji Contact Manager. Dzięki temu w następnej iteracji możemy dodać grupy kontaktów do naszej aplikacji bez konieczności przerywania istniejących funkcji.

> [!NOTE] 
> 
> Istnieją różne platformy testowania jednostkowego, w tym NUnit, xUnit.net i MbUnit. W tym samouczku używamy struktury testów jednostkowych dołączonej do programu Visual Studio. Można jednak równie łatwo korzystać z jednej z tych alternatywnych platform.

## <a name="what-gets-tested"></a>Co zostanie przetestowane

W idealnym świecie cały kod zostanie objęty testami jednostkowymi. W idealnym świecie będziesz mieć doskonałe bezpieczeństwo sieci. Można modyfikować każdą linię kodu w aplikacji i natychmiast wiedzieć, wykonując testy jednostkowe, niezależnie od tego, czy zmiana nie powoduje zerwania istniejących funkcji.

Jednak firma Microsoft nie pracuje na doskonałym świecie. W przypadku pisania testów jednostkowych skoncentrowano się na pisaniu testów dla logiki biznesowej (na przykład logiki walidacji). W szczególności *nie są* zapisywane testy jednostkowe dla logiki dostępu do danych ani do logiki widoku.

Aby być przydatne, testy jednostkowe muszą być wykonywane bardzo szybko. Można łatwo zbierać setki (lub nawet tysiące) testów jednostkowych dla aplikacji. Jeśli uruchomienie testów jednostkowych potrwa dużo czasu, należy unikać ich wykonywania. Innymi słowy, długotrwałe testy jednostkowe są bezużyteczne do celów kodowania od dnia do dnia.

Z tego powodu zazwyczaj nie należy pisać testów jednostkowych dla kodu, który współdziała z bazą danych. Uruchamianie setek testów jednostkowych względem aktywnej bazy danych byłoby zbyt wolne. Zamiast tego należy zasymulować bazę danych i napisać kod, który współdziała z bazą danych z makietą (omawiamy sposób tworzenia bazy danych poniżej).

Z podobnej przyczyny zazwyczaj nie są zapisywane testy jednostkowe dla widoków. Aby przetestować widok, należy uruchomić serwer sieci Web. Ponieważ połączenie serwera sieci Web jest procesem stosunkowo wolno, tworzenie testów jednostkowych dla widoków nie jest zalecane.

Jeśli widok zawiera skomplikowaną logikę, należy rozważyć przeniesienie logiki do metod pomocnika. Można napisać testy jednostkowe dla metod pomocniczych, które są wykonywane bez nawirowania serwera sieci Web.

> [!NOTE] 
> 
> Podczas pisania testów logiki dostępu do danych lub logiki widoku nie jest dobrym pomysłem podczas pisania testów jednostkowych, te testy mogą być bardzo cenne podczas tworzenia testów funkcjonalnych lub integracji.

> [!NOTE] 
> 
> ASP.NET MVC to aparat widoku formularzy sieci Web. Gdy aparat widoku formularzy sieci Web jest zależny od serwera sieci Web, inne aparaty widoków mogą nie być.

## <a name="using-a-mock-object-framework"></a>Korzystanie z struktury obiektów makiety

Podczas kompilowania testów jednostkowych niemal zawsze trzeba skorzystać z struktury obiektów makiety. Struktura obiektów makiety umożliwia tworzenie makiet i wycinków klas w aplikacji.

Na przykład można użyć struktury obiektu makiety do wygenerowania makiety klasy repozytorium. W ten sposób można użyć klasy repozytorium makiety zamiast rzeczywistej klasy repozytorium w testach jednostkowych. Użycie repozytorium makiety pozwala uniknąć wykonywania kodu bazy danych podczas wykonywania testu jednostkowego.

Program Visual Studio nie zawiera struktury obiektów makiety. Istnieje jednak kilka struktur obiektów dla programu .NET Framework komercyjnych i otwartych źródeł:

1. MOQ — Platforma ta jest dostępna w ramach licencji Open Source systemu BSD. Możesz pobrać MOQ z [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Makiety Rhino — Ta struktura jest dostępna w ramach licencji Open Source systemu BSD. Makiety Rhino można pobrać z [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator — to jest platforma komercyjna. Wersję próbną można pobrać z [http://www.typemock.com/](http://www.typemock.com/).

W tym samouczku postanowiono używać MOQ. Można jednak równie łatwo używać Rhinoów lub Isolator Typemock do tworzenia obiektów makiety dla aplikacji Contact Manager.

Aby można było korzystać z MOQ, należy wykonać następujące czynności:

1. .
2. Przed rozpakowaniem pobranego pliku upewnij się, że kliknięto prawym przyciskiem myszy plik, a następnie kliknij przycisk zatytułowany **Odblokuj** (patrz rysunek 1).
3. Rozpakuj pobieranie.
4. Dodaj odwołanie do zestawu MOQ do projektu testowego, wybierając opcję menu **projekt, Dodaj odwołanie,** aby otworzyć okno dialogowe **Dodawanie odwołania** . Na karcie przeglądanie przejdź do folderu, w którym został rozpakowany MOQ, a następnie wybierz zestaw MOQ. dll. Kliknij przycisk **OK** (patrz rysunek 2).

[![odblokowywanie MOQ](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Ilustracja 01**. Odblokowywanie MOQ ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image2.png))

[Odwołania ![po dodaniu MOQ](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Ilustracja 02**: odwołania po dodaniu MOQ ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Tworzenie testów jednostkowych dla warstwy usług

Zacznij od utworzenia zestawu testów jednostkowych dla naszej warstwy usług aplikacji menedżera kontaktów. Będziemy używać tych testów do weryfikacji logiki walidacji.

Utwórz nowy folder o nazwie models w projekcie ContactManager. tests. Następnie kliknij prawym przyciskiem myszy folder modele i wybierz polecenie **Dodaj, nowy test**. Zostanie wyświetlone okno dialogowe **Dodawanie nowego testu** pokazane na rysunku 3. Wybierz szablon **testu jednostkowego** i nazwij nowy test ContactManagerServiceTest. vb. Kliknij przycisk **OK** , aby dodać nowy test do projektu testowego.

> [!NOTE] 
> 
> Ogólnie rzecz biorąc, struktura folderów projektu testowego powinna być zgodna ze strukturą folderów projektu MVC ASP.NET. Na przykład umieszczasz testy kontrolera w folderze controllers, modeluje testy w folderze modeli i tak dalej.

[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Ilustracja 03**: Models\ContactManagerServiceTest.cs ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image6.png))

Początkowo chcemy przetestować metodę my Contact () uwidocznioną przez klasę ContactManagerService. Utworzymy następujące pięć testów:

- GetContact () — testuje, że element nocontact () zwraca wartość true, gdy prawidłowy kontakt jest przekazaniem do metody.
- CreateContactRequiredFirstName () — testuje, że komunikat o błędzie jest dodawany do stanu modelu, gdy kontakt z brakującą nazwą jest przesyłany do metody oncontact ().
- CreateContactRequiredLastName () — testuje, że komunikat o błędzie jest dodawany do stanu modelu, gdy kontakt z brakującą nazwiskiem jest przesyłany do metody oncontact ().
- CreateContactInvalidPhone () — testuje, że komunikat o błędzie jest dodawany do stanu modelu, gdy kontakt z nieprawidłowym numerem telefonu jest przesyłany do metody oncontact ().
- CreateContactInvalidEmail () — testuje, że komunikat o błędzie jest dodawany do stanu modelu, gdy kontakt z nieprawidłowym adresem e-mail jest przesyłany do metody oncontact ().

Pierwszy test weryfikuje, czy prawidłowy kontakt nie generuje błędu walidacji. Pozostałe testy sprawdzają każdą z reguł walidacji.

Kod dla tych testów znajduje się na liście 1.

**Lista 1 — Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]

Ponieważ korzystamy z klasy Contact w aukcji 1, musimy dodać odwołanie do Entity Framework firmy Microsoft do naszego projektu testowego. Dodaj odwołanie do zestawu System. Data. Entity.

Lista 1 zawiera metodę o nazwie Initialize (), która ma atrybut [TestInitialize]. Ta metoda jest wywoływana automatycznie przed uruchomieniem każdego z testów jednostkowych (jest wywoływana 5 razy bezpośrednio przed każdym z testów jednostkowych). Metoda Initialize () tworzy repozytorium makiety z następującym wierszem kodu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Ten wiersz kodu używa struktury MOQ do generowania repozytorium makiety z interfejsu IContactManagerRepository. Repozytorium makiety jest używane zamiast rzeczywistego EntityContactManagerRepository, aby uniknąć uzyskiwania dostępu do bazy danych podczas uruchamiania każdego testu jednostkowego. Repozytorium makiety implementuje metody interfejsu IContactManagerRepository, ale metody not nie wykonują żadnego działania.

> [!NOTE] 
> 
> W przypadku korzystania z MOQ Framework istnieje rozróżnienie między \_mockRepository i \_mockRepository. Object. Dawna odwołuje się do klasy makiety (of IContactManagerRepository), która zawiera metody określania sposobu zachowania repozytorium makiety. Ta ostatnia odwołuje się do rzeczywistego repozytorium makiety, które implementuje interfejs IContactManagerRepository.

Repozytorium makiety jest używane w metodzie Initialize () podczas tworzenia wystąpienia klasy ContactManagerService. Wszystkie testy jednostkowe są używane w tym wystąpieniu klasy ContactManagerService.

Lista 1 zawiera pięć metod, które odpowiadają każdej z testów jednostkowych. Każda z tych metod ma atrybut [TestMethod]. Po uruchomieniu testów jednostkowych każda metoda, która ma ten atrybut jest wywoływana. Innymi słowy, każda metoda, która jest uzupełniona atrybutem [TestMethod] jest testem jednostkowym.

Pierwszy test jednostkowy o nazwie CreateInstance () sprawdza, czy wywołanie metody CreateInstance () zwraca wartość true, gdy do metody jest przenoszona prawidłowe wystąpienie klasy Contact. Test tworzy wystąpienie klasy Contact, wywołuje metodę nocontact () i sprawdza, czy element "nocontact () zwróci wartość true.

Pozostałe testy sprawdzają, czy gdy metoda nocontact () jest wywoływana z nieprawidłowym kontaktem, a następnie metoda zwraca wartość false, a oczekiwany komunikat o błędzie walidacji jest dodawany do stanu modelu. Na przykład, test CreateContactRequiredFirstName () tworzy wystąpienie klasy Contact z pustym ciągiem dla jego właściwości FirstName. Następnie Metoda nocontact () jest wywoływana z nieprawidłowym kontaktem. Na koniec test weryfikuje, czy element iscontact () zwraca wartość false, a ten stan modelu zawiera oczekiwany komunikat o błędzie walidacji "pierwsze nazwisko jest wymagane".

Testy jednostkowe można uruchomić z listy 1, wybierając opcję menu **test, Uruchom, wszystkie testy w rozwiązaniu (Ctrl + R, A)** . Wyniki testów są wyświetlane w oknie Wyniki testów (zobacz rysunek 4).

[![Wyniki testów](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Ilustracja 04**: wyniki testów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-5-create-unit-tests-vb/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Tworzenie testów jednostkowych dla kontrolerów

Aplikacja ASP.NET MVC steruje przepływem interakcji z użytkownikiem. Podczas testowania kontrolera, należy sprawdzić, czy kontroler zwróci odpowiednie wyniki akcji i wyświetli dane. Warto również sprawdzić, czy kontroler współdziała z klasami modelu w oczekiwany sposób.

Na przykład lista 2 zawiera dwa testy jednostkowe dla metody Create () kontrolera Contact. Pierwszy test jednostkowy sprawdza, czy gdy prawidłowy kontakt jest przesyłany do metody Create (), a następnie Metoda Create () przekierowuje do akcji index. Innymi słowy, gdy przeszedł prawidłowy kontakt, Metoda Create () powinna zwrócić element RedirectToRouteResult, który reprezentuje akcję indeksu.

Nie chcemy przetestować warstwy usługi ContactManager podczas testowania warstwy kontrolera. W związku z tym tworzymy warstwę usługi przy użyciu następującego kodu w metodzie Initialize:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

W teście jednostkowym CreateValidContact () tworzymy zachowanie wywoływania metody () warstwy usługi z następującym wierszem kodu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Ten wiersz kodu powoduje, że usługa makieter nie zwraca wartości true, gdy wywoływana jest metoda oncontact (). Poprzez imitację warstwy usług możemy przetestować zachowanie naszego kontrolera bez konieczności wykonywania kodu w warstwie usług.

Drugi test jednostkowy sprawdza, czy akcja Utwórz () zwraca widok, gdy do metody jest przenoszona nieprawidłowy kontakt. Powoduje to, że metoda oncontact () warstwy usługi zwróci wartość false z następującym wierszem kodu:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Jeśli metoda Create () będzie działać w oczekiwany sposób, powinien zwrócić widok, gdy warstwa usługi zwróci wartość false. Dzięki temu kontroler może wyświetlać komunikaty o błędach walidacji w widoku tworzenia, a użytkownik ma szansę poprawić te nieprawidłowe właściwości kontaktu.

Jeśli planujesz kompilację testów jednostkowych dla kontrolerów, musisz zwrócić jawne nazwy widoków z akcji kontrolera. Na przykład nie należy zwracać widoku w następujący sposób:

Widok zwracany ()

Zamiast tego Zwróć widok podobny do tego:

Widok zwracany ("Utwórz")

Jeśli podczas zwracania widoku nie są jawne, właściwość ViewResult. viewName zwraca pusty ciąg.

**Lista 2 — Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Podsumowanie

W tej iteracji utworzyliśmy testy jednostkowe dla naszej aplikacji menedżera kontaktów. Możemy uruchomić te testy jednostkowe w dowolnym momencie, aby upewnić się, że aplikacja nadal działa w oczekiwany sposób. Testy jednostkowe działają jako sieć bezpieczeństwa dla naszej aplikacji, dzięki czemu możemy bezpiecznie modyfikować naszą aplikację w przyszłości.

Utworzyliśmy dwa zestawy testów jednostkowych. Najpierw przetestowano nasze logiki walidacji, tworząc testy jednostkowe dla naszej warstwy usług. Następnie przetestowano nasze logiki kontroli przepływu, tworząc testy jednostkowe dla naszej warstwy kontrolera. Podczas testowania naszej warstwy usług wykryliśmy nasze testy dla naszej warstwy usług z naszej warstwy repozytorium przez imitację naszej warstwy repozytorium. Podczas testowania warstwy kontrolera odizolowano nasze testy dla naszej warstwy kontrolera przez imitację warstwy usług.

W następnej iteracji zmodyfikujemy aplikację Contact Manager, aby obsługiwała grupy kontaktów. Ta nowa funkcjonalność zostanie dodana do naszej aplikacji przy użyciu procesu projektowania oprogramowania zwanego programowaniem opartym na testach.

> [!div class="step-by-step"]
> [Poprzednie](iteration-4-make-the-application-loosely-coupled-vb.md)
> [dalej](iteration-6-use-test-driven-development-vb.md)

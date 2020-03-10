---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET pomocników MVC 4, formularze i sprawdzanie poprawności | Microsoft Docs
author: rick-anderson
description: W przypadku modeli ASP.NET MVC 4 i dostępu do danych w laboratorium załadowane i wyświetlane są dane z bazy danych. W tym ćwiczeniu dowiesz się, jak dodać do...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539579"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>ASP.NET MVC 4 — pomocnicy, formularze i walidacja

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

W przypadku **modeli ASP.NET MVC 4 i dostępu do danych** w laboratorium załadowane i wyświetlane są dane z bazy danych. W tym ćwiczeniu należy dodać do aplikacji ze **sklepu muzycznego** możliwość edytowania tych danych.

W tym celu należy najpierw utworzyć kontroler, który będzie obsługiwał akcje tworzenia, odczytu, aktualizacji i usuwania (CRUD) albumów. Zostanie wygenerowany szablon widoku indeksu wykorzystujący funkcję tworzenia szkieletów ASP.NET MVC, aby wyświetlić właściwości albumów w tabeli HTML. Aby ulepszyć ten widok, należy dodać niestandardowego pomocnika HTML, który będzie obcinał długi opis.

Następnie dodasz widoki Edytuj i Utwórz, które umożliwią zmianę albumów w bazie danych, z pomocą elementów formularza, takich jak listy rozwijane.

Na koniec zezwolisz użytkownikom na usunięcie albumu, a ponadto uniemożliwimy im wprowadzanie nieprawidłowych danych, sprawdzając ich dane wejściowe.

W ramach tego ćwiczenia praktycznego założono, że masz podstawową wiedzę na temat **ASP.NET MVC**. Jeśli wcześniej nie korzystasz z **ASP.NET MVC** , zalecamy przejście do środowiska ASP.NET w oparciu o podstawowe informacje dotyczące **MVC** .

To laboratorium przeprowadzi Cię przez ulepszenia i nowe funkcje opisane wcześniej przez zastosowanie drobnych zmian do przykładowej aplikacji sieci Web, która znajduje się w folderze źródłowym.

> [!NOTE]
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [wersjach Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzny dla tego laboratorium jest dostępny w [ASP.NETach, formularzach i weryfikacji w MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Tworzenie kontrolera do obsługi operacji CRUD
- Generuj widok indeksu, aby wyświetlić właściwości jednostki w tabeli HTML
- Dodawanie niestandardowego pomocnika HTML
- Tworzenie i Dostosowywanie widoku edycji
- Różnice między metodami akcji, które reagują na wywołania HTTP-GET lub HTTP-POST
- Dodawanie i Dostosowywanie widoku tworzenia
- Obsłuż Usuwanie jednostki
- Weryfikowanie danych wejściowych użytkownika

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku B: Using fragmenty kodu](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

Następujące ćwiczenia składają się na te praktyczne laboratorium:

1. [Tworzenie kontrolera Menedżera sklepu i jego widoku indeksu](#Exercise1)
2. [Dodawanie pomocnika HTML](#Exercise2)
3. [Tworzenie widoku edycji](#Exercise3)
4. [Dodawanie widoku](#Exercise4)
5. [Obsługa usuwania](#Exercise5)
6. [Dodawanie weryfikacji](#Exercise6)
7. [Korzystanie z niezauważalnej technologii jQuery na stronie klienta](#Exercise7)

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

Szacowany czas wykonywania tego laboratorium: **60 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Ćwiczenie 1: Tworzenie kontrolera Menedżera sklepu i jego widoku indeksu

W tym ćwiczeniu dowiesz się, jak utworzyć nowy kontroler do obsługi operacji CRUD, dostosować jego metodę działania indeksu, aby zwracała listę albumów z bazy danych, a wreszcie wygenerować szablon widoku indeksu wykorzystujący tworzenie szkieletu ASP.NET MVC Funkcja umożliwiająca wyświetlanie właściwości albumów w tabeli HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Zadanie 1 — Tworzenie StoreManagerController

W tym zadaniu utworzysz nowy kontroler o nazwie **StoreManagerController** do obsługi operacji CRUD.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex1-CreatingTheStoreManagerController/BEGIN/** folder.

   1. Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Dodaj nowy kontroler. Aby to zrobić, kliknij prawym przyciskiem myszy folder **controllers** w obszarze Eksplorator rozwiązań wybierz pozycję **Dodaj** , a następnie polecenie **kontroler** . Zmień nazwę **kontrolera** na **StoreManagerController** i upewnij się, że jest zaznaczona opcja **kontroler MVC z pustymi akcjami odczytu/zapisu** . Kliknij pozycję **Add** (Dodaj).

    ![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Okno dialogowe Dodawanie kontrolera")

    *Okno dialogowe Dodawanie kontrolera*

    Jest generowana nowa klasa kontrolera. Ponieważ wskazane jest dodanie akcji do odczytu/zapisu, metody zastępcze dla tych, typowe akcje CRUD są tworzone z wypełnionymi komentarzami do wykonania, monitując o uwzględnienie logiki specyficznej dla aplikacji.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Zadanie 2 — Dostosowywanie indeksu Magazynumanager

W tym zadaniu zostanie dostosowana Metoda działania indeksu Magazynumanager w celu zwrócenia widoku z listą albumów z bazy danych.

1. W klasie StoreManagerController Dodaj następujące dyrektywy *using* .

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex1 przy użyciu MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Dodaj pole do **StoreManagerController** , aby pomieścić wystąpienie elementu **MusicStoreEntities.**

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Zaimplementuj akcję indeks StoreManagerController, aby zwrócić widok z listą albumów.

    Logika akcji kontrolera będzie bardzo podobna do akcji indeksu StoreController zapisaną wcześniej. Użyj LINQ do pobrania wszystkich albumów, w tym informacji o gatunku i wykonawcy do wyświetlenia.

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex1 StoreManagerController index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Zadanie 3 — Tworzenie widoku indeksu

W tym zadaniu utworzysz szablon widoku indeksu, aby wyświetlić listę albumów zwracanych przez kontroler **magazynu** .

1. Przed utworzeniem nowego szablonu widoku należy skompilować projekt, aby **okno dialogowe Dodawanie widoku** znało informacje o klasie **albumu** do użycia. Wybierz **kompilację | Kompiluj MvcMusicStore** w celu skompilowania projektu.
2. Kliknij prawym przyciskiem myszy wewnątrz metody akcji **indeksu** i wybierz polecenie **Dodaj widok**. Spowoduje to wyświetlenie okna dialogowego **Dodawanie widoku** .

    ![Dodaj widok](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Dodaj widok")

    *Dodawanie widoku z poziomu metody index*
3. W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **indeksowana**. Wybierz opcję **Utwórz widok z silną typem** i wybierz pozycję **album (MvcMusicStore. models)** z listy rozwijanej **Klasa modelu** . Wybierz pozycję **Lista** na liście rozwijanej **szablon szkieletu** . Pozostaw **aparat widoku** do **Razor** i inne pola z wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku indeksu](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Dodawanie widoku indeksu")

    *Dodawanie widoku indeksu*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Zadanie 4 — Dostosowywanie szkieletu widoku indeksu

W tym zadaniu dostosowano prosty szablon widoku utworzony przy użyciu funkcji szkieletowej ASP.NET MVC, aby wyświetlała odpowiednie pola.

> [!NOTE]
> Obsługa tworzenia **szkieletów** w ramach ASP.NET MVC generuje prosty szablon widoku, który zawiera listę wszystkich pól w modelu albumu. Tworzenie **szkieletów** umożliwia szybkie rozpoczęcie pracy w widoku o jednoznacznie określonym typie: zamiast konieczności ręcznego pisania szablonu widoku, tworzenie szkieletu szybko generuje szablon domyślny, a następnie można zmodyfikować wygenerowany kod.

1. Przejrzyj utworzony kod. Wygenerowana Lista pól będzie częścią poniższej tabeli **HTML, która** jest używana do wyświetlania danych tabelarycznych.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Zastąp **&lt;tabelę&gt;** kodem następującym kodem, aby wyświetlić tylko pola **gatunek**, **wykonawca**, **tytuł albumu**i **Price** . Spowoduje to usunięcie kolumn **AlbumId** i **URL albumów** . Ponadto zmienia kolumny GenreId i ArtistId, aby wyświetlić właściwości klasy połączonej **Artist.Name** i **Genre.Name**, a także usunąć łącze **szczegóły** .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Zmień poniższe opisy.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowana, że w szablonie widoku **indeksu** **magazynumanager** zostanie wyświetlona lista albumów zgodnie z projektem poprzednich kroków.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager** , aby sprawdzić, czy zostanie wyświetlona lista albumów pokazująca ich **tytuł**, **wykonawcę** i **gatunek**.

    ![Przeglądanie listy albumów](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Przeglądanie listy albumów")

    *Przeglądanie listy albumów*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Ćwiczenie 2: Dodawanie pomocnika HTML

Na stronie indeksu Magazynumanager występuje jeden potencjalny problem: właściwości nazwy tytułu i wykonawcy mogą być wystarczająco długie, aby wyrzucać formatowanie tabeli. W tym ćwiczeniu dowiesz się, jak dodać niestandardowy pomocnik HTML do obcięcia tego tekstu.

Na poniższej ilustracji widać, jak format jest modyfikowany z powodu długości tekstu, gdy jest używany mały rozmiar przeglądarki.

![Przeglądanie listy albumów z obciętym tekstem](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Przeglądanie listy albumów z obciętym tekstem")

*Przeglądanie listy albumów z obciętym tekstem*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Zadanie 1 — rozszerzanie pomocnika HTML

W tym zadaniu zostanie dodana nowa metoda **obcinania** do obiektu **HTML** uwidocznionego w widokach ASP.NET MVC. W tym celu należy zaimplementować **metodę rozszerzenia** do wbudowanej klasy **System. Web. MVC. HtmlHelper** dostarczonej przez ASP.NET MVC.

> [!NOTE]
> Aby dowiedzieć się więcej na temat **metod rozszerzających**, zobacz ten artykuł w witrynie MSDN. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex2-AddingAnHTMLHelper/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz widok indeksu elementu Storemanager. W tym celu w Eksplorator rozwiązań rozwiń folder **widoki** , a następnie pozycję **storemanager** i Otwórz plik **index. cshtml** .
3. Dodaj następujący kod poniżej dyrektywy <strong>@model</strong> , aby zdefiniować metodę pomocnika <strong>obcinania</strong> .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Zadanie 2 — obcinanie tekstu na stronie

W tym zadaniu zostanie użyta metoda **Truncate** do obcięcia tekstu w szablonie widoku.

1. Otwórz widok indeksu elementu Storemanager. W tym celu w Eksplorator rozwiązań rozwiń folder **widoki** , a następnie pozycję **storemanager** i Otwórz plik **index. cshtml** .
2. Zastąp wiersze pokazujące **nazwę wykonawcy** i **tytuł**albumu. Aby to zrobić, Zastąp poniższe wiersze.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowany, że szablon widoku **indeksu** **Magazynumanager** obcina tytuł albumu i nazwę wykonawcy.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager** , aby sprawdzić, czy długie teksty w **tytule** i kolumnie **wykonawcy** zostały obcięte.

    ![Nazwy tytułów i artystów](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nazwy tytułów i artystów")

    *Tytuły i nazwy wykonawców*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Ćwiczenie 3: Tworzenie widoku edycji

W tym ćwiczeniu dowiesz się, jak utworzyć formularz umożliwiający menedżerom magazynu edytowanie albumu. Przeszukamy adres URL **/StoreManager/Edit/ID** (**Identyfikator** jest unikatowym identyfikatorem albumu do edycji), co spowoduje wywołanie http-get do serwera.

Metoda akcji Edytuj kontrolera pobierze odpowiedni album z bazy danych, utworzyć obiekt **StoreManagerViewModel** do hermetyzacji (wraz z listą artystów i gatunków), a następnie przekazać go do szablonu widoku w celu renderowania strony HTML z powrotem do użytkownika. Ta strona będzie zawierać **&lt;formularz&gt;** z polami textlist i listami rozwijanymi służącymi do edytowania właściwości albumu.

Gdy użytkownik zaktualizuje wartości formularza albumu i kliknie przycisk **Zapisz** , zmiany są przesyłane za pośrednictwem wywołania http-post z powrotem do **/StoreManager/Edit/ID**. Mimo że adres URL pozostaje taki sam jak w przypadku ostatniego wywołania, ASP.NET MVC określa, że jest to czas POST protokołu HTTP i w związku z tym wykonuje inną metodę edycji akcji (z jedną dekoracyjną z **[HTTPPOST]** ).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Zadanie 1 — implementacja metody akcji edycji HTTP-GET

W tym zadaniu zostanie zaimplementowana wersja protokołu HTTP-GET akcji Edytuj akcję w celu pobrania odpowiedniego albumu z bazy danych, a także listy wszystkich gatunków i artystów. Spowoduje to spakowanie tych danych do obiektu **StoreManagerViewModel** zdefiniowanego w ostatnim kroku, który następnie zostanie przesłany do szablonu widoku w celu renderowania odpowiedzi.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex3-CreatingTheEditView/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz klasę **StoreManagerController** . Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.
3. Zastąp metodę **http-Get Edit** Action następującym kodem, aby pobrać odpowiedni **album** , a także listę **gatunku** i **artystów** .

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex3 STOREMANAGERCONTROLLER HTTP-GET Edycja akcji*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Używasz programu **System. Web. MVC** **SelectList** dla artystów i gatunek zamiast listy **System. Collections. Generic** .
    > 
    > **SelectList** jest przejrzystym sposobem wypełniania list rozwijanych HTML i zarządzania elementami, takimi jak bieżące zaznaczenie. Utworzenie wystąpienia i późniejsze skonfigurowanie tych obiektów ViewModel w akcji kontrolera spowoduje, że będzie to bardziej przejrzysty scenariusz edycji.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Zadanie 2 — Tworzenie widoku edycji

W tym zadaniu utworzysz szablon widoku edycji, który będzie później wyświetlał właściwości albumu.

1. Utwórz widok edycji. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody akcji **Edytuj** i wybierz polecenie **Dodaj widok**.
2. W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **edytowana**. Zaznacz pole wyboru **Utwórz widok o jednoznacznie określonym typie** i wybierz opcję **album (MvcMusicStore. models)** z listy rozwijanej **Wyświetl klasę danych** . Wybierz pozycję **Edytuj** z listy rozwijanej **szablon szkieletu** . Pozostaw pozostałe pola wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Dodawanie widoku edycji")

    *Dodawanie widoku edycji*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — Uruchamianie aplikacji

W tym zadaniu przetestujesz, że na **stronie widok programu** **storemanager** są wyświetlane wartości właściwości dla albumu przesłanego jako parametr.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager/Edit/1** , aby sprawdzić, czy są wyświetlane wartości właściwości dla przesłanego albumu.

    ![Przeglądanie widoku edycji albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Przeglądanie widoku edycji albumu")

    *Przeglądanie widoku edycji albumu*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Zadanie 4 — implementowanie list rozwijanych w szablonie edytora albumu

W tym zadaniu dodasz listę rozwijaną do szablonu widoku utworzonego w ostatnim zadaniu, dzięki czemu użytkownik może wybrać spośród listy artystów i gatunku.

1. Zastąp cały kod zestawu pól **albumu** następującym:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Dodano pomocnika **HTML. DropDownList** do listy rozwijanej renderowania do wybierania artystów i gatunków. Parametry przesłane do **języka HTML. DropDownList** są następujące:
    > 
    > 1. Nazwa pola formularza ( **&quot;ArtistId&quot;** ).
    > 2. **SelectList** wartości dla listy rozwijanej.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowany, że na **stronie widok widoku** **sklepumanager** są wyświetlane listy rozwijane zamiast pól tekstowych Nazwa wykonawca i gatunek.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager/Edit/1** , aby upewnić się, że wyświetla listę rozwijaną zamiast pól tekstowych Nazwa wykonawca i gatunek.

    ![Przeglądanie widoku edycji albumu z listami rozwijanymi](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Przeglądanie widoku edycji albumu z listami rozwijanymi")

    *Przeglądanie widoku edycji albumu, ten czas z listami rozwijanymi*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Zadanie 6 — implementacja metody akcji edycji HTTP-POST

Teraz, gdy widok edycji jest wyświetlany zgodnie z oczekiwaniami, należy zaimplementować metodę akcji edycji HTTP-POST w celu zapisania zmian wprowadzonych w albumie.

1. W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio. Otwórz **StoreManagerController** z folderu **controllers** .
2. Zamień kod metody akcji **edycji http-post** na następujący (należy zauważyć, że metoda, która musi zostać zastąpiona, ma przeciążoną wersję, która odbiera dwa parametry):

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — Ex3 STOREMANAGERCONTROLLER http-post Edycja*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Ta metoda zostanie wykonana po kliknięciu przez użytkownika przycisku **Zapisz** i przeniesieniu http-post wartości formularza z powrotem na serwer w celu utrwalenia ich w bazie danych. Dekoratora **[HTTPPOST]** wskazuje, że metoda powinna być stosowana dla tych scenariuszy http-post. Metoda przyjmuje obiekt **albumu** . ASP.NET MVC automatycznie utworzy obiekt albumu na podstawie &lt;formularza&gt; wartości.
    > 
    > Ta metoda wykona następujące czynności:
    > 
    > 1. Jeśli model jest prawidłowy:
    > 
    >     1. Zaktualizuj wpis albumu w kontekście, aby oznaczyć go jako zmodyfikowany obiekt.
    >     2. Zapisz zmiany i Przekieruj do widoku indeksu.
    > 2. Jeśli model jest nieprawidłowy, wypełni ViewBag z **GenreId** i **ArtistId**, a następnie zwróci widok z odebranym obiektem albumu, aby zezwolić użytkownikowi na wykonanie dowolnej wymaganej aktualizacji.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Zadanie 7 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowana, że na stronie widok **magazynumanager** , w rzeczywistości zapisywane są zaktualizowane dane albumu w bazie danych.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager/Edit/1**. Zmień tytuł albumu na **Załaduj** i kliknij pozycję **Zapisz**. Sprawdź, czy tytuł albumu został rzeczywiście zmieniony na liście albumów.

    ![Aktualizowanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Aktualizowanie albumu")

    *Aktualizowanie albumu*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Ćwiczenie 4: Dodawanie widoku tworzenia

Teraz, gdy **StoreManagerController** obsługuje możliwość **edycji** , w tym ćwiczeniu dowiesz się, jak dodać szablon tworzenia widoku, aby umożliwić menedżerom sklepu Dodawanie nowych albumów do aplikacji.

Podobnie jak w przypadku korzystania z funkcji edycji, należy zaimplementować scenariusz tworzenia przy użyciu dwóch oddzielnych metod w klasie **StoreManagerController** :

1. Jedna z metod akcji będzie wyświetlała pusty formularz, gdy menedżerowie sklepu najpierw odwiedzają adres URL **/StoreManager/Create** .
2. Druga metoda działania będzie obsługiwać scenariusz, w którym Menedżer sklepu klika przycisk **Zapisz** w formularzu i przesyła wartości z powrotem do adresu URL **/StoreManager/Create** jako http-post.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Zadanie 1 — implementacja metody działania HTTP-GET Create

W tym zadaniu zostanie zaimplementowana wersja protokołu HTTP-GET metody Create Action w celu pobrania listy wszystkich gatunków i artystów, spakowanie tych danych w obiekt **StoreManagerViewModel** , który zostanie następnie przesłany do szablonu widoku.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/EX4-AddingACreateView/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz klasę **StoreManagerController** . Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.
3. Zamień kod metody **tworzenia** akcji na następujący:

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — EX4 STOREMANAGERCONTROLLER HTTP-GET Create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Zadanie 2 — Dodawanie widoku

W tym zadaniu dodasz szablon Utwórz widok, który będzie wyświetlał nowy (pusty) formularz albumu.

1. Kliknij prawym przyciskiem myszy wewnątrz metody **tworzenia** akcji i wybierz polecenie **Dodaj widok**. Spowoduje to wyświetlenie okna dialogowego Dodawanie widoku.
2. W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **utworzona**. Wybierz opcję **Utwórz widok z silną typem** i wybierz pozycję **album (MvcMusicStore. models)** z listy rozwijanej **Klasa modelu** i **Utwórz** z listy rozwijanej **szablon szkieletu** . Pozostaw pozostałe pola wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-CREATE-VIEW. png")

    *Dodawanie widoku*
3. Zaktualizuj pola **GenreId** i **ArtistId** , aby użyć listy rozwijanej, jak pokazano poniżej:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — Uruchamianie aplikacji

W tym zadaniu sprawdzisz, że na stronie **Tworzenie** widoku **magazynumanager** zostanie wyświetlony pusty formularz albumu.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager/Create**. Sprawdź, czy jest wyświetlany pusty formularz służący do wypełniania właściwości nowego albumu.

    ![Utwórz widok z pustym formularzem](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Utwórz widok z pustym formularzem")

    *Utwórz widok z pustym formularzem*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Zadanie 4 — implementacja metody działania HTTP-POST Create

W tym zadaniu zostanie zaimplementowana wersja HTTP-POST metody Create Action, która będzie wywoływana, gdy użytkownik kliknie przycisk **Zapisz** . Metoda powinna zapisać nowy album w bazie danych.

1. W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio. Otwórz klasę **StoreManagerController** . Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.
2. Zamień kod metody akcji **Create http-post** na następujący:

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularze i sprawdzanie poprawności — EX4 STOREMANAGERCONTROLLER http-post create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > Akcja tworzenia jest całkiem podobna do poprzedniej metody edycji akcji, ale zamiast ustawiania obiektu jako zmodyfikowany, jest dodawany do kontekstu.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowana, że strona **Tworzenie widoku magazynumanager** umożliwia utworzenie nowego albumu, a następnie przekierowanie do widoku indeksu magazynumanager.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager/Create**. Wypełnij wszystkie pola formularza danymi dla nowego albumu, tak jak na poniższej ilustracji:

    ![Tworzenie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Tworzenie albumu")

    *Tworzenie albumu*
3. Upewnij się, że nastąpi przekierowanie do widoku indeksu Magazynumanager zawierającego nowy album właśnie utworzony.

    ![Utworzono nowy album](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Utworzono nowy album")

    *Utworzono nowy album*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Ćwiczenie 5: obsługa usuwania

Możliwość usuwania albumów nie została jeszcze zaimplementowana. W tym ćwiczeniu będzie to możliwe. Podobnie jak przed, zaimplementowano scenariusz usuwania przy użyciu dwóch oddzielnych metod w klasie **StoreManagerController** :

1. Jedna metoda akcji będzie wyświetlać formularz potwierdzenia
2. Druga metoda działania będzie obsługiwać przesyłanie formularza

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Zadanie 1 — implementacja metody akcji usuwania HTTP-GET

W tym zadaniu zostanie zaimplementowana wersja protokołu HTTP-GET metody usuwania akcji w celu pobrania informacji o albumie.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex5-HandlingDeletion/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz klasę **StoreManagerController** . Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.
3. Akcja Usuń kontroler jest dokładnie taka sama jak akcja kontrolera poprzedniego sklepu szczegóły magazynu: wysyła zapytanie do obiektu **albumu** z bazy danych przy użyciu **identyfikatora** POdanego w adresie URL i zwraca odpowiedni **Widok**. Aby to zrobić, Zastąp kod metody akcji HTTP-GET **delete** następującym:

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularzy i walidacji — usuwanie Ex5 obsługa operacji HTTP-GET Delete*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Kliknij prawym przyciskiem myszy wewnątrz metody **usuwania** akcji i wybierz polecenie **Dodaj widok**. Spowoduje to wyświetlenie okna dialogowego Dodawanie widoku.
5. W oknie dialogowym Dodawanie widoku Sprawdź, czy nazwa widoku jest **usuwana**. Wybierz opcję **Utwórz widok z silną typem** i wybierz pozycję **album (MvcMusicStore. models)** z listy rozwijanej **Klasa modelu** . Wybierz pozycję **Usuń** z listy rozwijanej **szablon szkieletu** . Pozostaw pozostałe pola wartościami domyślnymi, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku usuwania](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Dodawanie widoku usuwania")

    *Dodawanie widoku usuwania*
6. Szablon Usuń zawiera wszystkie pola z modelu. Zostanie wyświetlony tylko tytuł albumu. Aby to zrobić, Zastąp zawartość widoku następującym kodem:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — Uruchamianie aplikacji

W tym zadaniu przetestujesz, że na stronie **usuwanie** widoku **magazynumanager** zostanie wyświetlony formularz usuwania potwierdzenia.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager**. Wybierz jeden z albumów do usunięcia, klikając przycisk **Usuń** i sprawdź, czy nowy widok został przekazany.

    ![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Usuwanie albumu")

    *Usuwanie albumu*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Zadanie 3 — Implementacja metody akcji HTTP-POST Delete

W tym zadaniu zostanie zaimplementowana wersja HTTP-POST metody usuwania akcji, która będzie wywoływana, gdy użytkownik kliknie przycisk **Usuń** . Metoda powinna usunąć album w bazie danych.

1. W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio. Otwórz klasę **StoreManagerController** . Aby to zrobić, rozwiń folder **controllers** , a następnie kliknij dwukrotnie pozycję **StoreManagerController.cs**.
2. Zastąp kod metody akcji **usuwania http-post** następującym:

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularzy i weryfikacji — Ex5 obsługa usuwania operacji http-post*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowana Strona widok **usuwania widoku magazynumanager** umożliwia usunięcie albumu, a następnie przekierowanie do widoku indeksu magazynumanager.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager**. Wybierz jeden album do usunięcia, klikając przycisk **Usuń.** Potwierdź usunięcie, klikając przycisk **Usuń** .

    ![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Usuwanie albumu")

    *Usuwanie albumu*
3. Sprawdź, czy album został usunięty, ponieważ nie jest wyświetlany na stronie **indeksu** .

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Ćwiczenie 6: Dodawanie walidacji

Obecnie w miejscu tworzenia i edytowania formularzy nie ma żadnego rodzaju walidacji. Jeśli użytkownik pozostawi pole wymagane puste lub wpisz litery w polu Cena, pierwszy z nich zostanie pobrany z bazy danych.

Możesz dodać weryfikację do aplikacji, dodając adnotacje danych do klasy modelu. Adnotacje danych umożliwiają opisywanie reguł, które mają być stosowane do właściwości modelu, a ASP.NET MVC zajmie się wymuszeniem i wyświetleniem odpowiedniego komunikatu dla użytkowników.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Zadanie 1 — Dodawanie adnotacji do danych

W tym zadaniu zostaną dodane adnotacje danych do modelu albumu, który spowoduje, że w razie potrzeby będzie wyświetlała się Strona tworzenie i edytowanie.

W przypadku prostej klasy modelu Dodawanie adnotacji do danych jest obsługiwane tylko przez dodanie instrukcji **using** dla elementu **System. ComponentModel. DataAnnotation**, a następnie umieszczenie atrybutu **[Required]** w odpowiednich właściwościach. W poniższym przykładzie właściwość **name** jest wymagana w widoku.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Jest to nieco bardziej skomplikowany sposób, jak w przypadku tej aplikacji, w której jest generowana Entity Data Model. Jeśli dodano adnotacje do danych bezpośrednio do klas modelu, zostaną one nadpisywane, jeśli zaktualizujesz model z bazy danych. Zamiast tego można użyć klas częściowych metadanych, które będą istniały do przechowywania adnotacji i są skojarzone z klasami modelu przy użyciu atrybutu **[metadatatype]** .

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex6-AddingValidation/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **album.cs** z folderu **modele** .
3. Zamień zawartość **album.cs** na wyróżniony kod, tak aby wyglądał wyglądać następująco:

    > [!NOTE]
    > Wiersz **[DisplayFormat (ConvertEmptyStringToNull = false)]** wskazuje, że puste ciągi z modelu nie będą konwertowane na wartość null, gdy pole danych zostanie zaktualizowane w źródle danych. To ustawienie pozwala uniknąć wyjątku, gdy Entity Framework przypisuje wartości null do modelu, zanim adnotacja danych potwierdzi pola.

    (Fragment kodu- *ASP.NET pomocników MVC 4 i formularzy i weryfikacji — Ex6 — Klasa częściowa metadanych albumu*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Ta klasa częściowego **albumu** ma atrybut **datadatatype** , który wskazuje klasę **AlbumMetaData** dla adnotacji danych. Oto niektóre z atrybutów adnotacji danych, które są używane do dodawania adnotacji do modelu albumu:
    > 
    > - Wymagane — wskazuje, że właściwość jest polem wymaganym
    > - DisplayName — definiuje tekst, który ma być używany dla pól formularza i komunikatów weryfikacji
    > - DisplayFormat — określa sposób wyświetlania i formatowania pól danych.
    > - StringLength — definiuje maksymalną długość pola ciągu
    > - Zakres — daje maksymalną i minimalną wartość pola liczbowego
    > - ScaffoldColumn — umożliwia ukrywanie pól z formularzy edytora

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — Uruchamianie aplikacji

W tym zadaniu sprawdzisz, czy pola Utwórz i edytuj weryfikują strony przy użyciu nazw wyświetlanych wybranych w ostatnim zadaniu.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/StoreManager/Create**. Sprawdź, czy nazwy wyświetlane są zgodne z tymi w klasie częściowej (na przykład **adres URL grafiki albumu** zamiast **AlbumArtUrl**)
3. Kliknij przycisk **Utwórz**, bez wypełniania formularza. Sprawdź, czy są wyświetlone odpowiednie komunikaty weryfikacyjne.

    ![Zweryfikowane pola na stronie tworzenia](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Zweryfikowane pola na stronie tworzenia")

    *Zweryfikowane pola na stronie tworzenia*
4. Możesz sprawdzić, czy na stronie **edytowania** jest to samo. Zmień adres URL na **/StoreManager/Edit/1** i upewnij się, że nazwy wyświetlane są zgodne z tymi w klasie częściowej (na przykład **adres URL grafiki albumu** zamiast **AlbumArtUrl**). Opróżnij pola **tytuł** i **Cena** , a następnie kliknij przycisk **Zapisz**. Sprawdź, czy są wyświetlone odpowiednie komunikaty weryfikacyjne.

    ![Zweryfikowane pola na stronie edytowania](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Zweryfikowane pola na stronie edytowania*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Ćwiczenie 7: korzystanie z niezauważalnej jQuery na stronie klienta

W tym ćwiczeniu dowiesz się, jak włączyć sprawdzanie poprawności jQuery na platformie MVC 4 po stronie klienta.

> [!NOTE]
> Niewygodna jQuery używa prefiksu danych-AJAX języka JavaScript do wywoływania metod akcji na serwerze, a nie w sposób niezauważalny emituje wbudowane skrypty klienta.

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Zadanie 1 — Uruchamianie aplikacji przed włączeniem dyskretnej jQuery

W tym zadaniu zostanie uruchomiona aplikacja przed dołączeniem jQuery w celu porównania obu modeli walidacji.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex7-UnobtrusivejQueryValidation/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Naciśnij klawisz **F5**, aby uruchomić aplikację.
3. Projekt zostanie uruchomiony na stronie głównej. Przeglądaj **/StoreManager/Create** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy są odbierane komunikaty weryfikacji:

    ![Weryfikacja klienta wyłączona](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Weryfikacja klienta wyłączona")

    *Weryfikacja klienta wyłączona*
4. W przeglądarce Otwórz kod źródłowy HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Zadanie 2 — Włączanie niezauważalnej weryfikacji klienta

W tym zadaniu zostanie włączone **niezauważalne sprawdzanie poprawności klienta** w usłudze jQuery z pliku **Web. config** , który domyślnie ma wartość false we wszystkich nowych projektach ASP.NET MVC 4. Ponadto dodasz niezbędne odwołania do skryptów, aby umożliwić niewygodną weryfikację klienta w technologii jQuery.

1. Otwórz plik **Web. config** w katalogu głównym projektu i upewnij się, że wartości kluczy **ClientValidationEnabled** i **UnobtrusiveJavaScriptEnabled** są ustawione na **wartość true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > Możesz również włączyć sprawdzanie poprawności klienta przez kod w Global.asax.cs, aby uzyskać te same wyniki:
    > 
    > **HtmlHelper. ClientValidationEnabled = true;**
    > 
    > Ponadto można przypisać atrybut ClientValidationEnabled do dowolnego kontrolera, aby mieć niestandardowe zachowanie.
2. Otwórz **Create. cshtml** w **Views\StoreManager**.
3. Upewnij się, że następujące pliki skryptów, **jQuery. Validate** i **jQuery. Validate**, są przywoływane w widoku za pomocą pakietu &quot; **~/bundles/jqueryval**&quot;.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Wszystkie te biblioteki jQuery są zawarte w nowych projektach MVC 4. Więcej bibliotek można znaleźć w folderze **/scripts** projektu.
    > 
    > Aby te biblioteki sprawdzania poprawności działały, należy dodać odwołanie do biblioteki platformy jQuery. Ponieważ to odwołanie zostało już dodane w pliku **\_Layout. cshtml** , nie trzeba go dodawać w tym konkretnym widoku.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Zadanie 3 — Uruchamianie aplikacji przy użyciu niezauważalnej weryfikacji jQuery

W tym zadaniu zostanie przetestowana, że szablon Tworzenie widoku programu **storemanager** wykonuje weryfikację po stronie klienta przy użyciu bibliotek jQuery, gdy użytkownik tworzy nowy album.

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Przeglądaj **/StoreManager/Create** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy są odbierane komunikaty weryfikacji:

    ![Sprawdzanie poprawności klienta przy włączonej platformie jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Sprawdzanie poprawności klienta przy włączonej platformie jQuery")

    *Sprawdzanie poprawności klienta przy włączonej platformie jQuery*
3. W przeglądarce Otwórz kod źródłowy dla tworzenia widoku:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Dla każdej reguły walidacji klienta niezauważalne jQuery dodaje atrybut z danymi-Val-*rulename*=&quot;*komunikatów*&quot;. Poniżej znajduje się lista tagów niezauważalnych przez jQuery wstawianych do pola wejściowego HTML w celu przeprowadzenia weryfikacji klienta:
   > 
   > - Dane — Val
   > - Data-Val-Number
   > - Data-Val-Range
   > - Data-Val-Range-min/Data-Val-Range-Max
   > - Data-Val — wymagane
   > - Data-Val — Długość
   > - Data-Val-Length-Max/Data-Val-Length — min
   > 
   > Wszystkie wartości danych są wypełnione **adnotacjami danych**modelu. Następnie Cała logika, która działa po stronie serwera, może być uruchamiana po stronie klienta. Na przykład atrybut Price ma następującą adnotację danych w modelu:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Po użyciu dyskretnej technologii jQuery wygenerowany kod jest:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Dzięki zakończeniu tego praktycznego laboratorium wiesz już, jak umożliwić użytkownikom zmianę danych przechowywanych w bazie danych przy użyciu następujących elementów:

- Akcje kontrolera, takie jak indeks, tworzenie, edytowanie, usuwanie
- Funkcja szkieletu ASP.NET MVC do wyświetlania właściwości w tabeli HTML
- Niestandardowi pomocnicy HTML do ulepszania środowiska użytkownika
- Metody akcji, które reagują na wywołania HTTP-GET lub HTTP-POST
- Szablon edytora udostępnionego dla podobnych szablonów widoku, takich jak tworzenie i edytowanie
- Elementy formularza, takie jak listy rozwijane
- Adnotacje danych na potrzeby walidacji modelu
- Sprawdzanie poprawności po stronie klienta przy użyciu niedyskretnej biblioteki jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *Kafelek VS Express for Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
2. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

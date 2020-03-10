---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 — podstawy | Microsoft Docs
author: rick-anderson
description: To praktyczne laboratorium jest oparte na sklepie MVC (Model View Controller), aplikacji samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598820"
---
# <a name="aspnet-mvc-4-fundamentals"></a>ASP.NET MVC 4 — podstawy

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

To praktyczne laboratorium jest oparte na sklepie MVC (Model View Controller) — aplikacji samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i programu Visual Studio. W całym laboratorium uczysz się prostoty, a tym samym korzystać z tych technologii jednocześnie. Zaczniesz korzystać z prostej aplikacji i utworzymy ją do momentu, gdy będzie ona w pełni funkcjonalna ASP.NET aplikacji sieci Web MVC 4.

To laboratorium działa z ASP.NET MVC 4.

Jeśli chcesz poznać wersję ASP.NET MVC 3 aplikacji samouczka, możesz ją znaleźć w [sklepie MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

W ramach tego praktycznego laboratorium przyjęto założenie, że deweloper ma doświadczenie w technologiach programistycznych dla sieci Web, takich jak HTML i JavaScript.

> [!NOTE]
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [wersjach Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt charakterystyczny dla tego laboratorium jest dostępny pod adresem [ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>Aplikacja ze sklepu muzycznego

Aplikacja sieci Web do przechowywania utworów muzycznych, która będzie skompilowana w ramach tego laboratorium, obejmuje trzy główne części: zakupy, Wyewidencjonowywanie i administrowanie. Osoby odwiedzające będą mogły przeglądać albumy według gatunku, dodawać albumy do swojego koszyka, sprawdzać ich wybór, a następnie przejść do wyewidencjonowania, aby zalogować się i zakończyć zamówienie. Ponadto administratorzy sklepu będą mogli zarządzać dostępnymi albumami oraz ich głównymi właściwościami.

![Ekrany sklepu muzycznego](aspnet-mvc-4-fundamentals/_static/image1.png "Ekrany sklepu muzycznego")

*Ekrany sklepu muzycznego*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

Aplikacja ze sklepu muzycznego będzie skompilowana przy użyciu **kontrolera widoku modelu (MVC)** , wzorca architektury, który oddziela aplikację do trzech głównych składników:

- **Modele**: obiekty modelu są częściami aplikacji, które implementują logikę domeny. Często obiekty modelu również pobierają i przechowują stan modelu w bazie danych.
- **Widoki:** Widoki to składniki, które wyświetlają interfejs użytkownika aplikacji. Zazwyczaj interfejs ten jest tworzony na podstawie danych modelu. Przykładem może być widok do edycji albumów, który wyświetla pola tekstowe i listę rozwijaną na podstawie bieżącego stanu obiektu albumu.
- **Kontrolery:** Kontrolery to składniki obsługujące interakcję z użytkownikiem, manipulowanie modelem i ostatecznie Wybieranie widoku do renderowania interfejsu użytkownika. W aplikacji MVC widok służy wyłącznie do wyświetlania informacji. Za obsługę danych wprowadzanych przez użytkownika i interakcję z użytkownikiem odpowiada kontroler.

Wzorzec MVC ułatwia tworzenie aplikacji, które oddzielają różne aspekty aplikacji (logiki wejściowej, logiki biznesowej i logiki interfejsu użytkownika), jednocześnie zapewniając swobodny sprzężenie między tymi elementami. Ta separacja ułatwia zarządzanie złożonością podczas kompilowania aplikacji, ponieważ umożliwia skoncentrowanie się na jednym z aspektów implementacji w danym momencie. Ponadto wzorzec MVC ułatwia testowanie aplikacji, ułatwiając korzystanie z programowania opartego na testach (TDD) do tworzenia aplikacji.

Struktura **ASP.NET MVC** stanowi alternatywę dla wzorca formularzy sieci Web ASP.NET do tworzenia ASP.NET aplikacji sieci Web opartych na MVC. **ASP.NET MVC** Framework jest lekkim i wysoce weryfikowalne strukturą prezentacji, która (podobnie jak w przypadku aplikacji opartych na formularzach internetowych) jest zintegrowana z istniejącymi funkcjami ASP.NET, takimi jak strony główne i uwierzytelnianie oparte na członkostwie, dzięki czemu można korzystać z zalet podstawowego programu .NET Framework. Jest to przydatne, jeśli znasz już ASP.NET formularze sieci Web, ponieważ wszystkie używane biblioteki są również dostępne w ASP.NET MVC 4.

Ponadto luźne sprzęganie między trzema głównymi składnikami aplikacji MVC również wspiera programowanie równoległe. Na przykład jeden deweloper może pracować w widoku, drugi deweloper może pracować na logice kontrolera, a trzeci deweloper może skupić się na logice biznesowej w modelu.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Tworzenie aplikacji ASP.NET MVC od podstaw w oparciu o samouczek aplikacji do sklepu muzycznego
- Dodaj kontrolery, aby obsługiwać adresy URL na stronie głównej witryny i przeglądać jej główne funkcje
- Dodaj widok, aby dostosować wyświetlaną zawartość wraz z jej stylem
- Dodawanie klas modelu w celu uwzględnienia danych i logiki domeny oraz zarządzania nimi
- Używanie wzorca widoku do przekazywania informacji z akcji kontrolera do szablonów widoków
- Poznaj nowy szablon ASP.NET MVC 4 dla aplikacji internetowych

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje dotyczące sposobu jego instalacji)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku C: Using fragmenty kodu](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web MusicStore ASP.NET MVC](#Exercise1)
2. [Ćwiczenie 2: Tworzenie kontrolera](#Exercise2)
3. [Ćwiczenie 3: przekazywanie parametrów do kontrolera](#Exercise3)
4. [Ćwiczenie 4: Tworzenie widoku](#Exercise4)
5. [Ćwiczenie 5. Tworzenie modelu widoku](#Exercise5)
6. [Ćwiczenie 6: Używanie parametrów w widoku](#Exercise6)
7. [Ćwiczenie 7: kolano wokół ASP.NET MVC 4 nowy szablon](#Exercise7)

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

Szacowany czas wykonywania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Ćwiczenie 1: Tworzenie projektu aplikacji sieci Web MusicStore ASP.NET MVC

W tym ćwiczeniu dowiesz się, jak utworzyć aplikację ASP.NET MVC w programie Visual Studio 2012 Express for Web oraz jej organizacji głównego folderu. Ponadto dowiesz się, jak dodać nowy kontroler i wyświetlić prosty ciąg na stronie głównej aplikacji.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Zadanie 1 — Tworzenie projektu aplikacji sieci Web ASP.NET MVC

1. W tym zadaniu utworzysz pusty projekt aplikacji ASP.NET MVC przy użyciu szablonu MVC Visual Studio. Rozpocznij **vs Express for Web**.
2. W menu **plik** kliknij pozycję **Nowy projekt**.
3. W oknie dialogowym **Nowy projekt** wybierz typ projektu **aplikacja sieci Web MVC 4 ASP.NET** , który znajduje się w obszarze **Wizualizacja C#,** Lista szablonów **sieci Web** .
4. Zmień **nazwę** na *MvcMusicStore*.
5. Ustaw lokalizację rozwiązania w nowym folderze **BEGIN** w folderze źródłowym tego ćwiczenia, na przykład **[Twoje-hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**. Kliknij przycisk **OK**.

    ![Okno dialogowe Tworzenie nowego projektu](aspnet-mvc-4-fundamentals/_static/image2.png "Okno dialogowe Tworzenie nowego projektu")

    *Okno dialogowe Tworzenie nowego projektu*
6. W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon **podstawowy** i upewnij się, że wybrany **aparat widoku** to **Razor**. Kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Okno dialogowe Nowy projekt ASP.NET MVC 4")

    *Okno dialogowe Nowy projekt ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Zadanie 2 — Eksplorowanie struktury rozwiązania

Struktura ASP.NET MVC obejmuje szablon projektu programu Visual Studio, który ułatwia tworzenie aplikacji sieci Web obsługujących wzorzec MVC. Ten szablon służy do tworzenia nowej aplikacji sieci Web ASP.NET MVC z wymaganymi folderami, szablonami elementów i wpisami plików konfiguracji.

W tym zadaniu sprawdzisz strukturę rozwiązania, aby poznać elementy, które są objęte usługą, i ich relacje. Następujące foldery są dołączone do wszystkich aplikacji ASP.NET MVC, ponieważ struktura ASP.NET MVC domyślnie używa konwencji &quot;w porównaniu z podejściem&quot; konfiguracji i wprowadza pewne domyślne założenia na podstawie konwencji nazewnictwa folderów.

1. Po utworzeniu projektu Przejrzyj strukturę folderów utworzoną w Eksplorator rozwiązań po prawej stronie:

    ![Struktura folderu ASP.NET MVC w Eksplorator rozwiązań](aspnet-mvc-4-fundamentals/_static/image4.png "Struktura folderu ASP.NET MVC w Eksplorator rozwiązań")

    *Struktura folderu ASP.NET MVC w Eksplorator rozwiązań*

   1. **Kontrolery**. Ten folder będzie zawierać klasy kontrolerów. W aplikacji opartej na platformie MVC kontrolery są odpowiedzialne za obsługę interakcji z użytkownikami końcowymi, manipulowanie modelem i ostatecznie Wybieranie widoku w celu renderowania interfejsu użytkownika.

       > [!NOTE]
       > Struktura MVC wymaga, aby nazwy wszystkich kontrolerów były kończyć się &quot;kontrolerem&quot;— na przykład HomeController, LoginController lub ProductController.
   2. **Modele**. Ten folder jest udostępniany dla klas, które reprezentują model aplikacji dla aplikacji sieci Web MVC. Obejmuje to zazwyczaj kod definiujący obiekty i logikę do współpracy z magazynem danych. Zazwyczaj rzeczywiste obiekty modelu będą znajdować się w osobnych bibliotekach klas. Jednak podczas tworzenia nowej aplikacji można uwzględnić klasy, a następnie przenieść je do oddzielnych bibliotek klas w późniejszym czasie w cyklu projektowania.
   3. **Widoki**. Ten folder jest zalecaną lokalizacją widoków, składnikami odpowiedzialnymi za wyświetlanie interfejsu użytkownika aplikacji. Widoki używają plików. aspx,. ascx,. cshtml i. Master, oprócz innych plików, które są powiązane z widokami renderowania. Folder widoki zawiera folder dla każdego kontrolera; folder nosi nazwę z prefiksem nazwy kontrolera. Na przykład jeśli masz kontroler o nazwie **HomeController**, folder widoki będzie zawierać folder o nazwie Home. Domyślnie, gdy struktura ASP.NET MVC ładuje widok, szuka pliku. aspx z żądaną nazwą widoku w folderze Views\controllerName (**widoki [ControllerName] [Action]. aspx**) lub (**widoki [ControllerName] [Action]. cshtml**) dla widoków Razor.

      > [!NOTE]
      > Oprócz folderów wymienionych wcześniej aplikacja sieci Web MVC używa pliku **Global. asax** do ustawiania ustawień globalnych routingu adresów URL i używa pliku **Web. config** w celu skonfigurowania aplikacji.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Zadanie 3 — Dodawanie elementu HomeController

W aplikacjach ASP.NET, które nie korzystają z platformy MVC, interakcja użytkownika jest zorganizowana wokół stron i wokół zdarzeń z tych stron. Z kolei Interakcje użytkownika z aplikacjami ASP.NET MVC są zorganizowane wokół kontrolerów i ich metod działania.

Z drugiej strony ASP.NET MVC Framework mapuje adresy URL na klasy, które są określane jako kontrolery. Kontrolery przetwarzają żądania przychodzące, obsługują dane wejściowe użytkownika i interakcje, wykonują odpowiednią logikę aplikacji i określają odpowiedź do wysłania do klienta (wyświetla kod HTML, pobiera plik, przekierowuje do innego adresu URL itp.). W przypadku wyświetlania kodu HTML Klasa kontrolera zazwyczaj wywołuje oddzielny składnik widoku w celu wygenerowania znacznika HTML dla żądania. W aplikacji MVC widok służy wyłącznie do wyświetlania informacji. Za obsługę danych wprowadzanych przez użytkownika i interakcję z użytkownikiem odpowiada kontroler.

W tym zadaniu zostanie dodana klasa kontrolera, która będzie obsługiwać adresy URL na stronie głównej witryny sklepu muzyczne.

1. Kliknij prawym przyciskiem myszy folder **controllers** w obszarze Eksplorator rozwiązań wybierz pozycję **Dodaj** , a następnie polecenie **Controller** :

    ![Dodaj polecenie kontrolera](aspnet-mvc-4-fundamentals/_static/image5.png "Dodaj polecenie kontrolera")

    *Dodaj kontroler — polecenie*
2. Zostanie wyświetlone okno dialogowe **Dodaj kontroler** . Nazwij *HomeController* kontrolera i naciśnij przycisk **Add (Dodaj**).

    ![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-fundamentals/_static/image6.png "Okno dialogowe Dodawanie kontrolera")

    *Okno dialogowe Dodawanie kontrolera*
3. Plik **HomeController.cs** jest tworzony w folderze **controllers** . Aby **HomeController** zwracał ciąg w akcji indeksu, Zastąp metodę **index** następującym kodem:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex1 HomeController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — Uruchamianie aplikacji

To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację. Projekt jest kompilowany i uruchamiany jest lokalny serwer sieci Web usług IIS. Lokalny serwer sieci Web usług IIS automatycznie otworzy przeglądarkę internetową wskazującą adres URL serwera sieci Web.

    ![Aplikacja działająca w przeglądarce sieci Web](aspnet-mvc-4-fundamentals/_static/image7.png "Aplikacja działająca w przeglądarce sieci Web")

    *Aplikacja działająca w przeglądarce sieci Web*

    > [!NOTE]
    > Lokalny serwer sieci Web usług IIS uruchomi witrynę internetową przy użyciu losowego numeru portu. Na powyższej ilustracji lokacja jest uruchomiona w `http://localhost:50103/`, więc używa portu 50103. Numer portu może się różnić.
2. Zamknij okno przeglądarki.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Ćwiczenie 2: Tworzenie kontrolera

W tym ćwiczeniu dowiesz się, jak zaktualizować kontroler w celu zaimplementowania prostych funkcji aplikacji do sklepu muzycznego. Ten kontroler określi metody akcji do obsługi każdego z następujących konkretnych żądań:

- Strona z listą gatunku muzyka w sklepie Music
- Strona przeglądania zawierająca listę wszystkich albumów muzycznych dla określonego gatunku
- Strona szczegółów, która zawiera informacje o konkretnym albumie muzycznym

Dla zakresu tego ćwiczenia te akcje będą po prostu zwracać ciąg przez teraz.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Zadanie 1 — Dodawanie nowej klasy StoreController

W tym zadaniu zostanie dodany nowy kontroler.

1. Jeśli nie jest jeszcze otwarty, uruchom **VS Express for Web 2012**.
2. W menu **plik** wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt przejdź do **Source\Ex02-CreatingAController\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**. Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Dodaj nowy kontroler. Aby to zrobić, kliknij prawym przyciskiem myszy folder **controllers** w obszarze Eksplorator rozwiązań wybierz pozycję **Dodaj** , a następnie polecenie **kontroler** . Zmień **nazwę kontrolera** na *StoreController*, a następnie kliknij przycisk **Dodaj**.

    ![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-fundamentals/_static/image8.png "Okno dialogowe Dodawanie kontrolera")

    *Okno dialogowe Dodawanie kontrolera*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Zadanie 2 — Modyfikowanie akcji StoreController

W tym zadaniu zmodyfikujesz metody kontrolera, które są nazywane **akcjami**. Akcje są odpowiedzialne za obsługę żądań adresów URL i Określanie zawartości, która powinna zostać wysłana z powrotem do przeglądarki lub użytkownika, który wywołał adres URL.

1. Klasa **StoreController** ma już metodę **index** . Zostanie ona użyta w dalszej części tego laboratorium do zaimplementowania strony, która wyświetla listę wszystkich gatunków sklepu muzycznego. Na razie po prostu Zastąp metodę **index** następującym kodem, który zwraca ciąg &quot;Hello z elementu Store. index ()&quot;:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex2 StoreController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Dodawanie metod **przeglądania** i **szczegółów** . Aby to zrobić, Dodaj następujący kod do **StoreController**:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — Uruchamianie aplikacji

To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie **głównej** . Zmień adres URL, aby zweryfikować implementację każdej akcji.

    1. **/Store**. Zobaczysz, **&quot;Hello z&quot;Store. index ()** .
    2. **/Store/Browse**. Zostanie wyświetlona **&quot;Hello ze sklepu. Browse ()&quot;** .
    3. **/Store/Details**. Zobaczysz, **&quot;Hello ze sklepu. Details ()&quot;** .

        ![Przeglądanie StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Przeglądanie StoreBrowse")

        *Przeglądanie/Store/Browse*
3. Zamknij okno przeglądarki.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Ćwiczenie 3: przekazywanie parametrów do kontrolera

Do tej pory zostały zwrócone ciągi stałe z kontrolerów. W tym ćwiczeniu dowiesz się, jak przekazać parametry do kontrolera przy użyciu adresu URL i ciągu QueryString, a następnie wykonać akcje metody reagują na tekst do przeglądarki.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Zadanie 1 — Dodawanie parametru gatunku do StoreController

W tym zadaniu zostanie użyta **Kolekcja QueryString** w celu wysłania parametrów do metody **Browse** Action w **StoreController**.

1. Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**.
2. W menu **plik** wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt przejdź do **Source\Ex03-PassingParametersToAController\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**. Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Otwórz klasę **StoreController** . W tym celu w **Eksplorator rozwiązań**rozwiń folder **Kontrolery** i kliknij dwukrotnie pozycję **StoreController.cs**.
4. Zmień metodę **przeglądania** , dodając parametr ciągu do żądania dla określonego gatunku. ASP.NET MVC automatycznie przekaże wszystkie **Parametry w postaci** ciągu lub formularza post do tej metody akcji po wywołaniu. Aby to zrobić, Zastąp metodę **Browse** następującym kodem:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Używasz metody narzędziowej **HttpUtility. HtmlEncode** , aby uniemożliwiać użytkownikom wprowadzanie kodu JavaScript do widoku za pomocą linku, takiego jak **/Store/Browse? Gatunek =&lt;&gt;Window. Location = "[http://hackersite.com](http://hackersite.com)"&lt;/Script&gt;** .
> 
> Aby uzyskać więcej informacji, zobacz [ten artykuł w witrynie MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Zadanie 2 — Uruchamianie aplikacji

To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web i użycie parametru **gatunek** .

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie **głównej** . Czy zmienić adres URL na */Store/Browse? Gatunek = Disco* , aby sprawdzić, czy akcja odbiera parametr gatunku.

    ![Przeglądanie StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Przeglądanie StoreBrowseGenre = Disco")

    *Przeglądasz/Store/Browse? Gatunek = Disco*
3. Zamknij okno przeglądarki.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Zadanie 3 — Dodawanie parametru identyfikatora osadzonego w adresie URL

W tym zadaniu zostanie użyty **adres URL** do przekazania parametru **ID** do metody akcji **Details** elementu **StoreController**. Domyślna konwencja routingu ASP.NET MVC to traktowanie segmentu adresu URL po nazwie metody akcji jako parametru o nazwie **ID**. Jeśli metoda akcji ma parametr o nazwie ID, ASP.NET MVC automatycznie przekaże segment adresu URL jako parametr. W polu Magazyn adresów URL **/szczegóły/5** **Identyfikator** będzie interpretowany jako **5**.

1. Zmień metodę **Details** elementu **StoreController**, dodając parametr **int** o nazwie **ID**. Aby to zrobić, Zastąp metodę **Details** następującym kodem:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — Uruchamianie aplikacji

To zadanie spowoduje wypróbowanie aplikacji w przeglądarce sieci Web i użycie parametru **ID** .

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie **głównej** . Zmień adres URL na */Store/Details/5* , aby sprawdzić, czy akcja odbiera parametr ID.

    ![Przeglądanie StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Przeglądanie StoreDetails5")

    *Przeglądanie/Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Ćwiczenie 4: Tworzenie widoku

Do tej pory zostały zwrócone ciągi z akcji kontrolera. Chociaż jest to przydatny sposób na zrozumienie sposobu działania kontrolerów, nie jest to sposób, w jaki są kompilowane prawdziwe aplikacje sieci Web. Widoki to składniki, które zapewniają lepsze podejście do generowania kodu HTML z powrotem do przeglądarki przy użyciu plików szablonów.

W tym ćwiczeniu dowiesz się, jak dodać stronę wzorcową układu, aby skonfigurować szablon dla typowej zawartości HTML, arkusz stylów, aby wzbogacić wygląd i działanie witryny, a wreszcie szablon widoku, aby umożliwić HomeController zwracanie kodu HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a>Zadanie 1 — Modyfikowanie pliku \_Layout. cshtml

Plik **~/Views/Shared/\_Layout. cshtml** umożliwia skonfigurowanie szablonu do użycia w całej witrynie sieci Web. W tym zadaniu zostanie dodana Strona wzorcowa układu ze wspólnym nagłówkiem z linkami do strony głównej i obszaru magazynu.

1. Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**.
2. W menu **plik** wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt przejdź do **Source\Ex04-CreatingAView\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**. Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Plik <strong>\_Layout. cshtml</strong> zawiera układ kontenera HTML dla wszystkich stron w witrynie. Zawiera element <strong>&lt;html&gt;</strong> dla odpowiedzi HTML, a także <strong>&lt;&gt;</strong> i <strong>&lt;treść</strong>&gt;. <strong>@RenderBody()</strong> w treści HTML Zidentyfikuj regiony, w których szablony będą mogły być wypełniane zawartością dynamiczną.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Dodaj wspólny nagłówek z linkami do strony głównej i obszaru przechowywania na wszystkich stronach w witrynie. Aby to zrobić, Dodaj następujący kod poniżej &lt;treści&gt; instrukcji.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Dołącz element DIV, aby renderować sekcję treści każdej strony. Zamień <strong>@RenderBody()</strong> na następujący wyróżniony kod: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > Czy wiesz? Program Visual Studio 2012 zawiera fragmenty, które ułatwiają dodawanie często używanego kodu w języku HTML, pliki kodu i inne elementy. Wypróbuj go, wpisując **&lt;div&gt;** i naciskając klawisz **Tab** dwa razy, aby wstawić kompletny tag **DIV** .

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Zadanie 2 — Dodawanie arkusza stylów CSS

Szablon pustego projektu zawiera bardzo uproszczony plik CSS, który zawiera tylko style używane do wyświetlania podstawowych formularzy i komunikatów weryfikacyjnych. Będziesz używać dodatkowych arkuszy CSS i obrazów (potencjalnie oferowanych przez projektanta) w celu zwiększenia wyglądu i działania witryny.

W tym zadaniu dodasz arkusz stylów CSS do definiowania stylów witryny.

1. Plik CSS i obrazy są zawarte w folderze **Source\Assets\Content** w tym laboratorium. Aby dodać je do aplikacji, przeciągnij jej zawartość z okna **Eksploratora Windows** do **Eksplorator rozwiązań** w Visual Studio Express dla sieci Web, jak pokazano poniżej:

    ![Przeciąganie zawartości stylu](aspnet-mvc-4-fundamentals/_static/image12.png "Przeciąganie zawartości stylu")

    *Przeciąganie zawartości stylu*
2. Zostanie wyświetlone okno dialogowe z ostrzeżeniem z prośbą o potwierdzenie zastąpienia pliku **site. css** i niektórych istniejących obrazów. Zaznacz pole wyboru **Zastosuj do wszystkich elementów** , a następnie kliknij przycisk **tak**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Zadanie 3 — Dodawanie szablonu widoku

W tym zadaniu dodasz szablon widoku w celu wygenerowania odpowiedzi HTML, która będzie używać strony wzorcowej układu i CSS dodanej w tym ćwiczeniu.

1. Aby użyć szablonu widoku podczas przeglądania strony głównej witryny, najpierw należy wskazać, że zamiast zwracać ciąg, Metoda **indeksu HomeController** zwróci **Widok**. Otwórz klasę **HomeController** i zmień jej metodę **index** w taki sposób, aby zwracała **ActionResult**i zwracała **Widok ()** .

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-EX4 HomeController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Teraz musisz dodać odpowiedni szablon widoku. Aby to zrobić, **kliknij prawym przyciskiem myszy** wewnątrz metody akcji **indeksu** i wybierz polecenie **Dodaj widok**. Spowoduje to wyświetlenie okna dialogowego **Dodawanie widoku** .

    ![Dodawanie widoku z poziomu metody index](aspnet-mvc-4-fundamentals/_static/image13.png "Dodawanie widoku z poziomu metody index")

    *Dodawanie widoku z poziomu metody index*
3. Zostanie wyświetlone okno dialogowe **Dodaj widok** , aby wygenerować plik szablonu widoku. Domyślnie to okno dialogowe wstępnie wypełnia nazwę szablonu widoku, aby była zgodna z metodą akcji, która będzie z niego korzystać. Ze względu na to, że w ramach metody akcji **index** w HomeController użyto menu kontekstowego **Dodaj** widok, w oknie dialogowym **Dodawanie widoku** znajduje się indeks jako domyślna nazwa widoku. Kliknij pozycję **Add** (Dodaj).

    ![Okno dialogowe Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image14.png "Okno dialogowe Dodawanie widoku")

    *Okno dialogowe Dodawanie widoku*
4. Program Visual Studio generuje szablon widoku **index. cshtml** w folderze **Views\Home** , a następnie go otwiera.

    ![Utworzono widok indeksu macierzystego](aspnet-mvc-4-fundamentals/_static/image15.png "Utworzono widok indeksu macierzystego")

    *Utworzono widok indeksu macierzystego*

    > [!NOTE]
    > Nazwa i lokalizacja pliku **index. cshtml** są odpowiednie i są zgodne z domyślnymi konwencjami nazewnictwa MVC ASP.NET.
    > 
    > Folder \Views\**Home** jest zgodny z nazwą kontrolera (kontrolerem**głównym** ). Nazwa szablonu widoku (**indeks**) jest zgodna z metodą akcji kontrolera, która będzie wyświetlać widok.
    > 
    > W ten sposób ASP.NET MVC unika konieczności jawnego określenia nazwy lub lokalizacji szablonu widoku w przypadku używania tej konwencji nazewnictwa do zwrócenia widoku.
5. Wygenerowany szablon widoku jest oparty na wcześniej zdefiniowanym szablonie **\_Layout. cshtml** . Zaktualizuj Właściwość ViewBag. title do strony **głównej**i zmień jej główną zawartość na **stronę główną**, jak pokazano w poniższym kodzie:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Wybierz projekt **MvcMusicStore** w Eksplorator rozwiązań a następnie naciśnij klawisz **F5** , aby uruchomić aplikację.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Zadanie 4. Weryfikacja

Aby sprawdzić, czy wszystkie kroki w poprzednim ćwiczeniu zostały wykonane prawidłowo, wykonaj następujące czynności:

W przypadku aplikacji otwartej w przeglądarce należy zauważyć, że:

1. Metoda akcji indeksu HomeController znaleziona i wyświetlana w szablonie widoku **\Views\Home\Index.cshtml** , nawet jeśli kod nosi nazwę **Return ()** , ponieważ szablon widoku przestrzega standardowej konwencji nazewnictwa.
2. Na stronie głównej zostanie wyświetlony komunikat powitalny zdefiniowany w szablonie widoku **\Views\Home\Index.cshtml** .
3. Na stronie głównej jest używany szablon **\_Layout. cshtml** , dlatego Komunikat powitalny jest zawarty w układzie HTML witryny standardowej.

    ![Widok indeksu macierzystego przy użyciu zdefiniowanego LayoutPage i stylu](aspnet-mvc-4-fundamentals/_static/image16.png "Widok indeksu macierzystego przy użyciu zdefiniowanego LayoutPage i stylu")

    *Widok indeksu macierzystego przy użyciu zdefiniowanego LayoutPage i stylu*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Ćwiczenie 5. Tworzenie modelu widoku

Do tej pory Twoje widoki wyświetlają kod HTML stałe, ale w celu utworzenia dynamicznych aplikacji sieci Web szablon widoku powinien otrzymywać informacje z kontrolera. Jedną z typowych technik do użycia w tym celu jest wzorzec **ViewModel** , który umożliwia kontrolerowi spakowanie wszystkich informacji potrzebnych do wygenerowania odpowiedniej odpowiedzi html.

W tym ćwiczeniu dowiesz się, jak utworzyć klasę ViewModel i dodać wymagane właściwości: liczbę gatunków w sklepie i listę tych gatunków. Można również zaktualizować StoreController, aby korzystał z utworzonych ViewModel, a wreszcie utworzyć nowy szablon widoku, który będzie wyświetlał wymienione właściwości na stronie.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Zadanie 1 — Tworzenie klasy ViewModel

W tym zadaniu utworzysz klasę ViewModel, która będzie implementować scenariusz tworzenia gatunku dla danego sklepu.

1. Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**.
2. W menu **plik** wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt przejdź do **Source\Ex05-CreatingAViewModel\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**. Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Utwórz folder **modele widoków** , aby pomieścić ViewModel. W tym celu kliknij prawym przyciskiem myszy projekt **MvcMusicStore** najwyższego poziomu, wybierz polecenie **Dodaj** , a następnie pozycję **Nowy folder**.

    ![Dodawanie nowego folderu](aspnet-mvc-4-fundamentals/_static/image17.png "Dodawanie nowego folderu")

    *Dodawanie nowego folderu*
4. Nazwij folder *modele widoków*.

    ![Folder modele widoków w Eksplorator rozwiązań](aspnet-mvc-4-fundamentals/_static/image18.png "Folder modele widoków w Eksplorator rozwiązań")

    *Folder modele widoków w Eksplorator rozwiązań*
5. Utwórz klasę **ViewModel** . Aby to zrobić, kliknij prawym przyciskiem myszy folder **modele widoków** , a następnie wybierz pozycję **Dodaj** , a następnie pozycję **nowy element**. W obszarze **kod**wybierz element **klasy** i Nazwij plik *StoreIndexViewModel.cs*, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie nowej klasy](aspnet-mvc-4-fundamentals/_static/image19.png "Dodawanie nowej klasy")

    *Dodawanie nowej klasy*

    ![Tworzenie klasy StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Tworzenie klasy StoreIndexViewModel")

    *Tworzenie klasy StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Zadanie 2 — Dodawanie właściwości do klasy ViewModel

Istnieją dwa parametry, które mają zostać przesłane z StoreController do szablonu widoku, aby wygenerować oczekiwaną odpowiedź HTML: liczbę gatunków w sklepie i listę tych gatunków.

W tym zadaniu dodasz te 2 właściwości do klasy **StoreIndexViewModel** : **NumberOfGenres** (Integer) i **gatunek** (lista ciągów).

1. Dodaj właściwości **NumberOfGenres** i **gatuneks** do klasy **StoreIndexViewModel** . Aby to zrobić, Dodaj następujące 2 wiersze do definicji klasy:

    (Fragment kodu- *ASP.NET MVC 4 podstawowe-Ex5 StoreIndexViewModel Properties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> W notacji **{get; set;}** jest używana C#funkcja właściwości, która została zaimplementowana. Zapewnia korzyści wynikające z właściwości bez konieczności deklarowania pola zapasowego.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Zadanie 3 — Aktualizowanie StoreController do korzystania z StoreIndexViewModel

Klasa **StoreIndexViewModel** hermetyzuje informacje, które są konieczne do przekazania z metody **indeksu** **StoreController**do szablonu widoku w celu wygenerowania odpowiedzi.

W tym zadaniu zaktualizujesz **StoreController** , aby użyć **StoreIndexViewModel**.

1. Otwórz klasę **StoreController** .

    ![Otwieranie klasy StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Otwieranie klasy StoreController")

    *Otwieranie klasy StoreController*
2. Aby można było użyć klasy **StoreIndexViewModel** z **StoreController**, Dodaj następującą przestrzeń nazw u góry kodu **StoreController** :

    (Fragment kodu- *ASP.NET MVC 4 — podstawowe-Ex5 StoreIndexViewModel za pomocą modele widoków*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Zmień metodę akcji **index** **StoreController**tak, aby tworzyła i wypełnia obiekt **StoreIndexViewModel** , a następnie przekazuje go do szablonu widoku w celu wygenerowania odpowiedzi html.

    > [!NOTE]
    > W programie Lab &quot;ASP.NET modele MVC i dostęp do danych&quot; napiszesz kod, który pobiera listę gatunków sklepu z bazy danych. W poniższym kodzie zostanie utworzona **Lista** fikcyjnych gatunków danych, które zapełnią **StoreIndexViewModel**.
    > 
    > Po utworzeniu i skonfigurowaniu obiektu **StoreIndexViewModel** zostanie on przesłany jako argument do metody **widoku** . Oznacza to, że szablon widoku będzie używać tego obiektu do generowania odpowiedzi HTML.
4. Zastąp metodę **index** następującym kodem:

    (Fragment kodu- *ASP.NET MVC 4 podstawowe-Ex5 StoreController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Jeśli nie znasz programu C#, możesz założyć, że użycie funkcji **var** oznacza, że zmienna **viewModel** jest opóźniona. To nie jest poprawne — C# kompilator używa wnioskowania typu w oparciu o przypisane elementy do zmiennej, aby określić, że **ViewModel** jest typu **StoreIndexViewModel**. Ponadto przez skompilowanie lokalnej zmiennej **viewModel** jako typu **StoreIndexViewModel** uzyskasz sprawdzanie czasu kompilowania i obsługę edytora kodu programu Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Zadanie 4 — Tworzenie szablonu widoku korzystającego z StoreIndexViewModel

W tym zadaniu utworzysz szablon widoku, który będzie używać obiektu StoreIndexViewModel przekazaną przez kontroler do wyświetlania listy gatunków.

1. Przed utworzeniem nowego szablonu widoku skompilujemy projekt, aby **okno dialogowe Dodawanie widoku** wie o klasie **StoreIndexViewModel** . Skompiluj projekt, wybierając element menu **kompilacja** , a następnie **Skompiluj MvcMusicStore**.

    ![Kompilowanie projektu](aspnet-mvc-4-fundamentals/_static/image22.png "Kompilowanie projektu")

    *Kompilowanie projektu*
2. Utwórz nowy szablon widoku. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz metody **index** i wybierz polecenie **Dodaj widok**.

    ![Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image23.png "Dodawanie widoku")

    *Dodawanie widoku*
3. Ponieważ **okno dialogowe Dodawanie widoku** zostało wywołane z **StoreController**, spowoduje to dodanie domyślnego szablonu widoku w pliku **\Views\Store\Index.cshtml** . Zaznacz pole wyboru **Utwórz silnie wpisaną-View** , a następnie wybierz **StoreIndexViewModel** jako **klasę modelu**. Upewnij się również, że wybrany aparat widoku to **Razor**. Kliknij pozycję **Add** (Dodaj).

    ![Okno dialogowe Dodawanie widoku](aspnet-mvc-4-fundamentals/_static/image24.png "Okno dialogowe Dodawanie widoku")

    *Okno dialogowe Dodawanie widoku*

    Plik szablonu widoku **\Views\Store\Index.cshtml** zostanie utworzony i otwarty. W oparciu o informacje podane do okna dialogowego **Dodawanie widoku** w ostatnim kroku, szablon widoku będzie oczekiwać wystąpienia **StoreIndexViewModel** jako danych do użycia w celu wygenerowania odpowiedzi html. Zobaczysz, że szablon dziedziczy `ViewPage<musicstore.viewmodels.storeindexviewmodel>` w C#elemencie.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Zadanie 5 — Aktualizowanie szablonu widoku

W tym zadaniu zostanie zaktualizowany szablon widoku utworzony w ostatnim zadaniu w celu pobrania liczby gatunków i ich nazw na stronie.

> [!NOTE]
> Użyjesz @ Syntax (często nazywanej &quot;kodem Nuggets&quot;), aby wykonać kod w szablonie widoku.

1. W pliku **index. cshtml** w folderze **Store** Zamień swój kod na następujący:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Pętla nad listą gatunek w **StoreIndexViewModel** i Utwórz kod HTML **&lt;ul&gt;** list przy użyciu pętli **foreach** .
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Naciśnij klawisz **F5** , aby uruchomić aplikację i przeglądać **/Store**. Zostanie wyświetlona lista gatunków przeniesiona w obiekcie **StoreIndexViewModel** z **StoreController** do szablonu widoku.

    ![Widok wyświetlania listy gatunków](aspnet-mvc-4-fundamentals/_static/image26.png "Widok wyświetlania listy gatunków")

    *Widok wyświetlania listy gatunków*
4. Zamknij okno przeglądarki.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Ćwiczenie 6: Używanie parametrów w widoku

W ćwiczeniu 3 przedstawiono sposób przekazywania parametrów do kontrolera. W tym ćwiczeniu dowiesz się, jak używać tych parametrów w szablonie widoku. W tym celu należy najpierw wprowadzić klasy modelu, które ułatwią zarządzanie danymi i logiką domeny. Ponadto dowiesz się, jak tworzyć linki do stron w aplikacji ASP.NET MVC, bez obaw, takich jak kodowanie ścieżek adresów URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Zadanie 1 — Dodawanie klas modelu

W przeciwieństwie do modele widoków, które są tworzone tylko w celu przekazywania informacji z kontrolera do widoku, klasy modelu są kompilowane tak, aby zawierały dane i logikę domen i zarządzać nimi. W tym zadaniu zostaną dodane dwie klasy modelu do reprezentowania tych koncepcji: **gatunek** i **album**.

1. Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**
2. W menu **plik** wybierz **Otwórz projekt**. W oknie dialogowym Otwórz projekt przejdź do **Source\Ex06-UsingParametersInView\Begin**, wybierz pozycję **Rozpocznij. sln** , a następnie kliknij pozycję **Otwórz**. Alternatywnie można nadal korzystać z rozwiązania uzyskanego po zakończeniu poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Dodaj klasę modelu **gatunek** . W tym celu kliknij prawym przyciskiem myszy folder **modele** w **Eksplorator rozwiązań**, wybierz polecenie **Dodaj** , a następnie opcję **nowy element** . W obszarze **kod**wybierz element **klasy** i Nazwij plik *Genre.cs*, a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie klasy](aspnet-mvc-4-fundamentals/_static/image27.png "Dodawanie klasy")

    *Dodawanie nowego elementu*

    ![Dodaj klasę modelu gatunku](aspnet-mvc-4-fundamentals/_static/image28.png "Dodaj klasę modelu gatunku")

    *Dodaj klasę modelu gatunku*
4. Dodaj właściwość **name** do klasy gatunek. Aby to zrobić, Dodaj następujący kod:

    (Fragment kodu- *ASP.NET MVC 4 podstawy-Ex6 gatunek*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Postępując zgodnie z tą samą procedurą jak wcześniej, Dodaj klasę **albumu** . W tym celu kliknij prawym przyciskiem myszy folder **modele** w **Eksplorator rozwiązań**, wybierz polecenie **Dodaj** , a następnie opcję **nowy element** . W obszarze **kod**wybierz element **klasy** i Nazwij plik *album.cs*, a następnie kliknij przycisk **Dodaj**.
6. Dodaj dwie właściwości do klasy albumu: **gatunek** i **tytuł**. Aby to zrobić, Dodaj następujący kod:

    (Fragment kodu — *ASP.NET MVC 4 — podstawy — Ex6 album*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Zadanie 2 — Dodawanie elementu StoreBrowseViewModel

W tym zadaniu zostanie użyta **StoreBrowseViewModel** , aby wyświetlić albumy pasujące do wybranego gatunku. W tym zadaniu utworzysz tę klasę, a następnie dodasz dwie właściwości, aby obsłużyć **gatunek** i jego listę **albumów**.

1. Dodaj klasę **StoreBrowseViewModel** . W tym celu kliknij prawym przyciskiem myszy folder **modele widoków** w **Eksplorator rozwiązań**, wybierz pozycję **Dodaj** , a następnie opcję **nowy element** . W obszarze **kod**wybierz element **klasy** i Nazwij plik *StoreBrowseViewModel.cs*, a następnie kliknij przycisk **Dodaj**.
2. Dodaj odwołanie do modeli w klasie **StoreBrowseViewModel** . Aby to zrobić, Dodaj następujące polecenie przy użyciu przestrzeni nazw:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Dodaj dwie właściwości do klasy **StoreBrowseViewModel** : **gatunek** i **albumy**. Aby to zrobić, Dodaj następujący kod:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> Co to **jest lista&lt;&gt;albumu** ?: w tej definicji jest **używana lista&lt;t&gt;** typ, gdzie **t** ogranicza typ, do którego należą elementy tej **listy** , w tym przypadku **album** (lub dowolne z jego obiektów podrzędnych).
> 
> Możliwość projektowania klas i metod, które opóźniają specyfikację jednego lub więcej typów do momentu zadeklarowania klasy lub metody i wystąpienia przez kod klienta jest funkcją C# języka o nazwie **Generics**.
> 
> **Lista&lt;t&gt;** jest ogólnym odpowiednikiem typu **ArrayList** i jest dostępna w przestrzeni nazw **System. Collections. Generic** . Jedną z korzyści wynikających z używania typów **ogólnych** jest to, że od momentu określenia typu, nie trzeba zadbać o wykonywanie operacji sprawdzania typu, takich jak Rzutowanie elementów do **albumu** , tak jak w przypadku **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Zadanie 3 — Używanie nowego ViewModel w StoreController

W tym zadaniu zmodyfikujemy metody akcji **przeglądania** i **szczegółów** **StoreController**, aby użyć nowej **StoreBrowseViewModel**.

1. Dodaj odwołanie do folderu models w klasie **StoreController** . W tym celu rozwiń folder **controllers** w **Eksplorator rozwiązań** i Otwórz klasę **StoreController** . Następnie Dodaj następujący kod:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Zastąp metodę **przeglądania** akcją, aby użyć klasy **StoreViewBrowseController** . Utworzysz gatunek i dwa nowe obiekty albumów z fikcyjnymi danymi (w następnym ćwiczeniu laboratorium będziesz używać rzeczywistych danych z bazy danych). Aby to zrobić, Zastąp metodę **Browse** następującym kodem:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Zastąp metodę akcji **Details** , aby użyć klasy **StoreViewBrowseController** . Utworzysz nowy obiekt **albumu** do zwrócenia do **widoku**. Aby to zrobić, Zastąp metodę **Details** następującym kodem:

    (Fragment kodu — *ASP.NET MVC 4 — podstawowe-Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Zadanie 4 — Dodawanie szablonu widoku przeglądania

W tym zadaniu zostanie dodany widok **przeglądania** w celu wyświetlenia albumów znalezionych dla określonego gatunku.

1. Przed utworzeniem nowego szablonu widoku należy skompilować projekt, aby wyświetlić okno dialogowe **Dodawanie widoku** o klasie **ViewModel** do użycia. Skompiluj projekt, wybierając element menu **kompilacja** , a następnie **Skompiluj MvcMusicStore**.
2. Dodaj widok **przeglądania** . Aby to zrobić, kliknij prawym przyciskiem myszy metodę **przeglądania** akcji **StoreController** i kliknij polecenie **Dodaj widok**.
3. W oknie dialogowym **Dodawanie widoku** Sprawdź, czy nazwa widoku jest **przeszukiwana**. Zaznacz pole wyboru **Utwórz widok o jednoznacznie określonym typie** i wybierz pozycję **StoreBrowseViewModel** z listy rozwijanej **klasy modeli** . Pozostaw pozostałe pola wartością domyślną. Następnie kliknij pozycję **Dodaj**.

    ![Dodawanie widoku przeglądania](aspnet-mvc-4-fundamentals/_static/image29.png "Dodawanie widoku przeglądania")

    *Dodawanie widoku przeglądania*
4. Zmodyfikuj polecenie **Browse. cshtml** , aby wyświetlić informacje o gatunku, uzyskując dostęp do obiektu **StoreBrowseViewModel** , który jest przesyłany do szablonu widoku. Aby to zrobić, Zastąp zawartość następującym: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Zadanie 5 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowane, że metoda **Browse** pobiera albumy z akcji **Przeglądaj** .

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Czy zmienić adres URL na **/Store/Browse? Gatunek = Disco** , aby sprawdzić, czy akcja zwraca dwa albumy.

    ![Przeglądanie albumów Disco w sklepie](aspnet-mvc-4-fundamentals/_static/image30.png "Przeglądanie albumów Disco w sklepie")

    *Przeglądanie albumów Disco w sklepie*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Zadanie 6 — Wyświetlanie informacji o określonym albumie

W tym zadaniu zostanie wdrożony widok **Magazyn/szczegóły** w celu wyświetlenia informacji o określonym albumie. W tym ćwiczeniu w tym ćwiczeniu wszystko, co będzie wyświetlane na albumie, jest już zawarte w szablonie **widoku** . Dlatego zamiast tworzenia klasy **StoreDetailsViewModel** będzie używany bieżący szablon **StoreBrowseViewModel** , który przekazuje do niego album.

1. W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio. Dodaj nowy widok **szczegółów** dla metody akcji **Details** **StoreController**. W tym celu kliknij prawym przyciskiem myszy metodę **Details** w klasie **StoreController** , a następnie kliknij polecenie **Dodaj widok**.
2. W oknie dialogowym **Dodawanie widoku** Sprawdź, czy **Nazwa widoku** to **szczegóły**. Zaznacz pole wyboru **Utwórz widok o jednoznacznie określonym typie** i wybierz opcję **album** z listy rozwijanej **Klasa modelu** . Pozostaw pozostałe pola wartością domyślną. Następnie kliknij pozycję **Dodaj**. Spowoduje to utworzenie i otwarcie pliku **\Views\Store\Details.cshtml** .

    ![Dodawanie widoku szczegółów](aspnet-mvc-4-fundamentals/_static/image31.png "Dodawanie widoku szczegółów")

    *Dodawanie widoku szczegółów*
3. Zmodyfikuj plik **details. cshtml** , aby wyświetlić informacje o albumie, uzyskując dostęp do obiektu **albumu** , który jest przesyłany do szablonu widoku. Aby to zrobić, Zastąp zawartość następującym: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Zadanie 7 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowany, że widok **szczegóły** pobiera informacje o albumie z metody **akcji szczegóły** .

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie **głównej** . Zmień adres URL na **/Store/Details/5** , aby zweryfikować informacje o albumie.

    ![Szczegóły przeglądania albumów](aspnet-mvc-4-fundamentals/_static/image32.png "Szczegóły przeglądania albumów")

    *Przeglądanie szczegółów albumu*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Zadanie 8 — Dodawanie linków między stronami

W tym zadaniu dodasz w widoku sklepu link do odpowiedniego adresu URL **/Store/Browse** w polu Nazwa każdego gatunku. W ten sposób po kliknięciu gatunku dla wystąpienia **Disco**zostanie przeszukany **/Store/Browse? gatunek = Disco** URL.

1. W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio. Zaktualizuj stronę **indeksu** , aby dodać łącze do strony **przeglądania** . W tym celu w **Eksplorator rozwiązań** rozwiń folder **widoki** , a następnie folder **Store** i kliknij dwukrotnie stronę **index. cshtml** .
2. Dodaj link do widoku przeglądania wskazującego wybrany gatunek. Aby to zrobić, Zastąp następujący wyróżniony kod w **&lt;li&gt;** tagów: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > innym podejściem będzie łączenie bezpośrednio ze stroną, z kodem podobnym do poniższego:
   > 
   > &lt;a href =&quot;/Store/Browse? gatunek =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Chociaż to podejście działa, zależy to od ciągu stałe. W przypadku późniejszej zmiany nazwy kontrolera należy ręcznie zmienić tę instrukcję. Lepszym rozwiązaniem jest użycie metody **pomocnika HTML** . ASP.NET MVC zawiera metodę pomocnika HTML, która jest dostępna dla takich zadań. Metoda pomocnika **HTML. ActionLink ()** ułatwia tworzenie kodu HTML&lt;linków **&gt;** , co sprawia, że ścieżki URL są poprawnie zakodowane w adresie URL.
   > 
   > Plik HTML. ActionLink ma kilka przeciążeń. W tym ćwiczeniu będziesz używać jednego z trzech parametrów:
   > 
   > 1. Tekst łącza, który będzie wyświetlał nazwę gatunku
   > 2. Nazwa akcji kontrolera (**Przeglądaj**)
   > 3. Wartości parametrów trasy, określające zarówno nazwę (**gatunek**), jak i wartość (**nazwę gatunku**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Zadanie 9 — Uruchamianie aplikacji

W tym zadaniu sprawdzisz, że każdy gatunek jest wyświetlany z linkiem do odpowiedniego adresu URL **/Store/Browse** .

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie głównej. Zmień adres URL na **/Store** , aby sprawdzić, czy każdy gatunek łączy się z odpowiednim adresem URL **/Store/Browse** .

    ![Przeglądanie gatunków z linkami do strony przeglądania](aspnet-mvc-4-fundamentals/_static/image33.png "Przeglądanie gatunków z linkami do strony przeglądania")

    *Przeglądanie gatunków z linkami do strony przeglądania*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Zadanie 10 — Używanie dynamicznej kolekcji ViewModel do przekazywania wartości

W tym zadaniu przedstawiono prostą i zaawansowaną metodę przekazywania wartości między kontrolerem a widokiem bez wprowadzania żadnych zmian w modelu. ASP.NET MVC 4 udostępnia kolekcje &quot;ViewModel&quot;, które można przypisać do dowolnej wartości dynamicznej i uzyskać do nich dostęp tylko w kontrolerach i widokach.

Teraz użyjesz dynamicznej kolekcji ViewBag, aby przekazać listę &quot;ych&quot; **gatunku** z kontrolera do widoku. Widok indeksu magazynu będzie miał dostęp do **ViewModel** i wyświetlania informacji.

1. W razie potrzeby Zamknij przeglądarkę, aby powrócić do okna programu Visual Studio. Otwórz **StoreController.cs** i zmodyfikuj metodę **index** , aby utworzyć listę gatunków oznaczona gwiazdkami w kolekcji ViewModel:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > Aby uzyskać dostęp do właściwości, można również użyć składni **ViewBag [&quot;oznaczona gwiazdkami&quot;]** .
2. Ikona gwiazdki **&quot;oznaczona gwiazdkami. png&quot;** jest zawarta w folderze **Source\Assets\Images** w tym laboratorium. Aby dodać ją do aplikacji, przeciągnij jej zawartość z okna **Eksploratora Windows** do **Eksplorator rozwiązań** w programie Visual Web Developer Express, jak pokazano poniżej:

    ![Dodawanie obrazu gwiazdy do rozwiązania](aspnet-mvc-4-fundamentals/_static/image34.png "Dodawanie obrazu gwiazdy do rozwiązania")

    *Dodawanie obrazu gwiazdy do rozwiązania*
3. Otwórz widok **Store/index. cshtml** i zmodyfikuj zawartość. Właściwość &quot;oznaczona gwiazdkami&quot; zostanie odczytana w kolekcji **ViewBag** i zażądać, jeśli nazwa bieżącego gatunku znajduje się na liście. W takim przypadku zostanie wyświetlona ikona gwiazdki z prawej strony linku do gatunku.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Zadanie 11 — uruchamianie aplikacji

W tym zadaniu sprawdzisz, że w gatunku oznaczona gwiazdkami zostanie wyświetlona ikona gwiazdki.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację.
2. Projekt zostanie uruchomiony na stronie **głównej** . Zmień adres URL na **/Store** , aby upewnić się, że każdy proponowany gatunek ma etykietę respektują:

    ![Przeglądanie gatunków przy użyciu elementów oznaczona gwiazdkami](aspnet-mvc-4-fundamentals/_static/image35.png "Przeglądanie gatunków przy użyciu elementów oznaczona gwiazdkami")

    *Przeglądanie gatunków przy użyciu elementów oznaczona gwiazdkami*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Ćwiczenie 7: kolano wokół ASP.NET MVC 4 nowy szablon

W tym ćwiczeniu zapoznajesz ulepszenia w szablonach projektów ASP.NET MVC 4, biorąc pod uwagę najbardziej odpowiednie funkcje nowego szablonu.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Zadanie 1. Eksplorowanie szablonu aplikacji internetowej ASP.NET MVC 4

1. Jeśli nie jest jeszcze otwarty, uruchom **vs Express for Web**
2. Wybierz **plik | Nowy |** Polecenie menu projektu. W oknie dialogowym **Nowy projekt** wybierz **wizualizację C#| Szablon sieci Web** w okienku po lewej stronie, a następnie wybierz **aplikację sieci Web ASP.NET MVC 4**. **Nazwij** projekt *MusicStore* i zaktualizuj **nazwę rozwiązania** , aby *rozpocząć*, a następnie wybierz lokalizację (lub pozostaw wartość domyślną), a następnie kliknij przycisk **OK**.

    ![Tworzenie nowego projektu ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Tworzenie nowego projektu ASP.NET MVC 4")

    *Tworzenie nowego projektu ASP.NET MVC 4*
3. W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon projektu **aplikacji internetowej** i kliknij przycisk **OK**. Zwróć uwagę, że można wybrać opcję Razor lub ASPX jako aparat widoku.

    ![Tworzenie nowej aplikacji internetowej ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Tworzenie nowej aplikacji internetowej ASP.NET MVC 4")

    *Tworzenie nowej aplikacji internetowej ASP.NET MVC 4*

    > [!NOTE]
    > Składnia Razor wprowadzono w ASP.NET MVC 3. Celem jest Minimalizacja liczby znaków i naciśnięć klawiszy wymaganych w pliku, co pozwala na szybkie i płynne przepływność kodowania. Razor wykorzystuje istniejące C#umiejętności języka/vb (lub inne) i dostarcza składnię znaczników szablonów, która umożliwia tworzenie doskonałych przepływów pracy konstrukcyjnych html.
4. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie i zapoznać się z odnowionym szablonem. Można wyewidencjonować następujące funkcje:

    1. **Szablony w stylu Modern**

        Szablony zostały odnowione, co zapewnia bardziej nowoczesny wygląd stylów.

        ![Szablony z ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image38.png "Szablony z ASP.NET MVC 4")

        *Szablony z ASP.NET MVC 4*
    2. **Renderowanie adaptacyjne**

        Sprawdź zmianę rozmiaru okna przeglądarki i zwróć uwagę na to, jak układ strony dynamicznie dostosowuje się do nowego rozmiaru okna. Te szablony wykorzystują technikę renderowania adaptacyjnego do prawidłowego renderowania na platformach stacjonarnych i mobilnych bez żadnego dostosowania.

        ![Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki](aspnet-mvc-4-fundamentals/_static/image39.png "Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki")

        *Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki*
5. Zamknij przeglądarkę, aby zatrzymać debuger i wrócić do programu Visual Studio.
6. Teraz możesz eksplorować rozwiązanie i zapoznać się z kilkoma nowymi funkcjami wprowadzonymi przez ASP.NET MVC 4 w szablonie projektu.

    ![ASP.NET MVC4 — Internet-aplikacja-projekt-szablon](aspnet-mvc-4-fundamentals/_static/image40.png "Szablon projektu aplikacji internetowej ASP.NET MVC 4")

    *Szablon projektu aplikacji internetowej ASP.NET MVC 4*

   1. **Znaczniki HTML5**

       Przeglądaj widoki szablonów, aby dowiedzieć się więcej o nowym znaczniku motywu, np. Otwórz widok **about. cshtml** w folderze **głównym** .

       ![Nowy szablon, używanie języka Razor i znaczników HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nowy szablon, używanie języka Razor i znaczników HTML5")

       *Nowy szablon, używanie języka Razor i znaczników HTML5*
   2. **Uwzględnione biblioteki JavaScript**

      1. **jQuery**: jQuery upraszcza przechodzenie do dokumentów HTML, obsługę zdarzeń, animowanie i interakcje AJAX.
      2. **interfejs użytkownika jQuery**: Ta biblioteka zawiera abstrakcje dla interakcji i animacji niskiego poziomu, zaawansowanych efektów i elementów widget z motywem, utworzonych na podstawie biblioteki jQuery JavaScript.

         > [!NOTE]
         > Informacje na temat interfejsu użytkownika jQuery i jQuery można znaleźć w [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
      3. **KnockoutJS**: szablon domyślny ASP.NET MVC 4 zawiera teraz **KNOCKOUTJS**, środowisko MVVM języka JavaScript, które umożliwia tworzenie rozbudowanych i wysoce reagujących aplikacji sieci Web przy użyciu języków JavaScript i HTML. Podobnie jak w przypadku bibliotek interfejsu użytkownika ASP.NET MVC 3, jQuery i jQuery są również dołączone do ASP.NET MVC 4.

          > [!NOTE]
          > Więcej informacji na temat biblioteki KnockOutJS można znaleźć w tym łączu: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
      4. **Modernizacja**: Ta biblioteka działa automatycznie, dzięki czemu witryna jest zgodna ze starszymi przeglądarkami w przypadku korzystania z technologii HTML5 i CSS3.

          > [!NOTE]
          > W tym łączu można uzyskać więcej informacji na temat biblioteki programu modernizacji: [http://www.modernizr.com/](http://www.modernizr.com/).
   3. **SimpleMembership dołączone do rozwiązania**

       SimpleMembership został zaprojektowany jako zamiennik dla poprzedniej roli ASP.NET i systemu dostawcy członkostwa. Ma wiele nowych funkcji, które ułatwiają deweloperom Zabezpieczanie stron sieci Web w bardziej elastyczny sposób.

       Szablon internetowy już skonfigurował kilka rzeczy do integracji SimpleMembership, na przykład elementu AccountController jest przygotowana do korzystania z OAuthWebSecurity (w przypadku rejestracji, logowania, zarządzania itp.) i zabezpieczeń sieci Web.

       ![SimpleMembership dołączone do rozwiązania](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership dołączone do rozwiązania")

       *SimpleMembership dołączone do rozwiązania*

       > [!NOTE]
       > Więcej informacji na temat [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) w witrynie MSDN.

> [!NOTE]
> Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure, które zostały opisane w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wypełniając to praktyczne laboratorium, uczysz się podstaw ASP.NET MVC:

- Podstawowe elementy aplikacji MVC oraz sposób ich działania
- Jak utworzyć aplikację ASP.NET MVC
- Jak dodać i skonfigurować kontrolery do obsługi parametrów przesyłanych za pomocą adresu URL i ciągu QueryString
- Jak dodać stronę wzorcową układu, aby skonfigurować szablon dla typowej zawartości HTML, arkusz stylów, aby wzbogacić wygląd i działanie oraz szablon widoku w celu wyświetlenia zawartości HTML
- Jak użyć wzorca ViewModel do przekazywania właściwości do szablonu widoku, aby wyświetlić informacje dynamiczne
- Jak używać parametrów przesyłanych do kontrolerów w szablonie widoku
- Jak dodać linki do stron wewnątrz aplikacji ASP.NET MVC
- Jak dodać właściwości dynamiczne i używać ich w widoku
- Ulepszenia w szablonach projektów ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    *Kafelek VS Express for Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure

1. Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu. Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do systemu Windows Azure Portal](aspnet-mvc-4-fundamentals/_static/image48.png "Zaloguj się do systemu Windows Azure Portal")

    *Zaloguj się do usługi Windows Azure portal zarządzania*
2. Na pasku poleceń kliknij pozycję **Nowy** .

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image49.png "Tworzenie nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**. Następnie wybierz opcję **szybkie tworzenie** . Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią. Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu. Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](aspnet-mvc-4-fundamentals/_static/image50.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")

    *Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*
4. Poczekaj na utworzenie nowej **witryny sieci Web** .
5. Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** . Sprawdź, czy nowa witryna sieci Web działa.

    ![Przeglądanie w nowej witrynie sieci Web](aspnet-mvc-4-fundamentals/_static/image51.png "Przeglądanie w nowej witrynie sieci Web")

    *Przeglądanie w nowej witrynie sieci Web*

    ![Uruchomiona witryna sieci Web](aspnet-mvc-4-fundamentals/_static/image52.png "Uruchomiona witryna sieci Web")

    *Uruchomiona witryna sieci Web*
6. Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania witryną sieci Web](aspnet-mvc-4-fundamentals/_static/image53.png "Otwieranie stron zarządzania witryną sieci Web")

    *Otwieranie stron zarządzania witryną sieci Web*
7. Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji. **Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.

    ![Pobieranie profilu publikowania witryny sieci Web](aspnet-mvc-4-fundamentals/_static/image54.png "Pobieranie profilu publikowania witryny sieci Web")

    *Pobieranie profilu publikowania witryny sieci Web*
8. Pobierz plik profilu publikowania do znanej lokalizacji. W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-fundamentals/_static/image55.png "Zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer. Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.

1. Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database. Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń. Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach. Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.

    ![Pulpit nawigacyjny serwera SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "Pulpit nawigacyjny serwera SQL Database")

    *Pulpit nawigacyjny serwera SQL Database*
2. W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera. W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](aspnet-mvc-4-fundamentals/_static/image57.png).

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Dodawanie adresu IP klienta*
3. Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.

    ![Potwierdź zmiany](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.

    ![Publikowanie aplikacji](aspnet-mvc-4-fundamentals/_static/image60.png "Publikowanie aplikacji")

    *Publikowanie witryny sieci Web*
2. Zaimportuj profil publikowania zapisany w pierwszym zadaniu.

    ![Importowanie profilu publikowania](aspnet-mvc-4-fundamentals/_static/image61.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij pozycję **Weryfikuj połączenie**. Po zakończeniu walidacji kliknij przycisk **dalej**.

    > [!NOTE]
    > Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.

    ![Weryfikowanie połączenia](aspnet-mvc-4-fundamentals/_static/image62.png "Weryfikowanie połączenia")

    *Weryfikowanie połączenia*
4. Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).

    ![Konfiguracja narzędzia Web Deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Konfiguracja narzędzia Web Deploy")

    *Konfiguracja narzędzia Web Deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .
   - W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.
   - W polu **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nową nazwę bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia docelowego](aspnet-mvc-4-fundamentals/_static/image64.png "Konfigurowanie parametrów połączenia docelowego")

     *Konfigurowanie parametrów połączenia docelowego*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-fundamentals/_static/image65.png "Tworzenie ciągu bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie. Następnie kliknij przycisk **Next** (Dalej).

    ![Parametry połączenia wskazujące SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Parametry połączenia wskazujące SQL Database")

    *Parametry połączenia wskazujące SQL Database*
8. Na stronie **Podgląd** kliknij przycisk **Publikuj**.

    ![Publikowanie aplikacji sieci Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publikowanie aplikacji sieci Web")

    *Publikowanie aplikacji sieci Web*
9. Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.

    ![Aplikacja opublikowana w systemie Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Aplikacja opublikowana w systemie Windows Azure")

    *Aplikacja opublikowana w systemie Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-fundamentals/_static/image69.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-fundamentals/_static/image70.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-fundamentals/_static/image71.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-fundamentals/_static/image72.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
2. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-fundamentals/_static/image73.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-fundamentals/_static/image74.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

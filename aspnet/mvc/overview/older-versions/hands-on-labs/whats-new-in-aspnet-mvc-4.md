---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Co nowego w ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 to platforma służąca do tworzenia skalowalnych, opartych na standardach aplikacji sieci Web przy użyciu dobrze ustanowionych wzorców projektowych i możliwości ASP.NET i...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 4235f4fe666cdeb7d0821127a2b349f2ff30cd6e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539439"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Co nowego we wzorcu ASP.NET MVC 4

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 to platforma służąca do tworzenia skalowalnych, opartych na standardach aplikacji sieci Web przy użyciu dobrze ustanowionych wzorców projektowych i możliwości ASP.NET i .NET Framework. Ta nowa, czwarta wersja platformy koncentruje się na ułatwianiu tworzenia aplikacji sieci Web dla urządzeń przenośnych.

Aby rozpocząć pracę z programem, podczas tworzenia nowego projektu ASP.NET MVC 4 istnieje teraz szablon projektu aplikacji mobilnej, którego można użyć do utworzenia autonomicznej aplikacji przeznaczonej dla urządzeń przenośnych. Ponadto ASP.NET MVC 4 integruje się z usługą jQuery Mobile za pomocą pakietu NuGet platformy jQuery. Mobile. MVC. jQuery Mobile to platforma oparta na języku HTML5 służąca do opracowywania aplikacji sieci Web, które są zgodne ze wszystkimi popularnymi platformami urządzeń przenośnych, w tym Windows Phone, iPhone, Android i tak dalej. Jeśli jednak potrzebujesz specjalizacji, ASP.NET MVC 4 umożliwia również obsługę różnych widoków dla różnych urządzeń i zapewnia optymalizacje specyficzne dla urządzenia.

W tym ćwiczeniu można rozpocząć pracę z szablonem projektu ASP.NET MVC 4 &quot;Internet aplikacji internetowej&quot;, aby utworzyć aplikację galerii zdjęć. Stopniowo ulepszasz aplikację przy użyciu nowych funkcji jQuery Mobile i ASP.NET MVC 4, aby zapewnić ich zgodność z różnymi urządzeniami przenośnymi i przeglądarkami sieci Web dla komputerów stacjonarnych. Zapoznaj się również z nowymi przepisami kodu dotyczącymi generowania kodu i, w jaki sposób ASP.NET MVC 4 ułatwia pisanie metod akcji asynchronicznych przez obsługę zadań&lt;ActionResult&gt; typów zwracanych.

> [!NOTE]
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [wersjach Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt charakterystyczny dla tego laboratorium jest dostępny dla [nowości w formularzach sieci Web w programie ASP.NET 4,5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Skorzystaj z ulepszeń szablonów projektów ASP.NET MVC — w tym szablonu projektu nowej aplikacji mobilnej
- Użycie atrybutu okienka ekranu HTML5 i zapytań o multimedia CSS w celu usprawnienia wyświetlania na urządzeniach przenośnych
- Korzystanie z platformy jQuery Mobile do stopniowego udoskonalania i tworzenia interfejsu użytkownika sieci Web zoptymalizowanego pod kątem dotyku
- Tworzenie widoków specyficznych dla urządzeń przenośnych
- Użycie składnika View-przełącznik do przełączania między widokami mobilnymi i klasycznymi w aplikacji
- Tworzenie kontrolerów asynchronicznych za pomocą obsługi zadań

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek B](#AppendixB) , aby uzyskać instrukcje dotyczące sposobu ich instalacji).
- [ASP.NET MVC 4](../../../mvc4.md) (zawarta w instalacji Microsoft Visual Studio 2012)
- Emulator Windows Phone (zawarty w [zestawie SDK Windows Phone 7.1.1](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcjonalne — [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) z rozszerzeniem **elektrycznym symulatora iPhone** (tylko dla ćwiczenia 3 używanym do przeglądania aplikacji sieci Web za pomocą symulatora iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu. Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, których można użyć w programie Visual Studio, aby uniknąć konieczności ręcznego dodawania go.

Aby zainstalować fragmenty kodu:

1. Otwórz okno Eksploratora Windows i przejdź do folderu **Source\Setup** laboratorium.
2. Kliknij dwukrotnie plik **Setup. cmd** w tym folderze, aby zainstalować fragmenty kodu programu Visual Studio.

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatek A: używanie fragmentów kodu](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Nowe szablony projektów ASP.NET MVC 4](#Exercise1)
2. [Tworzenie aplikacji sieci Web Photo Gallery](#Exercise2)
3. [Dodawanie obsługi urządzeń przenośnych](#Exercise3)
4. [Korzystanie z kontrolerów asynchronicznych](#Exercise4)

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

Szacowany czas wykonywania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Ćwiczenie 1: nowe szablony projektów ASP.NET MVC 4

W tym ćwiczeniu zapoznajesz ulepszenia w szablonach projektów ASP.NET MVC 4. Oprócz szablonu aplikacji internetowej, który jest już obecny w MVC 3, ta wersja zawiera teraz osobny szablon dla aplikacji mobilnych. Najpierw należy zapoznać się z odpowiednimi funkcjami każdego z szablonów. Następnie będziesz korzystać z prawidłowego renderowania strony na różnych platformach przy użyciu odpowiedniego podejścia.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Zadanie 1 — Eksplorowanie szablonu aplikacji internetowej

1. Otwórz **program Visual Studio**.
2. Wybierz **plik | Nowy |** Polecenie menu projektu. W oknie dialogowym **Nowy projekt** wybierz **wizualizację C# | Szablon sieci Web** w okienku po lewej stronie, a następnie wybierz **ASP.NET aplikacji sieci Web MVC 4.** Nazwij projekt **Galeria**, wybierz lokalizację (lub pozostaw wartość domyślną), a następnie kliknij przycisk **OK**.

    > [!NOTE]
    > Później zostanie dostosowane rozwiązanie ASP.NET MVC 4, które tworzysz.

    ![Tworzenie nowego projektu](whats-new-in-aspnet-mvc-4/_static/image1.png "Tworzenie nowego projektu")

    *Tworzenie nowego projektu*
3. W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon projektu **aplikacji internetowej** i kliknij przycisk **OK**. Upewnij się, że wybrano Razor jako aparat widoku.

    ![Tworzenie nowej aplikacji internetowej ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "Tworzenie nowej aplikacji internetowej ASP.NET MVC 4")

    *Tworzenie nowej aplikacji internetowej ASP.NET MVC 4*

    > [!NOTE]
    > Składnia Razor wprowadzono w ASP.NET MVC 3. Celem jest Minimalizacja liczby znaków i naciśnięć klawiszy wymaganych w pliku, co pozwala na szybkie i płynne przepływność kodowania. Razor wykorzystuje istniejące C# umiejętności językowe w języku vb (lub inne) i dostarcza składnię znaczników szablonów, która umożliwia tworzenie doskonałych przepływów pracy konstrukcyjnych html.
4. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie i zobaczyć odnowione szablony. Można wyewidencjonować następujące funkcje:

    **Szablony w stylu Modern**

    Szablony zostały odnowione, co zapewnia bardziej nowoczesny wygląd stylów.

    ![Szablony z ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image3.png "Szablony ze stylem MVC 4")

    *Szablony z ASP.NET MVC 4*

    ![Nowa strona kontaktu](whats-new-in-aspnet-mvc-4/_static/image4.png "Nowa strona kontaktu")

    *Nowa strona kontaktu*

    **Renderowanie adaptacyjne**

    Sprawdź zmianę rozmiaru okna przeglądarki i zwróć uwagę na to, jak układ strony dynamicznie dostosowuje się do nowego rozmiaru okna. Te szablony wykorzystują technikę renderowania adaptacyjnego do prawidłowego renderowania na platformach stacjonarnych i mobilnych bez żadnego dostosowania.

    ![Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki](whats-new-in-aspnet-mvc-4/_static/image5.png "Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki")

    *Szablon projektu ASP.NET MVC 4 w różnych rozmiarach przeglądarki*

    **Bogatszy interfejs użytkownika z językiem JavaScript**

    Innym ulepszeniem szablonów projektów domyślnych jest użycie języka JavaScript w celu zapewnienia bardziej interaktywnego języka JavaScript. Linki logowania i rejestrowania używane w szablonie exemplify, jak używać walidacji jQuery do walidacji pól wejściowych ze strony klienta.

    ![Walidacja jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Walidacja jQuery*

    > [!NOTE]
    > Zwróć uwagę na dwie sekcje logowania w pierwszej sekcji, w której możesz zalogować się przy użyciu zarejestrowanego konta z lokacji i w drugiej sekcji możesz alternatywnie zalogować się przy użyciu innej usługi uwierzytelniania, takiej jak Google (domyślnie wyłączona).
5. Zamknij przeglądarkę, aby zatrzymać debuger i wrócić do programu Visual Studio.
6. Otwórz plik **authconfig.cs** znajdujący się w folderze **Start\_aplikacji** .
7. Usuń komentarz z ostatniego wiersza, aby zarejestrować klienta Google Client na potrzeby uwierzytelniania *OAuth* .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Należy zauważyć, że można łatwo włączyć uwierzytelnianie przy użyciu dowolnej usługi OpenID Connect lub OAuth, takiej jak Facebook, Twitter, Microsoft itp.
8. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie, a następnie przejdź do strony logowania.
9. Wybierz pozycję Usługa **Google** , aby się zalogować.

    ![Wybieranie usługi logowania](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Wybieranie usługi logowania*
10. Zaloguj się przy użyciu konta Google.
11. Zezwalaj witrynie (localhost) na pobieranie informacji z konta Google.
12. Na koniec musisz zarejestrować się w witrynie, aby skojarzyć konto Google.

   ![Kojarzenie konta Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Kojarzenie konta Google*
13. Zamknij przeglądarkę, aby zatrzymać debuger i wrócić do programu Visual Studio.
14. Poznaj teraz rozwiązanie, aby sprawdzić inne nowe funkcje wprowadzone przez ASP.NET MVC 4 w szablonie projektu.

   ![Szablon projektu aplikacji internetowej ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "Szablon projektu aplikacji internetowej ASP.NET MVC 4")

   *Szablon projektu aplikacji internetowej ASP.NET MVC 4*

    - **Znaczniki HTML 5**

       Przeglądaj widoki szablonów, aby dowiedzieć się więcej o nowym znaczniku motywu.

       ![Nowy szablon, przy użyciu składni Razor i HTML5 Markup for. cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "Nowy szablon, przy użyciu składni Razor i HTML5 Markup for. cshtml.")

       *Nowy szablon, przy użyciu składni Razor i HTML5 Markup Language (about. cshtml).*
    - **Zaktualizowane biblioteki JavaScript**

       Szablon domyślny ASP.NET MVC 4 zawiera teraz KnockoutJS, środowisko JavaScript MVVM, które umożliwia tworzenie rozbudowanych i wysoce reagujących aplikacji sieci Web przy użyciu języków JavaScript i HTML. Podobnie jak w przypadku bibliotek interfejsu użytkownika MVC3, jQuery i jQuery są również dołączone do ASP.NET MVC 4.

     > [!NOTE]
     > Więcej informacji na temat biblioteki KnockOutJS można znaleźć w tym łączu: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). Ponadto można dowiedzieć się więcej na temat interfejsu użytkownika jQuery i jQuery w [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Zadanie 2 — Eksplorowanie szablonu aplikacji mobilnej

ASP.NET MVC 4 ułatwia opracowywanie witryn sieci Web na potrzeby przeglądarek mobilnych i tabletów. Ten szablon ma taką samą strukturę aplikacji jak szablon aplikacji internetowej (należy zauważyć, że kod kontrolera jest praktycznie identyczny), ale jego styl został zmodyfikowany w celu poprawnego renderowania w urządzeniach przenośnych opartych na dotyku.

1. Wybierz **plik | Nowy |** Polecenie menu projektu. W oknie dialogowym **Nowy projekt** wybierz **wizualizację C# | Szablon sieci Web** w okienku po lewej stronie, a następnie wybierz **aplikację sieci Web ASP.NET MVC 4.** Nazwij projekt **Galeria. Mobile**, wybierz lokalizację (lub pozostaw wartość domyślną), wybierz pozycję &quot;Dodaj do rozwiązania&quot; a następnie kliknij przycisk **OK**.
2. W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon projektu **aplikacji mobilnej** , a następnie kliknij przycisk **OK**. Upewnij się, że wybrano Razor jako aparat widoku.

    ![Tworzenie nowej aplikacji mobilnej ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image11.png "Tworzenie nowej aplikacji mobilnej ASP.NET MVC 4")

    *Tworzenie nowej aplikacji mobilnej ASP.NET MVC 4*
3. Teraz możesz eksplorować rozwiązanie i zapoznać się z kilkoma nowymi funkcjami wprowadzonymi przez szablon rozwiązania ASP.NET MVC 4 dla urządzeń przenośnych:

    - **Biblioteka jQuery Mobile**

        Szablon projektu aplikacji mobilnej zawiera bibliotekę platformy jQuery Mobile, która jest biblioteką Open Source w celu zachowania zgodności z przeglądarką mobilną. jQuery Mobile stosuje udoskonalenia progresywne dla przeglądarek mobilnych, które obsługują CSS i JavaScript. Rozszerzenie progresywne umożliwia wszystkim przeglądarkom wyświetlanie podstawowej zawartości strony sieci Web, a jednocześnie umożliwia tylko najbardziej wydajnym przeglądarkom wyświetlanie zawartości bogatej. Pliki JavaScript i CSS, zawarte w stylu jQuery Mobile, ułatwiają przeglądarkom mobilnym dopasowanie zawartości ekranu bez wprowadzania jakichkolwiek zmian w znacznikach strony.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *Biblioteka jQuery Mobile dołączona do szablonu*
    - **Znaczniki języka HTML5**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Szablon aplikacji mobilnej z użyciem znaczników HTML5 (login. cshtml i index. cshtml)*
4. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.
5. Otwórz **Emulator Windows Phone 7**.
6. Na ekranie startowym telefonu Otwórz program Internet Explorer. Sprawdź adres URL, pod którym uruchomiono aplikację klasyczną, i przejdź do tego adresu URL za pomocą telefonu (np. `http://localhost:[PortNumber]/`).
7. Teraz możesz wprowadzić stronę logowania lub zapoznaj się ze stroną informacje. Należy zauważyć, że styl witryny sieci Web jest oparty na nowej aplikacji Metro dla urządzeń przenośnych. Szablon projektu ASP.NET MVC 4 jest poprawnie wyświetlany na urządzeniach przenośnych, upewniając się, że wszystkie elementy strony są widoczne i włączone. Zauważ, że linki w nagłówku są wystarczająco duże, aby można je było kliknąć lub włączyć.

    ![Strony szablonu projektu na urządzeniu przenośnym](whats-new-in-aspnet-mvc-4/_static/image14.png "Strony szablonu projektu na urządzeniu przenośnym")

    *Strony szablonu projektu na urządzeniu przenośnym*
8. Nowy szablon używa również **znacznika okienka ekranu**. Większość przeglądarek dla urządzeń przenośnych definiuje szerokość okna przeglądarki wirtualnej lub &quot;&quot;okienka ekranu, które jest większe niż rzeczywista szerokość urządzenia przenośnego. Dzięki temu przeglądarki mobilne mogą wyświetlać całą stronę sieci Web wewnątrz ekranu wirtualnego. **Tag meta okienka ekranu** umożliwia deweloperom sieci Web Ustawianie szerokości, wysokości i skali obszaru przeglądarki na urządzeniach przenośnych **.** Szablon ASP.NET MVC 4 dla aplikacji mobilnych ustawia szerokość ekranu (&quot;Width =&quot;szerokości urządzenia) w szablonie układu (*Views\Shared\_Layout. cshtml*), dzięki czemu wszystkie strony mają ustawiony rozmiar okienka ekranu urządzenia. Zauważ, że tag meta okienka ekranu nie zmieni domyślnego widoku przeglądarki.
9. Otwórz plik **\_Layout. cshtml**, który znajduje się w **widokach | Folder udostępniony** i komentarz do znacznika okienka ekranu. Uruchom aplikację, jeśli nie została jeszcze otwarta, i sprawdź różnice.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Witryna po komentarzu do znacznika okienka ekranu](whats-new-in-aspnet-mvc-4/_static/image15.png "Witryna po komentarzu do znacznika okienka ekranu")

*Witryna po komentarzu do znacznika okienka ekranu*
10. W programie Visual Studio naciśnij klawisz **SHIFT** + **F5** , aby zatrzymać debugowanie aplikacji.
11. Usuń komentarz znacznika z okienka ekranu.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Zadanie 3 — Używanie funkcji renderowania adaptacyjnego

W tym zadaniu przedstawiono kolejną metodę renderowania strony sieci Web poprawnie na urządzeniach przenośnych i w przeglądarkach sieci Web bez konieczności dostosowywania. Tag meta okienka ekranu został już użyty w podobnym przeznaczeniu. Teraz będzie można uzyskać kolejną zaawansowaną metodę: *renderowanie adaptacyjne*.

Renderowanie adaptacyjne to technika, która używa **zapytań mediów CSS3** do dostosowywania stylu zastosowanego do strony. Zapytania o multimedia definiują warunki w arkuszu stylów, grupując Style CSS w określonym stanie. Tylko wtedy, gdy warunek ma wartość true, styl jest stosowany do zadeklarowanych obiektów.

Elastyczność zapewniana przez technikę renderowania adaptacyjnego umożliwia dostosowanie do wyświetlania witryny na różnych urządzeniach. Można zdefiniować dowolną liczbę stylów w jednym arkuszu stylów bez pisania kodu logiki w celu wybrania stylu. W związku z tym jest to bardzo czujna Metoda adaptacji stylów strony, ponieważ zmniejsza ona liczbę zduplikowanych kodów i logiki na potrzeby renderowania. Z drugiej strony zwiększenie zużycia przepustowości zwiększy się, ponieważ rozmiar plików CSS może wzrosnąć na marginesie.

Przy użyciu techniki dostosowywania adaptacyjnego witryna będzie **wyświetlana prawidłowo, niezależnie od przeglądarki.** Należy jednak wziąć pod uwagę, czy dodatkowe obciążenie przepustowością jest problemem.

> [!NOTE]
> Podstawowy format zapytania o multimedia to: @media \[zakres: wszystkie | urządzenie podręczne | Drukuj | Projekcja |\] ekranu ([Właściwość: Value] i... [Właściwość: Value])

Przykłady zapytań dotyczących multimediów: &gt; **@media wszystkie i (max-width: 1000px) i (min-width: 700px) {}:** dla wszystkich rozdzielczości między 700px i 1000px.

> **@media ekranie i (minimalna szerokość: 400px) i (maks-width: 700px) {...}:** Tylko dla ekranów. Rozwiązanie musi mieć wartość z zakresu od 400 do 700px.
> 
> **@media palmtop i (minimalna szerokość: 20em), ekran i (minimalna szerokość: 20em) {...}:** Dla urządzeń przenośnych i ekranów. Minimalna szerokość musi być większa niż 20em.
> 
> Więcej informacji na ten temat można znaleźć w [witrynie W3C](http://www.w3.org/TR/css3-mediaqueries/).

Teraz można dowiedzieć się, jak działa Render adaptacyjny, co poprawia czytelność szablonu domyślnego witryny sieci Web ASP.NET MVC 4.

1. Otwórz rozwiązanie do **zdjęć. sln** utworzone w zadaniu 1 i wybierz projekt **galerii** . Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.
2. Zmień rozmiar szerokości przeglądarki, ustawiając okna na połowę lub na mniejsze niż w kwartale oryginalnego rozmiaru. Zwróć uwagę na to, co się dzieje z elementami w nagłówku: niektóre elementy nie pojawią się w obszarze widocznym nagłówka.
3. Otwórz plik **site. css** z Eksploratora rozwiązań programu Visual Studio znajdującego się w folderze Project **Content** . Naciśnij **kombinację klawiszy Ctrl + F** , aby otworzyć okno wyszukiwanie zintegrowane programu Visual Studio i napisać `@media`, aby zlokalizować **zapytanie o multimedia CSS**.

    Warunek zapytania o multimedia zdefiniowany w tym szablonie działa w następujący sposób: gdy rozmiar okna przeglądarki wynosi poniżej **850 pikseli**, zastosowane reguły CSS są definiowane w tym bloku nośnika.

    ![Lokalizowanie zapytania o multimedia](whats-new-in-aspnet-mvc-4/_static/image16.png "Lokalizowanie zapytania o multimedia")

    *Lokalizowanie zapytania o multimedia*
4. Zastąp wartość atrybutu max-width ustawioną w 850 piks. **10px**, aby wyłączyć renderowanie adaptacyjne, a następnie naciśnij **klawisze CTRL + S** , aby zapisać zmiany. Wróć do przeglądarki i naciśnij **klawisze CTRL + F5** , aby odświeżyć stronę przy użyciu wprowadzonych zmian. Zauważ różnice w obu stronach podczas dopasowywania szerokości okna.

    ![Po lewej stronie Strona stosuje styl @media w prawym okienku stylu zostanie pominięty.](whats-new-in-aspnet-mvc-4/_static/image17.png "Po lewej stronie Strona stosuje styl @media w prawym okienku stylu zostanie pominięty.")

    *Po lewej stronie Strona stosuje styl @media w prawym okienku stylu zostanie pominięty.*

    Teraz zobaczmy, co się dzieje na urządzeniach przenośnych:

    ![Po lewej stronie Strona stosuje styl @media w prawym okienku stylu zostanie pominięty.](whats-new-in-aspnet-mvc-4/_static/image18.png "Po lewej stronie Strona stosuje styl @media w prawym okienku stylu zostanie pominięty.")

    *Po lewej stronie Strona stosuje styl @media w prawym okienku stylu zostanie pominięty.*

    Mimo że zmiany, gdy strona jest renderowana w przeglądarce sieci Web, nie są bardzo znaczące, podczas korzystania z urządzenia przenośnego różnice stają się bardziej oczywiste. Po lewej stronie obrazu zobaczymy, że styl niestandardowy poprawił czytelność.

    Renderowanie adaptacyjne może być używane w wielu innych scenariuszach, ułatwiając stosowanie stylów warunkowych do witryny sieci Web i rozwiązywanie typowych problemów z stylem z podejściem do zaewidencjonowania.

    Tag meta okienka ekranu i zapytania o multimedia CSS nie są specyficzne dla ASP.NET MVC 4, więc można korzystać z tych funkcji w dowolnej aplikacji sieci Web.
5. W programie Visual Studio naciśnij klawisz **SHIFT** + **F5** , aby zatrzymać debugowanie aplikacji.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Ćwiczenie 2: Tworzenie aplikacji internetowej Photo Gallery

W tym ćwiczeniu będziesz używać aplikacji galerii zdjęć do wyświetlania zdjęć. Zaczniesz od szablonu projektu ASP.NET MVC 4, a następnie dodasz funkcję do pobierania zdjęć z usługi i wyświetlania ich na stronie głównej.

W poniższym ćwiczeniu można zaktualizować to rozwiązanie, aby ulepszyć sposób wyświetlania na urządzeniach przenośnych.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Zadanie 1 — Tworzenie usługi fotografii

To zadanie spowoduje utworzenie makiety usługi fotografii w celu pobrania zawartości, która będzie wyświetlana w galerii. W tym celu dodasz nowy kontroler, który po prostu zwróci plik JSON z danymi każdego zdjęcia.

1. Otwórz **program Visual Studio** , jeśli nie został jeszcze otwarty.
2. Wybierz **plik | Nowy |** Polecenie menu projektu. W oknie dialogowym **Nowy projekt** wybierz **wizualizację C# | Szablon sieci Web** w okienku po lewej stronie, a następnie wybierz **ASP.NET aplikacji sieci Web MVC 4.** Nazwij projekt **Galeria**, wybierz lokalizację (lub pozostaw wartość domyślną), a następnie kliknij przycisk **OK**. Alternatywnie możesz kontynuować pracę z istniejącego rozwiązania **aplikacji internetowej** MVC 4 ASP.NET od **ćwiczenia 1** i pominąć następny krok.
3. W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon projektu **aplikacji internetowej** i kliknij przycisk **OK**. Upewnij się, że wybrano Razor jako aparat widoku.
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy **aplikację\_folder danych** projektu i wybierz polecenie **Dodaj | Istniejący element**. Przejdź do folderu **danych Source\Assets\App\_** tego laboratorium i Dodaj plik **photos. JSON** .
5. Utwórz **nowy kontroler z nazwą.** W tym celu kliknij prawym przyciskiem myszy folder **controllers** , przejdź do pozycji **Dodaj** i wybierz pozycję **kontroler.** Uzupełnij nazwę kontrolera, pozostaw **pusty szablon kontrolera MVC** , a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie sterownika](whats-new-in-aspnet-mvc-4/_static/image19.png "Dodawanie sterownika")

    *Dodawanie sterownika*
6. Zastąp metodę **index** następującą akcją **galerii** i zwróć zawartość z pliku JSON, który został ostatnio dodany do projektu.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex02-Gallery*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie, a następnie przejdź do następującego adresu URL, aby przetestować zainstalowaną usługę fotograficzną: `http://localhost:[port]/photo/gallery` (wartość [port] zależy od bieżącego portu, w którym uruchomiono aplikację). Żądanie do tego adresu URL powinno pobrać zawartość pliku **photos. JSON** .

    ![Testowanie usługi z fotografiami](whats-new-in-aspnet-mvc-4/_static/image20.png "Testowanie usługi z fotografiami")

    *Testowanie usługi z fotografiami*

W prawdziwej implementacji można użyć [interfejsu API sieci Web ASP.NET](../../../../web-api/index.md) w celu zaimplementowania usługi Photo Gallery. Składnik Web API platformy ASP.NET to środowisko ułatwiające tworzenie usług HTTP, które można udostępniać dla wielu różnych klientów, takich jak przeglądarki i urządzenia przenośne. Składnik Web API platformy ASP.NET jest idealną platformą do tworzenia aplikacji o architekturze REST na platformie .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Zadanie 2 — Wyświetlanie galerii zdjęć

To zadanie spowoduje zaktualizowanie strony głównej w celu wyświetlenia galerii zdjęć przy użyciu wbudowanej usługi utworzonej w pierwszym zadaniu tego ćwiczenia. Dodasz pliki modelu i zaktualizujesz widoki galerii.

1. W programie Visual Studio naciśnij klawisz **SHIFT** + **F5** , aby zatrzymać debugowanie aplikacji.
2. Utwórz klasę **Photo** w folderze **models** . Aby to zrobić, kliknij prawym przyciskiem myszy folder **modele** , wybierz polecenie **Dodaj** i kliknij pozycję **Klasa**. Następnie ustaw nazwę na **Photo.cs** , a następnie kliknij przycisk **Dodaj**.
3. Dodaj następujące elementy członkowskie do klasy **Photo** .

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex02-Photo model*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Otwórz plik **HomeController.cs** z folderu **controllers** .
5. Dodaj następujące instrukcje using.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex02-HomeController usings*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Zaktualizuj akcję **indeksu** , aby użyć **HttpClient** do pobrania danych z galerii, a następnie użyć **klasy JavaScriptSerializer** do deserializacji go w modelu widoku.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex02-index*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Otwórz plik **index. cshtml** znajdujący się w folderze **Views\Home** i Zastąp całą zawartość następującym kodem.

    Ten kod przechodzi przez wszystkie zdjęcia pobrane z usługi i wyświetla je na liście nieuporządkowanej.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex02-Photo list*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **zawartości** projektu i wybierz polecenie **Dodaj | Istniejący element**. Przejdź do folderu **Source\Assets\Content** w tym laboratorium i Dodaj plik **site. css** . Konieczne będzie potwierdzenie jego zastąpienia. Jeśli masz otwarty plik **site. css** , konieczne będzie potwierdzenie załadowania pliku także.
9. Otwórz Eksploratora plików i skopiuj cały folder **fotografie** znajdujący się w folderze **Source\Assets** tego laboratorium do folderu głównego projektu w Eksplorator rozwiązań.
10. Uruchom aplikację. Powinna zostać wyświetlona strona główna wyświetlająca zdjęcia w galerii.

    ![Galeria zdjęć](whats-new-in-aspnet-mvc-4/_static/image21.png "Galeria zdjęć")

    *Galeria zdjęć*
11. W programie Visual Studio naciśnij klawisz **SHIFT** + **F5** , aby zatrzymać debugowanie aplikacji.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Ćwiczenie 3: Dodawanie obsługi urządzeń przenośnych

Jedną z najważniejszych aktualizacji w ASP.NET MVC 4 jest wsparcie dla opracowywania aplikacji mobilnych. W tym ćwiczeniu zapoznajesz się z nowymi funkcjami ASP.NET MVC 4 dla aplikacji mobilnych, rozszerzając rozwiązanie z galerii utworzone w poprzednim ćwiczeniu.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Zadanie 1 — Instalowanie oprogramowania jQuery Mobile w aplikacji ASP.NET MVC 4

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex3-MobileSupport/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **konsolę Menedżera pakietów** , klikając pozycję **Narzędzia** > **Menedżer pakietów NuGet** > opcji menu **konsoli Menedżera pakietów** .

    ![Otwieranie konsoli Menedżera pakietów NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "Otwieranie konsoli Menedżera pakietów NuGet")

    *Otwieranie konsoli Menedżera pakietów NuGet*
3. W konsoli Menedżera pakietów Uruchom następujące polecenie, aby zainstalować pakiet **jQuery. Mobile. MVC** .

    jQuery Mobile to Biblioteka open source służąca do tworzenia interfejsu użytkownika sieci Web zoptymalizowanej pod kątem dotyku. Pakiet NuGet platformy jQuery. Mobile. MVC obejmuje pomocników do korzystania z platformy jQuery Mobile z aplikacją ASP.NET MVC 4.

    > [!NOTE]
    > Uruchomienie następującego polecenia spowoduje pobranie biblioteki jQuery. Mobile. MVC z narzędzia NuGet.

    23:59:59

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    To polecenie powoduje zainstalowanie oprogramowania jQuery Mobile i niektórych plików pomocnika, w tym następujących:

    - **Widoki/Shared/\_Layout. Mobile. cshtml**: to układ platformy jQuery oparty na urządzeniach przenośnych zoptymalizowany pod kątem mniejszego ekranu. Gdy witryna sieci Web otrzyma żądanie od przeglądarki mobilnej, zamieni oryginalny układ (\_Layout. cshtml) na ten.
    - Składnik przełącznika widoku: składa się z widoku częściowego **widoków/Shared/\_ViewSwitcher. cshtml** i kontrolera **ViewSwitcherController.cs** . Ten składnik będzie zawierać link w przeglądarkach mobilnych, aby umożliwić użytkownikom przechodzenie do wersji klasycznej strony.  
        ![Projekt galerii zdjęć z obsługą urządzeń przenośnych](whats-new-in-aspnet-mvc-4/_static/image23.png "Projekt galerii zdjęć z obsługą urządzeń przenośnych")

        *Projekt galerii zdjęć z obsługą urządzeń przenośnych*
4. Zarejestruj zbiory mobilne. Aby to zrobić, Otwórz plik **Global.asax.cs** i Dodaj następujący wiersz.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex03-Registering mobileers*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Uruchom aplikację przy użyciu przeglądarki sieci Web na komputerze.
6. Otwórz **Emulator Windows Phone 7** znajdujący się w **menu Start | Wszystkie programy | Windows Phone zestaw SDK 7,1 | Emulator Windows Phone.**
7. Na ekranie startowym telefonu Otwórz program Internet Explorer. Sprawdź adres URL, pod którym uruchomiono aplikację, i przejdź do tego adresu URL za pomocą przeglądarki telefonu (np. `http://localhost:[PortNumber]/`).

    Zobaczysz, że aplikacja będzie wyglądać inaczej w emulatorze Windows Phone, ponieważ jQuery. Mobile. MVC utworzy nowe zasoby w projekcie, które pokazują widoki zoptymalizowane pod kątem urządzeń przenośnych.

    Zwróć uwagę na komunikat znajdujący się u góry telefonu, który pokazuje link do widoku pulpitu. Ponadto układ **\_Layout. Mobile. cshtml** , który został utworzony przez zainstalowaną pakiet, zawiera inny układ w aplikacji.

    > [!NOTE]
    > Do tej pory nie ma linku umożliwiającego powrót do widoku mobilnego. Zostanie ona uwzględniona w nowszych wersjach.

    ![Widok mobilny strony głównej galerii zdjęć](whats-new-in-aspnet-mvc-4/_static/image24.png "Widok mobilny strony głównej galerii zdjęć")

    *Widok mobilny strony głównej galerii zdjęć*
8. W programie Visual Studio naciśnij klawisz **SHIFT** + **F5** , aby zatrzymać debugowanie aplikacji.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Zadanie 2 — Tworzenie widoków mobilnych

W tym zadaniu utworzysz wersję przenośną widoku indeksu z zawartością dostosowaną do lepszego wyglądu na urządzeniach przenośnych.

1. Skopiuj widok **Views\Home\Index.cshtml** i wklej go, aby utworzyć kopię, a następnie zmień nazwę nowego pliku na **index. Mobile. cshtml**.
2. Otwórz widok nowy utworzony **indeks. Mobile. cshtml** i Zastąp istniejący tag &lt;ul&gt; tym kodem. W ten sposób aktualizujesz tag &lt;ul&gt; za pomocą adnotacji danych platformy jQuery Mobile, aby używać motywów mobilnych z platformy jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Zwróć uwagę, że:
    > 
    > - Atrybut **rola danych** ustawiony na **element ListView** będzie renderować listę przy użyciu stylów ListView.
    > 
    > - Atrybut **wstawania danych** ustawiony na wartość true spowoduje wyświetlenie listy z zaokrąglonym obramowaniem i marginesem.
    > 
    > - Atrybut **filtru danych** ustawiony na **wartość true** spowoduje wygenerowanie pola wyszukiwania.
    > 
    > Więcej informacji na temat Konwencji jQuery Mobile można znaleźć w dokumentacji projektu: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.
4. Przejdź do **emulatora Windows Phone** i Odśwież lokację. Zwróć uwagę na nowy wygląd i sposób działania listy galerii, a także nowe pole wyszukiwania znajdujące się u góry. Następnie wpisz słowo w polu wyszukiwania (na przykład **Tulips**), aby rozpocząć wyszukiwanie w Galerii fotografii.

    ![Galeria używająca stylu ListView z filtrowaniem](whats-new-in-aspnet-mvc-4/_static/image25.png "Galeria używająca stylu ListView z filtrowaniem")

    *Galeria używająca stylu ListView z filtrowaniem*

    W celu podsumowania użyto przepisu "Wyświetl program do uruchamiania", aby utworzyć kopię widoku indeksu z sufiksem&quot; &quot;Mobile. Ten sufiks wskazuje na ASP.NET MVC 4, że każde żądanie wygenerowane na urządzeniu przenośnym będzie używać tej kopii indeksu. Ponadto zaktualizowano wersję przenośną widoku indeksu w celu korzystania z platformy jQuery Mobile w celu zwiększenia wyglądu i działania witryny na urządzeniach przenośnych.
5. Wróć do programu Visual Studio i Otwórz **witrynę site. Mobile. css** znajdującą się w folderze **Content** .
6. Popraw położenie tytułu zdjęcia, aby był wyświetlany w prawej części obrazu. Aby to zrobić, Dodaj następujący kod do pliku **site. Mobile. css** .

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.
8. Przełącz się z powrotem do **emulatora Windows Phone** i Odśwież lokację. Zwróć uwagę, że tytuł zdjęcia jest prawidłowo umieszczony.

    ![Tytuł umieszczony po prawej stronie obrazu](whats-new-in-aspnet-mvc-4/_static/image26.png "Tytuł umieszczony po prawej stronie obrazu")

    *Tytuł umieszczony po prawej stronie obrazu*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Zadanie 3 — kompozycje mobilne dla technologii jQuery

Każdy układ i widżet w jQuery Mobile jest zaprojektowana na podstawie nowej struktury CSS zorientowanej obiektowo, która umożliwia stosowanie kompletnego ujednoliconego projektu wizualizacji do witryn i aplikacji.

domyślny motyw platformy jQuery Mobile obejmuje 5 próbek, które są podane litery (a, b, c, d, e), aby uzyskać krótkie informacje.

W tym zadaniu zostanie zaktualizowany układ urządzenia przenośnego w celu użycia innego motywu niż domyślny.

1. Przełącz się z powrotem do programu Visual Studio.
2. Otwórz plik **\_Layout. Mobile. cshtml** znajdujący się w **Views\Shared**.
3. Znajdź element DIV z rolą danych ustawioną na &quot;strony&quot; i zaktualizuj **motyw danych** do &quot;&quot;**e** .

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.
5. Odśwież lokację w **emulatorze Windows Phone** i zwróć uwagę na nowy schemat kolorów.

    ![Układ komórkowy z innym schematem kolorów](whats-new-in-aspnet-mvc-4/_static/image27.png "Układ komórkowy z innym schematem kolorów")

    *Układ komórkowy z innym schematem kolorów*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Zadanie 4 — Używanie składnika do wyświetlania przełączników i funkcji przesłaniania w przeglądarce

Konwencja dla stron sieci Web zoptymalizowanych pod kątem urządzeń przenośnych polega na dodaniu linku, którego tekst jest taki sam jak widok pulpitu lub tryb pełnej witryny, który umożliwia użytkownikom przełączanie do wersji klasycznej strony. Pakiet jQuery. Mobile. MVC zawiera przykładowy składnik **-przełącznik View** do użycia w tym celu w widoku **\_Layout. Mobile. cshtml** .

![Link do przełączania do widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image28.png "Link do przełączania do widoku pulpitu")

*Link do przełączania do widoku pulpitu*

Przełącznik widoku używa nowej funkcji o nazwie " **zastępowanie przeglądarki**". Ta funkcja umożliwia aplikacji traktowanie żądań tak, jakby znajdowały się one w innej przeglądarce (agent użytkownika) niż ta, z której rzeczywiście pochodzą.

W tym zadaniu przedstawiono przykładową implementację przełącznika widoku dodaną przez jQuery. Mobile. MVC i nową przeglądarkę zastępującą funkcje w ASP.NET MVC 4.

1. Przełącz się z powrotem do programu Visual Studio.
2. Otwórz widok **\_Layout. Mobile. cshtml** znajdujący się w folderze **Views\Shared** i zwróć uwagę na składnik-przełącznik widoku, do którego odwołuje się widok częściowy.

    ![Układ urządzenia przenośnego z użyciem składnika do przełączników widoku](whats-new-in-aspnet-mvc-4/_static/image29.png "Układ urządzenia przenośnego z użyciem składnika do przełączników widoku")

    *Układ urządzenia przenośnego z użyciem składnika do przełączników widoku*
3. Otwórz widok częściowy **\_ViewSwitcher. cshtml** .

    Widok częściowy używa nowej metody **ViewContext. HttpContext. GetOverriddenBrowser ()** w celu określenia pochodzenia żądania sieci Web i wyświetlenia odpowiedniego linku w celu przełączenia go do widoków pulpitu lub urządzeń przenośnych.

    Metoda **GetOverriddenBrowser** zwraca wystąpienie **HttpBrowserCapabilitiesBase** , które odnosi się do agenta użytkownika aktualnie ustawionego dla żądania (wartość rzeczywista lub zastąpiona). Możesz użyć tej wartości, aby uzyskać właściwości, takie jak **IsMobileDevice**.

    ![Widok częściowy ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "Widok częściowy ViewSwitcher")

    *Widok częściowy ViewSwitcher*
4. Otwórz klasę **ViewSwitcherController.cs** znajdującą się w folderze **controllers** . Sprawdź, czy akcja SwitchView jest wywoływana przez link w składniku ViewSwitcher i zwróć uwagę na nowe metody HttpContext.

    - Metoda **HttpContext. ClearOverriddenBrowser ()** usuwa dowolnego przesłoniętego agenta użytkownika dla bieżącego żądania.
    - Metoda **HttpContext. SetOverriddenBrowser ()** przesłania rzeczywistą wartość agenta użytkownika żądania przy użyciu określonego agenta użytkownika.  
        ![Kontroler ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "Kontroler ViewSwitcher")  
*Kontroler ViewSwitcher*

        Zastępowanie przeglądarki jest podstawową funkcją ASP.NET MVC 4, która jest również dostępna, nawet jeśli nie instalujesz pakietu jQuery. Mobile. MVC. Jednak ta funkcja ma wpływ tylko na widok, układ i widok częściowy i nie ma wpływu na żadne funkcje, które zależą od obiektu request. browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Zadanie 5 — Dodawanie przełączników widoku w widoku pulpitu

W tym zadaniu zostanie zaktualizowany układ pulpitu w celu uwzględnienia przełączników View. Pozwoli to użytkownikom mobilnym wrócić do widoku mobilnego podczas przeglądania widoku pulpitu.

1. Odśwież lokację w **emulatorze Windows Phone**.
2. Kliknij link **widok pulpitu** w górnej części galerii. Zwróć uwagę na to, że w widoku pulpitu nie ma przełącznika widoku, który umożliwia powrót do widoku mobilnego.
3. Wróć do programu Visual Studio i Otwórz widok **\_Layout. cshtml** .
4. Znajdź sekcję login i Wstaw wywołanie w celu renderowania\_widoku częściowego **ViewSwitcher** poniżej widoku części **\_LogOnPartial** . Następnie naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.
6. Odśwież stronę w emulatorze Windows Phone i kliknij dwukrotnie ekran, aby powiększyć. Zwróć uwagę na to, że na stronie głównej jest teraz wyświetlany link do **widoku mobilnego** , który przełącza się z widoku Mobile do pulpitu.

    ![Wyświetl przełącznik renderowany w widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image32.png "Wyświetl przełącznik renderowany w widoku pulpitu")

    *Wyświetl przełącznik renderowany w widoku pulpitu*
7. Przełącz się do widoku Mobile i przejdź do strony **informacje** (http://localhost[port]/Home/about). Zwróć uwagę, że nawet jeśli widok informacje o. Mobile. cshtml nie został utworzony, Strona informacje jest wyświetlana przy użyciu układu mobilnego (\_Layout. Mobile. cshtml).

    ![Informacje o stronie](whats-new-in-aspnet-mvc-4/_static/image33.png "Informacje o stronie")

    *Informacje o stronie*
8. Na koniec Otwórz witrynę w przeglądarce sieci Web. Zwróć uwagę, że żadna z poprzednich aktualizacji nie ma żadnego na widok pulpitu.

    ![Widok pulpitu galerii](whats-new-in-aspnet-mvc-4/_static/image34.png "Widok pulpitu galerii")

    *Widok pulpitu galerii*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Zadanie 6 — Tworzenie nowych trybów wyświetlania

Nowa funkcja trybów wyświetlania umożliwia aplikacji Wybieranie widoków w zależności od przeglądarki generującej żądanie. Na przykład jeśli przeglądarka pulpitu żąda strony głównej, aplikacja zwróci szablon **Views\Home\Index.cshtml** . Gdy przeglądarka mobilna zażąda strony głównej, aplikacja zwróci szablon **Views\Home\Index.Mobile.cshtml** .

W tym zadaniu utworzysz dostosowany układ dla urządzeń iPhone i konieczne będzie symulowanie żądań od urządzeń iPhone. W tym celu można użyć emulatora lub symulatora telefonu iPhone (na przykład [elektrycznego symulatora urządzeń przenośnych](http://www.electricplum.com/)) lub przeglądarki z dodatkami modyfikującymi agenta użytkownika. Aby uzyskać instrukcje dotyczące sposobu ustawiania ciągu agenta użytkownika w przeglądarce Safari w celu emulowania telefonu iPhone, zobacz [How to let Safari poudawać The IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) w blogu David Alison.

**Zwróć uwagę, że to zadanie jest opcjonalne i możesz kontynuować w całym laboratorium bez jego wykonywania.**

1. W programie Visual Studio naciśnij klawisz **SHIFT** + **F5** , aby zatrzymać debugowanie aplikacji.
2. Otwórz **Global.asax.cs** i Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Dodaj następujący wyróżniony kod do metody Start\_aplikacji.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex03-iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Zarejestrowano nowy **DefaultDisplayMode o nazwie &quot;iPhone&quot;** , w obrębie statycznej listy **DisplayModeProvider. Instance. Modes** list static, która zostanie dopasowana do każdego przychodzącego żądania. Jeśli żądanie przychodzące zawiera ciąg &quot;telefonie iPhone&quot;, ASP.NET MVC znajdzie widoki, których nazwa zawiera sufiks &quot;iPhone&quot;. Parametr 0 wskazuje, jak konkretny jest tryb nowy; na przykład ten widok jest bardziej szczegółowy niż ogólna reguła&quot; &quot;. Mobile, która jest zgodna z żądaniami z urządzeń przenośnych.

Po uruchomieniu tego kodu, gdy przeglądarka telefonu iPhone generuje żądanie, aplikacja będzie korzystać z układu **Views\Shared\\_Layout. iPhone. cshtml** , który zostanie utworzony w następnych krokach.

> [!NOTE]
> W ten sposób testowanie żądania dla telefonu iPhone zostało uproszczone w celach demonstracyjnych i może nie zadziałać zgodnie z oczekiwaniami dla każdego ciągu agenta użytkownika telefonu iPhone (na przykład w testach jest uwzględniana wielkość liter).

4. Utwórz kopię pliku **\_Layout. Mobile. cshtml** w folderze **Views\Shared** i zmień nazwę kopii na &quot; **\_Layout. cshtml**&quot;.
5. Otwórz **\_Layout. iPhone. cshtml** utworzony w poprzednim kroku.
6. Znajdź element DIV z atrybutem rola danych ustawionym na **stronę** i Zmień atrybut **motyw danych** **na &quot;&quot;** .

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Teraz masz 3 układy w aplikacji ASP.NET MVC 4:

1. **\_Layout. cshtml**: domyślny układ używany na potrzeby przeglądarek klasycznych.
2. **\_Layout. Mobile. cshtml**: domyślny układ używany na urządzeniach przenośnych.
3. **\_Layout. iPhone. cshtml**: konkretny układ dla urządzeń iPhone, przy użyciu innego schematu kolorów do odróżnienia od \_układu. Mobile. cshtml.
7. Naciśnij klawisz **F5** , aby uruchomić aplikację i przeglądać witrynę w **emulatorze Windows Phone**.
8. Otwórz **symulator telefonu iPhone** (zobacz [dodatek C](#AppendixC) , aby uzyskać instrukcje dotyczące sposobu instalowania i konfigurowania symulatora dla telefonu iPhone), i przejdź do witryny. Zauważ, że każdy telefon używa określonego szablonu.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Korzystanie z różnych widoków dla każdego urządzenia przenośnego*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Ćwiczenie 4: korzystanie z kontrolerów asynchronicznych

Microsoft .NET Framework 4,5 wprowadza nowe funkcje językowe w C# programie i Visual Basic, aby zapewnić nową podstawę asynchroniczności w programowaniu platformy .NET. Ta nowa podstawa sprawia, że programowanie asynchroniczne jest podobne do-i tak samo jak w przypadku programowania synchronicznego. Teraz można zapisywać metody akcji asynchronicznych w ASP.NET MVC 4 przy użyciu klasy **AsyncController** . Można używać asynchronicznych metod akcji dla długotrwałych żądań, które nie są powiązane z procesorem CPU. Pozwala to uniknąć blokowania działania serwera sieci Web podczas przetwarzania żądania. Klasa AsyncController jest zwykle używana do długotrwałych wywołań usługi sieci Web.

W tym ćwiczeniu objaśniono podstawy operacji asynchronicznej w ASP.NET MVC 4. Jeśli chcesz uzyskać bardziej szczegółowe szczegółowe, możesz zapoznać się z następującym artykułem: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Zadanie 1 — implementowanie kontrolera asynchronicznego

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/EX4-Async/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz klasę **HomeController.cs** z folderu **controllers** .
3. Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Zaktualizuj klasę **HomeController** , aby dziedziczyć z **AsyncController**. Kontrolery, które pochodzą z AsyncController umożliwiają ASP.NET w celu przetwarzania żądań asynchronicznych i mogą nadal obsłużyć synchroniczne metody akcji.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Dodaj słowo kluczowe **Async** do metody **index** , aby zwracało typ **Task&lt;ActionResult&gt;** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Słowo kluczowe **Async** jest jednym z nowych słów kluczowych, które zapewnia .NET Framework 4,5; informuje kompilator, że ta metoda zawiera kod asynchroniczny. Obiekt **Task** reprezentuje operację asynchroniczną, która może zakończyć się w pewnym momencie w przyszłości.
6. Zastąp **klienta. GetAsync ()** wywoływanie z pełną wersją asynchroniczną przy użyciu słowa kluczowego await, jak pokazano poniżej.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex04-GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > W poprzedniej wersji użyto właściwości **wynik** z obiektu **Task** , aby zablokować wątek do momentu zwrócenia wyniku (wersja synchroniczna).
    > 
    > Dodanie słowa kluczowego **await** instruuje kompilator, aby asynchronicznie czekał na zadanie zwrócone przez wywołanie metody. Oznacza to, że pozostała część kodu zostanie wykonana jako wywołanie zwrotne dopiero po zakończeniu oczekiwanej metody. Inną kwestią jest to, że nie trzeba zmieniać bloku try-catch w celu wykonania tej czynności: wyjątki, które występują w tle lub na pierwszym planie, nadal będą przechwytywane bez żadnej dodatkowej pracy przy użyciu programu obsługi dostarczonego przez platformę.
7. Zmień kod, aby kontynuować implementację asynchroniczną przez zastąpienie wierszy nowym kodem, jak pokazano poniżej

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex04-ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Uruchom aplikację. Nie zobaczysz żadnych istotnych zmian, ale Twój kod nie zablokuje wątku z puli wątków w celu lepszego użycia zasobów serwera i zwiększenia wydajności.

    > [!NOTE]
    > Aby dowiedzieć się więcej o nowych funkcjach programowania asynchronicznego w środowisku laboratoryjnym &quot;**programowanie asynchroniczne w programie C# .NET 4,5 z i Visual Basic**&quot; zawartymi w zestawie szkoleniowym programu Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Zadanie 2 — przekroczenie limitu czasu obsługi przy użyciu tokenów anulowania

Asynchroniczne metody akcji, które zwracają wystąpienia zadań, mogą również obsługiwać limity czasu. W tym zadaniu zaktualizujesz kod metody indeksu, aby obsługiwał scenariusz limitu czasu przy użyciu tokenu anulowania.

1. Wróć do programu Visual Studio i naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.
2. Dodaj następującą instrukcję using do pliku **HomeController.cs** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Zaktualizuj akcję indeksu w celu otrzymania argumentu **CancellationToken** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Zaktualizuj wywołanie **GetAsync** , aby przekazać token anulowania.

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex04-SendAsync with CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Dekorować metodę *index* z atrybutem **AsyncTimeout** ustawionym na 500 milisekundy i atrybut **HandleError** skonfigurowany do obsługi **TaskCanceledException** przez przekierowanie do widoku **TimedOut** .

    (Fragment kodu — *ASP.NET MVC 4 Lab-Ex04-Attributes*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Otwórz klasę moje **Kontrolery** i zaktualizuj metodę **galerii** , aby opóźnić wykonanie 1000 milisekund (1 sekund) w celu symulowania długotrwałego zadania.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Otwórz plik **Web. config** i Włącz błędy niestandardowe, dodając następujący element.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Utwórz nowy widok w **Views\Shared** o nazwie **TimedOut** i Użyj domyślnego układu. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder **Views\Shared** i wybierz polecenie **Dodaj | Widok**.

    ![Korzystanie z różnych widoków dla każdego urządzenia przenośnego](whats-new-in-aspnet-mvc-4/_static/image36.png "Korzystanie z różnych widoków dla każdego urządzenia przenośnego")

    *Korzystanie z różnych widoków dla każdego urządzenia przenośnego*
9. Zaktualizuj zawartość widoku **TimedOut** , jak pokazano poniżej.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Uruchom aplikację i przejdź do głównego adresu URL. Po dodaniu **wątku. uśpienie** 1000 milisekund, pojawi się błąd limitu czasu wygenerowane przez atrybut **AsyncTimeout** i Przechwyć przez atrybut **HandleError** .

    ![Obsłużony wyjątek limitu czasu](whats-new-in-aspnet-mvc-4/_static/image37.png "Obsłużony wyjątek limitu czasu")

    *Obsłużony wyjątek limitu czasu*

> [!NOTE]
> Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure w [dodatku D: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixD).

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym ćwiczeniu w laboratorium zostały zaobserwowane niektóre nowe funkcje, które zamieszkują w ASP.NET MVC 4. Omówione zostały następujące koncepcje:

- Skorzystaj z ulepszeń szablonów projektów ASP.NET MVC — w tym szablonu projektu nowej aplikacji mobilnej
- Użycie atrybutu okienka ekranu HTML5 i zapytań o multimedia CSS w celu usprawnienia wyświetlania na urządzeniach przenośnych
- Korzystanie z platformy jQuery Mobile do stopniowego udoskonalania i tworzenia interfejsu użytkownika sieci Web zoptymalizowanego pod kątem dotyku
- Tworzenie widoków specyficznych dla urządzeń przenośnych
- Użycie składnika View-przełącznik do przełączania między widokami mobilnymi i klasycznymi w aplikacji
- Tworzenie kontrolerów asynchronicznych za pomocą obsługi zadań

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Dodatek A: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](whats-new-in-aspnet-mvc-4/_static/image38.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](whats-new-in-aspnet-mvc-4/_static/image39.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](whats-new-in-aspnet-mvc-4/_static/image40.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](whats-new-in-aspnet-mvc-4/_static/image41.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)***

1. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.
2. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
3. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](whats-new-in-aspnet-mvc-4/_static/image42.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](whats-new-in-aspnet-mvc-4/_static/image43.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Dodatek B: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;*Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK*&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Kafelek VS Express for Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Dodatek C: Instalowanie programu WebMatrix 2 i symulatora iPhone

Aby uruchomić witrynę na symulowanym urządzeniu iPhone, można użyć rozszerzenia WebMatrix &quot;elektryczny symulator Mobile dla&quot;iPhone. Ponadto można skonfigurować to samo rozszerzenie do uruchamiania symulatora z programu Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Zadanie 1 — Instalowanie programu WebMatrix 2

1. Przejdź do [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;*WebMatrix 2*&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj program WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Zainstaluj program WebMatrix 2")

    *Zainstaluj program WebMatrix 2*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](whats-new-in-aspnet-mvc-4/_static/image50.png "Akceptowanie postanowień licencyjnych")

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image51.png "Postęp instalacji")

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](whats-new-in-aspnet-mvc-4/_static/image52.png "Instalacja zakończona")

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Zadanie 2 — Instalowanie rozszerzenia symulatora telefonu iPhone

1. Uruchom **WebMatrix** i otwórz istniejącą witrynę sieci Web lub Utwórz nową.
2. Kliknij przycisk **Run (Uruchom** ) na Wstążce **Narzędzia główne** , a następnie wybierz pozycję **Dodaj nowy**.

    ![Dodawanie nowego rozszerzenia WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "Dodawanie nowego rozszerzenia WebMatrix")

    *Dodawanie nowego rozszerzenia WebMatrix*
3. Wybierz pozycję **symulator urządzenia iPhone** , a następnie kliknij przycisk **Instaluj**.

    ![Przeglądanie rozszerzeń WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "Przeglądanie rozszerzeń WebMatrix")

    *Przeglądanie rozszerzeń WebMatrix*
4. W obszarze Szczegóły pakietu kliknij przycisk **Zainstaluj** , aby kontynuować instalację rozszerzenia.

    ![rozszerzenie symulatora telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "rozszerzenie symulatora telefonu iPhone")

    *rozszerzenie symulatora telefonu iPhone*
5. Przeczytaj i zaakceptuj umowę EULA rozszerzenia.

    ![Umowa EULA rozszerzenia WebMatrix](whats-new-in-aspnet-mvc-4/_static/image56.png "Umowa EULA rozszerzenia WebMatrix")

    *Umowa EULA rozszerzenia WebMatrix*
6. Teraz możesz uruchomić witrynę internetową z poziomu WebMatrix przy użyciu opcji symulatora telefonu iPhone.

    ![Uruchamianie przy użyciu telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Uruchamianie przy użyciu telefonu iPhone")

    *Uruchamianie przy użyciu telefonu iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Zadanie 3 — Konfigurowanie programu Visual Studio 2012 do uruchamiania symulatora telefonu iPhone

1. Otwórz **program Visual Studio 2012** i Otwórz dowolną witrynę sieci Web lub Utwórz nowy projekt.
2. Kliknij strzałkę w dół w przycisk Run (Uruchom) i wybierz pozycję **Przeglądaj za pomocą**.

    ![Przeglądaj w usłudze](whats-new-in-aspnet-mvc-4/_static/image58.png "Przeglądaj w usłudze")

    *Przeglądaj w usłudze*
3. W oknie dialogowym &quot;Przeglądaj z&quot; kliknij przycisk **Dodaj**.
4. W oknie dialogowym &quot;Dodawanie programu&quot; Użyj następujących wartości:

   - **Program**: C:\Users\*{CurrentUser} * \AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(odpowiednio zaktualizuj ścieżkę)*
   - **Argumenty**: &quot;1&quot;
   - **Przyjazna nazwa**: symulator telefonu iPhone

     ![Dodaj program](whats-new-in-aspnet-mvc-4/_static/image59.png "Dodaj program")

     *Dodaj program do przeglądania*
5. Kliknij przycisk **OK** i Zamknij okno dialogowe.
6. Teraz można uruchamiać aplikacje sieci Web w symulatorze iPhone z poziomu programu Visual Studio 2012.

    ![Przeglądanie za pomocą symulatora telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image60.png "Przeglądanie za pomocą symulatora telefonu iPhone")

    *Przeglądanie za pomocą symulatora telefonu iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek D: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure

1. Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu. Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do systemu Windows Azure Portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Zaloguj się do systemu Windows Azure Portal")

    *Zaloguj się do usługi Windows Azure portal zarządzania*
2. Na pasku poleceń kliknij pozycję **Nowy** .

    ![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-mvc-4/_static/image62.png "Tworzenie nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**. Następnie wybierz opcję **szybkie tworzenie** . Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią. Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu. Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](whats-new-in-aspnet-mvc-4/_static/image63.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")

    *Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*
4. Poczekaj na utworzenie nowej **witryny sieci Web** .
5. Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** . Sprawdź, czy nowa witryna sieci Web działa.

    ![Przeglądanie w nowej witrynie sieci Web](whats-new-in-aspnet-mvc-4/_static/image64.png "Przeglądanie w nowej witrynie sieci Web")

    *Przeglądanie w nowej witrynie sieci Web*

    ![Uruchomiona witryna sieci Web](whats-new-in-aspnet-mvc-4/_static/image65.png "Uruchomiona witryna sieci Web")

    *Uruchomiona witryna sieci Web*
6. Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania witryną sieci Web](whats-new-in-aspnet-mvc-4/_static/image66.png "Otwieranie stron zarządzania witryną sieci Web")

    *Otwieranie stron zarządzania witryną sieci Web*
7. Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji. **Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.

    ![Pobieranie profilu publikowania witryny sieci Web](whats-new-in-aspnet-mvc-4/_static/image67.png "Pobieranie profilu publikowania witryny sieci Web")

    *Pobieranie profilu publikowania witryny sieci Web*
8. Pobierz plik profilu publikowania do znanej lokalizacji. W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image68.png "Zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer. Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.

1. Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database. Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń. Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach. Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.

    ![Pulpit nawigacyjny serwera SQL Database](whats-new-in-aspnet-mvc-4/_static/image69.png "Pulpit nawigacyjny serwera SQL Database")

    *Pulpit nawigacyjny serwera SQL Database*
2. W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera. W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](whats-new-in-aspnet-mvc-4/_static/image70.png).

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Dodawanie adresu IP klienta*
3. Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.

    ![Potwierdź zmiany](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Potwierdź zmiany*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.

    ![Publikowanie aplikacji](whats-new-in-aspnet-mvc-4/_static/image73.png "Publikowanie aplikacji")

    *Publikowanie witryny sieci Web*
2. Zaimportuj profil publikowania zapisany w pierwszym zadaniu.

    ![Importowanie profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image74.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij pozycję **Weryfikuj połączenie**. Po zakończeniu walidacji kliknij przycisk **dalej**.

    > [!NOTE]
    > Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.

    ![Weryfikowanie połączenia](whats-new-in-aspnet-mvc-4/_static/image75.png "Weryfikowanie połączenia")

    *Weryfikowanie połączenia*
4. Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).

    ![Konfiguracja narzędzia Web Deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "Konfiguracja narzędzia Web Deploy")

    *Konfiguracja narzędzia Web Deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .
   - W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.
   - W polu **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nową nazwę bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia docelowego](whats-new-in-aspnet-mvc-4/_static/image77.png "Konfigurowanie parametrów połączenia docelowego")

     *Konfigurowanie parametrów połączenia docelowego*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.

    ![Tworzenie bazy danych](whats-new-in-aspnet-mvc-4/_static/image78.png "Tworzenie ciągu bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie. Następnie kliknij przycisk **Next** (Dalej).

    ![Parametry połączenia wskazujące SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Parametry połączenia wskazujące SQL Database")

    *Parametry połączenia wskazujące SQL Database*
8. Na stronie **Podgląd** kliknij przycisk **Publikuj**.

    ![Publikowanie aplikacji sieci Web](whats-new-in-aspnet-mvc-4/_static/image80.png "Publikowanie aplikacji sieci Web")

    *Publikowanie aplikacji sieci Web*
9. Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.

    ![Aplikacja opublikowana w systemie Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Aplikacja opublikowana w systemie Windows Azure")

    *Aplikacja opublikowana w systemie Windows Azure*

---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Określanie, które pliki muszą zostać wdrożoneC#() | Microsoft Docs
author: rick-anderson
description: Jakie pliki muszą zostać wdrożone ze środowiska programistycznego w środowisku produkcyjnym, zależnie od tego, czy aplikacja ASP.NET została skompilowana...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 1dd4a1179d32f776626c08a07205dc9aabed588d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74596400"
---
# <a name="determining-what-files-need-to-be-deployed-c"></a>Określanie, które pliki muszą zostać wdrożone (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Jakie pliki muszą zostać wdrożone ze środowiska programistycznego w środowisku produkcyjnym, zależnie od tego, czy aplikacja ASP.NET została skompilowana przy użyciu modelu witryny sieci Web, czy modelu aplikacji sieci Web. Dowiedz się więcej o tych dwóch modelach projektów oraz o tym, jak model projektu wpływa na wdrożenie.

## <a name="introduction"></a>Wprowadzenie

Wdrożenie aplikacji sieci Web ASP.NET wiąże się z kopiowaniem plików związanych z ASP.NET ze środowiska deweloperskiego do środowiska produkcyjnego. Pliki powiązane z ASP.NET obejmują znaczniki stron sieci Web ASP.NET i kod oraz pliki obsługi po stronie klienta i serwera. Pliki obsługi po stronie klienta to pliki, do których odwołują się strony sieci Web i wysyłane bezpośrednio do obrazów przeglądarki, plików CSS i plików JavaScript, na przykład. Pliki obsługi po stronie serwera obejmują te, które są używane do przetwarzania żądania po stronie serwera. Dotyczy to plików konfiguracji, usług sieci Web, plików klas, wpisanych zestawów danych i plików LINQ to SQL, między innymi.

Ogólnie rzecz biorąc, wszystkie pliki obsługi po stronie klienta powinny zostać skopiowane ze środowiska projektowego do środowiska produkcyjnego, ale te pliki obsługi po stronie serwera są kopiowane, zależnie od tego, czy jest jawnie kompilowany kod po stronie serwera do zestawu (plik `.dll`) czy te zestawy są generowane automatycznie. W tym samouczku przedstawiono, które pliki muszą zostać wdrożone w przypadku jawnego kompilowania kodu do zestawu, a ten krok kompilacji następuje automatycznie.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Kompilacja jawna a kompilacja automatyczna

ASP.NET strony sieci Web są podzielone na znaczniki deklaratywne i kod źródłowy. Deklaratywna część znaczników obejmuje kod HTML, kontrolki sieci Web i składnię wiązania danych. część kodu zawiera programy obsługi zdarzeń zapisywane w Visual Basic lub C# kodzie. Fragmenty znaczników i kodu są zwykle rozdzielone na różne pliki: `WebPage.aspx` zawiera znaczniki deklaratywne, podczas gdy `WebPage.aspx.cs` zamieszczono w kodzie.

Rozważmy stronę ASP.NET o nazwie Clock. aspx, która zawiera kontrolkę etykiety, której właściwość Text jest ustawiona na bieżącą datę i godzinę ładowania strony. Deklaratywna część znaczników (w `Clock.aspx`) będzie zawierać znaczniki dla kontrolki sieci Web etykiety,`<asp:Label runat="server" id="TimeLabel" />`-podczas gdy część kodu (w `Clock.aspx.cs`) będzie miała `Page_Load` obsługi zdarzeń z następującym kodem:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Aby aparat ASP.NET mógł obsłużyć żądanie dotyczące tej strony, należy najpierw skompilować część kodu strony (plik `WebPage.aspx.cs`). Ta kompilacja może być wykonana jawnie lub automatycznie.

Jeśli kompilacja jest wykonywana jawnie, cały kod źródłowy aplikacji jest kompilowany do co najmniej jednego zestawu (`.dll` plików) znajdującego się w katalogu `Bin` aplikacji. Jeśli kompilacja odbywa się automatycznie, powstały wygenerowany automatycznie zestaw jest domyślnie umieszczony w folderze plików `Temporary ASP.NET`, który można znaleźć w `%WINDOWS%\Microsoft.NET\Framework\` *&lt;wersji&gt;* , mimo że ta lokalizacja jest konfigurowalna za pośrednictwem [`<compilation>` elementu](https://msdn.microsoft.com/library/s10awwz0.aspx) w `Web.config`. W przypadku jawnej kompilacji należy wykonać jakąś akcję, aby skompilować kod aplikacji ASP.NET do zestawu i ten krok występuje przed wdrożeniem. Dzięki automatycznej kompilacji proces kompilacji odbywa się na serwerze sieci Web, gdy następuje pierwszy dostęp do zasobu.

Niezależnie od tego, jaki model kompilacji jest używany, fragment znacznika wszystkich stron ASP.NET (pliki `WebPage.aspx`) musi być kopiowany do środowiska produkcyjnego. Z jawną kompilacją należy skopiować zestawy w folderze `Bin`, ale nie trzeba kopiować fragmentów kodu stron ASP.NET (plików `WebPage.aspx.cs`). Dzięki automatycznej kompilacji potrzebne jest skopiowanie plików fragmentów kodu, aby kod był obecny i mógł zostać skompilowany automatycznie podczas odwiedzania strony. Fragment znacznika każdej strony sieci Web ASP.NET zawiera dyrektywę `@Page` z atrybutami wskazującymi, czy skojarzony kod strony został już jawnie skompilowany lub czy musi być automatycznie kompilowany. W związku z tym środowisko produkcyjne może bezproblemowo współpracować z modelem kompilacji i nie trzeba stosować żadnych specjalnych ustawień konfiguracji, aby wskazać, że używana jest jawna lub automatyczna kompilacja.

Tabela 1 podsumowuje różne pliki do wdrożenia podczas korzystania z kompilacji jawnej i automatycznej kompilacji. Należy pamiętać, że niezależnie od używanego modelu kompilacji należy zawsze wdrażać zestawy w folderze `Bin`, jeśli ten folder istnieje. Folder `Bin` zawiera zestawy specyficzne dla aplikacji sieci Web, które obejmują skompilowany kod źródłowy przy użyciu jawnego modelu kompilacji. Katalog `Bin` zawiera również zestawy z innych projektów i wszystkich zestawów typu "open source" lub innych firm, które mogą być używane, i muszą znajdować się na serwerze produkcyjnym. W związku z tym, jako ogólna reguła kciuka, skopiuj folder `Bin` do środowiska produkcyjnego podczas wdrażania. (Jeśli używasz modelu kompilacji automatycznej i nie korzystasz z żadnych zestawów zewnętrznych, nie masz katalogu `Bin` — to wszystko jest prawidłowe!)

| **Model kompilacji** | **Czy wdrożyć plik fragmentu znaczników?** | **Czy wdrożyć plik kodu źródłowego?** | **Czy wdrożyć zestawy w `Bin` katalogu?** |
| --- | --- | --- | --- |
| Kompilacja jawna | Tak | Nie | Tak |
| Kompilacja automatyczna | Tak | Tak | Tak (jeśli istnieje) |

**Tabela 1:** Wdrażane pliki są zależne od używanego modelu kompilacji.

## <a name="taking-a-trip-down-memory-lane"></a>Trwa wyjazdowanie ścieżki pamięci

Używane podejście kompilacji zależy od części, w jaki sposób aplikacja ASP.NET jest zarządzana w programie Visual Studio. Fire. W roku 2000 istnieją cztery różne wersje programu Visual Studio — Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 i Visual Studio 2008. Zarządzane aplikacje ASP.NET programu Visual Studio .NET 2002 i 2003 przy użyciu *modelu projektu aplikacji sieci Web*. Najważniejsze funkcje modelu projektu aplikacji sieci Web:

- Pliki, które korzeń projekt są zdefiniowane w pojedynczym pliku projektu. Wszystkie pliki, które nie są zdefiniowane w pliku projektu, nie są uważane za część aplikacji sieci Web przez program Visual Studio.
- Używa kompilacji jawnej. Kompilowanie projektu kompiluje pliki kodu w ramach projektu do jednego zestawu, który znajduje się w folderze `Bin`.

Gdy firma Microsoft wywydała program Visual Studio 2005, porzuciła obsługę modelu projektu aplikacji sieci Web i zamieni go na model projektu witryny sieci Web. Model projektu witryny sieci Web różni się od modelu projektu aplikacji sieci Web w następujący sposób:

- Zamiast pojedynczego pliku projektu, który sprawdza pliki projektu, używany jest system plików. W skrócie wszystkie pliki w folderze aplikacji sieci Web (lub podfolderach) są uważane za część projektu.
- Kompilowanie projektu w programie Visual Studio nie powoduje utworzenia zestawu w katalogu `Bin`. Zamiast tego, kompilowanie projektu witryny sieci Web zgłasza wszelkie błędy czasu kompilacji.
- Obsługa automatycznej kompilacji. Projekty witryn sieci Web są zwykle wdrażane przez skopiowanie znaczników i kodu źródłowego do środowiska produkcyjnego, chociaż kod może być wstępnie skompilowany (kompilacja jawna).

Firma Microsoft odradza model projektu aplikacji sieci Web po wydaniu programu Visual Studio 2005 z dodatkiem Service Pack 1. Jednak Visual Web Developer nadal obsługuje tylko model projektu witryny sieci Web. Dobra wiadomość polega na tym, że to ograniczenie zostało porzucone z programem Visual Web Developer 2008 z dodatkiem Service Pack 1. Obecnie można tworzyć aplikacje ASP.NET w programie Visual Studio (i Visual Web Developer) przy użyciu modelu projektu aplikacji sieci Web lub modelu projektu witryny sieci Web. Oba modele mają swoje zalety i wady. Zapoznaj się z artykułem [wprowadzenie do projektów aplikacji sieci Web: porównywanie projektów witryny sieci Web i projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) w celu porównania dwóch modeli i aby pomóc w określeniu, który model projektu najlepiej sprawdza się w danej sytuacji.

## <a name="exploring-the-sample-web-application"></a>Eksplorowanie przykładowej aplikacji sieci Web

Pobranie dla tego samouczka obejmuje aplikację ASP.NET o nazwie Recenzje książki. Witryna sieci Web zostanie wykorzystana z witryny sieci Web hobby, którą ktoś może utworzyć, aby podzielić się swoimi recenzjami z społecznością online. Ta aplikacja sieci Web ASP.NET jest bardzo prosta i składa się z następujących zasobów:

- `Web.config`, plik konfiguracyjny aplikacji.
- Strona wzorcowa (`Site.master`).
- Siedem różnych ASP.NET stron: 

    - ~`/Default.aspx`— Strona główna witryny.
    - ~`/About.aspx` — Strona "informacje o witrynie".
    - ~`/Fiction/Default.aspx` — Strona zawierająca listę sprawdzonych ksiąg fikcja. 

        - ~`/Fiction/Blaze.aspx` — przegląd Richard Bachman nowej *Blaze*.
    - ~/`Tech/Default.aspx` — Strona zawierająca listę książek technologicznych, które zostały zrecenzowane. 

        - ~/`Tech/CYOW.aspx`— przegląd *tworzenia własnej witryny sieci Web*.
        - ~/`Tech/TYASP35.aspx` — przegląd *nauki ASP.NET 3,5 w ciągu 24 godzin*.
- Trzy różne pliki CSS w folderze Style.
- Cztery pliki obrazów — logo obsługiwane przez ASP.NET i obrazy okładek trzech sprawdzonych ksiąg — wszystkie znajdujące się w folderze `Images`.
- Plik `Web.sitemap`, który definiuje mapę witryny i służy do wyświetlania menu na stronach `Default.aspx` w katalogu głównym oraz w folderach `Fiction` i `Tech`.
- Plik klasy o nazwie `BasePage.cs`, który definiuje klasę `Page` bazowej. Ta Klasa rozszerza funkcjonalność klasy `Page` przez automatyczne ustawienie właściwości `Title` na podstawie położenia strony na mapie witryny. W Nutshell, każda klasa ASP.NET z kodem, która rozszerza `BasePage` (zamiast `System.Web.UI.Page`) będzie mieć ustawioną wartość, w zależności od jej pozycji w mapie witryny. Na przykład podczas wyświetlania strony ~/`Tech/CYOW.aspx` tytuł jest ustawiony na "Strona główna: Technologia: Tworzenie własnej witryny sieci Web".

Rysunek 1 przedstawia zrzut ekranu witryny internetowej przeglądów książki, gdy jest wyświetlany za pomocą przeglądarki. W tym miejscu zostanie wyświetlona strona ~/`Tech/TYASP35.aspx`, która przegląda książkę z *ASP.NET 3,5 w ciągu 24 godzin*. Pasek nawigacyjny obejmujący górną część strony i menu w lewej kolumnie są zależne od struktury mapy witryny zdefiniowanej w `Web.sitemap`. Obraz w prawym górnym rogu jest jednym z obrazów okładki książki znajdujących się w folderze `Images`. Wygląd i działanie witryny sieci Web są definiowane za pośrednictwem reguł kaskadowego arkusza stylów, które są opisane przez pliki CSS w folderze Style, podczas gdy układ strony z przełożeniem jest zdefiniowany na stronie wzorcowej `Site.master`.

[![witryna sieci Web przeglądający książki oferuje Przeglądy dotyczące asortymentu tytułów](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Rysunek 1.** Witryna internetowa przeglądów książek oferuje Przeglądy dotyczące asortymentu tytułów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](determining-what-files-need-to-be-deployed-cs/_static/image3.png))

Ta aplikacja nie korzysta z bazy danych programu; Każdy przegląd jest implementowany jako osobna strona sieci Web w aplikacji. W tym samouczku (i następnych kilku samouczkach) omówiono wdrażanie aplikacji sieci Web, która nie ma bazy danych. Jednak w przyszłym samouczku poprawimy tę aplikację do przechowywania przeglądów, komentarzy do czytelników i innych informacji w bazie danych, a także dowiesz się, jakie kroki należy wykonać w celu prawidłowego wdrożenia aplikacji sieci Web opartej na danych.

> [!NOTE]
> Te samouczki koncentrują się na hostingu aplikacji ASP.NET z dostawcą hosta sieci Web i nie eksplorują pomocniczych tematów, takich jak ASP. System mapy witryny sieci lub korzysta z klasy `Page` podstawowej. Aby uzyskać więcej informacji na temat tych technologii, a także w celu uzyskania większej liczby innych tematów omówionych w samouczku, zapoznaj się z sekcją dalsze informacje na końcu każdego samouczka.

Ten samouczek pobiera dwie kopie aplikacji sieci Web, każdy zaimplementowany jako inny typ projektu programu Visual Studio: BookReviewsWAP, projekt aplikacji sieci Web i BookReviewsWSP, projekt witryny sieci Web. Oba projekty zostały utworzone za pomocą programu Visual Web Developer 2008 SP1 i używają ASP.NET 3,5 z dodatkiem SP1. Aby rozpocząć pracę z tymi projektami, należy rozpakować zawartość na pulpicie. Aby otworzyć projekt aplikacji sieci Web (BookReviewsWAP), przejdź do folderu BookReviewsWAP i kliknij dwukrotnie plik rozwiązania, `BookReviewsWAP.sln`. Aby otworzyć projekt witryny sieci Web (BookReviewsWSP), uruchom program Visual Studio, a następnie w menu plik wybierz opcję Otwórz witrynę sieci Web, przejdź do folderu `BookReviewsWSP` na pulpicie, a następnie kliknij przycisk OK.

Pozostałe dwie sekcje w tym samouczku zapoznają się z plikami, które będą potrzebne do skopiowania do środowiska produkcyjnego podczas wdrażania aplikacji. Następne dwa samouczki — *[wdrażanie witryny przy użyciu protokołu FTP](deploying-your-site-using-an-ftp-client-cs.md)* i *[wdrażanie witryny przy użyciu programu Visual Studio](deploying-your-site-using-visual-studio-cs.md)* — Pokaż różne sposoby kopiowania tych plików do dostawcy hosta sieci Web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Określanie plików do wdrożenia dla projektu aplikacji sieci Web

Model projektu aplikacji sieci Web używa jawnej kompilacji — kod źródłowy projektu jest kompilowany do jednego zestawu przy każdym kompilowaniu aplikacji. Ta kompilacja zawiera pliki związane z kodem ASP.NET strony (~/`Default.aspx.cs`, ~/`About.aspx.cs`itd.), a także klasę `BasePage.cs`. Zestaw, który powstaje, nosi nazwę BookReviewsWAP. dll i znajduje się w katalogu `Bin` aplikacji.

Rysunek 2 przedstawia pliki, które składają się na projekt aplikacji sieci Web.

[![Eksplorator rozwiązań wyświetla listę plików wchodzących w skład projektu aplikacji sieci Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Rysunek 2**. Eksplorator rozwiązań wyświetla listę plików wchodzących w skład projektu aplikacji sieci Web

Aby wdrożyć aplikację ASP.NET opracowaną przy użyciu modelu projektu aplikacji sieci Web, można utworzyć aplikację, tak aby jawnie skompilować najnowszy kod źródłowy do zestawu. Następnie skopiuj następujące pliki do środowiska produkcyjnego:

- Pliki zawierające znaczniki deklaracyjne dla każdej strony ASP.NET, takie jak ~/`Default.aspx`, ~/`About.aspx`i tak dalej. Ponadto Skopiuj deklaratywne znaczniki dla wszystkich stron wzorcowych i kontrolek użytkownika.
- Zestawy (pliki`.dll`) w folderze `Bin`. Nie trzeba kopiować plików bazy danych programu (`.pdb`) ani żadnych plików XML, które mogą znajdować się w katalogu `Bin`.

Nie trzeba kopiować plików kodu źródłowego stron ASP.NET do środowiska produkcyjnego ani nie trzeba kopiować pliku klasy `BasePage.cs`.

> [!NOTE]
> Jak pokazano na rysunku 2, Klasa `BasePage` jest zaimplementowana jako plik klasy w projekcie, umieszczony w folderze o nazwie `HelperClasses`. Podczas kompilowania projektu kod w pliku `BasePage.cs` jest kompilowany wraz z klasami ASP.NET stron "w jednym zestawie, `BookReviewsWAP.dll.` ASP.NET ma specjalny folder o nazwie `App_Code`, który jest przeznaczony do przechowywania plików klas dla projektów witryny sieci Web. Kod w folderze `App_Code` jest kompilowany automatycznie i dlatego nie powinien być używany z projektami aplikacji sieci Web. Zamiast tego należy umieścić pliki klas aplikacji w normalnym folderze o nazwie `HelperClasses`lub `Classes`lub coś podobnego. Alternatywnie można umieścić pliki klas w osobnym projekcie biblioteki klas.

Oprócz kopiowania plików znaczników związanych z ASP.NET i zestawu w folderze `Bin`, należy również skopiować pliki obsługi po stronie klienta — obrazy i pliki CSS oraz inne pliki obsługi po stronie serwera, `Web.config` i `Web.sitemap`. Te pliki obsługi klienta i serwera muszą być kopiowane do środowiska produkcyjnego, niezależnie od tego, czy jest używana kompilacja jawna, czy automatyczna.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Określanie plików do wdrożenia dla plików projektu witryny sieci Web

Model projektu witryny sieci Web obsługuje kompilację automatyczną, a funkcja nie jest dostępna w przypadku używania modelu projektu aplikacji sieci Web. Z jawną kompilacją należy skompilować kod źródłowy projektu do zestawu i skopiować ten zestaw do środowiska produkcyjnego. Z drugiej strony, z automatyczną kompilacją, wystarczy skopiować kod źródłowy do środowiska produkcyjnego, który jest kompilowany przez środowisko uruchomieniowe na żądanie zgodnie z potrzebami.

Opcja menu Kompilacja w programie Visual Studio jest obecna zarówno w projektach aplikacji sieci Web, jak i w projektach witryn sieci Web. Kompilowanie projektów aplikacji sieci Web kompiluje kod źródłowy projektu do jednego zestawu znajdującego się w katalogu `Bin`; Kompilowanie projektu witryny sieci Web sprawdza wszystkie błędy czasu kompilacji, ale nie tworzy żadnych zestawów. Aby wdrożyć aplikację ASP.NET opracowaną przy użyciu modelu projektu witryny sieci Web, wystarczy skopiować odpowiednie pliki do środowiska produkcyjnego, ale zachęcamy do wcześniejszego skompilowania projektu, aby upewnić się, że nie występują błędy czasu kompilacji.

Rysunek 3 przedstawia pliki, które składają się na projekt witryny sieci Web.

 [![Eksplorator rozwiązań wyświetla listę plików wchodzących w skład projektu witryny sieci Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Rysunek 3**. Eksplorator rozwiązań wyświetla listę plików wchodzących w skład projektu witryny sieci Web

Wdrożenie projektu witryny sieci Web polega na skopiowaniu wszystkich plików związanych z ASP.NET do środowiska produkcyjnego, w tym stron znaczników dla stron ASP.NET, stron wzorcowych i kontrolek użytkownika, wraz z plikami kodu. Należy również skopiować wszystkie pliki klas, takie jak BasePage.cs. Należy pamiętać, że plik `BasePage.cs` znajduje się w folderze `App_Code`, który jest specjalnym folderem ASP.NET używanym w projektach witryn sieci Web dla plików klas. Folder specjalny należy utworzyć w środowisku produkcyjnym, a także, ponieważ pliki klas w folderze `App_Code` środowiska deweloperskiego muszą zostać skopiowane do folderu `App_Code` na etapie produkcji.

Oprócz kopiowania ASP.NET znaczników i plików kodu źródłowego należy również skopiować pliki obsługi po stronie klienta — obrazy i pliki CSS oraz inne pliki obsługi po stronie serwera, `Web.config` i `Web.sitemap`.

> [!NOTE]
> Projekty witryn sieci Web mogą również używać kompilacji jawnej. W przyszłości zapoznaj się z tym, jak jawnie skompilować projekt witryny sieci Web.

## <a name="summary"></a>Podsumowanie

Wdrożenie aplikacji ASP.NET wiąże się z kopiowaniem niezbędnych plików ze środowiska deweloperskiego do środowiska produkcyjnego. Dokładny zestaw plików, które muszą być synchronizowane, zależy od tego, czy kod aplikacji ASP.NET jest jawnie czy automatycznie kompilowany. Zastosowana strategia kompilacji ma wpływ na to, czy program Visual Studio jest skonfigurowany do zarządzania aplikacją ASP.NET przy użyciu modelu projektu aplikacji sieci Web lub modelu projektu witryny sieci Web.

Model projektu aplikacji sieci Web używa kompilacji jawnej i kompiluje kod projektu do jednego zestawu w folderze `Bin`. Podczas wdrażania aplikacji fragment znacznika stron ASP.NET i zawartość folderu `Bin` muszą zostać wypchnięte do środowiska produkcyjnego. kod źródłowy w aplikacji — pliki kodu i klasy związane z kodem, na przykład nie muszą być kopiowane do środowiska produkcyjnego.

Model projektu witryny sieci Web domyślnie używa kompilacji automatycznej, chociaż można jawnie skompilować projekt witryny sieci Web, ponieważ będzie on widoczny w przyszłych samouczkach. Wdrożenie aplikacji ASP.NET używającej automatycznej kompilacji wymaga, aby część znaczników *i* kod źródłowy musiały zostać skopiowane do środowiska produkcyjnego. Kod jest automatycznie kompilowany w środowisku produkcyjnym, gdy jest wymagany po raz pierwszy.

Teraz, gdy analizujemy, które pliki muszą zostać zsynchronizowane między środowiskami deweloperskimi i produkcyjnymi, zalecamy wdrożenie aplikacji do przeglądu książek dla dostawcy hosta sieci Web.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Przegląd kompilacji ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Kontrolki użytkownika ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Badanie ASP. Nawigacja w witrynie sieci](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Wprowadzenie do projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Samouczki stron wzorcowych](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Udostępnianie kodu między stronami](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Używanie niestandardowej klasy bazowej dla klas ASP.NET stron związanych z kodem](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [System projektu witryny sieci Web programu Visual Studio 2005: co to jest i dlaczego?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Przewodnik: konwertowanie projektu witryny sieci Web na projekt aplikacji sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Poprzednie](asp-net-hosting-options-cs.md)
> [dalej](deploying-your-site-using-an-ftp-client-cs.md)

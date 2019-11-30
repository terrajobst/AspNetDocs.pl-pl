---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: Tworzenie układu dla całej witryny za pomocą stron wzorcowychC#() | Microsoft Docs
author: rick-anderson
description: W tym samouczku przedstawiono podstawowe informacje o stronie głównej. Co oznacza, co to są strony wzorcowe, jak tworzy jedną stronę wzorcową, co oznacza posiadaczy zawartości, w jaki sposób jest to jedna CR...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a5e85c443a2a3642ec185ab1897c43cdb2ab1f7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74619514"
---
# <a name="creating-a-site-wide-layout-using-master-pages-c"></a>Tworzenie układu dla całej witryny za pomocą stron wzorcowych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> W tym samouczku przedstawiono podstawowe informacje o stronie głównej. Co oznacza, co to są strony wzorcowe, jak tworzy jedną stronę wzorcową, co oznacza posiadaczy zawartości, w jaki sposób tworzy stronę ASP.NET, która korzysta z strony wzorcowej, w jaki sposób modyfikowanie strony wzorcowej jest automatycznie odzwierciedlane na skojarzonych stronach zawartości itd.

## <a name="introduction"></a>Wprowadzenie

Jeden atrybut dobrze zaprojektowanej witryny sieci Web jest spójnym układem strony w całej lokacji. Zapoznaj się z witryną sieci Web www.asp.net, na przykład. Podczas tego pisania każda strona ma tę samą zawartość u góry i u dołu strony. Jak pokazano na rysunku 1, w górnej części każdej strony jest wyświetlany szary pasek z listą społeczności firmy Microsoft. Poniżej znajduje się logo lokacji, lista języków, do których przeprowadzono translację lokacji, oraz podstawowe sekcje: Strona główna, rozpoczęcie, uczenie, pobieranie i tak dalej. Podobnie, w dolnej części strony znajdują się informacje dotyczące reklamy w www.asp.net, zasady autorskiej i łącze do zasad zachowania poufności informacji.

[![witryna sieci Web www.asp.net wykorzystuje spójny wygląd i działanie na wszystkich stronach](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

<strong>Ilustracja 01</strong>. witryna sieci Web www.ASP.NET korzysta ze spójnego wyglądu i działania na wszystkich stronach ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))

Innym atrybutem dobrze zaprojektowanej witryny jest łatwość, za pomocą których można zmienić wygląd witryny. Na rysunku 1 przedstawiono stronę główną www.asp.net od marca 2008, ale między nimi a publikacją w tym samouczku można zmienić wygląd i działanie. Prawdopodobnie elementy menu na górze zostaną rozwinięte w celu uwzględnienia nowej sekcji dla struktury MVC. Lub może to być radykalnie nowy projekt z różnymi kolorami, czcionkami i układem przedstawiliśmy. Zastosowanie takich zmian w całej lokacji powinno być procesem szybkim i prostym, który nie wymaga modyfikacji tysięcy stron sieci Web, które tworzą lokację.

Tworzenie szablonu strony dla całej witryny w programie ASP.NET jest możliwe za pomocą *stron wzorcowych*. W Nutshell Strona wzorcowa jest specjalnym typem strony ASP.NET, która definiuje znaczniki, które są wspólne dla wszystkich *stron zawartości* , a także regiony, które można dostosować na podstawie strony zawartości na stronie zawartość. (Strona zawartości jest stroną ASP.NET, która jest powiązana z stroną wzorcową). Za każdym razem, gdy zmieni się układ lub formatowanie strony wzorcowej, wszystkie dane wyjściowe stron zawartości są natychmiast aktualizowane, co sprawia, że wprowadzanie zmian wyglądu całej witryny odbywa się tak samo jak aktualizowanie i wdrażanie pojedynczego pliku (tj. strony głównej).

Jest to pierwszy samouczek z serii samouczków, które eksplorują korzystanie z stron wzorcowych. W ramach tej serii samouczków:

- Badaj tworzenie stron wzorcowych i skojarzonych z nimi stron zawartości,
- Omawiaj różne porady, wskazówki i pułapki,
- Zidentyfikuj wspólną stronę wzorcową pułapek i Eksploruj obejścia,
- Zobacz, jak uzyskać dostęp do strony wzorcowej ze strony zawartości i vice versa,
- Dowiedz się, jak określić stronę wzorcową strony zawartości w czasie wykonywania i
- Inne tematy zaawansowanej strony wzorcowej.

Te samouczki są zwięzłe i zawierają instrukcje krok po kroku dotyczące dużej ilości zrzutów ekranu, aby przeprowadzić Cię przez proces wizualnie. Każdy samouczek jest dostępny w C# systemach i Visual Basic wersje i zawiera pobieranie pełnego użytego kodu.

Ten samouczek Inaugural rozpoczyna się od podstaw na stronie wzorcowej. Omawiamy sposób działania stron wzorcowych, przyjrzyj się tworzeniu strony wzorcowej i skojarzonych stronach zawartości przy użyciu programu Visual Web Developer i zobacz, jak zmiany na stronie wzorcowej są natychmiast odzwierciedlane na stronach zawartości. Zacznijmy!

## <a name="understanding-how-master-pages-work"></a>Zrozumienie, jak działają strony wzorcowe

Kompilowanie witryny sieci Web ze spójnym układem strony w całej lokacji wymaga, aby każda Strona internetowa emituje typowe znaczniki formatowania jako uzupełnienie zawartości niestandardowej. Na przykład, chociaż każdy samouczek lub wpis na forum w witrynie www.asp.net ma własną unikatową zawartość, każda z tych stron również renderuje serię typowych `<div>`ych elementów, które wyświetlają linki do sekcji najwyższego poziomu: Strona główna, rozpoczęcie, uczenie i tak dalej.

Istnieją różne techniki tworzenia stron sieci Web o spójnym wyglądzie i działaniu. Podejście algorytmie to po prostu skopiowanie i wklejenie wspólnego znacznika układu do wszystkich stron sieci Web, ale takie podejście ma wiele downsides. W przypadku uruchamiania, za każdym razem, gdy tworzona jest nowa strona, należy pamiętać o skopiowaniu i wklejeniu zawartości udostępnionej na stronie. Takie operacje kopiowania i wklejania są dojrzałe dla błędu, ponieważ można przypadkowo skopiować tylko podzestaw udostępnionego znacznika do nowej strony. I aby je wycofać, Metoda ta sprawia, że zastąpi istniejący wygląd całej witryny nowym, ponieważ każda pojedyncza strona w lokacji musi być edytowana, aby można było użyć nowego wyglądu i działania.

Przed ASP.NET w wersji 2,0, deweloperzy stron często umieszczają typowe znaczniki w [kontrolkach użytkownika](https://msdn.microsoft.com/library/y6wb1a0e.aspx) , a następnie dodaliśmy te kontrolki użytkownika do każdej strony. Takie podejście wymagało, aby deweloper strony pamiętał ręcznie dodać kontrolki użytkownika do każdej nowej strony, ale jest to dozwolone w przypadku łatwiejszej modyfikacji w całej lokacji, ponieważ podczas aktualizacji wspólnego oznakowania tylko kontrolki użytkownika wymagane do zmodyfikowania. Niestety, Visual Studio .NET 2002 i 2003 — wersje programu Visual Studio służące do tworzenia aplikacji ASP.NET 1. x renderowanych kontrolek użytkownika w widok Projekt jako szare pola. W związku z tym deweloperzy stron korzystających z tego podejścia nie korzystały ze środowiska czasu projektowania w trybie WYSIWYG.

W programie ASP.NET w wersji 2,0 i Visual Studio 2005 nie ma wad *korzystania z funkcji kontroli użytkownika.* Strona wzorcowa jest specjalnym typem strony ASP.NET, który definiuje zarówno znacznik dotyczący całej witryny, jak i *regiony* , w których skojarzone *strony zawartości* definiują własne znaczniki niestandardowe. Ponieważ zobaczymy w kroku 1, te regiony są zdefiniowane przez kontrolki ContentPlaceHolder. Formant ContentPlaceHolder po prostu oznacza pozycję w hierarchii formantów strony wzorcowej, w której zawartość niestandardowa może zostać wprowadzona przez stronę zawartości.

> [!NOTE]
> Podstawowe pojęcia i funkcje stron wzorcowych nie zmieniły się od czasu ASP.NET w wersji 2,0. Jednak program Visual Studio 2008 oferuje obsługę w czasie projektowania dla zagnieżdżonych stron wzorcowych, a funkcja niewymagająca programu Visual Studio 2005. Zobaczmy, jak używać zagnieżdżonych stron wzorcowych w przyszłym samouczku.

Rysunek 2 pokazuje, co może wyglądać Strona wzorcowa dla www.asp.net. Należy pamiętać, że strona wzorcowa definiuje wspólny układ całej witryny — znacznik u góry, u dołu i prawej krawędzi każdej strony, a także element ContentPlaceHolder w lewej części, gdzie znajduje się unikatowa zawartość dla każdej pojedynczej strony sieci Web.

![Strona wzorcowa definiuje układ całej witryny i edytowalne regiony na stronie zawartości na podstawie zawartości](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**Ilustracja 02**: Strona wzorcowa definiuje układ całej witryny i edytowalne regiony na stronie zawartości na podstawie zawartości

Po zdefiniowaniu strony wzorcowej można ją powiązać z nowymi stronami ASP.NET za pomocą znacznika wyboru. Te strony ASP.NET — nazywane stronami zawartości — zawierają kontrolkę zawartości dla każdej kontrolki ContentPlaceHolder na stronie wzorcowej. Po odwiedzeniu strony zawartości za pomocą przeglądarki aparat ASP.NET tworzy hierarchię formantów strony wzorcowej i wprowadza hierarchię kontroli strony zawartości do odpowiednich miejsc. Ta hierarchia połączonych formantów jest renderowana, a wynikowy kod HTML jest zwracany do przeglądarki użytkownika końcowego. W związku z tym strona zawartości emituje wspólne znaczniki zdefiniowane na stronie wzorcowej poza kontrolkami ContentPlaceHolder i znaczniki specyficzne dla strony zdefiniowane w ramach własnych kontrolek zawartości. Rysunek 3 ilustruje tę koncepcję.

[![adiustacji żądanej strony są odrzucane na stronie wzorcowej](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**Ilustracja 03**: odmowa od strony wzorcowej adiustacji żądanej strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))

Teraz, gdy już omawiamy sposób działania stron wzorcowych, przyjrzyjmy się tworzeniu strony wzorcowej i skojarzonych stron zawartości przy użyciu programu Visual Web Developer.

> [!NOTE]
> Aby dotrzeć do szerokiego grona odbiorców, ASP.NET witrynę sieci Web utworzoną przez nas w tej serii samouczków zostanie utworzona przy użyciu ASP.NET 3,5 z bezpłatną wersją programu Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Jeśli jeszcze nie uaktualniono do ASP.NET 3,5, nie martw się — koncepcje omówione w tych samouczkach działają równie dobrze w przypadku ASP.NET 2,0 i programu Visual Studio 2005. Jednak niektóre aplikacje demonstracyjne mogą korzystać z funkcji nowych do .NET Framework wersji 3,5; w przypadku korzystania z funkcji specyficznych dla 3,5 należy dodać uwagę, w której omówiono sposób implementacji podobnych funkcji w wersji 2,0. Należy pamiętać, że aplikacje demonstracyjne dostępne do pobrania z każdego samouczka są przeznaczone dla .NET Framework w wersji 3,5, co powoduje, że plik `Web.config` zawiera elementy konfiguracji specyficzne dla 3,5 i odwołania do 3,5 określonych przestrzeni nazw w instrukcjach `using` w klasach związanych z kodem na stronach ASP.NET. Długi scenariusz krótko, jeśli na komputerze nie jest jeszcze zainstalowany program .NET 3,5, aplikacja sieci Web do pobrania nie będzie działała bez uprzedniego usunięcia oznakowania specyficznego dla 3,5 z `Web.config`. Aby uzyskać więcej informacji na temat tego tematu, zobacz artykuł dotyczący [wyASP.NET wersji 3.5 pliku `Web.config`](http://www.4guysfromrolla.com/articles/121207-1.aspx) . Należy również usunąć instrukcje `using`, które odwołują się do określonych przestrzeni nazw 3,5.

## <a name="step-1-creating-a-master-page"></a>Krok 1. Tworzenie strony wzorcowej

Zanim będziemy mogli eksplorować tworzenie i używanie stron głównych i zawartości, najpierw potrzebujemy witryny sieci Web ASP.NET. Zacznij od utworzenia nowej witryny sieci Web ASP.NET opartej na systemie plików. Aby to osiągnąć, uruchom program Visual Web Developer, a następnie przejdź do menu plik i wybierz polecenie Nowa witryna sieci Web, wyświetlając okno dialogowe Nowa witryna sieci Web (zobacz rysunek 4). Wybierz szablon witryna sieci Web ASP.NET, Ustaw listę rozwijaną lokalizacja na system plików, wybierz folder, w którym ma zostać umieszczona witryna sieci Web, i Ustaw język C#na. Spowoduje to utworzenie nowej witryny sieci Web z `Default.aspx` strony ASP.NET, `App_Data` folderze i pliku `Web.config`.

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektami: projekty witryny sieci Web i projekty aplikacji sieci Web. Projekty witryn sieci Web bez pliku projektu, natomiast projekty aplikacji sieci Web naśladuje architekturę projektu w programie Visual Studio .NET 2002/2003 — obejmują one plik projektu i kompilują kod źródłowy projektu do jednego zestawu, który znajduje się w folderze `/bin`. Program Visual Studio 2005 początkowo tylko obsługiwane projekty witryny sieci Web, mimo że [model projektu aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) został wprowadzony ponownie z dodatkiem Service Pack 1. Program Visual Studio 2008 oferuje modele projektów. Wersje Visual Web Developer 2005 i 2008, jednak obsługują tylko projekty witryn sieci Web. Używam modelu projektu witryny sieci Web dla moich pokazów w tej serii samouczków. Jeśli używasz wersji innej niż Express i chcesz zamiast tego używać modelu projektu aplikacji sieci Web, możesz to zrobić, ale pamiętaj, że mogą wystąpić pewne rozbieżności między elementami widocznymi na ekranie i czynnościami, które należy wykonać w porównaniu z wyświetlanymi i instructio zrzutami ekranu. NS udostępniane w tych samouczkach.

[![utworzyć nową witrynę sieci Web opartą na systemie plików](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**Ilustracja 04**. Tworzenie nowej witryny sieci Web opartej na systemie plików ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))

Następnie Dodaj stronę wzorcową do lokacji w katalogu głównym, klikając prawym przyciskiem myszy nazwę projektu, wybierając pozycję Dodaj nowy element i wybierając szablon strony głównej. Zwróć uwagę, że strony wzorcowe kończą się rozszerzeniem `.master`. Nadaj nazwę nowej stronie głównej `Site.master` a następnie kliknij przycisk Dodaj.

[![dodać strony wzorcowej o nazwie Site. Master do witryny sieci Web](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**Ilustracja 05**: Dodawanie strony wzorcowej o nazwie `Site.master` do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))

Dodanie nowego pliku strony głównej za pomocą programu Visual Web Developer tworzy stronę wzorcową z następującymi znakami deklaracyjne:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

Pierwszym wierszem znacznika deklaracyjnego jest [dyrektywa`@Master`](https://msdn.microsoft.com/library/ms228176.aspx). Dyrektywa `@Master` jest podobna do [dyrektywy`@Page`](https://msdn.microsoft.com/library/ydy4x04a.aspx) , która pojawia się na stronach ASP.NET. Definiuje on język po stronie serwera (C#) oraz informacje o lokalizacji i dziedziczeniu klasy powiązanej z kodem.

`DOCTYPE` i znaczniki deklaratywne strony pojawiają się poniżej dyrektywy `@Master`. Strona zawiera statyczny kod HTML wraz z czterema kontrolkami po stronie serwera:

- **Formularz sieci Web (`<form runat="server">`)** — ponieważ wszystkie strony ASP.NET mają zwykle postać sieci Web, a strona wzorcowa może zawierać kontrolki sieci Web, które muszą znajdować się w formularzu sieci Web — Pamiętaj, aby dodać formularz sieci Web do strony wzorcowej (zamiast dodawać formularz sieci Web do każdej strony zawartości).
- **Kontrolka ContentPlaceHolder o nazwie `ContentPlaceHolder1`** — ta kontrolka ContentPlaceHolder pojawia się w formularzu sieci Web i służy jako region interfejsu użytkownika strony zawartości.
- **Element `<head>` po stronie serwera** — element `<head>` ma atrybut `runat="server"`, dzięki czemu jest dostępny za pomocą kodu po stronie serwera. Element `<head>` jest implementowany w ten sposób, dzięki czemu tytuł strony i inne znaczniki powiązane `<head>`mogą być dodawane lub dostosowywane programowo. Na przykład ustawienie właściwości `Title` strony ASP.NET zmienia element `<title>` renderowany przez formant serwera `<head>`.
- **Kontrolka ContentPlaceHolder o nazwie `head`** — ta kontrolka ContentPlaceHolder pojawia się w kontrolce serwera `<head>` i może służyć do deklaratywnego dodawania zawartości do elementu `<head>`.

Ten domyślny, deklaratywny znak strony głównej służy jako punkt wyjścia do projektowania własnych stron wzorcowych. Możesz swobodnie edytować kod HTML lub dodać do strony głównej dodatkowe formanty sieci Web lub Elementy ContentPlaceHolders.

> [!NOTE]
> Podczas projektowania strony wzorcowej upewnij się, że strona wzorcowa zawiera formularz sieci Web i że w tym formularzu sieci Web jest wyświetlana co najmniej jedna kontrolka ContentPlaceHolder.

### <a name="creating-a-simple-site-layout"></a>Tworzenie prostego układu witryny

Rozwińmy domyślne znaczniki deklaratywne `Site.master`, aby utworzyć układ witryny, w którym wszystkie strony współużytkują: wspólny nagłówek; Lewa kolumna z nawigacją, wiadomościami i inną zawartością w całej lokacji; i stopka, w której jest wyświetlana ikona "obsługiwane przez Microsoft ASP.NET". Rysunek 6 pokazuje końcowy wynik strony głównej, gdy jedna z jej stron zawartości jest oglądana za pomocą przeglądarki. Region czerwony okrąg na rysunku 6 jest specyficzny dla odwiedzonej strony (`Default.aspx`); inna zawartość jest definiowana na stronie wzorcowej i dlatego jest spójna na wszystkich stronach zawartości.

[![Strona wzorcowa definiuje znaczniki dla części Top, Left i Bottom](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**Ilustracja 06**. Strona wzorcowa definiuje znaczniki dla części górnej, lewej i dolnej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))

Aby osiągnąć układ witryny przedstawiony na rysunku 6, Zacznij od zaktualizowania `Site.master` stronie wzorcowej, tak aby zawierała następujące znaczniki deklaratywne:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

Układ strony wzorcowej jest definiowany przy użyciu serii `<div>` elementów HTML. `<div>` `topContent` zawiera znaczniki, które pojawiają się w górnej części każdej strony, podczas gdy `mainContent`, `leftContent`i `footerContent`y `<div>` s są używane do wyświetlania zawartości strony, lewej kolumny i ikony "obsługiwane przez Microsoft ASP.NET". Oprócz dodawania tych elementów `<div>` zmieniono również właściwość `ID` podstawowego formantu ContentPlaceHolder z `ContentPlaceHolder1` na `MainContent`.

Reguły formatowania i układu dla tych elementów `<div>` są wpisane w pliku [kaskadowego arkusza stylów (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) `Styles.css`, który jest określany za pośrednictwem &lt;łącza&gt; elementu na stronie głównej &lt;&gt;. Te różne reguły określają wygląd i sposób działania każdego elementu `<div>` wymienione powyżej. Na przykład, `topContent` element `<div>`, który wyświetla tekst "samouczki" "i link do stron wzorcowych, ma reguły formatowania określone w `Styles.css` w następujący sposób:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

Jeśli na komputerze używasz programu, musisz pobrać towarzyszący kod tego samouczka i dodać plik `Styles.css` do projektu. Podobnie należy również utworzyć folder o nazwie obrazy i skopiować ikonę "obsługiwane przez Microsoft ASP.NET" z pobranej demonstracyjnej witryny sieci Web do projektu.

> [!NOTE]
> Omówienie formatowania stron CSS i sieci Web wykracza poza zakres tego artykułu. Aby uzyskać więcej informacji na temat CSS, zapoznaj się z [samouczkami](http://www.w3schools.com/css/default.asp) dotyczącymi CSS w witrynie [w3schools.com](http://www.w3schools.com/). Zachęcamy również do pobrania dołączonego kodu tego samouczka i odtworzenia ustawień CSS w `Styles.css`, aby zobaczyć efekty różnych reguł formatowania.

### <a name="creating-a-master-page-using-an-existing-design-template"></a>Tworzenie strony wzorcowej przy użyciu istniejącego szablonu projektu

W latach zostały skompilowane kilka ASP.NET aplikacji sieci Web dla małych i średnich firm. Niektórzy klienci mają istniejący układ lokacji, którego chciały użyć; inne osoby zatrudniają właściwego projektanta grafiki. Kilka powierzonych mi projektowania układu witryny sieci Web. W miarę jak można poznać rysunek 6, zadanie programisty do projektowania układu witryny sieci Web jest zwykle tak samo samo, jak w przypadku, gdy lekarz wykonuje Twoje podatki.

Na szczęście istnieją nieliczne witryny sieci Web, które oferują bezpłatne szablony projektu HTML — firma Google zwróciła więcej niż 6 000 000 wyników wyszukiwania "bezpłatne szablony witryn sieci Web". Jednym z moich ulubionych elementów jest [OpenDesigns.org](http://opendesigns.org/). Po znalezieniu szablonu witryny sieci Web Dodaj pliki i obrazy CSS do projektu witryny sieci Web i Zintegruj kod HTML szablonu ze stroną wzorcową.

> [!NOTE]
> Firma Microsoft oferuje również wiele [bezpłatnych szablonów zestawu startowego ASP.NET projektu](https://msdn.microsoft.com/asp.net/aa336613.aspx) , które integrują się z oknem dialogowym Nowa witryna sieci Web w programie Visual Studio.

## <a name="step-2-creating-associated-content-pages"></a>Krok 2. Tworzenie skojarzonych stron zawartości

Po utworzeniu strony głównej wszystko jest gotowe do rozpoczęcia tworzenia stron ASP.NET, które są powiązane z stroną wzorcową. Takie strony są określane jako *strony zawartości*.

Dodajmy nową stronę ASP.NET do projektu i powiążesz ją ze stroną wzorcową `Site.master`. Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz opcję Dodaj nowy element. Wybierz szablon formularza sieci Web, wprowadź nazwę `About.aspx`, a następnie zaznacz pole wyboru "Wybierz stronę wzorcową", jak pokazano na rysunku 7. Spowoduje to wyświetlenie okna dialogowego Wybierz stronę wzorcową (patrz rysunek 8), w którym można wybrać stronę wzorcową do użycia.

> [!NOTE]
> Jeśli witryna sieci Web ASP.NET została utworzona przy użyciu modelu projektu aplikacji internetowej zamiast modelu projektu witryny sieci Web, w oknie dialogowym Dodaj nowy element zostanie wyświetlona wartość pola wyboru "Wybierz stronę wzorcową" na rysunku 7. Aby utworzyć stronę zawartości przy użyciu modelu projektu aplikacji sieci Web, należy wybrać szablon formularza zawartości sieci Web zamiast szablonu formularza sieci Web. Po wybraniu szablonu formularza zawartości sieci Web i kliknięciu przycisku Dodaj zostanie wyświetlone okno dialogowe Wybieranie strony głównej wyświetlone na rysunku 8.

[![dodać nową stronę zawartości](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**Ilustracja 07**. Dodawanie nowej strony zawartości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))

[![wybierz stronę wzorcową witryny. Master](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**Ilustracja 08**: Wybierz stronę wzorcową `Site.master` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))

Jak pokazano poniżej, Nowa strona zawartości zawiera `@Page` dyrektywy, która wskazuje z powrotem na stronę wzorcową i kontrolkę zawartości dla każdej kontrolki elementu ContentPlaceHolder na stronie wzorcowej.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> W sekcji "Tworzenie prostego układu witryny" w kroku 1 zmieniono nazwę `ContentPlaceHolder1` na `MainContent`. Jeśli nie zmienisz nazwy tego `ID` formantu elementu ContentPlaceHolder w taki sam sposób, znaczniki deklaratywne ze strony zawartości różnią się nieco od znaczników pokazanych powyżej. W takim przypadku druga `ContentPlaceHolderID` kontrolki zawartości odzwierciedla `ID` odpowiadającego im formantu ContentPlaceHolder na stronie wzorcowej.

Podczas renderowania strony zawartości aparat ASP.NET musi odrzucać kontrolki zawartości strony ze swoimi formantami ContentPlaceHolder strony wzorcowej. Aparat ASP.NET określa stronę wzorcową strony zawartości na podstawie atrybutu `MasterPageFile` dyrektywy `@Page`. Jak widać powyższe oznakowanie, ta strona zawartości jest powiązana z `~/Site.master`.

Ponieważ Strona główna ma dwie kontrolki ContentPlaceHolder-`head` i `MainContent` — Visual Web Developer wygenerował dwie kontrolki zawartości. Każda kontrolka zawartości odwołuje się do określonego elementu ContentPlaceHolder za pośrednictwem jego właściwości `ContentPlaceHolderID`.

W przypadku, gdy strony wzorcowe są wyższe niż poprzednie metody szablonu w całej lokacji, są w trakcie projektowania. Rysunek 9 przedstawia stronę zawartości `About.aspx` podczas wyświetlania za pomocą widok Projekt programu Visual Web Developer. Należy pamiętać, że podczas gdy zawartość strony głównej jest widoczna, jest wyszarzona i nie można jej modyfikować. Kontrolki zawartości odpowiadające elementom ContentPlaceHolders strony wzorcowej są jednak edytowalne. Podobnie jak w przypadku dowolnej innej strony ASP.NET, można utworzyć interfejs strony zawartości, dodając formanty sieci Web za pomocą widoków źródła lub projektu.

[![widok Projekt strony zawartości wyświetla zarówno zawartość strony, jak i stronę wzorcową.](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**Ilustracja 09**: widok projektu strony zawartości wyświetla zarówno zawartość strony, jak i stronę wzorcową (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))

### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Dodawanie znaczników i kontrolek sieci Web do strony zawartości

Poświęć chwilę na utworzenie zawartości dla strony `About.aspx`. Jak widać na rysunku 10, został wprowadzony nagłówek "informacje o autorze" i kilka akapitów tekstu, ale możesz również dodać kontrolki sieci Web. Po utworzeniu tego interfejsu odwiedź stronę `About.aspx` za pomocą przeglądarki.

[![odwiedź stronę about. aspx za pomocą przeglądarki](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**Ilustracja 10**. odwiedź stronę `About.aspx` za pomocą przeglądarki ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))

Ważne jest, aby zrozumieć, że żądana strona zawartości i skojarzona z nią strona wzorcowa są odrzucane i renderowane całkowicie na serwerze sieci Web. Następnie przeglądarka użytkownika końcowego otrzymuje otrzymany, odmowa kod HTML. Aby to sprawdzić, Wyświetl kod HTML otrzymany przez przeglądarkę, przechodząc do menu Widok i wybierając pozycję źródło. Należy zauważyć, że nie ma żadnych ramek ani innych wyspecjalizowanych technik wyświetlania dwóch różnych stron sieci Web w jednym oknie.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Powiązywanie strony wzorcowej z istniejącą stroną ASP.NET

Jak pokazano w tym kroku, Dodawanie nowej strony zawartości do aplikacji sieci Web ASP.NET jest tak proste, jak zaznaczenie pola wyboru "Wybierz stronę wzorcową" i wybranie strony głównej. Niestety, Konwertowanie istniejącej strony ASP.NET na stronę wzorcową nie jest tak proste.

Aby powiązać stronę wzorcową z istniejącą stroną ASP.NET, należy wykonać następujące czynności:

1. Dodaj atrybut `MasterPageFile` do dyrektywy `@Page` strony ASP.NET, wskazując ją na odpowiednią stronę wzorcową.
2. Dodaj kontrolki zawartości dla każdego elementu ContentPlaceHolders na stronie wzorcowej.
3. Umożliwia selektywne wycinanie i wklejanie istniejącej zawartości strony ASP.NET do odpowiednich kontrolek zawartości. W tym miejscu mówimy "selektywne", ponieważ strona ASP.NET prawdopodobnie zawiera znaczniki, które są już wyrażone przez stronę wzorcową, takie jak `DOCTYPE`, element `<html>` i formularz sieci Web.

Aby uzyskać instrukcje krok po kroku dotyczące tego procesu wraz z zrzutami ekranu, zapoznaj się z tematem [Scott Guthrie](https://weblogs.asp.net/scottgu/), [korzystając ze stron wzorcowych i](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx) samouczka nawigacji po witrynie. Sekcja "Aktualizacja `Default.aspx` i `DataSample.aspx` do korzystania ze strony głównej" zawiera szczegółowe informacje o tych krokach.

Ponieważ tworzenie nowych stron zawartości nie jest znacznie łatwiejsze niż w przypadku konwertowania istniejących stron ASP.NET na strony zawartości, zaleca się, aby podczas tworzenia nowej witryny sieci Web ASP.NET dodać stronę wzorcową do lokacji. Powiąż wszystkie nowe strony ASP.NET z tą stroną wzorcową. Nie martw się, jeśli początkowa Strona główna jest bardzo prosta lub zwykła; Możesz później zaktualizować stronę wzorcową.

> [!NOTE]
> Podczas tworzenia nowej aplikacji ASP.NET, Visual Web Developer dodaje stronę `Default.aspx`, która nie jest powiązana z stroną wzorcową. Jeśli chcesz przeprowadzić konwersję istniejącej strony ASP.NET na stronę zawartości, przejdź dalej i zrób `Default.aspx`. Alternatywnie możesz usunąć `Default.aspx` a następnie dodać go ponownie, ale tym razem zaznaczając pole wyboru "Wybierz stronę wzorcową".

## <a name="step-3-updating-the-master-pages-markup"></a>Krok 3. Aktualizowanie znacznika strony wzorcowej

Jedną z głównych zalet stron wzorcowych jest to, że pojedyncza strona wzorcowa może służyć do definiowania ogólnego układu dla wielu stron w witrynie. W związku z tym aktualizowanie wyglądu i działania witryny wymaga aktualizacji pojedynczego pliku — strony głównej.

Aby zilustrować to zachowanie, zaktualizuj stronę wzorcową w celu uwzględnienia bieżącej daty w górnej części lewej kolumny. Dodaj etykietę o nazwie `DateDisplay` do `<div>``leftContent`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń `Page_Load` dla strony wzorcowej i Dodaj następujący kod:

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

Powyższy kod ustawia właściwość `Text` etykiety na bieżącą datę i godzinę sformatowaną jako dzień tygodnia, nazwę miesiąca i dzień dwucyfrowy (patrz rysunek 11). W przypadku tej zmiany należy ponownie odwiedzić jedną ze stron zawartości. Jak pokazano na rysunku 11, powstające znaczniki są natychmiast aktualizowane w celu uwzględnienia zmiany na stronie wzorcowej.

[![zmiany na stronie wzorcowej są uwzględniane podczas wyświetlania strony zawartości](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**Ilustracja 11**. zmiany na stronie wzorcowej są odzwierciedlane podczas wyświetlania strony zawartości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))

> [!NOTE]
> Jak pokazano na przykładzie, strony wzorcowe mogą zawierać kontrolki sieci Web po stronie serwera, kod i programy obsługi zdarzeń.

## <a name="summary"></a>Podsumowanie

Strony wzorcowe umożliwiają deweloperom ASP.NET projektowanie spójnego układu dla całej witryny, który można łatwo aktualizować. Tworzenie stron wzorcowych i skojarzonych z nimi stron zawartości jest tak proste jak tworzenie standardowych stron ASP.NET, ponieważ Visual Web Developer oferuje zaawansowane wsparcie w czasie projektowania.

Przykładowa strona wzorcowa utworzona w tym samouczku ma dwie kontrolki ContentPlaceHolder, `head` i `MainContent`. Należy jednak tylko określić dla formantu `MainContent` ContentPlaceHolder na naszej stronie zawartości. W następnym samouczku Przyjrzyjmy się użyciu wielu kontrolek zawartości na stronie zawartość. Zobaczmy również, jak definiować domyślne znaczniki dla formantów zawartości na stronie wzorcowej, a także jak przełączać się między użyciem znaczników domyślnych zdefiniowanych na stronie wzorcowej i udostępnianiu znaczników niestandardowych na stronie zawartości.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [ASP.NET dla projektantów: bezpłatne szablony projektu i wskazówki dotyczące tworzenia witryn sieci Web ASP.NET przy użyciu standardów internetowych](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [ASP.NET strony wzorcowe — Omówienie](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Kaskadowe samouczki arkuszy stylów (CSS)](http://www.w3schools.com/css/default.asp)
- [Dynamiczne ustawianie tytułu strony](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Strony wzorcowe w ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Samouczki szybkiego startu stron wzorcowych](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Next](multiple-contentplaceholders-and-default-content-cs.md)

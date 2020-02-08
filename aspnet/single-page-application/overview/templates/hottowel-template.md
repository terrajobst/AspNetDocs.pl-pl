---
uid: single-page-application/overview/templates/hottowel-template
title: Szablon gorąca ręczników | Microsoft Docs
author: madskristensen
description: Szablon HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075063"
---
# <a name="hot-towel-template"></a>Szablon Hot Towel

Autor [produktywność Madsa Kristensena](https://github.com/madskristensen)

> Szablon Hot ręczników MVC jest zapisywana przez Jan Papa
> 
> Wybierz wersję do pobrania:
> 
> [Szablon ręczników MVC dla programu Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Szablon Hot ręczników MVC dla Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Gorąca ręczników: ponieważ nie chcesz przejść do SPA bez użycia

Chcesz skompilować SPA, ale nie możesz zdecydować, gdzie zacząć? Używaj gorącej ręczników i w ciągu kilku sekund będziesz mieć SPA i wszystkie narzędzia potrzebne do kompilowania na nim!

Gorąca ręczników tworzy doskonały punkt wyjścia do tworzenia aplikacji jednostronicowej (SPA) z ASP.NET. Poza ramką udostępniamy modularną strukturę kodu, nawigację w widoku, powiązanie danych, zaawansowane zarządzanie danymi oraz proste, ale elegancki styl. Gorąca ręczników zapewnia wszystko, co jest potrzebne do utworzenia SPA, dzięki czemu możesz skupić się na aplikacji, a nie na instalacji wodociągowej.

> Dowiedz się więcej na temat tworzenia SPA z [filmów wideo, samouczków i kursów Pluralsight firmy John Papa](http://johnpapa.net/spa?vsix).

## <a name="application-structure"></a>Struktura aplikacji

Hot ręczników SPA udostępnia folder aplikacji, który zawiera pliki JavaScript i HTML, które definiują aplikację.

W folderze aplikacji:

- durandal
- services
- modele widoków
- widoki

Folder App zawiera kolekcję modułów. Te moduły hermetyzują funkcjonalność i deklarują zależności w innych modułach. Folder widoki zawiera kod HTML dla aplikacji, a folder modele widoków zawiera logikę prezentacji dla widoków (wspólny wzorzec MVVM). Folder usługi doskonale nadaje się do przechowywania wszelkich typowych usług, których może potrzebować aplikacja, takich jak pobieranie danych HTTP lub interakcja z magazynem lokalnym. Często wiele modele widoków umożliwia ponowne użycie kodu z modułów usług.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struktura aplikacji po stronie serwera ASP.NET MVC

Gorąca ręczników jest oparta na znanej i zaawansowanej strukturze ASP.NET MVC.

- Rozpoczęcie\_aplikacji
- Zawartość
- Kontrolery
- Modele
- Scripts
- Widoki

## <a name="featured-libraries"></a>Polecane biblioteki

- ASP.NET MVC
- Internetowy interfejs API platformy ASP.NET
- ASP.NET optymalizacja sieci Web — dzielenie i minifikacja
- [Breeze. js](http://Breezejs.com) — zaawansowane zarządzanie danymi
- [Durandal. js](http://Durandaljs.com) — tworzenie nawigacji i wyświetlania
- [Odcinanie. js](http://Knockoutjs.com) — powiązania danych
- [Wymagaj. js](http://requirejs.org) — modularność z AMD i optymalizacją
- Wyskakujące okienko [. js](http://jpapa.me/c7toastr) — komunikaty podręczne
- Zaawansowane ustawianie stylów CSS w usłudze [Twitter](https://twitter.github.com/bootstrap/)

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalowanie za pomocą szablonu programu Visual Studio 2012 gorąca ręczników SPA

Gorącą ręczników można zainstalować jako szablon programu Visual Studio 2012. Po prostu kliknij pozycję `File` | `New Project` i wybierz pozycję `ASP.NET MVC 4 Web Application`. Następnie wybierz szablon "aplikacja jednostronicowa na gorąco ręczników" i uruchom polecenie.

## <a name="installing-via-the-nuget-package"></a>Instalowanie za pośrednictwem pakietu NuGet

Gorąca ręczników jest również pakietem NuGet, który rozszerza istniejący pusty projekt ASP.NET MVC. Po prostu zainstaluj program NuGet, a następnie uruchom polecenie.

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Jak kompilować na gorącą ręczników?

Po prostu zacznij dodawać kod!

1. Dodaj swój własny kod po stronie serwera, najlepiej Entity Framework i WebAPI (który naprawdę ma Breeze. js)
2. Dodawanie widoków do folderu `App/views`
3. Dodawanie modele widoków do folderu `App/viewmodels`
4. Dodawanie powiązań HTML i odcinania danych do nowych widoków
5. Aktualizowanie tras nawigacji w `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Przewodnik po kodzie HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index. cshtml to początkowa trasa i widok dla aplikacji MVC. Zawiera wszystkie standardowe Tagi meta, linki CSS i odwołania do języka JavaScript, których oczekujesz. Treść zawiera pojedynczy `<div>`, w którym zostanie umieszczona cała zawartość (widoki), gdy zostanie ona zażądana. `@Scripts.Render` używa programu Wymagaj. js do uruchomienia punktu wejścia dla kodu aplikacji, który znajduje się w pliku `main.js`. Zostanie dostarczony ekran powitalny pokazujący, jak utworzyć ekran powitalny podczas ładowania aplikacji.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

Plik `main.js` zawiera kod, który zostanie uruchomiony zaraz po załadowaniu aplikacji. Jest to miejsce, w którym chcesz zdefiniować trasy nawigacji, ustawić widoki uruchamiania i wykonać dowolne instalacji/uruchomienia, takie jak napełnianiu danych aplikacji.

Plik `main.js` definiuje kilka modułów Durandal, aby ułatwić rozpoczęcie uruchamiania aplikacji. Instrukcja define pomaga rozpoznać zależności modułów, aby były dostępne dla funkcji. Najpierw są włączone komunikaty debugowania, które wysyłają komunikaty o zdarzeniach wykonywanych przez aplikację w oknie konsoli. Kod App. Start mówi Durandal Framework, aby uruchomić aplikację. Konwencje są ustawiane tak, aby Durandal wie, że wszystkie widoki i modele widoków są zawarte w tych samych nazwanych folderach, odpowiednio. Na koniec `app.setRoot` rozpoczyna ładowanie `shell` przy użyciu wstępnie zdefiniowanej animacji `entrance`.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Widoki

Widoki znajdują się w folderze `App/views`.

### <a name="shellhtml"></a>shell.html

`shell.html` zawiera układ wzorca dla kodu HTML. Wszystkie inne widoki będą składać się gdzieś na stronie widoku `shell`. Gorąca ręczników zapewnia `shell` z trzema takimi regionami: nagłówkiem, obszarem zawartości i stopką. Każdy z tych regionów jest ładowany wraz z zawartością w innych widokach, gdy jest to wymagane.

`compose` powiązania dla nagłówka i stopki są twarde kodowane w gorącą ręczników, aby wskazywały odpowiednio `nav` i `footer` widoków. Powiązanie redagowania dla sekcji `#content` jest powiązane z aktywnym elementem modułu `router`. Innymi słowy po kliknięciu linku nawigacyjnego w tym obszarze jest ładowany odpowiedni widok.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

`nav.html` zawiera linki nawigacji dla SPA. Jest to miejsce, w którym można na przykład umieścić strukturę menu. Często jest to powiązane z danymi (za pomocą odcinania) do modułu `router`, aby wyświetlić nawigację zdefiniowane w `shell.js`. Odcinanie szuka atrybutów powiązania danych i wiąże je z `shell` ViewModel, aby wyświetlić trasy nawigacji i wyświetlić element ProgressBar (przy użyciu Bootstrap usługi Twitter), jeśli moduł `router` jest w trakcie przechodzenia z jednego widoku do innego (zobacz `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home. html i details. html

Te widoki zawierają kod HTML dla widoków niestandardowych. Po kliknięciu linku `home` w menu Widok `nav` zostanie umieszczony widok `home` w obszarze zawartość widoku `shell`. Te widoki można rozszerzyć lub zastąpić własnymi widokami niestandardowymi.

### <a name="footerhtml"></a>footer.html

`footer.html` zawiera kod HTML, który pojawia się w stopce w dolnej części widoku `shell`.

## <a name="viewmodels"></a>Modele widoków

Modele widoków znajdują się w folderze `App/viewmodels`.

### <a name="shelljs"></a>shell.js

ViewModel `shell` zawiera właściwości i funkcje, które są powiązane z widokiem `shell`. Często jest to miejsce, w którym znajdują się powiązania nawigacji menu (patrz logika `router.mapNav`).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home. js i details. js

Te modele widoków zawierają właściwości i funkcje, które są powiązane z widokiem `home`. zawiera również logikę prezentacji dla widoku i jest przyklejana między danymi i widokiem.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Usługi

Usługi są dostępne w folderze aplikacji/usług. W idealnym przypadku można umieścić w przyszłości usługi, takie jak moduł DataService, odpowiedzialne za pobieranie i publikowanie danych zdalnych.

### <a name="loggerjs"></a>logger.js

Gorąca ręczników udostępnia moduł `logger` w folderze Services. Moduł `logger` jest idealnym rozwiązaniem w przypadku rejestrowania komunikatów w konsoli programu i dla użytkownika w oknie wyskakujących okienek.

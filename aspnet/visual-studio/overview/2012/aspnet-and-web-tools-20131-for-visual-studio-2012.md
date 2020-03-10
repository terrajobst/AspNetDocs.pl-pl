---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Informacje o wersji dla ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012 | Microsoft Docs
author: microsoft
description: W tym dokumencie opisano wydanie ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578443"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Informacje o wersji rozszerzenia ASP.NET and Web Tools 2013.1 dla programu Visual Studio 2012

przez [firmę Microsoft](https://github.com/microsoft)

> W tym dokumencie opisano wydanie ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.

## <a name="contents"></a>Spis treści

- [Uwagi dotyczące instalacji](#install)
- [Wymagania dotyczące oprogramowania](#requirements)
- Nowe funkcje w ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Szablony](#templates)

        - [Szablon ASP.NET MVC 5](#mvc5template)
        - [Szablon ASP.NET Web API 2](#apitemplate)
        - [Szablony elementów](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Tworzenie szkieletu ASP.NET](#scaffold)
    - [Edytor Razor](#razor)
    - [NuGet 2.7](#nuget)
- Znane problemy i istotne zmiany

    - [Tworzenie szkieletu ASP.NET](#issuescaffolding)

        - [Tworzenie szkieletu kodu MVC i interfejsu API sieci Web-HTTP 404, nie znaleziono błędu](#404issue)
        - [Visual Studio Express 2012 dla sieci Web przestaje działać po dodaniu szkieletu elementu](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Wyświetlanie pliku cshtml z przeglądaniem lub F5 powoduje błąd serwera](#browseissue)
        - [Ponowne zapisywanie adresów URL i Tylda (~)](#rewriteissue)
    - [Szablony](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

[Zainstaluj](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) program ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Wymagania programowe

Musisz mieć program Visual Studio 2012 lub Visual Studio Express 2012 dla sieci Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nowe funkcje w ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

W przypadku tworzenia szkieletów kontrolerów i widoków MVC 5 znaczniki dla widoków używają [ładowania początkowego](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Szablony

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Szablon ASP.NET MVC 5

Dodaliśmy nowy szablon MVC 5. Odwołuje się do najnowszych pakietów NuGet MVC 5 i można użyć szkieletu do dodawania kontrolerów i widoków.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Szablon ASP.NET Web API 2

Dodaliśmy nowy szablon interfejsu Web API 2. Odwołuje się do najnowszych pakietów NuGet interfejsu Web API 2 i można użyć szkieletu do dodawania kontrolerów i widoków.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Szablony elementów

Dodaliśmy nowe szablony elementów dla widoków MVC 5, Web Pages (Razor 3) i Web API 2 controllers. Instalują one powiązane pakiety NuGet do projektu podczas dodawania nowych elementów.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

W przypadku tworzenia szkieletu kontrolera MVC lub interfejsu API sieci Web przy użyciu Entity Framework używany jest program Framework 6. Aby uzyskać więcej informacji na temat Entity Framework, zobacz [Entity Framework historię wersji](https://msdn.com/data/jj574253).

Możesz również pobrać i zainstalować Entity Framework 6 Tools for Visual Studio 2012. Zobacz [Entity Framework pobierania](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Tworzenie szkieletu ASP.NET

Tworzenie szkieletów ASP.NET jest strukturą generowania kodu dla aplikacji sieci Web ASP.NET. Ułatwia to dodawanie kodu standardowego do projektu, który współdziała z modelem danych.

W poprzednich wersjach programu Visual Studio funkcja tworzenia szkieletów została ograniczona do projektów ASP.NET MVC. Dzięki tej aktualizacji można teraz używać szkieletów dla dowolnego projektu ASP.NET, w tym formularzy sieci Web. Ta aktualizacja nie obsługuje generowania stron dla projektu formularzy sieci Web, ale nadal można używać szkieletów z formularzami sieci Web, dodając zależności MVC do projektu. Obsługa generowania stron dla formularzy sieci Web zostanie dodana w przyszłej aktualizacji.

W przypadku korzystania z szkieletów upewnij się, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład, jeśli zaczniesz od projektu ASP.NET Web Forms, a następnie użyjesz szkieletu do dodania kontrolera interfejsu API sieci Web, wymagane pakiety i odwołania NuGet są dodawane do projektu automatycznie.

Aby dodać szkielet MVC do projektu formularzy sieci Web, Dodaj **nowy element szkieletowy** i wybierz **zależności MVC 5** w oknie dialogowym. Dostępne są dwie opcje tworzenia szkieletów MVC; Minimalny i pełny. W przypadku wybrania opcji minimalny tylko pakiety NuGet i odwołania dla ASP.NET MVC zostaną dodane do projektu. W przypadku wybrania opcji Pełna są dodawane minimalne zależności, a także wymagane pliki zawartości dla projektu MVC.

Obsługa tworzenia szkieletów kontrolerów asynchronicznych używa nowych funkcji asynchronicznych programu Entity Framework 6.

Aby uzyskać więcej informacji i samouczków, zobacz [Omówienie tworzenia szkieletów ASP.NET](../2013/aspnet-scaffolding-overview.md). Te samouczki przedstawiają tworzenie szkieletów Visual Studio 2013, ale mają również zastosowanie do ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Edytor Razor

Dzięki tej aktualizacji program Visual Studio 2012 obsługuje teraz narzędzia/edycję Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

Pakiet NuGet 2,7 zawiera rozbudowany zestaw nowych funkcji, które są szczegółowo opisane w [informacjach o wersji programu nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Ta wersja programu NuGet eliminuje konieczność jawnego zezwalania użytkownikom na Przywracanie brakujących pakietów przez pakiet NuGet. Podczas instalowania programu NuGet 2,7 Użytkownicy niejawnie wyrażają zgodę na automatyczne przywracanie brakujących pakietów. Użytkownicy mogą jawnie zrezygnować z przywracania pakietu za pomocą ustawień NuGet w programie Visual Studio. Ta zmiana upraszcza działanie przywracania pakietu.

## <a name="known-issues-and-breaking-changes"></a>Znane problemy i istotne zmiany

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Tworzenie szkieletu ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Tworzenie szkieletu kodu MVC i interfejsu API sieci Web-HTTP 404, nie znaleziono błędu

Jeśli wystąpi błąd podczas dodawania elementu szkieletowego do projektu, istnieje możliwość, że projekt zostanie pozostawiony w stanie niespójnym. Niektóre zmiany zostaną wycofane, ale inne zmiany, takie jak zainstalowane pakiety NuGet, nie zostaną wycofane. Jeśli zmiany konfiguracji routingu są wycofywane, użytkownicy otrzymają błąd HTTP 404 podczas przechodzenia do elementów szkieletowych.

Aby naprawić ten błąd dla składnika MVC, Dodaj nowy element szkieletowy i wybierz zależności MVC 5 (minimalnie lub pełne). Ten proces spowoduje dodanie wszystkich wymaganych zmian do projektu.

Aby naprawić ten błąd dla interfejsu API sieci Web:

1. Dodaj następującą klasę WebApiConfig do projektu.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Skonfiguruj WebApiConfig. Register w aplikacji\_Start w programie Global. asax w następujący sposób:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 dla sieci Web przestaje działać po dodaniu szkieletu elementu

Jeśli Visual Studio Express 2012 dla sieci Web przestaje działać po dodaniu szkieletu elementu z Entity Framework (takich jak kontroler Web API 2 z akcjami, przy użyciu Entity Framework), istnieje możliwość, że Visual Studio Express nie załadowała obrazu natywnego zestawu zależne od system. Web. Extensions.

Aby rozwiązać ten problem, skonfiguruj Visual Studio Express do pracy z obrazem MSIL systemu system. Web. Extensions:

1. Otwórz wiersz polecenia w trybie administratora.
2. Przejdź do pozycji%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE lub% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (w przypadku 64 bitowego systemu Windows).
3. Otwórz plik VWDExpress. exe. config w edytorze tekstów.
4. Dodaj następujący wiersz w obszarze &lt;konfiguracji&gt;/&lt;środowisko uruchomieniowe&gt;:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Uruchom ponownie Visual Studio Express 2012 dla sieci Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>Wyświetlanie pliku cshtml z przeglądaniem lub F5 powoduje błąd serwera

Podczas tworzenia projektu MVC 5 w programie Visual Studio 2012 (lub Otwórz w programie Visual Studio 2012 projekt MVC 5, który został utworzony w Visual Studio 2013) i próba wyświetlenia pliku cshtml przy użyciu polecenia Przeglądaj z lub F5, zostanie wyświetlony komunikat o błędzie z informacją o **błędzie serwera w aplikacji "/"** . Serwer próbuje przejść do `http://localhost:XXXX/Views/../XXXX.cshtml`

Aby rozwiązać ten problem, Zmień ustawienie **akcji Rozpocznij** w projekcie na **określoną stronę**. Nie musisz podawać wartości dla strony.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Po wprowadzeniu tej zmiany wybranie opcji F5 spowoduje przejście do katalogu głównego aplikacji (`http://localhost:XXXX`). Takie zachowanie nie jest takie samo jak zachowanie dla projektów MVC 5 w Visual Studio 2013, gdzie bieżące ustawienie **strony** powoduje uruchomienie otwartej strony.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Ponowne zapisywanie adresów URL i Tylda (~)

Po uaktualnieniu do ASP.NET Razor 3 lub ASP.NET MVC 5, notacja tyldy (~) może przestać działać poprawnie, jeśli używasz funkcji ponownego zapisywania adresów URL. Ponowny zapis URL ma wpływ na notację tyldy (~) w elementach HTML, takich jak &lt;A/&gt;, &lt;skrypt/&gt;, &lt;LINK/&gt;, i w związku z tym tylda nie jest już mapowana do katalogu głównego.

Na przykład w przypadku ponownego zapisu żądania dla **ASP.net/content** do **ASP.NET**, atrybut href w &lt;a href = "~/Content/"/&gt; jest rozpoznawany jako **/Content/Content/** zamiast **/** . Aby pominąć tę zmianę, można ustawić kontekst **WasUrlRewritten usługi IIS\_** na wartość false na każdej stronie sieci Web lub w **aplikacji\_BeginRequest** w Global. asax.

<a id="templateissue"></a>
### <a name="templates"></a>Szablony

Podczas tworzenia projektów ASP.NET MVC przy użyciu programu Visual Studio 2012 w systemie Windows 8.1 lub Windows Server 2012 R2 program Visual Studio wyświetli komunikat o błędzie z informacją o tym, że "Konfigurowanie sieci Web [URL] dla ASP.NET 4,5 nie powiodło się".

![Błąd konfiguracji](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Ten błąd jest wyświetlany, ponieważ program Visual Studio 2012 nie włącza funkcji ASP.NET 4,5, gdy jest ona zainstalowana w tych wersjach systemu Windows. Aby włączyć ASP.NET 4,5, wykonaj kroki opisane w sekcji [Włączanie lub wyłączanie funkcji systemu Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Włącz lub wyłącz funkcje systemu Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternatywnie można włączyć ASP.NET 4,5 za pomocą wiersza polecenia.

1. Otwórz wiersz polecenia w trybie administratora.
2. Uruchom następujące polecenie, aby włączyć ASP.NET 4,5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`

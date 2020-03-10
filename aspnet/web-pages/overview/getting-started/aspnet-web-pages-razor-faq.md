---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET strony sieci Web (Razor) — często zadawane pytania | Microsoft Docs
author: Rick-Anderson
description: W tym artykule przedstawiono kilka często zadawanych pytań dotyczących stron sieci Web ASP.NET (Razor) i WebMatrix. Wersje oprogramowania używane w samouczku ASP.NET strony sieci Web (R...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 6fa1b668e915af856a693e79eafb7180ed504dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640932"
---
# <a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) — często zadawane pytania

Autor [FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > Element WebMatrix nie jest już zalecany jako zintegrowane środowisko programistyczne dla stron sieci Web ASP.NET. Użyj [programu Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) lub [Visual Studio Code](https://code.visualstudio.com/).
>
> W tym artykule przedstawiono kilka często zadawanych pytań dotyczących stron sieci Web ASP.NET (Razor) i WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
> - Program Visual Studio 2013
> - WebMatrix 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2, WebMatrix 2 i Visual Studio 2012.

- [Jaka jest różnica między stronami sieci Web ASP.NET, ASP.NET Web Forms i ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Czy potrzebuję WebMatrix do pracy z stronami sieci Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Czy można używać kontrolek Web Forms ASP.NET na stronie stron sieci Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Czy mogę wdrożyć witrynę ASP.NET Web Pages bez używania WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Czy muszę używać pomocnika WebSecurity do obsługi logowań?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Czy strony sieci Web ASP.NET obsługują HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Czy mogę używać języków JavaScript i jQuery ze stronami sieci Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Dodatkowe zasoby](#AdditionalResources)

Pytania dotyczące błędów i innych problemów można znaleźć w [przewodniku rozwiązywania problemów z ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Jaka jest różnica między stronami sieci Web ASP.NET, ASP.NET Web Forms i ASP.NET MVC?

Wszystkie trzy są technologiami ASP.NET do tworzenia dynamicznych aplikacji sieci Web:

- ASP.NET strony sieci Web koncentrują się na dodawaniu dynamicznego (po stronie serwera) kodu i dostępu do bazy danych do stron HTML oraz funkcji prostych i uproszczonych składni.
- Formularze sieci Web ASP.NET opierają się na modelu obiektów strony i tradycyjnych kontrolkach typu Window (przyciski, listy itp.). Formularze sieci Web używają modelu opartego na zdarzeniach, który jest znany dla osób, które pracowały nad programowaniem opartym na kliencie (Windows Forms).
- ASP.NET MVC implementuje wzorzec Model-View-Controller dla ASP.NET. Nacisk dotyczy "oddzielenia obaw" (przetwarzanie, dane i warstwy interfejsu użytkownika).

Wszystkie trzy platformy są w pełni obsługiwane i nadal są opracowywane przez zespół ASP.NET. Ogólnie rzecz biorąc, wybór platformy, która ma być używana, zależy od tła i środowiska pracy z ASP.NET.

W szczególności strony sieci Web ASP.NET zostały zaprojektowane tak, aby ułatwić osobom, które już znają język HTML, Dodawanie serwera do swoich stron. Jest to dobry wybór dla studentów, hobby i osób, które są ogólnie nowością do programowania. Może to być również dobry wybór dla deweloperów, którzy korzystają z technologii sieci Web non-ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Czy potrzebuję WebMatrix do pracy z stronami sieci Web?

Nie. Element WebMatrix nie jest już zalecany jako zintegrowane środowisko programistyczne dla stron sieci Web ASP.NET. Użyj [programu Visual Studio](program-asp-net-web-pages-in-visual-studio.md) lub [Visual Studio Code](https://code.visualstudio.com/).

Jeśli nie chcesz używać programu Visual Studio ani Visual Studio Code, możesz zainstalować produkty składników osobno przy użyciu [Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx). Potrzebne są następujące produkty:

- Microsoft .NET Framework 4,5
- ASP.NET MVC 5 (który instaluje również strukturę Web Pages ASP.NET)
- IIS Express (serwer sieci Web)
- Microsoft SQL Server Compact 4,0 (baza danych)

Za pomocą edytora tekstów można edytować strony *. cshtml* (lub *. vbhtml*).

Zarządzanie bazami danych SQL Server Compact (pliki*SDF* ) bez narzędzia nie jest nieco trudniejsze. Narzędzia Visual Studio containds Tools dla zarządzania bazami danych *. sdf* . Możesz również uruchomić polecenia SQL w kodzie, aby wykonać wiele zadań zarządzania SQL Server.

Aby przetestować strony *. cshtml* bez korzystania ze zintegrowanego środowiska programistycznego (IDE), można wdrożyć je na serwerze działającym na żywo. (Zobacz [, czy mogę wdrożyć witrynę ASP.NET Web Pages bez używania WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Uruchamianie IIS Express bez użycia środowiska IDE

W przypadku instalowania IIS Express na komputerze jako serwer sieci Web, można użyć go do przetestowania stron. IIS Express można uruchomić z wiersza polecenia i skojarzyć go z określonym numerem portu. Następnie należy określić ten port, gdy zażądasz plików *. cshtml* w przeglądarce.

W systemie Windows otwórz wiersz polecenia z uprawnieniami administratora i przejdź do *lokalizacji C:\Program Files\IIS Express.* (W przypadku systemów 64-bitowych należy użyć folderu *C:\Program Files (x86) \IIS Express).* Następnie wprowadź następujące polecenie, używając ścieżki rzeczywistej do witryny:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Można użyć dowolnego numeru portu, który nie został jeszcze zarezerwowany przez inny proces. (Numery portów powyżej 1024 są zwykle bezpłatne). Dla wartości `path` użyj ścieżki folderu witryny sieci Web, w którym znajdują się pliki *. cshtml* .

Po uruchomieniu tego polecenia, aby skonfigurować IIS Express do obsłużenia stron, możesz otworzyć przeglądarkę i przejść do pliku *. cshtml* . Użyj adresu URL, takiego jak:

`http://localhost:35896/default.cshtml`

Aby uzyskać pomoc dotyczącą opcji wiersza polecenia IIS Express, wprowadź `iisexpress.exe /?` w wierszu polecenia.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Czy można używać kontrolek Web Forms ASP.NET na stronie stron sieci Web?

Nie. Formanty formularzy sieci Web, takie jak kontrolka [CheckBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) , [kontrolki walidacji](https://msdn.microsoft.com/library/bwd43d0x)i formant [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) działają tylko na stronach formularzy sieci Web (pliki *. aspx* ). Formanty te wymagają struktury strony formularzy sieci Web.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Czy mogę wdrożyć witrynę ASP.NET Web Pages bez używania WebMatrix?

Tak. Pliki witryny sieci Web można skopiować ręcznie na serwer (zazwyczaj przy użyciu protokołu FTP). W przypadku wykonywania kopii ręcznej należy również skopiować pliki obsługujące SQL Server Compact (bazę danych). Aby uzyskać szczegółowe informacje, zobacz wpis w blogu [wdrażanie aplikacji sieci Web bez narzędzia](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Czy muszę używać pomocnika WebSecurity do obsługi logowań?

Nie. Dostawca `SimpleMembership`, który jest częścią stron sieci Web ASP.NET to jedna z opcji. Dostępne są również dostawcy zabezpieczeń, którzy są częścią ASP.NET (które mogą być używane do pracy z programem w formularzach sieci Web). Na przykład można użyć uwierzytelniania formularzy na stronach sieci Web ASP.NET tak samo jak w przypadku formularzy sieci Web. Aby zapoznać się z przykładem użycia uwierzytelniania formularzy, zapoznaj się z artykułem pomoc techniczna firmy Microsoft [jak zaimplementować uwierzytelnianie oparte na formularzach w aplikacji ASP.NET C#przy użyciu platformy .NET](https://support.microsoft.com/kb/301240). Aby pobrać prosty przykład, zobacz [ASP.NET Version of "Login &amp; Password"](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Aby uzyskać informacje o sposobach korzystania z uwierzytelniania systemu Windows, zobacz wpis w blogu [przy użyciu uwierzytelniania systemu Windows na stronach sieci Web ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Czy strony sieci Web ASP.NET obsługują HTML5?

Tak. Strony tworzone za pomocą stron sieci Web ASP.NET (strony *. cshtml* lub *. vbhtml* ) są zasadniczo stronami HTML, które również zawierają kod, który jest uruchamiany na serwerze przed renderowaniem strony. O ile przeglądarka użytkownika obsługuje HTML5, można użyć elementów HTML5 na stronie *. cshtml* lub *. vbhtml* .

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Czy mogę używać języków JavaScript i jQuery ze stronami sieci Web?

Naturalnie. Strony tworzone za pomocą stron sieci Web ASP.NET (strony *. cshtml* lub *. vbhtml* ) to po prostu strony HTML z kodem serwerowym. W związku z tym wszystkie czynności, które można wykonać w normalnej stronie HTML przy użyciu języka JavaScript lub jQuery, można także wykonać na stronie *. cshtml* lub *. vbhtml* .

Szablon **Witryna początkowa** w programie WebMatrix zawiera wiele bibliotek jQuery. Jeśli tworzysz lokację przy użyciu tego szablonu, folder *skryptów* zawiera bibliotekę jQuery Core (*jQuery-1.6.2. js)* i biblioteki dla walidacji jQuery (*jQuery. Validate. js*itp.).

Poniżej przedstawiono niektóre wpisy w blogu ilustrujące sposoby korzystania z platformy jQuery ze stronami sieci Web ASP.NET:

- [Dodawanie dobrych technologii jQuery do ASP.NET stron sieci Web za pomocą programu WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) przez Rachel Appel
- [5 min: WebMatrix + jQuery UI + JSON + jQuery szablony](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) według Jonas Eriksson
- [Formularze WebMatrix i jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) według Jan solance

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix i ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix) w witrynie ASP.NET

---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Tworzenie projektów sieci Web ASP.NET w Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: W tym temacie opisano opcje tworzenia projektów sieci Web ASP.NET w Visual Studio 2013 z aktualizacją Update 3 poniżej przedstawiono niektóre z nowych funkcji tworzenia aplikacji sieci Web w języku c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519274"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Tworzenie projektów internetowych ASP.NET w programie Visual Studio 2013

Autor [Dykstra](https://github.com/tdykstra)

> W tym temacie opisano opcje tworzenia projektów sieci Web ASP.NET w programie Visual Studio 2013 z aktualizacją Update 3
> 
> Oto niektóre z nowych funkcji tworzenia aplikacji sieci Web w porównaniu z wcześniejszymi wersjami programu Visual Studio:
> 
> - Prosty interfejs użytkownika do tworzenia projektów oferujących [obsługę wielu platform ASP.NET](#add) (formularzy sieci Web, MVC i Web API).
> - [ASP.NET Identity](#indauth)nowy system członkostwa ASP.NET, który działa tak samo we wszystkich strukturach ASP.NET i współpracuje z oprogramowaniem hostingu w sieci Web innym niż IIS.
> - Używanie [ładowania początkowego](#bootstrap) w celu zapewnienia możliwości projektowania i tworzenia obrazów.
> - Nowe funkcje dla formularzy sieci Web, które były oferowane tylko przez MVC, takie jak [Automatyczne tworzenie projektów testowych](#testproj) i [szablon witryny intranetowej](#winauth).
> 
> Aby uzyskać informacje na temat sposobu tworzenia projektów sieci Web dla usługi Azure Cloud Services lub Azure Mobile Services, zobacz [Rozpoczynanie pracy z usługą azure Cloud Services i ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) oraz [Tworzenie aplikacji rankingów przy użyciu zaplecza platformy Azure Mobile Services .NET](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Ten artykuł ma zastosowanie do [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) z zainstalowaną [aktualizacją Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) .

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projekty aplikacji sieci Web i projekty witryn sieci Web

ASP.NET umożliwia wybór między dwoma rodzajami projektów sieci Web: *projektami aplikacji sieci Web* i *projektami witryny sieci Web*. Zalecamy projekty aplikacji sieci Web na potrzeby nowego programowania, a ten artykuł dotyczy tylko projektów aplikacji sieci Web. Aby uzyskać więcej informacji, zobacz [projekty aplikacji sieci Web i projekty witryn sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) w witrynie MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Przegląd tworzenia projektu aplikacji sieci Web

Poniższe kroki pokazują, jak utworzyć projekt sieci Web:

1. Na stronie **startowej** lub w menu **plik** kliknij pozycję **Nowy projekt** .
2. W oknie dialogowym **Nowy projekt** kliknij pozycję **Sieć Web** w lewym okienku i **aplikację sieci Web ASP.NET** w środkowym okienku.

    ![Okno dialogowe nowego projektu](creating-web-projects-in-visual-studio/_static/image1.png)

    W okienku po lewej stronie możesz wybrać **chmurę** , aby utworzyć [usługę w chmurze platformy Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [usługę mobilną Azure](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)lub [zadanie WebJob platformy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Ten temat nie obejmuje tych szablonów.
3. W prawym okienku kliknij pole wyboru **dodaj Application Insights do projektu** , jeśli chcesz monitorować kondycję i użycie aplikacji. Aby uzyskać więcej informacji, zobacz [monitorowanie wydajności w aplikacjach sieci Web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Określ **nazwę**projektu, **lokalizację**i inne opcje, a następnie kliknij przycisk **OK**.

    Zostanie wyświetlone okno dialogowe **Nowy projekt ASP.NET** .

    ![Okno dialogowe nowego projektu](creating-web-projects-in-visual-studio/_static/image2.png)
5. Kliknij szablon.

    ![Wybierz szablon](creating-web-projects-in-visual-studio/_static/image3.png)
6. Jeśli chcesz dodać obsługę dodatkowych platform, które nie znajdują się w szablonie, kliknij odpowiednie pole wyboru. (W pokazanym przykładzie można dodać interfejs MVC i/lub Web API do projektu formularzy sieci Web).

    ![Dodaj struktury](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Jeśli chcesz dodać projekt testu jednostkowego, kliknij przycisk **Dodaj testy jednostkowe**.

    ![Dodaj testy jednostkowe](creating-web-projects-in-visual-studio/_static/image5.png)
8. Jeśli chcesz, aby metoda uwierzytelniania była inna niż domyślnie udostępniana przez szablon, kliknij przycisk **Zmień uwierzytelnianie**.

    ![Przycisk Konfiguruj uwierzytelnianie](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Okno dialogowe Konfigurowanie uwierzytelniania](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Tworzenie aplikacji sieci Web lub maszyny wirtualnej na platformie Azure

Program Visual Studio zawiera funkcje, które ułatwiają współpracę z usługami platformy Azure na potrzeby hostowania aplikacji sieci Web. Na przykład w środowisku IDE programu Visual Studio można wykonać wszystkie następujące czynności:

- Twórz aplikacje sieci Web lub maszyny wirtualne, które udostępniają aplikację za pośrednictwem Internetu, oraz zarządzaj nimi.
- Wyświetlanie dzienników utworzonych przez aplikację jako działającą w chmurze.
- Uruchom w trybie debugowania zdalnie, gdy aplikacja jest uruchomiona w chmurze.
- Wyświetlanie innych usług platformy Azure, takich jak bazy danych SQL, i zarządzanie nimi.

Możesz [utworzyć konto platformy Azure](https://www.windowsazure.com/pricing/free-trial/) zawierające podstawowe usługi, takie jak aplikacje sieci Web, a jeśli jesteś subskrybentem MSDN, możesz [aktywować korzyści](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , które zapewniają miesięczne kredyty na korzystanie z dodatkowych usług platformy Azure. 

Domyślnie okno dialogowe **Nowy projekt ASP.NET** umożliwia utworzenie aplikacji sieci Web lub maszyny wirtualnej dla nowego projektu sieci Web. Jeśli nie chcesz tworzyć nowej aplikacji sieci Web lub maszyny wirtualnej, wyczyść pole wyboru **host w chmurze** .

![Tworzenie zasobów zdalnych](creating-web-projects-in-visual-studio/_static/image8.png)

Podpis pola wyboru może być **hostem w chmurze** lub **tworzyć zasoby zdalne**, a w obu przypadkach efekt jest taki sam. Jeśli to pole wyboru nie zostanie zaznaczone, program Visual Studio domyślnie utworzy aplikację sieci Web w Azure App Service. Jeśli wolisz, możesz użyć pola rozwijanego, aby zmienić je na **maszynę wirtualną** . Jeśli użytkownik nie jest jeszcze zalogowany na platformie Azure, zostanie wyświetlony monit o podanie poświadczeń platformy Azure. Po zalogowaniu się okno dialogowe umożliwia skonfigurowanie zasobów, które program Visual Studio utworzy dla projektu. Na poniższej ilustracji przedstawiono okno dialogowe aplikacji sieci Web. różne opcje są wyświetlane, jeśli zdecydujesz się utworzyć maszynę wirtualną.

![Konfigurowanie ustawień aplikacji platformy Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Aby uzyskać więcej informacji na temat sposobu korzystania z tego procesu podczas tworzenia zasobów platformy Azure, zobacz [Rozpoczynanie pracy z platformą Azure i ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) i [Tworzenie maszyny wirtualnej dla witryny sieci Web za pomocą programu Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Pozostała część tego artykułu zawiera więcej informacji na temat dostępnych szablonów i ich opcji. W tym artykule wprowadzono również elementy Bootstrap, układ i struktura z motywem używane w szablonach.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 szablonów projektu sieci Web

Program Visual Studio używa szablonów do tworzenia projektów sieci Web. Szablon projektu może tworzyć pliki i foldery w nowym projekcie, instalować pakiety NuGet oraz dostarczać przykładowy kod dla aplikacji działającej w podstawowe. Szablony implementują najnowsze standardy sieci Web i mają na celu zaprezentowanie najlepszych rozwiązań dotyczących korzystania z technologii ASP.NET, a także umożliwiają rozpoczęcie tworzenia własnych aplikacji.

Visual Studio 2013 zapewnia następujące opcje dla szablonów projektów sieci Web dla projektów przeznaczonych dla programu .NET 4,5 lub nowszych wersji programu .NET Framework:

- [Pusty szablon](#empty)
- [Szablon formularzy sieci Web](#wf)
- [Szablon MVC](#mvc)
- [Szablon internetowego interfejsu API](#webapi)
- [Szablon aplikacji jednostronicowej](#spa)
- [Szablon usługi mobilnej platformy Azure](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 Templates](#vs2012)

Możesz również zainstalować rozszerzenie programu Visual Studio, które udostępnia [szablon w serwisie Facebook](#facebook).

Aby uzyskać informacje o sposobach tworzenia projektów przeznaczonych dla platformy .NET 4, zobacz [Szablony programu Visual Studio 2012](#vs2012) w dalszej części tego tematu.

Informacje o sposobach tworzenia aplikacji ASP.NET dla klientów mobilnych można znaleźć [w temacie obsługa urządzeń przenośnych w programie ASP.NET](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Pusty szablon

Pusty szablon zawiera minimalne foldery i pliki dla aplikacji sieci Web ASP.NET, takie jak plik projektu ( *. csproj* lub. *vbproj*) i plik *Web. config* . Obsługę formularzy sieci Web, MVC i/lub interfejsu API sieci Web można dodać przy użyciu pól wyboru w obszarze **Dodaj foldery i podstawowe odwołania dla:** .

W przypadku pustego szablonu nie są dostępne żadne opcje uwierzytelniania. Funkcje uwierzytelniania są implementowane w przykładowych aplikacjach, a pusty szablon nie tworzy przykładowej aplikacji.

<a id="wf"></a>
### <a name="web-forms-template"></a>Szablon formularzy sieci Web

Struktura formularzy sieci Web udostępnia następujące funkcje, które umożliwiają szybkie tworzenie witryn sieci Web, które są bogate w funkcje interfejsu użytkownika i dostępu do danych:

- Projektant WYSIWYG w programie Visual Studio.
- Formanty serwera, które renderuje HTML i które można dostosować przez ustawienie właściwości i stylów.
- Bogaty asortyment kontroli dostępu do danych i ich wyświetlania.
- Model zdarzenia, który udostępnia zdarzenia, które można programować tak, jak w przypadku aplikacji klienckiej, takiej jak WPF.
- Automatyczne zachowywanie stanu (danych) między żądaniami HTTP.

Ogólnie rzecz biorąc, tworzenie aplikacji formularzy sieci Web wymaga mniejszego nakładu pracy programistycznej niż tworzenie tej samej aplikacji przy użyciu platformy MVC ASP.NET. Jednak formularze sieci Web nie tylko do szybkiego tworzenia aplikacji. Istnieje wiele złożonych aplikacji komercyjnych i struktur utworzonych na podstawie formularzy sieci Web.

Ze względu na to, że strona formularzy sieci Web i kontrolki na stronie automatycznie generują wiele znaczników, które są wysyłane do przeglądarki, nie masz precyzyjnej kontroli nad kodem HTML, który oferuje ASP.NET MVC. Model deklaratywny służący do konfigurowania stron i kontrolek minimalizuje ilość kodu, który trzeba napisać, ale ukrywa niektóre zachowania języka HTML i HTTP. Na przykład nie zawsze można określić, jakie znaczniki mogą być generowane przez formant.

Struktura formularzy sieci Web nie pozwala na to, aby ASP.NET MVC do opartych na wzorcach praktyk programistycznych, takich jak [programowanie sterowane testami](http://en.wikipedia.org/wiki/Test-driven_development), [oddzielenie problemów](http://en.wikipedia.org/wiki/Separation_of_concerns), [niewersja kontroli](http://en.wikipedia.org/wiki/Inversion_of_control)i [iniekcja zależności](http://en.wikipedia.org/wiki/Dependency_injection). Jeśli chcesz napisać kod w ten sposób, możesz; nie jest to tak samo automatyczne, jak w strukturze ASP.NET MVC. Projekt [ASP.NET Web Forms MVP](http://webformsmvp.com/) przedstawia podejście, które ułatwia rozdzielenie problemów i możliwości testowania, przy jednoczesnym zachowaniu szybkiego rozwoju, który tworzy formularz sieci Web do dostarczenia. Program Microsoft SharePoint jest oparty na programie Web Forms MVP.

Szablon formularzy sieci Web tworzy przykładową aplikację formularzy sieci Web, która używa narzędzia [ładowania początkowego](#bootstrap) do zapewniania odpowiedzi na funkcje projektowania i tworzenia motywów. Na poniższej ilustracji przedstawiono stronę główną.

![Strona główna aplikacji szablonu formularzy sieci Web](creating-web-projects-in-visual-studio/_static/image10.png)

Aby uzyskać więcej informacji na temat formularzy sieci Web, zobacz [ASP.NET Web Forms](https://asp.net/web-forms). Aby uzyskać informacje o tym, co robi szablon formularzy sieci Web, zobacz [Tworzenie podstawowej aplikacji formularzy sieci Web przy użyciu Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Szablon MVC

ASP.NET MVC został zaprojektowany w celu ułatwienia opartych na wzorcach praktyk programistycznych, takich jak [programowanie sterowane testami](http://en.wikipedia.org/wiki/Test-driven_development), [separacja problemów](http://en.wikipedia.org/wiki/Separation_of_concerns), [niewersja kontroli](http://en.wikipedia.org/wiki/Inversion_of_control)i [iniekcja zależności](http://en.wikipedia.org/wiki/Dependency_injection). Struktura zachęca do rozdzielania warstwy logiki biznesowej aplikacji sieci Web od jej warstwy prezentacji. Dzieląc aplikację na modele (M), widoki (V) i kontrolery (C), ASP.NET MVC może ułatwić zarządzanie złożonością w większych aplikacjach.

Dzięki ASP.NET MVC możesz bezpośrednio korzystać z kodu HTML i HTTP niż w formularzach sieci Web. Na przykład formularze sieci Web mogą automatycznie zachować stan między żądaniami HTTP, ale musisz kod, który jest jawnie w MVC. Zaletą modelu MVC jest to, że umożliwia ona pełną kontrolę nad tym, co aplikacja działa i jak działa w środowisku sieci Web. Wadą jest konieczność pisania większej ilości kodu.

MVC został zaprojektowany do rozszerzalności, zapewniając deweloperom energetycznym możliwość dostosowywania struktury do potrzeb aplikacji. Ponadto kod źródłowy ASP.NET MVC jest dostępny w ramach licencji OSI.

Szablon MVC tworzy przykładową aplikację MVC 5, która używa narzędzia [Bootstrap](#bootstrap) do zapewniania odpowiedzi na funkcje projektowania i tworzenia motywów. Na poniższej ilustracji przedstawiono stronę główną.

![Przykładowa aplikacja MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Aby uzyskać więcej informacji na temat MVC, zobacz [ASP.NET MVC](https://asp.net/mvc). Aby uzyskać informacje o sposobach wybierania szablonu MVC 4, zobacz [Szablony programu Visual Studio 2012](#vs2012) w dalszej części tego artykułu.

<a id="webapi"></a>
### <a name="web-api-template"></a>Szablon internetowego interfejsu API

Szablon internetowego interfejsu API tworzy przykładową usługę sieci Web opartą na interfejsie API sieci Web, w tym strony pomocy interfejsu API oparte na MVC.

Składnik Web API platformy ASP.NET to środowisko ułatwiające tworzenie usług HTTP, które można udostępniać dla wielu różnych klientów, takich jak przeglądarki i urządzenia przenośne. Interfejs API sieci Web ASP.NET to idealna platforma służąca do tworzenia usług RESTful na .NET Framework.

Szablon internetowego interfejsu API tworzy przykładową usługę sieci Web. Na poniższych ilustracjach przedstawiono przykładowe strony pomocy.

![Strona pomocy interfejsu API sieci Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Strona pomocy interfejsu API sieci Web dla interfejsu GET API](creating-web-projects-in-visual-studio/_static/image13.png)

Aby uzyskać więcej informacji na temat interfejsu API sieci Web, zobacz [ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Szablon aplikacji jednostronicowej

Szablon aplikacji jednostronicowej (SPA) tworzy przykładową aplikację, która używa języka JavaScript, HTML 5 i [KnockoutJS](http://knockoutjs.com/) na kliencie oraz ASP.NET internetowego interfejsu API na serwerze.

Jedyną opcją uwierzytelniania dla szablonu SPA są [konta poszczególnych użytkowników](#indauth).

Na poniższej ilustracji przedstawiono początkowy stan przykładowej aplikacji, którą kompiluje szablon SPA.

![Aplikacja Przykładowa SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Aby uzyskać informacje na temat sposobu tworzenia aplikacji przy użyciu szablonu SPA, zobacz [Web API-External Authentication Services](../../../web-api/overview/security/external-authentication-services.md).

Aby uzyskać więcej informacji na temat aplikacji jednostronicowych ASP.NET oraz dodatkowych szablonów SPA używających platform języka JavaScript innych niż KnockoutJS, zobacz następujące zasoby:

- [ASP.NET aplikację jednostronicową](../../../single-page-application/index.md).
- [Informacje o funkcjach zabezpieczeń w szablonie SPA dla VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Aplikacje jednostronicowe: Kompiluj nowoczesne, reagujące Web Apps z ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Facebook Template

Możesz zainstalować [rozszerzenie programu Visual Studio, które udostępnia szablon w serwisie Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Ten szablon służy do tworzenia przykładowej aplikacji, która została zaprojektowana do działania w witrynie w sieci Web w serwisie Facebook. Jest on oparty na ASP.NET MVC i używa interfejsu API sieci Web do obsługi funkcji aktualizacji w czasie rzeczywistym.

Dla szablonu Facebook nie są dostępne żadne opcje uwierzytelniania, ponieważ aplikacje w serwisie Facebook działają w ramach witryny Facebook i polegają na uwierzytelnianiu w serwisie Facebook.

Aby uzyskać więcej informacji na temat ASP.NET aplikacji Facebook, zobacz [aktualizowanie interfejsu API usługi Facebook w serwisie MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 Templates

Okno dialogowe tworzenia projektu sieci Web Visual Studio 2013 nie zapewnia dostępu do niektórych szablonów, które były dostępne w programie Visual Studio 2012. Jeśli chcesz użyć jednego z tych szablonów, możesz kliknąć węzeł programu Visual Studio 2012 w lewym okienku okna dialogowego Nowy projekt programu Visual Studio.

![Szablony programu Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Węzeł **programu Visual Studio 2012** umożliwia wybranie następujących szablonów sieci Web, do których nie masz dostępu, w domyślnej liście szablonów dla Visual Studio 2013:

- ASP.NET aplikacji sieci Web MVC 4
- Aplikacja sieci Web ASP.NET Dynamic Data Entities
- Kontrolka serwera ASP.NET AJAX
- Rozszerzenie kontroli serwera ASP.NET AJAX
- ASP.NET — formant serwera

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Uruchomienie Bootstrap w szablonach projektu sieci Web Visual Studio 2013

Szablony projektów Visual Studio 2013 używają [Bootstrap](http://getbootstrap.com/), układu i struktury z motywem utworzonym przez serwis Twitter. Funkcja ładowania początkowego korzysta z CSS3 w celu zapewnienia, że program umożliwia dynamiczne dostosowanie do różnych rozmiarów okna przeglądarki. Na przykład w oknie szerokiej przeglądarki Strona główna utworzona przez szablon formularzy sieci Web wygląda jak Następująca ilustracja:

![Strona główna aplikacji szablonu formularzy sieci Web](creating-web-projects-in-visual-studio/_static/image16.png)

Zmniejsz poziom okna, a uporządkowane kolumny przechodzą na układ pionowy:

![Układ pionowej kolumny Bootstrap](creating-web-projects-in-visual-studio/_static/image17.png)

Zawęź okno nieco więcej, a poziome menu przełączy się w ikonę, którą można kliknąć, aby rozwinąć w menu zorientowanym pionowo:

![Ikona menu Bootstrap](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu pionowe uruchamiania](creating-web-projects-in-visual-studio/_static/image19.png)

Możesz również użyć funkcji obsługi motywów Bootstrap, aby łatwo zmienić wygląd i działanie aplikacji. Na przykład możesz wykonać poniższe kroki, aby zmienić motyw.

1. W przeglądarce przejdź do [http://Bootswatch.com](http://Bootswatch.com), wybierz motyw, a następnie kliknij pozycję **Pobierz**. (Spowoduje to pobranie domyślnie pliku *Bootstrap. min. css* ; Jeśli chcesz PRZEJRZEĆ kod CSS, Pobierz polecenie *Bootstrap. css* zamiast wersji zminimalizowanego).
2. Skopiuj zawartość pobranego pliku CSS.
3. W programie Visual Studio Utwórz nowy plik **arkusza stylów** o nazwie *Bootstrap-Theme. css* w folderze *Content* i wklej do niego pobrany kod CSS.
4. Otwórz *aplikację\_Start/pakiet. config* i Zmień plik *Bootstrap. css* na *Bootstrap-Theme. css*.

Uruchom projekt ponownie, a aplikacja ma nowy wygląd. Na poniższej ilustracji przedstawiono efekt motywu Amelia:

![Motyw Amelia Bootstrap](creating-web-projects-in-visual-studio/_static/image20.png)

Dostępnych jest wiele kompozycji Bootstrap, zarówno wersji bezpłatnych, jak i Premium. Polecenie Bootstrap oferuje także szeroką gamę składników interfejsu użytkownika, takich jak [listy rozwijane](http://twitter.github.io/bootstrap/components.html#dropdowns), [grupy przycisków](http://twitter.github.io/bootstrap/components.html#buttonGroups)i [ikony](http://twitter.github.io/bootstrap/base-css.html#images). Aby uzyskać więcej informacji na temat ładowania początkowego, zobacz [lokację Bootstrap](http://twitter.github.io/bootstrap/).

W przypadku korzystania z projektanta formularzy sieci Web w programie Visual Studio należy pamiętać, że projektant nie obsługuje CSS3, więc nie pokazuje prawidłowo wszystkich efektów motywów ładowania lub zmiany układu. Jednak strony formularzy sieci Web będą wyświetlane prawidłowo podczas wyświetlania w przeglądarce.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Dodawanie obsługi dla dodatkowych platform

Po wybraniu szablonu pola wyboru dla struktur, które są używane przez szablon, są automatycznie wybierane. Na przykład w przypadku wybrania szablonu **formularzy sieci Web** jest zaznaczone pole wyboru **formularze sieci Web** i nie można go usunąć.

![Wybierz szablon](creating-web-projects-in-visual-studio/_static/image21.png)

![Dodaj struktury](creating-web-projects-in-visual-studio/_static/image22.png)

Możesz zaznaczyć pole wyboru dla struktury, która nie znajduje się w szablonie, aby dodać obsługę tej struktury podczas tworzenia projektu. Na przykład, aby włączyć strony Web Forms *. aspx* w przypadku wybrania szablonu MVC, zaznacz pole wyboru **formularze sieci Web** . Lub aby włączyć MVC, gdy używasz szablonu formularzy sieci Web, kliknij pole wyboru **MVC** . Dodanie struktury umożliwia obsługę czasu projektowania i czasu wykonywania. Na przykład jeśli dodasz obsługę MVC do projektu formularzy sieci Web, będziesz mieć możliwość tworzenia szkieletów kontrolerów i widoków.

W przypadku łączenia formularzy sieci Web i MVC w projekcie i włączania [przyjaznych adresów URL](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) w formularzach sieci Web mogą wystąpić nieoczekiwane problemy z routingiem, gdy jeden z adresów URL ma wiele możliwych elementów docelowych. Określone trasy będą mieć pierwszeństwo. Na przykład jeśli masz kontroler `Home` i stronę *Home. aspx* , adres URL `http://contoso.com/home` zostanie przestawiony do strony *Home. aspx* , jeśli wywołasz metodę `EnableFriendlyUrls` przed wywołaniem metody `MapRoute` w *RouteConfig.cs*lub ten sam adres URL przejdzie do widoku domyślnego dla kontrolera `Home`, jeśli wywoła `MapRoute` przed `EnableFriendlyUrls`.

Dodanie struktury nie powoduje dodania żadnej z przykładowych funkcji aplikacji. Jeśli na przykład dodasz obsługę formularzy sieci Web po wybraniu szablonu MVC, nie zostanie utworzony plik domyślnej strony głównej *. aspx* . Dodawane są tylko foldery, pliki i odwołania wymagane do obsługi platformy. Z tego powodu Dodawanie struktur nie zmienia opcji uwierzytelniania, które są implementowane przez kod w przykładowych aplikacjach utworzonych przez szablony. Jeśli na przykład wybierzesz pusty szablon i dodasz obsługę formularzy sieci Web lub MVC, przycisk **Konfiguruj uwierzytelnianie** będzie nadal wyłączony.

W poniższych sekcjach opisano krótko efekt każdego pola wyboru.

### <a name="add-web-forms-support"></a>Dodawanie obsługi formularzy sieci Web

Tworzy pustą *aplikację\_foldery danych* i *modeli* oraz plik *Global. asax* . Są one już utworzone przez wszystkie szablony inne niż pusty szablon, dlatego zaznaczenie pola wyboru formularze sieci Web nie powoduje żadnych różnic w odniesieniu do innych szablonów.

Szablon formularzy sieci Web domyślnie włącza przyjazne adresy URL, ale po dodaniu obsługi formularzy sieci Web do innych szablonów, zaznaczając pole wyboru formularze sieci Web przyjazne adresy URL nie są automatycznie włączane.

### <a name="add-mvc-support"></a>Dodawanie obsługi MVC

Instaluje pakiety NuGet MVC, Razor i Webpages, tworzy pustą *aplikację\_dane*, *Kontrolery*, *modele*i foldery *widoków* , tworzy *aplikację\_Start* folderu z plikiem *RouteConfig.cs* i tworzy plik *Global. asax* .

### <a name="add-web-api-support"></a>Dodawanie obsługi interfejsu API sieci Web

Instaluje pakiety NuGet WebApi i Newtonsoft. JSON, tworzy puste *aplikacje\_dane*, *Kontrolery*i foldery *modeli* , tworzy *aplikację\_folder początkowy* z plikiem *WebApiConfig.cs* i tworzy plik *Global. asax* .

<a id="auth"></a>
## <a name="authentication-methods"></a>Metody uwierzytelniania

Visual Studio 2013 oferuje kilka opcji uwierzytelniania dla szablonów Web Forms, MVC i Web API:

- [Bez uwierzytelniania](#noauth)
- [Indywidualne konta użytkowników](#indauth) (ASP.NET Identity, wcześniej znane jako członkostwo ASP.NET)
- [Konta organizacyjne](#orgauth) (system Windows Server Active Directory lub Azure Active Directory)
- [Uwierzytelnianie systemu Windows](#winauth) (intranet)

![Okno dialogowe Konfigurowanie uwierzytelniania](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Bez uwierzytelniania

W przypadku wybrania opcji **Brak uwierzytelniania**Przykładowa aplikacja nie będzie zawierać żadnych stron sieci Web do zalogowania, żaden interfejs użytkownika wskazujący, kto jest zalogowany, nie ma klas jednostek dla bazy danych członkostwa ani parametrów połączenia dla bazy danych członkostwa.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Indywidualne konta użytkowników

W przypadku wybrania **poszczególnych kont użytkowników**Przykładowa aplikacja zostanie skonfigurowana do korzystania z ASP.NET Identity (wcześniej znanej jako członkostwo ASP.NET) na potrzeby uwierzytelniania użytkowników. ASP.NET Identity umożliwia użytkownikowi zarejestrowanie konta przez utworzenie nazwy użytkownika i hasła w witrynie lub zalogowanie się przy użyciu dostawców społecznościowych, takich jak Facebook, Google, konto Microsoft lub Twitter. Domyślny magazyn danych dla profilów użytkowników w ASP.NET Identity to SQL Server baza danych LocalDB, którą można wdrożyć w SQL Server lub Azure SQL Database dla lokacji produkcyjnej.

W Visual Studio 2013 te funkcje są takie same jak w programie Visual Studio 2012, ale kod źródłowy systemu członkostwa ASP.NET został ponownie zapisany. Zalety nowej bazy kodu są następujące:

- Nowy system członkostwa jest oparty na [Owin](http://owin.org/) , a nie w module uwierzytelniania formularzy ASP.NET. Oznacza to, że można użyć tego samego mechanizmu uwierzytelniania, niezależnie od tego, czy używasz formularzy sieci Web, czy MVC w usługach IIS, czy jesteś własnym interfejsem API sieci Web lub sygnalizującym.
- Nowa baza danych członkostwa jest zarządzana przez Entity Framework Code First, a wszystkie tabele są reprezentowane przez klasy jednostek, które można modyfikować. Oznacza to, że można łatwo dostosować schemat bazy danych i interfejs użytkownika sieci Web związany z profilem, aby dopasować go do własnych potrzeb. Możesz też łatwo wdrożyć aktualizacje przy użyciu Migracje Code First.

Nowy system członkostwa jest implementowany automatycznie w nowych szablonach i może być zaimplementowany ręcznie w dowolnym projekcie przeznaczonym dla programu .NET 4,5 lub nowszego.

ASP.NET Identity jest dobrym rozwiązaniem, jeśli tworzysz internetową witrynę sieci Web, która jest przeznaczona głównie dla klientów zewnętrznych. Jeśli Twoja organizacja używa Active Directory lub pakietu Office 365 i chcesz utworzyć projekt, który umożliwia logowanie jednokrotne dla pracowników i partnerów gospodarczych, opcja **konta organizacji** może być lepszym wyborem.

Więcej informacji o opcjach poszczególnych kont użytkowników można znaleźć w następujących zasobach:

- [www.asp.net/identity](../../../identity/index.md). Dokumentacja dotycząca ASP.NET Identity w witrynie sieci Web ASP.NET.
- [Utwórz aplikację ASP.NET MVC 5 za pomocą usługi Facebook i usługi Google OAuth2 i OpenID Connect](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Pokazuje również, jak dostosować dane profilu użytkownika.
- [Interfejs API sieci Web — zewnętrzne usługi uwierzytelniania](../../../web-api/overview/security/external-authentication-services.md)
- [Dodawanie zewnętrznych logowań do aplikacji ASP.NET w Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Konta organizacyjne

W przypadku wybrania **konta organizacyjnego**aplikacja Przykładowa zostanie skonfigurowana do uwierzytelniania przy użyciu programu Windows Identity Foundation (WIF) na podstawie kont użytkowników w Azure Active Directory (Azure AD, który obejmuje pakiet Office 365) lub Active Directory systemu Windows Server. Aby uzyskać więcej informacji, zobacz [Opcje uwierzytelniania konta organizacyjnego](#orgauthoptions) w dalszej części tego tematu.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Uwierzytelnianie systemu Windows

W przypadku wybrania opcji **uwierzytelnianie systemu Windows**aplikacja Przykładowa zostanie skonfigurowana do uwierzytelniania przy użyciu modułu usług IIS uwierzytelniania systemu Windows. Aplikacja wyświetli identyfikator domeny i użytkownika konta usługi Active Directory lub komputera lokalnego, które jest zalogowany do systemu Windows, ale nie będzie zawierać rejestracji użytkownika ani logowania. Ta opcja jest przeznaczona dla intranetowych witryn sieci Web.

Alternatywnie można utworzyć witrynę intranetową, która używa uwierzytelniania usługi AD, wybierając [opcję lokalna w obszarze konta organizacyjne](#orgauthonprem). Opcja lokalna używa systemu Windows Identity Foundation (WIF) zamiast modułu uwierzytelniania systemu Windows. Niektóre dodatkowe kroki są wymagane w celu skonfigurowania opcji lokalnej, ale WIF umożliwia korzystanie z funkcji, które nie są dostępne w module uwierzytelniania systemu Windows. Na przykład za pomocą WIF można skonfigurować dostęp do aplikacji w Active Directory i wysyłać zapytania dotyczące danych katalogu.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Opcje uwierzytelniania konta organizacyjnego

W oknie dialogowym **Konfigurowanie uwierzytelniania** dostępne są następujące opcje Azure Active Directory (usługa Azure AD, która obejmuje pakiet Office 365) lub uwierzytelnianie konta systemu Windows Server Active Directory (AD):

- [Chmura — pojedyncza organizacja](#orgauthsingle) (Azure AD lub AD z integracją katalogów z usługą Azure AD)
- [Chmura — wiele organizacji](#orgauthmulti) (Azure AD lub AD z integracją katalogów z usługą Azure AD)
- [Lokalna](#orgauthonprem) (AD)

Jeśli chcesz wypróbować jedną z opcji usługi Azure AD, ale nie masz jeszcze konta, [kliknij tutaj, aby utworzyć konto usługi Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> W przypadku wybrania jednej z opcji usługi Azure AD projekt wymaga bazy danych, a użytkownik musi zalogować się do konta administratora globalnego dla dzierżawy usługi Azure AD. Wprowadź nazwę i hasło konta organizacji (na przykład admin@contoso.onmicrosoft.com) z uprawnieniami administracyjnymi dla dzierżawy usługi Azure AD.
> 
> **Nie wprowadzaj poświadczeń dla konto Microsoft (na przykład contoso@hotmail.com) w oknie dialogowym logowania.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Uwierzytelnianie w chmurze — pojedyncza organizacja

![Uwierzytelnianie pojedynczej organizacji](creating-web-projects-in-visual-studio/_static/image24.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w jednej [dzierżawie](https://technet.microsoft.com/library/jj573650.aspx)usługi Azure AD. Na przykład witryna jest contoso.com i zostanie udostępniona pracownikom firmy Contoso, którzy znajdują się w dzierżawie contoso.onmicrosoft.com. Nie będzie można skonfigurować usługi Azure AD, aby umożliwić użytkownikom z innych dzierżawców dostęp do aplikacji.

#### <a name="domain"></a>Domena

Wprowadź domenę usługi Azure AD, w której chcesz skonfigurować aplikację, na przykład: `contoso.onmicrosoft.com`. Jeśli masz [domenę niestandardową](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), taką jak `contoso.com`, a nie `contoso.onmicrosoft.com`, możesz wprowadzić ją tutaj.

#### <a name="access-level"></a>Poziom dostępu

Jeśli aplikacja musi zbadać lub zaktualizować informacje o katalogu przy użyciu interfejs API programu Graph, wybierz pozycję Logowanie jednokrotne **, Odczytaj dane katalogu** lub **Logowanie jednokrotne, Odczytaj i Zapisz dane katalogu**. W przeciwnym razie wybierz pozycję **Logowanie jednokrotne**. Aby uzyskać więcej informacji, zobacz [poziomy dostępu do aplikacji](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) i [Używanie interfejs API programu Graph do wysyłania zapytań do usługi Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Identyfikator URI identyfikatora aplikacji

Domyślnie szablon tworzy identyfikator URI aplikacji przez dołączenie nazwy projektu do domeny usługi Azure AD. Na przykład jeśli nazwa projektu jest `Example` a domena jest `contoso.onmicrosoft.com`, identyfikator URI aplikacji zostanie `https://contoso.onmicrosoft.com/Example`. Jeśli chcesz ręcznie określić identyfikator URI identyfikatora aplikacji, rozwiń sekcję **więcej opcji** i wprowadź identyfikator URI aplikacji w polu tekstowym. Identyfikator URI identyfikatora aplikacji musi zaczynać się od `https://`.

Domyślnie, jeśli aplikacja, która jest już obsługiwana w usłudze Azure AD ma ten sam identyfikator URI identyfikatora aplikacji, która jest używana przez program Visual Studio dla projektu, projekt zostanie połączony z istniejącą aplikacją zamiast aprowizacji nowej. Jeśli chcesz, aby w takim przypadku potrzebna była obsługa nowej aplikacji, wyczyść pole wyboru **Zastąp wpis aplikacji, jeśli istnieje już taki sam identyfikator** .

Jeśli pole wyboru **Zastąp** jest wyczyszczone, a program Visual Studio wykryje istniejącą aplikację z identyfikatorem URI tego samego identyfikatora aplikacji, zostanie utworzony nowy identyfikator URI przez dołączenie liczby do identyfikatora URI, który ma być używany. Załóżmy na przykład, że nazwa projektu to `Example`, pole tekstowe pozostaw puste, wyczyść pole wyboru **Zastąp** , a dzierżawa usługi Azure AD ma już aplikację z `https://contoso.onmicrosoft.com/Example`URI. W takim przypadku zostanie zainicjowana Nowa aplikacja z IDENTYFIKATORem URI aplikacji, taki jak `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Inicjowanie obsługi aplikacji w usłudze Azure AD

Aby można było zainicjować obsługę administracyjną aplikacji w usłudze Azure AD lub połączyć projekt z istniejącą aplikacją, program Visual Studio wymaga poświadczeń administratora globalnego dla domeny. Po kliknięciu przycisku **OK** w oknie dialogowym **Konfigurowanie uwierzytelniania** zostanie wyświetlony monit o podanie nazwy użytkownika i hasła administratora globalnego dla określonej domeny. Później po kliknięciu przycisku **Utwórz projekt** w oknie dialogowym **Nowy projekt ASP.NET** program Visual Studio inicjuje aplikację w usłudze Azure AD. Należy pamiętać, że w ramach tego procesu program Visual Studio osadza wartości klucza tajnego klienta w pliku Web. config, który wygasa rok po utworzeniu.

Aby uzyskać informacje na temat sposobu tworzenia aplikacji korzystających z uwierzytelniania w chmurze w ramach **jednej organizacji** , zobacz następujące zasoby:

- [Uwierzytelnianie na platformie Azure](../2012/windows-azure-authentication.md)
- [Dodawanie logowania do aplikacji sieci Web przy użyciu usługi Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Tworzenie aplikacji ASP.NET z wykorzystaniem usługi Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Zabezpieczanie interfejsu API sieci Web ASP.NET za pomocą usługi Azure AD i składników programu Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Samouczki nie zostały jeszcze zaktualizowane do Visual Studio 2013; Niektóre samouczki kierowane ręcznie są zautomatyzowane w Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Chmura — uwierzytelnianie wiele organizacji

![Uwierzytelnianie wielu organizacji](creating-web-projects-in-visual-studio/_static/image25.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w wielu [dzierżawach](https://technet.microsoft.com/library/jj573650.aspx)usługi Azure AD. Na przykład witryna jest contoso.com i zostanie udostępniona pracownikom firmy Contoso, którzy znajdują się w dzierżawie contoso.onmicrosoft.com i pracownicy firmy Fabrikam, którzy znajdują się w dzierżawie fabrikam.onmicrosoft.com.

Ustawienia wprowadzane przez użytkownika i krok aprowizacji aplikacji są podobne do [uwierzytelniania pojedynczej organizacji](#orgauthsingle).

Aby uzyskać informacje dotyczące sposobu tworzenia aplikacji korzystających z uwierzytelniania w chmurze w ramach **chmury** , zobacz następujące zasoby:

- [Łatwa integracja aplikacji sieci Web z Azure Active Directory, ASP.NET &amp; programu Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) na blogu zespołu Active Directory.
- [Tworzenie wielodostępnych aplikacji sieci Web za pomocą samouczka usługi Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) . Samouczek nie został jeszcze zaktualizowany dla Visual Studio 2013; część samouczka, którą można wykonać ręcznie, jest zautomatyzowana w Visual Studio 2013.
- [Aby móc się zalogować, musisz zarejestrować się przy użyciu własnych organizacji ASP.NET App](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog Vittorio Bertocci, który wyjaśnia, jak rozwiązać typowe problemy występujące podczas tworzenia projektu, który korzysta z uwierzytelniania wielofirmowego.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Lokalne uwierzytelnianie organizacyjne

![Lokalne uwierzytelnianie organizacyjne](creating-web-projects-in-visual-studio/_static/image26.png)

Wybierz tę opcję, jeśli chcesz włączyć uwierzytelnianie dla kont użytkowników, które są zdefiniowane w systemie Windows Server Active Directory (AD), i nie chcesz używać usługi Azure AD. Za pomocą tej opcji można utworzyć witrynę intranetową lub witrynę internetową. W przypadku witryny internetowej należy zapewnić dostęp do usługi AD przy użyciu Active Directory Federation Services (AD FS). Aby uzyskać więcej informacji, zobacz [Korzystanie z opcji lokalnego uwierzytelniania w organizacji (AD FS) z usługą ASP.NET w Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

W przypadku witryny intranetowej Alternatywnie można wybrać opcję [uwierzytelnianie systemu Windows](#winauth) zamiast tej opcji. W przypadku opcji uwierzytelniania systemu Windows nie trzeba podawać adresu URL dokumentu metadanych. Jednak uwierzytelnianie systemu Windows nie zapewnia możliwości kontrolowania dostępu do aplikacji w Active Directory lub do wykonywania zapytań dotyczących danych katalogu.

#### <a name="on-premises-authority"></a>Urząd lokalny

Wprowadź adres URL, który wskazuje na dokument metadanych. Dokument metadanych zawiera współrzędne urzędu. Aplikacja będzie używać tych współrzędnych do kierowania przepływem logowania w sieci Web.

#### <a name="application-id-uri"></a>Identyfikator URI identyfikatora aplikacji

Podaj unikatowy identyfikator URI, który może być używany przez usługi AD do identyfikowania tej aplikacji, lub pozostaw to pole puste, aby pozwolić programowi Visual Studio na utworzenie go.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Następne kroki

W tym dokumencie przedstawiono podstawową pomoc dotyczącą tworzenia nowego projektu sieci Web ASP.NET w programie Visual Studio 2013. Aby uzyskać więcej informacji na temat korzystania z programu Visual Studio do programowania w sieci Web, zobacz [https://www.asp.net/visual-studio/](../../index.md).

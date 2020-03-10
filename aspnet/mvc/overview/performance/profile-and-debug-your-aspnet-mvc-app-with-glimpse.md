---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilowanie i debugowanie aplikacji ASP.NET MVC przy użyciu możliwość wypróbowania innowacyjnego | Microsoft Docs
author: Rick-Anderson
description: Możliwość wypróbowania innowacyjnego to rozwijana i rosnąca rodzina pakietów NuGet typu open source, które udostępniają szczegółowe informacje o wydajności, debugowaniu i diagnostyki dla ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538536"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profilowanie i debugowanie aplikacji ASP.NET MVC za pomocą pakietów Glimpse

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Możliwość wypróbowania innowacyjnego to rozwijana i rosnąca rodzina pakietów NuGet typu open source, które udostępniają szczegółowe informacje o wydajności, debugowaniu i diagnostyki dla aplikacji ASP.NET. Jest to bardzo proste Instalowanie, lekkie, niezwykle szybkie i wyświetlane kluczowe metryki wydajności w dolnej części każdej strony. Pozwala to na przechodzenie do aplikacji w celu sprawdzenia, co dzieje się na serwerze. Usługa możliwość wypróbowania innowacyjnego zapewnia dużo cennych informacji, zalecamy korzystanie z nich w całym cyklu programistycznym, w tym w środowisku testowym platformy Azure. Podczas gdy [programu Fiddler](http://www.telerik.com/fiddler) i [Narzędzia deweloperskie F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) zapewniają widok po stronie klienta, możliwość wypróbowania innowacyjnego udostępnia szczegółowy widok z serwera. Ten samouczek koncentruje się na korzystaniu z pakietów MVC i EF możliwość wypróbowania innowacyjnego ASP.NET, ale wiele innych pakietów jest dostępnych. Gdzie to możliwe, połączymy się z odpowiednimi [dokumentami możliwość wypróbowania innowacyjnego](http://getglimpse.com/Docs/) , które ułatwiają konserwację. Możliwość wypróbowania innowacyjnego to projekt open source, który może być również współautorem kodu źródłowego i dokumentów.

- [Instalowanie możliwość wypróbowania innowacyjnego](#ig)
- [Włącz możliwość wypróbowania innowacyjnego dla hosta lokalnego](#eg)
- [Karta oś czasu](#Time)
- [Wiązanie modelu](#mb)
- [Trasy](#route)
- [Korzystanie z programu możliwość wypróbowania innowacyjnego na platformie Azure](#da)
- [Dodatkowe zasoby](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalowanie możliwość wypróbowania innowacyjnego

Program możliwość wypróbowania innowacyjnego można zainstalować z konsoli Menedżera pakietów NuGet lub z konsoli **Zarządzanie pakietami NuGet** . W tej wersji demonstracyjnej zainstaluję pakiety Mvc5 i EF6:

![Zainstaluj możliwość wypróbowania innowacyjnego z poziomu narzędzia NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Wyszukaj *możliwość wypróbowania innowacyjnego. Ef*

![Możliwość wypróbowania innowacyjnego. EF z poziomu instalacji NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Wybierając **zainstalowane pakiety**, można zobaczyć zainstalowane moduły zależne możliwość wypróbowania innowacyjnego:

![Zainstalowano pakiety możliwość wypróbowania innowacyjnego z poziomu okno dialogowe](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Następujące polecenia instalują moduły możliwość wypróbowania innowacyjnego MVC5 i EF6 z konsoli Menedżera pakietów:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Włącz możliwość wypróbowania innowacyjnego dla hosta lokalnego

Przejdź do http://localhost:&lt&gt;;p wartość/Glimpse.axd, a następnie kliknij przycisk <strong>Włącz możliwość wypróbowania innowacyjnego on</strong> .

![Strona możliwość wypróbowania innowacyjnego AXD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Jeśli masz wyświetlany pasek ulubionych, możesz przeciągnąć i upuścić przyciski możliwość wypróbowania innowacyjnego i dodać je jako Bookmarklets:

![Program IE z możliwość wypróbowania innowacyjnego Bookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Teraz możesz nawigować po swojej aplikacji, a w dolnej części strony zostanie wyświetlony **ekran** (HUD).

![Strona menedżera kontaktów z HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

Na [stronie możliwość WYPRÓBOWANIA innowacyjnego HUD](http://getglimpse.com/Docs/Heads-up-Display) szczegółowo przedstawiono informacje o chronometrażu. Niezauważalne dane wydajności wyświetlane przez HUD mogą natychmiast powiadomić użytkownika o problemie — przed przejściem do cyklu testowego. Kliknięcie&quot; &quot;g w prawym dolnym rogu spowoduje wyświetlenie panelu możliwość wypróbowania innowacyjnego:

![Panel możliwość wypróbowania innowacyjnego](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na powyższym obrazie zostanie wybrana [karta wykonywanie](http://getglimpse.com/Docs/Execution-Tab) , która wyświetla szczegóły czasu akcji i filtrów w potoku. [Zegar filtru Zatrzymaj czujkę](http://www.nuget.org/packages/StopWatch/) można zobaczyć na stronie etap 6 potoku. Gdy mój czasomierz wagi uproszczonej może zapewnić przydatne dane dotyczące profilu/czasu, nie wszystkie czasu poświęcają na autoryzację i renderowanie widoku. Informacje o moim czasomierzu można odczytać w [profilu i w czasie, gdy aplikacja ASP.NET MVC jest w całości na platformie Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Strona [karty](http://getglimpse.com/Docs/Tabs) zawiera linki do szczegółowych informacji na poszczególnych kartach.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Karta oś czasu

Po zmodyfikowaniu pozostałego [samouczka Dr 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) z następującej zmiany kodu do kontrolera instruktorów:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Powyższy kod umożliwia przekazywanie danych w ciągu zapytania (`eager`) w celu kontrolowania eager lub jawnego ładowania dane. Na poniższym obrazie jest używane jawne ładowanie, a strona chronometrażu przedstawia każdą rejestrację załadowana w `Index`ej metodzie działania:

![jawne ładowanie](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

W poniższym kodzie określono eager i każda Rejestracja jest pobierana po wywołaniu widoku `Index`:

![eager jest określony](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Możesz umieścić wskaźnik myszy nad segmentem czasu, aby uzyskać szczegółowe informacje o chronometrażu:

![Aktywuj wskaźnik, aby wyświetlić szczegółowy chronometraż](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Powiązanie modelu

[Karta powiązanie modelu](http://getglimpse.com/Docs/Model-Binding-Tab) zawiera mnóstwo informacji, które pomogą zrozumieć, w jaki sposób zmienne formularzy są powiązane i dlaczego niektóre nie są związane z oczekiwaniami. Na poniższej ilustracji przedstawiono **?** ikona, którą można kliknąć, aby wyświetlić stronę pomocy możliwość wypróbowania innowacyjnego dla tej funkcji.

![Widok powiązania modelu możliwość wypróbowania innowacyjnego](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Trasy

 Karta trasy możliwość wypróbowania innowacyjnego może pomóc w debugowaniu i zrozumieniu routingu. Na poniższej ilustracji wybrano trasę produktu (i jest ona wyświetlana w kolorze zielonym, Konwencja możliwość wypróbowania innowacyjnego). ![wybraną nazwę produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) ograniczenia trasy, obszary i tokeny danych są również wyświetlane. Aby uzyskać więcej informacji, zobacz [trasy możliwość wypróbowania innowacyjnego](http://getglimpse.com/Docs/Routes-Tab) i [Routing atrybutów w ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) . 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Korzystanie z programu możliwość wypróbowania innowacyjnego na platformie Azure

Domyślne zasady zabezpieczeń możliwość wypróbowania innowacyjnego umożliwiają wyświetlanie danych możliwość wypróbowania innowacyjnego z hosta lokalnego. Te zasady zabezpieczeń można zmienić, aby można było wyświetlać te dane na serwerze zdalnym (na przykład w aplikacji sieci Web na platformie Azure). W przypadku środowisk testowych na platformie Azure Dodaj wyróżniony znacznik do dolnej części pliku *Web. config* , aby włączyć możliwość wypróbowania innowacyjnego:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

W przypadku tej zmiany każdy użytkownik może zobaczyć dane możliwość wypróbowania innowacyjnego w zdalnej witrynie. Rozważ dodanie znacznika powyżej do profilu publikowania, aby został wdrożony tylko podczas korzystania z tego profilu publikowania (na przykład profilu testu platformy Azure). Aby ograniczyć możliwość wypróbowania innowacyjnego dane, dodamy rolę `canViewGlimpseData` i zezwolisz użytkownikom z tej roli na wyświetlanie danych możliwość wypróbowania innowacyjnego.

Usuń Komentarze z pliku *GlimpseSecurityPolicy.cs* i Zmień wywołanie [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) z `Administrator` na rolę `canViewGlimpseData`:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Zabezpieczenia — bogate dane udostępniane przez możliwość wypróbowania innowacyjnego mogą uwidaczniać bezpieczeństwo aplikacji. Firma Microsoft nie wykonała inspekcji zabezpieczeń możliwość wypróbowania innowacyjnego do użycia w aplikacjach do produkcji.

Aby uzyskać informacje na temat dodawania ról, zobacz moje [wdrożenie bezpiecznego ASP.NET MVC 5 aplikacji sieci Web z członkostwem, uwierzytelnianiem OAuth i SQL Database w samouczku platformy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwem, uwierzytelnianiem OAuth i SQL Database na platformie Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Konfiguracja możliwość wypróbowania innowacyjnego](http://getglimpse.com/Docs/Configuration) — Strona doc na temat konfigurowania kart, zasad środowiska uruchomieniowego, rejestrowania i nie tylko.

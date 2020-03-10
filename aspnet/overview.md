---
uid: overview
title: Przegląd ASP.NET | Microsoft Docs
author: rick-anderson
description: Wprowadzenie do usługi ASP.NET — bezpłatnej platformy do tworzenia witryn internetowych, aplikacji sieci Web i interfejsów API sieci Web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 08/10/2019
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: aa4f627bca99f0a7ffbbb53ea45ebdcf0850fd89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537346"
---
# <a name="aspnet-overview"></a>Omówienie platformy ASP.NET

ASP.NET to bezpłatna platforma internetowa do tworzenia doskonałych witryn internetowych i aplikacji sieci Web przy użyciu języków HTML, CSS i JavaScript. Można również tworzyć interfejsy API sieci Web i używać technologii w czasie rzeczywistym, takich jak sieci Web.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) jest alternatywą dla ASP.NET.  Zapoznaj się ze [wskazówkami dotyczącymi wybierania między ASP.NET i ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Rozpoczynanie pracy

Zainstaluj [program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019) Community Edition, bezpłatne środowisko IDE dla ASP.NET w systemie Windows.

## <a name="websites-and-web-applications"></a>Witryny internetowe i aplikacje sieci Web

 ASP.NET oferuje trzy platformy do tworzenia aplikacji sieci Web: Web Forms, ASP.NET MVC i ASP.NET Web Pages. Wszystkie trzy platformy są stabilne i dojrzałe i można tworzyć wspaniałe aplikacje sieci Web przy użyciu dowolnego z nich. Bez względu na to, którą platformę wybierzesz, uzyskasz wszystkie korzyści i funkcje ASP.NET wszędzie.

Każda struktura jest przeznaczona dla innego stylu programowania. Wybór wybierany zależy od kombinacji zasobów programistycznych (wiedzy, umiejętności i środowiska programistycznego), typu tworzonej aplikacji oraz podejścia do programowania, z jakim jesteś.

Poniżej znajduje się przegląd poszczególnych platform oraz kilka pomysłów dotyczących wyboru między nimi. Jeśli wolisz wybrać film wideo, zobacz [Tworzenie witryn internetowych z ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) i [co to są narzędzia sieci Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Jeśli masz doświadczenie w programie | Styl programowania | Doświadczenie |
|-----------|----------------------|-----------------------------------------------------|----------------|
| Formularze sieci Web | Win Forms, WPF, .NET | Szybkie programowanie przy użyciu bogatej biblioteki formantów, która hermetyzuje znaczniki HTML | Średni poziom, zaawansowana RAD |
| MVC       | Język Ruby on-szyny, .NET  | Pełna kontrola nad adiustacją HTML, kodem i rozdzielonymi znacznikami oraz łatwym do pisania testami. Najlepszy wybór dla aplikacji mobilnych i jednostronicowych (SPA). | Na poziomie średniej, zaawansowane |
| Model Web Pages  | Klasyczne środowisko ASP, PHP     | Znaczniki HTML i kod w tym samym pliku | Nowy, średni poziom |

### <a name="web-forms"></a>Formularze sieci Web

Za pomocą formularzy sieci Web ASP.NET można tworzyć dynamiczne witryny internetowe przy użyciu znanego modelu typu "przeciągnij i upuść". Powierzchnia projektowa i setki kontrolek i składników pozwalają na szybkie tworzenie zaawansowanych, zaawansowanych witryn opartych na interfejsie użytkownika z dostępem do danych.

[Dowiedz się więcej o formularzach sieci Web](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC oferuje zaawansowany, oparty na wzorcach sposób tworzenia dynamicznych witryn sieci Web, które umożliwiają czyste rozdzielenie problemów i zapewnia pełną kontrolę nad adiustacjami, aby można było korzystać z programowania Agile. ASP.NET MVC zawiera wiele funkcji, które umożliwiają szybkie i niezrozumiałe Programowanie dla tworzenia zaawansowanych aplikacji, które korzystają z najnowszych standardów sieci Web.

[Dowiedz się więcej o MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Strony sieci Web programu ASP.NET

ASP.NET strony sieci Web i składnia Razor zapewniają szybki, efektywny i prosty sposób łączenia kodu serwera z HTML w celu utworzenia dynamicznej zawartości sieci Web. Łączenie z bazami danych, Dodawanie wideo, łączenie z witrynami sieci społecznościowych i zawiera wiele innych funkcji, które ułatwiają tworzenie atrakcyjnych witryn, które są zgodne z najnowszymi standardami sieci Web.

[Dowiedz się więcej o stronach sieci Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Uwagi dotyczące formularzy sieci Web, MVC i stron sieci Web

Wszystkie trzy platformy ASP.NET są oparte na .NET Framework i udostępniają podstawowe funkcje platformy .NET i ASP.NET. Na przykład wszystkie trzy struktury oferują model zabezpieczeń logowania na podstawie członkostwa, a wszystkie trzy współużytkują te same obiekty do zarządzania żądaniami, obsługi sesji i tak dalej, które są częścią podstawowych funkcji ASP.NET.

Ponadto trzy struktury nie są całkowicie niezależne i wybór jednego z nich nie wyklucza użycia innego. Ponieważ struktury mogą współistnieć w tej samej aplikacji sieci Web, nie jest rzadko widoczne poszczególne składniki aplikacji pisanych przy użyciu różnych platform. Na przykład składniki aplikacji dostępne dla klientów mogą być opracowywane w MVC, aby zoptymalizować znaczniki, podczas gdy dane dotyczące dostępu do danych i administracyjne są opracowywane w formularzach sieci Web w celu wykorzystania kontroli danych i prostego dostępu do danych.

## <a name="web-apis"></a>Interfejsy API sieci Web

Składnik Web API platformy ASP.NET to środowisko ułatwiające tworzenie usług HTTP, które można udostępniać dla wielu różnych klientów, takich jak przeglądarki i urządzenia przenośne. Składnik Web API platformy ASP.NET jest idealną platformą do tworzenia aplikacji o architekturze REST na platformie .NET Framework.

[Dowiedz się więcej o interfejsie API sieci Web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Technologie w czasie rzeczywistym

ASP.NET Signal to nowa biblioteka dla deweloperów ASP.NET, która ułatwia tworzenie funkcji sieci Web w czasie rzeczywistym. Usługa sygnalizująca umożliwia komunikację dwukierunkową między serwerem a klientem. Serwery mogą natychmiast wysyłać zawartość do podłączonych klientów, gdy stanie się ona dostępna. Program sygnalizujący obsługuje gniazda sieci Web i powraca do innych zgodnych technik dla starszych przeglądarek. Sygnalizujący obejmuje interfejsy API do zarządzania połączeniami (na przykład zdarzenia łączenia i rozłączania), grupowanie połączeń i autoryzację.

[Dowiedz się więcej o usłudze sygnalizującej](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Aplikacje mobilne i witryny

ASP.NET mogą korzystać z natywnych aplikacji mobilnych z zapleczem interfejsu API sieci Web, a także witryn sieci Web dla urządzeń przenośnych korzystających z platform projektowania, takich jak Bootstrap. Jeśli tworzysz natywną aplikację mobilną, możesz łatwo utworzyć interfejs API sieci Web oparty na notacji JSON, aby obsługiwać dostęp do danych, uwierzytelnianie i powiadomienia wypychane dla aplikacji. Jeśli tworzysz witrynę mobilną, możesz użyć dowolnej struktury CSS lub systemu otwartej siatki lub wybrać zaawansowany system mobilny, taki jak jQuery Mobile lub Sencha i wspaniałe aplikacje mobilne z PhoneGap.

[Dowiedz się więcej o tworzeniu aplikacji mobilnych i witrynach](mobile/overview.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Aplikacje jednostronicowe

Aplikacja jednostronicowa ASP.NET (SPA) ułatwia tworzenie aplikacji, które obejmują znaczące interakcje po stronie klienta przy użyciu języka HTML 5, CSS 3 i JavaScript. Program Visual Studio zawiera szablon służący do tworzenia aplikacji jednostronicowych przy użyciu narzędzia separowania. js i interfejsu API sieci Web ASP.NET. Oprócz wbudowanego szablonu SPA tworzone przez społeczność szablony SPA są również dostępne do pobrania.

[Dowiedz się więcej o tworzeniu aplikacji jednostronicowej](single-page-application/index.md)

## <a name="webhooks"></a>Elementy webhook

Elementy webhook to lekki wzorzec HTTP udostępniający prosty model pub/sub dla połączeń interfejsów API sieci Web i usług SaaS. Po wystąpieniu zdarzenia w usłudze powiadomienie jest wysyłane w formie żądania HTTP POST do zarejestrowanych subskrybentów. Żądanie POST zawiera informacje o zdarzeniu, które umożliwi odbiorcy odpowiednie działanie.

Elementy webhook są udostępniane przez dużą liczbę usług, w tym Dropbox, GitHub, usługi Instagram, MailChimp, PayPal, zapasowy, Trello i wiele innych. Na przykład element webhook może wskazywać, że plik został zmieniony w usłudze Dropbox lub że w usłudze GitHub została zatwierdzona zmiana kodu, lub Zainicjowanie płatności w systemie PayPal lub utworzenie karty w usłudze Trello.

[Dowiedz się więcej o elementach webhook](webhooks/index.md)

<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->

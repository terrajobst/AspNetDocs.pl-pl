---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Śledzenie informacji odwiedzających (analiza) dla witryny ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Po zakończeniu korzystania z witryny sieci Web warto przeanalizować ruch w witrynie sieci Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525187"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Śledzenie informacji odwiedzających (analiza) dla witryny ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób dodawania usługi Web Analytics do stron w witrynie sieci Web ASP.NET (Razor) przy użyciu pomocnika.
> 
> Zawartość:
> 
> - Jak wysyłać informacje o ruchu w witrynie sieci Web do dostawcy analizy.
> 
> Są to funkcje programowania ASP.NET wprowadzone w artykule:
> 
> - Pomocnik `Analytics`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - Biblioteka pomocników sieci Web ASP.NET (pakiet NuGet)

Analiza jest ogólnym terminem dla technologii, która mierzy ruch w witrynie sieci Web, dzięki czemu można zrozumieć, jak użytkownicy korzystają z tej witryny. Dostępnych jest wiele usług analitycznych, w tym usług firmy Google, Yahoo, StatCounter i innych.

Sposób działania analizy polega na zarejestrowaniu się w celu uzyskania konta u dostawcy analizy, w którym można zarejestrować lokację, którą chcesz śledzić. Dostawca wysyła fragment kodu JavaScript, który zawiera identyfikator lub kod śledzenia dla Twojego konta. Dodaj fragment kodu JavaScript do stron sieci Web w witrynie, która ma być śledzona. (Zazwyczaj można dodać fragment analityczny do stopki lub strony układu lub innego znacznika HTML, który pojawia się na każdej stronie w witrynie). Gdy użytkownicy zażądają strony zawierającej jeden z tych fragmentów kodu JavaScript, fragment kodu wyśle informacje o bieżącej stronie do dostawcy analitycznego, który rejestruje różne szczegółowe informacje o stronie.

Aby zapoznać się z statystyką lokacji, należy zalogować się do witryny sieci Web dostawcy analizy. Następnie można wyświetlić wszystkie rodzaje raportów o swojej witrynie, takie jak:

- Liczba wyświetleń stron dla poszczególnych stron. Oznacza to, że (w przybliżeniu) liczbę osób odwiedzających witrynę i strony, które są najbardziej popularne.
- Jak długo ludzie spędzają na określonych stronach. Może to pomóc w zapewnieniu, że Strona główna ma zainteresować osoby.
- Jakie lokacje znajdowały się przed odwiedzeniem Twojej witryny. Dzięki temu można zrozumieć, czy ruch pochodzi z linków, od wyszukiwań i tak dalej.
- Osoby odwiedzające witrynę i czas ich pobytu.
- Kraje, z których pochodzą odwiedzający.
- Jakich przeglądarek i systemów operacyjnych używają osoby odwiedzające.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Dodawanie analiz do strony przy użyciu pomocnika

ASP.NET strony sieci Web zawierają kilka pomocników analizy (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`i `Analytics.GetStatCounterHtml`), które ułatwiają zarządzanie fragmentami kodu JavaScript używanymi do analizy. Zamiast postanowić się, jak i gdzie umieścić kod JavaScript, wystarczy dodać pomocnika do strony. Jedyne informacje, które należy podać, to nazwa konta, identyfikator lub kod śledzenia. (W przypadku StatCounter należy również podać kilka dodatkowych wartości).

W tej procedurze utworzysz stronę układu, która używa pomocnika `GetGoogleHtml`. Jeśli masz już konto z innym dostawcą analitycznym, możesz użyć tego konta zamiast tego w razie konieczności wprowadzić niewielkie korekty.

> [!NOTE]
> Podczas tworzenia konta usługi Analytics należy zarejestrować adres URL witryny, która ma być śledzona. Jeśli testujesz wszystko na komputerze lokalnym, nie będziesz śledzić rzeczywistego ruchu (jedynym ruchem), więc nie będzie można rejestrować i wyświetlać statystyk lokacji. Jednak ta procedura pokazuje, jak dodać pomocnika analizy do strony. Gdy publikujesz swoją witrynę, na żywo witryna wyśle informacje do dostawcy analizy.

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny internetowej zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie została dodana.
2. Utwórz konto z usługą Google Analytics i Zapisz nazwę konta.
3. Utwórz stronę układu o nazwie *Analytics. cshtml* i Dodaj następujące znaczniki:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Należy umieścić wywołanie pomocnika `Analytics` w treści strony sieci Web (przed tagiem `</body>`). W przeciwnym razie przeglądarka nie uruchomi skryptu.

    Jeśli używasz innego dostawcy analitycznego, Użyj zamiast niego jednego z następujących pomocników:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Zastąp `myaccount` nazwą konta, identyfikatora lub kodu śledzenia utworzonego w kroku 1.
5. Uruchom stronę w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem).
6. W przeglądarce Wyświetl źródło strony. Będzie można zobaczyć renderowany kod analityczny:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Zaloguj się do witryny Google Analytics i sprawdź statystyki dla swojej witryny. Jeśli uruchamiasz stronę w aktywnej witrynie, zobaczysz wpis służący do rejestrowania odwiedzin na stronie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Witryna usługi Google Analytics](https://www.google.com/analytics/)
- [Witryna usługi Web Analytics dla usługi Yahoo!](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Witryna StatCounter](http://statcounter.com/)

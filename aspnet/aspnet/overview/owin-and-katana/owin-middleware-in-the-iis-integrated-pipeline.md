---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: OWIN oprogramowanie pośredniczące w zintegrowanym potoku usług IIS | Microsoft Docs
author: Praburaj
description: W tym artykule pokazano, jak uruchamiać składniki pośredniczące OWIN (OMCs) w zintegrowanym potoku usług IIS oraz jak ustawić zdarzenie potoku, w którym jest uruchamiane OMC. Należy...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 7d157fb6bd9e2ae9b55af41ef06c1eb5e6310ce1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617139"
---
# <a name="owin-middleware-in-the-iis-integrated-pipeline"></a>OWIN oprogramowanie pośredniczące w zintegrowanym potoku usług IIS

Autor [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym artykule pokazano, jak uruchamiać składniki pośredniczące OWIN (OMCs) w zintegrowanym potoku usług IIS oraz jak ustawić zdarzenie potoku, w którym jest uruchamiane OMC. Przed przeczytaniem tego samouczka należy zapoznać [się z omówieniem](an-overview-of-project-katana.md) [wykrywania klasy](owin-startup-class-detection.md) projektu Katana i Owin. Ten samouczek został utworzony przez Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Krzysztof Ross, Praburaj Thiagarajan i Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).

Chociaż składniki pośredniczące [Owin](an-overview-of-project-katana.md) (OMCs) są przeznaczone głównie do uruchamiania w potoku serwer-niezależny od, można również uruchomić OMC w zintegrowanym potoku usług IIS (**Tryb klasyczny *nie* jest obsługiwany**). OMC można wykonać w celu pracy w zintegrowanym potoku usług IIS, instalując następujący pakiet z konsoli Menedżera pakietów (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Oznacza to, że wszystkie struktury aplikacji, nawet te, które nie są jeszcze uruchomione poza usługami IIS i system. Web, mogą korzystać z istniejących składników oprogramowania pośredniczącego OWIN. 

> [!NOTE]
> Wszystkie pakiety `Microsoft.Owin.Security.*` wysyłki przy użyciu nowego systemu tożsamości w Visual Studio 2013 (na przykład: pliki cookie, konta Microsoft, Google, Facebook, Twitter, [token](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html)osoby podpisującej, uwierzytelniania OAuth, serwera autoryzacji, tokenu JWT, usługi Azure Active Directory i usług federacyjnych Active Directory) są tworzone jako OMCs i mogą być używane w scenariuszach hostowanych przez aplikacje samodzielne i IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Jak działa oprogramowanie pośredniczące OWIN w zintegrowanym potoku usług IIS

W przypadku aplikacji konsolowych OWIN potok aplikacji zbudowany przy użyciu [konfiguracji uruchamiania](owin-startup-class-detection.md) jest ustawiany w kolejności, w jakiej składniki są dodawane za pomocą metody `IAppBuilder.Use`. Oznacza to, że potok OWIN w środowisku uruchomieniowym [Katana](an-overview-of-project-katana.md) przetwarza OMCs w kolejności, w jakiej zostały zarejestrowane przy użyciu `IAppBuilder.Use`. W zintegrowanym potoku usług IIS potok żądań składa się z elementu [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) subskrybowanego ze wstępnie zdefiniowanym zestawem zdarzeń potoku, takich jak [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)itd.

W przypadku porównania OMC z tym modułem [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) w świecie ASP.NET, OMC musi być zarejestrowany w poprawnym wstępnie zdefiniowanym zdarzeniu potoku. Na przykład `MyModule` HttpModule zostanie wywołane, gdy żądanie znajdzie się na etapie [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) w potoku:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Aby OMC uczestniczył w tej samej kolejności wykonywania opartej na zdarzeniach, kod środowiska uruchomieniowego [Katana](an-overview-of-project-katana.md) skanuje się za pośrednictwem [konfiguracji uruchamiania](owin-startup-class-detection.md) i subskrybuje poszczególne składniki oprogramowania pośredniczącego w ramach zintegrowanego zdarzenia potoku. Na przykład poniższy kod rejestracyjny OMC i Registration umożliwia wyświetlenie domyślnej rejestracji zdarzeń komponentów oprogramowania pośredniczącego. (Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia klasy startowej OWIN, zobacz [wykrywanie klasy początkowej Owin](owin-startup-class-detection.md)).

1. Utwórz pusty projekt aplikacji sieci Web i nadaj mu nazwę **owin2**.
2. W konsoli Menedżera pakietów (PMC) Uruchom następujące polecenie: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Dodaj `OWIN Startup Class` i nadaj jej nazwę `Startup`. Zamień wygenerowany kod na następujący (zmiany są wyróżnione):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Naciśnij klawisz F5, aby uruchomić aplikację.

Konfiguracja uruchamiania konfiguruje potok z trzema składnikami pośredniczącymi, pierwsze dwa wyświetlania informacji diagnostycznych i ostatnią odpowiedzią na zdarzenia (a także wyświetlaniem informacji diagnostycznych). Metoda `PrintCurrentIntegratedPipelineStage` wyświetla zintegrowane zdarzenie potoku, w którym jest wywoływana oprogramowanie pośredniczące i komunikat. W oknach danych wyjściowych są wyświetlane następujące elementy:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Środowisko uruchomieniowe Katana zamapować każdy składnik oprogramowania pośredniczącego OWIN na [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) domyślnie, który odpowiada [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)zdarzeń potoku usług IIS.

## <a name="stage-markers"></a>Znaczniki etapu

Można oznaczyć OMCs do wykonania na określonych etapach potoku przy użyciu metody rozszerzenia `IAppBuilder UseStageMarker()`. Aby uruchomić zestaw komponentów oprogramowania pośredniczącego na określonym etapie, Wstaw znacznik etapu bezpośrednio po ostatnim składniku jest ustawiony podczas rejestracji. Istnieją reguły, na których etapie potoku można wykonywać oprogramowanie pośredniczące, a składniki zamówienia muszą zostać uruchomione (reguły są wyjaśnione w dalszej części tego samouczka). Dodaj metodę `UseStageMarker` do kodu `Configuration`, jak pokazano poniżej:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

Wywołanie `app.UseStageMarker(PipelineStage.Authenticate)` konfiguruje wszystkie zarejestrowane wcześniej składniki pośredniczące (w tym przypadku nasze dwa składniki diagnostyczne) do uruchomienia na etapie uwierzytelniania potoku. Ostatni składnik pośredniczący (który wyświetla diagnostykę i reaguje na żądania) zostanie uruchomiony na etapie `ResolveCache` (zdarzenie [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) ).

Naciśnij klawisz F5, aby uruchomić aplikację. W oknie dane wyjściowe są wyświetlane następujące elementy:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Reguły znaczników etapu

Składniki pośredniczące Owin (OMC) można skonfigurować tak, aby były uruchamiane w następujących zdarzeniach etapów potoku OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Domyślnie OMCs jest uruchamiany przy ostatnim zdarzeniu (`PreHandlerExecute`). Dlatego nasz pierwszy przykładowy kod jest wyświetlany jako "PreExecuteRequestHandler".
2. Można użyć metody `app.UseStageMarker`, aby zarejestrować OMC do uruchamiania wcześniej, na dowolnym etapie potoku OWIN na liście `PipelineStage` enum.
3. Potok OWIN i potok usług IIS są uporządkowane, dlatego wywołania do `app.UseStageMarker` muszą być w kolejności. Nie można ustawić obsługi zdarzeń przed ostatnim zdarzeniem zarejestrowanym w usłudze do `app.UseStageMarker`. Na przykład *po* wywołaniu:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   wywołania `app.UseStageMarker` przekazywanie `Authenticate` lub `PostAuthenticate` nie będą honorowane i żaden wyjątek nie zostanie wygenerowany. OMCs jest uruchamiany na najnowszym etapie, który domyślnie jest `PreHandlerExecute`. Znaczniki etapów są używane do uruchamiania ich wcześniej. Jeśli określisz znaczniki etapu poza kolejnością, zaokrąglimy do wcześniejszego znacznika. Innymi słowy dodanie znacznika etapu ma wartość "Uruchom nie później niż etap X". OMC jest uruchamiany na najwcześniejszym znaczniku etapu dodanym po nim w potoku OWIN.
4. Najwcześniejszy etap wywołań do `app.UseStageMarker` usługi WINS. Jeśli na przykład przełączysz kolejność `app.UseStageMarker` wywołań z poprzedniego przykładu:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Zostanie wyświetlone okno dane wyjściowe: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   OMCs wszystkie uruchomione na etapie `AuthenticateRequest`, ponieważ ostatnie OMC zarejestrowane w zdarzeniu `Authenticate` i zdarzenie `Authenticate` poprzedza wszystkie inne zdarzenia.

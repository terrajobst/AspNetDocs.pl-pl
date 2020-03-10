---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Śledzenie w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Pokazuje, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598554"
---
# <a name="tracing-in-aspnet-web-api-2"></a>Śledzenie w programie ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

> Podczas próby debugowania aplikacji sieci Web nie ma znaczenia dla dobrego zestawu dzienników śledzenia. W tym samouczku pokazano, jak włączyć śledzenie w interfejsie API sieci Web ASP.NET. Za pomocą tej funkcji można śledzić działanie platformy internetowego interfejsu API przed wywołaniem kontrolera i po nim. Można go również użyć do śledzenia własnego kodu.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
> - [Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (działa również z programem visual Studio 2015)
> - Internetowy interfejs API 2
> - [Microsoft. AspNet. WebApi. Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Włącz śledzenie system. Diagnostics w interfejsie Web API

Najpierw utworzymy nowy projekt aplikacji sieci Web ASP.NET. W programie Visual Studio w menu **plik** wybierz pozycję **Nowy** **projekt** > . W obszarze **Szablony**, **Sieć Web**wybierz pozycję **aplikacja sieci Web ASP.NET**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Wybierz szablon projektu interfejsu API sieci Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie polecenie **Zarządzanie pakietem**.

W oknie Konsola Menedżera pakietów wpisz następujące polecenia.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Pierwsze polecenie instaluje najnowszy pakiet śledzenia interfejsu API sieci Web. Aktualizuje również podstawowe pakiety interfejsów API sieci Web. Drugie polecenie aktualizuje pakiet WebApi. WebHost do najnowszej wersji.

> [!NOTE]
> Jeśli chcesz wskazać określoną wersję interfejsu API sieci Web, Użyj flagi-Version podczas instalacji pakietu śledzenia.

Otwórz plik WebApiConfig.cs w folderze Start\_. Dodaj następujący kod do metody **register** .

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Ten kod dodaje klasę [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) do potoku interfejsu API sieci Web. Klasa **SystemDiagnosticsTraceWriter** zapisuje ślady do [System. Diagnostics. Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Aby wyświetlić ślady, uruchom aplikację w debugerze. W przeglądarce przejdź do strony `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Instrukcje śledzenia są zapisywane w oknie danych wyjściowych w programie Visual Studio. (Z menu **Widok** wybierz pozycję **dane wyjściowe**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Ponieważ **SystemDiagnosticsTraceWriter** zapisuje ślady do **System. Diagnostics. Trace**, można zarejestrować dodatkowe detektory śledzenia; na przykład, aby zapisać ślady w pliku dziennika. Aby uzyskać więcej informacji na temat składników zapisywania śladów, zobacz temat [detektory śledzenia](https://msdn.microsoft.com/library/4y5y10s7.aspx) w witrynie MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurowanie SystemDiagnosticsTraceWriter

Poniższy kod przedstawia sposób konfigurowania składnika zapisywania śledzenia.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Istnieją dwa ustawienia, które można kontrolować:

- Isverbose: w przypadku wartości false każdy ślad zawiera minimalną ilość informacji. Jeśli wartość jest równa true, ślady zawierają więcej informacji.
- MinimumLevel: ustawia minimalny poziom śledzenia. Poziomy śledzenia, w kolejności, są debugowaniem, informacjami, ostrzeżeniem, błędem i krytycznym.

## <a name="adding-traces-to-your-web-api-application"></a>Dodawanie śladów do aplikacji internetowego interfejsu API

Dodanie składnika zapisywania śledzenia daje natychmiastowy dostęp do śladów utworzonych przez potok interfejsu API sieci Web. Możesz również użyć składnika zapisywania śledzenia do śledzenia własnego kodu:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Aby uzyskać składnik zapisywania śledzenia, wywołaj **HttpConfiguration. Services. GetTraceWriter**. Z poziomu kontrolera ta metoda jest dostępna za pośrednictwem właściwości **ApiController. Configuration** .

Aby napisać ślad, można wywołać metodę **ITraceWriter. Trace** , ale Klasa [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) definiuje niektóre metody rozszerzające, które są bardziej przyjazne. Na przykład pokazana powyżej Metoda **info** tworzy ślad z **informacjami**o poziomie śledzenia.

## <a name="web-api-tracing-infrastructure"></a>Infrastruktura śledzenia interfejsu API sieci Web

W tej sekcji opisano sposób pisania niestandardowego składnika zapisywania śledzenia dla internetowego interfejsu API.

Pakiet Microsoft. AspNet. WebApi. Tracing został utworzony na podstawie bardziej ogólnej infrastruktury śledzenia w interfejsie API sieci Web. Zamiast używać elementu Microsoft. AspNet. WebApi. Tracing, można również podłączyć inne biblioteki śledzenia/rejestrowania, takie jak [nLOG](http://nlog-project.org/) lub [log4net](http://logging.apache.org/log4net/).

Aby zebrać ślady, zaimplementuj Interfejs **ITraceWriter** . Oto prosty przykład:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

Metoda **ITraceWriter. Trace** tworzy ślad. Obiekt wywołujący określa kategorię i poziom śledzenia. Kategoria może być dowolnym ciągiem zdefiniowanym przez użytkownika. W ramach implementacji **śledzenia** należy wykonać następujące czynności:

1. Utwórz nowy **TraceRecord**. Zainicjuj go przy użyciu żądania, kategorii i poziomu śledzenia, jak pokazano poniżej. Te wartości są udostępniane przez obiekt wywołujący.
2. Wywołaj delegata *traceAction* . Wewnątrz tego delegata oczekuje się, że obiekt wywołujący będzie wypełniał resztę **TraceRecord**.
3. Napisz **TraceRecord**przy użyciu dowolnej techniki rejestrowania. W przykładzie pokazano po prostu wywołania do **System. Diagnostics. Trace**.

## <a name="setting-the-trace-writer"></a>Ustawianie składnika zapisywania śledzenia

Aby włączyć śledzenie, należy skonfigurować internetowy interfejs API do korzystania z implementacji **ITraceWriter** . Można to zrobić za pomocą obiektu **HttpConfiguration** , jak pokazano w poniższym kodzie:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Tylko jeden składnik zapisywania śledzenia może być aktywny. Domyślnie interfejs API sieci Web ustawia &quot;No-op&quot; Tracer, który nic nie robi. (&quot;No-op&quot; Tracer istnieje, tak że kod śledzenia nie musi sprawdzać, czy moduł zapisujący śledzenia ma **wartość null** przed zapisaniem śladu.)

## <a name="how-web-api-tracing-works"></a>Jak działa śledzenie interfejsu API sieci Web

Śledzenie w interfejsie API sieci Web używa wzorca *elewacji* : gdy śledzenie jest włączone, interfejs API sieci Web otacza różne części potoku żądań z klasami, które wykonują wywołania śledzenia.

Na przykład podczas wybierania kontrolera potok używa interfejsu **IHttpControllerSelector** . Po włączeniu śledzenia potok wstawia klasę implementującą **IHttpControllerSelector** , ale wywołuje do rzeczywistej implementacji:

![Śledzenie interfejsu API sieci Web używa wzorca elewacji.](tracing-in-aspnet-web-api/_static/image8.png)

Korzyści wynikające z tego projektu obejmują:

- Jeśli nie dodasz składnika zapisywania śladów, składniki śledzenia nie są tworzone i nie mają wpływu na wydajność.
- Jeśli zastąpisz domyślne usługi, takie jak **IHttpControllerSelector** z własną implementacją niestandardową, nie będzie to miało wpływ na śledzenie, ponieważ śledzenie jest wykonywane przez obiekt otoki.

Możesz również zastąpić całą strukturę śledzenia interfejsu API sieci Web własnymi platformami niestandardowymi, zastępując domyślną usługę **ITraceManager** :

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Zaimplementuj **ITraceManager. Initialize** , aby zainicjować system śledzenia. Należy pamiętać, że spowoduje to zastąpienie *całej* struktury śledzenia, w tym wszystkich kodów śledzenia wbudowanych w interfejs API sieci Web.

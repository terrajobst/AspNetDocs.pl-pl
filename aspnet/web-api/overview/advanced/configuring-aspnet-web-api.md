---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Konfigurowanie ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: 'Skonfiguruj ASP.NET Web API 2 dla ASP.NET 4. x: Skonfiguruj ustawienia, ASP.NET w wersji 4. x hosting, OWIN własne, usługi globalne i konfigurację przed kontrolerem.'
ms.author: riande
ms.date: 03/31/2014
ms.custom: seoapril2019
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4f76728fa5e4602e35e1b7cb2d41b2245093cad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557723"
---
# <a name="configuring-aspnet-web-api-2"></a>Konfigurowanie interfejsu Web API ASP.NET 2

według [Jan Wasson](https://github.com/MikeWasson)

W tym temacie opisano sposób konfigurowania interfejsu API sieci Web ASP.NET.

- [Ustawienia konfiguracji](#settings)
- [Konfigurowanie interfejsu API sieci Web z hostingiem ASP.NET](#webhost)
- [Konfigurowanie interfejsu API sieci Web za pomocą samoobsługowego OWIN](#selfhost)
- [Globalne usługi interfejsu API sieci Web](#services)
- [Konfiguracja na kontroler](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Ustawienia konfiguracji

Ustawienia konfiguracji interfejsu API sieci Web są zdefiniowane w klasie [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) .

| Członek | Opis |
| --- | --- |
| **DependencyResolver** | Włącza iniekcję zależności dla kontrolerów. Zobacz [Korzystanie z programu rozpoznawania zależności interfejsu API sieci Web](dependency-injection.md). |
| **Filtry** | Filtry akcji. |
| **Elementy formatujące** | Elementy [formatujące typy multimediów](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Określa, czy serwer powinien zawierać szczegóły błędu, takie jak komunikaty o wyjątkach i ślady stosu, w komunikatach odpowiedzi HTTP. Zobacz [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Skład** | Funkcja, która wykonuje ostateczne inicjowanie **HttpConfiguration**. |
| **MessageHandlers** | [Procedury obsługi komunikatów http](http-message-handlers.md). |
| **ParameterBindingRules** | Kolekcja reguł dla parametrów powiązań w akcjach kontrolera. |
| **Właściwości** | Ogólny zbiór właściwości. |
| **Trasy** | Kolekcja tras. Zobacz [Routing w interfejsie API sieci Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Usługi** | Kolekcja usług. Zobacz [usługi](#services). |

## <a name="prerequisites"></a>Wymagania wstępne

[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017), wersja Community, Professional lub Enterprise.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Konfigurowanie interfejsu API sieci Web z hostingiem ASP.NET

W aplikacji ASP.NET Skonfiguruj internetowy interfejs API, wywołując [GlobalConfiguration. configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) w metodzie **Start\_aplikacji** . Metoda **Configure** przyjmuje delegata z pojedynczym parametrem typu **HttpConfiguration**. Wykonaj całą konfigurację w ramach delegata.

Oto przykład użycia anonimowego delegata:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

W programie Visual Studio 2017 szablon projektu "aplikacja sieci Web ASP.NET" automatycznie konfiguruje kod konfiguracji, w przypadku wybrania opcji "Web API" w oknie dialogowym **Nowy projekt ASP.NET** .

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Szablon projektu tworzy plik o nazwie WebApiConfig.cs w folderze Start\_. Ten plik kodu definiuje delegata, w którym należy umieścić kod konfiguracji interfejsu API sieci Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Szablon projektu dodaje również kod, który wywołuje delegata z **aplikacji\_Start**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Konfigurowanie interfejsu API sieci Web za pomocą samoobsługowego OWIN

Jeśli jesteś własnym programem OWIN, Utwórz nowe wystąpienie usługi **HttpConfiguration** . Wykonaj dowolną konfigurację w tym wystąpieniu, a następnie Przekaż wystąpienie do metody rozszerzenia **Owin. UseWebApi** .

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

W samouczku [użyto Owin do samoobsługowego hostowania ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) . przedstawiono tu kompletną procedurę.

<a id="services"></a>
## <a name="global-web-api-services"></a>Globalne usługi interfejsu API sieci Web

Kolekcja **HttpConfiguration. Services** zawiera zestaw usług globalnych używanych przez interfejs API sieci Web do wykonywania różnych zadań, takich jak wybór kontrolera i negocjowanie zawartości.

> [!NOTE]
> Kolekcja **usług** nie jest mechanizmem ogólnego przeznaczenia do odnajdowania usług lub iniekcji zależności. Przechowuje tylko typy usług znane dla struktury internetowego interfejsu API.

Kolekcja **usług** jest inicjowana przy użyciu domyślnego zestawu usług i można zapewnić własne implementacje. Niektóre usługi obsługują wiele wystąpień, a inne mogą mieć tylko jedno wystąpienie. (Jednak można również udostępnić usługi na poziomie kontrolera; zobacz [Konfiguracja dla kontrolera](#percontrollerconfig).

Usługi pojedynczego wystąpienia

| NDES | Opis |
| --- | --- |
| **IActionValueBinder** | Pobiera powiązanie dla parametru. |
| **IApiExplorer** | Pobiera opisy interfejsów API uwidocznionych przez aplikację. Zobacz [Tworzenie strony pomocy dla internetowego interfejsu API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Pobiera listę zestawów dla aplikacji. Zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Weryfikuje model odczytywany z treści żądania przez program formatujący typ nośnika. |
| **IContentNegotiator** | Przeprowadza negocjowanie zawartości. |
| **IDocumentationProvider** | Zawiera dokumentację dla interfejsów API. Wartość domyślna to **null**. Zobacz [Tworzenie strony pomocy dla internetowego interfejsu API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Wskazuje, czy host powinien buforować treści jednostki komunikatów HTTP. |
| **IHttpActionInvoker** | Wywołuje akcję kontrolera. Zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Wybiera akcję kontrolera. Zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Aktywuje kontroler. Zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Wybiera kontroler. Zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Zawiera listę typów kontrolerów interfejsu API sieci Web w aplikacji. Zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicjuje strukturę śledzenia. Zobacz [Śledzenie w interfejsie API sieci Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Udostępnia moduł zapisujący śledzenia. Wartość domyślna to moduł zapisujący śledzenia "No-op". Zobacz [Śledzenie w interfejsie API sieci Web ASP.NET](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Udostępnia pamięć podręczną modułów walidacji modelu. |

Usługi o wielu wystąpieniach

|                 NDES                 |                                                                                                              Opis                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Zwraca listę filtrów dla akcji kontrolera.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Zwraca spinacz modelu dla danego typu.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Zawiera metadane dla modelu.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Udostępnia moduł sprawdzania poprawności dla modelu.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Tworzy dostawcę wartości. Aby uzyskać więcej informacji, zobacz wpis w blogu Jan, [jak utworzyć niestandardowego dostawcę wartości w WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Aby dodać implementację niestandardową do usługi o kilku wystąpieniach, wywołaj **Dodawanie** lub **Wstawianie** do kolekcji **usług** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Aby zamienić usługę pojedynczego wystąpienia z implementacją niestandardową, wywołaj polecenie **Zamień** w kolekcji **usług** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Konfiguracja na kontroler

Poniższe ustawienia można zastąpić dla poszczególnych kontrolerów:

- Elementy formatujące typy multimediów
- Reguły powiązań parametrów
- Usługi

W tym celu należy zdefiniować atrybut niestandardowy, który implementuje interfejs **IControllerConfiguration** . Następnie zastosuj atrybut do kontrolera.

W poniższym przykładzie zastępują domyślne elementy formatujące typ nośnika przy użyciu niestandardowego programu formatującego.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Metoda **IControllerConfiguration. Initialize** przyjmuje dwa parametry:

- Obiekt **HttpControllerSettings**
- Obiekt **HttpControllerDescriptor**

**HttpControllerDescriptor** zawiera opis kontrolera, który można przejrzeć w celach informacyjnych (powiedzmy, aby rozróżnić dwa kontrolery).

Użyj obiektu **HttpControllerSettings** , aby skonfigurować kontroler. Ten obiekt zawiera podzestaw parametrów konfiguracji, które można przesłonić dla poszczególnych kontrolerów. Wszystkie ustawienia, które nie są zmieniane domyślnie na globalny obiekt **HttpConfiguration** .

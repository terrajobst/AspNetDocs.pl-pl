---
uid: web-api/overview/error-handling/exception-handling
title: Obsługa wyjątków w interfejsie Web API ASP.NET — ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622319"
---
# <a name="exception-handling-in-aspnet-web-api"></a>Obsługa wyjątków w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

W tym artykule opisano obsługę błędów i wyjątków w interfejsie API sieci Web ASP.NET.

- [HttpResponseException](#httpresponserexception)
- [Filtry wyjątków](#exception_filters)
- [Rejestrowanie filtrów wyjątków](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Co się stanie, Jeśli kontroler internetowego interfejsu API zgłasza nieprzechwycony wyjątek? Domyślnie większość wyjątków jest tłumaczona na odpowiedź HTTP z kodem stanu 500, wewnętrzny błąd serwera.

Typ **HttpResponseException** jest szczególnym przypadkiem. Ten wyjątek zwraca kod stanu HTTP określony w konstruktorze wyjątków. Na przykład następująca metoda zwraca 404, nie znaleziono, jeśli parametr *ID* jest nieprawidłowy.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Aby uzyskać większą kontrolę nad odpowiedzią, można również utworzyć cały komunikat odpowiedzi i dołączyć go do **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtry wyjątków

Możesz dostosować sposób, w jaki interfejs API sieci Web obsługuje wyjątki, pisząc *Filtr wyjątków*. Filtr wyjątku jest wykonywany, gdy metoda kontrolera zgłasza dowolny nieobsługiwany wyjątek, który *nie* jest wyjątkiem **HttpResponseException** . Typ **HttpResponseException** jest szczególnym przypadkiem, ponieważ został zaprojektowany specjalnie w celu ZWRÓCENIA odpowiedzi HTTP.

Filtry wyjątków implementują interfejs **System. Web. http. filters. IExceptionFilter** . Najprostszym sposobem pisania filtru wyjątków jest uzyskanie z klasy **System. Web. http. filters. ExceptionFilterAttribute** i zastąpienie metody **onException** .

> [!NOTE]
> Filtry wyjątków w interfejsie Web API ASP.NET są podobne do tych w ASP.NET MVC. Jednak są one deklarowane w odrębnej przestrzeni nazw i funkcji oddzielnie. W szczególności Klasa **HandleErrorAttribute** używana w MVC nie obsługuje wyjątków zgłoszonych przez kontrolery interfejsu API sieci Web.

Poniżej znajduje się filtr, który konwertuje **NotImplementedException** wyjątków na kod stanu HTTP 501, nie zaimplementowane:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

Właściwość **Response** obiektu **HttpActionExecutedContext** zawiera komunikat odpowiedzi HTTP, który zostanie wysłany do klienta.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Rejestrowanie filtrów wyjątków

Istnieje kilka sposobów rejestrowania filtru wyjątków interfejsu API sieci Web:

- Według akcji
- Według kontrolera
- globalnie

Aby zastosować filtr do określonej akcji, Dodaj filtr jako atrybut do akcji:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Aby zastosować filtr do wszystkich akcji na kontrolerze, Dodaj filtr jako atrybut do klasy kontrolera:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Aby zastosować filtr globalnie do wszystkich kontrolerów interfejsu API sieci Web, Dodaj wystąpienie filtru do kolekcji **GlobalConfiguration. Configuration. filters** . Filtry wyjątków w tej kolekcji mają zastosowanie do dowolnej akcji kontrolera interfejsu API sieci Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Jeśli tworzysz projekt przy użyciu szablonu projektu "aplikacja sieci Web ASP.NET MVC 4", umieść kod konfiguracji interfejsu API sieci Web wewnątrz klasy `WebApiConfig`, która znajduje się w aplikacji\_folderze początkowym:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

Obiekt **HttpError** zapewnia spójny sposób zwrócenia informacji o błędach w treści odpowiedzi. Poniższy przykład pokazuje, jak zwrócić kod stanu HTTP 404 (nie znaleziono) z **HttpError** w treści odpowiedzi.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** to metoda rozszerzająca zdefiniowana w klasie **System .NET. http. HttpRequestMessageExtensions** . Wewnętrznie, **CreateErrorResponse** tworzy wystąpienie **HttpError** , a następnie tworzy **HttpResponseMessage** , który zawiera **HttpError**.

W tym przykładzie, jeśli metoda zakończy się pomyślnie, zwraca produkt w odpowiedzi HTTP. Ale jeśli żądany produkt nie zostanie znaleziony, odpowiedź HTTP zawiera **HttpError** w treści żądania. Odpowiedź może wyglądać następująco:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Zwróć uwagę, że w tym przykładzie **HttpError** został Zserializowany do formatu JSON. Jedną z zalet korzystania z **HttpError** jest to, że przechodzą przez ten sam proces [negocjacji zawartości](../formats-and-model-binding/content-negotiation.md) i serializacji co inny model o jednoznacznie określonym typie.

### <a name="httperror-and-model-validation"></a>HttpError i walidacja modelu

Aby sprawdzić poprawność modelu, można przekazać stan modelu do **CreateErrorResponse**, aby uwzględnić błędy walidacji w odpowiedzi:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Ten przykład może zwrócić następujące odpowiedzi:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Aby uzyskać więcej informacji na temat weryfikacji modelu, zobacz [Walidacja modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Używanie HttpError z HttpResponseException

Poprzednie przykłady zwracają komunikat **HttpResponseMessage** z akcji kontrolera, ale można również użyć **HttpResponseException** do zwrócenia **HttpError**. Dzięki temu można zwrócić silnie wpisaną model w normalnym przypadku sukcesu, ale nadal zwraca **HttpError** , jeśli wystąpi błąd:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]

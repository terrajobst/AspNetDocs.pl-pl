---
title: Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnie typizowane oprogramowanie pośredniczące oparte na fabryce aktywacji i kontenerem innych firm w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2018
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 6af775c66a1de7f1a4f06a4a639ade20c6493b2a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57066503"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Oprogramowanie pośredniczące aktywacji z kontenerem innych firm w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W tym artykule przedstawiono sposób użycia [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) i [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jako punkt rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji z kontenerem innej firmy. Aby uzyskać wprowadzające informacje na `IMiddlewareFactory` i `IMiddleware`, zobacz [aktywacji oprogramowanie pośredniczące oparte na fabryce](xref:fundamentals/middleware/extensibility) tematu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

Przykładowa aplikacja pokazuje, oprogramowanie pośredniczące aktywacji przez `IMiddlewareFactory` implementacji `SimpleInjectorMiddlewareFactory`. W przykładzie użyto [proste iniektor](https://simpleinjector.org) kontenera (DI) iniekcji zależności.

Przykład oprogramowania pośredniczącego implementacji rejestruje wartość dostarczona przez parametr ciągu zapytania (`key`). Oprogramowanie pośredniczące używa kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.

> [!NOTE]
> Ta aplikacja używa przykładowych [proste iniektor](https://github.com/simpleinjector/SimpleInjector) wyłącznie w celach demonstracyjnych. Korzystanie z prostych iniektor nie potwierdzenia. Metody aktywacji oprogramowania pośredniczącego opisanej w dokumentacji iniektor proste i problemy usługi GitHub są zalecane przez maintainers z prostego iniektor. Aby uzyskać więcej informacji, zobacz [dokumentacji proste iniektor](https://simpleinjector.readthedocs.io/en/latest/index.html) i [repozytorium GitHub usługi Simple iniektor](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego.

W przykładowej aplikacji fabrykę oprogramowanie pośredniczące jest implementowane w celu tworzenia `SimpleInjectorActivatedMiddleware` wystąpienia. Fabryka oprogramowania pośredniczącego używa iniektor prosty kontener, aby rozwiązać oprogramowania pośredniczącego:

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji.

Oprogramowanie pośredniczące aktywowany przez `IMiddlewareFactory` implementacji (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Rozszerzenie jest tworzona dla oprogramowania pośredniczącego (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` należy wykonać kilka zadań:

* Skonfiguruj kontener iniektor proste.
* Zarejestruj fabryki i oprogramowanie pośredniczące.
* Udostępnia kontekst bazy danych aplikacji z kontenera proste iniektor strony Razor.

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

Oprogramowanie pośredniczące jest zarejestrowany w potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Oprogramowanie pośredniczące oparte na fabryce aktywacji](xref:fundamentals/middleware/extensibility)
* [Proste repozytorium GitHub iniektor](https://github.com/simpleinjector/SimpleInjector)
* [Proste iniektor dokumentacji](https://simpleinjector.readthedocs.io/en/latest/index.html)

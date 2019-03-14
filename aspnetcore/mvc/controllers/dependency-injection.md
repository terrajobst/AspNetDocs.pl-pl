---
title: Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak kontrolerów platformy ASP.NET Core MVC zażądać ich zależności, które jawnie za pomocą ich konstruktory mogę przy użyciu iniekcji zależności w programie ASP.NET Core.
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 898e98f4c5d472ca96c6a8ad07dddd1a4ef54fe9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077855"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>Wstrzykiwanie zależności do kontrolerów w programie ASP.NET Core

<a name="dependency-injection-controllers"></a>

Przez [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), i [Steve Smith](https://github.com/ardalis)

Kontrolerów MVC platformy ASP.NET Core żądania zależności jawnie za pomocą konstruktorów. Platforma ASP.NET Core ma wbudowaną obsługę [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). DI ułatwia aplikacji do testowania i obsługi.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="constructor-injection"></a>Iniekcji konstruktora

Usługi są dodawane jako parametr konstruktora i środowiska uruchomieniowego jest rozpoznawana jako usługa z kontenera usług. Usługi są zazwyczaj zdefiniowane przy użyciu interfejsów. Rozważmy na przykład aplikacji, która wymaga bieżący czas. Następujące interfejsu ujawnia `IDateTime` usługi:

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

Poniższy kod implementuje `IDateTime` interfejsu:

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

Dodaj usługę do kontenera usługi:

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

Aby uzyskać więcej informacji na temat <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, zobacz [okresy istnienia usługi DI](xref:fundamentals/dependency-injection#service-lifetimes).

Poniższy kod wyświetla pozdrowienia dla użytkownika na podstawie czasu dnia:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

Uruchom aplikację i zostanie wyświetlony komunikat na podstawie czasu.

## <a name="action-injection-with-fromservices"></a>Akcja iniekcja FromServices

<xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> Umożliwia wstawianie usługi bezpośrednio do metody akcji bez przy użyciu iniekcji konstruktora:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a>Ustawienia dostępu za pomocą kontrolera

Uzyskiwanie dostępu do aplikacji lub konfiguracji ustawień z poziomu kontrolera jest typowy wzorzec. *Wzorzec opcje* opisanego w <xref:fundamentals/configuration/options> jest preferowanym podejściem do zarządzania ustawieniami. Ogólnie rzecz biorąc, bezpośrednio nie wstrzyknąć <xref:Microsoft.Extensions.Configuration.IConfiguration> w kontroler.

Utwórz klasę, która reprezentuje opcje. Na przykład:

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

Dodaj klasę konfiguracji do kolekcji usługi:

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

Konfigurowanie aplikacji do odczytywania ustawień z pliku w formacie JSON:

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

Poniższy kod żądania `IOptions<SampleWebSettings>` ustawienia z kontenera usługi i używa ich w `Index` metody:

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* Zobacz <xref:mvc/controllers/testing> dowiesz się, jak ułatwiają kodu do przetestowania przez jawne żądanie zależności w kontrolerach.

* [Zamień domyślny kontener iniekcji zależności implementacji firm](xref:fundamentals/dependency-injection#default-service-container-replacement).

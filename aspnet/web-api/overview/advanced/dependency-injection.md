---
uid: web-api/overview/advanced/dependency-injection
title: Iniekcja zależności w ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: W tym samouczku pokazano, jak wstrzyknąć zależności do kontrolera internetowego interfejsu API ASP.NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622606"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a>Iniekcja zależności w ASP.NET Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> W tym samouczku pokazano, jak wstrzyknąć zależności do kontrolera interfejsu API sieci Web ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Internetowy interfejs API 2
> - [Blok aplikacji aparatu Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (wersja 5 działa również)

## <a name="what-is-dependency-injection"></a>Co to jest iniekcja zależności?

*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt. Na przykład często istnieje możliwość zdefiniowania [repozytorium](http://martinfowler.com/eaaCatalog/repository.html) , które obsługuje dostęp do danych. Ilustrujemy przykład. Najpierw zdefiniujemy model domeny:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Oto prosta Klasa repozytorium, która przechowuje elementy w bazie danych przy użyciu Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Teraz zdefiniujemy kontroler interfejsu API sieci Web, który obsługuje żądania GET dla jednostek `Product`. (Pozostawiam wpis i inne metody dla uproszczenia). Oto pierwsza próba:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Należy zauważyć, że klasa kontrolera zależy od `ProductRepository`i zezwalamy kontrolerowi na tworzenie wystąpienia `ProductRepository`. Niemniej jednak dobrym pomysłem jest napełnienie kodu zależności w ten sposób, z kilku powodów.

- Aby zastąpić `ProductRepository` z inną implementacją, należy również zmodyfikować klasę kontrolera.
- Jeśli `ProductRepository` ma zależności, należy je skonfigurować w ramach kontrolera. W przypadku dużego projektu z wieloma kontrolerami kod konfiguracji jest rozmieszczany w całym projekcie.
- Test jednostkowy jest trudno, ponieważ jest on zakodowany do wykonywania zapytań w bazie danych. W przypadku testów jednostkowych należy użyć repozytorium makiety lub stub, które nie jest możliwe w bieżącym projekcie.

Możemy rozwiązać te problemy poprzez *wstrzyknięcie* repozytorium do kontrolera. Najpierw Refaktoryzacja klasy `ProductRepository` w interfejsie:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Następnie podaj `IProductRepository` jako parametr konstruktora:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

W tym przykładzie zastosowano [iniekcję konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Można również użyć *iniekcji setter*, gdzie ustawia się zależność za pomocą metody lub właściwości setter.

Jednak teraz występuje problem, ponieważ aplikacja nie tworzy kontrolera bezpośrednio. Interfejs API sieci Web tworzy kontroler, gdy kieruje żądanie, a interfejs API sieci Web nie wie niczego o `IProductRepository`. Jest to miejsce, w którym znajduje się program rozpoznawania zależności interfejsu API sieci Web.

## <a name="the-web-api-dependency-resolver"></a>Program rozpoznawania zależności interfejsu API sieci Web

Internetowy interfejs API definiuje interfejs **IDependencyResolver** do rozpoznawania zależności. Oto definicja interfejsu:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Interfejs **IDependencyScope** ma dwie metody:

- **GetService** tworzy jedno wystąpienie typu.
- **GetServices** tworzy kolekcję obiektów określonego typu.

Metoda **IDependencyResolver** dziedziczy **IDependencyScope** i dodaje metodę **BeginScope** . Porozmawiamy o zakresach w dalszej części tego samouczka.

Gdy interfejs API sieci Web tworzy wystąpienie kontrolera, najpierw wywołuje **IDependencyResolver. GetService**, przekazując do typu kontrolera. Za pomocą tego punktu zaczepienia rozszerzalności można utworzyć kontroler, rozwiązując wszystkie zależności. Jeśli **GetService** zwraca wartość null, interfejs API sieci Web szuka konstruktora bez parametrów w klasie Controller.

## <a name="dependency-resolution-with-the-unity-container"></a>Rozpoznawanie zależności przy użyciu kontenera Unity

Chociaż można napisać kompletną implementację **IDependencyResolver** od podstaw, interfejs jest naprawdę zaprojektowany do działania jako Most między interfejsem API sieci Web a istniejącymi kontenerami IOC.

Kontener IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami. Możesz zarejestrować typy w kontenerze, a następnie użyć kontenera do tworzenia obiektów. Kontener automatycznie określa relacje zależności. Wiele kontenerów IoC umożliwia również Sterowanie elementami, takimi jak okres istnienia obiektu i zakres.

> [!NOTE]
> "IoC" oznacza "Inversion of Control", który jest ogólnym wzorcem, w którym struktura wywołuje kod aplikacji. Kontener IoC konstruuje obiekty, co oznacza, że jest to normalny przepływ sterowania.

W tym samouczku będziemy używać [aparatu Unity](https://msdn.microsoft.com/library/ff647202.aspx) firmy Microsoft dla &amp; praktyk. (Inne popularne biblioteki obejmują [Castle Windsor](http://www.castleproject.org/), [Spring.NET](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)i [StructureMap](http://structuremap.github.io/documentation/)). Do zainstalowania aparatu Unity można użyć Menedżera pakietów NuGet. W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Oto implementacja **IDependencyResolver** , która otacza kontener Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Jeśli metoda **GetService** nie może rozpoznać typu, powinien zwrócić **wartość null**. Jeśli metoda **GetServices** nie może rozpoznać typu, powinien zwrócić pusty obiekt kolekcji. Nie zgłaszaj wyjątków dla nieznanych typów.

## <a name="configuring-the-dependency-resolver"></a>Konfigurowanie programu rozpoznawania zależności

Ustaw program rozpoznawania zależności we właściwości **DependencyResolver** globalnego obiektu **HttpConfiguration** .

Poniższy kod rejestruje interfejs `IProductRepository` przy użyciu aparatu Unity, a następnie tworzy `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Zakres zależności i okres istnienia kontrolera

Na żądanie są tworzone kontrolery. Aby zarządzać okresami istnienia obiektów, **IDependencyResolver** używa koncepcji *zakresu*.

Program rozpoznawania zależności dołączony do obiektu **HttpConfiguration** ma zakres globalny. Gdy interfejs API sieci Web tworzy kontroler, wywołuje **BeginScope**. Ta metoda zwraca **IDependencyScope** , który reprezentuje zakres podrzędny.

Interfejs API sieci Web następnie wywołuje metodę **GetService** w zakresie podrzędnym, aby utworzyć kontroler. Po zakończeniu żądania interfejs API sieci Web wywołuje metodę **Dispose** dla zakresu podrzędnego. Użyj metody **Dispose** , aby usunąć zależności kontrolera.

Sposób implementacji **BeginScope** zależy od kontenera IOC. W przypadku aparatu Unity zakres odpowiada kontenerowi podrzędnemu:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Większość kontenerów IoC ma podobne odpowiedniki.

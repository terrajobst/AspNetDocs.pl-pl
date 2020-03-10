---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Badanie metod Details i DELETE | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: da06815b5c1d76a939fdfb77ce11774081dfb881
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582510"
---
# <a name="examining-the-details-and-delete-methods"></a>Badanie metod Details i Delete

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

W tej części samouczka sprawdzisz automatycznie wygenerowany `Details` i `Delete` metod.

## <a name="examining-the-details-and-delete-methods"></a>Badanie metod Details i Delete

Otwórz kontroler `Movie` i Przeanalizuj metodę `Details`.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Aparat tworzenia szkieletu MVC, który utworzył tę metodę akcji, dodaje komentarz zawierający żądanie HTTP, które wywołuje metodę. W tym przypadku jest to żądanie `GET` z trzema segmentami adresów URL, kontrolerem `Movies`, metodą `Details` i `ID` wartością.

Code First ułatwia wyszukiwanie danych przy użyciu metody `Find`. Ważna funkcja zabezpieczeń wbudowana w metodę polega na tym, że kod sprawdza, czy metoda `Find` odnalazła film, zanim kod próbuje wykonać dowolne czynności. Na przykład haker może wprowadzić błędy do witryny przez zmianę adresu URL utworzonego przez linki z `http://localhost:xxxx/Movies/Details/1` na element podobny do `http://localhost:xxxx/Movies/Details/12345` (lub innej wartości, która nie reprezentuje rzeczywistego filmu). Jeśli nie sprawdzono filmu o wartości null, film o wartości null spowoduje błąd bazy danych.

Przeanalizuj metody `Delete` i `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Należy zauważyć, że metoda HTTP GET `Delete` nie usuwa określonego filmu, zwraca widok filmu, w którym można przesłać (`HttpPost`) usunięcie. Wykonanie operacji usuwania w odpowiedzi na żądanie GET (lub w tym przypadku wykonanie operacji edycji, operacji tworzenia lub jakiejkolwiek innej operacji, która zmienia dane) powoduje otwarcie otworu zabezpieczeń. Aby uzyskać więcej informacji na ten temat, zobacz wpis w blogu Stephen Walther [ASP.NET MVC Tip #46 — nie używaj linków usuwania, ponieważ tworzą one luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Metoda `HttpPost`, która usuwa dane, ma nazwę `DeleteConfirmed`, aby nadać metodzie POST protokołu HTTP unikatowy podpis lub nazwę. Poniżej przedstawiono dwie sygnatury metod:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga, aby przeciążone metody miały unikatowy podpis parametru (taka sama nazwa metody, ale inna lista parametrów). Jednak w tym miejscu wymagane są dwie metody usuwania — jeden dla elementu GET i jeden dla elementu POST--oba mają taki sam podpis parametru. (Oba muszą akceptować jedną liczbę całkowitą jako parametr).

Aby to zrobić, możesz wykonać kilka czynności. Jedną z nich jest nadanie metodom różnych nazw. To właśnie mechanizm tworzenia szkieletu w poprzednim przykładzie. Wprowadzamy jednak niewielki problem: ASP.NET mapuje segmenty adresu URL na metody akcji według nazwy, a jeśli zmienisz nazwę metody, routing zwykle nie będzie mógł znaleźć tej metody. To rozwiązanie jest widoczne w przykładzie, czyli dodanie atrybutu `ActionName("Delete")` do metody `DeleteConfirmed`. Efektywnie wykonuje to mapowanie dla systemu routingu, aby adres URL, który zawiera */delete/* dla żądania post, znalazł metodę `DeleteConfirmed`.

Innym typowym sposobem, aby uniknąć problemu z metodami, które mają identyczne nazwy i podpisy, jest sztuczna zmiana sygnatury metody POST w celu uwzględnienia nieużywanego parametru. Na przykład niektórzy deweloperzy dodają typ parametru `FormCollection`, który jest przesyłany do metody POST, a następnie po prostu nie należy używać parametru:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Podsumowanie

Masz teraz kompletną aplikację ASP.NET MVC, która przechowuje dane w lokalnej bazie danych. Możesz tworzyć, odczytywać, aktualizować, usuwać i wyszukiwać filmy.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Następne kroki

Po skompilowaniu i przetestowaniu aplikacji sieci Web następnym krokiem jest udostępnienie go innym osobom, które będą mogły korzystać z Internetu. W tym celu należy wdrożyć go w dostawcy hostingu w sieci Web. Firma Microsoft oferuje bezpłatny hosting w sieci Web dla maksymalnie 10 witryn sieci Web w ramach [bezpłatnego konta wersji próbnej platformy Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Sugeruję, aby dalej postępować zgodnie z moim samouczkiem [Wdróż aplikację Secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i SQL Database na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Doskonały samouczek to element pośredni Dykstra [, który tworzy model danych Entity Framework dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) i [ASP.NET MVC](https://forums.asp.net/1146.aspx) są doskonałym miejscem do zadawania pytań. Obserwuj [mnie](https://twitter.com/RickAndMSFT) w serwisie Twitter, aby otrzymywać aktualizacje dotyczące moich najnowszych samouczków.

Opinia jest powitania.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Wstecz](adding-validation.md)

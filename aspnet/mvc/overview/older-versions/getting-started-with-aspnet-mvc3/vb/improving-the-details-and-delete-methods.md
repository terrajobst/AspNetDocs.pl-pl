---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Ulepszanie metod Details i Delete (VB) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 08d80cac071907e927bb30df53c6f84a28f53156
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599954"
---
# <a name="improving-the-details-and-delete-methods-vb"></a>Ulepszanie metod Details i Delete (VB)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępny do tego tematu. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przejdź do [ C# wersji](../cs/improving-the-details-and-delete-methods.md) tego samouczka.

W tej części samouczka wprowadzisz ulepszenia do automatycznie generowanych `Details` i `Delete`. Te zmiany nie są wymagane, ale za pomocą zaledwie kilku małych bitów kodu można łatwo ulepszyć aplikację.

## <a name="improving-the-details-and-delete-methods"></a>Ulepszanie metod Details i DELETE

Podczas tworzenia szkieletu `Movie` kontrolerem MVC wygenerował kod, który pracował dobrze, ale może być bardziej niezawodny z zaledwie kilkoma małymi zmianami.

Otwórz kontroler `Movie` i zmodyfikuj metodę `Details`, zwracając `HttpNotFound`, gdy film nie zostanie znaleziony. Należy również zmodyfikować metodę `Details`, aby ustawić wartość domyślną dla identyfikatora, który jest do niego przesłany. (W [części 6](examining-the-edit-methods-and-edit-view.md) tego samouczka wprowadzono podobne zmiany w metodzie `Edit`). Należy jednak zmienić zwracany typ metody `Details` z `ViewResult` na `ActionResult`, ponieważ metoda `HttpNotFound` nie zwraca obiektu `ViewResult`. Poniższy przykład przedstawia zmodyfikowaną metodę `Details`.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Code First ułatwia wyszukiwanie danych przy użyciu metody `Find`. Ważna funkcja zabezpieczeń wbudowana w metodę polega na tym, że kod sprawdza, czy metoda `Find` odnalazła film, zanim kod próbuje wykonać dowolne czynności. Na przykład haker może wprowadzić błędy do witryny przez zmianę adresu URL utworzonego przez linki z `http://localhost:xxxx/Movies/Details/1` na element podobny do `http://localhost:xxxx/Movies/Details/12345` (lub innej wartości, która nie reprezentuje rzeczywistego filmu). Jeśli nie sprawdzasz filmu o wartości null, może to spowodować błąd bazy danych.

Analogicznie Zmień metody `Delete` i `DeleteConfirmed`, aby określić wartość domyślną parametru ID i zwrócić `HttpNotFound`, gdy film nie zostanie znaleziony. Poniżej przedstawiono zaktualizowane metody `Delete` na kontrolerze `Movie`.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Należy pamiętać, że metoda `Delete` nie usuwa danych. Wykonanie operacji usuwania w odpowiedzi na żądanie GET (lub w tym przypadku wykonanie operacji edycji, operacji tworzenia lub jakiejkolwiek innej operacji, która zmienia dane) powoduje otwarcie otworu zabezpieczeń. Aby uzyskać więcej informacji na ten temat, zobacz wpis w blogu Stephen Walther [ASP.NET MVC Tip #46 — nie używaj linków usuwania, ponieważ tworzą one luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Metoda `HttpPost`, która usuwa dane, ma nazwę `DeleteConfirmed`, aby nadać metodzie POST protokołu HTTP unikatowy podpis lub nazwę. Poniżej przedstawiono dwie sygnatury metod:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga, aby przeciążone metody miały unikatowy podpis (taka sama nazwa, inna lista parametrów). Jednak w tym miejscu wymagane są dwie metody usuwania — jeden dla GET i jeden dla elementu POST--oba wymagają tego samego podpisu. (Oba muszą akceptować jedną liczbę całkowitą jako parametr).

Aby to zrobić, możesz wykonać kilka czynności. Jedną z nich jest nadanie metodom różnych nazw. To wszystko w poprzednim przykładzie. Wprowadzamy jednak niewielki problem: ASP.NET mapuje segmenty adresu URL na metody akcji według nazwy, a jeśli zmienisz nazwę metody, routing zwykle nie będzie mógł znaleźć tej metody. To rozwiązanie jest widoczne w przykładzie, czyli dodanie atrybutu `ActionName("Delete")` do metody `DeleteConfirmed`. Efektywnie wykonuje to mapowanie dla systemu routingu, aby adres URL, który zawiera <em>/delete/</em>dla żądania post, znalazł metodę `DeleteConfirmed`.

Innym sposobem na uniknięcie problemu przy użyciu metod o identycznych nazwach i sygnaturach jest sztuczne Zmienianie sygnatury metody POST w celu uwzględnienia nieużywanego parametru. Na przykład niektórzy deweloperzy dodają typ parametru `FormCollection`, który jest przesyłany do metody POST, a następnie po prostu nie należy używać parametru:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Zawijanie w górę

Masz teraz kompletną aplikację ASP.NET MVC, która przechowuje dane w bazie danych SQL Server Compact. Możesz tworzyć, odczytywać, aktualizować, usuwać i wyszukiwać filmy.

![](improving-the-details-and-delete-methods/_static/image1.png)

W tym podstawowym samouczku przedstawiono tworzenie kontrolerów, kojarzenie ich z widokami i przekazywanie wokół zakodowanych danych. Następnie utworzony i zaprojektowany model danych. Entity Framework Code First utworzyć bazę danych z modelu danych na bieżąco, a system szkieletu MVC ASP.NET automatycznie wygenerował metody akcji i widoki dla podstawowych operacji CRUD. Następnie dodano formularz wyszukiwania, który umożliwia użytkownikom wyszukiwanie w bazie danych. Baza danych została zmieniona tak, aby zawierała nową kolumnę danych, a następnie Zaktualizowano dwie strony, aby utworzyć i wyświetlić te nowe dane. Dodano weryfikację, zaznaczając model danych z atrybutami z przestrzeni nazw `DataAnnotations`. Wyniki walidacji są uruchamiane na kliencie i na serwerze programu.

Jeśli chcesz wdrożyć aplikację, warto najpierw przetestować aplikację na lokalnym serwerze usług IIS 7. Możesz użyć tego linku [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) , aby włączyć ustawienia usług IIS dla aplikacji ASP.NET. Zobacz następujące linki wdrażania:

- [Mapa zawartości wdrożenia ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Włączanie usług IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Wdrażanie projektów aplikacji sieci Web](https://msdn.microsoft.com/library/dd394698.aspx)

Teraz zachęcamy do przechodzenia do naszego poziomu pośredniego, który [tworzy Entity Framework model danych dla aplikacji ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) oraz samouczków [sklepu MVC Music](../../mvc-music-store/mvc-music-store-part-1.md) , aby poznać [artykuły ASP.NET w witrynie MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)oraz zapoznać się z wieloma filmami wideo i zasobami w [https://asp.net/mvc](https://asp.net/mvc) , aby dowiedzieć się więcej na temat ASP.NET MVC! [Fora ASP.NET MVC](https://forums.asp.net/1146.aspx) są doskonałym miejscem na zadawanie pytań.

Owocnej pracy.

— Scott Hanselman ([http://hanselman.com](http://hanselman.com) i [@shanselman](http://twitter.com/shanselman) w serwisie Twitter) i Rick Anderson [blogs.MSDN.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Wstecz](adding-validation-to-the-model.md)

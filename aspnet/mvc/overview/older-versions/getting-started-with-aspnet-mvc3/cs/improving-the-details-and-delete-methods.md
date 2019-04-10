---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Ulepszanie metod Details i Delete (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: d4003dba8530d2e72c514c572ffc28ef942fd437
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59379368"
---
# <a name="improving-the-details-and-delete-methods-c"></a>Ulepszanie metod Details i Delete (C#)

Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express SP1 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, należy zainstalować wymagania wstępne, klikając poniższe łącze: [Visual Studio 2010 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.


W tej części samouczka, należy podjąć kilka ulepszeń do automatycznie generowanego `Details` i `Delete` metody. Te zmiany nie są wymagane, ale przy użyciu zaledwie kilku małe fragmenty kodu można łatwo zwiększyć aplikacji.

## <a name="improving-the-details-and-delete-methods"></a>Ulepszanie metod Details i Delete

Gdy działanie `Movie` kontrolera ASP.NET MVC wygenerowanego kodu, bardzo dobrze funkcjonowało, ale który mogą być działał on bardziej niezawodnie za pomocą zaledwie kilku niewielkich zmian.

Otwórz `Movie` kontrolera i modyfikować `Details` metody, zwracając `HttpNotFound` po filmu nie zostanie znaleziona. Należy również zmodyfikować `Details` metodę, aby ustawić wartość domyślną dla Identyfikatora, który jest przekazywany do niego. (Zostały wprowadzone zmiany podobne do `Edit` method in Class metoda [część 6](examining-the-edit-methods-and-edit-view.md) po ukończeniu tego samouczka.) Jednakże, należy zmienić typ zwracany `Details` metody z `ViewResult` do `ActionResult`, ponieważ `HttpNotFound` metoda nie zwraca `ViewResult` obiektu. W poniższym przykładzie pokazano zmodyfikowanego `Details` metody.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Kod najpierw ułatwia wyszukiwanie danych przy użyciu `Find` metody. Funkcja ważne zabezpieczeń, którą utworzyliśmy do metody jest kod sprawdza, czy `Find` znaleziono filmu metody, zanim kod próbuje wykonywać żadnych czynności z nim. Na przykład haker może spowodować błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` na wartość podobną `http://localhost:xxxx/Movies/Details/12345` (lub inną wartość, która nie zawiera rzeczywistych filmu). Jeśli nie jest sprawdzanie wartości null film, może to spowodować błąd bazy danych.

Podobnie, zmiany `Delete` i `DeleteConfirmed` metody, aby określić wartość domyślną dla parametru Identyfikatora i zwrócić `HttpNotFound` po filmu nie zostanie znaleziona. Zaktualizowany interfejs `Delete` metody `Movie` kontrolera zostały wymienione poniżej.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Należy pamiętać, że `Delete` metoda nie powoduje usunięcia danych. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonywania operacji Edytuj, Utwórz operacji lub innej operacji, które zmieniają dane) otwiera lukę w zabezpieczeniach. Aby uzyskać więcej informacji na ten temat, zobacz wpis w blogu Autor: Stephen Walther [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` zapewnienie metodą HTTP POST unikatowy podpis lub nazwy. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążone metody ma unikatowy podpis (tej samej nazwie, inną listę parametrów). Jednak w tym miejscu należy dwie metody usuwania — jeden dla GET--i jeden dla wpisu, wymagać taki sam podpis. (Obaj użytkownicy muszą zaakceptować pojedyncze liczby całkowite jako parametr.)

Aby posortować tę możliwość, można zrobić kilka rzeczy. Jeden to nadać różne nazwy metody. To zrobiliśmy w on poprzedzających przykład. Jednak wprowadza mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy, a jeśli zmienisz metodę, routing zwykle nie można znaleźć tej metody. Rozwiązanie jest widoczny w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. Skutecznie wykonuje to mapowanie systemu routingu, aby adres URL, który zawiera <em>/Delete/</em>dla wpisu znajdzie żądanie `DeleteConfirmed` metody.

Innym sposobem, aby uniknąć problemu, za pomocą metod, które mają identyczne nazwy i wzory podpisów jest sztucznie Zmień podpis metody POST, aby uwzględnić Nieużywany parametr. Na przykład niektórzy deweloperzy dodać typ parametru `FormCollection` który jest przekazywany do metody POST, a następnie po prostu nie używaj parametru:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Zakończeniem

Masz teraz kompletnej aplikacji ASP.NET MVC, która przechowuje dane w bazie danych programu SQL Server Compact. Można utworzyć, Odczyt, aktualizowanie, usuwanie i wyszukaj filmy.

![](improving-the-details-and-delete-methods/_static/image1.png)

W tym samouczku podstawowe stało się ułatwiające rozpoczęcie pracy, dzięki czemu kontrolerów, skojarzenie ich z widoków i przekazanie ustalonych danymi. Następnie zostało utworzone i przeznaczone do modelu danych. Entity Framework Code First utworzono bazę danych z modelu danych na bieżąco, a system scaffoldingu platformy ASP.NET MVC automatycznie generowane metody akcji i wyświetlenia dla podstawowych operacji CRUD. Następnie została dodana formularza wyszukiwania, które umożliwiają użytkownikom wyszukiwanie bazy danych. Zmienione znaleźć nową kolumnę danych w bazie danych, a następnie zaktualizować dwie strony, aby utworzyć i wyświetlić te nowe dane. Dodano sprawdzanie poprawności, oznaczając modelu danych za pomocą atrybutów z `DataAnnotations` przestrzeni nazw. Wynikowy sprawdzania poprawności jest uruchamiany na kliencie i na serwerze.

Jeśli chcesz wdrożyć aplikację, warto pierwszego testu aplikacji na lokalnym serwerze usług IIS 7. Możesz użyć tej funkcji [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) link, aby włączyć ustawienie programu IIS dla aplikacji ASP.NET. Zobacz poniższe linki wdrożenia:

- [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/dd394698.aspx)
- [Enabling IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Wdrażanie projektów aplikacji sieci Web](https://msdn.microsoft.com/library/dd394698.aspx)

Teraz zachęcam Cię do przejdź do naszego poziomu pośredniego [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) i [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) samouczki, aby zapoznać się z [platformy ASP.NET artykuły w witrynie MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)i zapoznaj się z wielu filmów wideo i zasobów w [ https://asp.net/mvc ](https://asp.net/mvc) się jeszcze bardziej poświęcone platformie ASP.NET MVC! [Fora platformy ASP.NET MVC](https://forums.asp.net/1146.aspx) są doskonałym miejscem do zadawania pytań.

Ciesz się!

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) i [ @shanselman ](http://twitter.com/shanselman) w serwisie Twitter) i Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Poprzednie](adding-validation-to-the-model.md)

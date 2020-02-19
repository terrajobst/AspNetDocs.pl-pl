---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Dodawanie modelu | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457807"
---
# <a name="adding-a-model"></a>Dodawanie modelu

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.

W tej sekcji dodasz kilka klas do zarządzania filmami w bazie danych. Te klasy będą modelem &quot;&quot; częścią aplikacji ASP.NET MVC.

Będziesz używać .NET Framework technologii dostępu do danych, znanej jako [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) do definiowania i pracy z tymi klasami modelu. Entity Framework (często określana jako EF) obsługuje model programistyczny o nazwie *Code First*. Code First umożliwia tworzenie obiektów modelu przez pisanie prostych klas. (Są one również znane jako klasy POCO, od &quot;zwykłych obiektów CLR.&quot;) Następnie można utworzyć bazę danych na bieżąco od klas, co umożliwia bardzo czysty i szybki przepływ pracy programistycznej.

## <a name="adding-model-classes"></a>Dodawanie klas modelu

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.

![](adding-a-model/_static/image1.png)

Wprowadź nazwę *klasy* &quot;filmu&quot;.

Dodaj następujące pięć właściwości do klasy `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Użyjemy klasy `Movie` do reprezentowania filmów w bazie danych. Każde wystąpienie obiektu `Movie` będzie odpowiadać wierszowi w tabeli bazy danych, a każda właściwość klasy `Movie` zostanie zamapowana na kolumnę w tabeli.

W tym samym pliku Dodaj następującą `MovieDBContext` klasy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

Klasa `MovieDBContext` reprezentuje kontekst bazy danych filmów Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Movie` w bazie danych. `MovieDBContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework.

Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następującą instrukcję `using` na początku pliku:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Pełny plik *Movie.cs* jest przedstawiony poniżej. (Kilka instrukcji using, które nie są konieczne, zostało usuniętych).

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i praca z bazą danych SQL Server LocalDB

Utworzona Klasa `MovieDBContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych. Jednym z pytań, które można zadać, jest określenie, jak należy określić bazę danych, z którą zostanie nawiązane połączenie. W tym celu należy dodać informacje o połączeniu w pliku *Web. config* aplikacji.

Otwórz główny plik *Web. config* aplikacji. (Plik *Web. config* znajduje się w folderze *widoki* ). Otwórz plik *Web. config* przedstawiony na czerwono.

![](adding-a-model/_static/image2.png)

Dodaj następujące parametry połączenia do elementu `<connectionStrings>` w pliku *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Poniższy przykład przedstawia część pliku *Web. config* z nowymi dodanymi parametrami połączenia:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Ta niewielka ilość kodu i XML to wszystko, czego potrzebujesz do reprezentowania i przechowywania danych filmowych w bazie danych.

Następnie utworzysz nową klasę `MoviesController`, która może być używana do wyświetlania danych filmowych i umożliwia użytkownikom tworzenie nowych list filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)

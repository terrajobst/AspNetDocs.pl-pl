---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Dodawanie modelu | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: c5525cfe940cadff5113c63eb0508d15697b5606
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615753"
---
# <a name="adding-a-model"></a>Dodawanie modelu

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

W tej sekcji dodasz kilka klas do zarządzania filmami w bazie danych. Te klasy będą modelem &quot;&quot; częścią aplikacji ASP.NET MVC.

Będziesz używać .NET Framework technologii dostępu do danych, znanej jako [Entity Framework](https://docs.microsoft.com/ef/) do definiowania i pracy z tymi klasami modelu. Entity Framework (często określana jako EF) obsługuje model programistyczny o nazwie *Code First*. Code First umożliwia tworzenie obiektów modelu przez pisanie prostych klas. (Są one również znane jako klasy POCO, od &quot;zwykłych obiektów CLR.&quot;) Następnie można utworzyć bazę danych na bieżąco od klas, co umożliwia bardzo czysty i szybki przepływ pracy programistycznej. Jeśli musisz najpierw utworzyć bazę danych, możesz skorzystać z tego samouczka, aby dowiedzieć się więcej na temat programowania aplikacji MVC i EF. Następnie można postępować zgodnie z samouczkiem tworzenie [szkieletu Fizmakens ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) , który obejmuje pierwsze podejście do bazy danych.

## <a name="adding-model-classes"></a>Dodawanie klas modelu

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.

![](adding-a-model/_static/image1.png)

Wprowadź nazwę *klasy* &quot;filmu&quot;.

Dodaj następujące pięć właściwości do klasy `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Użyjemy klasy `Movie` do reprezentowania filmów w bazie danych. Każde wystąpienie obiektu `Movie` będzie odpowiadać wierszowi w tabeli bazy danych, a każda właściwość klasy `Movie` zostanie zamapowana na kolumnę w tabeli.

Uwaga: aby można było użyć klasy System. Data. Entity i powiązanej, należy zainstalować [pakiet NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/). Aby uzyskać dalsze instrukcje, postępuj zgodnie z linkiem.

W tym samym pliku Dodaj następującą `MovieDBContext` klasy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

Klasa `MovieDBContext` reprezentuje kontekst bazy danych filmów Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Movie` w bazie danych. `MovieDBContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework.

Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następującą instrukcję `using` na początku pliku:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Można to zrobić przez ręczne dodanie instrukcji using lub umieszczenie wskaźnika myszy na czerwono falistej linii, kliknij przycisk `Show potential fixes` i kliknij przycisk `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Uwaga: Usunięto kilka nieużywanych instrukcji `using`. Program Visual Studio będzie wyświetlał nieużywane zależności jako szare. Nieużywane zależności można usunąć, umieszczając kursor na szarych zależnościach, klikając `Show potential fixes` i klikając polecenie **Usuń nieużywane użycie.**

![](adding-a-model/_static/image3.png)

Ostatecznie dodaliśmy model (M w MVC). W następnej sekcji będziesz korzystać z parametrów połączenia z bazą danych.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-view.md)
> [dalej](creating-a-connection-string.md)

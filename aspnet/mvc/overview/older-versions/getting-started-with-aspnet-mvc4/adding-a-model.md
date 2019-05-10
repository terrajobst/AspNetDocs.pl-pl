---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Dodawanie modelu | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Dostępne jest zaktualizowana wersja tego samouczka, która korzysta z platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 5b26b79de99763bd41d0c3471a666cd6bb4d2d75
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65129908"
---
# <a name="adding-a-model"></a>Dodawanie modelu

Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.

W tej sekcji dodasz niektóre klasy zarządzania filmów w bazie danych. Te klasy będzie &quot;modelu&quot; wchodzi w skład aplikacji platformy ASP.NET MVC.

Użyjesz technologii dostępu do danych .NET Framework, znane jako [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) do definiowania i pracować z tych klas modelu. Obsługuje platformy Entity Framework (często określanymi jako EF) o nazwie paradygmat programowania *Code First*. Najpierw kod pozwala na tworzenie obiektów modelu, pisząc proste klas. (Te są również nazywane klasami POCO z &quot;obiektów CLR zwykły stary.&quot;) Można następnie skonfigurować bazę danych, tworzone na bieżąco z klas, co umożliwia przepływ pracy tworzenia bardzo szybkie i przejrzysty.

## <a name="adding-model-classes"></a>Dodawanie klasy modelu

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *modeli* folderu, wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.

![](adding-a-model/_static/image1.png)

Wprowadź *klasy* nazwa &quot;filmu&quot;.

Dodaj następujące właściwości pięć `Movie` klasy:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Użyjemy `Movie` klasy do reprezentowania filmów w bazie danych. Każde wystąpienie `Movie` obiektu odnoszą się do wiersza w tabeli bazy danych, a każda właściwość `Movie` klasy będzie zmapowana do kolumny w tabeli.

W tym samym pliku Dodaj następujący kod `MovieDBContext` klasy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Klasa reprezentuje kontekst bazy danych programu Entity Framework film, który obsługuje pobieranie, przechowywania i aktualizowania `Movie` klasy wystąpień w bazie danych. `MovieDBContext` Pochodzi od klasy `DbContext` dostarczane przez program Entity Framework klasy bazowej.

Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następujące `using` instrukcji w górnej części pliku:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Pełne *Movie.cs* plików znajdują się poniżej. (Kilka przy użyciu instrukcji, które nie są potrzebne zostały usunięte.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i praca z bazą danych SQL Server LocalDB

`MovieDBContext` Utworzone klasy obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Jedno pytanie, które możesz zadawać, jest jednak sposób określić bazę danych, która zostanie nawiązane połączenie. Należy to zrobić, dodając informacje o połączeniu w *Web.config* pliku aplikacji.

Otwórz katalog główny aplikacji *Web.config* pliku. (Nie *Web.config* w pliku *widoków* folderu.) Otwórz *Web.config* pliku wyróżnione kolorem czerwonym.

![](adding-a-model/_static/image2.png)

Dodaj poniższe parametry połączenia do `<connectionStrings>` element *Web.config* pliku.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

W poniższym przykładzie pokazano część *Web.config* pliku dodano nowy ciąg połączenia:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Ta niewielką ilość kodu i XML to wszystko, czego potrzebujesz do zapisu w celu reprezentowania i przechowywanie danych filmów w bazie danych.

Następnie utworzysz nowy `MoviesController` klasę, która służy do wyświetlania danych filmów i Zezwalaj użytkownikom na tworzenie nowych list filmu.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)

---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Dodawanie modelu (VB) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540524"
---
# <a name="adding-a-model-vb"></a>Dodawanie modelu (VB)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępny do tego tematu. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przejdź do [ C# wersji](../cs/adding-a-model.md) tego samouczka.

## <a name="adding-a-model"></a>Dodawanie modelu

W tej sekcji dodasz kilka klas do zarządzania filmami w bazie danych. Te klasy będą częścią "modelem" aplikacji ASP.NET MVC.

Będziesz używać .NET Framework technologii dostępu do danych, znanej jako Entity Framework do definiowania i pracy z tymi klasami modelu. Entity Framework (często określana jako EF) obsługuje model programistyczny o nazwie *Code First*. Code First umożliwia tworzenie obiektów modelu przez pisanie prostych klas. (Są one również znane jako klasy POCO z "zwykłych, starych obiektów CLR"). Następnie można utworzyć bazę danych na bieżąco od klas, co umożliwia bardzo czysty i szybki przepływ pracy programistycznej.

## <a name="adding-model-classes"></a>Dodawanie klas modelu

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.

![](adding-a-model/_static/image1.png)

Nazwij klasę "Movie".

Dodaj następujące pięć właściwości do klasy `Movie`:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Użyjemy klasy `Movie` do reprezentowania filmów w bazie danych. Każde wystąpienie obiektu `Movie` będzie odpowiadać wierszowi w tabeli bazy danych, a każda właściwość klasy `Movie` zostanie zamapowana na kolumnę w tabeli.

W tym samym pliku Dodaj następującą `MovieDBContext` klasy:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

Klasa `MovieDBContext` reprezentuje kontekst bazy danych filmów Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Movie` w bazie danych. `MovieDBContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework. Aby uzyskać więcej informacji na temat `DbContext` i `DbSet`, zobacz [ulepszenia produktywności Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Aby można było odwoływać się do `DbContext` i `DbSet`, należy dodać następującą instrukcję `imports` na początku pliku:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Pełny plik *film. vb* jest przedstawiony poniżej.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Tworzenie parametrów połączenia i praca z SQL Server Compact

Utworzona Klasa `MovieDBContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych. Jednym z pytań, które można zadać, jest określenie, jak należy określić bazę danych, z którą zostanie nawiązane połączenie. W tym celu należy dodać informacje o połączeniu w pliku *Web. config* aplikacji.

Otwórz główny plik *Web. config* aplikacji. (Plik *Web. config* znajduje się w folderze *widoki* ). Na poniższej ilustracji przedstawiono oba pliki *Web. config* ; Otwórz plik *Web. config* w kolorze czerwonym.

![](adding-a-model/_static/image2.png)

Dodaj następujące parametry połączenia do elementu `<connectionStrings>` w pliku *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Poniższy przykład przedstawia część pliku *Web. config* z nowymi dodanymi parametrami połączenia:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Ta niewielka ilość kodu i XML to wszystko, czego potrzebujesz do reprezentowania i przechowywania danych filmowych w bazie danych.

Następnie utworzysz nową klasę `MoviesController`, która może być używana do wyświetlania danych filmowych i umożliwia użytkownikom tworzenie nowych list filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-view.md)
> [dalej](accessing-your-models-data-from-a-controller.md)

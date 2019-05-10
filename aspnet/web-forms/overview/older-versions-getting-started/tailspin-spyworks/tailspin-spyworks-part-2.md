---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: Część 2. Data Access Layer | Microsoft Docs
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 2 obejmuje dodawanie warstwy dostępu do danych.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130614"
---
# <a name="part-2-data-access-layer"></a>Część 2. Warstwa dostępu do danych

przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 2 obejmuje dodawanie warstwy dostępu do danych.

## <a id="_Toc260221668"></a>  Dodawanie warstwy dostępu do danych

Nasza aplikacja handlu elektronicznego będzie zależeć od dwóch baz danych.

Aby uzyskać informacje o kliencie użyjemy standardowa baza danych członkostwa ASP.NET. Dla wykazu zakupów koszyka i produktu będzie wdrażamy bazy danych programu SQL Express w następujący sposób.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Jeśli utworzono bazę danych (Commerce.mdf) w aplikacji App\_folder dane możemy rozpocząć tworzenie warstwy dostępu do danych, nasze używający narzędzia .NET Entity Framework.

Utworzymy folder o nazwie "danych\_dostępu" i je w tym folderze kliknij prawym przyciskiem myszy i wybierz pozycję "Dodaj nowy element".

W "Zainstalowane szablony" elementu, a następnie wybierz pozycję "ADO.NET Entity Data Model" Wprowadź EDM\_Commerce.edmx jako nazwę i kliknij przycisk "Dodaj".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Wybierz pozycję "Generuj z bazy danych".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Zapisz i kompilacji.

Teraz jesteśmy gotowi dodać naszej pierwszej funkcji — menu kategorii produktów.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-1.md)
> [dalej](tailspin-spyworks-part-3.md)

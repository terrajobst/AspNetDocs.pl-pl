---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Część 2: Warstwa dostępu do danych | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 2 obejmuje dodanie warstwy dostępu do danych.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573249"
---
# <a name="part-2-data-access-layer"></a>Część 2. Warstwa dostępu do danych

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 2 obejmuje dodanie warstwy dostępu do danych.

## <a id="_Toc260221668"></a>Dodawanie warstwy dostępu do danych

Nasza aplikacja handlu elektronicznego będzie zależeć od dwóch baz danych.

Informacje o klientach używają standardowej bazy danych członkostwa ASP.NET. W naszym koszyku i katalogu produktów zostanie wdrożona baza danych SQL Express w następujący sposób.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Po utworzeniu bazy danych (commerce. mdf) w aplikacji aplikacji\_folderze danych możemy utworzyć naszą warstwę dostępu do danych za pomocą programu .NET Entity Framework.

Utworzymy folder o nazwie "dane\_Access" i kliknij prawym przyciskiem myszy ten folder i wybierz pozycję "Dodaj nowy element".

W elemencie "zainstalowane szablony", a następnie wybierz pozycję "ADO.NET Entity Data Model" wpisz EDM\_commerce. edmx jako nazwę, a następnie kliknij przycisk "Dodaj".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Wybierz pozycję "Generuj z bazy danych".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Zapisz i skompiluj.

Teraz jesteśmy gotowi do dodania naszej pierwszej funkcji — menu kategorii produktów.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-1.md)
> [dalej](tailspin-spyworks-part-3.md)

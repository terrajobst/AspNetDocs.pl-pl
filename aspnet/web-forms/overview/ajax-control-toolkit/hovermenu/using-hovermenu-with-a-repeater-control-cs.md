---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Używanie kontrolki hovermenu z kontrolką (C#) | Microsoft Docs
author: wenz
description: 'Kontrolka kontrolki hovermenu w zestawie narzędzi AJAX Control oferuje prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, pojawi się okno podręczne ze specyfikatorem...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e38b91d837c65191d4b3797fa31ef6112a1f070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578128"
---
# <a name="using-hovermenu-with-a-repeater-control-c"></a>Używanie kontrolki HoverMenu z kontrolką elementu powtarzanego (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> Kontrolka kontrolki hovermenu w zestawie narzędzi AJAX Control oferuje prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, zostanie wyświetlone okno podręczne na określonej pozycji. Można również użyć tej kontrolki w ramach wzmacniaka.

## <a name="overview"></a>Omówienie

Formant `HoverMenu` w zestawie narzędzi AJAX Control udostępnia prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, pojawi się okno podręczne na określonej pozycji. Można również użyć tej kontrolki w ramach wzmacniaka.

## <a name="steps"></a>Kroki

Najpierw wymagane jest źródło danych. Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.

W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony. Aby można było używać ograniczonej ilości danych, wybieramy tylko pięć pierwszych wpisów w tabeli dostawcy bazy danych AdventureWorks. Jeśli używasz asystenta programu Visual Studio do utworzenia źródła danych, weź pod uwagę, że usterka w bieżącej wersji nie zawiera prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujące znaczniki pokazują poprawną składnię:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

Następnie Dodaj panel, który służy jako modalne menu podręczne:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

Teraz `HoverMenuExtender` jest odtwarzany. Aby każdy element w źródle danych otrzymywał własne okno podręczne, rozszerzenie musi zostać umieszczone w sekcji `<ItemTemplate>`ka. Oto znacznik:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

Teraz każdy element w źródle danych wyświetla okno podręczne z prawej strony (`PopupPosition` atrybutu) po opóźnieniu 50 milisekund (`PopDelay` Attribute).

[![menu aktywacja pojawia się obok każdego elementu w wzmacniake](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

Menu aktywacja pojawia się obok każdego elementu w wzmacniake ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Dalej](using-hovermenu-with-a-repeater-control-vb.md)

---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Korzystanie z kontrolki hovermenu z formantem wzmacniania (VB) | Microsoft Docs
author: wenz
description: 'Kontrolka kontrolki hovermenu w zestawie narzędzi AJAX Control oferuje prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, pojawi się okno podręczne ze specyfikatorem...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9386aa2fe3a6174bbed52218337107733cb1fa99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606654"
---
# <a name="using-hovermenu-with-a-repeater-control-vb"></a>Używanie kontrolki HoverMenu z kontrolką elementu powtarzanego (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> Kontrolka kontrolki hovermenu w zestawie narzędzi AJAX Control oferuje prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, zostanie wyświetlone okno podręczne na określonej pozycji. Można również użyć tej kontrolki w ramach wzmacniaka.

## <a name="overview"></a>Omówienie

Formant `HoverMenu` w zestawie narzędzi AJAX Control udostępnia prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, pojawi się okno podręczne na określonej pozycji. Można również użyć tej kontrolki w ramach wzmacniaka.

## <a name="steps"></a>Kroki

Najpierw wymagane jest źródło danych. Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.

W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony. Aby można było używać ograniczonej ilości danych, wybieramy tylko pięć pierwszych wpisów w tabeli dostawcy bazy danych AdventureWorks. Jeśli używasz asystenta programu Visual Studio do utworzenia źródła danych, weź pod uwagę, że usterka w bieżącej wersji nie zawiera prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujące znaczniki pokazują poprawną składnię:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Następnie Dodaj panel, który służy jako modalne menu podręczne:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Teraz `HoverMenuExtender` jest odtwarzany. Aby każdy element w źródle danych otrzymywał własne okno podręczne, rozszerzenie musi zostać umieszczone w sekcji `<ItemTemplate>`ka. Oto znacznik:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Teraz każdy element w źródle danych wyświetla okno podręczne z prawej strony (`PopupPosition` atrybutu) po opóźnieniu 50 milisekund (`PopDelay` Attribute).

[![menu aktywacja pojawia się obok każdego elementu w wzmacniake](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Menu aktywacja pojawia się obok każdego elementu w wzmacniake ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Ubiegł](using-hovermenu-with-a-repeater-control-cs.md)

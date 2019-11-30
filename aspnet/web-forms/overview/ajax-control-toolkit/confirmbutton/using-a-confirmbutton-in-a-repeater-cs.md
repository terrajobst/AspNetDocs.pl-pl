---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Używanie kontrolki confirmbutton w Wzmacniake (C#) | Microsoft Docs
author: wenz
description: Rozszerzenie kontrolki confirmbutton w zestawie narzędzi AJAX Control tworzy okno podręczne tak/nie, gdy użytkownik kliknie przycisk (łącznie z kontrolką element LinkButton). Tylko wtedy, gdy jest to...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 468a830f01c48dc39b22bc5d826f80533df65c1a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574396"
---
# <a name="using-a-confirmbutton-in-a-repeater-c"></a>Używanie kontrolki ConfirmButton w elemencie powtarzanym (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> Rozszerzenie kontrolki confirmbutton w zestawie narzędzi AJAX Control tworzy okno podręczne tak/nie, gdy użytkownik kliknie przycisk (łącznie z kontrolką element LinkButton). Tylko wtedy, gdy kliknięto przycisk tak, akcja jest wykonywana, w przeciwnym razie anulowane. Jest to również możliwe w wzmacniake.

## <a name="overview"></a>Omówienie

Rozszerzenie kontrolki confirmbutton w zestawie narzędzi AJAX Control tworzy okno podręczne tak/nie, gdy użytkownik kliknie przycisk (łącznie z kontrolką element LinkButton). Tylko wtedy, gdy kliknięto przycisk tak, akcja jest wykonywana, w przeciwnym razie anulowane. Jest to również możliwe w wzmacniake.

## <a name="steps"></a>Kroki

Najpierw wymagane jest źródło danych. Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.

W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

Następnie wymagane jest źródło danych. Ze względu na prostotę pobierane są tylko pięć pierwszych wpisów w tabeli "dostawcy AdventureWorks". Należy pamiętać, że w przypadku tworzenia źródła danych za pomocą Kreatora programu Visual Studio nazwa tabeli (`Vendors`) nie jest obecnie poprawnie naprawiona przy użyciu `Purchasing`. Następujące znaczniki są poprawne:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

To źródło danych może być następnie używane w ramach wzmacniania. Jak zwykle, Metoda `DataBinder.Eval()` pobiera dane ze źródła danych. Kontrolka `ConfirmButtonExtender` musi zostać umieszczona w sekcji `<ItemTemplate>`, aby była wyświetlana dla każdego wpisu w źródle danych.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]

[![przycisk Potwierdź pojawia się obok każdego wpisu ze źródła danych](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

Przycisk Potwierdź pojawia się obok każdego wpisu ze źródła danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-a-confirmbutton-in-a-repeater-vb.md)

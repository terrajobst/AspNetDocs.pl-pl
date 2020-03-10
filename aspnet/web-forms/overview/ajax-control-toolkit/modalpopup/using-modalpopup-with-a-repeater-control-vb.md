---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Korzystanie z kontrolki modalpopup z formantem wzmacniania (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Istnieje również możliwość użycia tego programu...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0966770f0218ca91ba7d25e7bf703bf7b005738e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613135"
---
# <a name="using-modalpopup-with-a-repeater-control-vb"></a>Używanie kontrolki ModalPopup z kontrolką elementu powtarzanego (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)

> Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Można również użyć tej kontrolki w ramach wzmacniaka.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki modalpopup w zestawie narzędzi AJAX Control oferuje prosty sposób tworzenia modalnego podręcznego przy użyciu metod po stronie klienta. Można również użyć tej kontrolki w ramach wzmacniaka.

## <a name="steps"></a>Kroki

Najpierw wymagane jest źródło danych. Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`. W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych. Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony. Aby można było używać ograniczonej ilości danych, wybieramy tylko pięć pierwszych wpisów w tabeli dostawcy bazy danych AdventureWorks. Jeśli używasz asystenta programu Visual Studio do utworzenia źródła danych, weź pod uwagę, że usterka w bieżącej wersji nie zawiera prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujące znaczniki pokazują poprawną składnię:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

Następnie Dodaj panel, który służy jako modalne menu podręczne. Zawiera kontrolkę `Button`, aby ponownie zamknąć okno podręczne:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

Aby menu podręczne działało w ramach wzmacniania, formant `ModalPopupExtender` musi być umieszczony w sekcji `<ItemTemplate>` wzmacniak. Więc panel znajduje się poza wzmacniakem, ale rozszerzenie jest wewnątrz. Oto znaczniki dla wzmacniakka:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

Następnie wszystkie elementy w źródle danych są wyświetlane z przyciskiem obok niego, który wyzwala modalne menu podręczne.

[![modalne menu podręczne może zostać wyzwolone dla każdego wpisu źródła danych](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)

Modalne menu podręczne może być wyzwalane dla każdego wpisu źródła danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](launching-a-modal-popup-window-from-server-code-vb.md)
> [dalej](handling-postbacks-from-a-modalpopup-vb.md)

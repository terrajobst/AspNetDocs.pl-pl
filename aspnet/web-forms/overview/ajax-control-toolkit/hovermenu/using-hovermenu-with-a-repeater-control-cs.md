---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Używanie kontrolki HoverMenu z kontrolką elementu powtarzanego (C#) | Dokumentacja firmy Microsoft
author: wenz
description: 'Na formant kontrolki HoverMenu z zestawu narzędzi AJAX Control Toolkit zawiera efekt proste okno podręczne: Po zatrzymaniu wskaźnika myszy nad elementem, pojawi się okno podręczne na specifi...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 33f65a132ea9f0a939ab70ac249b397ea883ac8c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068327"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a>Używanie kontrolki HoverMenu z kontrolką elementu powtarzanego (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> Na formant kontrolki HoverMenu z zestawu narzędzi AJAX Control Toolkit zawiera efekt proste okno podręczne: Po zatrzymaniu wskaźnika myszy nad elementem, pojawi się okno podręczne na określonej pozycji. Użytkownik może również użyć tej kontrolki w elemencie powtarzanym.


## <a name="overview"></a>Omówienie

`HoverMenu` Formantu w zestawu narzędzi AJAX Control Toolkit zawiera efekt proste okno podręczne: Po zatrzymaniu wskaźnika myszy nad elementem, pojawi się okno podręczne na określonej pozycji. Użytkownik może również użyć tej kontrolki w elemencie powtarzanym.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.

W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja. Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

Następnie dodaj źródła danych do strony. Aby skorzystać z ograniczoną ilością danych, możemy wybrać tylko pięć pierwszych wpisów w tabeli Dostawca bazy danych AdventureWorks. Jeśli są przy użyciu Asystenta ustawień programu Visual Studio, aby utworzyć źródło danych, należy uwzględnić następujące kwestie usterkę w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) przy użyciu `Purchasing`. Następujący kod przedstawia poprawnej składni:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

Następnie dodaj panel, który służy jako modalne okno podręczne:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

Teraz `HoverMenuExtender` trafia do odtwarzania. Tak, aby każdy element w źródle danych uzyskuje swój własny okna podręcznego, urządzenia extender muszą znajdować się w elemencie powtarzanym `<ItemTemplate>` sekcji. Oto znaczniki:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

Teraz każdy element w źródle danych wyświetla wyskakującego okienka po prawej stronie (`PopupPosition` atrybutu) z opóźnieniem 50 MS (`PopDelay` atrybutu).


[![W menu po wskazaniu wskaźnikiem pojawia się obok każdego elementu w elemencie powtarzanym](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

W menu po wskazaniu wskaźnikiem pojawia się obok każdego elementu w elemencie powtarzanym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-hovermenu-with-a-repeater-control-vb.md)

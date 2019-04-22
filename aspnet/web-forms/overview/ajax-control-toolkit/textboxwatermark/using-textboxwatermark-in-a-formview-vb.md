---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Używanie kontrolki TextBoxWatermark w kontrolce FormView (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, go czy...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 056e89b20ccab0e56b1fab422c817d842beff446
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59400844"
---
# <a name="using-textboxwatermark-in-a-formview-vb"></a>Używanie kontrolki TextBoxWatermark w kontrolce FormView (VB)

przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> Kontrolki TextBoxWatermark w zestawu narzędzi AJAX Control Toolkit rozszerza polu tekstowym, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, będzie pusta. Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. Ponadto jest to możliwe w kontrolce FormView.


## <a name="overview"></a>Omówienie

`TextBoxWatermark` Formantu w zestawu narzędzi AJAX Control Toolkit rozszerza pola tekstowego, więc, że tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, będzie pusta. Jeśli użytkownik opuści pole bez konieczności wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. Jest to możliwe w ramach również `FormView` kontroli.

## <a name="steps"></a>Kroki

Po pierwsze źródła danych jest wymagana. Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.

W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja. Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.

W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony, która obsługuje `DELETE`, `INSERT` i `UPDATE` instrukcji języka SQL. Jeśli są przy użyciu Asystenta ustawień programu Visual Studio, aby utworzyć źródło danych, należy uwzględnić następujące kwestie usterkę w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) przy użyciu `Purchasing`. Następujący kod przedstawia poprawnej składni:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Zapamiętaj nazwę (`ID`) źródła danych, ponieważ będzie on używany w `DataSourceID` właściwość `FormView` kontroli. `<InsertItemTemplate>` Części `FormView` zawiera pole tekstowe, która zostanie rozszerzona `TextBoxWatermarkExtender` kontroli. Upewnij się, że urządzenia extender znajduje się w szablonie, a nie poza nią.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Teraz, gdy użytkownik zmieni do trybu wstawiania `FormView` kontrolować, pole tekstowe dla nowego dostawcy jest wstępnie dzięki `TextBoxWatermarkExtender` kontroli. Kliknij wewnątrz pola tekstowego umożliwia tekstu wypełniacza zniknąć.


[![Znak wodny w polu pochodzi z urządzenia extender](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

Znak wodny w polu pochodzi z urządzenia extender ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-textboxwatermark-with-validation-controls-cs.md)
> [dalej](using-textboxwatermark-with-validation-controls-vb.md)

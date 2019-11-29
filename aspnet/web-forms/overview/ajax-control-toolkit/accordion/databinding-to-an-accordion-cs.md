---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Powiązanie danych z przepisami (C#) | Microsoft Docs
author: wenz
description: Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle zadeklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c28cc958a1de9844627ae16175a5aed153993a8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607269"
---
# <a name="databinding-to-an-accordion-c"></a>Powiązanie danych z kontrolką Accordion (C#)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle deklarowane na samej stronie, ale powiązanie ze źródłem danych zapewnia większą elastyczność.

## <a name="overview"></a>Omówienie

Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz. Panele są zwykle deklarowane na samej stronie, ale powiązanie ze źródłem danych zapewnia większą elastyczność.

## <a name="steps"></a>Kroki

Najpierw wymagane jest źródło danych. Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.

W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.

Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Następnie Dodaj źródło danych do strony. Aby można było używać ograniczonej ilości danych, wybieramy tylko pięć pierwszych wpisów w tabeli dostawcy bazy danych AdventureWorks. Jeśli używasz asystenta programu Visual Studio do utworzenia źródła danych, weź pod uwagę, że usterka w bieżącej wersji nie zawiera prefiksu nazwy tabeli (`Vendor`) z `Purchasing`. Następujące znaczniki pokazują poprawną składnię:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Zapamiętaj nazwę (identyfikator) źródła danych. Ta bardzo znacząca identyfikacja musi być następnie użyta we właściwości `DataSourceID` kontrolki kontroli:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

W ramach kontroli zgodnie z przepisami można dostarczyć szablony dla różnych części formantu, w tym nagłówek (`<HeaderTemplate>`) i zawartość (`<ContentTemplate>`). W ramach tych elementów po prostu dane są wyprowadzane ze źródła danych przy użyciu metody `DataBinder.Eval()`:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Po załadowaniu strony źródło danych musi być powiązane z uwzględnieniem tego kodu po stronie serwera:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Aby zawrzeć ten przykład, należy zdefiniować dwie klasy CSS, do których odwołuje się kontrolka przydzielenia (we właściwościach `HeaderCssClass` i `ContentCssClass`). Umieść następujące znaczniki w sekcji `<head>` strony:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

[![dane w postawce przychodzą bezpośrednio ze źródła danych](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Dane w postawce są dostarczane bezpośrednio ze źródła danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](dynamically-adding-an-accordion-pane-cs.md)

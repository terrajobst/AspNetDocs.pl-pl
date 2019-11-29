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
# <a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="db4e5-103">Używanie kontrolki HoverMenu z kontrolką elementu powtarzanego (VB)</span><span class="sxs-lookup"><span data-stu-id="db4e5-103">Using HoverMenu with a Repeater Control (VB)</span></span>

<span data-ttu-id="db4e5-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="db4e5-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="db4e5-105">[Pobierz kod](https://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="db4e5-105">[Download Code](https://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="db4e5-106">Kontrolka kontrolki hovermenu w zestawie narzędzi AJAX Control oferuje prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, zostanie wyświetlone okno podręczne na określonej pozycji.</span><span class="sxs-lookup"><span data-stu-id="db4e5-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="db4e5-107">Można również użyć tej kontrolki w ramach wzmacniaka.</span><span class="sxs-lookup"><span data-stu-id="db4e5-107">It is also possible to use this control within a repeater.</span></span>

## <a name="overview"></a><span data-ttu-id="db4e5-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="db4e5-108">Overview</span></span>

<span data-ttu-id="db4e5-109">Formant `HoverMenu` w zestawie narzędzi AJAX Control udostępnia prosty efekt podręczny: gdy wskaźnik myszy znajduje się nad elementem, pojawi się okno podręczne na określonej pozycji.</span><span class="sxs-lookup"><span data-stu-id="db4e5-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="db4e5-110">Można również użyć tej kontrolki w ramach wzmacniaka.</span><span class="sxs-lookup"><span data-stu-id="db4e5-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="db4e5-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="db4e5-111">Steps</span></span>

<span data-ttu-id="db4e5-112">Najpierw wymagane jest źródło danych.</span><span class="sxs-lookup"><span data-stu-id="db4e5-112">First of all, a data source is required.</span></span> <span data-ttu-id="db4e5-113">Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="db4e5-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="db4e5-114">Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="db4e5-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="db4e5-115">Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="db4e5-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="db4e5-116">Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.</span><span class="sxs-lookup"><span data-stu-id="db4e5-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="db4e5-117">W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna.</span><span class="sxs-lookup"><span data-stu-id="db4e5-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="db4e5-118">Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="db4e5-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="db4e5-119">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="db4e5-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="db4e5-120">Następnie Dodaj źródło danych do strony.</span><span class="sxs-lookup"><span data-stu-id="db4e5-120">Then, add a data source to the page.</span></span> <span data-ttu-id="db4e5-121">Aby można było używać ograniczonej ilości danych, wybieramy tylko pięć pierwszych wpisów w tabeli dostawcy bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="db4e5-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="db4e5-122">Jeśli używasz asystenta programu Visual Studio do utworzenia źródła danych, weź pod uwagę, że usterka w bieżącej wersji nie zawiera prefiksu nazwy tabeli (`Vendor`) z `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="db4e5-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="db4e5-123">Następujące znaczniki pokazują poprawną składnię:</span><span class="sxs-lookup"><span data-stu-id="db4e5-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="db4e5-124">Następnie Dodaj panel, który służy jako modalne menu podręczne:</span><span class="sxs-lookup"><span data-stu-id="db4e5-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="db4e5-125">Teraz `HoverMenuExtender` jest odtwarzany.</span><span class="sxs-lookup"><span data-stu-id="db4e5-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="db4e5-126">Aby każdy element w źródle danych otrzymywał własne okno podręczne, rozszerzenie musi zostać umieszczone w sekcji `<ItemTemplate>`ka.</span><span class="sxs-lookup"><span data-stu-id="db4e5-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="db4e5-127">Oto znacznik:</span><span class="sxs-lookup"><span data-stu-id="db4e5-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="db4e5-128">Teraz każdy element w źródle danych wyświetla okno podręczne z prawej strony (`PopupPosition` atrybutu) po opóźnieniu 50 milisekund (`PopDelay` Attribute).</span><span class="sxs-lookup"><span data-stu-id="db4e5-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>

<span data-ttu-id="db4e5-129">[![menu aktywacja pojawia się obok każdego elementu w wzmacniake](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="db4e5-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="db4e5-130">Menu aktywacja pojawia się obok każdego elementu w wzmacniake ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="db4e5-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="db4e5-131">Ubiegł</span><span class="sxs-lookup"><span data-stu-id="db4e5-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)

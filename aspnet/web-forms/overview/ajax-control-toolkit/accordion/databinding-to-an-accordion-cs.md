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
# <a name="databinding-to-an-accordion-c"></a><span data-ttu-id="465af-104">Powiązanie danych z kontrolką Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="465af-104">Databinding to an Accordion (C#)</span></span>

<span data-ttu-id="465af-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="465af-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="465af-106">[Pobierz kod](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="465af-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="465af-107">Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz.</span><span class="sxs-lookup"><span data-stu-id="465af-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="465af-108">Panele są zwykle deklarowane na samej stronie, ale powiązanie ze źródłem danych zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="465af-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="465af-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="465af-109">Overview</span></span>

<span data-ttu-id="465af-110">Kontrola zgodnie z zestawem narzędzi AJAX Control Toolkit udostępnia wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich naraz.</span><span class="sxs-lookup"><span data-stu-id="465af-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="465af-111">Panele są zwykle deklarowane na samej stronie, ale powiązanie ze źródłem danych zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="465af-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="465af-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="465af-112">Steps</span></span>

<span data-ttu-id="465af-113">Najpierw wymagane jest źródło danych.</span><span class="sxs-lookup"><span data-stu-id="465af-113">First of all, a data source is required.</span></span> <span data-ttu-id="465af-114">Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="465af-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="465af-115">Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="465af-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="465af-116">Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="465af-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="465af-117">Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.</span><span class="sxs-lookup"><span data-stu-id="465af-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="465af-118">W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna.</span><span class="sxs-lookup"><span data-stu-id="465af-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="465af-119">Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="465af-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="465af-120">Aby aktywować funkcje ASP.NET AJAX i zestawu narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w obrębie elementu `<form>`):</span><span class="sxs-lookup"><span data-stu-id="465af-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="465af-121">Następnie Dodaj źródło danych do strony.</span><span class="sxs-lookup"><span data-stu-id="465af-121">Then, add a data source to the page.</span></span> <span data-ttu-id="465af-122">Aby można było używać ograniczonej ilości danych, wybieramy tylko pięć pierwszych wpisów w tabeli dostawcy bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="465af-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="465af-123">Jeśli używasz asystenta programu Visual Studio do utworzenia źródła danych, weź pod uwagę, że usterka w bieżącej wersji nie zawiera prefiksu nazwy tabeli (`Vendor`) z `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="465af-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="465af-124">Następujące znaczniki pokazują poprawną składnię:</span><span class="sxs-lookup"><span data-stu-id="465af-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="465af-125">Zapamiętaj nazwę (identyfikator) źródła danych.</span><span class="sxs-lookup"><span data-stu-id="465af-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="465af-126">Ta bardzo znacząca identyfikacja musi być następnie użyta we właściwości `DataSourceID` kontrolki kontroli:</span><span class="sxs-lookup"><span data-stu-id="465af-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="465af-127">W ramach kontroli zgodnie z przepisami można dostarczyć szablony dla różnych części formantu, w tym nagłówek (`<HeaderTemplate>`) i zawartość (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="465af-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="465af-128">W ramach tych elementów po prostu dane są wyprowadzane ze źródła danych przy użyciu metody `DataBinder.Eval()`:</span><span class="sxs-lookup"><span data-stu-id="465af-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="465af-129">Po załadowaniu strony źródło danych musi być powiązane z uwzględnieniem tego kodu po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="465af-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="465af-130">Aby zawrzeć ten przykład, należy zdefiniować dwie klasy CSS, do których odwołuje się kontrolka przydzielenia (we właściwościach `HeaderCssClass` i `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="465af-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="465af-131">Umieść następujące znaczniki w sekcji `<head>` strony:</span><span class="sxs-lookup"><span data-stu-id="465af-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]

<span data-ttu-id="465af-132">[![dane w postawce przychodzą bezpośrednio ze źródła danych](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="465af-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="465af-133">Dane w postawce są dostarczane bezpośrednio ze źródła danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="465af-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="465af-134">Next</span><span class="sxs-lookup"><span data-stu-id="465af-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)

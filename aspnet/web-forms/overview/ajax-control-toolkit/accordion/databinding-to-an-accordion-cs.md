---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Powiązanie danych z kontrolką Accordion (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie. Panele zwykle są deklarowane w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 28e001059cb1853d21175da2a2b1af2c75364485
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59380369"
---
# <a name="databinding-to-an-accordion-c"></a><span data-ttu-id="ce1bb-104">Powiązanie danych z kontrolką Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="ce1bb-104">Databinding to an Accordion (C#)</span></span>

<span data-ttu-id="ce1bb-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ce1bb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ce1bb-106">[Pobierz program Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce1bb-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="ce1bb-107">Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="ce1bb-108">Panele zwykle są zadeklarowane w obrębie sama strona, ale powiązania ze źródłem danych zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="ce1bb-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="ce1bb-109">Overview</span></span>

<span data-ttu-id="ce1bb-110">Kontrolki właściwości Accordion na zestawu narzędzi AJAX Control Toolkit zawiera wiele okienek i umożliwia użytkownikowi wyświetlanie jednego z nich jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="ce1bb-111">Panele zwykle są zadeklarowane w obrębie sama strona, ale powiązania ze źródłem danych zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="ce1bb-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="ce1bb-112">Steps</span></span>

<span data-ttu-id="ce1bb-113">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-113">First of all, a data source is required.</span></span> <span data-ttu-id="ce1bb-114">Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="ce1bb-115">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="ce1bb-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="ce1bb-116">Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="ce1bb-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="ce1bb-117">Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="ce1bb-118">W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="ce1bb-119">Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="ce1bb-120">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="ce1bb-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="ce1bb-121">Następnie dodaj źródła danych do strony.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-121">Then, add a data source to the page.</span></span> <span data-ttu-id="ce1bb-122">Aby skorzystać z ograniczoną ilością danych, możemy wybrać tylko pięć pierwszych wpisów w tabeli Dostawca bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="ce1bb-123">Jeśli są przy użyciu Asystenta ustawień programu Visual Studio, aby utworzyć źródło danych, należy uwzględnić następujące kwestie usterkę w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) przy użyciu `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="ce1bb-124">Następujący kod przedstawia poprawnej składni:</span><span class="sxs-lookup"><span data-stu-id="ce1bb-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="ce1bb-125">Zapamiętaj nazwę (ID) źródło danych.</span><span class="sxs-lookup"><span data-stu-id="ce1bb-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="ce1bb-126">Ten identyfikator bardzo następnie musi być używany w `DataSourceID` właściwości formantu właściwości Accordion:</span><span class="sxs-lookup"><span data-stu-id="ce1bb-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="ce1bb-127">W kontrolce właściwości Accordion zapewnieniem szablonów dla różnych części kontrolki, w tym nagłówku (`<HeaderTemplate>`) i zawartości (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="ce1bb-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="ce1bb-128">W ramach tych elementów, wyświetla dane z danych źródłowych, przy użyciu `DataBinder.Eval()` metody:</span><span class="sxs-lookup"><span data-stu-id="ce1bb-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="ce1bb-129">Po załadowaniu strony właściwości accordion przy użyciu tego kodu po stronie serwera musi być powiązana ze źródłem danych:</span><span class="sxs-lookup"><span data-stu-id="ce1bb-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="ce1bb-130">Zawierania, w tym przykładzie, należy zdefiniować dwie klasy CSS, które są określone w formancie właściwości Accordion (w jego właściwościach `HeaderCssClass` i `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="ce1bb-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="ce1bb-131">Umieść następujący kod w `<head>` sekcji strony:</span><span class="sxs-lookup"><span data-stu-id="ce1bb-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![T<span data-ttu-id="ce1bb-132">dane w właściwości accordion przybywa bezpośrednio ze źródła danych]</span><span class="sxs-lookup"><span data-stu-id="ce1bb-132">he data in the accordion comes directly from the data source]</span></span>(databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

<span data-ttu-id="ce1bb-133">Dane w właściwości accordion pochodzą bezpośrednio ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce1bb-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ce1bb-134">Next</span><span class="sxs-lookup"><span data-stu-id="ce1bb-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)

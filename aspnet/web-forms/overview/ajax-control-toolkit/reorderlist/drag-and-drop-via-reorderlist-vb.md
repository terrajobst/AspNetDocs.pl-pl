---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Przeciąganie i upuszczanie za pomocą kontrolki reorderlist (VB) | Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f7c5749053d8bf587467fb1939fca05ce2872a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598629"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="f4121-103">Przeciąganie i upuszczanie za pomocą kontrolki ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="f4121-103">Drag and Drop via ReorderList (VB)</span></span>

<span data-ttu-id="f4121-104">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f4121-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f4121-105">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f4121-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="f4121-106">Formant kontrolki reorderlist w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4121-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="f4121-107">Bieżąca kolejność listy powinna być utrwalana na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f4121-107">The current order of the list shall be persisted on the server.</span></span>

## <a name="overview"></a><span data-ttu-id="f4121-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="f4121-108">Overview</span></span>

<span data-ttu-id="f4121-109">Formant `ReorderList` w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4121-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="f4121-110">Bieżąca kolejność listy powinna być utrwalana na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f4121-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="f4121-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="f4121-111">Steps</span></span>

<span data-ttu-id="f4121-112">Kontrolka `ReorderList` obsługuje Powiązywanie danych z bazy danych na listę.</span><span class="sxs-lookup"><span data-stu-id="f4121-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="f4121-113">Najlepszym rozwiązaniem jest również możliwość zapisywania zmian w kolejności elementu listy z powrotem do magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="f4121-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="f4121-114">Ten przykład używa Microsoft SQL Server 2005 Express Edition jako magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="f4121-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="f4121-115">Baza danych jest opcjonalną (i bezpłatną) częścią instalacji programu Visual Studio, w tym Express Edition.</span><span class="sxs-lookup"><span data-stu-id="f4121-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="f4121-116">Jest on również dostępny jako osobny plik do pobrania w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="f4121-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="f4121-117">W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna.</span><span class="sxs-lookup"><span data-stu-id="f4121-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="f4121-118">Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f4121-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="f4121-119">Najprostszym sposobem skonfigurowania bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="f4121-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="f4121-120">Połącz się z serwerem, kliknij dwukrotnie `Databases` i Utwórz nową bazę danych (kliknij prawym przyciskiem myszy i wybierz `New Database`) o nazwie `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="f4121-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="f4121-121">W tej bazie danych Utwórz nową tabelę o nazwie `AJAX` z następującymi czterema kolumnami:</span><span class="sxs-lookup"><span data-stu-id="f4121-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="f4121-122">`id` (klucz podstawowy, liczba całkowita, tożsamość, wartość nie jest równa NULL)</span><span class="sxs-lookup"><span data-stu-id="f4121-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="f4121-123">`char` (Char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="f4121-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="f4121-124">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="f4121-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="f4121-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="f4121-125">`position` (int, NULL)</span></span>

<span data-ttu-id="f4121-126">[![układ tabeli AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f4121-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="f4121-127">Układ tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f4121-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>

<span data-ttu-id="f4121-128">Następnie wypełnij tabelę za pomocą kilku wartości.</span><span class="sxs-lookup"><span data-stu-id="f4121-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="f4121-129">Należy zauważyć, że kolumna `position` zawiera porządek sortowania elementów.</span><span class="sxs-lookup"><span data-stu-id="f4121-129">Note that the `position` column holds the sort order of the elements.</span></span>

<span data-ttu-id="f4121-130">[![dane początkowe w tabeli AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f4121-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="f4121-131">Początkowe dane w tabeli AJAX ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f4121-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>

<span data-ttu-id="f4121-132">Następny krok wymaga wygenerowania formantu `SqlDataSource`, aby komunikować się z nową bazą danych i jej tabelą.</span><span class="sxs-lookup"><span data-stu-id="f4121-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="f4121-133">Źródło danych musi obsługiwać `SELECT` i `UPDATE` polecenia SQL.</span><span class="sxs-lookup"><span data-stu-id="f4121-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="f4121-134">W przypadku późniejszej zmiany kolejności elementów listy formant `ReorderList` automatycznie przesyła dwie wartości do polecenia `Update` źródła danych: nowe położenie i identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="f4121-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="f4121-135">W związku z tym źródło danych wymaga sekcji `<UpdateParameters>` dla tych dwóch wartości:</span><span class="sxs-lookup"><span data-stu-id="f4121-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="f4121-136">Kontrolka `ReorderList` musi ustawić następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="f4121-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="f4121-137">`AllowReorder`: Czy można zmienić rozmieszczenie elementów listy</span><span class="sxs-lookup"><span data-stu-id="f4121-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="f4121-138">`DataSourceID`: identyfikator źródła danych</span><span class="sxs-lookup"><span data-stu-id="f4121-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="f4121-139">`DataKeyField`: Nazwa kolumny klucza podstawowego w źródle danych</span><span class="sxs-lookup"><span data-stu-id="f4121-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="f4121-140">`SortOrderField`: kolumna źródła danych, która zapewnia porządek sortowania dla elementów listy</span><span class="sxs-lookup"><span data-stu-id="f4121-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="f4121-141">W sekcjach `<DragHandleTemplate>` i `<ItemTemplate>` układ listy może być dostrojony.</span><span class="sxs-lookup"><span data-stu-id="f4121-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="f4121-142">Ponadto wiązanie danych jest możliwe przy użyciu metody `Eval()`, jak pokazano tutaj:</span><span class="sxs-lookup"><span data-stu-id="f4121-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="f4121-143">Poniższe informacje o stylu CSS (przywoływane w sekcji `<DragHandleTemplate>` kontrolki `ReorderList`) sprawdzają, czy wskaźnik myszy zmienia się odpowiednio po umieszczeniu wskaźnika na uchwycie przeciągania:</span><span class="sxs-lookup"><span data-stu-id="f4121-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="f4121-144">Na koniec formant `ScriptManager` inicjuje ASP.NET AJAX dla strony:</span><span class="sxs-lookup"><span data-stu-id="f4121-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="f4121-145">Uruchom ten przykład w przeglądarce i ponownie Rozmieść elementy listy jako bity.</span><span class="sxs-lookup"><span data-stu-id="f4121-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="f4121-146">Następnie załaduj ponownie stronę i/lub zapoznaj się z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="f4121-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="f4121-147">Zmienione pozycje zostały zachowane i są również odzwierciedlane przez wartości w kolumnie `position` w bazie danych i wszystkie bez żadnego kodu, tylko przy użyciu znaczników.</span><span class="sxs-lookup"><span data-stu-id="f4121-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>

<span data-ttu-id="f4121-148">[![dane w bazie danych zmieniają się zgodnie z kolejnością nowego elementu listy](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f4121-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="f4121-149">Dane w bazie danych zmieniają się zgodnie z kolejnością nowego elementu listy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="f4121-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f4121-150">Ubiegł</span><span class="sxs-lookup"><span data-stu-id="f4121-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)

---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Przeciąganie i upuszczanie za pomocą kontrolki ReorderList (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania. Bieżący kolejność listy jest...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 988aa9252cfd93067888734006e6003347f1fb5e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59414754"
---
# <a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="c5452-104">Przeciąganie i upuszczanie za pomocą kontrolki ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="c5452-104">Drag and Drop via ReorderList (C#)</span></span>

<span data-ttu-id="c5452-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c5452-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c5452-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c5452-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="c5452-107">Kontrolki kontrolki ReorderList na zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="c5452-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="c5452-108">Bieżący kolejność listy jest utrwalony na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c5452-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="c5452-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="c5452-109">Overview</span></span>

<span data-ttu-id="c5452-110">`ReorderList` Formantu w zestawu narzędzi AJAX Control Toolkit zawiera listy, które można zmienić kolejności przez użytkownika za pomocą przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="c5452-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="c5452-111">Bieżący kolejność listy jest utrwalony na serwerze.</span><span class="sxs-lookup"><span data-stu-id="c5452-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="c5452-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="c5452-112">Steps</span></span>

<span data-ttu-id="c5452-113">`ReorderList` Kontrolka obsługuje powiązanie danych z bazy danych do listy.</span><span class="sxs-lookup"><span data-stu-id="c5452-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="c5452-114">Przede wszystkim obsługuje ona również zapisywanie zmian w kolejności element listy do magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="c5452-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="c5452-115">Ta próbka używa programu Microsoft SQL Server 2005 Express Edition do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="c5452-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="c5452-116">Baza danych jest opcjonalny (i bezpłatne) część instalacji programu Visual Studio, w tym wersja express.</span><span class="sxs-lookup"><span data-stu-id="c5452-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="c5452-117">Jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="c5452-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="c5452-118">W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="c5452-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="c5452-119">Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c5452-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="c5452-120">Najprostszym sposobem konfigurowania bazy danych jest używać programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="c5452-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="c5452-121">Połączenie z serwerem, kliknij go dwukrotnie `Databases` i Utwórz nową bazę danych (kliknij prawym przyciskiem myszy i wybierz polecenie `New Database`) o nazwie `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="c5452-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="c5452-122">W tej bazie danych, Utwórz nową tabelę o nazwie `AJAX` następujące cztery kolumny:</span><span class="sxs-lookup"><span data-stu-id="c5452-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- `id` <span data-ttu-id="c5452-123">(podstawowego klucza, liczbę całkowitą z zakresu tożsamości, not NULL)</span><span class="sxs-lookup"><span data-stu-id="c5452-123">(primary key, integer, identity, not NULL)</span></span>
- `char` <span data-ttu-id="c5452-124">(char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="c5452-124">(char(1), NULL)</span></span>
- `description` <span data-ttu-id="c5452-125">(varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="c5452-125">(varchar(50), NULL)</span></span>
- `position` <span data-ttu-id="c5452-126">(int, NULL)</span><span class="sxs-lookup"><span data-stu-id="c5452-126">(int, NULL)</span></span>


[![T<span data-ttu-id="c5452-127">Układ HE tabeli AJAX]</span><span class="sxs-lookup"><span data-stu-id="c5452-127">he layout of the AJAX table]</span></span>(drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

<span data-ttu-id="c5452-128">Układ tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c5452-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="c5452-129">Następnie wypełnij tabelę przy użyciu kilku wartości.</span><span class="sxs-lookup"><span data-stu-id="c5452-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="c5452-130">Należy pamiętać, że `position` kolumna zawiera kolejność sortowania elementów.</span><span class="sxs-lookup"><span data-stu-id="c5452-130">Note that the `position` column holds the sort order of the elements.</span></span>


[![T<span data-ttu-id="c5452-131">HE początkowe dane w tabeli AJAX]</span><span class="sxs-lookup"><span data-stu-id="c5452-131">he initial data in the AJAX table]</span></span>(drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

<span data-ttu-id="c5452-132">Początkowe dane w tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c5452-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="c5452-133">Następnym krokiem wymaga, aby wygenerować `SqlDataSource` formantu do komunikowania się z nową bazę danych i jego tabeli.</span><span class="sxs-lookup"><span data-stu-id="c5452-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="c5452-134">Źródło danych musi obsługiwać `SELECT` i `UPDATE` poleceń SQL.</span><span class="sxs-lookup"><span data-stu-id="c5452-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="c5452-135">Po zmianie kolejności elementów listy `ReorderList` formant automatycznie przesyła dwie wartości do źródła danych `Update` polecenia: nowe miejsce i identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="c5452-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="c5452-136">W związku z tym, źródła danych, potrzeb `<UpdateParameters>` sekcję, aby te dwie wartości:</span><span class="sxs-lookup"><span data-stu-id="c5452-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="c5452-137">`ReorderList` Kontroli musi ustawić następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="c5452-137">The `ReorderList` control needs to set the following attributes:</span></span>

- `AllowReorder`<span data-ttu-id="c5452-138">: Czy można przestawiać elementów listy</span><span class="sxs-lookup"><span data-stu-id="c5452-138">: Whether the list items may be rearranged</span></span>
- `DataSourceID`<span data-ttu-id="c5452-139">: Identyfikator źródła danych</span><span class="sxs-lookup"><span data-stu-id="c5452-139">: The ID of the data source</span></span>
- `DataKeyField`<span data-ttu-id="c5452-140">: Nazwa kolumny klucza podstawowego w źródle danych</span><span class="sxs-lookup"><span data-stu-id="c5452-140">: The name of the primary key column in the data source</span></span>
- `SortOrderField`<span data-ttu-id="c5452-141">: Kolumny źródła danych, zapewniająca porządek sortowania dla elementów listy</span><span class="sxs-lookup"><span data-stu-id="c5452-141">: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="c5452-142">W `<DragHandleTemplate>` i `<ItemTemplate>` sekcjach układ listy można dostosować.</span><span class="sxs-lookup"><span data-stu-id="c5452-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="c5452-143">Wiązanie danych jest także możliwe przy użyciu `Eval()` metody, jak pokazano tutaj:</span><span class="sxs-lookup"><span data-stu-id="c5452-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="c5452-144">Następujące arkusze CSS stylu informacji (zawartymi w `<DragHandleTemplate>` części `ReorderList` kontroli) upewnia się, że wskaźnik myszy zmienia się odpowiednio kiedy jest przemieszczane nad przeciągnij uchwyt:</span><span class="sxs-lookup"><span data-stu-id="c5452-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="c5452-145">Na koniec `ScriptManager` kontroli inicjuje ASP.NET AJAX dla strony:</span><span class="sxs-lookup"><span data-stu-id="c5452-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="c5452-146">Uruchomić ten przykład w przeglądarce i nieco zmienić kolejność elementów listy.</span><span class="sxs-lookup"><span data-stu-id="c5452-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="c5452-147">Następnie ponownie załaduj stronę i/lub przyjrzenie się firmie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c5452-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="c5452-148">Zmieniony pozycji została zachowana, a także są odzwierciedlane według wartości w `position` kolumny w bazie danych, które wszystko to bez żadnego kodu tylko za pomocą znaczników.</span><span class="sxs-lookup"><span data-stu-id="c5452-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


[![T<span data-ttu-id="c5452-149">on dane zmian w bazie danych zgodnie z nowego elementu listy.]</span><span class="sxs-lookup"><span data-stu-id="c5452-149">he data in the database changes according to the new list item order]</span></span>(drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

<span data-ttu-id="c5452-150">Dane w zmian w bazie danych zgodnie z listą nowych elementów zamówienia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="c5452-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c5452-151">[Poprzednie](using-postbacks-with-reorderlist-cs.md)
> [dalej](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c5452-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>

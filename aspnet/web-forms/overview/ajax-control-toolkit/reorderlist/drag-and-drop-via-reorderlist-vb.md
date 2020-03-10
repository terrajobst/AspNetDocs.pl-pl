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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553922"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a>Przeciąganie i upuszczanie za pomocą kontrolki ReorderList (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Formant kontrolki reorderlist w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Bieżąca kolejność listy powinna być utrwalana na serwerze.

## <a name="overview"></a>Omówienie

Formant `ReorderList` w zestawie narzędzi AJAX Control zawiera listę, którą można zmienić za pomocą przeciągania i upuszczania przez użytkownika. Bieżąca kolejność listy powinna być utrwalana na serwerze.

## <a name="steps"></a>Kroki

Kontrolka `ReorderList` obsługuje Powiązywanie danych z bazy danych na listę. Najlepszym rozwiązaniem jest również możliwość zapisywania zmian w kolejności elementu listy z powrotem do magazynu danych.

Ten przykład używa Microsoft SQL Server 2005 Express Edition jako magazynu danych. Baza danych jest opcjonalną (i bezpłatną) częścią instalacji programu Visual Studio, w tym Express Edition. Jest on również dostępny jako osobny plik do pobrania w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.

Najprostszym sposobem skonfigurowania bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Połącz się z serwerem, kliknij dwukrotnie `Databases` i Utwórz nową bazę danych (kliknij prawym przyciskiem myszy i wybierz `New Database`) o nazwie `Tutorials`.

W tej bazie danych Utwórz nową tabelę o nazwie `AJAX` z następującymi czterema kolumnami:

- `id` (klucz podstawowy, liczba całkowita, tożsamość, wartość nie jest równa NULL)
- `char` (Char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)

[![układ tabeli AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

Układ tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image3.png))

Następnie wypełnij tabelę za pomocą kilku wartości. Należy zauważyć, że kolumna `position` zawiera porządek sortowania elementów.

[![dane początkowe w tabeli AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Początkowe dane w tabeli AJAX ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image6.png))

Następny krok wymaga wygenerowania formantu `SqlDataSource`, aby komunikować się z nową bazą danych i jej tabelą. Źródło danych musi obsługiwać `SELECT` i `UPDATE` polecenia SQL. W przypadku późniejszej zmiany kolejności elementów listy formant `ReorderList` automatycznie przesyła dwie wartości do polecenia `Update` źródła danych: nowe położenie i identyfikator elementu. W związku z tym źródło danych wymaga sekcji `<UpdateParameters>` dla tych dwóch wartości:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

Kontrolka `ReorderList` musi ustawić następujące atrybuty:

- `AllowReorder`: Czy można zmienić rozmieszczenie elementów listy
- `DataSourceID`: identyfikator źródła danych
- `DataKeyField`: Nazwa kolumny klucza podstawowego w źródle danych
- `SortOrderField`: kolumna źródła danych, która zapewnia porządek sortowania dla elementów listy

W sekcjach `<DragHandleTemplate>` i `<ItemTemplate>` układ listy może być dostrojony. Ponadto wiązanie danych jest możliwe przy użyciu metody `Eval()`, jak pokazano tutaj:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Poniższe informacje o stylu CSS (przywoływane w sekcji `<DragHandleTemplate>` kontrolki `ReorderList`) sprawdzają, czy wskaźnik myszy zmienia się odpowiednio po umieszczeniu wskaźnika na uchwycie przeciągania:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Na koniec formant `ScriptManager` inicjuje ASP.NET AJAX dla strony:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Uruchom ten przykład w przeglądarce i ponownie Rozmieść elementy listy jako bity. Następnie załaduj ponownie stronę i/lub zapoznaj się z bazą danych. Zmienione pozycje zostały zachowane i są również odzwierciedlane przez wartości w kolumnie `position` w bazie danych i wszystkie bez żadnego kodu, tylko przy użyciu znaczników.

[![dane w bazie danych zmieniają się zgodnie z kolejnością nowego elementu listy](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Dane w bazie danych zmieniają się zgodnie z kolejnością nowego elementu listy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Wstecz](using-postbacks-with-reorderlist-vb.md)

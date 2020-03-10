---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Używanie wartości ciągu zapytania do filtrowania danych przy użyciu powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639098"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Używanie wartości ciągu zapytania do filtrowania danych przy użyciu powiązania modelu i formularzy sieci Web

Autor [FitzMacken](https://github.com/tfitzmac)

> Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource). Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak przekazać wartość w ciągu zapytania i użyć tej wartości do pobierania danych za pomocą powiązania modelu.
> 
> Ten samouczek jest oparty na projekcie utworzonym w [poprzednich](retrieving-data.md) częściach serii.
> 
> Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB. Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.

## <a name="what-youll-build"></a>Co będziesz kompilować

W tym samouczku przedstawiono następujące instrukcje:

1. Dodaj nową stronę, aby wyświetlić zarejestrowane kursy dla ucznia
2. Pobierz zarejestrowane kursy dla wybranego ucznia na podstawie wartości w ciągu zapytania
3. Dodaj hiperlink z wartością ciągu zapytania z widoku siatki do nowej strony

Kroki opisane w tym samouczku są stosunkowo podobne do tego, co zostało zrobione w poprzednim [samouczku](sorting-paging-and-filtering-data.md) , aby odfiltrować wyświetlanych uczniów w oparciu o wybór użytkownika na liście rozwijanej. W tym samouczku użyto atrybutu **Control** w metodzie SELECT, aby określić, że wartość parametru pochodzi z formantu. W tym samouczku użyjesz atrybutu **QueryString** w metodzie SELECT, aby określić, że wartość parametru pochodzi z ciągu zapytania.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Dodaj nową stronę do wyświetlania kursów ucznia

Dodaj nowy formularz sieci Web, który używa strony głównej witryny. Master, a następnie nazwij **kursy**strony.

W pliku **kursy. aspx** Dodaj widok siatki, aby wyświetlić kursy dla wybranego ucznia.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Zdefiniuj metodę select

W **courses.aspx.cs**zostanie dodana Metoda SELECT o nazwie określonej we właściwości **SelectMethod** widoku siatki. W tej metodzie zdefiniujesz zapytanie do pobierania kursów studenta i określisz, że parametr pochodzi z wartości ciągu zapytania o tej samej nazwie co parametr.

Najpierw należy dodać następujące instrukcje **using** .

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Następnie Dodaj następujący kod do Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Atrybut QueryString oznacza, że wartość ciągu zapytania o nazwie StudentID jest automatycznie przypisywana do parametru w tej metodzie.

## <a name="add-hyperlink-with-query-string-value"></a>Dodaj hiperlink z wartością ciągu zapytania

W widoku siatki w programie Students. aspx dodasz pole Hyperlink, które łączy się ze swoją nową stroną kursów. Hiperłącze będzie zawierać wartość ciągu zapytania z identyfikatorem studenta.

W polu Students. aspx Dodaj następujące pole do kolumn w widoku siatki tuż poniżej pola dla łącznej liczby kredytów.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Uruchom aplikację i zwróć uwagę, że widok siatki zawiera teraz link kursy.

![Dodaj hiperlink](using-query-string-values-to-retrieve-data/_static/image1.png)

Po kliknięciu jednego z tych linków zobaczysz zarejestrowane kursy tego ucznia.

![Pokaż kursy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Podsumowanie

W tym samouczku Dodano łącze z wartością ciągu zapytania. Użyto tej wartości ciągu zapytania dla wartości parametru w metodzie SELECT.

W następnym [samouczku](adding-business-logic-layer.md)kod jest przenoszony z plików związanych z kodem do warstwy logiki biznesowej i warstwy dostępu do danych.

> [!div class="step-by-step"]
> [Poprzednie](integrating-jquery-ui.md)
> [dalej](adding-business-logic-layer.md)

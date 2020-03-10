---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Dodawanie warstwy logiki biznesowej do projektu używającego powiązań modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634835"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Dodawanie warstwy logiki biznesowej do projektu używającego powiązań modelu i formularzy sieci Web

Autor [FitzMacken](https://github.com/tfitzmac)

> Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource). Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak używać powiązania modelu z warstwą logiki biznesowej. Należy ustawić składową OnCallingDataMethods, aby określić, że obiekt inny niż bieżąca strona jest używany do wywoływania metod danych.
> 
> Ten samouczek jest oparty na projekcie utworzonym w [poprzednich](retrieving-data.md) częściach serii.
> 
> Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB. Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.

## <a name="what-youll-build"></a>Co będziesz kompilować

Powiązanie modelu umożliwia umieszczenie kodu interakcji z danymi w pliku związanym z kodem dla strony sieci Web lub w oddzielnej klasie logiki biznesowej. W poprzednich samouczkach pokazano, jak używać plików związanych z kodem dla kodu interakcji z danymi. Ta metoda działa w przypadku małych lokacji, ale może prowadzić do powtarzania kodu i większego trudności podczas konserwacji dużej lokacji. Może być również bardzo trudne do programistycznego testowania kodu, który znajduje się w pliku znajdującym się w kodzie, ponieważ nie ma warstwy abstrakcji.

Aby scentralizować kod interakcji z danymi, można utworzyć warstwę logiki biznesowej, która zawiera całą logikę interakcji z danymi. Następnie Wywołaj warstwę logiki biznesowej ze stron sieci Web. W tym samouczku pokazano, jak przenieść cały kod zapisany w poprzednich samouczkach do warstwy logiki biznesowej, a następnie użyć tego kodu ze stron.

W tym samouczku przedstawiono następujące instrukcje:

1. Przenoszenie kodu z plików związanych z kodem do warstwy logiki biznesowej
2. Zmień kontrolki powiązane z danymi, aby wywoływać metody z warstwy logiki biznesowej

## <a name="create-business-logic-layer"></a>Tworzenie warstwy logiki biznesowej

Teraz utworzysz klasę, która jest wywoływana ze stron sieci Web. Metody w tej klasie wyglądają podobnie jak metody używane w poprzednich samouczkach i zawierają atrybuty dostawcy wartości.

Najpierw Dodaj nowy folder o nazwie **logiki biznesowej**.

![Dodaj folder](adding-business-logic-layer/_static/image1.png)

W folderze LOGIKI biznesowej Utwórz nową klasę o nazwie **SchoolBL.cs**. Będzie zawierać wszystkie operacje na danych, które pierwotnie znajdowały się w plikach związanych z kodem. Metody są prawie takie same jak metody w pliku związanym z kodem, ale zawierają pewne zmiany.

Najważniejszym zmianą w notatce jest to, że kod nie jest już wykonywany z poziomu instancji klasy **Page** . Klasa Page zawiera metodę **TryUpdateModel** i Właściwość **ModelState** . Gdy ten kod jest przenoszony do warstwy logiki biznesowej, nie ma już wystąpienia klasy Page do wywołania tych elementów członkowskich. Aby obejść ten problem, należy dodać parametr **ModelMethodContext** do dowolnej metody, która uzyskuje dostęp do TryUpdateModel lub ModelState. Ten parametr ModelMethodContext służy do wywoływania TryUpdateModel lub pobierania ModelState. Nie trzeba zmieniać żadnych elementów na stronie sieci Web, aby uwzględnić ten nowy parametr.

Zastąp kod w SchoolBL.cs następującym kodem.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Popraw istniejące strony, aby pobrać dane z warstwy logiki biznesowej

Na koniec przekonwertujesz strony uczniowie. aspx, addstudents. aspx i kursy. aspx z używania zapytań w pliku związanym z kodem, aby użyć warstwy logiki biznesowej.

W plikach związanych z kodem dla studentów, addstudenta i kursów Usuń lub Skomentuj następujące metody zapytania:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

W pliku związanym z kodem nie powinien już znajdować się kod, który odnosi się do operacji na danych.

Program obsługi zdarzeń **OnCallingDataMethods** umożliwia określenie obiektu, który ma być używany w metodach danych. W uczniów. aspx Dodaj wartość dla tego programu obsługi zdarzeń i Zmień nazwy metod danych na nazwy metod w klasie logiki biznesowej.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

W pliku związanym z kodem dla uczniów. aspx Zdefiniuj program obsługi zdarzeń dla zdarzenia CallingDataMethods. W tym obsłudze zdarzeń należy określić klasę logiki biznesowej dla operacji na danych.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

W polu addstudent. aspx wprowadź podobne zmiany.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

W polu kursy. aspx Zmień podobne zmiany.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Uruchom aplikację i zwróć uwagę, że wszystkie strony działają tak jak wcześniej. Logika walidacji działa poprawnie.

## <a name="conclusion"></a>Podsumowanie

W tym samouczku utworzysz swoją aplikację, aby użyć warstwy dostępu do danych i warstwy logiki biznesowej. Określono, że kontrolki danych używają obiektu, który nie jest bieżącą stroną dla operacji na danych.

> [!div class="step-by-step"]
> [Wstecz](using-query-string-values-to-retrieve-data.md)

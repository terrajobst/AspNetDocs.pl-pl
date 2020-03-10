---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortowanie, stronicowanie i filtrowanie danych za pomocą powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548063"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sortowanie, stronicowanie i filtrowanie danych za pomocą powiązania modelu i formularzy sieci Web

Autor [FitzMacken](https://github.com/tfitzmac)

> Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource). Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak dodać sortowanie, stronicowanie i filtrowanie danych za pomocą powiązania modelu.
> 
> Ten samouczek jest oparty na projekcie utworzonym w pierwszej [części](retrieving-data.md) serii.
> 
> Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB. Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.

## <a name="what-youll-build"></a>Co będziesz kompilować

W tym samouczku przedstawiono następujące instrukcje:

1. Włącz sortowanie i stronicowanie danych
2. Włącz filtrowanie danych na podstawie wyboru przez użytkownika

## <a name="add-sorting"></a>Dodaj sortowanie

Włączenie sortowania w widoku GridView jest bardzo proste. W pliku student. aspx po prostu ustaw **AllowSorting** na **true** w widoku GridView. Nie trzeba ustawiać wartości **'sortexpression** dla każdej kolumny, ponieważ pole DataField jest automatycznie używane. Widok GridView modyfikuje zapytanie, aby uwzględnić porządkowanie danych według wybranej wartości. Wyróżniony kod poniżej pokazuje dodanie, które należy wykonać, aby włączyć sortowanie.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Uruchom aplikację sieci Web i przetestuj sortowanie rekordów uczniów według wartości w różnych kolumnach.

![Sortuj uczniów](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Dodaj stronicowanie

Włączenie stronicowania jest również bardzo proste. W widoku GridView ustaw właściwość **Właściwość AllowPaging** na **wartość true** i ustaw właściwość **PageSize** na liczbę rekordów, które mają być wyświetlane na każdej stronie. W tym samouczku można ustawić wartość 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Uruchom aplikację sieci Web i zwróć uwagę, że teraz rekordy są podzielone na wiele stron, które nie mają więcej niż 4 rekordów wyświetlanych na jednej stronie.

![Dodaj stronicowanie](sorting-paging-and-filtering-data/_static/image4.png)

Opóźnione wykonywanie zapytań poprawia wydajność aplikacji. Zamiast pobierać cały zestaw danych, GridView modyfikuje zapytanie w celu pobrania tylko rekordów dla bieżącej strony.

## <a name="filter-records-by-user-selection"></a>Filtruj rekordy według wyboru użytkownika

Powiązanie modelu dodaje kilka atrybutów, które umożliwiają określenie sposobu ustawiania wartości parametru w metodzie powiązania modelu. Te atrybuty znajdują się w przestrzeni nazw **System. Web. ModelBinding** . Dostępne są następujące ustawienia:

- Kontrolka
- Plik cookie
- Formularz
- Profil
- Ciąg zapytania
- RouteData
- Sesja
- UserProfile
- Stan widoku

W tym samouczku zostanie użyta wartość kontrolki do filtrowania, które rekordy są wyświetlane w widoku GridView. Dodasz atrybut **Control** do utworzonej wcześniej metody zapytania. W [kolejnym](using-query-string-values-to-retrieve-data.md) samouczku zastosujesz atrybut **QueryString** do parametru, aby określić, że wartość parametru pochodzi z wartości ciągu zapytania.

Najpierw, powyżej podsumowania walidacji, dodać listę rozwijaną umożliwiającą filtrowanie, które studenci są pokazywane.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

W pliku związanym z kodem zmodyfikuj metodę select, aby otrzymać wartość z kontrolki, i Ustaw nazwę parametru na nazwę kontrolki, która zawiera wartość.

Należy dodać instrukcję **using** dla przestrzeni nazw **System. Web. ModelBinding** , aby rozpoznać atrybut Control.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Poniższy kod pokazuje, że metoda SELECT została przeprowadzona ponownie w celu filtrowania zwracanych danych na podstawie wartości listy rozwijanej. Dodanie atrybutu kontrolki przed parametrem określa, że wartość tego parametru pochodzi z kontrolki o tej samej nazwie.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Uruchom aplikację sieci Web i wybierz różne wartości z listy rozwijanej, aby przefiltrować listę studentów.

![Filtruj uczniów](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Podsumowanie

W tym samouczku włączono sortowanie i stronicowanie danych. Można również włączyć filtrowanie danych przez wartość kontrolki.

W następnym [samouczku](integrating-jquery-ui.md) zostanie wzbogacenie interfejsu użytkownika przez integrację WIDŻETU interfejsu użytkownika jQuery z szablonem danych dynamicznych.

> [!div class="step-by-step"]
> [Poprzednie](updating-deleting-and-creating-data.md)
> [dalej](integrating-jquery-ui.md)

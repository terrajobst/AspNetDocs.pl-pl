---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualizowanie, usuwanie i tworzenie danych za pomocą powiązania modelu i formularzy sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586647"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aktualizowanie, usuwanie i tworzenie danych za pomocą powiązania modelu i formularzy sieci Web

Autor [FitzMacken](https://github.com/tfitzmac)

> Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource). Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak tworzyć, aktualizować i usuwać dane przy użyciu powiązania modelu. Należy ustawić następujące właściwości:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Te właściwości otrzymują nazwę metody, która obsługuje odpowiednią operację. W ramach tej metody należy zapewnić logikę do współpracy z danymi.
> 
> Ten samouczek jest oparty na projekcie utworzonym w pierwszej [części](retrieving-data.md) serii.
> 
> Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB. Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.

## <a name="what-youll-build"></a>Co będziesz kompilować

W tym samouczku przedstawiono następujące instrukcje:

1. Dodawanie dynamicznych szablonów danych
2. Włącz aktualizowanie i usuwanie danych za poorednictwem metod powiązania modelu
3. Zastosuj reguły sprawdzania poprawności danych — Włącz tworzenie nowego rekordu w bazie danych

## <a name="add-dynamic-data-templates"></a>Dodawanie dynamicznych szablonów danych

Aby zapewnić najlepsze środowisko użytkownika i zminimalizować powtarzanie kodu, będziesz używać dynamicznych szablonów danych. Możesz łatwo zintegrować wstępnie skompilowane szablony danych dynamicznych z istniejącą lokacją, instalując pakiet NuGet.

Na stronie **Zarządzanie pakietami NuGet**zainstaluj **DynamicDataTemplatesCS**.

![dynamiczne szablony danych](updating-deleting-and-creating-data/_static/image1.png)

Zwróć uwagę, że projekt zawiera teraz folder o nazwie **DynamicData**. W tym folderze znajdują się szablony, które są automatycznie stosowane do formantów dynamicznych w formularzach sieci Web.

![folder danych dynamicznych](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Włącz aktualizowanie i usuwanie

Umożliwienie użytkownikom aktualizowania i usuwania rekordów w bazie danych jest bardzo podobne do procesu pobierania danych. We właściwościach **UpdateMethod** i **DeleteMethod** należy określić nazwy metod, które wykonują te operacje. Za pomocą kontrolki GridView można także określić automatyczne generowanie przycisków Edytuj i Usuń. Poniższy wyróżniony kod pokazuje Dodatki do kodu GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

W pliku związanym z kodem Dodaj instrukcję using dla elementu **System. Data. Entity. Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Następnie Dodaj następujące metody Update i DELETE.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Metoda **TryUpdateModel** stosuje odpowiednie wartości powiązane z danymi z formularza sieci Web do elementu danych. Element danych jest pobierany na podstawie wartości parametru ID.

## <a name="enforce-validation-requirements"></a>Wymuś wymagania dotyczące weryfikacji

Atrybuty walidacji zastosowane do właściwości FirstName, LastName i Year w klasie student są automatycznie wymuszane podczas aktualizacji danych. Kontrolki DynamicField dodają moduł sprawdzania poprawności klienta i serwera na podstawie atrybutów walidacji. Właściwości FirstName i LastName są wymagane. Długość FirstName nie może przekraczać 20 znaków, a LastName nie może przekraczać 40 znaków. Rok musi być prawidłową wartością wyliczenia AcademicYear.

Jeśli użytkownik narusza jedno z wymagań dotyczących weryfikacji, aktualizacja nie zostanie przebiegać. Aby wyświetlić komunikat o błędzie, Dodaj kontrolkę podsumowania walidacji nad elementem GridView. Aby wyświetlić błędy walidacji z powiązania modelu, ustaw dla właściwości **ShowModelStateErrors** **wartość true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Uruchom aplikację sieci Web i zaktualizuj i Usuń wszystkie rekordy.

![Aktualizowanie danych](updating-deleting-and-creating-data/_static/image3.png)

Zauważ, że w trybie edycji wartość właściwości Year jest automatycznie renderowany jako lista rozwijana. Właściwość Year jest wartością wyliczenia, a dynamiczny szablon danych dla wartości wyliczenia określa listę rozwijaną do edycji. Ten szablon można znaleźć, otwierając **wyliczenie\_Edytuj plik. ascx** w folderze **DynamicData**/**FieldTemplates** .

W przypadku podania prawidłowych wartości aktualizacja zostanie zakończona pomyślnie. W przypadku naruszenia jednego z wymagań dotyczących weryfikacji aktualizacja nie zostanie przebiegać i zostanie wyświetlony komunikat o błędzie powyżej siatki.

![komunikat o błędzie](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Dodaj nowe rekordy

Formant GridView nie zawiera właściwości **InsertMethod** i dlatego nie można go użyć do dodania nowego rekordu z powiązaniem modelu. Właściwość InsertMethod można znaleźć w kontrolkach **FormView**, **DetailsView**lub **ListView** . W tym samouczku użyjesz kontrolki FormView, aby dodać nowy rekord.

Najpierw Dodaj link do nowej strony, która zostanie utworzona w celu dodania nowego rekordu. Powyżej podsumowania walidacji Dodaj:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Nowe łącze pojawi się u góry zawartości strony uczniów.

![nowy link](updating-deleting-and-creating-data/_static/image5.png)

Następnie Dodaj nowy formularz sieci Web przy użyciu strony głównej, a następnie nadaj mu nazwę **addstudent**. Wybierz pozycję site. Master jako stronę wzorcową.

Zostaną wyrenderowane pola umożliwiające dodanie nowego ucznia przy użyciu kontrolki **DynamicControl** . Kontrolka DynamicControl renderuje te edytowalne właściwości w klasie określonej we właściwości ItemType. Właściwość StudentID została oznaczona przy użyciu atrybutu **[ScaffoldColumn (false)]** , więc nie jest renderowana. W symbolu zastępczym kontrolka mainContent strony addstudent Dodaj następujący kod.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

W pliku związanym z kodem (AddStudent.aspx.cs) Dodaj instrukcję **using** dla przestrzeni nazw **ContosoUniversityModelBinding. models** .

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Następnie Dodaj następujące metody, aby określić, jak wstawić nowy rekord i program obsługi zdarzeń dla przycisku Anuluj.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Zapisz wszystkie zmiany.

Uruchom aplikację sieci Web i Utwórz nowego ucznia.

![Dodaj nowego ucznia](updating-deleting-and-creating-data/_static/image6.png)

Kliknij przycisk **Wstaw** , aby zauważyć, że nowy student został utworzony.

![Wyświetl nowych uczniów](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Podsumowanie

W ramach tego samouczka włączono aktualizowanie, usuwanie i tworzenie danych. Reguły sprawdzania poprawności są stosowane podczas korzystania z danych.

W następnym [samouczku](sorting-paging-and-filtering-data.md) w tej serii zostanie włączone sortowanie, stronicowanie i filtrowanie danych.

> [!div class="step-by-step"]
> [Poprzednie](retrieving-data.md)
> [dalej](sorting-paging-and-filtering-data.md)

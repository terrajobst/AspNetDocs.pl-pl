---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrowanie interfejsu użytkownika JQuery DatePicker z powiązaniem modelu i formularzami sieci Web | Microsoft Docs
author: Rick-Anderson
description: Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642479"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrowanie interfejsu użytkownika JQuery DatePicker z powiązaniem modelu i formularzami sieci Web

Autor [FitzMacken](https://github.com/tfitzmac)

> Ta seria samouczków pokazuje podstawowe aspekty używania powiązania modelu z projektem formularzy sieci Web ASP.NET. Powiązanie modelu sprawia, że interakcje danych są bardziej proste, niż w przypadku obiektów źródła danych (np. ObjectDataSource lub kontrolki SqlDataSource). Ta seria rozpoczyna się od materiału wstępnego i przenosi do bardziej zaawansowanych koncepcji w kolejnych samouczkach.
> 
> W tym samouczku pokazano, jak dodać [widżet DatePicker](http://jqueryui.com/datepicker/) interfejsu użytkownika jQuery do formularza sieci Web, a następnie użyć powiązania modelu w celu zaktualizowania bazy danych przy użyciu wybranej wartości.
> 
> Ten samouczek jest oparty na projekcie utworzonym w [pierwszej](retrieving-data.md) i [drugiej](updating-deleting-and-creating-data.md) części serii.
> 
> Możesz [pobrać](https://go.microsoft.com/fwlink/?LinkId=286116) kompletny projekt w C# języku lub VB. Kod do pobrania współdziała z programem Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inny niż szablon Visual Studio 2013 przedstawiony w tym samouczku.

## <a name="what-youll-build"></a>Co będziesz kompilować

W tym samouczku przedstawiono następujące instrukcje:

1. Dodaj właściwość do modelu, aby zarejestrować datę rejestracji ucznia
2. Umożliwia użytkownikowi wybranie daty rejestracji za pomocą widżetu DatePicker interfejsu użytkownika JQuery
3. Wymuś reguły walidacji dla daty rejestracji

Widżet DatePicker interfejsu użytkownika JQuery pozwala użytkownikom łatwo wybrać datę z kalendarza, który pojawia się, gdy użytkownik współdziała z polem. Korzystanie z tego widżetu może być wygodniejsze dla użytkowników niż Ręczne wpisywanie daty. Integrowanie widżetu DatePicker ze stroną, która używa powiązania modelu dla operacji na danych, wymaga jedynie niewielkiej ilości dodatkowego nakładu pracy.

## <a name="add-a-new-property-to-the-model"></a>Dodaj nową właściwość do modelu

Najpierw należy dodać właściwość **DateTime** do modelu studenta i zmigrować tę zmianę do bazy danych. Otwórz **UniversityModels.cs**i Dodaj wyróżniony kod do modelu ucznia.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Element **RangeAttribute** jest dołączany do wymuszania reguł walidacji dla właściwości. W tym samouczku przyjęto założenie, że firma Contoso University była oparta na 1 stycznia 2013 i dlatego wcześniejsze daty rejestracji są nieprawidłowe.

W oknie Zarządzanie pakietami Dodaj migrację, uruchamiając polecenie **Add-Migration AddEnrollmentDate**. Zwróć uwagę, że kod migracji dodaje nową kolumnę datetime do tabeli student. Aby dopasować wartość określoną w polu RangeAttribute, Dodaj wartość domyślną dla nowej kolumny, jak pokazano w wyróżnionym kodzie poniżej.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Zapisz zmiany w pliku migracji.

Nie trzeba ponownie wypełniać danych. W związku z tym Otwórz **Configuration.cs** w folderze migrations i Usuń lub Skomentuj kod w metodzie **inicjatora** . Zapisz i zamknij plik.

Teraz uruchom polecenie **Update-Database**. Zwróć uwagę, że kolumna już istnieje w bazie danych, a wszystkie istniejące rekordy mają wartość domyślną dla EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Dodaj kontrolki dynamiczne dla daty rejestracji

Teraz dodasz kontrolki do wyświetlania i edytowania daty rejestracji. W tym momencie wartość jest edytowana za pomocą pola tekstowego. W dalszej części tego samouczka zmienisz pole tekstowe do widżetu JQuery DatePicker.

Najpierw należy zauważyć, że nie trzeba wprowadzać zmian w pliku **addstudent. aspx** . Kontrolka DynamicControl automatycznie renderuje nową właściwość.

Otwórz **uczniów. aspx**i Dodaj następujący wyróżniony kod.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Uruchom aplikację i zwróć uwagę, że wartość daty rejestracji można ustawić, wpisując datę. Podczas dodawania nowego ucznia:

![Ustaw datę](integrating-jquery-ui/_static/image1.png)

Lub, edytując istniejącą wartość:

![Edytuj datę](integrating-jquery-ui/_static/image2.png)

Wpisywanie daty działa, ale może to nie być środowisko klienta, które chcesz podać. W następnej sekcji zostanie włączone wybieranie daty przez kalendarz.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Zainstaluj pakiet NuGet do pracy z interfejsem użytkownika JQuery

Pakiet NuGet **interfejsu użytkownika soku** umożliwia łatwą integrację WIDŻETÓW interfejsu użytkownika jQuery z aplikacją sieci Web. Aby użyć tego pakietu, zainstaluj go za pomocą narzędzia NuGet.

![Dodaj interfejs użytkownika soku](integrating-jquery-ui/_static/image3.png)

Instalowana wersja interfejsu użytkownika soku może powodować konflikt z wersją JQuery w aplikacji. Przed kontynuowaniem pracy z tym samouczkiem spróbuj uruchomić aplikację. Jeśli wystąpi błąd języka JavaScript, należy uzgodnić wersję JQuery. Możesz dodać oczekiwaną wersję platformy JQuery do folderu Scripts (wersja 1.8.2 w czasie pisania tego samouczka) lub w obszarze site. Master określ ścieżkę do pliku JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Dostosuj szablon DateTime w celu uwzględnienia widżetu DatePicker

Do edycji wartości DateTime zostanie dodany widżet DatePicker do szablonu danych dynamicznych. Przez dodanie widżetu do szablonu, jest on automatycznie renderowany w formularzu dodawania nowego ucznia i w widoku siatki do edytowania uczniów. Otwórz **element DateTime\_Edit. ascx**i Dodaj następujący wyróżniony kod.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

W pliku związanym z kodem należy określić minimalną i maksymalną datę dla DatePicker. Ustawienie tych wartości uniemożliwi użytkownikom przechodzenie do nieprawidłowych dat. Wartości minimalne i maksymalne zostaną pobrane z **klasy RangeAttribute** we właściwości DateTime, jeśli została podana. Otwórz **element DateTime\_Edit.ascx.cs**i Dodaj następujący wyróżniony kod do strony\_metody ładowania.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Uruchom aplikację sieci Web i przejdź do strony addstudent. Podaj wartości pól i Zauważ, że po kliknięciu pola tekstowego dla daty rejestracji zostanie wyświetlony kalendarz.

![wybór daty](integrating-jquery-ui/_static/image4.png)

Wybierz datę, a następnie kliknij pozycję **Wstaw**. Element RangeAttribute wymusza weryfikację na serwerze. Ustawiając właściwość minDate w DatePicker, należy również zastosować weryfikację na kliencie. Kalendarz nie zezwala użytkownikowi na przechodzenie do daty przed wartością minDate.

Podczas edytowania rekordu w widoku siatki zostanie również wyświetlony kalendarz.

![DatePicker w GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Podsumowanie

W tym samouczku dowiesz się, jak dołączyć widżet JQuery do formularza sieci Web, który używa powiązania modelu.

W następnym [samouczku](using-query-string-values-to-retrieve-data.md)podczas wybierania danych zostanie użyta wartość ciągu zapytania.

> [!div class="step-by-step"]
> [Poprzednie](sorting-paging-and-filtering-data.md)
> [dalej](using-query-string-values-to-retrieve-data.md)

---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Dodawanie weryfikacji do modelu (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 3eef7934fac8c16dc7517ca16cab67c0ae55907f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59398023"
---
# <a name="adding-validation-to-the-model-vb"></a>Dodawanie walidacji do modelu (VB)

Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express SP1 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, należy zainstalować wymagania wstępne, klikając poniższe łącze: [Visual Studio 2010 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-validation-to-the-model.md) po ukończeniu tego samouczka.


W tej sekcji dodasz logikę walidacji do `Movie` modelu, a będzie upewnij się, że reguły sprawdzania poprawności są wymuszane ilekroć użytkownik próbuje utworzyć lub edytować film przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Utrzymywanie susz rzeczy

Jednym z podstawowych zasadach projektowania platformy ASP.NET MVC jest PRÓBNEGO ("nie należy powtórzyć samodzielnie"). ASP.NET MVC zachęca można określić funkcji lub zachowanie tylko raz, a następnie go wszędzie, gdzie odzwierciedlone w aplikacji. Zmniejsza ilość kodu, który trzeba było pisać i sprawia, że kod, który napiszesz znacznie ułatwia utrzymanie.

Obsługa weryfikacji platformy ASP.NET MVC i Entity Framework Code First to świetny przykład susz zasady w akcji. Można deklaratywne określenie reguł sprawdzania poprawności w jednym miejscu (w klasie modelu), a następnie te zasady są wymuszane wszędzie, gdzie w aplikacji.

Oto jak możesz korzystać z zalet tej obsługi weryfikacji w aplikacji filmu.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawania reguł sprawdzania poprawności do modelu Movie

Rozpocznie się przez dodanie niektórych logikę walidacji do `Movie` klasy.

Otwórz *Movie.vb* pliku. Dodaj `Imports` instrukcji w górnej części pliku, który odwołuje się do [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

Przestrzeń nazw jest częścią programu .NET Framework. Zapewnia ona wbudowany zestaw atrybutów sprawdzania poprawności, które są stosowane w sposób deklaratywny do dowolnej klasy lub właściwości.

Teraz zaktualizować `Movie` klasy, aby skorzystać z wbudowanych [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), i [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atrybutów sprawdzania poprawności . Użyj poniższego kodu, na przykład gdzie można zastosować atrybuty.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Atrybuty weryfikacji określić zachowanie, które mają zostać wymuszone we właściwościach modelu, które są stosowane względem. `Required` Atrybut wskazuje, że właściwość musi mieć wartość; w tym przykładzie filmu musi mieć wartości `Title`, `ReleaseDate`, `Genre`, i `Price` właściwości, aby był prawidłowy. `Range` Atrybut ogranicza wartości do określonego zakresu. `StringLength` Atrybut pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie długości minimalnej.

Kod najpierw gwarantuje, że reguł sprawdzania poprawności, które określisz w klasie modelu są wymuszane, zanim aplikacja zapisuje zmiany w bazie danych. Na przykład, poniższy kod spowoduje zgłoszenie wyjątku podczas `SaveChanges` metoda jest wywoływana, ponieważ niektóre wymagane `Movie` brakuje wartości właściwości, a cena jest równa zero, (która jest poza prawidłowym zakresem).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Posiadanie reguły sprawdzania poprawności, które automatycznie wymuszanych przez program .NET Framework ułatwia zapewnienie aplikacji bardziej niezawodne. Gwarantuje również, że nie pamiętasz do sprawdzania poprawności coś i przypadkowo umożliwiają złe dane do bazy danych.

Oto kompletny kod dla zaktualizowanego *Movie.vb* pliku:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Błąd sprawdzania poprawności UI we wzorcu ASP.NET MVC

Uruchom ponownie aplikację i przejdź do */Movies* adresu URL.

Kliknij przycisk **utworzyć film** łącze, aby dodać nowy film. Wypełnij formularz z niektórych z nieprawidłowymi wartościami, a następnie kliknij przycisk **Utwórz** przycisku.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Zwróć uwagę, jak formularz automatycznie został użyty kolor tła do wyróżnienia pola tekstowe, zawiera nieprawidłowe dane, które ma wysyłanego komunikatu o błędzie weryfikacji odpowiednich obok każdej z nich. Komunikaty o błędach dopasowania ciągi błędów, określić, kiedy adnotację `Movie` klasy. Błędy są wymuszane, zarówno po stronie klienta (przy użyciu języka JavaScript) i po stronie serwera (w przypadku, gdy użytkownik ma Obsługa skryptów JavaScript wyłączona).

Korzyści z rzeczywistych jest, nie należy zmieniać jednego wiersza kodu w `MoviesController` klasy lub *Create.vbhtml* widoku w celu włączenia tej weryfikacji interfejsu użytkownika. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobrana sprawdzania poprawności reguły określone przy użyciu atrybutów na górę `Movie` klasa modelu.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>W jaki sposób weryfikacji odbywa się w tworzenie wyświetlanie i Tworzenie metody akcji

Być może zastanawiasz się, jak sprawdzanie poprawności UI został wygenerowany bez wykonywania żadnych aktualizacji do kodu w kontrolerze lub widoków. Dalej prezentuje co `Create` metody `MovieController` jak wyglądają klasy. Są one w porównaniu z jak utworzone wcześniej w tym samouczku.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

Pierwsza metoda akcji Wyświetla początkowy formularz Utwórz. Drugi obsługuje post formularza. Drugi `Create` wywołania metody `ModelState.IsValid` do sprawdzenia, czy ten film zawiera wszystkie błędy weryfikacji. Wywołanie tej metody ocenia wszelkie atrybuty weryfikacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy sprawdzania poprawności `Create` metoda ładowaniu formularza. Jeśli nie ma żadnych błędów, metoda zapisuje ten nowy film w bazie danych.

Poniżej znajduje się *Create.vbhtml* Wyświetl szablon, którego szkielet we wcześniejszej części tego samouczka. Jest on używany przez metody akcji, zarówno powyżej początkowy formularz wyświetlania i wyświetlić ją ponownie w przypadku wystąpienia błędu.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Zwróć uwagę, jak kod używa `Html.EditorFor` pomocnika służący do wypełniania wyjściowego `<input>` elementu dla każdego `Movie` właściwości. Obok tego pomocnika jest wywołaniem `Html.ValidationMessageFor` metody pomocnika. Te dwie metody pomocnika pracować obiekt modelu, który jest przekazywany przez kontrolera do widoku (w tym przypadku `Movie` obiektu). Poszukaj one automatycznie atrybutów sprawdzania poprawności, określone w modelu i wyświetlanie komunikatów o błędach zgodnie z potrzebami.

Co to jest bardzo NAS cieszy się o to podejście jest, czy kontroler ani Utwórz szablon widoku nie wie, nic o regułach rzeczywista weryfikacja wymuszany ani o zbyt małą określone komunikaty o błędach wyświetlane. Reguł sprawdzania poprawności i ciągi błędów są określane tylko w `Movie` klasy.

Aby zmienić później logikę weryfikacji, możesz to zrobić w dokładnie jednego miejsca. Nie trzeba już martwić się o różnych części aplikacji jest niespójna z jak zasady są wymuszane — całą logikę weryfikacji będą zdefiniowane w jednym miejscu i użyć wszędzie. Zapewnia bardzo czystym kodzie i ułatwia utrzymanie i rozwój. I oznacza, że można będzie można w pełni zapewniane susz zasady.

## <a name="adding-formatting-to-the-movie-model"></a>Dodanie formatowania do modelu Movie

Otwórz *Movie.vb* pliku. [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Przestrzeń nazw zawiera atrybuty formatowania, oprócz wbudowanych zestaw atrybutów weryfikacji. Zastosowania [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu i [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) wartości wyliczenia, Data wydania i pola Cena. Poniższy kod przedstawia `ReleaseDate` i `Price` właściwości z odpowiednią [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Alternatywnie można jawnie ustawić [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) wartości. Poniższy kod pokazuje właściwości daty wydania z ciągiem formatu daty (to znaczy, "d"). Będzie to użyć, aby określić, że nie chcesz czas jako część daty wydania.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

Poniższy kod formaty `Price` właściwości jako walutę.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

Pełne `Movie` klasy znajdują się poniżej.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Uruchom aplikację, a następnie przejdź do `Movies` kontrolera.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

W następnej części serii, utworzymy aplikację i wprowadzić kilka ulepszeń do automatycznie generowanego `Details` i `Delete` metody...

> [!div class="step-by-step"]
> [Poprzednie](adding-a-new-field.md)
> [dalej](improving-the-details-and-delete-methods.md)

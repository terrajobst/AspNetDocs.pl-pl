---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Dodawanie walidacji do modelu (C#) | Microsoft Docs
author: Rick-Anderson
description: Tworzenie kontrolera
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 19d86dc0df931a9d135e46209559892b77626cf6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615452"
---
# <a name="adding-validation-to-the-model-c"></a>Dodawanie walidacji do modelu (C#)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.
> 
> 
> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu. [Pobierz wersję C# programu](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przejdź do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.

W tej sekcji dodasz logikę walidacji do modelu `Movie` i będziesz mieć pewność, że reguły sprawdzania poprawności zostaną wymuszone za każdym razem, gdy użytkownik spróbuje utworzyć lub edytować film przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Przechowywanie SUCHEj zawartości

Jeden z podstawowych założenia projektu ASP.NET MVC jest SUCHy ("Nie powtarzaj się"). ASP.NET MVC zachęca do określania funkcjonalności lub zachowania tylko raz, a następnie znajdować się w dowolnym miejscu w aplikacji. Pozwala to zmniejszyć ilość kodu, który trzeba napisać, i ułatwia przechowywanie kodu.

Obsługa walidacji świadczona przez ASP.NET MVC i Entity Framework Code First to doskonały przykład zasady SUCHa w działaniu. Można deklaratywnie określić reguły walidacji w jednym miejscu (w klasie modelu), a następnie te reguły są wymuszane wszędzie w aplikacji.

Przyjrzyjmy się sposobom korzystania z tej obsługi walidacji w aplikacji filmowej.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji do modelu filmu

Zacznij od dodania logiki walidacji do klasy `Movie`.

Otwórz plik *Movie.cs* . Dodaj instrukcję `using` w górnej części pliku, która odwołuje się do [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Przestrzeń nazw jest częścią .NET Framework. Zapewnia Wbudowany zestaw atrybutów walidacji, które można zastosować deklaratywnie do dowolnej klasy lub właściwości.

Teraz zaktualizuj klasę `Movie`, aby skorzystać z wbudowanych atrybutów [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)i [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) weryfikacji. Użyj poniższego kodu jako przykładu, gdzie zastosować atrybuty.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Atrybuty walidacji określają zachowanie, które chcesz wymusić na właściwościach modelu, do których są stosowane. Atrybut `Required` wskazuje, że właściwość musi mieć wartość; w tym przykładzie film musi mieć wartości właściwości `Title`, `ReleaseDate`, `Genre`i `Price`, aby był prawidłowy. Atrybut `Range` ogranicza wartość do określonego zakresu. Atrybut `StringLength` pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie jej długość minimalną.

Code First zapewnia, że reguły sprawdzania poprawności określone dla klasy modelu są wymuszane przed zapisaniem zmian w bazie danych przez aplikację. Na przykład poniższy kod zgłosi wyjątek w przypadku wywołania metody `SaveChanges`, ponieważ brakuje kilku wymaganych wartości właściwości `Movie` a cena wynosi zero (poza prawidłowym zakresem).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Automatyczne Wymuszanie reguł sprawdzania poprawności przez .NET Framework pomaga zwiększyć niezawodność aplikacji. Gwarantuje to również, że nie można zapomnieć, aby zweryfikować coś i przypadkowo umożliwić niewłaściwe dane w bazie danych.

Oto kompletna lista kodu dla zaktualizowanego pliku *Movie.cs* :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfejs użytkownika błędu walidacji w ASP.NET MVC

Uruchom aplikację jeszcze raz i przejdź do adresu URL */Movies* .

Kliknij link **Utwórz film** , aby dodać nowy film. Wypełnij formularz z nieprawidłowymi wartościami, a następnie kliknij przycisk **Utwórz** .

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Zwróć uwagę, jak formularz automatycznie użył koloru tła w celu wyróżnienia pól tekstowych, które zawierają nieprawidłowe dane i wyemitują odpowiedni komunikat o błędzie walidacji obok każdego z nich. Komunikaty o błędach pasują do ciągów błędów określonych podczas dodawania adnotacji do klasy `Movie`. Błędy są wymuszane po stronie klienta (przy użyciu języka JavaScript) i po stronie serwera (w przypadku, gdy użytkownik ma wyłączony kod JavaScript).

Rzeczywista korzyść polega na tym, że nie trzeba zmieniać jednego wiersza kodu w klasie `MoviesController` lub w widoku *Create. cshtml* , aby włączyć ten interfejs użytkownika weryfikacji. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierają reguły sprawdzania poprawności określone przy użyciu atrybutów klasy modelu `Movie`.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Jak następuje Walidacja w metodzie tworzenia widoku i tworzenia akcji

Możesz zastanawiać się, jak został wygenerowany interfejs użytkownika weryfikacji bez aktualizacji kodu w kontrolerze lub widokach. Następna lista pokazuje, jak wyglądają `Create` metod w klasie `MovieController`. Są one niezmienione w sposób wcześniej utworzony w tym samouczku.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

Pierwsza metoda działania wyświetla początkowy formularz tworzenia. Drugi obsługuje wpis w formularzu. Druga metoda `Create` wywołuje `ModelState.IsValid`, aby sprawdzić, czy film ma błędy walidacji. Wywołanie tej metody szacuje wszystkie atrybuty walidacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy walidacji, Metoda `Create` ponownie wyświetla formularz. Jeśli nie ma żadnych błędów, Metoda zapisuje nowy film w bazie danych.

Poniżej przedstawiono szablon widoku *Create. cshtml* , który został poddany wcześniej w samouczku. Jest on używany przez metody akcji pokazane powyżej obydwu, aby wyświetlić początkowy formularz i ponownie wyświetlić go w przypadku błędu.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Zwróć uwagę, jak kod używa pomocnika `Html.EditorFor` do wyprowadzania elementu `<input>` dla każdej właściwości `Movie`. Obok tego pomocnika jest wywołanie metody pomocnika `Html.ValidationMessageFor`. Te dwie metody pomocnika współpracują z obiektem modelu, który jest przesyłany przez kontroler do widoku (w tym przypadku `Movie` obiektu). Automatycznie wyszukują atrybuty sprawdzania poprawności określone w modelu i wyświetlają komunikaty o błędach zgodnie z potrzebami.

W rzeczywistości to podejście jest takie, że ani kontroler, ani szablon Create View nie wie o faktycznych regułach walidacji lub o określonych komunikatach o błędach. Reguły walidacji i ciągi błędów są określone tylko w klasie `Movie`.

Jeśli chcesz później zmienić logikę walidacji, możesz to zrobić w dokładnie jednym miejscu. Nie trzeba martwić się o różne części aplikacji, które nie są zgodne z zasadami, w których są wymuszane — Cała logika walidacji zostanie zdefiniowana w jednym miejscu i użyta wszędzie. Dzięki temu kod jest bardzo czysty i ułatwia utrzymanie i rozwój. Oznacza to, że będziesz w pełni przestrzegać SUCHEj zasady.

## <a name="adding-formatting-to-the-movie-model"></a>Dodawanie formatowania do modelu filmu

Otwórz plik *Movie.cs* . Przestrzeń nazw [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) zawiera atrybuty formatowania oprócz wbudowanego zestawu atrybutów walidacji. Zastosujesz atrybut [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) i [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) wartość wyliczenia do daty wydania i do pól cen. Poniższy kod pokazuje `ReleaseDate` i `Price` właściwości z odpowiednim atrybutem [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) .

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Alternatywnie można jawnie ustawić wartość [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . Poniższy kod przedstawia Właściwość Data wydania z ciągiem formatu daty (tj. "d"). Możesz użyć tej wartości, aby określić, że nie chcesz przekroczyć czasu w ramach daty wydania.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Poniższy kod formatuje Właściwość `Price` jako walutę.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Poniżej przedstawiono kompletną klasę `Movie`.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Uruchom aplikację i przejdź do kontrolera `Movies`.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

W następnej części serii sprawdzimy aplikację i wprowadzimy pewne usprawnienia `Details` i `Delete` metod.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-new-field.md)
> [dalej](improving-the-details-and-delete-methods.md)

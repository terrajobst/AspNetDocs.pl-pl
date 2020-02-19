---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: Dodawanie walidacji do modelu | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: c9f6699c5d3500d4c1fcade9252aeb9dd92983da
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455961"
---
# <a name="adding-validation-to-the-model"></a>Dodawanie walidacji do modelu

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.

W tej sekcji dodasz logikę walidacji do modelu `Movie` i będziesz mieć pewność, że reguły sprawdzania poprawności zostaną wymuszone za każdym razem, gdy użytkownik spróbuje utworzyć lub edytować film przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Przechowywanie SUCHEj zawartości

Jeden z podstawowych założenia projektu ASP.NET MVC jest SUCHy (&quot;Nie powtarzaj się&quot;). ASP.NET MVC zachęca do określania funkcjonalności lub zachowania tylko raz, a następnie znajdować się w dowolnym miejscu w aplikacji. Pozwala to zmniejszyć ilość kodu, który trzeba napisać, i sprawia, że pisanie kodu jest mniej podatne na błędy i łatwiejsze w obsłudze.

Obsługa walidacji świadczona przez ASP.NET MVC i Entity Framework Code First to doskonały przykład zasady SUCHa w działaniu. Można deklaratywnie określić reguły walidacji w jednym miejscu (w klasie modelu), a reguły są wymuszane wszędzie w aplikacji.

Przyjrzyjmy się sposobom korzystania z tej obsługi walidacji w aplikacji filmowej.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji do modelu filmu

Zacznij od dodania logiki walidacji do klasy `Movie`.

Otwórz plik *Movie.cs* . Dodaj instrukcję `using` w górnej części pliku, która odwołuje się do [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) przestrzeni nazw:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Należy zauważyć, że przestrzeń nazw nie zawiera `System.Web`. DataAnnotations zawiera wbudowany zestaw atrybutów sprawdzania poprawności, które można zastosować deklaratywnie do dowolnej klasy lub właściwości.

Teraz zaktualizuj klasę `Movie`, aby skorzystać z wbudowanych atrybutów [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)i [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) weryfikacji. Użyj poniższego kodu jako przykładu, gdzie zastosować atrybuty.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Uruchom aplikację i ponownie Uzyskaj następujący błąd czasu wykonania:

***Model z kopią zapasową kontekstu "MovieDBContext" został zmieniony od czasu utworzenia bazy danych. Aby zaktualizować bazę danych ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)), należy rozważyć użycie migracje Code First.***

W celu zaktualizowania schematu będziemy używać migracji. Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** i wprowadź następujące polecenia:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę pochodną `DbMigration` o określonej nazwie (*AddDataAnnotationsMig*), a w metodzie `Up` można zobaczyć kod, który aktualizuje ograniczenia schematu. Pola `Title` i `Genre` nie dopuszczają wartości null (oznacza to, że należy wprowadzić wartość), a pole `Rating` ma maksymalną długość równą 5.

Atrybuty walidacji określają zachowanie, które chcesz wymusić na właściwościach modelu, do których są stosowane. Atrybut `Required` wskazuje, że właściwość musi mieć wartość; w tym przykładzie film musi mieć wartości właściwości `Title`, `ReleaseDate`, `Genre`i `Price`, aby był prawidłowy. Atrybut `Range` ogranicza wartość do określonego zakresu. Atrybut `StringLength` pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie jej długość minimalną. Typy wewnętrzne (takie jak `decimal, int, float, DateTime`) są domyślnie wymagane i nie potrzebują atrybutu `Required`.

Code First zapewnia, że reguły sprawdzania poprawności określone dla klasy modelu są wymuszane przed zapisaniem zmian w bazie danych przez aplikację. Na przykład poniższy kod zgłosi wyjątek w przypadku wywołania metody `SaveChanges`, ponieważ brakuje kilku wymaganych wartości właściwości `Movie` a cena wynosi zero (poza prawidłowym zakresem).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Automatyczne Wymuszanie reguł sprawdzania poprawności przez .NET Framework pomaga zwiększyć niezawodność aplikacji. Gwarantuje to również, że nie można zapomnieć, aby zweryfikować coś i przypadkowo umożliwić niewłaściwe dane w bazie danych.

Oto kompletna lista kodu dla zaktualizowanego pliku *Movie.cs* :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfejs użytkownika błędu walidacji w ASP.NET MVC

Uruchom aplikację jeszcze raz i przejdź do adresu URL */Movies* .

Kliknij link **Utwórz nowy** , aby dodać nowy film. Wypełnij formularz z nieprawidłowymi wartościami, a następnie kliknij przycisk **Utwórz** .

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> Aby zapewnić obsługę walidacji jQuery dla ustawień regionalnych innych niż angielskie, które używają przecinka (&quot;,&quot;) dla punktu dziesiętnego, należy dołączyć plik *globalizacjs. js* i Twoje określone *kultury/globalizacja* (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`. Poniższy kod przedstawia modyfikacje pliku Views\Movies\Edit.cshtml do pracy &quot;z&quot; kulturą fr-FR:

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Zwróć uwagę, jak formularz automatycznie używa koloru czerwonego obramowania, aby zaznaczyć pola tekstowe, które zawierają nieprawidłowe dane i wyemitują odpowiedni komunikat o błędzie walidacji obok każdego z nich. Błędy są wymuszane po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma wyłączony kod JavaScript).

Rzeczywista korzyść polega na tym, że nie trzeba zmieniać jednego wiersza kodu w klasie `MoviesController` lub w widoku *Create. cshtml* , aby włączyć ten interfejs użytkownika weryfikacji. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierają reguły sprawdzania poprawności określone przy użyciu atrybutów walidacji we właściwościach klasy modelu `Movie`.

Być może zauważono, że właściwości `Title` i `Genre`, wymagany atrybut nie jest wymuszany do momentu przesłania formularza (należy kliknąć przycisk **Utwórz** ) lub wpisać tekst w polu wejściowym i usunąć go. Dla pola, które jest początkowo puste (na przykład pól w widoku tworzenia) i który ma tylko wymagany atrybut i nie ma innych atrybutów sprawdzania poprawności, można wykonać następujące czynności w celu wyzwolenia weryfikacji:

1. Do pola.
2. Wprowadź tekst.
3. Karta.
4. Wróć do pola.
5. Usuń tekst.
6. Karta.

Powyższa sekwencja spowoduje wyzwolenie wymaganej walidacji bez naciśnięcia przycisku Prześlij. Po prostu naciśnięcie przycisku Prześlij bez wprowadzenia jakichkolwiek pól spowoduje wyzwolenie walidacji po stronie klienta. Dane formularza nie są wysyłane do serwera, dopóki nie zostaną wykryte błędy weryfikacji po stronie klienta. Można to przetestować, umieszczając punkt przerwania w metodzie post protokołu HTTP lub korzystając z [Narzędzia programu Fiddler](http://fiddler2.com/fiddler2/) lub [narzędzia deweloperskiego](https://msdn.microsoft.com/ie/aa740478)programu IE 9 F12.

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Jak następuje Walidacja w metodzie tworzenia widoku i tworzenia akcji

Możesz zastanawiać się, jak został wygenerowany interfejs użytkownika weryfikacji bez aktualizacji kodu w kontrolerze lub widokach. Następna lista pokazuje, jak wyglądają `Create` metod w klasie `MovieController`. Są one niezmienione w sposób wcześniej utworzony w tym samouczku.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

W pierwszej metodzie akcji `Create` (HTTP GET) jest wyświetlany początkowy formularz tworzenia. Druga wersja (`[HttpPost]`) obsługuje wpis w formularzu. Druga metoda `Create` (wersja `HttpPost`) `ModelState.IsValid`, aby sprawdzić, czy film ma błędy walidacji. Wywołanie tej metody szacuje wszystkie atrybuty walidacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy walidacji, Metoda `Create` ponowne wyświetli formularz. Jeśli nie ma żadnych błędów, Metoda zapisuje nowy film w bazie danych. W naszym przykładzie filmów, **formularz nie jest ogłaszany na serwerze, gdy na stronie klienta wykryto błędy walidacji. druga** **Metoda `Create`nie zostanie wywołana**. Jeśli wyłączysz język JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączone i wywołanie metody HTTP POST `Create` wywoła `ModelState.IsValid`, aby sprawdzić, czy film ma błędy walidacji.

Można ustawić punkt przerwania w metodzie `HttpPost Create` i sprawdzić, czy metoda nie jest nigdy wywoływana, podczas weryfikacji po stronie klienta nie będą przesyłane dane formularza po wykryciu błędów walidacji. Jeśli wyłączysz JavaScript w przeglądarce, a następnie prześlesz formularz z błędami, zostanie osiągnięty punkt przerwania. Nadal będziesz mieć pełną weryfikację bez języka JavaScript. Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w programie Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w przeglądarce FireFox.

![](adding-validation-to-the-model/_static/image5.png)

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript za pomocą przeglądarki Chrome.

![](adding-validation-to-the-model/_static/image6.png)

Poniżej przedstawiono szablon widoku *Create. cshtml* , który został poddany wcześniej w samouczku. Jest on używany przez metody akcji pokazane powyżej obydwu, aby wyświetlić początkowy formularz i ponownie wyświetlić go w przypadku błędu.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Zwróć uwagę, jak kod używa pomocnika `Html.EditorFor` do wyprowadzania elementu `<input>` dla każdej właściwości `Movie`. Obok tego pomocnika jest wywołanie metody pomocnika `Html.ValidationMessageFor`. Te dwie metody pomocnika współpracują z obiektem modelu, który jest przesyłany przez kontroler do widoku (w tym przypadku `Movie` obiektu). Automatycznie wyszukują atrybuty sprawdzania poprawności określone w modelu i wyświetlają komunikaty o błędach zgodnie z potrzebami.

W rzeczywistości to podejście jest takie, że ani kontroler, ani szablon Create View nie wie o faktycznych regułach walidacji lub o określonych komunikatach o błędach. Reguły walidacji i ciągi błędów są określone tylko w klasie `Movie`. Te same reguły sprawdzania poprawności są automatycznie stosowane do widoku edycji i innych szablonów widoków, które można utworzyć, edytując model.

Aby zmienić logikę walidacji później, można to zrobić w dokładnie jednym miejscu przez dodanie atrybutów walidacji do modelu (w tym przykładzie Klasa `movie`). Nie trzeba martwić się o różne części aplikacji, które nie są zgodne z zasadami, w których są wymuszane — Cała logika walidacji zostanie zdefiniowana w jednym miejscu i użyta wszędzie. Dzięki temu kod jest bardzo czysty i ułatwia utrzymanie i rozwój. Oznacza to, że będziesz w pełni przestrzegać SUCHEj zasady.

## <a name="adding-formatting-to-the-movie-model"></a>Dodawanie formatowania do modelu filmu

Otwórz plik *Movie.cs* i zapoznaj się z klasą `Movie`. Przestrzeń nazw [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) zawiera atrybuty formatowania oprócz wbudowanego zestawu atrybutów walidacji. W dacie wydania i w polach cen została już zastosowana wartość wyliczenia [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) . Poniższy kod pokazuje `ReleaseDate` i `Price` właściwości z odpowiednim atrybutem [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) .

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Atrybuty [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nie są atrybutami walidacji, są używane do poinformowania aparatu widoku o sposobie renderowania kodu HTML. W powyższym przykładzie atrybut `DataType.Date` wyświetla daty filmu tylko jako daty, bez czasu. Na przykład następujące atrybuty [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) nie weryfikują formatu danych:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Wymienione powyżej atrybuty zawierają tylko wskazówki dla aparatu widoku do formatowania danych (i dostarczania atrybutów, takich jak &lt;&gt; dla adresu URL i &lt;a href =&quot;mailto:EmailAddress. com&quot;&gt; do wiadomości e-mail. Aby sprawdzić format danych, można użyć atrybutu [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) .

Alternatywne podejście do używania atrybutów `DataType`, można jawnie ustawić wartość [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) . Poniższy kod przedstawia Właściwość Data wydania z ciągiem formatu daty (mianowicie &quot;d&quot;). Możesz użyć tej wartości, aby określić, że nie chcesz przekroczyć czasu w ramach daty wydania.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

Poniżej przedstawiono kompletną klasę `Movie`.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Uruchom aplikację i przejdź do kontrolera `Movies`. Data i cena wydania są sformatowane w dobrze. Na poniższej ilustracji przedstawiono datę i cenę wydania przy użyciu &quot;fr-FR&quot; jako kulturę.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

Na poniższej ilustracji przedstawiono te same dane, które są wyświetlane w domyślnej kulturze (w języku angielskim).

![](adding-validation-to-the-model/_static/image8.png)

W następnej części serii sprawdzimy aplikację i wprowadzimy pewne usprawnienia `Details` i `Delete` metod.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-new-field-to-the-movie-model-and-table.md)
> [dalej](examining-the-details-and-delete-methods.md)

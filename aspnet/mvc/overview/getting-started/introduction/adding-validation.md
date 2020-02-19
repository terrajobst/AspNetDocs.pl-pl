---
uid: mvc/overview/getting-started/introduction/adding-validation
title: Dodawanie walidacji | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: f508d9e38dab5cc4cc44cc5aaa4eae87cf273bd5
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456053"
---
# <a name="adding-validation"></a>Dodawanie walidacji

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

W tej sekcji dodasz logikę walidacji do modelu `Movie` i będziesz mieć pewność, że reguły sprawdzania poprawności zostaną wymuszone za każdym razem, gdy użytkownik spróbuje utworzyć lub edytować film przy użyciu aplikacji.

## <a name="keeping-things-dry"></a>Przechowywanie SUCHEj zawartości

Jeden z podstawowych założenia projektu ASP.NET MVC jest [suchy](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;nie powtarzaj się&quot;). ASP.NET MVC zachęca do określania funkcjonalności lub zachowania tylko raz, a następnie znajdować się w dowolnym miejscu w aplikacji. Pozwala to zmniejszyć ilość kodu, który trzeba napisać, i sprawia, że pisanie kodu jest mniej podatne na błędy i łatwiejsze w obsłudze.

Obsługa walidacji świadczona przez ASP.NET MVC i Entity Framework Code First to doskonały przykład zasady SUCHa w działaniu. Można deklaratywnie określić reguły walidacji w jednym miejscu (w klasie modelu), a reguły są wymuszane wszędzie w aplikacji.

Przyjrzyjmy się sposobom korzystania z tej obsługi walidacji w aplikacji filmowej.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji do modelu filmu

Zacznij od dodania logiki walidacji do klasy `Movie`.

Otwórz plik *Movie.cs* . Zauważ, że przestrzeń nazw [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) nie zawiera `System.Web`. DataAnnotations zawiera wbudowany zestaw atrybutów sprawdzania poprawności, które można zastosować deklaratywnie do dowolnej klasy lub właściwości. (Zawiera również atrybuty formatowania, takie jak [Typ danych](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , które ułatwiają formatowanie i nie zapewniają weryfikacji.)

Teraz zaktualizuj klasę `Movie`, aby skorzystać z wbudowanych atrybutów [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)i [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) . Zastąp klasę `Movie` następującymi:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

Atrybut [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) ustawia maksymalną długość ciągu i ustawia to ograniczenie dla bazy danych, w związku z czym schemat bazy danych ulegnie zmianie. Kliknij prawym przyciskiem myszy tabelę **filmy** w **Eksploratorze serwera** i kliknij polecenie **Otwórz definicję tabeli**:

![](adding-validation/_static/image1.png)

Na powyższym obrazie wszystkie pola ciągów są ustawione na [nvarchar (max)](https://technet.microsoft.com/library/ms186939.aspx). W celu zaktualizowania schematu będziemy używać migracji. Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** i wprowadź następujące polecenia:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę pochodną `DbMigration` o określonej nazwie (`DataAnnotations`), a w metodzie `Up` można zobaczyć kod, który aktualizuje ograniczenia schematu:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

Pole `Genre` nie ma już wartości null (oznacza to, że należy wprowadzić wartość). Pole `Rating` ma maksymalną długość 5, a `Title` ma maksymalną długość 60. Minimalna długość 3 w `Title` i zakres w `Price` nie utworzył zmian schematu.

Przejrzyj schemat filmu:

![](adding-validation/_static/image2.png)

Pola ciągów pokazują nowe limity długości, a `Genre` nie są już sprawdzane jako wartości null.

Atrybuty walidacji określają zachowanie, które chcesz wymusić na właściwościach modelu, do których są stosowane. Atrybuty `Required` i `MinimumLength` wskazują, że właściwość musi mieć wartość; jednak nic nie zapobiega wprowadzaniu przez użytkownika białych znaków w celu zaspokojenia tej walidacji. Atrybut [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) służy do ograniczania, jakie znaki mogą być wprowadzane. W powyższym kodzie, `Genre` i `Rating` muszą używać tylko liter (odstępy, cyfry i znaki specjalne są niedozwolone). Atrybut [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) ogranicza wartość do określonego zakresu. Atrybut `StringLength` pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie jej długość minimalną. Typy wartości (takie jak `decimal, int, float, DateTime`) są z natury wymagane i nie potrzebują atrybutu `Required`.

Code First zapewnia, że reguły sprawdzania poprawności określone dla klasy modelu są wymuszane przed zapisaniem zmian w bazie danych przez aplikację. Na przykład poniższy kod zgłosi wyjątek [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) po wywołaniu metody `SaveChanges`, ponieważ brakuje kilku wymaganych wartości właściwości `Movie`:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

Powyższy kod zgłasza następujący wyjątek:

*Walidacja nie powiodła się dla co najmniej jednej jednostki. Aby uzyskać więcej informacji, zobacz Właściwość "EntityValidationErrors".*

Automatyczne Wymuszanie reguł sprawdzania poprawności przez .NET Framework pomaga zwiększyć niezawodność aplikacji. Gwarantuje to również, że nie można zapomnieć, aby zweryfikować coś i przypadkowo umożliwić niewłaściwe dane w bazie danych.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Interfejs użytkownika błędu walidacji w ASP.NET MVC

Uruchom aplikację i przejdź do adresu URL */Movies* .

Kliknij link **Utwórz nowy** , aby dodać nowy film. Wypełnij formularz nieprawidłowymi wartościami. Gdy tylko Walidacja po stronie klienta jQuery wykryje błąd, zostanie wyświetlony komunikat o błędzie.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> Aby zapewnić obsługę walidacji jQuery dla ustawień regionalnych innych niż angielskie, które używają przecinka (",") dla punktu dziesiętnego, należy dołączyć globalizację NuGet, jak opisano wcześniej w tym samouczku.

Zwróć uwagę, jak formularz automatycznie używa koloru czerwonego obramowania, aby zaznaczyć pola tekstowe, które zawierają nieprawidłowe dane i wyemitują odpowiedni komunikat o błędzie walidacji obok każdego z nich. Błędy są wymuszane po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma wyłączony kod JavaScript).

Rzeczywista korzyść polega na tym, że nie trzeba zmieniać jednego wiersza kodu w klasie `MoviesController` lub w widoku *Create. cshtml* , aby włączyć ten interfejs użytkownika weryfikacji. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierają reguły sprawdzania poprawności określone przy użyciu atrybutów walidacji we właściwościach klasy modelu `Movie`. Sprawdzanie poprawności testu przy użyciu metody akcji `Edit` i tej samej walidacji jest stosowane.

Dane formularza nie są wysyłane do serwera, dopóki nie zostaną wykryte błędy weryfikacji po stronie klienta. Można to sprawdzić, umieszczając punkt przerwania w metodzie post protokołu HTTP przy użyciu [Narzędzia programu Fiddler](http://fiddler2.com/fiddler2/)lub [narzędzi programistycznych](https://msdn.microsoft.com/ie/aa740478)programu IE F12.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Jak następuje Walidacja w metodzie tworzenia widoku i tworzenia akcji

Możesz zastanawiać się, jak został wygenerowany interfejs użytkownika weryfikacji bez aktualizacji kodu w kontrolerze lub widokach. Następna lista pokazuje, jak wyglądają `Create` metod w klasie `MovieController`. Są one niezmienione w sposób wcześniej utworzony w tym samouczku.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

W pierwszej metodzie akcji `Create` (HTTP GET) jest wyświetlany początkowy formularz tworzenia. Druga wersja (`[HttpPost]`) obsługuje wpis w formularzu. Druga metoda `Create` (wersja `HttpPost`) sprawdza `ModelState.IsValid`, aby sprawdzić, czy film ma błędy walidacji. Pobieranie tej właściwości oblicza wszystkie atrybuty walidacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy walidacji, Metoda `Create` ponownie wyświetla formularz. Jeśli nie ma żadnych błędów, Metoda zapisuje nowy film w bazie danych. W naszym przykładzie filmu **nie jest on ogłaszany na serwerze, gdy na stronie klienta wykryto błędy walidacji. druga** **Metoda `Create` nie zostanie wywołana**. Jeśli wyłączysz język JavaScript w przeglądarce, sprawdzanie poprawności klienta jest wyłączone, a metoda HTTP POST `Create` pobiera `ModelState.IsValid`, aby sprawdzić, czy film ma błędy walidacji.

Można ustawić punkt przerwania w metodzie `HttpPost Create` i sprawdzić, czy metoda nie jest nigdy wywoływana, podczas weryfikacji po stronie klienta nie będą przesyłane dane formularza po wykryciu błędów walidacji. Jeśli wyłączysz JavaScript w przeglądarce, a następnie prześlesz formularz z błędami, zostanie osiągnięty punkt przerwania. Nadal będziesz mieć pełną weryfikację bez języka JavaScript. Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w programie Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w przeglądarce FireFox.

![](adding-validation/_static/image7.png)

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w przeglądarce Chrome.

![](adding-validation/_static/image8.png)

Poniżej przedstawiono szablon widoku *Create. cshtml* , który został poddany wcześniej w samouczku. Jest on używany przez metody akcji pokazane powyżej obydwu, aby wyświetlić początkowy formularz i ponownie wyświetlić go w przypadku błędu.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Zwróć uwagę, jak kod używa pomocnika `Html.EditorFor` do wyprowadzania elementu `<input>` dla każdej właściwości `Movie`. Obok tego pomocnika jest wywołanie metody pomocnika `Html.ValidationMessageFor`. Te dwie metody pomocnika współpracują z obiektem modelu, który jest przesyłany przez kontroler do widoku (w tym przypadku `Movie` obiektu). Automatycznie wyszukują atrybuty sprawdzania poprawności określone w modelu i wyświetlają komunikaty o błędach zgodnie z potrzebami.

W rzeczywistości to podejście jest takie, że żaden kontroler ani szablon widoku `Create` nie wie o faktycznych regułach walidacji lub o określonych komunikatach o błędach. Reguły walidacji i ciągi błędów są określone tylko w klasie `Movie`. Te same reguły sprawdzania poprawności są automatycznie stosowane do widoku `Edit` i wszystkich innych szablonów widoków, które można utworzyć, edytując model.

Aby zmienić logikę walidacji później, można to zrobić w dokładnie jednym miejscu przez dodanie atrybutów walidacji do modelu (w tym przykładzie Klasa `movie`). Nie trzeba martwić się o różne części aplikacji, które nie są zgodne z zasadami, w których są wymuszane — Cała logika walidacji zostanie zdefiniowana w jednym miejscu i użyta wszędzie. Dzięki temu kod jest bardzo czysty i ułatwia utrzymanie i rozwój. Oznacza to, że będziesz w pełni przestrzegać *suchej* zasady.

## <a name="using-datatype-attributes"></a>Używanie atrybutów DataType

Otwórz plik *Movie.cs* i zapoznaj się z klasą `Movie`. Przestrzeń nazw [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) zawiera atrybuty formatowania oprócz wbudowanego zestawu atrybutów walidacji. W dacie wydania i w polach cen została już zastosowana wartość wyliczenia [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) . Poniższy kod pokazuje `ReleaseDate` i `Price` właściwości z odpowiednim atrybutem [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) .

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

Atrybuty [typu DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) udostępniają wskazówki dla aparatu widoku, aby sformatować dane (i podać atrybuty, takie jak `<a>` dla adresu URL i `<a href="mailto:EmailAddress.com">` dla wiadomości e-mail. Aby sprawdzić format danych, można użyć atrybutu [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) . Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych, ***nie*** są atrybutami walidacji. W tym przypadku chcemy tylko śledzić datę, a nie datę i godzinę. [Wyliczenie DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera wiele typów danych, takich jak *Data, godzina, numer telefonu, waluta, EmailAddress* i inne. Atrybut `DataType` może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na przykład łącze `mailto:` można utworzyć dla elementu [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), a selektor daty można dostarczyć dla elementu [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) w przeglądarkach, które obsługują [HTML5](http://html5.org/). Atrybuty [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emitują [dane](http://ejohn.org/blog/html-5-data-attributes/) HTML 5 (wymawiane *kreski danych*), które mogą zrozumieć przeglądarki HTML 5. Atrybuty [typu danych](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) nie zapewniają żadnej weryfikacji.

`DataType.Date` nie określa formatu wyświetlanej daty. Domyślnie pole dane jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)serwera.

Atrybut `DisplayFormat` jest używany do jawnego określania formatu daty:

[!code-csharp[Main](adding-validation/samples/sample8.cs)]

Ustawienie `ApplyFormatInEditMode` określa, że należy również zastosować określone formatowanie, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Możesz nie chcieć, aby dla niektórych pól — na przykład w przypadku wartości walutowych, można nie chcieć, aby symbol waluty w polu tekstowym do edycji.)

Możesz użyć atrybutu [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) przez samego siebie, ale zazwyczaj dobrym pomysłem jest użycie atrybutu [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) również. Atrybut `DataType` przekazuje *semantykę* danych w przeciwieństwie do sposobu renderowania na ekranie i zapewnia następujące korzyści, których nie można uzyskać za pomocą `DisplayFormat`:

- Przeglądarka może włączać funkcje HTML5 (na przykład w celu wyświetlania kontrolki kalendarza, symbolu waluty właściwej dla ustawień regionalnych, linków e-mail itp.).
- Domyślnie przeglądarka będzie renderować dane przy użyciu poprawnego formatu na podstawie [ustawień regionalnych](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) umożliwia wybranie odpowiedniego szablonu pola w celu renderowania danych ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) , jeśli jest używany przez siebie za pomocą szablonu ciągu). Aby uzyskać więcej informacji, zobacz Wilson [ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (W przypadku pisania dla MVC 2, ten artykuł nadal dotyczy bieżącej wersji ASP.NET MVC).

Jeśli używasz atrybutu `DataType` z polem Date, musisz określić atrybut `DisplayFormat` również, aby upewnić się, że pole jest renderowane prawidłowo w przeglądarkach Chrome. Aby uzyskać więcej informacji, zobacz [ten wątek StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> Walidacja jQuery nie działa z atrybutem [Range](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) i [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Na przykład poniższy kod zawsze będzie wyświetlał błąd walidacji po stronie klienta, nawet wtedy, gdy data jest w określonym zakresie:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Należy wyłączyć weryfikację daty platformy jQuery, aby użyć atrybutu [zakresu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) z [datą i godziną](https://msdn.microsoft.com/library/system.datetime.aspx). Zazwyczaj nie jest dobrym sposobem kompilowania dat stałych w modelach, dlatego nie zaleca się używania atrybutu [zakresu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) i [daty/godziny](https://msdn.microsoft.com/library/system.datetime.aspx) .

Poniższy kod ilustruje łączenie atrybutów w jednym wierszu:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=4,6,10,12)]

W następnej części serii sprawdzimy aplikację i wprowadzimy pewne usprawnienia `Details` i `Delete` metod.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-new-field.md)
> [dalej](examining-the-details-and-delete-methods.md)

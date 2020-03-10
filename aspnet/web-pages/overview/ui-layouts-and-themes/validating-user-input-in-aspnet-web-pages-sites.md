---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Sprawdzanie poprawności danych wejściowych użytkownika w witrynach ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule omówiono sposób sprawdzania poprawności informacji uzyskanych od użytkowników &mdash; to, aby upewnić się, że użytkownicy wprowadzają prawidłowe informacje w formularzach HTML w formie jako...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563505"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Sprawdzanie poprawności danych wejściowych użytkownika w witrynach ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule omówiono sposób sprawdzania poprawności informacji uzyskanych od użytkowników &mdash; to, aby upewnić się, że użytkownicy wprowadzają prawidłowe informacje w formularzach HTML w witrynie ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Sprawdzanie, czy dane wejściowe użytkownika odpowiadają zdefiniowanym kryteriom walidacji.
> - Jak ustalić, czy wszystkie testy weryfikacyjne zostały zakończone pomyślnie.
> - Jak wyświetlić błędy walidacji (i sposób formatowania).
> - Sprawdzanie poprawności danych, które nie pochodzą bezpośrednio od użytkowników.
> 
> Są to koncepcje programowania ASP.NET wprowadzone w artykule:
> 
> - Pomocnik `Validation`.
> - Metody `Html.ValidationSummary` i `Html.ValidationMessage`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

Ten artykuł zawiera następujące sekcje:

- [Przegląd weryfikacji danych wejściowych użytkownika](#Overview_of_User_Input_Validation)
- [Sprawdzanie poprawności danych wejściowych użytkownika](#Validating_User_Input)
- [Dodawanie weryfikacji po stronie klienta](#Adding_Client-Side_Validation)
- [Błędy walidacji formatowania](#Formatting_Validation_Errors)
- [Sprawdzanie poprawności danych, które nie pochodzą bezpośrednio od użytkowników](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Przegląd weryfikacji danych wejściowych użytkownika

Jeśli podasz użytkownikowi monit o wprowadzenie informacji na stronie — na przykład w formularzu — ważne jest, aby upewnić się, że wprowadzane wartości są prawidłowe. Na przykład nie chcesz przetworzyć formularza, który nie zawiera krytycznych informacji.

Gdy użytkownicy wprowadzają wartości do formularza HTML, wprowadzane przez nie wartości są ciągami. W wielu przypadkach potrzebne wartości to inne typy danych, takie jak liczby całkowite lub daty. W związku z tym należy również upewnić się, że wartości wprowadzane przez użytkowników mogą być prawidłowo konwertowane na odpowiednie typy danych.

Mogą być również określone ograniczenia dotyczące wartości. Nawet jeśli użytkownicy poprawnie wprowadzają liczbę całkowitą, na przykład może być konieczne upewnienie się, że wartość znajduje się w określonym zakresie.

![Błędy walidacji korzystające z klas stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Ważne** Sprawdzanie poprawności danych wejściowych użytkownika jest również ważne dla bezpieczeństwa. Po ograniczeniu wartości, które użytkownicy mogą wprowadzać w formularzach, zmniejszasz prawdopodobieństwo, że ktoś może wprowadzić wartość, która może naruszyć bezpieczeństwo witryny.

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Sprawdzanie poprawności danych wejściowych użytkownika

W witrynie ASP.NET Web Pages 2 można używać pomocnika `Validator` do testowania danych wejściowych użytkownika. Podstawowym podejściem jest wykonanie następujących czynności:

1. Ustal, które elementy wejściowe (pola) chcesz zweryfikować.

    Zwykle sprawdzane są wartości w `<input>` elementów w formularzu. Jednak dobrym sposobem jest zweryfikowanie wszystkich danych wejściowych, nawet danych wejściowych pochodzących z ograniczonego elementu, takich jak lista `<select>`. Pozwala to upewnić się, że użytkownicy nie pomijają formantów na stronie i przesyłają formularz.
2. W kodzie strony Dodaj indywidualne sprawdzanie poprawności dla każdego elementu wejściowego przy użyciu metod pomocnika `Validation`.

    Aby sprawdzić wymagane pola, użyj `Validation.RequireField(field, [error message])` (dla pojedynczego pola) lub `Validation.RequireFields(field1, field2, ...))` (dla listy pól). W przypadku innych typów walidacji należy użyć `Validation.Add(field, ValidationType)`. Aby uzyskać `ValidationType`, można użyć następujących opcji:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Gdy strona zostanie przesłana, sprawdź, czy sprawdzanie poprawności zakończyło się pomyślnie, sprawdzając `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Jeśli wystąpią jakieś błędy sprawdzania poprawności, można pominąć normalne przetwarzanie strony. Na przykład, jeśli celem strony jest zaktualizowanie bazy danych, nie należy tego robić, dopóki nie zostaną naprawione wszystkie błędy walidacji.
4. Jeśli występują błędy walidacji, Wyświetl komunikaty o błędach w znacznikach strony przy użyciu `Html.ValidationSummary` lub `Html.ValidationMessage`lub obu tych elementów.

Poniższy przykład przedstawia stronę, która ilustruje te kroki.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Aby zobaczyć, jak działa Walidacja, uruchom tę stronę i świadomie wypełniania błędów. Na przykład poniżej przedstawiono, jak wygląda strona, jeśli zapomnisz wprowadzić nazwę kursu, jeśli wprowadzisz i wprowadzisz nieprawidłową datę:

![Błędy walidacji na renderowanej stronie](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Dodawanie weryfikacji po stronie klienta

Domyślnie dane wejściowe użytkownika są sprawdzane po przesłaniu strony przez użytkowników — to znaczy, że sprawdzanie poprawności jest wykonywane w kodzie serwera. Wadą tego podejścia jest to, że użytkownicy nie wiedzą, że wystąpił błąd do momentu przesłania strony. Jeśli formularz jest długi lub złożony, raportowanie błędów tylko po przesłaniu strony może być niewygodny dla użytkownika.

Możesz dodać obsługę, aby przeprowadzić walidację w skrypcie klienta. W takim przypadku sprawdzanie poprawności jest wykonywane, gdy użytkownicy pracują w przeglądarce. Załóżmy na przykład, że określisz, że wartość powinna być liczbą całkowitą. Jeśli użytkownik wprowadzi wartość niebędącą liczbą całkowitą, zostanie zgłoszony błąd zaraz po opuszczeniu pola entry przez użytkownika. Użytkownicy uzyskują natychmiastowe Opinie, które są dla nich wygodne. Walidacja oparta na kliencie może również zmniejszyć liczbę przypadków, w których użytkownik musi przesłać formularz, aby skorygować wiele błędów.

> [!NOTE]
> Nawet w przypadku korzystania z walidacji po stronie klienta sprawdzanie poprawności jest zawsze wykonywane również w kodzie serwera. Wykonanie walidacji w kodzie serwera jest środkiem bezpieczeństwa, w przypadku gdy użytkownicy omijają weryfikację opartą na kliencie.

1. Zarejestruj następujące biblioteki JavaScript na stronie:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Dwie biblioteki są ładowane z usługi Content Delivery Network (CDN), dzięki czemu nie trzeba mieć ich na komputerze lub serwerze. Wymagana jest jednak lokalna kopia *platformy jQuery. Sprawdź poprawność*. Jeśli nie pracujesz jeszcze z szablonem WebMatrix (na przykład **lokacją startową** ) zawierającym bibliotekę, Utwórz witrynę strony sieci Web opartą na **witrynie Starter**. Następnie skopiuj plik *js* do bieżącej lokacji.
2. W znaczniku dla każdego elementu, który jest weryfikowany, Dodaj wywołanie do `Validation.For(field)`. Ta metoda emituje atrybuty, które są używane przez weryfikację po stronie klienta. (Zamiast emitowania rzeczywistego kodu JavaScript metoda emituje atrybuty, takie jak `data-val-...`. Te atrybuty obsługują niezauważalną weryfikację klienta, która korzysta z technologii jQuery do wykonania pracy.

Na poniższej stronie pokazano, jak dodać funkcje sprawdzania poprawności klienta do pokazanego wcześniej przykładu.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Nie wszystkie sprawdzenia poprawności są uruchamiane na kliencie. W szczególności Walidacja typu danych (liczba całkowita, Data i tak dalej) nie jest uruchamiana na kliencie. Następujące sprawdzenia działają zarówno na kliencie, jak i na serwerze:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

W tym przykładzie test dla prawidłowej daty nie będzie działał w kodzie klienta. Jednak test zostanie wykonany w kodzie serwera.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Błędy walidacji formatowania

Można kontrolować sposób wyświetlania błędów sprawdzania poprawności, definiując klasy CSS, które mają następujące zastrzeżone nazwy:

- `field-validation-error`. Definiuje dane wyjściowe metody `Html.ValidationMessage`, gdy jest wyświetlany błąd.
- `field-validation-valid`. Definiuje dane wyjściowe metody `Html.ValidationMessage` w przypadku braku błędu.
- `input-validation-error`. Definiuje, w jaki sposób elementy `<input>` są renderowane w przypadku wystąpienia błędu. (Na przykład można użyć tej klasy, aby ustawić kolor tła &lt;wejściowego&gt; elementu na inny kolor, jeśli jego wartość jest nieprawidłowa.) Ta klasa CSS jest używana tylko podczas weryfikacji klienta (w ASP.NET Web Pages 2).
- `input-validation-valid`. Definiuje wygląd elementów `<input>` w przypadku braku błędu.
- `validation-summary-errors`. Definiuje dane wyjściowe metody `Html.ValidationSummary`, która wyświetla listę błędów.
- `validation-summary-valid`. Definiuje dane wyjściowe metody `Html.ValidationSummary` w przypadku braku błędu.

Poniższy `<style>` bloku pokazuje reguły dotyczące warunków błędu.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Jeśli ten blok stylu zostanie uwzględniony na przykładowych stronach z wcześniejszego artykułu, zostanie wyświetlony komunikat o błędzie podobny do poniższego:

![Błędy walidacji korzystające z klas stylów CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Jeśli weryfikacja klienta nie jest używana w programie ASP.NET Web Pages 2, klasy CSS dla elementów `<input>` (`input-validation-error` i `input-validation-valid` nie mają żadnego efektu.

### <a name="static-and-dynamic-error-display"></a>Wyświetlanie błędów statycznych i dynamicznych

Reguły CSS są dostępne w parach, takich jak `validation-summary-errors` i `validation-summary-valid`. Te pary umożliwiają definiowanie reguł dla obu warunków: warunku błędu i warunku "normal" (bez błędu). Ważne jest, aby zrozumieć, że znaczniki wyświetlania błędów są zawsze renderowane, nawet jeśli nie ma błędów. Na przykład jeśli strona ma metodę `Html.ValidationSummary` w znaczniku, Źródło strony będzie zawierać następujące znaczniki, nawet gdy strona jest żądana po raz pierwszy:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Innymi słowy, Metoda `Html.ValidationSummary` zawsze renderuje element `<div>` i listę, nawet jeśli lista błędów jest pusta. Podobnie Metoda `Html.ValidationMessage` zawsze renderuje element `<span>` jako symbol zastępczy dla pojedynczego błędu pola, nawet jeśli nie ma błędu.

W niektórych sytuacjach wyświetlenie komunikatu o błędzie może spowodować zmianę przepływu strony i spowodować, że elementy na stronie mogą się poruszać. Reguły CSS, które kończą się w `-valid` umożliwiają zdefiniowanie układu, który może pomóc uniknąć tego problemu. Na przykład można zdefiniować `field-validation-error` i `field-validation-valid` do obu mają ten sam stały rozmiar. Dzięki temu obszar wyświetlania pola jest statyczny i nie zmienia przepływu strony, jeśli zostanie wyświetlony komunikat o błędzie.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Sprawdzanie poprawności danych, które nie pochodzą bezpośrednio od użytkowników

Czasami musisz sprawdzić poprawność informacji, które nie pochodzą bezpośrednio z formularza HTML. Typowy przykład to strona, w której wartość jest przenoszona w ciągu zapytania, jak w poniższym przykładzie:

`http://server/myapp/EditClassInformation?classid=1022`

W tym przypadku chcesz upewnić się, że wartość, która jest przenoszona na stronę (tutaj, 1022 dla wartości `classid`) jest prawidłowa. Nie można bezpośrednio użyć pomocnika `Validation` do przeprowadzenia tej walidacji. Można jednak użyć innych funkcji systemu sprawdzania poprawności, takich jak możliwość wyświetlania komunikatów o błędach walidacji.

> [!NOTE] 
> 
> **Ważne** Zawsze sprawdzaj poprawność wartości pobieranych z *dowolnego* źródła, w tym wartości pól formularza, wartości ciągu zapytania i wartości plików cookie. Można łatwo zmienić te wartości (na przykład w przypadku złośliwych celów). Dlatego należy sprawdzić te wartości w celu ochrony aplikacji.

Poniższy przykład pokazuje, jak można sprawdzić poprawność wartości, która została przekazana w ciągu zapytania. Kod sprawdza, czy wartość nie jest pusta i czy jest liczbą całkowitą.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Należy zauważyć, że test jest wykonywany, gdy żądanie nie jest przesłanym formularzem (`if(!IsPost)`). Ten test zostałby przekazany po raz pierwszy, gdy żądanie strony jest wymagane, ale nie w przypadku przesłania formularza.

Aby wyświetlić ten błąd, można dodać błąd do listy błędów walidacji, wywołując `Validation.AddFormError("message")`. Jeśli strona zawiera wywołanie metody `Html.ValidationSummary`, błąd jest wyświetlany w tym miejscu, podobnie jak w przypadku błędu walidacji danych wejściowych przez użytkownika.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Praca z formularzami HTML w witrynach stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)

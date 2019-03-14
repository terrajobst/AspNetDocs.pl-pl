---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067610"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a>[Wtyczka weryfikacji jQuery](https://jqueryvalidation.org/) — stanowią łatwe sprawdzania poprawności
================================

[![Stan kompilacji](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency stanu](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)

Wtyczka weryfikacji jQuery dostarcza sprawdzanie poprawności dzięki wsuwanej konstrukcji dla istniejących formularzy, podczas wykonywania wszystkich rodzajów dostosowań do aplikacji bardzo proste.

## <a name="getting-started"></a>Wprowadzenie

### <a name="downloading-the-prebuilt-files"></a>Pobieranie plików wbudowanych

Wstępnie utworzone pliki można pobrać z https://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Pobieranie najnowszych zmian

Pliki niewydany programistycznego można uzyskać przez:

 1. [Pobieranie](https://github.com/jquery-validation/jquery-validation/archive/master.zip) lub rozwidlenia tego repozytorium
 2. [Konfigurowanie kompilacji](CONTRIBUTING.md#build-setup)
 3. Uruchom `grunt` do utworzenia skompilowanych plików w katalogu "dist"

### <a name="including-it-on-your-page"></a>W tym go na stronie

Obejmują jQuery i wtyczki na stronie. Następnie wybierz formularz, aby zweryfikować i wywołać `validate` metody.

```html
<form>
    <input required>
</form>
<script src="jquery.js"></script>
<script src="jquery.validate.js"></script>
<script>
$("form").validate();
</script>
```

Można również uwzględnić jQuery i wtyczki za pomocą requirejs w module.

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

Aby uzyskać więcej informacji na temat konfigurowania reguł i dostosowania, [zapoznaj się z dokumentacją](https://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Raportowanie problemów i zasad współtworzenia

Zobacz [współtworzenia wytycznych](CONTRIBUTING.md) Aby uzyskać szczegółowe informacje.

**WAŻNA UWAGA DOTYCZĄCA WERYFIKACJI WIADOMOŚCI E-MAIL**. Począwszy od wersji 1.12.0 Ta wtyczka jest przy użyciu tego samego wyrażenia regularnego, [specyfikacji HTML5 sugeruje dla przeglądarek użyć](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Firma Microsoft ich kierownictwem i używać tego samego sprawdzenia. Jeśli uważasz, że specyfikację jest nieprawidłowy, zgłoś go do nich. Jeśli masz inne wymagania, należy wziąć pod uwagę [niestandardowej metody](https://jqueryvalidation.org/jQuery.validator.addMethod/).
W przypadku, gdy wymagane jest dostosowanie weryfikacji wbudowane wzorce wyrażeń regularnych, prosimy [zgodnie z dokumentacją](https://jqueryvalidation.org/jQuery.validator.methods/).

**WAŻNA UWAGA DOTYCZĄCA METODY WYMAGANE**. Począwszy od wersji 1.14.0 Ta wtyczka zatrzymuje przycinania spacji z wartości elementu dołączone. Jeśli chcesz osiągnąć ten sam rezultat, można użyć [ `normalizer` ](https://jqueryvalidation.org/normalizer/) który może służyć do przekształcania wartości elementu przed sprawdzania poprawności. Ta funkcja była dostępna od `v1.15.0`. Innymi słowy należy wykonać podobny do poniższego:
``` js
$("#myForm").validate({
    rules: {
        username: {
            required: true,
            // Using the normalizer to trim the value of the element
            // before validating it.
            //
            // The value of `this` inside the `normalizer` is the corresponding
            // DOMElement. In this example, `this` references the `username` element.
            normalizer: function(value) {
                return $.trim(value);
            }
        }
    }
});
```

## <a name="license"></a>Licencja
Copyright &copy; Jörn Zaefferer<br>
Licencjonowane na licencji MIT.

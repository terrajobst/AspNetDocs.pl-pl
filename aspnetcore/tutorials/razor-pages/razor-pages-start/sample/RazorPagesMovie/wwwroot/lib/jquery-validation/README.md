---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068504"
---
<a name="--"></a>--
================================

[![Stan kompilacji](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency stanu](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)

Wtyczka weryfikacji jQuery dostarcza sprawdzanie poprawności dzięki wsuwanej konstrukcji dla istniejących formularzy, podczas wykonywania wszystkich rodzajów dostosowań do aplikacji bardzo proste.

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[Pomoc projektu](http://pledgie.com/campaigns/18159)

[![Pomoc projektu](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)

Ten projekt jest szukasz pomocy! [Możesz przekazać do pledgie trwającej kampanii](http://pledgie.com/campaigns/18159) i rozłożyć wyraz. Jeśli była używana wtyczki lub planujesz użyć, należy wziąć pod uwagę darowizn — może pomóc w dowolnej ilości.

Można znaleźć planu, jak kupować [strony pledgie](http://pledgie.com/campaigns/18159).

## <a name="get-started"></a>Rozpocznij

### <a name="downloading-the-prebuilt-files"></a>Pobieranie plików wbudowanych

Wstępnie utworzone pliki można pobrać z http://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Pobieranie najnowszych zmian

Pliki niewydany programistycznego można uzyskać przez:

 1. [Pobieranie](https://github.com/jzaefferer/jquery-validation/archive/master.zip) lub rozwidlenia tego repozytorium
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

Aby uzyskać więcej informacji na temat konfigurowania reguł i dostosowania, [zapoznaj się z dokumentacją](http://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Raportowanie problemów i zasad współtworzenia

Zobacz [współtworzenia wytycznych](CONTRIBUTING.md) Aby uzyskać szczegółowe informacje.

**WAŻNA UWAGA DOTYCZĄCA WERYFIKACJI WIADOMOŚCI E-MAIL**. Począwszy od wersji 1.12.0 Ta wtyczka jest przy użyciu tego samego wyrażenia regularnego, [specyfikacji HTML5 sugeruje dla przeglądarek użyć](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Firma Microsoft ich kierownictwem i używać tego samego sprawdzenia. Jeśli uważasz, że specyfikację jest nieprawidłowy, zgłoś go do nich. Jeśli masz inne wymagania, należy wziąć pod uwagę [niestandardowej metody](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="license"></a>Licencja
Copyright &copy; Jörn Zaefferer<br>
Licencjonowane na licencji MIT.

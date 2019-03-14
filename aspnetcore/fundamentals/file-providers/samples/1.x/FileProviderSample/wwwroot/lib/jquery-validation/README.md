---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067610"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="6181c-101">[Wtyczka weryfikacji jQuery](https://jqueryvalidation.org/) — stanowią łatwe sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="6181c-101">[jQuery Validation Plugin](https://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="6181c-102">[![Stan kompilacji](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency stanu](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="6181c-102">[![Build Status](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="6181c-103">Wtyczka weryfikacji jQuery dostarcza sprawdzanie poprawności dzięki wsuwanej konstrukcji dla istniejących formularzy, podczas wykonywania wszystkich rodzajów dostosowań do aplikacji bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="6181c-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="getting-started"></a><span data-ttu-id="6181c-104">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="6181c-104">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="6181c-105">Pobieranie plików wbudowanych</span><span class="sxs-lookup"><span data-stu-id="6181c-105">Downloading the prebuilt files</span></span>

<span data-ttu-id="6181c-106">Wstępnie utworzone pliki można pobrać z https://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="6181c-106">Prebuilt files can be downloaded from https://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="6181c-107">Pobieranie najnowszych zmian</span><span class="sxs-lookup"><span data-stu-id="6181c-107">Downloading the latest changes</span></span>

<span data-ttu-id="6181c-108">Pliki niewydany programistycznego można uzyskać przez:</span><span class="sxs-lookup"><span data-stu-id="6181c-108">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="6181c-109">[Pobieranie](https://github.com/jquery-validation/jquery-validation/archive/master.zip) lub rozwidlenia tego repozytorium</span><span class="sxs-lookup"><span data-stu-id="6181c-109">[Downloading](https://github.com/jquery-validation/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="6181c-110">Konfigurowanie kompilacji</span><span class="sxs-lookup"><span data-stu-id="6181c-110">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="6181c-111">Uruchom `grunt` do utworzenia skompilowanych plików w katalogu "dist"</span><span class="sxs-lookup"><span data-stu-id="6181c-111">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="6181c-112">W tym go na stronie</span><span class="sxs-lookup"><span data-stu-id="6181c-112">Including it on your page</span></span>

<span data-ttu-id="6181c-113">Obejmują jQuery i wtyczki na stronie.</span><span class="sxs-lookup"><span data-stu-id="6181c-113">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="6181c-114">Następnie wybierz formularz, aby zweryfikować i wywołać `validate` metody.</span><span class="sxs-lookup"><span data-stu-id="6181c-114">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="6181c-115">Można również uwzględnić jQuery i wtyczki za pomocą requirejs w module.</span><span class="sxs-lookup"><span data-stu-id="6181c-115">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="6181c-116">Aby uzyskać więcej informacji na temat konfigurowania reguł i dostosowania, [zapoznaj się z dokumentacją](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="6181c-116">For more information on how to setup a rules and customizations, [check the documentation](https://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="6181c-117">Raportowanie problemów i zasad współtworzenia</span><span class="sxs-lookup"><span data-stu-id="6181c-117">Reporting issues and contributing code</span></span>

<span data-ttu-id="6181c-118">Zobacz [współtworzenia wytycznych](CONTRIBUTING.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="6181c-118">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="6181c-119">**WAŻNA UWAGA DOTYCZĄCA WERYFIKACJI WIADOMOŚCI E-MAIL**.</span><span class="sxs-lookup"><span data-stu-id="6181c-119">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="6181c-120">Począwszy od wersji 1.12.0 Ta wtyczka jest przy użyciu tego samego wyrażenia regularnego, [specyfikacji HTML5 sugeruje dla przeglądarek użyć](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="6181c-120">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="6181c-121">Firma Microsoft ich kierownictwem i używać tego samego sprawdzenia.</span><span class="sxs-lookup"><span data-stu-id="6181c-121">We will follow their lead and use the same check.</span></span> <span data-ttu-id="6181c-122">Jeśli uważasz, że specyfikację jest nieprawidłowy, zgłoś go do nich.</span><span class="sxs-lookup"><span data-stu-id="6181c-122">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="6181c-123">Jeśli masz inne wymagania, należy wziąć pod uwagę [niestandardowej metody](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="6181c-123">If you have different requirements, consider [using a custom method](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>
<span data-ttu-id="6181c-124">W przypadku, gdy wymagane jest dostosowanie weryfikacji wbudowane wzorce wyrażeń regularnych, prosimy [zgodnie z dokumentacją](https://jqueryvalidation.org/jQuery.validator.methods/).</span><span class="sxs-lookup"><span data-stu-id="6181c-124">In case you need to adjust the built-in validation regular expression patterns, please [follow the documentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span></span>

<span data-ttu-id="6181c-125">**WAŻNA UWAGA DOTYCZĄCA METODY WYMAGANE**.</span><span class="sxs-lookup"><span data-stu-id="6181c-125">**IMPORTANT NOTE ABOUT REQUIRED METHOD**.</span></span> <span data-ttu-id="6181c-126">Począwszy od wersji 1.14.0 Ta wtyczka zatrzymuje przycinania spacji z wartości elementu dołączone.</span><span class="sxs-lookup"><span data-stu-id="6181c-126">As of version 1.14.0 this plugin stops trimming white spaces from the value of the attached element.</span></span> <span data-ttu-id="6181c-127">Jeśli chcesz osiągnąć ten sam rezultat, można użyć [ `normalizer` ](https://jqueryvalidation.org/normalizer/) który może służyć do przekształcania wartości elementu przed sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="6181c-127">If you want to achieve the same result, you can use the [`normalizer`](https://jqueryvalidation.org/normalizer/) that can be used to transform the value of an element before validation.</span></span> <span data-ttu-id="6181c-128">Ta funkcja była dostępna od `v1.15.0`.</span><span class="sxs-lookup"><span data-stu-id="6181c-128">This feature was available since `v1.15.0`.</span></span> <span data-ttu-id="6181c-129">Innymi słowy należy wykonać podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="6181c-129">In other words, you can do something like this:</span></span>
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

## <a name="license"></a><span data-ttu-id="6181c-130">Licencja</span><span class="sxs-lookup"><span data-stu-id="6181c-130">License</span></span>
<span data-ttu-id="6181c-131">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="6181c-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="6181c-132">Licencjonowane na licencji MIT.</span><span class="sxs-lookup"><span data-stu-id="6181c-132">Licensed under the MIT license.</span></span>

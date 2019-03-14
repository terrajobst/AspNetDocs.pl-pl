---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072608"
---
<a name="--"></a>--
================================

<span data-ttu-id="6f2cb-101">[![Stan kompilacji](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency stanu](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="6f2cb-101">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="6f2cb-102">Wtyczka weryfikacji jQuery dostarcza sprawdzanie poprawności dzięki wsuwanej konstrukcji dla istniejących formularzy, podczas wykonywania wszystkich rodzajów dostosowań do aplikacji bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-102">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="6f2cb-103">Pomoc projektu</span><span class="sxs-lookup"><span data-stu-id="6f2cb-103">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="6f2cb-104">[![Pomoc projektu](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="6f2cb-104">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="6f2cb-105">Ten projekt jest szukasz pomocy!</span><span class="sxs-lookup"><span data-stu-id="6f2cb-105">This project is looking for help!</span></span> <span data-ttu-id="6f2cb-106">[Możesz przekazać do pledgie trwającej kampanii](http://pledgie.com/campaigns/18159) i rozłożyć wyraz.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-106">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="6f2cb-107">Jeśli była używana wtyczki lub planujesz użyć, należy wziąć pod uwagę darowizn — może pomóc w dowolnej ilości.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-107">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="6f2cb-108">Można znaleźć planu, jak kupować [strony pledgie](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="6f2cb-108">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="get-started"></a><span data-ttu-id="6f2cb-109">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="6f2cb-109">Get Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="6f2cb-110">Pobieranie plików wbudowanych</span><span class="sxs-lookup"><span data-stu-id="6f2cb-110">Downloading the prebuilt files</span></span>

<span data-ttu-id="6f2cb-111">Wstępnie utworzone pliki można pobrać z http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="6f2cb-111">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="6f2cb-112">Pobieranie najnowszych zmian</span><span class="sxs-lookup"><span data-stu-id="6f2cb-112">Downloading the latest changes</span></span>

<span data-ttu-id="6f2cb-113">Pliki niewydany programistycznego można uzyskać przez:</span><span class="sxs-lookup"><span data-stu-id="6f2cb-113">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="6f2cb-114">[Pobieranie](https://github.com/jzaefferer/jquery-validation/archive/master.zip) lub rozwidlenia tego repozytorium</span><span class="sxs-lookup"><span data-stu-id="6f2cb-114">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="6f2cb-115">Konfigurowanie kompilacji</span><span class="sxs-lookup"><span data-stu-id="6f2cb-115">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="6f2cb-116">Uruchom `grunt` do utworzenia skompilowanych plików w katalogu "dist"</span><span class="sxs-lookup"><span data-stu-id="6f2cb-116">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="6f2cb-117">W tym go na stronie</span><span class="sxs-lookup"><span data-stu-id="6f2cb-117">Including it on your page</span></span>

<span data-ttu-id="6f2cb-118">Obejmują jQuery i wtyczki na stronie.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-118">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="6f2cb-119">Następnie wybierz formularz, aby zweryfikować i wywołać `validate` metody.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-119">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="6f2cb-120">Można również uwzględnić jQuery i wtyczki za pomocą requirejs w module.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-120">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="6f2cb-121">Aby uzyskać więcej informacji na temat konfigurowania reguł i dostosowania, [zapoznaj się z dokumentacją](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="6f2cb-121">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="6f2cb-122">Raportowanie problemów i zasad współtworzenia</span><span class="sxs-lookup"><span data-stu-id="6f2cb-122">Reporting issues and contributing code</span></span>

<span data-ttu-id="6f2cb-123">Zobacz [współtworzenia wytycznych](CONTRIBUTING.md) Aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-123">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="6f2cb-124">**WAŻNA UWAGA DOTYCZĄCA WERYFIKACJI WIADOMOŚCI E-MAIL**.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-124">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="6f2cb-125">Począwszy od wersji 1.12.0 Ta wtyczka jest przy użyciu tego samego wyrażenia regularnego, [specyfikacji HTML5 sugeruje dla przeglądarek użyć](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="6f2cb-125">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="6f2cb-126">Firma Microsoft ich kierownictwem i używać tego samego sprawdzenia.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-126">We will follow their lead and use the same check.</span></span> <span data-ttu-id="6f2cb-127">Jeśli uważasz, że specyfikację jest nieprawidłowy, zgłoś go do nich.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-127">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="6f2cb-128">Jeśli masz inne wymagania, należy wziąć pod uwagę [niestandardowej metody](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="6f2cb-128">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="6f2cb-129">Licencja</span><span class="sxs-lookup"><span data-stu-id="6f2cb-129">License</span></span>
<span data-ttu-id="6f2cb-130">Copyright &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="6f2cb-130">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="6f2cb-131">Licencjonowane na licencji MIT.</span><span class="sxs-lookup"><span data-stu-id="6f2cb-131">Licensed under the MIT license.</span></span>

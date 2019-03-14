---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067097"
---
# <a name="jquery"></a><span data-ttu-id="54956-101">jQuery</span><span class="sxs-lookup"><span data-stu-id="54956-101">jQuery</span></span>

> <span data-ttu-id="54956-102">jQuery jest szybkie, małe i bogate biblioteki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="54956-102">jQuery is a fast, small, and feature-rich JavaScript library.</span></span>

<span data-ttu-id="54956-103">Aby uzyskać informacji o sposobie rozpoczęcia pracy i sposobu użycia jQuery, zobacz [dokumentacji firmy jQuery](http://api.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="54956-103">For information on how to get started and how to use jQuery, please see [jQuery's documentation](http://api.jquery.com/).</span></span>
<span data-ttu-id="54956-104">Pliki źródłowe i problemy można znaleźć [repozytorium jQuery](https://github.com/jquery/jquery).</span><span class="sxs-lookup"><span data-stu-id="54956-104">For source files and issues, please visit the [jQuery repo](https://github.com/jquery/jquery).</span></span>

<span data-ttu-id="54956-105">Jeśli uaktualnienie, zobacz [wpis w blogu dla 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span><span class="sxs-lookup"><span data-stu-id="54956-105">If upgrading, please see the [blog post for 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span></span> <span data-ttu-id="54956-106">Dotyczy to również istotne różnice w poprzedniej wersji i bardziej czytelny dziennika zmian.</span><span class="sxs-lookup"><span data-stu-id="54956-106">This includes notable differences from the previous version and a more readable changelog.</span></span>

## <a name="including-jquery"></a><span data-ttu-id="54956-107">W tym jQuery</span><span class="sxs-lookup"><span data-stu-id="54956-107">Including jQuery</span></span>

<span data-ttu-id="54956-108">Poniżej przedstawiono niektóre z najbardziej typowych sposobów, aby uwzględnić jQuery.</span><span class="sxs-lookup"><span data-stu-id="54956-108">Below are some of the most common ways to include jQuery.</span></span>

### <a name="browser"></a><span data-ttu-id="54956-109">Przeglądarka</span><span class="sxs-lookup"><span data-stu-id="54956-109">Browser</span></span>

#### <a name="script-tag"></a><span data-ttu-id="54956-110">Tag skryptu</span><span class="sxs-lookup"><span data-stu-id="54956-110">Script tag</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a><span data-ttu-id="54956-111">Babel</span><span class="sxs-lookup"><span data-stu-id="54956-111">Babel</span></span>

<span data-ttu-id="54956-112">[Babel](http://babeljs.io/) jest dalej kompilatora JavaScript generacji.</span><span class="sxs-lookup"><span data-stu-id="54956-112">[Babel](http://babeljs.io/) is a next generation JavaScript compiler.</span></span> <span data-ttu-id="54956-113">Jedna z funkcji jest możliwość używania modułów ES6/ES2015 teraz, mimo, że przeglądarki nie jest jeszcze obsługiwany tej funkcji natywnie.</span><span class="sxs-lookup"><span data-stu-id="54956-113">One of the features is the ability to use ES6/ES2015 modules now, even though browsers do not yet support this feature natively.</span></span>

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a><span data-ttu-id="54956-114">Browserify Webpack</span><span class="sxs-lookup"><span data-stu-id="54956-114">Browserify/Webpack</span></span>

<span data-ttu-id="54956-115">Istnieje kilka sposobów, aby użyć [Browserify](http://browserify.org/) i [Webpack](https://webpack.github.io/).</span><span class="sxs-lookup"><span data-stu-id="54956-115">There are several ways to use [Browserify](http://browserify.org/) and [Webpack](https://webpack.github.io/).</span></span> <span data-ttu-id="54956-116">Aby uzyskać więcej informacji na temat korzystania z tych narzędzi można znaleźć w dokumentacji odpowiedniego projektu.</span><span class="sxs-lookup"><span data-stu-id="54956-116">For more information on using these tools, please refer to the corresponding project's documention.</span></span> <span data-ttu-id="54956-117">W skrypcie łącznie z jQuery zazwyczaj będzie wyglądał jak poniżej...</span><span class="sxs-lookup"><span data-stu-id="54956-117">In the script, including jQuery will usually look like this...</span></span>

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a><span data-ttu-id="54956-118">AMD (definicja asynchronicznego modułu)</span><span class="sxs-lookup"><span data-stu-id="54956-118">AMD (Asynchronous Module Definition)</span></span>

<span data-ttu-id="54956-119">AMD jest stworzona z myślą o przeglądarce formatu modułu.</span><span class="sxs-lookup"><span data-stu-id="54956-119">AMD is a module format built for the browser.</span></span> <span data-ttu-id="54956-120">Aby uzyskać więcej informacji, firma Microsoft zaleca [dokumentacji require.js](http://requirejs.org/docs/whyamd.html).</span><span class="sxs-lookup"><span data-stu-id="54956-120">For more information, we recommend [require.js' documentation](http://requirejs.org/docs/whyamd.html).</span></span>

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a><span data-ttu-id="54956-121">Węzeł</span><span class="sxs-lookup"><span data-stu-id="54956-121">Node</span></span>

<span data-ttu-id="54956-122">Aby uwzględnić jQuery w [węzła](nodejs.org), najpierw zainstaluj za pomocą pakietu npm.</span><span class="sxs-lookup"><span data-stu-id="54956-122">To include jQuery in [Node](nodejs.org), first install with npm.</span></span>

```sh
npm install jquery
```

<span data-ttu-id="54956-123">W bibliotece jQuery pracę w węźle okno dokumentu jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="54956-123">For jQuery to work in Node, a window with a document is required.</span></span> <span data-ttu-id="54956-124">Ponieważ nie takie istnieje przedział czasu natywnie w węźle, jeden może być w postaci makiet przez narzędzia takie jak [jsdom](https://github.com/tmpvar/jsdom).</span><span class="sxs-lookup"><span data-stu-id="54956-124">Since no such window exists natively in Node, one can be mocked by tools such as [jsdom](https://github.com/tmpvar/jsdom).</span></span> <span data-ttu-id="54956-125">Może to być przydatne do celów testowych.</span><span class="sxs-lookup"><span data-stu-id="54956-125">This can be useful for testing purposes.</span></span>

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```

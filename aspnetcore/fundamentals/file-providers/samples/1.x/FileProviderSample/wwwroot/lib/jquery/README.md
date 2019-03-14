---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067097"
---
# <a name="jquery"></a>jQuery

> jQuery jest szybkie, małe i bogate biblioteki JavaScript.

Aby uzyskać informacji o sposobie rozpoczęcia pracy i sposobu użycia jQuery, zobacz [dokumentacji firmy jQuery](http://api.jquery.com/).
Pliki źródłowe i problemy można znaleźć [repozytorium jQuery](https://github.com/jquery/jquery).

Jeśli uaktualnienie, zobacz [wpis w blogu dla 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/). Dotyczy to również istotne różnice w poprzedniej wersji i bardziej czytelny dziennika zmian.

## <a name="including-jquery"></a>W tym jQuery

Poniżej przedstawiono niektóre z najbardziej typowych sposobów, aby uwzględnić jQuery.

### <a name="browser"></a>Przeglądarka

#### <a name="script-tag"></a>Tag skryptu

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a>Babel

[Babel](http://babeljs.io/) jest dalej kompilatora JavaScript generacji. Jedna z funkcji jest możliwość używania modułów ES6/ES2015 teraz, mimo, że przeglądarki nie jest jeszcze obsługiwany tej funkcji natywnie.

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a>Browserify Webpack

Istnieje kilka sposobów, aby użyć [Browserify](http://browserify.org/) i [Webpack](https://webpack.github.io/). Aby uzyskać więcej informacji na temat korzystania z tych narzędzi można znaleźć w dokumentacji odpowiedniego projektu. W skrypcie łącznie z jQuery zazwyczaj będzie wyglądał jak poniżej...

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a>AMD (definicja asynchronicznego modułu)

AMD jest stworzona z myślą o przeglądarce formatu modułu. Aby uzyskać więcej informacji, firma Microsoft zaleca [dokumentacji require.js](http://requirejs.org/docs/whyamd.html).

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a>Węzeł

Aby uwzględnić jQuery w [węzła](nodejs.org), najpierw zainstaluj za pomocą pakietu npm.

```sh
npm install jquery
```

W bibliotece jQuery pracę w węźle okno dokumentu jest wymagana. Ponieważ nie takie istnieje przedział czasu natywnie w węźle, jeden może być w postaci makiet przez narzędzia takie jak [jsdom](https://github.com/tmpvar/jsdom). Może to być przydatne do celów testowych.

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```

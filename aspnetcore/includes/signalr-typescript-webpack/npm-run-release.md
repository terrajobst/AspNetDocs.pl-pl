---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175902"
---
```console
npm run release
```

To polecenie daje zasoby po stronie klienta, który ma być obsługiwana podczas uruchamiania aplikacji. Zasoby są umieszczane w *wwwroot* folderu.

Webpack wykonane następujące zadania:

* Przeczyścić zawartość *wwwroot* katalogu.
* Przekonwertowany TypeScript w kodzie JavaScript&mdash;proces ten jest znany jako *transpilation*.
* Wygenerowany język JavaScript, aby zmniejszyć rozmiar pliku zniekształcone&mdash;proces ten jest znany jako *minimalizację*.
* Skopiowane przetworzone pliki JavaScript, CSS i HTML z *src* do *wwwroot* katalogu.
* Dodane następujące elementy do *wwwroot/index.html* pliku:
    * A `<link>` tag, odwołuje się do *wwwroot/main.\< skrót\>.css* pliku. Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</head>` tagu.
    * A `<script>` tagu, odwołuje się do zminimalizowany *wwwroot/main.\< skrót\>js* pliku. Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</body>` tagu.

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

<span data-ttu-id="f1db7-101">To polecenie daje zasoby po stronie klienta, który ma być obsługiwana podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1db7-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="f1db7-102">Zasoby są umieszczane w *wwwroot* folderu.</span><span class="sxs-lookup"><span data-stu-id="f1db7-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="f1db7-103">Webpack wykonane następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="f1db7-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="f1db7-104">Przeczyścić zawartość *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="f1db7-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="f1db7-105">Przekonwertowany TypeScript w kodzie JavaScript&mdash;proces ten jest znany jako *transpilation*.</span><span class="sxs-lookup"><span data-stu-id="f1db7-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="f1db7-106">Wygenerowany język JavaScript, aby zmniejszyć rozmiar pliku zniekształcone&mdash;proces ten jest znany jako *minimalizację*.</span><span class="sxs-lookup"><span data-stu-id="f1db7-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="f1db7-107">Skopiowane przetworzone pliki JavaScript, CSS i HTML z *src* do *wwwroot* katalogu.</span><span class="sxs-lookup"><span data-stu-id="f1db7-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="f1db7-108">Dodane następujące elementy do *wwwroot/index.html* pliku:</span><span class="sxs-lookup"><span data-stu-id="f1db7-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
    * <span data-ttu-id="f1db7-109">A `<link>` tag, odwołuje się do *wwwroot/main.\< skrót\>.css* pliku.</span><span class="sxs-lookup"><span data-stu-id="f1db7-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="f1db7-110">Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</head>` tagu.</span><span class="sxs-lookup"><span data-stu-id="f1db7-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
    * <span data-ttu-id="f1db7-111">A `<script>` tagu, odwołuje się do zminimalizowany *wwwroot/main.\< skrót\>js* pliku.</span><span class="sxs-lookup"><span data-stu-id="f1db7-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="f1db7-112">Ten tag jest umieszczany bezpośrednio przed tagiem zamykającym `</body>` tagu.</span><span class="sxs-lookup"><span data-stu-id="f1db7-112">This tag is placed immediately before the closing `</body>` tag.</span></span>

---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57078122"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[<span data-ttu-id="b8c97-101">Otwórz v1.1.1 ikony</span><span class="sxs-lookup"><span data-stu-id="b8c97-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a><span data-ttu-id="b8c97-102">Otwórz Iconic jest równorzędny typu open source [Iconic](http://useiconic.com).</span><span class="sxs-lookup"><span data-stu-id="b8c97-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="b8c97-103">Jest to kolekcja hyper czytelne 223 ikon z niewielką zużycie&mdash;gotowe do użycia z ładowania początkowego i Foundation.</span><span class="sxs-lookup"><span data-stu-id="b8c97-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="b8c97-104">Widok kolekcji</span><span class="sxs-lookup"><span data-stu-id="b8c97-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="b8c97-105">Co to jest otwarta ikony?</span><span class="sxs-lookup"><span data-stu-id="b8c97-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="b8c97-106">223 ikon, które mają być czytelny w dół do 8 pikseli</span><span class="sxs-lookup"><span data-stu-id="b8c97-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="b8c97-107">Bardzo lekkie pliki SVG — 61.8 dla całego zestawu</span><span class="sxs-lookup"><span data-stu-id="b8c97-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="b8c97-108">SVG sprite&mdash;nowoczesnego zamiennika ikona czcionki</span><span class="sxs-lookup"><span data-stu-id="b8c97-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="b8c97-109">Formatuje Webfont (EOT OTF, SVG, TTF, WOFF), PNG i WebP</span><span class="sxs-lookup"><span data-stu-id="b8c97-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="b8c97-110">Webfont arkuszy stylów (łącznie z wersjami narzędzia Bootstrap i Foundation) w pliku CSS mniej formaty SCSS i pióra</span><span class="sxs-lookup"><span data-stu-id="b8c97-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="b8c97-111">PNG i WebP obrazy rastrowe w 8px, 16px, 24px, 32px 48px i 64px.</span><span class="sxs-lookup"><span data-stu-id="b8c97-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="b8c97-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="b8c97-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a><span data-ttu-id="b8c97-113">Przykłady kodu i wszystkie inne konieczne wprowadzenie Otwórz ikony zapoznaj się z naszym [ikony](http://useiconic.com/open#icons) i [odwołania](http://useiconic.com/open#reference) sekcje.</span><span class="sxs-lookup"><span data-stu-id="b8c97-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="b8c97-114">Zastosowania ogólne</span><span class="sxs-lookup"><span data-stu-id="b8c97-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="b8c97-115">Za pomocą SVGs Open ikony firmy</span><span class="sxs-lookup"><span data-stu-id="b8c97-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="b8c97-116">Firma Microsoft, takich jak SVGs, a sądzimy, że są one sposobem wyświetlania ikon w sieci web.</span><span class="sxs-lookup"><span data-stu-id="b8c97-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="b8c97-117">Ponieważ Otwórz ikony są tylko podstawowe SVGs, zalecamy ich wyświetlania, jak w przypadku dowolnego innego obrazu (nie zapomnij `alt` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="b8c97-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="b8c97-118">Za pomocą Sprite SVG Open ikony firmy</span><span class="sxs-lookup"><span data-stu-id="b8c97-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="b8c97-119">Otwórz Iconic jest dostępna również w sprite SVG, dzięki czemu można wyświetlić wszystkie ikony w zestawie przy użyciu pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="b8c97-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="b8c97-120">Przypomina to czcionka ikon, nie będąc hack.</span><span class="sxs-lookup"><span data-stu-id="b8c97-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="b8c97-121">Dodawanie ikony z SVG sprite nieco różni się od co to są używane do, ale nadal jest Tort z fragmentem.</span><span class="sxs-lookup"><span data-stu-id="b8c97-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="b8c97-122">*Porada: Aby ikony łatwo styl możliwe, zalecamy dodawanie klasy ogólnej do* `<svg>` *tag i unikalną nazwę klasy dla każdego inną ikonę w* `<use>` *tagu.*</span><span class="sxs-lookup"><span data-stu-id="b8c97-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="b8c97-123">Ustalanie rozmiaru ikony musi tylko podstawowe CSS.</span><span class="sxs-lookup"><span data-stu-id="b8c97-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="b8c97-124">Wszystkie ikony w kształcie kwadratu formacie, więc po prostu ustaw `<svg>` oznaczanie numerem jednakowe wymiary szerokość i wysokość.</span><span class="sxs-lookup"><span data-stu-id="b8c97-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="b8c97-125">Kolorowanie ikony jest jeszcze łatwiejsze.</span><span class="sxs-lookup"><span data-stu-id="b8c97-125">Coloring icons is even easier.</span></span> <span data-ttu-id="b8c97-126">Wszystko, czego potrzebujesz, aby zrobić ustawiono `fill` reguła `<use>` tagu.</span><span class="sxs-lookup"><span data-stu-id="b8c97-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="b8c97-127">Aby dowiedzieć się więcej na temat obrazki SVG, przeczytaj [przewodnik Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="b8c97-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="b8c97-128">Przy użyciu ikony firmy Open ikona czcionki...</span><span class="sxs-lookup"><span data-stu-id="b8c97-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="b8c97-129">...zwykle bootstrap</span><span class="sxs-lookup"><span data-stu-id="b8c97-129">…with Bootstrap</span></span>

<span data-ttu-id="b8c97-130">Możesz znaleźć naszych Bootstrap arkusze stylów w `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="b8c97-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="b8c97-131">...zwykle foundation</span><span class="sxs-lookup"><span data-stu-id="b8c97-131">…with Foundation</span></span>

<span data-ttu-id="b8c97-132">Możesz znaleźć naszych arkusze stylów Foundation, w `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="b8c97-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="b8c97-133">...na ekranie swój własny</span><span class="sxs-lookup"><span data-stu-id="b8c97-133">…on its own</span></span>

<span data-ttu-id="b8c97-134">Możesz znaleźć naszych arkusze stylów domyślny, w `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="b8c97-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="b8c97-135">Licencja</span><span class="sxs-lookup"><span data-stu-id="b8c97-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="b8c97-136">Ikony</span><span class="sxs-lookup"><span data-stu-id="b8c97-136">Icons</span></span>

<span data-ttu-id="b8c97-137">Cały kod (w tym znaczników SVG) podlega [licencją MIT](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="b8c97-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="b8c97-138">Czcionki</span><span class="sxs-lookup"><span data-stu-id="b8c97-138">Fonts</span></span>

<span data-ttu-id="b8c97-139">Wszystkie czcionki znajdują się w [licencjonowane SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="b8c97-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>

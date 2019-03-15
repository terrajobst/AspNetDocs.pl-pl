---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074921"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[Otwórz v1.1.1 ikony](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a>Otwórz Iconic jest równorzędny typu open source [Iconic](http://useiconic.com). Jest to kolekcja hyper czytelne 223 ikon z niewielką zużycie&mdash;gotowe do użycia z ładowania początkowego i Foundation. [Widok kolekcji](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>Co to jest otwarta ikony?

* 223 ikon, które mają być czytelny w dół do 8 pikseli
* Bardzo lekkie pliki SVG — 61.8 dla całego zestawu 
* SVG sprite&mdash;nowoczesnego zamiennika ikona czcionki
* Formatuje Webfont (EOT OTF, SVG, TTF, WOFF), PNG i WebP
* Webfont arkuszy stylów (łącznie z wersjami narzędzia Bootstrap i Foundation) w pliku CSS mniej formaty SCSS i pióra
* PNG i WebP obrazy rastrowe w 8px, 16px, 24px, 32px 48px i 64px.


## <a name="getting-started"></a>Wprowadzenie

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a>Przykłady kodu i wszystkie inne konieczne wprowadzenie Otwórz ikony zapoznaj się z naszym [ikony](http://useiconic.com/open#icons) i [odwołania](http://useiconic.com/open#reference) sekcje.

### <a name="general-usage"></a>Zastosowania ogólne

#### <a name="using-open-iconics-svgs"></a>Za pomocą SVGs Open ikony firmy

Firma Microsoft, takich jak SVGs, a sądzimy, że są one sposobem wyświetlania ikon w sieci web. Ponieważ Otwórz ikony są tylko podstawowe SVGs, zalecamy ich wyświetlania, jak w przypadku dowolnego innego obrazu (nie zapomnij `alt` atrybutu).

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Za pomocą Sprite SVG Open ikony firmy

Otwórz Iconic jest dostępna również w sprite SVG, dzięki czemu można wyświetlić wszystkie ikony w zestawie przy użyciu pojedynczego żądania. Przypomina to czcionka ikon, nie będąc hack.

Dodawanie ikony z SVG sprite nieco różni się od co to są używane do, ale nadal jest Tort z fragmentem. *Porada: Aby ikony łatwo styl możliwe, zalecamy dodawanie klasy ogólnej do* `<svg>` *tag i unikalną nazwę klasy dla każdego inną ikonę w* `<use>` *tagu.*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

Ustalanie rozmiaru ikony musi tylko podstawowe CSS. Wszystkie ikony w kształcie kwadratu formacie, więc po prostu ustaw `<svg>` oznaczanie numerem jednakowe wymiary szerokość i wysokość.

```
.icon {
  width: 16px;
  height: 16px;
}
```

Kolorowanie ikony jest jeszcze łatwiejsze. Wszystko, czego potrzebujesz, aby zrobić ustawiono `fill` reguła `<use>` tagu.

```
.icon-account-login {
  fill: #f00;
}
```

Aby dowiedzieć się więcej na temat obrazki SVG, przeczytaj [przewodnik Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Przy użyciu ikony firmy Open ikona czcionki...


##### <a name="with-bootstrap"></a>...zwykle bootstrap

Możesz znaleźć naszych Bootstrap arkusze stylów w `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>...zwykle foundation

Możesz znaleźć naszych arkusze stylów Foundation, w `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>...na ekranie swój własny

Możesz znaleźć naszych arkusze stylów domyślny, w `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Licencja

### <a name="icons"></a>Ikony

Cały kod (w tym znaczników SVG) podlega [licencją MIT](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Czcionki

Wszystkie czcionki znajdują się w [licencjonowane SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).

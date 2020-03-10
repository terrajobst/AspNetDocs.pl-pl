---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Używanie klasy TagBuilder do kompilowania pomocników HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther wprowadza do przydatnej klasy narzędzi w strukturze ASP.NET MVC o nazwie klasy TagBuilder. Można użyć klasy TagBuilder, aby łatwo...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b0aa9816209cc326d3dea4b8dfb1b13cf697fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600003"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>Używanie klasy TagBuilder do kompilowania pomocników HTML (VB)

Autor [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther wprowadza do przydatnej klasy narzędzi w strukturze ASP.NET MVC o nazwie klasy TagBuilder. Można użyć klasy TagBuilder, aby łatwo tworzyć Tagi HTML.

Struktura ASP.NET MVC zawiera użyteczną klasę narzędzi o nazwie Klasa TagBuilder, której można użyć podczas tworzenia pomocników HTML. Klasa TagBuilder, jako nazwa klasy sugeruje, pozwala łatwo tworzyć Tagi HTML. W tym krótkim samouczku udostępniono Przegląd klasy TagBuilder i dowiesz się, jak używać tej klasy podczas kompilowania prostego pomocnika HTML, który renderuje Tagi &lt;IMG&gt; HTML.

## <a name="overview-of-the-tagbuilder-class"></a>Przegląd klasy TagBuilder

Klasa TagBuilder jest zawarta w przestrzeni nazw System. Web. MVC. Ma pięć metod:

- AddCssClass () — umożliwia dodanie nowego atrybutu *Class = ""* do tagu.
- GenerateId () — umożliwia dodanie atrybutu ID do znacznika. Ta metoda automatycznie zastępuje okresy w identyfikatorze (domyślnie okresy są zamieniane na znaki podkreślenia)
- Mergeattribute () — umożliwia dodanie atrybutów do tagu. Istnieje wiele przeciążeń tej metody.
- SetInnerText () — umożliwia ustawienie wewnętrznego tekstu znacznika. Tekst wewnętrzny jest automatycznie zakodowany w formacie HTML.
- ToString () — umożliwia renderowanie znacznika. Możesz określić, czy chcesz utworzyć tag normalny, tag początkowy, tag końcowy lub tag samozamykający.

Klasa TagBuilder ma cztery ważne właściwości:

- Atrybuty — reprezentuje wszystkie atrybuty znacznika.
- IdAttributeDotReplacement — reprezentuje znak używany przez metodę GenerateId () do zastępowania kropek (wartość domyślna to podkreślenie).
- InnerHTML — reprezentuje wewnętrzną zawartość znacznika. Przypisanie ciągu do tej właściwości nie *powoduje* kodowania ciągu html.
- TagName — reprezentuje nazwę znacznika.

Te metody i właściwości zawierają wszystkie podstawowe metody i właściwości, które należy wykonać, aby utworzyć tag HTML. Nie musisz naprawdę używać klasy TagBuilder. Zamiast tego można użyć klasy StringBuilder. Jednak Klasa TagBuilder sprawia, że życie jest nieco prostsze.

## <a name="creating-an-image-html-helper"></a>Tworzenie pomocnika HTML obrazu

Podczas tworzenia wystąpienia klasy TagBuilder, należy przekazać nazwę tagu, który chcesz skompilować do konstruktora TagBuilder. Następnie można wywołać metody, takie jak AddCssClass i Mergeattribute (), aby zmodyfikować atrybuty znacznika. Na koniec należy wywołać metodę ToString (), aby renderować tag.

Na przykład lista 1 zawiera pomocnika HTML obrazu. Pomocnik obrazu jest implementowany wewnętrznie z TagBuilder, który reprezentuje tag&gt; HTML &lt;img.

**Lista 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

Moduł na liście 1 zawiera dwie przeciążone metody o nazwie Image (). Po wywołaniu metody Image () można przekazać obiekt, który reprezentuje zestaw atrybutów HTML.

Zwróć uwagę, jak Metoda TagBuilder. Mergeattribute () służy do dodawania pojedynczych atrybutów, takich jak atrybut src do TagBuilder. Należy zauważyć, że Ponadto Metoda TagBuilder. MergeAttributes () służy do dodawania kolekcji atrybutów do TagBuilder. Metoda MergeAttributes () akceptuje&lt;słownika,&gt; parametr obiektu. Klasa RouteValueDictionary jest używana do konwersji obiektu reprezentującego kolekcję atrybutów do słownika&lt;ciągu,&gt;obiektu.

Po utworzeniu pomocnika obrazów można używać pomocnika w widokach ASP.NET MVC podobnie jak w przypadku innych pomocników w języku HTML. Widok w liście 2 używa pomocnika obrazu do wyświetlenia tego samego obrazu konsoli Xbox dwa razy (patrz rysunek 1). Pomocnik obrazu () jest wywoływana zarówno z, jak i bez kolekcji atrybutów HTML.

**Lista 2 — Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]

[![okno dialogowe Nowy projekt](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Ilustracja 01**. Korzystanie z pomocnika obrazu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))

Należy zauważyć, że należy zaimportować przestrzeń nazw skojarzoną z pomocnikiem obrazu w górnej części widoku index. aspx. Pomocnik zostaje zaimportowany do następującej dyrektywy:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

W aplikacji Visual Basic domyślna przestrzeń nazw jest taka sama jak nazwa aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](creating-custom-html-helpers-vb.md)
> [dalej](creating-page-layouts-with-view-master-pages-vb.md)

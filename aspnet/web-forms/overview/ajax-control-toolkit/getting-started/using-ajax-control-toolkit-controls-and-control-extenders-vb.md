---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Korzystanie z kontrolek zestawu narzędzi AJAX Control i rozszerzeń formantów (VB) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak dodać kontrolki i rozszerzalności zestawu narzędzi AJAX Control do stron ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 90a6003ff50ba6e85196c25cf175e057810f0f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613380"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Używanie kontrolek i rozszerzeń kontrolek zestawu narzędzi AJAX Control Toolkit (VB)

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać kontrolki i rozszerzalności zestawu narzędzi AJAX Control do stron ASP.NET.

Zestaw narzędzi AJAX Control Toolkit zawiera zestaw kontrolek i rozszerzeń kontrolek. W tym krótkim samouczku dowiesz się, jak dodać kontrolki i regulatory formantów do strony ASP.NET.

> [!NOTE] 
> 
> Aby uzyskać instrukcje dotyczące instalowania zestawu narzędzi AJAX Control Toolkit i dodawania zestawu narzędzi AJAX Control Toolkit do przybornika Visual Studio/Visual Web Developer, zobacz samouczek [wprowadzenie do zestawu narzędzi AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).

## <a name="using-ajax-control-toolkit-controls"></a>Korzystanie z formantów zestawu narzędzi AJAX Control Toolkit

Kontrolka zestawu narzędzi AJAX Control działa podobnie jak normalna kontrolka ASP.NET. Możesz przeciągnąć formant z przybornika na stronę ASP.NET. Możesz dodać formant do strony w widok Projekt lub widoku źródła.

Istnieje jedno specjalne wymaganie w przypadku używania kontrolek z zestawu narzędzi AJAX Control Toolkit. Strona musi zawierać formant ScriptManager. Kontrolka ScriptManager jest odpowiedzialna za dołączenie wszystkich wymaganych języków JavaScript, które są wymagane przez kontrolki zestawu narzędzi AJAX Control.

Na przykład karta zestawu narzędzi AJAX Control Toolkit zawiera kontrolkę o nazwie kontrolka edytora. Ten formant Wyświetla bogaty Edytor HTML. Wykonaj następujące kroki, aby dodać kontrolkę edytor do strony:

1. Utwórz nową stronę ASP.NET o nazwie ShowEditor. aspx
2. Wybierz kontrolkę ScriptManager z poziomu karty rozszerzenia AJAX w przyborniku i przeciągnij kontrolkę na stronę.
3. Wybierz kontrolkę Edytor z poniżej karty zestawu narzędzi AJAX Control Toolkit w przyborniku i przeciągnij kontrolkę na stronę (patrz rysunek 1). Projektant powinien wyglądać jak rysunek 2.
4. Uruchom witrynę sieci Web, wybierając opcję menu **Debuguj, Rozpocznij debugowanie** lub naciśnij klawisz F5.
5. Strona powinna zostać wyświetlona na rysunku 3.

[![wybranie kontrolki edytora HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Ilustracja 01**. Wybieranie kontrolki Edytor HTML ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))

[![projektanta programu Visual Studio z kontrolką ScriptManager i edycją](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Ilustracja 02**: projektant programu Visual Studio z kontrolką ScriptManager i edycją ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))

[![strony DisplayEditor. aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Ilustracja 03**: Strona DisplayEditor. aspx ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Używanie rozszerzeń formantów zestawu narzędzi AJAX Control Toolkit

Zestaw narzędzi AJAX Control zawiera również rozszerzone formanty. Jak sugeruje nazwa, rozszerzenie sterujące rozszerza funkcjonalność istniejącej kontrolki. Na przykład rozszerzenie kontrolki confirmbutton Control rozszerza kontrolkę standardowy przycisk ASP.NET. Rozszerzenie zmienia zachowanie kontrolki przycisk s, aby przycisk wyświetlał okno dialogowe potwierdzenia po jego kliknięciu.

Rozszerzenie kontrolki, podobnie jak kontrolka zestawu narzędzi AJAX Control, wymaga kontrolki ScriptManager. Przed rozpoczęciem korzystania z rozszerzeń formantów na stronie należy dodać formant ScriptManager do strony.

Wykonaj następujące kroki, aby użyć rozszerzenia formantu kontrolki confirmbutton:

1. Utwórz nową stronę ASP.NET o nazwie ShowConfirmButton. aspx
2. Dodaj kontrolkę ScriptManager do strony, przeciągając kontrolkę na stronę poniżej karty rozszerzenia AJAX.
3. Dodaj standardowy formant Button do strony, przeciągając przycisk znajdujący się poniżej karty standardowe w przyborniku na powierzchnię projektanta.
4. Kliknij opcję **Dodaj** zadanie rozszerzenia (patrz rysunek 4).
5. W oknie dialogowym Wybierz rozszerzenie wybierz pozycję ConfirmButtonExtender (zobacz rysunek 5), a następnie kliknij przycisk OK.
6. Wybierz kontrolkę przycisk w Projektancie i rozwiń węzeł rozszerzenia, Button1\_ConfirmButtonExtender w okno Właściwości (zobacz rysunek 6). Przypisać wartość *naprawdę?* do właściwości ConfirmText.
7. Uruchom stronę, wybierając opcję menu **Debuguj, Rozpocznij debugowanie** lub naciśnij klawisz F5.

[![opcji Dodaj zadanie rozszerzania](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Ilustracja 04**. opcja Dodaj zadanie rozszerzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))

[![wybranie rozszerzenia formantu kontrolki confirmbutton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Ilustracja 05**: wybór rozszerzenia formantu kontrolki confirmbutton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))

[![ustawienia właściwości kontrolki confirmbutton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Ilustracja 06**. Ustawianie właściwości kontrolki confirmbutton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))

Po otwarciu strony powinien pojawić się przycisk. Po kliknięciu tego przycisku zostanie wyświetlone okno dialogowe potwierdzenia na rysunku 7.

[![wyświetlania okna dialogowego potwierdzenia](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Ilustracja 07**. Wyświetlanie okna dialogowego potwierdzenia ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))

Należy zauważyć, że zwykle nie przeciągniesz rozszerzenia kontrolki na stronę. Zamiast tego należy użyć opcji **Dodaj zadanie rozszerzania** , aby dodać rozszerzenie do kontrolki, która została już dodana do strony. Należy zauważyć, że Ponadto ustawimy właściwości rozszerzenia kontroli, otwierając arkusz właściwości dla rozszerzonej kontrolki.

Pojedyncza kontrolka ASP.NET może być rozszerzona przez wiele rozszerzeń kontrolek. Arkusz właściwości dla rozszerzonej kontrolki wyświetli listę rozszerzeń formantów skojarzonych z kontrolką.

> [!div class="step-by-step"]
> [Poprzednie](get-started-with-the-ajax-control-toolkit-vb.md)
> [dalej](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)

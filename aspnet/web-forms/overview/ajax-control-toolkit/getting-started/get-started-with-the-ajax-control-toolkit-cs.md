---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Wprowadzenie do zestawu narzędzi AJAX Control Toolkit (C#) | Microsoft Docs
author: microsoft
description: Poznaj wszystko, co musisz wiedzieć, aby rozpocząć korzystanie z zestawu narzędzi AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 7478b090ec52778572d70065983de6be8bdb4e6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621605"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a>Wprowadzenie do zestawu narzędzi AJAX Control Toolkit (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> Poznaj wszystko, co musisz wiedzieć, aby rozpocząć korzystanie z zestawu narzędzi AJAX Control Toolkit.

Zestaw narzędzi AJAX Control Toolkit zawiera więcej niż 30 bezpłatnych kontrolek, których można używać w aplikacjach ASP.NET. W tym samouczku dowiesz się, jak pobrać zestaw narzędzi AJAX Control i dodać formanty zestawu narzędzi do przybornika Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Pobieranie zestawu narzędzi AJAX Control Toolkit

[Zestaw narzędzi AJAX Control Toolkit](http://devexpress.com/act) to projekt typu "open source" opracowany przez członków społeczności ASP.NET i zespołu ASP.NET. 

[![pobierania zestawu narzędzi AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Ilustracja 01**. Pobieranie zestawu narzędzi AJAX Control Toolkit ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))

Po pobraniu pliku należy odblokować plik. Kliknij plik prawym przyciskiem myszy, wybierz polecenie Właściwości, a następnie kliknij przycisk **Odblokuj** (patrz rysunek 2).

[![odblokowywanie pliku ZIP zestawu narzędzi AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Ilustracja 02**: Odblokowywanie pliku zip formantu AJAX Control Toolkit ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))

Po odblokowaniu pliku można rozpakować plik: kliknij prawym przyciskiem myszy plik i wybierz opcję **Wyodrębnij wszystkie** menu. Teraz wszystko jest gotowe do dodania zestawu narzędzi do przybornika Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Dodawanie zestawu narzędzi AJAX Control Toolkit do przybornika

Najprostszym sposobem korzystania z zestawu narzędzi AJAX Control Toolkit jest dodanie zestawu narzędzi do przybornika Visual Studio/Visual Web Developer (patrz rysunek 3). Dzięki temu można po prostu przeciągnąć formant zestawu narzędzi na stronę, gdy chcesz go używać.

[![zestaw narzędzi AJAX Control Toolkit pojawia się w przyborniku](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Ilustracja 03**: zestaw narzędzi AJAX Control Toolkit pojawia się w przyborniku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))

Najpierw należy dodać kartę zestawu narzędzi AJAX Control do przybornika. Wykonaj poniższe kroki.

1. Utwórz nową witrynę sieci Web ASP.NET, wybierając plik opcji menu, nową witrynę sieci Web. Kliknij dwukrotnie plik default. aspx w oknie Eksplorator rozwiązań, aby otworzyć go w edytorze.
2. Kliknij prawym przyciskiem myszy Przybornik poniżej karty Ogólne i wybierz opcję menu **Dodaj kartę** (zobacz rysunek 4).
3. Wprowadź nową kartę o nazwie zestaw narzędzi AJAX Control Toolkit.

[![dodawania nowej karty](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Ilustracja 04**: Dodawanie nowej karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))

Następnie należy dodać kontrolki zestawu narzędzi AJAX Control do nowej karty. wykonaj następujące kroki:

- Kliknij prawym przyciskiem myszy pod kartą zestawu narzędzi AJAX Control Toolkit i wybierz opcję menu **Wybierz pozycje (patrz rysunek 5)** .
- Przejdź do lokalizacji, w której został rozpakowany zestaw narzędzi AJAX Control Toolkit, i wybierz zestaw AjaxControlToolkit. dll.

[![wybierz elementy do dodania do przybornika](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Ilustracja 05**. Wybieranie elementów do dodania do przybornika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))

Po wykonaniu tych kroków wszystkie kontrolki zestawu narzędzi będą widoczne w przyborniku.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Uaktualnianie do nowej wersji zestawu narzędzi

Jeśli używasz starszej wersji zestawu narzędzi, a teraz konieczne jest przejście do nowszej wersji, należy wykonać następujące czynności:

- Pliki binarne — Usuń starą wersję zestawu AjaxControlToolkit. dll z folderu bin witryny sieci Web.
- Elementy przybornika — Usuń kartę zestawu narzędzi AJAX Control i postępuj zgodnie z powyższymi krokami, aby ponownie utworzyć kartę przy użyciu nowej wersji zestawu AjaxControlToolkit. dll.

> [!div class="step-by-step"]
> [Dalej](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)

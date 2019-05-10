---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Za pomocą technologii AJAX kontrolki zestawu narzędzi kontrolek i rozszerzeń (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać do stron ASP.NET AJAX Control Toolkit kontrolek i rozszerzeń.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 1acafaadaf373b488b9e85b7ba31f08cd3b53e85
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65127103"
---
# <a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Używanie kontrolek i rozszerzeń kontrolek zestawu narzędzi AJAX Control Toolkit (C#)

przez [firmy Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać do stron ASP.NET AJAX Control Toolkit kontrolek i rozszerzeń.

Zestawu narzędzi AJAX Control Toolkit zawiera zestaw kontrolek i rozszerzeń. W tym samouczku krótki dowiesz się, jak dodać zarówno kontrolek i rozszerzeń do strony ASP.NET.

> [!NOTE] 
> 
> Aby uzyskać instrukcje dotyczące instalowania zestawu narzędzi AJAX Control Toolkit i dodawanie zestawu narzędzi AJAX Control Toolkit do przybornika Visual Studio/Visual Web Developer, zapoznaj się z samouczkiem [wprowadzenie do zestawu narzędzi AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).

## <a name="using-ajax-control-toolkit-controls"></a>Korzystanie z kontrolek zestawu narzędzi kontrolka AJAX

Kontrolka w postaci zestawu narzędzi AJAX Control Toolkit działa tak samo jak normalne formant ASP.NET. Możesz przeciągnąć kontrolki z przybornika na stronie ASP.NET. Można dodać kontrolki do strony w widoku projektu lub źródła.

Istnieje jedno wymaganie dotyczące specjalne, gdy za pomocą kontrolek z zestawu narzędzi AJAX Control Toolkit. Strona musi zawierać formantu ScriptManager. Formantu ScriptManager jest odpowiedzialny za oraz wszystkie niezbędne języka JavaScript, wymagane przez formanty zestawu narzędzi AJAX Control Toolkit.

Na przykład karta zestawu narzędzi AJAX Control Toolkit zawiera kontrolki o nazwie kontrolka edytora. Ta kontrolka Wyświetla Zaawansowany edytor kodu HTML. Wykonaj następujące kroki, aby dodać kontrolkę edytora do strony:

1. Tworzenie nowej strony programu ASP.NET o nazwie ShowEditor.aspx
2. Wybierz formantu ScriptManager from beneath na karcie rozszerzenia AJAX w przyborniku, a następnie przeciągnij go na stronę.
3. Zaznacz formant edytora from beneath kartę zestawu narzędzi AJAX Control Toolkit w przyborniku, a następnie przeciągnij formant na stronie (patrz rysunek 1). Projektant powinien wyglądać jak rysunek 2.
4. Uruchom witrynę sieci web, wybierając opcję menu **debugowania i Rozpocznij debugowanie** lub naciskając klawisz F5.
5. Powinna zostać wyświetlona strona, na rysunku 3.

[![Wybieranie kontrolka edytora HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Rysunek 01**: Wybieranie kontrolka edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))

[![Projektanta Visual Studio za pomocą formantu ScriptManager i edytowanie](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Rysunek 02**: Projektanta Visual Studio za pomocą formantu ScriptManager i Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))

[![Na stronie DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Rysunek 03**: Na stronie DisplayEditor.aspx ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))

## <a name="using-ajax-control-toolkit-control-extenders"></a>Za pomocą rozszerzeń kontrolki zestawu narzędzi AJAX

Zestawu narzędzi AJAX Control Toolkit zawiera także kontrolek. Jak sugeruje nazwa, rozszerzenia kontrolki zestawu narzędzi rozszerza funkcjonalność istniejącej kontrolki. Na przykład rozszerzenie kontrolki ConfirmButton rozszerza standardowe kontrolki przycisku ASP.NET. Urządzenia extender zmienia zachowanie s kontrolki przycisku Tak, aby przycisk powoduje wyświetlenie okna dialogowego potwierdzenia, po jego kliknięciu.

Rozszerzenia kontrolki zestawu narzędzi, podobnie jak kontrolka w postaci zestawu narzędzi AJAX Control Toolkit wymaga formantu ScriptManager. Przed rozpoczęciem korzystania z kontrolek na stronie, należy dodać do strony formantu ScriptManager.

Wykonaj następujące kroki, aby użyć rozszerzenia kontrolki ConfirmButton:

1. Tworzenie nowej strony programu ASP.NET o nazwie ShowConfirmButton.aspx
2. Dodawanie formantu ScriptManager do strony, przeciągając kontrolki na stronie from beneath na karcie rozszerzenia AJAX.
3. Dodaj kontrolkę przycisk standardowy do strony, przeciągnij przycisk from beneath standardowa karta w przyborniku na powierzchnię projektanta.
4. Kliknij przycisk **Dodaj Extender** zadań opcji (zobacz rysunek 4).
5. W oknie dialogowym Wybierz rozszerzenia, wybierz ConfirmButtonExtender (zobacz rysunek 5) i kliknij przycisk OK.
6. Zaznacz formant przycisku w projektancie, a następnie rozwiń węzeł urządzenia Extender, Button1\_ConfirmButtonExtender węzła w oknie dialogowym właściwości (patrz rysunek 6). Przypisz wartość *naprawdę?* właściwości ConfirmText.
7. Uruchom stronę, wybierając opcję menu **debugowania i Rozpocznij debugowanie** lub naciśnij klawisz F5.

[![Dodawanie opcji urządzenia Extender zadania](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Rysunek 04**: Dodawanie opcji urządzenia Extender zadań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))

[![Wybierając rozszerzenie kontrolki ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Rysunek 05**: Wybierając rozszerzenie kontrolki ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))

[![Ustawienie właściwości kontrolki ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Rysunek 06**: Ustawienie właściwości kontrolki ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))

Po otwarciu strony, powinien zostać wyświetlony przycisk. Po kliknięciu przycisku, otrzymasz okno dialogowe potwierdzenia na rysunku 7.

[![Wyświetlanie okna dialogowego potwierdzenia](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Rysunek 07**: Wyświetlanie okna dialogowego potwierdzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))

Należy zauważyć, że zwykle nie przeciągniesz rozszerzenia kontrolki na stronie. Zamiast tego należy użyć **Dodaj Extender** zadań opcję, aby dodać rozszerzenie do kontrolki, które zostało już dodane do strony. Zwróć uwagę, ponadto ustawienie sterowania właściwości rozszerzeń, otwierając arkusz właściwości kontrolki zostanie przedłużony.

Jeden formant ASP.NET można rozszerzyć przez wiele kontrolek. Arkusz właściwości kontrolki zostanie przedłużony spowoduje wyświetlenie listy wszystkich kontrolek skojarzonego z kontrolką.

> [!div class="step-by-step"]
> [Poprzednie](get-started-with-the-ajax-control-toolkit-cs.md)
> [dalej](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)

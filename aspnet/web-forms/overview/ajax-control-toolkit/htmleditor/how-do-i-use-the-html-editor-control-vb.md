---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Jak mogę użyć kontrolki edytora HTML? (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor to ASP.NET AJAX kontrolki, która umożliwia łatwe tworzenie i Edytowanie zawartości HTML za pomocą przycisków na pasku narzędzi.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 20f8a2f8148bc658370ba1a939ebf1b62d376bc0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554153"
---
# <a name="how-do-i-use-the-html-editor-control-vb"></a>Jak mogę użyć kontrolki edytora HTML? (VB)

przez [firmę Microsoft](https://github.com/microsoft)

> HTMLEditor to ASP.NET AJAX kontrolki, która umożliwia łatwe tworzenie i Edytowanie zawartości HTML za pomocą przycisków na pasku narzędzi.

Celem tego samouczka jest zapewnienie omówienia kontrolki edytora HTML dołączonej do zestawu narzędzi AJAX Control Toolkit. Edytor HTML zawiera opcje zmiany rozmiaru czcionki, wybrania czcionki, zmiany koloru tła, modyfikacji koloru pierwszego planu, dodania linków, dodania obrazów, zmiany wyrównania tekstu oraz wykonania operacji wycinania, kopiowania i wklejania (patrz rysunek 1).

[![edytora HTML](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Ilustracja 01**. Edytor HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-vb/_static/image2.png))

Edytor HTML umożliwia wprowadzanie zawartości przy użyciu trybu projektowania lub bezpośrednie wprowadzanie kodu HTML. Dostępne są również opcje wyświetlania podglądu zawartości HTML (patrz rysunek 2).

[![przyciski projektowania, HTML i podglądu](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Ilustracja 02**: projektowanie, HTML i przyciski podglądu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-vb/_static/image4.png))

W tym samouczku dowiesz się, jak wyświetlić Edytor HTML, jak dostosować przyciski paska narzędzi, które pojawiają się w edytorze HTML, oraz jak unikać ataków skryptów między lokacjami.

## <a name="displaying-the-html-editor"></a>Wyświetlanie edytora HTML

Aby można było użyć edytora HTML na stronie ASP.NET, należy najpierw dodać kontrolkę ScriptManager do strony. Kontrolka ScriptManager znajduje się poniżej karty rozszerzenia AJAX w przyborniku Visual Studio/Visual Web Developer Express.

Kontrolka ScriptManager powinna zostać umieszczona w górnej części strony przed wszystkimi innymi kontrolkami na stronie. Można na przykład umieścić go bezpośrednio poniżej otwierającego&gt; &lt;formularza po stronie serwera.

Kontrolka Edytor HTML znajduje się w przyborniku z pozostałą częścią kontrolek zestawu narzędzi AJAX Control. Nosi nazwę kontrolki edytora (patrz rysunek 3).

[![kontrolki edytora HTML](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Ilustracja 03**: kontrolka edytora HTML ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-html-editor-control-vb/_static/image6.png))

Po przeciągnięciu edytora HTML na stronę, można ustawić jej właściwości w arkuszu właściwości. Na przykład zwykle chcesz ustawić właściwości width i height. Lista 1 zawiera źródło strony ASP.NET, która zawiera edytor HTML.

**Lista 1 — SimpleEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

Strona z listą 1 zawiera kontrolkę Edytor HTML, kontrolkę przycisku i kontrolkę literału. Po kliknięciu przycisku zawartość edytora HTML pojawia się w kontrolce literału (zobacz rysunek 4).

[![przesyłania formularza przy użyciu edytora HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Rysunek 04**: Przesyłanie formularza z edytorem HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-vb/_static/image8.png))

Właściwość zawartość edytora HTML służy do pobierania zawartości HTML wprowadzonej do edytora HTML. Należy pamiętać, że ta zawartość HTML może zawierać kod JavaScript. W następnej sekcji omówiono sposób zapobiegania atakom z użyciem kodu JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Dostosowywanie paska narzędzi edytora HTML

Można dokładnie dostosować przyciski wyświetlane w edytorze. Na przykład możesz chcieć usunąć kartę HTML, aby uniemożliwić użytkownikom przełączanie edytora HTML do trybu HTML. Można również usunąć listę rozwijaną rozmiar czcionki, aby uniemożliwić użytkownikom tworzenie zbyt dużej ilości tekstu w ogłoszeniu komunikatu na forum (patrz rysunek 5).

[![niestandardowego edytora HTML](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Ilustracja 05**: dostosowany edytor HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-html-editor-control-vb/_static/image10.png))

Przyciski paska narzędzi można dostosować, wprowadzając nowy edytor HTML z klasy podstawowego edytora. Na przykład edytor niestandardowy w liście 2 zawiera przyciski paska narzędzi dla pogrubienia i kursywy. Wszystkie pozostałe przyciski paska narzędzi zostały usunięte. Ponadto karta HTML została usunięta z dolnej części edytora (ale nadal znajdują się na niej karty projekt i Podgląd).

**Lista 2-App\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Należy dodać klasę z listy 2 do aplikacji\_folderze kodu, tak aby Klasa została skompilowana automatycznie. Jeśli folder kodu\_aplikacji nie istnieje w witrynie sieci Web, można po prostu dodać folder.

Po utworzeniu edytora niestandardowego można dodać go do strony ASP.NET w taki sam sposób, jak w przypadku standardowego edytora HTML (patrz lista 3).

**Lista 3-ShowCustomEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Unikanie ataków na skrypty między lokacjami (XSS)

Za każdym razem, gdy zaakceptujesz dane wejściowe od użytkownika, a następnie wyświetlasz je ponownie w witrynie sieci Web, możesz otworzyć witrynę internetową w celu ataków między lokacjami (XSS). W teorii złośliwy haker może przesłać kod JavaScript, który jest wykonywany podczas ponownego wyświetlania danych wejściowych. Kod JavaScript może służyć do kradzieży haseł użytkowników lub innych poufnych informacji.

Zwykle można pokonać ataki XSS przez kodowanie HTML, niezależnie od tego, jakie dane są pobierane od użytkownika przed wyświetleniem go na stronie sieci Web. Jednak kodowanie HTML w wyniku edytora HTML nie tylko koduje &lt;Tagi&gt; skryptu, a także zakodować wszystkie Tagi HTML. Innymi słowy, wszystkie elementy formatowania, takie jak typ czcionki, rozmiar czcionki i kolor tła, zostaną utracone.

Jeśli zbierasz poufne informacje od użytkowników, takie jak hasła, numery kart kredytowych i numery ubezpieczenia społecznego, nie należy wyświetlać niezakodowanej zawartości pobranej od użytkownika przy użyciu edytora HTML. Należy używać edytora HTML tylko w sytuacjach, w których nie jest ponownie wyświetlana zawartość HTML lub zawartość HTML jest przesyłana do witryny sieci Web przez zaufaną stronę.

Załóżmy na przykład, że tworzysz aplikację w blogu. W takiej sytuacji warto używać edytora HTML podczas redagowania wpisów w blogu. Jesteś jedyną osobą, która przesyła wpis w blogu i, najprawdopodobniej, możesz ufać sobie, aby nie przesyłać złośliwego kodu JavaScript. Jednak nie ma sensu korzystanie z edytora HTML w przypadku umożliwienia anonimowym użytkownikom ogłaszania komentarzy. W sytuacjach, w których użytkownicy przesyłają poufne informacje, takie jak hasła, należy zachować szczególną ostrożność. Potencjalnie złośliwy użytkownik może opublikować komentarz zawierający właściwy kod JavaScript do kradzieży hasła.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono krótkie omówienie kontrolki edytora HTML zawartej w zestawie narzędzi AJAX Control. Dowiesz się, jak za pomocą edytora HTML zaakceptować zawartość rozbudowaną od użytkownika i przesłać zawartość na serwer. Omówiono również, jak można dostosować przyciski paska narzędzi, które są wyświetlane w edytorze HTML. Wreszcie zawarto informacje na temat zapobiegania atakom na skrypty między lokacjami w przypadku używania edytora HTML do akceptowania potencjalnie złośliwych danych wejściowych.

> [!div class="step-by-step"]
> [Wstecz](how-do-i-use-the-html-editor-control-cs.md)

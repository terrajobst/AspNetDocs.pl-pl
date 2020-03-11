---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Tworzenie niestandardowego rozszerzenia kontrolki zestawu narzędzi AJAX ControlC#() | Microsoft Docs
author: microsoft
description: Niestandardowe rozszerzalności umożliwiają dostosowywanie i zwiększanie możliwości formantów ASP.NET bez konieczności tworzenia nowych klas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 7850e745f5985688c95fc7f649ccbb06b2f66e20
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535491"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Tworzenie niestandardowego rozszerzenia kontrolki zestawu narzędzi AJAX Control Toolkit (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> Niestandardowe rozszerzalności umożliwiają dostosowywanie i zwiększanie możliwości formantów ASP.NET bez konieczności tworzenia nowych klas.

W ramach tego samouczka dowiesz się, jak utworzyć niestandardowe rozszerzenie formantu zestawu narzędzi AJAX Control. Tworzymy proste, ale użyteczne, nowe rozszerzenie, które zmienia stan przycisku z wyłączone na włączone po wpisaniu tekstu do pola tekstowego. Po przeczytaniu tego samouczka będzie można zwiększyć zestaw narzędzi ASP.NET AJAX z własnymi kontrolkami kontrolek.

Niestandardowe elementy sterujące można tworzyć przy użyciu programu Visual Studio lub Visual Web Developer (Upewnij się, że masz najnowszą wersję Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Przegląd rozszerzenia DisabledButton

Nasze nowe rozszerzenie kontroli nosi nazwę rozszerzenia DisabledButton. To rozszerzenie będzie miało trzy właściwości:

- TargetControlID — pole tekstowe, które rozszerza formant.
- TargetButtonIID — przycisk, który jest wyłączony lub włączony.
- DisabledText — tekst, który jest początkowo wyświetlany na przycisku. Po rozpoczęciu wpisywania przycisk wyświetla wartość właściwości tekst przycisku.

Można podpiąć rozszerzenie DisabledButton do kontrolki TextBox i Button. Przed wpisaniem dowolnego tekstu przycisk jest wyłączony i pole tekstowe i przycisk wyglądają następująco:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))

Po rozpoczęciu wpisywania tekstu przycisk jest włączony i pole tekstowe i przycisk wyglądają następująco:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))

Aby utworzyć nasze rozszerzenie kontroli, musimy utworzyć następujące trzy pliki:

- DisabledButtonExtender.cs — ten plik jest klasą kontroli po stronie serwera, która będzie zarządzać tworzeniem rozszerzeń i umożliwia ustawianie właściwości w czasie projektowania. Definiuje również właściwości, które można ustawić na urządzeniu Extender. Te właściwości są dostępne za pośrednictwem kodu i w czasie projektowania i są zgodne z właściwościami zdefiniowanymi w pliku DisableButtonBehavior. js.
- DisabledButtonBehavior. js — ten plik polega na tym, że zostanie dodana Cała logika skryptu klienta.
- DisabledButtonDesigner.cs — Ta klasa umożliwia korzystanie z funkcji czasu projektowania. Ta klasa jest potrzebna, jeśli chcesz, aby rozszerzenie sterowania działało prawidłowo w programie Visual Studio/Visual Web Developer Designer.

Dlatego rozszerzenie sterujące składa się z kontrolki po stronie serwera, zachowania po stronie klienta i klasy projektanta po stronie serwera. Dowiesz się, jak utworzyć wszystkie trzy z tych plików w poniższych sekcjach.

## <a name="creating-the-custom-extender-website-and-project"></a>Tworzenie niestandardowej witryny sieci Web rozszerzeń i projektu

Pierwszym krokiem jest utworzenie projektu biblioteki klas i witryny internetowej w programie Visual Studio/Visual Web Developer. Utworzymy rozszerzenie niestandardowe w projekcie biblioteki klas i przetestuj niestandardowe rozszerzenie w witrynie sieci Web.

Zacznij od witryny sieci Web. Wykonaj następujące kroki, aby utworzyć witrynę sieci Web:

1. Wybierz menu plik opcji **, Nowa witryna sieci Web**.
2. Wybierz szablon **witryny sieci Web ASP.NET** .
3. Nazwij nową witrynę sieci Web *Website1*.
4. Kliknij przycisk **OK**.

Następnie musimy utworzyć projekt biblioteki klas, który będzie zawierać kod dla rozszerzenia kontroli:

1. Wybierz plik opcji menu **, Dodaj, nowy projekt**.
2. Wybierz szablon **Biblioteka klas** .
3. Nazwij nową bibliotekę klas o nazwie **CustomExtenders**.
4. Kliknij przycisk **OK**.

Po wykonaniu tych kroków okno Eksplorator rozwiązań powinno wyglądać jak rysunek 1.

[![rozwiązanie przy użyciu witryny sieci Web i projektu biblioteki klas](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Ilustracja 01**: rozwiązanie z witryną sieci Web i projektem biblioteki klas ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))

Następnie należy dodać wszystkie niezbędne odwołania do zestawu do projektu biblioteki klas:

1. Kliknij prawym przyciskiem myszy projekt CustomExtenders i wybierz opcję menu **Dodaj odwołanie**.
2. Wybierz kartę .NET.
3. Dodaj odwołania do następujących zestawów:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Wybierz kartę przeglądanie.
5. Dodaj odwołanie do zestawu AjaxControlToolkit. dll. Ten zestaw znajduje się w folderze, do którego pobrano zestaw narzędzi AJAX Control Toolkit.

Po wykonaniu tych kroków projekt biblioteki klas odwołuje się do folderu powinna wyglądać jak rysunek 2.

[folder odwołań ![z wymaganymi odwołaniami](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Ilustracja 02**: odwołuje się do folderu z wymaganymi odwołaniami ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))

## <a name="creating-the-custom-control-extender"></a>Tworzenie niestandardowego rozszerzenia kontrolki

Teraz, gdy mamy naszą bibliotekę klas, możemy zacząć tworzyć nasze kontrolki rozszerzeń. Niech s zaczyna od zera kości klasy kontrolki niestandardowej (patrz lista 1).

**Lista 1 — MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Istnieje kilka kwestii dotyczących klasy rozszerzania formantów na liście 1. Najpierw należy zauważyć, że klasa dziedziczy z bazowej klasy ExtenderControlBase. Wszystkie kontrolki zestawu narzędzi AJAX Control Toolkit pochodzą z tej klasy bazowej. Na przykład klasa bazowa zawiera właściwość TargetID, która jest wymaganą właściwością każdego rozszerzenia kontroli.

Następnie należy zauważyć, że Klasa zawiera następujące dwa atrybuty powiązane ze skryptem klienta:

- WebResource — powoduje, że plik jest uwzględniany jako zasób osadzony w zestawie.
- ClientScriptResource — powoduje pobranie zasobu skryptu z zestawu.

Atrybut WebResource służy do osadzenia pliku JavaScript MyControlBehavior. js w zestawie podczas kompilowania niestandardowego rozszerzenia. Atrybut ClientScriptResource jest używany do pobierania skryptu MyControlBehavior. js z zestawu, gdy rozszerzenie niestandardowe jest używane na stronie sieci Web.

Aby atrybuty usługi WebResource i ClientScriptResource działały, należy skompilować plik JavaScript jako zasób osadzony. Wybierz plik w oknie Eksplorator rozwiązań, otwórz arkusz właściwości i przypisz wartość *osadzony zasób* do właściwości **Akcja kompilacji** .

Należy zauważyć, że rozszerzenie Control zawiera również atrybut TargetControlType. Ten atrybut służy do określania typu kontrolki rozszerzonej przez rozszerzenie sterujące. W przypadku list 1 rozszerzenie kontrolki służy do rozszerzania pola tekstowego.

Na koniec należy zauważyć, że rozszerzenie niestandardowe zawiera właściwość o nazwie właściwości. Właściwość jest oznaczona za pomocą atrybutu ExtenderControlProperty. Metody GetPropertyValue () i SetPropertyValue () służą do przekazywania wartości właściwości z rozszerzenia kontroli po stronie serwera do zachowania po stronie klienta.

Powróć i zaimplementuj kod dla naszego rozszerzenia DisabledButton. Kod dla tego rozszerzenia można znaleźć na liście 2.

**Lista 2 — DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Rozszerzenie DisabledButton na liście 2 ma dwie właściwości o nazwie TargetButtonID i DisabledText. IDReferenceProperty zastosowana do właściwości TargetButtonID uniemożliwia przypisanie elementów innych niż identyfikator kontrolki Button do tej właściwości.

Atrybuty WebResource i ClientScriptResource kojarzą zachowanie po stronie klienta znajdujące się w pliku o nazwie DisabledButtonBehavior. js z tym urządzeniem Extender. Omawiamy ten plik JavaScript w następnej sekcji.

## <a name="creating-the-custom-extender-behavior"></a>Tworzenie niestandardowego zachowania rozszerzania

Składnik po stronie klienta rozszerzenia kontrolki nazywa się zachowaniem. Rzeczywista logika do wyłączania i włączania przycisku jest zawarta w działaniu DisabledButton. Kod JavaScript dotyczący zachowania znajduje się na liście 3.

**Lista 3-DisabledButton. js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Plik JavaScript w liście 3 zawiera klasę po stronie klienta o nazwie DisabledButtonBehavior. Ta klasa, taka jak stawka po stronie serwera, zawiera dwie właściwości o nazwach TargetButtonID i DisabledText, do których można uzyskać dostęp za pomocą polecenia Get\_TargetButtonID/Set\_TargetButtonID i Get\_DisabledText/Set\_DisabledText.

Metoda Initialize () kojarzy procedurę obsługi zdarzeń KeyUp z elementem docelowym zachowania. Za każdym razem, gdy wpiszesz literę do pola tekstowego skojarzonego z tym zachowaniem, program obsługi KeyUp zostanie wykonany. Procedura obsługi KeyUp włącza lub wyłącza przycisk w zależności od tego, czy pole tekstowe skojarzone z zachowaniem zawiera dowolny tekst.

Należy pamiętać, że plik JavaScript należy skompilować na liście 3 jako zasób osadzony. Wybierz plik w oknie Eksplorator rozwiązań, otwórz arkusz właściwości i przypisz wartość *osadzony zasób* do właściwości **Akcja kompilacji** (zobacz rysunek 3). Ta opcja jest dostępna zarówno w programie Visual Studio, jak i w programie Visual Web Developer.

[![dodawania pliku JavaScript jako zasobu osadzonego](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Ilustracja 03**: Dodawanie pliku JavaScript jako zasobu osadzonego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))

## <a name="creating-the-custom-extender-designer"></a>Tworzenie niestandardowego projektanta rozszerzeń

Istnieje jedna Ostatnia Klasa, którą należy utworzyć, aby ukończyć nasze rozszerzenie. Musimy utworzyć klasę projektanta na liście 4. Ta klasa jest wymagana, aby rozszerzenie działało poprawnie z programem Visual Studio/Visual Web Developer Designer.

**Lista 4 — DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Projektanta można skojarzyć z listą 4 za pomocą rozszerzenia DisabledButton z atrybutem projektanta. Należy zastosować atrybut projektanta do klasy DisabledButtonExtender w następujący sposób:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Korzystanie z rozszerzenia niestandardowego

Teraz, gdy zakończymy tworzenie rozszerzenia kontrolki DisabledButton, jest to czas na jego użycie w naszej witrynie ASP.NET. Najpierw musimy dodać rozszerzenie niestandardowe do przybornika. Wykonaj następujące kroki:

1. Otwórz stronę ASP.NET przez dwukrotne kliknięcie strony w oknie Eksplorator rozwiązań.
2. Kliknij prawym przyciskiem myszy Przybornik i wybierz opcję menu **Wybierz elementy**.
3. W oknie dialogowym Wybierz elementy przybornika przejdź do zestawu CustomExtenders. dll.
4. Kliknij przycisk **OK** , aby zamknąć okno dialogowe.

Po wykonaniu tych kroków rozszerzenie DisabledButton Control powinno pojawić się w przyborniku (zobacz rysunek 4).

[![DisabledButton w przyborniku](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Ilustracja 04**: DisabledButton w przyborniku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))

Następnie musimy utworzyć nową stronę ASP.NET. Wykonaj następujące kroki:

1. Utwórz nową stronę ASP.NET o nazwie ShowDisabledButton. aspx.
2. Przeciągnij element ScriptManager na stronę.
3. Przeciągnij kontrolkę TextBox na stronę.
4. Przeciągnij kontrolkę Button na stronę.
5. W okno Właściwości zmień właściwość ID przycisku na wartość <em>btnSave</em> i właściwość Text na wartość *Zapisz\** .

Utworzyliśmy stronę ze standardowym polem tekstowym ASP.NET i kontrolką przycisku.

Następnie musimy rozciągnąć formant TextBox z DisabledButton Extender:

1. Wybierz opcję **Dodaj** zadanie rozszerzenia, aby otworzyć okno dialogowe kreatora rozszerzeń (patrz rysunek 5). Należy zauważyć, że okno dialogowe zawiera nasze niestandardowe rozszerzenie DisabledButton.
2. Wybierz rozszerzenie DisabledButton, a następnie kliknij przycisk **OK** .

[![okno dialogowe kreatora rozszerzania](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Ilustracja 05**. okno dialogowe kreatora rozszerzeń ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))

Na koniec można ustawić właściwości rozszerzenia DisabledButton. Właściwości rozszerzenia DisabledButton można modyfikować, modyfikując właściwości kontrolki TextBox:

1. Zaznacz pole tekstowe w projektancie.
2. W okno Właściwości rozwiń węzeł rozszerzenia (patrz rysunek 6).
3. Przypisz wartość *Zapisz* do właściwości DisabledText i wartość *BtnSave* do właściwości TargetButtonID.

[![ustawienia właściwości rozszerzenia](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Ilustracja 06**. Ustawianie właściwości rozszerzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))

Po uruchomieniu strony (przez naciśnięcie klawisza F5) kontrolka przycisk jest początkowo wyłączona. Po rozpoczęciu wprowadzania tekstu do pola tekstowego kontrolka przycisk jest włączona (zobacz rysunek 7).

[![rozszerzanie DisabledButton w działaniu](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Ilustracja 07**: rozszerzenie DisabledButton w działaniu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))

## <a name="summary"></a>Podsumowanie

Celem tego samouczka jest wyjaśnienie, jak można rozszerzać zestaw narzędzi AJAX Control z niestandardowymi kontrolkami rozszerzeń. W tym samouczku utworzyliśmy proste rozszerzenie formantu DisabledButton. To rozszerzenie jest implementowane przez utworzenie klasy DisabledButtonExtender, zachowanie języka JavaScript DisabledButtonBehavior i klasy DisabledButtonDesigner. Po utworzeniu rozszerzenia kontrolki niestandardowej należy wykonać podobny zestaw kroków.

> [!div class="step-by-step"]
> [Poprzednie](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [dalej](get-started-with-the-ajax-control-toolkit-vb.md)

---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Za pomocą rozszerzenie kontrolki ColorPicker (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: ColorPicker jest rozszerzeń ASP.NET AJAX, który udostępnia funkcjonalność pobrania kolor po stronie klienta za pomocą interfejsu użytkownika w kontrolce popup. Będzie można dołączyć do dowolnej platformy ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 2fa3804411cb553de242a503f57e247efc990156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074756"
---
<a name="using-the-colorpicker-control-extender-vb"></a><span data-ttu-id="6e9a8-104">Za pomocą rozszerzenie kontrolki ColorPicker (VB)</span><span class="sxs-lookup"><span data-stu-id="6e9a8-104">Using the ColorPicker Control Extender (VB)</span></span>
====================
<span data-ttu-id="6e9a8-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6e9a8-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6e9a8-106">ColorPicker jest rozszerzeń ASP.NET AJAX, który udostępnia funkcjonalność pobrania kolor po stronie klienta za pomocą interfejsu użytkownika w kontrolce popup.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="6e9a8-107">Będzie można dołączyć do dowolnej kontrolki ASP.NET TextBox.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="6e9a8-108">Go.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-108">It.</span></span>


<span data-ttu-id="6e9a8-109">Celem tego samouczka jest wyjaśniają, jak można użyć rozszerzenie kontrolki Toolkit ColorPicker kontrolka AJAX.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="6e9a8-110">Rozszerzenie kontrolki ColorPicker wyświetla menu podręczne okno dialogowe, umożliwiające wybierz kolor.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="6e9a8-111">ColorPicker jest przydatne, gdy chcesz zapewnić intuicyjnego interfejsu użytkownika dla użytkownika, aby wybrać kolor.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="6e9a8-112">Rozszerzanie Formant TextBox z rozszerzenie kontrolki ColorPicker</span><span class="sxs-lookup"><span data-stu-id="6e9a8-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="6e9a8-113">Wyobraź sobie, na przykład chcesz utworzyć witrynę sieci Web, który umożliwia tworzenie dostosowanych wizytówki przez osoby odwiedzające.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="6e9a8-114">Osoby odwiedzające można wprowadzić tekst wizytówki i wybierz kolor.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="6e9a8-115">Strony ASP.NET w ofercie 1 zawiera dwie kontrolki TextBox o nazwie txtCardText i txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="6e9a8-116">Gdy prześlesz formularz, są wyświetlane wybrane wartości (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="6e9a8-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="6e9a8-117">[![Prosty formularz do tworzenia wizytówkę](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6e9a8-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span></span>

<span data-ttu-id="6e9a8-118">**Rysunek 01**: Prosty formularz do tworzenia wizytówki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6e9a8-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image2.png))</span></span>


<span data-ttu-id="6e9a8-119">**Wyświetlanie listy 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="6e9a8-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

<span data-ttu-id="6e9a8-120">Formularz w ofercie 1 działa, ale nie udostępnia doskonałe środowisko użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="6e9a8-121">Użytkownik będzie musiał wpisać koloru w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="6e9a8-122">Jeśli użytkownik chce wyspecjalizowane kolor — na przykład tylko odpowiednie odcień zielony PLA -, a następnie użytkownik musi ustalić kod HTML koloru bez pomocy.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="6e9a8-123">Rozszerzenie kontrolki ColorPicker służy do tworzenia lepszego środowiska użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="6e9a8-124">ColorPicker wyświetla okno dialogowe kolorów, po przeniesieniu fokusu do kontrolki TextBox (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="6e9a8-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="6e9a8-125">[![Rozszerzenie kontrolki ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="6e9a8-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span></span>

<span data-ttu-id="6e9a8-126">**Rysunek 02**: Rozszerzenie kontrolki ColorPicker ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="6e9a8-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image4.png))</span></span>


<span data-ttu-id="6e9a8-127">Należy wykonać dwa kroki, aby rozszerzenie kontrolki ColorPicker za pomocą formularza w ofercie 1:</span><span class="sxs-lookup"><span data-stu-id="6e9a8-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="6e9a8-128">Dodawanie formantu ScriptManager do strony</span><span class="sxs-lookup"><span data-stu-id="6e9a8-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="6e9a8-129">Dodaj rozszerzenie kontrolki ColorPicker do strony</span><span class="sxs-lookup"><span data-stu-id="6e9a8-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="6e9a8-130">Zanim użyjesz ColorPicker, należy dodać Menedżera skryptów do strony.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="6e9a8-131">Dobrym miejscem do dodania funkcja ScriptManager jest bezpośrednio poniżej po stronie serwera otwierania &lt;formularza&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="6e9a8-132">Funkcja ScriptManager można przeciągnąć na stronę z przybornika (funkcja ScriptManager znajduje się na karcie rozszerzenia AJAX).</span><span class="sxs-lookup"><span data-stu-id="6e9a8-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="6e9a8-133">Alternatywnie można wpisać następującego tagu w widoku źródła pod otwierający tag formularza po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="6e9a8-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="6e9a8-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="6e9a8-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="6e9a8-135">Najprostszym sposobem na stronie Dodaj rozszerzenie kontrolki ColorPicker jest w widoku Projekt.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="6e9a8-136">Jeśli wskaźnik myszy nad txtCardColor pola tekstowego, opcja inteligentne zadań jest wyświetlana, zapewniającą można dodać urządzenia extender (zobacz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="6e9a8-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="6e9a8-137">W przypadku wybrania tej opcji, zostanie wyświetlony Kreator urządzenia Extender, (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="6e9a8-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="6e9a8-138">[![Dodawanie urządzenia extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="6e9a8-138">[![Adding an extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span></span>

<span data-ttu-id="6e9a8-139">**Rysunek 03**: Dodawanie urządzenia extender ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6e9a8-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="6e9a8-140">[![Wybieranie rozszerzenia kontrolki zestawu narzędzi za pomocą kreatora rozszerzeń](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="6e9a8-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="6e9a8-141">**Rysunek 04**: Wybieranie rozszerzenia kontrolki zestawu narzędzi za pomocą kreatora rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="6e9a8-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image8.png))</span></span>


<span data-ttu-id="6e9a8-142">Można wybrać rozszerzenie ColorPicker rozszerzenie txtCardColor pola tekstowego przy użyciu rozszerzeń ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="6e9a8-143">Kliknij przycisk OK, aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="6e9a8-144">Po wprowadzeniu tych zmian, źródła dla strony wyglądają jak lista 2.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="6e9a8-145">**Wyświetlanie listy 2 - CreateCard.aspx (z ColorPicker)**</span><span class="sxs-lookup"><span data-stu-id="6e9a8-145">**Listing 2 - CreateCard.aspx (with ColorPicker)**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

<span data-ttu-id="6e9a8-146">Należy zauważyć, że strona zawiera teraz formant ColorPickerExtender, który pojawia się bezpośrednio pod txtCardColor formant pola tekstowego.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="6e9a8-147">Kontrolka ColorPickerExtender rozszerza formant txtCardColor tak, aby wyświetlał okna dialogowego selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="6e9a8-148">Za pomocą przycisku można uruchomić okna dialogowego selektora kolorów</span><span class="sxs-lookup"><span data-stu-id="6e9a8-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="6e9a8-149">Rozszerzenie ColorPicker obsługuje następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="6e9a8-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="6e9a8-150">PopupButtonId — identyfikator przycisk na stronie, która powoduje, że są wyświetlane okno dialogowe próbnika kolorów.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="6e9a8-151">PopupPosition — pozycja, względem formantu docelowego, okna dialogowego selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="6e9a8-152">Możliwe wartości to bezwzględną, Centrum, BottomLeft, BottomRight, TopLeft, TopRight i po lewej stronie (wartość domyślna to BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="6e9a8-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="6e9a8-153">SampleControlId — identyfikator formantu, który wyświetla wybrany kolor.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="6e9a8-154">SelectedColor — kolor początkowy wybranych przez ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="6e9a8-155">Aby dostosować sposób wyświetlania okna dialogowego selektora kolorów i sposób wyświetlania kolorów, można użyć tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="6e9a8-156">Na stronie w ofercie 3 ilustruje, jak skorzystać z kilku z tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="6e9a8-157">**Wyświetlanie listy 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="6e9a8-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

<span data-ttu-id="6e9a8-158">Na stronie w ofercie 3 obejmuje wybierz kolor przycisku (zobacz rysunek 5).</span><span class="sxs-lookup"><span data-stu-id="6e9a8-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="6e9a8-159">Po kliknięciu tego przycisku, powyżej pola tekstowego pojawi się okno dialogowe selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="6e9a8-160">Jeśli wybierzesz kolor z poziomu okna dialogowego wybrany kolor pojawia się jako kolor tła lblSample formant etykiety.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="6e9a8-161">Właściwość ColorPicker PopupButtonID jest używana do kojarzenia przycisk wyboru koloru z rozszerzeń ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="6e9a8-162">Podczas podawania wartości dla właściwości PopupButtonID okna dialogowego selektora kolorów nie jest już wyświetlany formantu docelowego po ustawieniu fokusu.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="6e9a8-163">Należy kliknąć przycisk aby wyświetlić okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="6e9a8-164">Właściwość SampleControlID jest używana do kojarzenia kontrolkę wyświetlającą wybranego koloru z ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="6e9a8-165">ColorPicker zmienia kolor tła tego formantu do aktualnie wybranego koloru.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="6e9a8-166">[![Wyświetlanie okna dialogowego selektora kolorów przy użyciu przycisku](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="6e9a8-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span></span>

<span data-ttu-id="6e9a8-167">**Rysunek 05**: Wyświetlanie okna dialogowego selektora kolorów przy użyciu przycisku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-colorpicker-control-extender-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="6e9a8-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="6e9a8-168">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="6e9a8-168">Summary</span></span>

<span data-ttu-id="6e9a8-169">W tym samouczku przedstawiono sposób użyć rozszerzenie kontrolki ColorPicker, aby wyświetlić okno podręczne okno dialogowe selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="6e9a8-170">Najpierw zbadaliśmy się, jak można wyświetlić okno dialogowe, gdy fokus zostanie przeniesiony do kontrolki TextBox.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="6e9a8-171">Następnie pokazaliśmy ci, jak utworzyć przycisk, który wyświetla okno dialogowe selektora kolorów, po kliknięciu przycisku.</span><span class="sxs-lookup"><span data-stu-id="6e9a8-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6e9a8-172">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="6e9a8-172">Previous</span></span>](using-the-colorpicker-control-extender-cs.md)

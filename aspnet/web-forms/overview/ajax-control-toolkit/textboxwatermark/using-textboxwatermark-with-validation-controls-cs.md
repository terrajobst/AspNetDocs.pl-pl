---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Używanie używanie textboxwatermark z kontrolkami walidacji (C#) | Microsoft Docs
author: wenz
description: Kontrolka używanie textboxwatermark w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu. Gdy użytkownik kliknie w polu i...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74610873"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="817aa-104">Używanie kontrolki TextBoxWatermark z kontrolkami walidacji (C#)</span><span class="sxs-lookup"><span data-stu-id="817aa-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>

<span data-ttu-id="817aa-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="817aa-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="817aa-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="817aa-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="817aa-107">Kontrolka używanie textboxwatermark w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu.</span><span class="sxs-lookup"><span data-stu-id="817aa-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="817aa-108">Gdy użytkownik kliknie pole, zostanie opróżnione.</span><span class="sxs-lookup"><span data-stu-id="817aa-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="817aa-109">Jeśli użytkownik opuści pole bez wprowadzania tekstu, zostanie wyświetlony tekst wstępnie wypełniony.</span><span class="sxs-lookup"><span data-stu-id="817aa-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="817aa-110">Może to spowodować konflikt z kontrolkami sprawdzania poprawności ASP.NET na tej samej stronie, ale te problemy mogą zostać przezwyciężyne.</span><span class="sxs-lookup"><span data-stu-id="817aa-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="817aa-111">Omówienie</span><span class="sxs-lookup"><span data-stu-id="817aa-111">Overview</span></span>

<span data-ttu-id="817aa-112">Kontrolka `TextBoxWatermark` w zestawie narzędzi AJAX Control rozszerza pole tekstowe tak, aby tekst był wyświetlany w polu.</span><span class="sxs-lookup"><span data-stu-id="817aa-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="817aa-113">Gdy użytkownik kliknie pole, zostanie opróżnione.</span><span class="sxs-lookup"><span data-stu-id="817aa-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="817aa-114">Jeśli użytkownik opuści pole bez wprowadzania tekstu, zostanie wyświetlony tekst wstępnie wypełniony.</span><span class="sxs-lookup"><span data-stu-id="817aa-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="817aa-115">Może to spowodować konflikt z kontrolkami sprawdzania poprawności ASP.NET na tej samej stronie, ale te problemy mogą zostać przezwyciężyne.</span><span class="sxs-lookup"><span data-stu-id="817aa-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="817aa-116">Kroki</span><span class="sxs-lookup"><span data-stu-id="817aa-116">Steps</span></span>

<span data-ttu-id="817aa-117">Podstawowa konfiguracja przykładu jest następująca: formant `TextBox` jest używany jako znak wodny przy użyciu kontrolki `TextBoxWatermarkExtender`.</span><span class="sxs-lookup"><span data-stu-id="817aa-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="817aa-118">Przycisk wyzwala ogłaszanie zwrotne i będzie później używany do wyzwalania kontrolek weryfikacji na stronie.</span><span class="sxs-lookup"><span data-stu-id="817aa-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="817aa-119">Ponadto kontrolka `ScriptManager` jest wymagana do zainicjowania ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="817aa-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="817aa-120">Teraz Dodaj kontrolkę `RequiredFieldValidator`, która sprawdza, czy w polu występuje tekst w czasie przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="817aa-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="817aa-121">Właściwość `InitialValue` modułu walidacji musi być ustawiona na taką samą wartość, która jest używana w kontrolce `TextBoxWatermarkExtender`: gdy formularz zostanie przesłany, wartość niezmienionego pola tekstowego jest wartością limitu dolnego:</span><span class="sxs-lookup"><span data-stu-id="817aa-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="817aa-122">Istnieje jednak jeden problem z tym podejściem: Jeśli klient wyłączy kod JavaScript, pole tekstowe nie jest wypełniane tekstem znaku wodnego, w związku z czym `RequiredFieldValidator` nie wyzwala komunikatu o błędzie.</span><span class="sxs-lookup"><span data-stu-id="817aa-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="817aa-123">W związku z tym, wymagana jest druga kontrolka `RequiredFieldValidator`, która sprawdza puste pole tekstowe (Pomijanie atrybutu `InitialValue`).</span><span class="sxs-lookup"><span data-stu-id="817aa-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="817aa-124">Ponieważ zarówno moduł sprawdzania poprawności używa `Display`=`"Dynamic"`, użytkownik końcowy nie może odróżnić od wyglądu wizualizacji, który został wyzwolony przez dwa moduły walidacji; Zamiast tego wygląda na to, że wystąpiły tylko jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="817aa-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="817aa-125">Na koniec Dodaj kod po stronie serwera, aby wyprowadzić tekst w polu, jeśli żaden moduł sprawdzania poprawności nie wygenerował komunikatu o błędzie:</span><span class="sxs-lookup"><span data-stu-id="817aa-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="817aa-126">[![moduł sprawdzania poprawności zgłasza, że w polu nie ma tekstu](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="817aa-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="817aa-127">Moduł sprawdzania poprawności zgłasza, że w polu nie ma tekstu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="817aa-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="817aa-128">[Poprzednie](using-textboxwatermark-in-a-formview-cs.md)
> [dalej](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="817aa-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>

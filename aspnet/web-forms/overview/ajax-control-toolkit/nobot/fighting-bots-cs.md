---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Walka botów (C#) | Microsoft Docs
author: wenz
description: Automatyczne botów tynków Weblogs i innych witryn internetowych z spamem, przesyłanie formularzy komentarzy bez interakcji z użytkownikiem. Kontrolka NoBot w ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: fef55edf12a024e4dd66e2a18ea371ab4dac861f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606447"
---
# <a name="fighting-bots-c"></a><span data-ttu-id="5098e-104">Zwalczanie botów (C#)</span><span class="sxs-lookup"><span data-stu-id="5098e-104">Fighting Bots (C#)</span></span>

<span data-ttu-id="5098e-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5098e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5098e-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5098e-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="5098e-107">Automatyczne botów tynków Weblogs i innych witryn internetowych z spamem, przesyłanie formularzy komentarzy bez interakcji z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="5098e-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="5098e-108">Formant NoBot w narzędziu ASP.NET AJAX Control Toolkit może pomóc w walce z tymi botówami.</span><span class="sxs-lookup"><span data-stu-id="5098e-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="overview"></a><span data-ttu-id="5098e-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5098e-109">Overview</span></span>

<span data-ttu-id="5098e-110">Automatyczne botów tynków Weblogs i innych witryn internetowych z spamem, przesyłanie formularzy komentarzy bez interakcji z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="5098e-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="5098e-111">Formant NoBot w narzędziu ASP.NET AJAX Control Toolkit może pomóc w walce z tymi botówami.</span><span class="sxs-lookup"><span data-stu-id="5098e-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="5098e-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="5098e-112">Steps</span></span>

<span data-ttu-id="5098e-113">Typowym podejściem do pokonania botów jest korzystanie z CAPTCHAs całkowicie zautomatyzowanego, publicznego testu Turing do informowania komputerów i ludzi.</span><span class="sxs-lookup"><span data-stu-id="5098e-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="5098e-114">Test Turing był pierwotnie testem, w którym ktoś musiał zdecydować, czy partner komunikacji jest człowiekiem, czy komputerem.</span><span class="sxs-lookup"><span data-stu-id="5098e-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="5098e-115">W sieci Web CAPTCHA zwykle składa się z obrazu z niezakłóconymi literami.</span><span class="sxs-lookup"><span data-stu-id="5098e-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="5098e-116">Pomysłem jest, że tylko człowiek może odczytać litery z obrazu, a algorytmy OCR nie powiodą się.</span><span class="sxs-lookup"><span data-stu-id="5098e-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="5098e-117">Istnieje kilka zalet i wad tego podejścia, ale dyskusje na ten temat wykraczają poza zakres tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5098e-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="5098e-118">Istnieje jednak kontrola w narzędziu ASP.NET AJAX Control Toolkit, która zapewnia podobne podejście: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="5098e-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="5098e-119">Jest to prostsze w porównaniu z CAPTCHA, ale jest bardzo łatwe w użyciu i jest bardzo proste w odniesieniu do witryn sieci Web, takich jak Blogi, które są uważane za sukces w przypadku, gdy większość prób spamu zostanie poddana kontroli `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="5098e-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="5098e-120">`NoBot` przechwytuje ogłaszanie zwrotne bieżącego formularza sieci Web ASP.NET w przypadku spełnienia co najmniej jednego z następujących warunków:</span><span class="sxs-lookup"><span data-stu-id="5098e-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="5098e-121">Przeglądarka nie może usunąć układanki JavaScript (na przykład w przypadku dezaktywowania kodu JavaScript)</span><span class="sxs-lookup"><span data-stu-id="5098e-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="5098e-122">Użytkownik przesłał formularz do programu Fast</span><span class="sxs-lookup"><span data-stu-id="5098e-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="5098e-123">Adres IP klienta przesłał formularz zbyt często w określonym czasie.</span><span class="sxs-lookup"><span data-stu-id="5098e-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="5098e-124">Aby można było sprawdzić te warunki, formant `NoBot` wymaga tych atrybutów (wszystkie opcjonalne):</span><span class="sxs-lookup"><span data-stu-id="5098e-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="5098e-125">`ResponseMinimumDelaySeconds` minimalną liczbę sekund między ogłaszaniem zwrotnym</span><span class="sxs-lookup"><span data-stu-id="5098e-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="5098e-126">`CutoffWindowSeconds` długość przedziału czasu, w którym zwroty od jednego adresu IP są mierzone</span><span class="sxs-lookup"><span data-stu-id="5098e-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="5098e-127">`CutoffMaximumInstances` maksymalną liczbę sekund na przedział czasu</span><span class="sxs-lookup"><span data-stu-id="5098e-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="5098e-128">Poniższe znaczniki zależą od tego, że co najmniej dwa sekundy upływa między ogłaszaniem zwrotnym i że w interwale 30-sekundowym występuje tylko pięć zwrotów.</span><span class="sxs-lookup"><span data-stu-id="5098e-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="5098e-129">Następnie w zwykły sposób Pamiętaj o uwzględnieniu `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:</span><span class="sxs-lookup"><span data-stu-id="5098e-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="5098e-130">Ponieważ większość kontroli `NoBot` odbywa się po stronie serwera, należy sprawdzić wynik tych walidacji.</span><span class="sxs-lookup"><span data-stu-id="5098e-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="5098e-131">Można to zrobić, wywołując metodę `IsValid()` `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="5098e-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="5098e-132">Ma jeden argument (jako `out` parametr/`ByRef`, który jest typu `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="5098e-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="5098e-133">Reprezentacja ciągu zawiera przyczynę niepowodzenia sprawdzania i `Valid` w przeciwnym razie.</span><span class="sxs-lookup"><span data-stu-id="5098e-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="5098e-134">Poniższy kod generuje komunikat zgodnie z wynikami `NoBot`:</span><span class="sxs-lookup"><span data-stu-id="5098e-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="5098e-135">Na koniec potrzebna jest forma przesyłania i elementu etykiety, który będzie wyprowadzać komunikat, a wszystko jest gotowe!</span><span class="sxs-lookup"><span data-stu-id="5098e-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="5098e-136">Po uruchomieniu tego skryptu i zdezaktywowaniu kodu JavaScript lub przesłaniu formularza w ciągu pierwszych dwóch sekund lub przesłaniu formularza siedem razy w ciągu 30 sekund zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="5098e-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="5098e-137">Należy jednak użyć tej kontrolki, ponieważ tylko od 90-95% użytkowników ma aktywowany kod JavaScript, w związku z czym 5-10% użytkowników zakończy się niepowodzeniem `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="5098e-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>

<span data-ttu-id="5098e-138">[![ten komunikat o błędzie mógł zostać spowodowany przez bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5098e-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="5098e-139">Ten komunikat o błędzie może być spowodowany przez bot ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5098e-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5098e-140">Next</span><span class="sxs-lookup"><span data-stu-id="5098e-140">Next</span></span>](fighting-bots-vb.md)

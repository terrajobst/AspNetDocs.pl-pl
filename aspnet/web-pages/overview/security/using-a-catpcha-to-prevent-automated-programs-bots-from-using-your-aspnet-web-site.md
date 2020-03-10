---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Korzystanie z CAPTCHA w celu uniemożliwienia botów korzystania z witryny ASP.NET Web Razor) | Microsoft Docs
author: microsoft
description: W tym artykule wyjaśniono, jak używać ReCaptcha (miary zabezpieczeń), aby zapobiec wykonywaniu zadań przez automatyczne programy (botów) na stronach sieci Web ASP.NET (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547048"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="13529-103">Korzystanie z CAPTCHA w celu uniemożliwienia botów z używania Twojej witryny ASP.NET Web Razor)</span><span class="sxs-lookup"><span data-stu-id="13529-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>

<span data-ttu-id="13529-104">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="13529-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="13529-105">W tym artykule wyjaśniono, jak używać ReCaptcha (miary zabezpieczeń) w celu zapobiegania wykonywaniu zadań przez automatyczne programy (botów) w witrynie sieci Web ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="13529-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="13529-106">**Dowiesz się:**</span><span class="sxs-lookup"><span data-stu-id="13529-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="13529-107">Jak dodać test CAPTCHA do lokacji.</span><span class="sxs-lookup"><span data-stu-id="13529-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="13529-108">Są to funkcje ASP.NET wprowadzone w artykule:</span><span class="sxs-lookup"><span data-stu-id="13529-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="13529-109">Pomocnik `ReCaptcha`.</span><span class="sxs-lookup"><span data-stu-id="13529-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="13529-110">Informacje przedstawione w tym artykule dotyczą ASP.NET Web Pages 1,0 i Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="13529-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>

## <a name="about-captchas"></a><span data-ttu-id="13529-111">Informacje o CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="13529-111">About CAPTCHAs</span></span>

<span data-ttu-id="13529-112">Za każdym razem, gdy zezwolisz użytkownikom na rejestrację w swojej witrynie, a nawet po prostu wprowadź nazwę i adres URL (na przykład komentarz bloga), możesz zapełnić fałszywe nazwy.</span><span class="sxs-lookup"><span data-stu-id="13529-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="13529-113">Są one często pozostawiane przez zautomatyzowane programy (botów), które próbują opuścić adresy URL w każdej z witryn sieci Web, które mogą znaleźć.</span><span class="sxs-lookup"><span data-stu-id="13529-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="13529-114">(Częstą motywacją jest opublikowanie adresów URL produktów do sprzedaży).</span><span class="sxs-lookup"><span data-stu-id="13529-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="13529-115">Aby upewnić się, że użytkownik jest osobą rzeczywistą, a nie programem komputerowym, za pomocą *CAPTCHA* można sprawdzić poprawność użytkowników podczas rejestrowania lub wprowadzania ich nazwy i witryny.</span><span class="sxs-lookup"><span data-stu-id="13529-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="13529-116">CAPTCHA oznacza całkowicie zautomatyzowany, publiczny test Turing do informowania komputerów i ludzi.</span><span class="sxs-lookup"><span data-stu-id="13529-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="13529-117">CAPTCHA jest testem *wyzwania-odpowiedzi* , w którym użytkownik jest proszony o to, że jest to bardzo proste dla osoby, które nie jest trudne do wykonania przez zautomatyzowany program.</span><span class="sxs-lookup"><span data-stu-id="13529-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="13529-118">Najczęściej spotykanym typem CAPTCHA jest to, gdzie widzisz pewne zniekształcane litery i prosisz o ich wpisywanie.</span><span class="sxs-lookup"><span data-stu-id="13529-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="13529-119">(Zniekształcenie powinny być trudne do odszyfrowania liter przez botów).</span><span class="sxs-lookup"><span data-stu-id="13529-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="13529-120">Dodawanie testu ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="13529-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="13529-121">Na stronach ASP.NET można użyć pomocnika `ReCaptcha`, aby renderować test CAPTCHA, który jest oparty na usłudze ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="13529-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="13529-122">Pomocnik `ReCaptcha` wyświetla obraz dwóch zniekształconych wyrazów, które użytkownicy muszą wprowadzić poprawnie przed zweryfikowaniem strony.</span><span class="sxs-lookup"><span data-stu-id="13529-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="13529-123">Odpowiedź użytkownika jest weryfikowana przez usługę ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="13529-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="13529-124">Zarejestruj swoją witrynę sieci Web pod adresem ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="13529-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="13529-125">Po zakończeniu rejestracji otrzymasz klucz publiczny i klucz prywatny.</span><span class="sxs-lookup"><span data-stu-id="13529-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="13529-126">Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).</span><span class="sxs-lookup"><span data-stu-id="13529-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="13529-127">Jeśli nie masz jeszcze pliku *\_AppStart. cshtml* , w folderze głównym witryny sieci Web Utwórz plik o nazwie *\_AppStart. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="13529-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="13529-128">Dodaj następujące `Recaptcha` ustawienia pomocnika w pliku *\_AppStart. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="13529-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="13529-129">Ustaw `PublicKey` i `PrivateKey` właściwości przy użyciu własnych kluczy publicznych i prywatnych.</span><span class="sxs-lookup"><span data-stu-id="13529-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="13529-130">Zapisz plik *\_AppStart. cshtml* i zamknij go.</span><span class="sxs-lookup"><span data-stu-id="13529-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="13529-131">W folderze głównym witryny sieci Web Utwórz nową stronę o nazwie *reCAPTCHA. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="13529-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="13529-132">Zastąp istniejącą zawartość następującym:</span><span class="sxs-lookup"><span data-stu-id="13529-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="13529-133">Uruchom stronę *reCAPTCHA. cshtml* w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="13529-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="13529-134">Jeśli wartość `PrivateKey` jest prawidłowa, na stronie zostanie wyświetlona kontrolka ReCaptcha i przycisk.</span><span class="sxs-lookup"><span data-stu-id="13529-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="13529-135">Jeśli klucz nie został ustawiony globalnie w *\_AppStart. html*, na stronie zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="13529-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="13529-136">Wprowadź słowa dla testu.</span><span class="sxs-lookup"><span data-stu-id="13529-136">Enter the words for the test.</span></span> <span data-ttu-id="13529-137">Jeśli przejdziesz test ReCaptcha, zobaczysz komunikat z tym efektem.</span><span class="sxs-lookup"><span data-stu-id="13529-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="13529-138">W przeciwnym razie zostanie wyświetlony komunikat o błędzie, a kontrolka ReCaptcha zostanie wyświetlona ponownie.</span><span class="sxs-lookup"><span data-stu-id="13529-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="13529-139">Jeśli komputer znajduje się w domenie, która używa serwera proxy, może być konieczne skonfigurowanie elementu `defaultproxy` pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="13529-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="13529-140">W poniższym przykładzie przedstawiono plik *Web. config* z elementem `defaultproxy` skonfigurowanym tak, aby umożliwić działanie usługi reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="13529-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="13529-141">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="13529-141">Additional Resources</span></span>

- [<span data-ttu-id="13529-142">Dostosowywanie zachowania całej witryny dla witryn ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="13529-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="13529-143">Witryna ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="13529-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)

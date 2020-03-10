---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testowanie siły hasła (VB) | Microsoft Docs
author: wenz
description: Hasła są wymagane niemal wszędzie, dzięki czemu użytkownicy z opóźnieniem mogą wybrać proste hasła, które są łatwe do przerwania. Kontrolka PasswordStrength w ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: b614e1788eeafc175dd792ec6d3e4619f9ea2b7a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627303"
---
# <a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="8f5cc-104">Testowanie siły hasła (VB)</span><span class="sxs-lookup"><span data-stu-id="8f5cc-104">Testing the Strength of a Password (VB)</span></span>

<span data-ttu-id="8f5cc-105">Autor [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8f5cc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8f5cc-106">[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8f5cc-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="8f5cc-107">Hasła są wymagane niemal wszędzie, dzięki czemu użytkownicy z opóźnieniem mogą wybrać proste hasła, które są łatwe do przerwania.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="8f5cc-108">Formant PasswordStrength w narzędziu ASP.NET AJAX Control Toolkit może sprawdzić, jak dobre jest hasło.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="overview"></a><span data-ttu-id="8f5cc-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8f5cc-109">Overview</span></span>

<span data-ttu-id="8f5cc-110">Hasła są wymagane niemal wszędzie, dzięki czemu użytkownicy z opóźnieniem mogą wybrać proste hasła, które są łatwe do przerwania.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="8f5cc-111">Formant `PasswordStrength` w narzędziu ASP.NET AJAX Control Toolkit może sprawdzić, jak dobre jest hasło.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="8f5cc-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="8f5cc-112">Steps</span></span>

<span data-ttu-id="8f5cc-113">Formant `PasswordStrength` rozszerza pole tekstowe i sprawdza, czy hasło jest wystarczające.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="8f5cc-114">Oferuje wiele opcji za pośrednictwem atrybutów; Oto kilka z nich:</span><span class="sxs-lookup"><span data-stu-id="8f5cc-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="8f5cc-115">`MinimumNumericCharacters` minimalna liczba znaków numerycznych wymaganych w haśle</span><span class="sxs-lookup"><span data-stu-id="8f5cc-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="8f5cc-116">`MinimumSymbolCharacters` minimalna liczba znaków symbolicznych (nie liter i cyfr) wymaganych w haśle</span><span class="sxs-lookup"><span data-stu-id="8f5cc-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="8f5cc-117">`PreferredPasswordLength` minimalnej długości hasła</span><span class="sxs-lookup"><span data-stu-id="8f5cc-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="8f5cc-118">`RequiresUpperAndLowerCaseCharacters`, czy hasło musi zawierać wielkie i małe litery</span><span class="sxs-lookup"><span data-stu-id="8f5cc-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="8f5cc-119">`StrengthIndicatorType` zawiera informacje na temat sposobu prezentowania siły hasła jako tekstu (`"Text"`wartości) lub jako rodzaju paska postępu (wartości `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="8f5cc-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="8f5cc-120">W atrybucie `DisplayPosition` można skonfigurować miejsce wyświetlania informacji.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="8f5cc-121">Oto kompletny przykład, w tym kontrolka ASP.NET AJAX `ScriptManager`, kontrolka `PasswordStrength` i kurs pola tekstowego, w którym użytkownik może wprowadzić hasło.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="8f5cc-122">Na potrzeby demonstracji pole to jest zwykłym polem tekstowym, a nie polem hasła, dzięki czemu możesz zobaczyć, co wpisujesz.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="8f5cc-123">Uruchom stronę i wpisz: tylko po wprowadzeniu małych liter, wielkich liter, cyfr i symboli, hasło jest uznawane za niemożliwe do oddzielenia.</span><span class="sxs-lookup"><span data-stu-id="8f5cc-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>

<span data-ttu-id="8f5cc-124">[![teraz hasło jest (całkiem) dobre](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f5cc-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="8f5cc-125">Teraz hasło jest (całkiem) dobre ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8f5cc-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8f5cc-126">Wstecz</span><span class="sxs-lookup"><span data-stu-id="8f5cc-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)

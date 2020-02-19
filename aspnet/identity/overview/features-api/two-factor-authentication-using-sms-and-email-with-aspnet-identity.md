---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail przy użyciu ASP.NET Identity-ASP.NET 4. x
author: HaoK
description: W tym samouczku przedstawiono sposób konfigurowania uwierzytelniania dwuskładnikowego (funkcji 2FA) przy użyciu wiadomości SMS i wiadomości e-mail. Ten artykuł został zapisany przez Rick Anderson (@RickAndMSFT), pr...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 527b4392846e60dae0b216fdeabf21fd6618e4d7
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456741"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="3d62b-104">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail przy użyciu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="3d62b-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>

<span data-ttu-id="3d62b-105">przez [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="3d62b-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="3d62b-106">W tym samouczku przedstawiono sposób konfigurowania uwierzytelniania dwuskładnikowego (funkcji 2FA) przy użyciu wiadomości SMS i wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="3d62b-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="3d62b-107">Ten artykuł został zapisany przez Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="3d62b-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="3d62b-108">Próbka NuGet została zapisywana głównie przez Hao Kung.</span><span class="sxs-lookup"><span data-stu-id="3d62b-108">The NuGet sample was written primarily by Hao Kung.</span></span>

<span data-ttu-id="3d62b-109">W tym temacie omówiono następujące zagadnienia:</span><span class="sxs-lookup"><span data-stu-id="3d62b-109">This topic covers the following:</span></span>

- [<span data-ttu-id="3d62b-110">Kompilowanie przykładu tożsamości</span><span class="sxs-lookup"><span data-stu-id="3d62b-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="3d62b-111">Konfigurowanie usługi SMS na potrzeby uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3d62b-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="3d62b-112">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3d62b-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="3d62b-113">Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3d62b-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="3d62b-114">Łączenie kont logowania społecznościowych i lokalnych</span><span class="sxs-lookup"><span data-stu-id="3d62b-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="3d62b-115">Blokada konta przed atakami polegającymi na rozsile</span><span class="sxs-lookup"><span data-stu-id="3d62b-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="3d62b-116">Kompilowanie przykładu tożsamości</span><span class="sxs-lookup"><span data-stu-id="3d62b-116">Building the Identity sample</span></span>

<span data-ttu-id="3d62b-117">W tej sekcji użyjesz narzędzia NuGet do pobrania przykładu, z którym będziemy współpracować.</span><span class="sxs-lookup"><span data-stu-id="3d62b-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="3d62b-118">Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="3d62b-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="3d62b-119">Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="3d62b-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="3d62b-120">Ostrzeżenie: aby ukończyć ten samouczek, musisz zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) .</span><span class="sxs-lookup"><span data-stu-id="3d62b-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>

1. <span data-ttu-id="3d62b-121">Utwórz nowy ***pusty*** projekt sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3d62b-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="3d62b-122">W konsoli Menedżera pakietów wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="3d62b-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="3d62b-123">W tym samouczku będziemy używać [SendGrid](http://sendgrid.com/) do wysyłania wiadomości E-mail i [Twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) na potrzeby tekstu SMS.</span><span class="sxs-lookup"><span data-stu-id="3d62b-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="3d62b-124">Pakiet `Identity.Samples` instaluje kod, z którym będziemy pracować.</span><span class="sxs-lookup"><span data-stu-id="3d62b-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="3d62b-125">Ustaw [dla projektu użycie protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="3d62b-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="3d62b-126">*Opcjonalnie*: Postępuj zgodnie z instrukcjami w [samouczku potwierdzającym pocztą e-mail](account-confirmation-and-password-recovery-with-aspnet-identity.md) , aby podłączyć program SendGrid, a następnie uruchomić aplikację i zarejestrować konto e-mail.</span><span class="sxs-lookup"><span data-stu-id="3d62b-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="3d62b-127">*Opcjonalne:* Usuń przykładowy kod potwierdzania linku do wiadomości e-mail z przykładu (kod `ViewBag.Link` na kontrolerze konta.</span><span class="sxs-lookup"><span data-stu-id="3d62b-127">*Optional:* Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="3d62b-128">Zobacz metody akcji `DisplayEmail` i `ForgotPasswordConfirmation` i widoki Razor).</span><span class="sxs-lookup"><span data-stu-id="3d62b-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="3d62b-129">*Opcjonalne:* Usuń kod `ViewBag.Status` z kontrolerów zarządzania i konta oraz z widoków *Views\Account\VerifyCode.cshtml* i *Views\Manage\VerifyPhoneNumber.cshtml* Razor.</span><span class="sxs-lookup"><span data-stu-id="3d62b-129">*Optional:* Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml* and *Views\Manage\VerifyPhoneNumber.cshtml* razor views.</span></span> <span data-ttu-id="3d62b-130">Możesz również zachować `ViewBag.Status` wyświetlania, aby sprawdzić, jak działa ta aplikacja lokalnie bez konieczności podłączania i wysyłania wiadomości e-mail i SMS.</span><span class="sxs-lookup"><span data-stu-id="3d62b-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="3d62b-131">Ostrzeżenie: Jeśli zmienisz dowolne z ustawień zabezpieczeń w tym przykładzie, aplikacje produkcji będą musiały zostać poddane inspekcji zabezpieczeń, która jawnie wywoła wprowadzone zmiany.</span><span class="sxs-lookup"><span data-stu-id="3d62b-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>

<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="3d62b-132">Konfigurowanie usługi SMS na potrzeby uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3d62b-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="3d62b-133">Ten samouczek zawiera instrukcje dotyczące korzystania z programu Twilio lub ASPSMS, ale można użyć dowolnego innego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3d62b-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="3d62b-134">**Tworzenie konta użytkownika za pomocą dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="3d62b-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="3d62b-135">Utwórz konto [Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .</span><span class="sxs-lookup"><span data-stu-id="3d62b-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="3d62b-136">**Instalowanie dodatkowych pakietów lub Dodawanie odwołań do usług**</span><span class="sxs-lookup"><span data-stu-id="3d62b-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="3d62b-137">Twilio</span><span class="sxs-lookup"><span data-stu-id="3d62b-137">Twilio:</span></span>  
   <span data-ttu-id="3d62b-138">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3d62b-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="3d62b-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3d62b-139">ASPSMS:</span></span>  
   <span data-ttu-id="3d62b-140">Należy dodać następujące odwołanie do usługi:</span><span class="sxs-lookup"><span data-stu-id="3d62b-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="3d62b-141">Ulica</span><span class="sxs-lookup"><span data-stu-id="3d62b-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="3d62b-142">Przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="3d62b-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="3d62b-143">**Rozprowadzenie poświadczeń użytkownika dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="3d62b-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="3d62b-144">Twilio</span><span class="sxs-lookup"><span data-stu-id="3d62b-144">Twilio:</span></span>  
   <span data-ttu-id="3d62b-145">Na karcie **pulpit nawigacyjny** konta usługi Twilio Skopiuj **Identyfikator SID konta** i **token uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="3d62b-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3d62b-146">ASPSMS:</span></span>  
   <span data-ttu-id="3d62b-147">W ustawieniach konta przejdź do **userKey** i skopiuj go wraz z własnym określonym **hasłem**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="3d62b-148">Te wartości zostaną później zapisane w zmiennych `SMSAccountIdentification` i `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="3d62b-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="3d62b-149">**Określanie SenderID/inicjatora**</span><span class="sxs-lookup"><span data-stu-id="3d62b-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="3d62b-150">Twilio</span><span class="sxs-lookup"><span data-stu-id="3d62b-150">Twilio:</span></span>  
   <span data-ttu-id="3d62b-151">Na karcie **liczby** Skopiuj numer telefonu Twilio.</span><span class="sxs-lookup"><span data-stu-id="3d62b-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="3d62b-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3d62b-152">ASPSMS:</span></span>  
   <span data-ttu-id="3d62b-153">W menu **odblokowane odblokowywanie** Odblokuj co najmniej jeden inicjator lub wybierz inicjator alfanumeryczny (nieobsługiwany przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="3d62b-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="3d62b-154">Ta wartość będzie później przechowywana w zmiennej `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="3d62b-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="3d62b-155">**Transferowanie poświadczeń dostawcy programu SMS do aplikacji**</span><span class="sxs-lookup"><span data-stu-id="3d62b-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="3d62b-156">Udostępnienie poświadczeń i numeru telefonu nadawcy dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3d62b-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="3d62b-157">Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="3d62b-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="3d62b-158">Konto i poświadczenia są dodawane do powyższego kodu, aby zapewnić prostą przykład.</span><span class="sxs-lookup"><span data-stu-id="3d62b-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="3d62b-159">Zobacz Jan ATTEN [ASP.NET MVC: Zachowaj ustawienia prywatne z kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d62b-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="3d62b-160">**Implementacja transferu danych do dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="3d62b-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="3d62b-161">Skonfiguruj klasę `SmsService` w *aplikacji\_pliku Start\IdentityConfig.cs* .</span><span class="sxs-lookup"><span data-stu-id="3d62b-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="3d62b-162">W zależności od użytego dostawcy programu SMS Aktywuj **Twilio** lub sekcję **ASPSMS** :</span><span class="sxs-lookup"><span data-stu-id="3d62b-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="3d62b-163">Uruchom aplikację i zaloguj się przy użyciu wcześniej zarejestrowanego konta.</span><span class="sxs-lookup"><span data-stu-id="3d62b-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="3d62b-164">Kliknij swój identyfikator użytkownika, który aktywuje metodę `Index` akcji w kontrolerze `Manage`.</span><span class="sxs-lookup"><span data-stu-id="3d62b-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="3d62b-165">Kliknij pozycję Add (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="3d62b-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="3d62b-166">W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="3d62b-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="3d62b-167">Wprowadź ją i naciśnij przycisk **Prześlij**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="3d62b-168">Widok zarządzanie pokazuje, że Twój numer telefonu został dodany.</span><span class="sxs-lookup"><span data-stu-id="3d62b-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="3d62b-169">Analizowanie kodu</span><span class="sxs-lookup"><span data-stu-id="3d62b-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="3d62b-170">Metoda akcji `Index` w kontrolerze `Manage` ustawia komunikat o stanie na podstawie poprzedniej akcji i zawiera linki umożliwiające zmianę hasła lokalnego lub dodanie konta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="3d62b-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="3d62b-171">W metodzie `Index` jest również wyświetlany stan lub numer telefonu funkcji 2FA, logowania zewnętrzne, funkcji 2FA włączone i Zapamiętaj funkcji 2FA metodę dla tej przeglądarki (wyjaśniono później).</span><span class="sxs-lookup"><span data-stu-id="3d62b-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="3d62b-172">Kliknięcie identyfikatora użytkownika (poczty e-mail) na pasku tytułu nie powoduje przekazania komunikatu.</span><span class="sxs-lookup"><span data-stu-id="3d62b-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="3d62b-173">Kliknięcie **numeru telefonu: Usuń** łącze passs `Message=RemovePhoneSuccess` jako ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="3d62b-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="3d62b-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="3d62b-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="3d62b-175">`AddPhoneNumber` Metoda akcji wyświetla okno dialogowe, w którym można wprowadzić numer telefonu, który może odbierać wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="3d62b-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="3d62b-176">Kliknięcie przycisku **Wyślij kod weryfikacyjny** spowoduje opublikowanie numeru telefonu w metodzie akcji post `AddPhoneNumber` http.</span><span class="sxs-lookup"><span data-stu-id="3d62b-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="3d62b-177">Metoda `GenerateChangePhoneNumberTokenAsync` generuje token zabezpieczający, który zostanie ustawiony w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="3d62b-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="3d62b-178">Jeśli usługa programu SMS została skonfigurowana, token jest wysyłany jako ciąg &quot;kod zabezpieczający &lt;token&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="3d62b-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="3d62b-179">Metoda `SmsService.SendAsync` do jest wywoływana asynchronicznie, a następnie aplikacja zostanie przekierowana do metody akcji `VerifyPhoneNumber` (która wyświetla następujące okno dialogowe), w której można wprowadzić kod weryfikacyjny.</span><span class="sxs-lookup"><span data-stu-id="3d62b-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="3d62b-180">Po wprowadzeniu kodu i kliknięciu przycisku Prześlij kod zostanie opublikowany w metodzie akcji POST protokołu HTTP `VerifyPhoneNumber`.</span><span class="sxs-lookup"><span data-stu-id="3d62b-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="3d62b-181">Metoda `ChangePhoneNumberAsync` sprawdza opublikowany kod zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3d62b-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="3d62b-182">Jeśli kod jest poprawny, numer telefonu zostanie dodany do pola `PhoneNumber` tabeli `AspNetUsers`.</span><span class="sxs-lookup"><span data-stu-id="3d62b-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="3d62b-183">Jeśli to wywołanie zakończyło się pomyślnie, wywoływana jest metoda `SignInAsync`:</span><span class="sxs-lookup"><span data-stu-id="3d62b-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="3d62b-184">Parametr `isPersistent` określa, czy sesja uwierzytelniania jest utrwalana w wielu żądaniach.</span><span class="sxs-lookup"><span data-stu-id="3d62b-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="3d62b-185">Po zmianie profilu zabezpieczeń jest generowana nowa Sygnatura zabezpieczeń, która będzie przechowywana w polu `SecurityStamp` tabeli *AspNetUsers* .</span><span class="sxs-lookup"><span data-stu-id="3d62b-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="3d62b-186">Zwróć uwagę, że pole `SecurityStamp` różni się od pliku cookie zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3d62b-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="3d62b-187">Plik cookie zabezpieczeń nie jest przechowywany w tabeli `AspNetUsers` (lub w innym miejscu w bazie danych tożsamości).</span><span class="sxs-lookup"><span data-stu-id="3d62b-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="3d62b-188">Token pliku cookie zabezpieczeń jest z podpisem własnym przy użyciu funkcji [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i jest tworzony z informacjami o `UserId, SecurityStamp` i czasie wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="3d62b-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="3d62b-189">Oprogramowanie pośredniczące cookie sprawdza plik cookie na każdym żądaniu.</span><span class="sxs-lookup"><span data-stu-id="3d62b-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="3d62b-190">Metoda `SecurityStampValidator` w klasie `Startup` trafi do bazy danych i okresowo sprawdza sygnaturę zabezpieczeń, jak określono w `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="3d62b-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="3d62b-191">Dzieje się to co 30 minut (w naszym przykładzie), chyba że zmienisz profil zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3d62b-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="3d62b-192">Wybrano 30-minutowy interwał minimalizowania podróży do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3d62b-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="3d62b-193">Metodę `SignInAsync` należy wywołać w przypadku wprowadzenia zmiany w profilu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3d62b-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="3d62b-194">Gdy profil zabezpieczeń ulegnie zmianie, baza danych programu aktualizuje pole `SecurityStamp` i bez wywoływania metody `SignInAsync`, *tylko* do momentu, gdy potok Owin trafi do bazy danych (`validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="3d62b-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="3d62b-195">Można to przetestować, zmieniając metodę `SignInAsync`, aby zwracała się natychmiast, i ustawiając właściwość `validateInterval` plików cookie z 30 minut na 5 sekund:</span><span class="sxs-lookup"><span data-stu-id="3d62b-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="3d62b-196">Po wprowadzeniu powyższych zmian w kodzie można zmienić profil zabezpieczeń (na przykład zmieniając stan **dwóch włączonych**), a użytkownik zostanie wylogowany w ciągu 5 sekund, gdy metoda `SecurityStampValidator.OnValidateIdentity` nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3d62b-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="3d62b-197">Usuń wiersz Return w metodzie `SignInAsync`, wprowadź inną zmianę profilu zabezpieczeń i nie będzie się wylogować. Metoda `SignInAsync` generuje nowy plik cookie zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3d62b-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="3d62b-198">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3d62b-198">Enable two-factor authentication</span></span>

<span data-ttu-id="3d62b-199">W przykładowej aplikacji należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (funkcji 2FA).</span><span class="sxs-lookup"><span data-stu-id="3d62b-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="3d62b-200">Aby włączyć funkcji 2FA, kliknij identyfikator użytkownika (alias e-mail) na pasku nawigacyjnym.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="3d62b-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="3d62b-201">Kliknij pozycję Włącz funkcji 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="3d62b-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="3d62b-202">Wyloguj się, a następnie zaloguj się ponownie.</span><span class="sxs-lookup"><span data-stu-id="3d62b-202">Log out, then log back in.</span></span> <span data-ttu-id="3d62b-203">Jeśli włączono obsługę poczty e-mail (Zobacz mój [poprzedni samouczek](account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomość SMS lub e-mail dla usługi funkcji 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="3d62b-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="3d62b-204">Zostanie wyświetlona strona Sprawdź kod, na której można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="3d62b-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="3d62b-205">Kliknięcie pola wyboru **Zapamiętaj tę przeglądarkę** spowoduje zwolnienie z używania funkcji 2FA do logowania się na tym komputerze i w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="3d62b-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="3d62b-206">Włączenie funkcji 2FA i kliknięcie opcji Pamiętaj, że **Ta przeglądarka** zapewni mocną ochronę funkcji 2FA przed złośliwymi użytkownikami próbującymi uzyskać dostęp do Twojego konta, o ile nie mają dostępu do komputera.</span><span class="sxs-lookup"><span data-stu-id="3d62b-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="3d62b-207">Można to zrobić na wszystkich maszynach prywatnych, których regularnie używasz.</span><span class="sxs-lookup"><span data-stu-id="3d62b-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="3d62b-208">Ustawienie **Zapamiętaj tę przeglądarkę**, aby zwiększyć bezpieczeństwo funkcji 2FA z komputerów, które nie są regularnie używane, i uzyskać wygodę, aby nie mieć możliwości przechodzenia przez funkcji 2FA na własnych komputerach.</span><span class="sxs-lookup"><span data-stu-id="3d62b-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="3d62b-209">Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3d62b-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="3d62b-210">Podczas tworzenia nowego projektu MVC plik *IdentityConfig.cs* zawiera następujący kod, aby zarejestrować dostawcę uwierzytelniania dwuskładnikowego:</span><span class="sxs-lookup"><span data-stu-id="3d62b-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="3d62b-211">Dodaj numer telefonu do funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="3d62b-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="3d62b-212">Metoda akcji `AddPhoneNumber` na kontrolerze `Manage` generuje token zabezpieczający i wysyła go do podanego numeru telefonu.</span><span class="sxs-lookup"><span data-stu-id="3d62b-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="3d62b-213">Po wysłaniu tokenu przekierowuje do metody akcji `VerifyPhoneNumber`, w której można wprowadzić kod w celu zarejestrowania programu SMS for funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="3d62b-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="3d62b-214">Program SMS funkcji 2FA nie jest używany do momentu zweryfikowania numeru telefonu.</span><span class="sxs-lookup"><span data-stu-id="3d62b-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="3d62b-215">Włączanie funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="3d62b-215">Enabling 2FA</span></span>

<span data-ttu-id="3d62b-216">Metoda akcji `EnableTFA` umożliwia funkcji 2FA:</span><span class="sxs-lookup"><span data-stu-id="3d62b-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="3d62b-217">Należy zwrócić uwagę, że należy wywołać `SignInAsync`, ponieważ wartość Enable funkcji 2FA to zmiana profilu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3d62b-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="3d62b-218">Gdy funkcji 2FA jest włączona, użytkownik będzie musiał użyć usługi funkcji 2FA do zalogowania się przy użyciu funkcji 2FA podejścia do rejestracji (wiadomości SMS i wiadomości e-mail w przykładzie).</span><span class="sxs-lookup"><span data-stu-id="3d62b-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="3d62b-219">Możesz dodać więcej dostawców funkcji 2FA, takich jak generatory kodu QR, lub napisać użytkownika (zobacz [Korzystanie z usługi Google Authenticator z ASP.NET Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="3d62b-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="3d62b-220">Kody funkcji 2FA są generowane przy użyciu [algorytmu hasła jednorazowego opartego na czasie](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) , a kody są ważne przez sześć minut.</span><span class="sxs-lookup"><span data-stu-id="3d62b-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="3d62b-221">W przypadku wprowadzenia kodu przez więcej niż 6 minut zostanie wyświetlony komunikat o błędzie nieprawidłowego kodu.</span><span class="sxs-lookup"><span data-stu-id="3d62b-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>

<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="3d62b-222">Łączenie kont logowania społecznościowych i lokalnych</span><span class="sxs-lookup"><span data-stu-id="3d62b-222">Combine social and local login accounts</span></span>

<span data-ttu-id="3d62b-223">Konta lokalne i społecznościowe można łączyć przez kliknięcie linku do poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="3d62b-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="3d62b-224">W następującej kolejności &quot;RickAndMSFT@gmail.com&quot; jest najpierw tworzony jako logowanie lokalne, ale w pierwszej kolejności można utworzyć konto w trybie społecznościowym, a następnie dodać lokalną nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="3d62b-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="3d62b-225">Kliknij link **Zarządzaj** .</span><span class="sxs-lookup"><span data-stu-id="3d62b-225">Click on the **Manage** link.</span></span> <span data-ttu-id="3d62b-226">Zwróć uwagę na wartość 0 External (logowanie społeczne) skojarzoną z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="3d62b-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="3d62b-227">Kliknij link do innej usługi logowania i zaakceptuj żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3d62b-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="3d62b-228">Te dwa konta zostały połączone, ale będzie można zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="3d62b-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="3d62b-229">Możesz chcieć, aby użytkownicy mogli dodawać konta lokalne w przypadku, gdy ich usługa uwierzytelniania w sieci społecznościowej nie działa, lub prawdopodobnie utraciły dostęp do konta społecznościowego.</span><span class="sxs-lookup"><span data-stu-id="3d62b-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="3d62b-230">Na poniższej ilustracji, Tomasz to logowanie społecznościowe (które można zobaczyć od **zewnętrznych logowań: 1** pokazane na stronie).</span><span class="sxs-lookup"><span data-stu-id="3d62b-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="3d62b-231">Kliknięcie przycisku **Wybierz hasło** umożliwia dodanie lokalnego logowania skojarzonego z tym samym kontem.</span><span class="sxs-lookup"><span data-stu-id="3d62b-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="3d62b-232">Blokada konta przed atakami polegającymi na rozsile</span><span class="sxs-lookup"><span data-stu-id="3d62b-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="3d62b-233">Możesz chronić konta w aplikacji przed atakami słownikowymi, włączając blokadę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3d62b-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="3d62b-234">Następujący kod w metodzie `ApplicationUserManager Create` konfiguruje blokadę:</span><span class="sxs-lookup"><span data-stu-id="3d62b-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="3d62b-235">Powyższy kod włącza blokadę tylko do uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="3d62b-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="3d62b-236">Można włączyć opcję Zablokuj dla logowań, zmieniając `shouldLockout` na true w metodzie `Login` kontrolera konta, zalecamy nie włączać blokowania dla logowań, ponieważ sprawia, że konto podatne na [ataki logowania.](http://en.wikipedia.org/wiki/Denial-of-service_attack)</span><span class="sxs-lookup"><span data-stu-id="3d62b-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="3d62b-237">W przykładowym kodzie blokada jest wyłączona dla konta administratora utworzonego w metodzie `ApplicationDbInitializer Seed`:</span><span class="sxs-lookup"><span data-stu-id="3d62b-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="3d62b-238">Wymaganie od użytkownika posiadania zweryfikowanego konta e-mail</span><span class="sxs-lookup"><span data-stu-id="3d62b-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="3d62b-239">Poniższy kod wymaga, aby użytkownik miał zweryfikowane konto e-mail przed zalogowaniem się:</span><span class="sxs-lookup"><span data-stu-id="3d62b-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="3d62b-240">Jak SignInManager sprawdza wymagania funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="3d62b-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="3d62b-241">Logowanie lokalne i logowanie w sieci społecznościowej sprawdzają, czy funkcji 2FA jest włączona.</span><span class="sxs-lookup"><span data-stu-id="3d62b-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="3d62b-242">Jeśli funkcji 2FA jest włączona, metoda logowania `SignInManager` zwraca `SignInStatus.RequiresVerification`i użytkownik zostanie przekierowany do metody akcji `SendCode`, gdzie będzie musiał wprowadzić kod, aby ukończyć sekwencję logowania.</span><span class="sxs-lookup"><span data-stu-id="3d62b-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="3d62b-243">Jeśli użytkownik ma kontrolka rememberMe jest ustawiony w lokalnym pliku cookie użytkowników, `SignInManager` zwróci `SignInStatus.Success` i nie będzie musiał przejść przez funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="3d62b-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="3d62b-244">Poniższy kod przedstawia metodę akcji `SendCode`.</span><span class="sxs-lookup"><span data-stu-id="3d62b-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="3d62b-245">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest tworzony ze wszystkimi metodami funkcji 2FA włączonymi dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3d62b-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="3d62b-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest przenoszona do pomocnika [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) , dzięki czemu użytkownik może wybrać podejście funkcji 2FA (zazwyczaj wiadomości e-mail i SMS).</span><span class="sxs-lookup"><span data-stu-id="3d62b-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="3d62b-247">Gdy użytkownik zaksięguje podejście funkcji 2FA, wywoływana jest metoda działania `HTTP POST SendCode`, `SignInManager` wysyła kod funkcji 2FA, a użytkownik zostaje przekierowany do metody akcji `VerifyCode`, gdzie mogą wprowadzić kod, aby zakończyć logowanie.</span><span class="sxs-lookup"><span data-stu-id="3d62b-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="3d62b-248">Blokada funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="3d62b-248">2FA Lockout</span></span>

<span data-ttu-id="3d62b-249">Mimo że można ustawić blokadę konta dla nieudanych prób logowania, to podejście sprawia, że logowanie jest podatne na blokady [systemu DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) .</span><span class="sxs-lookup"><span data-stu-id="3d62b-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="3d62b-250">Zalecamy używanie blokady konta tylko z funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="3d62b-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="3d62b-251">Po utworzeniu `ApplicationUserManager` przykładowy kod ustawia funkcji 2FA blokady i `MaxFailedAccessAttemptsBeforeLockout` na pięć.</span><span class="sxs-lookup"><span data-stu-id="3d62b-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="3d62b-252">Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowego) każda niepomyślna próba w funkcji 2FA jest przechowywana i jeśli osiągnięto maksymalną liczbę prób, użytkownik zostanie zablokowany przez pięć minut (można ustawić czas blokowania na `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="3d62b-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="3d62b-253">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3d62b-253">Additional Resources</span></span>

- <span data-ttu-id="3d62b-254">[ASP.NET Identity zalecane zasoby](../getting-started/aspnet-identity-recommended-resources.md) Pełna lista blogów, klipów wideo, samouczków i doskonałych elementów.</span><span class="sxs-lookup"><span data-stu-id="3d62b-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="3d62b-255">[Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) również pokazuje, jak dodać informacje o profilu do tabeli users.</span><span class="sxs-lookup"><span data-stu-id="3d62b-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="3d62b-256">[ASP.NET MVC i Identity 2,0: zrozumienie podstaw](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez John ATTEN.</span><span class="sxs-lookup"><span data-stu-id="3d62b-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="3d62b-257">Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="3d62b-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="3d62b-258">Wprowadzenie do produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="3d62b-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="3d62b-259">[Zapowiedź RTM ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="3d62b-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="3d62b-260">[ASP.NET Identity 2,0: Konfigurowanie weryfikacji konta i autoryzacji dwuskładnikowej](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Jan ATTEN.</span><span class="sxs-lookup"><span data-stu-id="3d62b-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>

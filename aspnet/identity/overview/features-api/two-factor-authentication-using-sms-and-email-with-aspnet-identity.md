---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity — ASP.NET 4.x
author: HaoK
description: Ten samouczek przedstawia sposób konfigurowania uwierzytelniania dwuskładnikowego (2FA) za pomocą wiadomości SMS i wiadomości e-mail. W tym artykule został napisany przez Rick Anderson ( @RickAndMSFT ), wzór...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c41fc06ad98665f7d48efde030c1341b06e49dd0
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395294"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="c7325-104">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="c7325-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>

<span data-ttu-id="c7325-105">przez [Hao Kung](https://github.com/HaoK), [autorem jest Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="c7325-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="c7325-106">Ten samouczek przedstawia sposób konfigurowania uwierzytelniania dwuskładnikowego (2FA) za pomocą wiadomości SMS i wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c7325-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="c7325-107">W tym artykule został napisany przez Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), autorem jest Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="c7325-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="c7325-108">Przykład NuGet został napisany głównie przez Hao Kung.</span><span class="sxs-lookup"><span data-stu-id="c7325-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="c7325-109">W tym temacie omówiono następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="c7325-109">This topic covers the following:</span></span>

- [<span data-ttu-id="c7325-110">Tworzenie przykładowej tożsamości</span><span class="sxs-lookup"><span data-stu-id="c7325-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="c7325-111">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="c7325-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="c7325-112">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="c7325-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="c7325-113">Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="c7325-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="c7325-114">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="c7325-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="c7325-115">Blokada konta przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="c7325-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="c7325-116">Tworzenie przykładowej tożsamości</span><span class="sxs-lookup"><span data-stu-id="c7325-116">Building the Identity sample</span></span>

<span data-ttu-id="c7325-117">W tej sekcji użyjesz NuGet do pobrania próbki, firma Microsoft będzie działać z.</span><span class="sxs-lookup"><span data-stu-id="c7325-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="c7325-118">Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="c7325-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="c7325-119">Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="c7325-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="c7325-120">Ostrzeżenie: Należy zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) do ukończenia tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c7325-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="c7325-121">Utwórz nową ***pusty*** projektu sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c7325-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="c7325-122">W konsoli Menedżera pakietów wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="c7325-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="c7325-123">W tym samouczku użyjemy [SendGrid](http://sendgrid.com/) do wysyłania wiadomości e-mail i [Twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) dla tekstowe sms.</span><span class="sxs-lookup"><span data-stu-id="c7325-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="c7325-124">`Identity.Samples` Pakiet instaluje kodu, firma Microsoft będzie pracował z.</span><span class="sxs-lookup"><span data-stu-id="c7325-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="c7325-125">Ustaw [projektu do używania protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="c7325-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="c7325-126">*Opcjonalnie*: Postępuj zgodnie z instrukcjami w mojej [samouczek potwierdzenie E-mail](account-confirmation-and-password-recovery-with-aspnet-identity.md) do podpinanie SendGrid, a następnie uruchom aplikację i zarejestrować konto e-mail.</span><span class="sxs-lookup"><span data-stu-id="c7325-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="c7325-127">*Opcjonalnie:* Usuń kod potwierdzenia łącze w wiadomości e-mail pokaz z przykładu ( `ViewBag.Link` kod na kontrolerze konta.</span><span class="sxs-lookup"><span data-stu-id="c7325-127">*Optional:* Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="c7325-128">Zobacz `DisplayEmail` i `ForgotPasswordConfirmation` metody akcji i widokami razor).</span><span class="sxs-lookup"><span data-stu-id="c7325-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="c7325-129">*Opcjonalnie:* Usuń `ViewBag.Status` kodu z kontrolerami zarządzania oraz konta i *Views\Account\VerifyCode.cshtml* i *Views\Manage\VerifyPhoneNumber.cshtml* widokami razor.</span><span class="sxs-lookup"><span data-stu-id="c7325-129">*Optional:* Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml* and *Views\Manage\VerifyPhoneNumber.cshtml* razor views.</span></span> <span data-ttu-id="c7325-130">Alternatywnie możesz zachować `ViewBag.Status` wyświetlany obraz do potrzeb testowania, w jaki sposób ta aplikacja działa lokalnie, bez konieczności Podłączanie i wysłać wiadomość e-mail i wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="c7325-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="c7325-131">Ostrzeżenie: W przypadku zmiany ustawień zabezpieczeń, w tym przykładzie produkcji aplikacji będzie musiała zostać poddane inspekcji zabezpieczeń, który jawnie wywołuje się zmian.</span><span class="sxs-lookup"><span data-stu-id="c7325-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="c7325-132">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="c7325-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="c7325-133">Ten samouczek zawiera instrukcje dotyczące korzystania z usługi Twilio lub ASPSMS, ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="c7325-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. **<span data-ttu-id="c7325-134">Tworzenie konta użytkownika z dostawcą programu SMS</span><span class="sxs-lookup"><span data-stu-id="c7325-134">Creating a User Account with an SMS provider</span></span>**  
  
   <span data-ttu-id="c7325-135">Tworzenie [Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) konta.</span><span class="sxs-lookup"><span data-stu-id="c7325-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. **<span data-ttu-id="c7325-136">Instalowanie dodatkowych pakietów lub dodawania odwołania do usług</span><span class="sxs-lookup"><span data-stu-id="c7325-136">Installing additional packages or adding service references</span></span>**  
  
   <span data-ttu-id="c7325-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="c7325-137">Twilio:</span></span>  
   <span data-ttu-id="c7325-138">W konsoli Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c7325-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="c7325-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="c7325-139">ASPSMS:</span></span>  
   <span data-ttu-id="c7325-140">Należy dodać następujące odwołanie do usługi:</span><span class="sxs-lookup"><span data-stu-id="c7325-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="c7325-141">Adres:</span><span class="sxs-lookup"><span data-stu-id="c7325-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="c7325-142">Przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="c7325-142">Namespace:</span></span>  
    `ASPSMSX2`
3. **<span data-ttu-id="c7325-143">Ustalenie, poświadczenia użytkownika dostawcy programu SMS</span><span class="sxs-lookup"><span data-stu-id="c7325-143">Figuring out SMS Provider User credentials</span></span>**  
  
   <span data-ttu-id="c7325-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="c7325-144">Twilio:</span></span>  
   <span data-ttu-id="c7325-145">Z **pulpit nawigacyjny** karty konta usługi Twilio, kopiowania **identyfikator SID konta** i **tokenu uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="c7325-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="c7325-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="c7325-146">ASPSMS:</span></span>  
   <span data-ttu-id="c7325-147">W ustawieniach konta, przejdź do **Userkey** i skopiować go wraz z własnym zdefiniowanych **hasło**.</span><span class="sxs-lookup"><span data-stu-id="c7325-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="c7325-148">Te wartości będą później przechowywane w zmiennych `SMSAccountIdentification` i `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="c7325-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. **<span data-ttu-id="c7325-149">Określanie SenderID / inicjatora</span><span class="sxs-lookup"><span data-stu-id="c7325-149">Specifying SenderID / Originator</span></span>**  
  
   <span data-ttu-id="c7325-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="c7325-150">Twilio:</span></span>  
   <span data-ttu-id="c7325-151">Z **numery** kartę, skopiuj numer telefonu usługi Twilio.</span><span class="sxs-lookup"><span data-stu-id="c7325-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="c7325-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="c7325-152">ASPSMS:</span></span>  
   <span data-ttu-id="c7325-153">W ramach **odblokować nadawcy** Menu odblokować przynajmniej jednego nadawcy, lub wybierz inicjatora alfanumeryczne (nie obsługiwany przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="c7325-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="c7325-154">Ta wartość później będą przechowywane w zmiennej `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="c7325-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. **<span data-ttu-id="c7325-155">Transferowanie poświadczenia dostawcy programu SMS w aplikacji</span><span class="sxs-lookup"><span data-stu-id="c7325-155">Transferring SMS provider credentials into app</span></span>**  
  
   <span data-ttu-id="c7325-156">Udostępnij poświadczenia i numer telefonu nadawcy aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c7325-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="c7325-157">Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="c7325-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="c7325-158">Konto i poświadczenia są dodawane do kodu powyżej, aby uprościć przykład.</span><span class="sxs-lookup"><span data-stu-id="c7325-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="c7325-159">Zobacz Jon Atten [platformy ASP.NET MVC: Zachowaj prywatne poza ustawienia kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="c7325-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. **<span data-ttu-id="c7325-160">Implementacja transfer danych do dostawcy programu SMS</span><span class="sxs-lookup"><span data-stu-id="c7325-160">Implementation of data transfer to SMS provider</span></span>**  
  
   <span data-ttu-id="c7325-161">Konfigurowanie `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="c7325-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="c7325-162">W zależności od dostawcy programu SMS używanej aktywować **Twilio** lub **ASPSMS** sekcji:</span><span class="sxs-lookup"><span data-stu-id="c7325-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="c7325-163">Uruchom aplikację i zaloguj się przy użyciu konta które wcześniej zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="c7325-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="c7325-164">Kliknij swój identyfikator użytkownika, które uaktywnia się `Index` metody akcji w `Manage` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c7325-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="c7325-165">Kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="c7325-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="c7325-166">W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="c7325-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="c7325-167">Wprowadź go, a następnie naciśnij klawisz **przesyłania**.</span><span class="sxs-lookup"><span data-stu-id="c7325-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="c7325-168">W widoku Zarządzanie pokazuje, że numer telefonu został dodany.</span><span class="sxs-lookup"><span data-stu-id="c7325-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="c7325-169">Poszukaj w kodzie</span><span class="sxs-lookup"><span data-stu-id="c7325-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="c7325-170">`Index` Metody akcji w `Manage` kontrolera ustawia komunikat o stanie, oparte na poprzedniej akcji i zawiera łącza, aby zmienić hasło lokalne lub dodać konto lokalne.</span><span class="sxs-lookup"><span data-stu-id="c7325-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="c7325-171">`Index` Metoda również Wyświetla stan lub usługi uwierzytelniania 2FA telefonów, numer, logowań zewnętrznych, funkcji 2FA, włączona i pamiętać metody uwierzytelniania 2FA, w tej przeglądarce (opisana w dalszej części).</span><span class="sxs-lookup"><span data-stu-id="c7325-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="c7325-172">Klikając nazwę użytkownika (adres e-mail) na pasku tytułu nie przeszło wiadomość.</span><span class="sxs-lookup"><span data-stu-id="c7325-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="c7325-173">Kliknięcie **numer telefonu: Usuń** link przebiegów `Message=RemovePhoneSuccess` jako ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="c7325-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="c7325-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="c7325-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="c7325-175">`AddPhoneNumber` Metody akcji, wyświetlane jest okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="c7325-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="c7325-176">Kliknięcie **Wyślij kod weryfikacyjny** przycisk wpisów numer telefonu HTTP POST `AddPhoneNumber` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="c7325-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="c7325-177">`GenerateChangePhoneNumberTokenAsync` Metoda generuje token zabezpieczający, które zostaną ustawione w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="c7325-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="c7325-178">Jeśli usługa programu SMS został skonfigurowany, token jest wysyłany jako parametry &quot;Twój kod zabezpieczający: &lt;tokenu&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7325-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="c7325-179">`SmsService.SendAsync` Metoda jest wywoływana asynchronicznie, a następnie aplikacja jest przekierowywane do `VerifyPhoneNumber` metody akcji, (która wyświetla następujące okno dialogowe), gdzie można wprowadzić kod weryfikacyjny.</span><span class="sxs-lookup"><span data-stu-id="c7325-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="c7325-180">Po wprowadzeniu kodu i kliknij przycisk Prześlij, aby HTTP POST opublikowaniu kodu `VerifyPhoneNumber` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="c7325-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="c7325-181">`ChangePhoneNumberAsync` Metoda sprawdza kod zabezpieczeń opublikowane.</span><span class="sxs-lookup"><span data-stu-id="c7325-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="c7325-182">Jeśli kod jest poprawny, numer telefonu jest dodawany do `PhoneNumber` pola `AspNetUsers` tabeli.</span><span class="sxs-lookup"><span data-stu-id="c7325-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="c7325-183">Jeśli to wywołanie zakończy się pomyślnie, `SignInAsync` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="c7325-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="c7325-184">`isPersistent` Parametr określa, czy sesja uwierzytelniania jest trwała dla wielu żądań.</span><span class="sxs-lookup"><span data-stu-id="c7325-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="c7325-185">Zmiany profilu zabezpieczeń nową sygnaturę bezpieczeństwa jest wygenerowany i przechowywany w `SecurityStamp` pole *AspNetUsers* tabeli.</span><span class="sxs-lookup"><span data-stu-id="c7325-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="c7325-186">Uwaga: `SecurityStamp` pola różni się od pliku cookie zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c7325-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="c7325-187">Plik cookie zabezpieczeń nie są przechowywane w `AspNetUsers` tabeli (lub dowolnego miejsca w bazie danych tożsamości).</span><span class="sxs-lookup"><span data-stu-id="c7325-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="c7325-188">Token pliku cookie zabezpieczeń jest z podpisem własnym za pomocą [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i jest tworzona przy użyciu `UserId, SecurityStamp` i informacji czasu wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="c7325-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="c7325-189">Oprogramowaniu pośredniczącym pliku cookie sprawdza, czy plik cookie na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="c7325-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="c7325-190">`SecurityStampValidator` Method in Class metoda `Startup` klasy trafienia bazy danych i okresowo sprawdza sygnaturę bezpieczeństwa określone za pomocą `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="c7325-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="c7325-191">Dzieje się to tylko co 30 minut (w naszym przykładzie) Jeśli nie zmienisz swój profil zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c7325-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="c7325-192">W okresie 30 minut została wybrana, aby zminimalizować przesłania do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c7325-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="c7325-193">`SignInAsync` Metody musi być wywoływany w przypadku wszelkich zmian do profilu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c7325-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="c7325-194">Po zmianie profil zabezpieczeń bazy danych jest aktualizacje `SecurityStamp` pola i bez wywoływania `SignInAsync` metody, czy użytkownik pozostanie zalogowany w *tylko* dopiero od następnej potoku OWIN uderza w bazie danych ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="c7325-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="c7325-195">Można to sprawdzić, zmieniając `SignInAsync` metody do zwrócenia natychmiast, a ustawienie pliku cookie `validateInterval` właściwości od 30 minut na 5 sekund:</span><span class="sxs-lookup"><span data-stu-id="c7325-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="c7325-196">Za pomocą powyższych zmian w kodzie, można zmienić swój profil zabezpieczeń (na przykład przez zmianę stanu **dwie włączone współczynnik**) i użytkownik zostanie wylogowany w ciągu 5 sekund po `SecurityStampValidator.OnValidateIdentity` metoda kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="c7325-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="c7325-197">Usuń wiersz zwrotu w `SignInAsync` metody, ustawić inny profil zabezpieczeń, zmiany i nie będziesz się logować. `SignInAsync` Metoda generuje nowy plik cookie zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c7325-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="c7325-198">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="c7325-198">Enable two-factor authentication</span></span>

<span data-ttu-id="c7325-199">W przykładowej aplikacji należy włączyć uwierzytelnianie dwuskładnikowe (2FA) za pomocą interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c7325-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="c7325-200">Aby włączyć funkcję 2FA, kliknij swoją nazwę użytkownika (alias adresu e-mail) na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="c7325-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
<span data-ttu-id="c7325-201">Kliknij pozycję Włącz 2FA.</span><span class="sxs-lookup"><span data-stu-id="c7325-201">Click on enable 2FA.</span></span>![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) <span data-ttu-id="c7325-202">Wyloguj się, a następnie zaloguj się ponownie.</span><span class="sxs-lookup"><span data-stu-id="c7325-202">Log out, then log back in.</span></span> <span data-ttu-id="c7325-203">Po włączeniu poczty e-mail (zobacz mój [poprzedniego samouczka](account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomości SMS lub wiadomości e-mail dla funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="c7325-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) <span data-ttu-id="c7325-204">Zostanie wyświetlona strona Sprawdź kodu, gdzie można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).</span><span class="sxs-lookup"><span data-stu-id="c7325-204">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) <span data-ttu-id="c7325-205">Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z funkcji 2FA, zaloguj się przy użyciu tego komputera i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="c7325-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="c7325-206">Włączanie funkcji 2FA, a następnie klikając **pamiętasz tę przeglądarkę** udostępni silnego uwierzytelniania 2FA, ochronę przed złośliwymi użytkownikami podjęcie próby dostępu do Twojego konta, tak długo, jak nie mają dostępu do komputera.</span><span class="sxs-lookup"><span data-stu-id="c7325-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="c7325-207">Można to zrobić na dowolnej maszynie prywatnych, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="c7325-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="c7325-208">Ustawiając **pamiętasz tę przeglądarkę**, uzyskaj dodatkowe zabezpieczenie w postaci funkcji 2FA z komputerów, które nie są regularnie używane i Uzyskaj wygody na ominięcie za pomocą funkcji 2FA na własnych komputerów.</span><span class="sxs-lookup"><span data-stu-id="c7325-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="c7325-209">Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="c7325-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="c7325-210">Podczas tworzenia nowego projektu MVC *IdentityConfig.cs* plik zawiera następujący kod, aby zarejestrować dostawcę uwierzytelniania dwuetapowego:</span><span class="sxs-lookup"><span data-stu-id="c7325-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="c7325-211">Dodaj numer telefonu dla funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="c7325-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="c7325-212">`AddPhoneNumber` Metody akcji w `Manage` kontrolera generuje token zabezpieczający, a następnie wysyła je z numerem telefonu numeru, jaki podano.</span><span class="sxs-lookup"><span data-stu-id="c7325-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="c7325-213">Po wysłaniu token, zostanie przekierowany do `VerifyPhoneNumber` metody akcji, w którym można wprowadzić kod, aby zarejestrować programu SMS dla uwierzytelniania 2FA.</span><span class="sxs-lookup"><span data-stu-id="c7325-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="c7325-214">2FA programu SMS nie jest używana do momentu zweryfikowania numeru telefonu.</span><span class="sxs-lookup"><span data-stu-id="c7325-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="c7325-215">Włączanie funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="c7325-215">Enabling 2FA</span></span>

<span data-ttu-id="c7325-216">`EnableTFA` Metody akcji umożliwia funkcji 2FA:</span><span class="sxs-lookup"><span data-stu-id="c7325-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="c7325-217">Uwaga `SignInAsync` musi zostać wywołana, ponieważ Włączanie funkcji 2FA według regionów powoduje zmianę do profilu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c7325-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="c7325-218">Po włączeniu funkcji 2FA, użytkownik musi zalogować się, za pomocą funkcji 2FA przy użyciu metod uwierzytelniania 2FA, zarejestrowanych (wiadomości SMS i wiadomości e-mail w przykładzie).</span><span class="sxs-lookup"><span data-stu-id="c7325-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="c7325-219">Możesz dodać więcej dostawców uwierzytelniania 2FA, takie jak generatory kodu QR lub napisać jesteś właścicielem (zobacz [za pomocą tokenu Google Authenticator w produkcie ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="c7325-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="c7325-220">Kody uwierzytelniania 2FA są generowane przy użyciu [oparte na czasie algorytm hasła jednorazowego](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) i kody są ważne przez 6 minut.</span><span class="sxs-lookup"><span data-stu-id="c7325-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="c7325-221">Jeśli nie podejmiesz więcej niż sześciu minut w celu wprowadzenia kodu, otrzymasz komunikat o błędzie nieprawidłowego kodu.</span><span class="sxs-lookup"><span data-stu-id="c7325-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="c7325-222">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="c7325-222">Combine social and local login accounts</span></span>

<span data-ttu-id="c7325-223">Konta lokalne i społecznościowych można łączyć, klikając link wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c7325-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="c7325-224">W następującej kolejności &quot; RickAndMSFT@gmail.com &quot; najpierw jest tworzony podczas logowania lokalnego, ale możesz utworzyć konto jako dziennik społecznościowych najpierw, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="c7325-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="c7325-225">Kliknij pozycję **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="c7325-225">Click on the **Manage** link.</span></span> <span data-ttu-id="c7325-226">Należy pamiętać, zewnętrzne 0 (logowania społecznościowego) skojarzony z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="c7325-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="c7325-227">Kliknij link do innego dziennika w usłudze i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c7325-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="c7325-228">Te dwa konta zostały połączone, będzie można zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="c7325-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="c7325-229">Możesz zechcieć użytkownikom dodawanie kont lokalnych, w przypadku, gdy ich społecznościowych logowania usługi uwierzytelniania nie działa lub większe prawdopodobieństwo po utracie dostępu do kont społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="c7325-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="c7325-230">Na poniższej ilustracji, Tom jest społecznościowych logowania (którą można zobaczyć z **logowania zewnętrzne: 1** wyświetlany na stronie).</span><span class="sxs-lookup"><span data-stu-id="c7325-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="c7325-231">Kliknięcie **Wybierz hasło** pozwala na dodawanie w lokalnym dzienniku na skojarzone z tego samego konta.</span><span class="sxs-lookup"><span data-stu-id="c7325-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="c7325-232">Blokada konta przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="c7325-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="c7325-233">Konta w aplikacji na ataki słownikowe można chronić przez włączenie blokady użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c7325-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="c7325-234">Poniższy kod w `ApplicationUserManager Create` metoda konfiguruje blokady:</span><span class="sxs-lookup"><span data-stu-id="c7325-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="c7325-235">Powyższy kod umożliwia blokady dla tylko z uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="c7325-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="c7325-236">Chociaż można włączyć blokady dla nazw logowania, zmieniając `shouldLockout` na wartość true w `Login` metody kontroler kont, firma Microsoft zaleca, możesz włączyć nie blokady dla nazw logowania, ponieważ to sprawia, że konto podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) logowania ataki.</span><span class="sxs-lookup"><span data-stu-id="c7325-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="c7325-237">W przykładowym kodzie blokada jest wyłączona, konto administratora jest utworzony w `ApplicationDbInitializer Seed` metody:</span><span class="sxs-lookup"><span data-stu-id="c7325-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="c7325-238">Wymaganie użytkownika konta e-mail zweryfikowane</span><span class="sxs-lookup"><span data-stu-id="c7325-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="c7325-239">Poniższy kod wymaga od użytkownika posiadania konta zatwierdzonych wiadomości e-mail przed ich może zalogować się:</span><span class="sxs-lookup"><span data-stu-id="c7325-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="c7325-240">Jak SignInManager sprawdza, czy wymaganie uwierzytelniania 2FA</span><span class="sxs-lookup"><span data-stu-id="c7325-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="c7325-241">Lokalne logowanie i społecznościowych logowania Sprawdź, czy włączono uwierzytelniania 2FA.</span><span class="sxs-lookup"><span data-stu-id="c7325-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="c7325-242">Po włączeniu funkcji 2FA `SignInManager` logowania, metoda zwraca `SignInStatus.RequiresVerification`, a użytkownik zostanie przekierowany do `SendCode` metody akcji, w którym konieczne będzie wprowadzenie kodu do wykonania w dzienniku w sekwencji.</span><span class="sxs-lookup"><span data-stu-id="c7325-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="c7325-243">Jeśli użytkownik ma RememberMe jest ustawiona na użytkowników lokalnych plików cookie, `SignInManager` zwróci `SignInStatus.Success` a nie zostaną wprowadzone za pośrednictwem funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="c7325-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="c7325-244">Poniższy kod przedstawia `SendCode` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="c7325-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="c7325-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) są tworzone za pomocą wszystkich metod uwierzytelniania 2FA, włączone dla danego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c7325-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="c7325-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest przekazywany do [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocnika, która umożliwia użytkownikowi wybranie metody uwierzytelniania 2FA (zwykle adres e-mail i wiadomości SMS).</span><span class="sxs-lookup"><span data-stu-id="c7325-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="c7325-247">Po użytkownik wysyła podejście funkcji 2FA, `HTTP POST SendCode` metody akcji jest wywoływana, `SignInManager` wysyła kodu funkcji 2FA, a użytkownik jest przekierowywany do `VerifyCode` metody akcji, w którym można wprowadzić kod, aby zakończyć logowanie.</span><span class="sxs-lookup"><span data-stu-id="c7325-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="c7325-248">Funkcji 2FA blokady</span><span class="sxs-lookup"><span data-stu-id="c7325-248">2FA Lockout</span></span>

<span data-ttu-id="c7325-249">Mimo że można ustawić blokady konta na niepowodzenia próby logowania do hasła, takie podejście sprawia, że Twoje dane logowania podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blokady.</span><span class="sxs-lookup"><span data-stu-id="c7325-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="c7325-250">Zaleca się, że używasz blokady konta tylko za pomocą funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="c7325-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="c7325-251">Gdy `ApplicationUserManager` jest tworzony, przykładowy kod ustawia blokady funkcji 2FA i `MaxFailedAccessAttemptsBeforeLockout` do pięciu.</span><span class="sxs-lookup"><span data-stu-id="c7325-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="c7325-252">Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowego), każdy nieudanej próby w funkcji 2FA są przechowywane i w przypadku osiągnięcia maksymalnej liczby prób użytkownik jest zablokowany przez pięć minut (można ustawić blokady razem z `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="c7325-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="c7325-253">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c7325-253">Additional Resources</span></span>

- <span data-ttu-id="c7325-254">[ASP.NET Identity — zalecane zasoby](../getting-started/aspnet-identity-recommended-resources.md) pełną listę tożsamości blogi, pliki wideo, samouczki i great WIĘC łączy.</span><span class="sxs-lookup"><span data-stu-id="c7325-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="c7325-255">[MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) także przedstawiono sposób dodawania informacji o profilu z tabelą użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c7325-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="c7325-256">[ASP.NET MVC i tożsamości w wersji 2.0: Opis podstawowych funkcji](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="c7325-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="c7325-257">Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="c7325-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="c7325-258">Wprowadzenie do systemu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="c7325-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="c7325-259">[Ogłoszenie RTM produktu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez autorem jest Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="c7325-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="c7325-260">[ASP.NET Identity 2.0: Sprawdzanie poprawności konta i autoryzacji Two-Factor Authentication](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="c7325-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>

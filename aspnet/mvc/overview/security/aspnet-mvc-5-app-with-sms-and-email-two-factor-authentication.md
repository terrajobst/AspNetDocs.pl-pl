---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikacja ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym za pomocą wiadomości SMS i wiadomości e-mail | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym. Należy ukończyć tworzenie aplikacji sieci Web Secure ASP.NET MVC 5 za pomocą...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457599"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="e1b92-104">Aplikacja ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym za pomocą wiadomości SMS i wiadomości e-mail</span><span class="sxs-lookup"><span data-stu-id="e1b92-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>

<span data-ttu-id="e1b92-105">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e1b92-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="e1b92-106">W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5 z uwierzytelnianiem dwuskładnikowym.</span><span class="sxs-lookup"><span data-stu-id="e1b92-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="e1b92-107">Aby móc kontynuować [, należy utworzyć aplikację sieci Web z funkcją logowania przy użyciu hasła i potwierdzenia poczty e-mail](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) .</span><span class="sxs-lookup"><span data-stu-id="e1b92-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="e1b92-108">Ukończoną aplikację można pobrać [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="e1b92-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="e1b92-109">Pobieranie zawiera pomocników debugowania, które umożliwiają testowanie potwierdzania wiadomości e-mail i wiadomości SMS bez konieczności konfigurowania dostawcy poczty e-mail lub programu SMS.</span><span class="sxs-lookup"><span data-stu-id="e1b92-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="e1b92-110">Ten samouczek został utworzony przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="e1b92-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

- [<span data-ttu-id="e1b92-111">Tworzenie aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e1b92-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="e1b92-112">Konfigurowanie usługi SMS na potrzeby uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="e1b92-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="e1b92-113">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="e1b92-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="e1b92-114">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e1b92-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="e1b92-115">Tworzenie aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e1b92-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="e1b92-116">Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="e1b92-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="e1b92-117">Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="e1b92-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="e1b92-118">Ostrzeżenie: należy ukończyć [tworzenie bezpiecznej aplikacji sieci Web MVC 5 z logowaniem, potwierdzenie poczty e-mail i resetowanie hasła](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="e1b92-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="e1b92-119">Aby ukończyć ten samouczek, musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="e1b92-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="e1b92-120">Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC.</span><span class="sxs-lookup"><span data-stu-id="e1b92-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="e1b92-121">Formularze sieci Web obsługują również ASP.NET Identity, więc można wykonać podobne kroki w aplikacji formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e1b92-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="e1b92-122">Pozostaw domyślne uwierzytelnianie jako **pojedyncze konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="e1b92-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="e1b92-123">Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="e1b92-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="e1b92-124">W dalszej części samouczka zostanie wdrożona na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="e1b92-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="e1b92-125">Możesz [bezpłatnie otworzyć konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e1b92-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="e1b92-126">Ustaw [dla projektu użycie protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="e1b92-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="e1b92-127">Konfigurowanie usługi SMS na potrzeby uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="e1b92-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="e1b92-128">Ten samouczek zawiera instrukcje dotyczące korzystania z programu Twilio lub ASPSMS, ale można użyć dowolnego innego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="e1b92-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="e1b92-129">**Tworzenie konta użytkownika za pomocą dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="e1b92-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="e1b92-130">Utwórz konto [Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .</span><span class="sxs-lookup"><span data-stu-id="e1b92-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="e1b92-131">**Instalowanie dodatkowych pakietów lub Dodawanie odwołań do usług**</span><span class="sxs-lookup"><span data-stu-id="e1b92-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="e1b92-132">Twilio</span><span class="sxs-lookup"><span data-stu-id="e1b92-132">Twilio:</span></span>  
   <span data-ttu-id="e1b92-133">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="e1b92-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="e1b92-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="e1b92-134">ASPSMS:</span></span>  
   <span data-ttu-id="e1b92-135">Należy dodać następujące odwołanie do usługi:</span><span class="sxs-lookup"><span data-stu-id="e1b92-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="e1b92-136">Ulica</span><span class="sxs-lookup"><span data-stu-id="e1b92-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="e1b92-137">Przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="e1b92-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="e1b92-138">**Rozprowadzenie poświadczeń użytkownika dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="e1b92-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="e1b92-139">Twilio</span><span class="sxs-lookup"><span data-stu-id="e1b92-139">Twilio:</span></span>  
   <span data-ttu-id="e1b92-140">Na karcie **pulpit nawigacyjny** konta usługi Twilio Skopiuj **Identyfikator SID konta** i **token uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="e1b92-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="e1b92-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="e1b92-141">ASPSMS:</span></span>  
   <span data-ttu-id="e1b92-142">W ustawieniach konta przejdź do **userKey** i skopiuj go wraz z własnym określonym **hasłem**.</span><span class="sxs-lookup"><span data-stu-id="e1b92-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="e1b92-143">Te wartości zostaną później zapisane w pliku *Web. config* w kluczach `"SMSAccountIdentification"` i `"SMSAccountPassword"`.</span><span class="sxs-lookup"><span data-stu-id="e1b92-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="e1b92-144">**Określanie SenderID/inicjatora**</span><span class="sxs-lookup"><span data-stu-id="e1b92-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="e1b92-145">Twilio</span><span class="sxs-lookup"><span data-stu-id="e1b92-145">Twilio:</span></span>  
   <span data-ttu-id="e1b92-146">Na karcie **liczby** Skopiuj numer telefonu Twilio.</span><span class="sxs-lookup"><span data-stu-id="e1b92-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="e1b92-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="e1b92-147">ASPSMS:</span></span>  
   <span data-ttu-id="e1b92-148">W menu **odblokowane odblokowywanie** Odblokuj co najmniej jeden inicjator lub wybierz inicjator alfanumeryczny (nieobsługiwany przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="e1b92-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="e1b92-149">Ta wartość zostanie później przechowana w pliku *Web. config* w ramach klucza `"SMSAccountFrom"`.</span><span class="sxs-lookup"><span data-stu-id="e1b92-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="e1b92-150">**Transferowanie poświadczeń dostawcy programu SMS do aplikacji**</span><span class="sxs-lookup"><span data-stu-id="e1b92-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="e1b92-151">Udostępnienie poświadczeń i numeru telefonu nadawcy dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e1b92-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="e1b92-152">Aby zachować prostotę, należy przechowywać te wartości w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="e1b92-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="e1b92-153">Po wdrożeniu na platformie Azure można bezpiecznie przechowywać wartości w sekcji **Ustawienia aplikacji** na karcie Konfiguracja witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e1b92-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="e1b92-154">Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="e1b92-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="e1b92-155">Konto i poświadczenia są dodawane do powyższego kodu, aby zapewnić prostą przykład.</span><span class="sxs-lookup"><span data-stu-id="e1b92-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="e1b92-156">Zapoznaj [się z najlepszymi rozwiązaniami dotyczącymi wdrażania haseł i innych poufnych danych w ASP.NET i na platformie Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="e1b92-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="e1b92-157">**Implementacja transferu danych do dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="e1b92-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="e1b92-158">Skonfiguruj klasę `SmsService` w *aplikacji\_pliku Start\IdentityConfig.cs* .</span><span class="sxs-lookup"><span data-stu-id="e1b92-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="e1b92-159">W zależności od użytego dostawcy programu SMS Aktywuj **Twilio** lub sekcję **ASPSMS** :</span><span class="sxs-lookup"><span data-stu-id="e1b92-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="e1b92-160">Aktualizowanie widoku Razor *Views\Manage\Index.cshtml* : (Uwaga: nie usuwaj po prostu komentarzy w kodzie wyjściowym, użyj poniższego kodu).</span><span class="sxs-lookup"><span data-stu-id="e1b92-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="e1b92-161">Sprawdź, czy `EnableTwoFactorAuthentication` i `DisableTwoFactorAuthentication` metody akcji w `ManageController` mają atrybut[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :</span><span class="sxs-lookup"><span data-stu-id="e1b92-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="e1b92-162">Uruchom aplikację i zaloguj się przy użyciu wcześniej zarejestrowanego konta.</span><span class="sxs-lookup"><span data-stu-id="e1b92-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="e1b92-163">Kliknij swój identyfikator użytkownika, który aktywuje metodę `Index` akcji w kontrolerze `Manage`.</span><span class="sxs-lookup"><span data-stu-id="e1b92-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="e1b92-164">Kliknij pozycję Add (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="e1b92-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="e1b92-165">`AddPhoneNumber` Metoda akcji wyświetla okno dialogowe, w którym można wprowadzić numer telefonu, który może odbierać wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="e1b92-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="e1b92-166">W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="e1b92-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="e1b92-167">Wprowadź ją i naciśnij przycisk **Prześlij**.</span><span class="sxs-lookup"><span data-stu-id="e1b92-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="e1b92-168">Widok zarządzanie pokazuje, że Twój numer telefonu został dodany.</span><span class="sxs-lookup"><span data-stu-id="e1b92-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="e1b92-169">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="e1b92-169">Enable two-factor authentication</span></span>

<span data-ttu-id="e1b92-170">W aplikacji wygenerowanej przez szablon należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (funkcji 2FA).</span><span class="sxs-lookup"><span data-stu-id="e1b92-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="e1b92-171">Aby włączyć funkcji 2FA, kliknij identyfikator użytkownika (alias e-mail) na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="e1b92-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="e1b92-172">Kliknij pozycję Włącz funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="e1b92-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="e1b92-173">Wyloguj się, a następnie zaloguj się ponownie.</span><span class="sxs-lookup"><span data-stu-id="e1b92-173">Logout, then log back in.</span></span> <span data-ttu-id="e1b92-174">Jeśli włączono obsługę poczty e-mail (Zobacz mój [poprzedni samouczek](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomość SMS lub e-mail dla usługi funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="e1b92-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="e1b92-175">Zostanie wyświetlona strona Sprawdź kod, na której można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).</span><span class="sxs-lookup"><span data-stu-id="e1b92-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="e1b92-176">Kliknięcie pola wyboru **Zapamiętaj tę przeglądarkę** spowoduje zwolnienie z używania funkcji 2FA do zalogowania się przy użyciu przeglądarki i urządzenia, w której zaznaczono pole.</span><span class="sxs-lookup"><span data-stu-id="e1b92-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="e1b92-177">Tak długo, jak Złośliwi użytkownicy nie mogą uzyskać dostępu do urządzenia, Włączanie funkcji 2FA i klikanie opcji **Zapamiętaj, że ta przeglądarka** zapewnia wygodny dostęp do jednego kroku, przy zachowaniu silnej ochrony funkcji 2FA w celu uzyskania dostępu z niezaufanych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="e1b92-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="e1b92-178">Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="e1b92-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="e1b92-179">Ten samouczek zawiera krótkie wprowadzenie do włączania funkcji 2FA na nowej aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1b92-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="e1b92-180">Mój samouczek [dwa-Factor Authentication za pomocą wiadomości SMS i wiadomości e-mail z ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) są szczegółowymi informacjami na temat kodu znajdującego się za przykładem.</span><span class="sxs-lookup"><span data-stu-id="e1b92-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="e1b92-181">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e1b92-181">Additional Resources</span></span>

- <span data-ttu-id="e1b92-182">[Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail przy użyciu ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Szczegółowe informacje na temat uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="e1b92-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="e1b92-183">Linki do ASP.NET Identity zalecanych zasobów</span><span class="sxs-lookup"><span data-stu-id="e1b92-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="e1b92-184">[Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Więcej szczegółów na temat odzyskiwania hasła i potwierdzania konta.</span><span class="sxs-lookup"><span data-stu-id="e1b92-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="e1b92-185">[Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) W tym samouczku pokazano, jak napisać aplikację ASP.NET MVC 5 z autoryzacją Facebook i usługą Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="e1b92-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="e1b92-186">Pokazano również, jak dodać dodatkowe dane do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="e1b92-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="e1b92-187">[Wdróż aplikację Secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i SQL Database w sieci Web platformy Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="e1b92-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="e1b92-188">W tym samouczku dodano wdrożenie platformy Azure, sposób zabezpieczania aplikacji za pomocą ról, jak używać interfejsu API członkostwa do dodawania użytkowników i ról oraz dodatkowych funkcji zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="e1b92-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="e1b92-189">Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji z projektem</span><span class="sxs-lookup"><span data-stu-id="e1b92-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="e1b92-190">Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji z projektem</span><span class="sxs-lookup"><span data-stu-id="e1b92-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="e1b92-191">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="e1b92-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="e1b92-192">Jak skonfigurować środowisko deweloperskie C# i ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e1b92-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)

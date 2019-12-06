---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Tworzenie aplikacji sieci Web Secure ASP.NET MVC 5 z logowaniem, potwierdzeniem wiadomości e-mail i resetowaniem hasła (C#) | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć aplikację internetową ASP.NET MVC 5 z potwierdzeniem poczty e-mail i resetowaniem hasła przy użyciu systemu członkostwa ASP.NET Identity. Twój urząd certyfikacji...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 07f5b290b73f75000e6f29fe09e4dc25e144452f
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899694"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="5695e-104">Tworzenie bezpiecznej aplikacji internetowej ASP.NET MVC 5 z logowaniem, potwierdzeniem adresu e-mail i resetowaniem hasła (C#)</span><span class="sxs-lookup"><span data-stu-id="5695e-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>

<span data-ttu-id="5695e-105">Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="5695e-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="5695e-106">W tym samouczku pokazano, jak utworzyć aplikację internetową ASP.NET MVC 5 z potwierdzeniem poczty e-mail i resetowaniem hasła przy użyciu systemu członkostwa ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="5695e-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span>

<span data-ttu-id="5695e-107">Aby uzyskać zaktualizowaną wersję tego samouczka korzystającego z platformy .NET Core, zobacz [potwierdzenie konta i odzyskiwanie hasła w ASP.NET Core [/ASPNET/Core/Security/Authentication/accconfirm).</span><span class="sxs-lookup"><span data-stu-id="5695e-107">For an updated version of this tutorial that uses .NET Core, see [Account confirmation and password recovery in ASP.NET Core[/aspnet/core/security/authentication/accconfirm).</span></span>

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="5695e-108">Tworzenie aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5695e-108">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="5695e-109">Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="5695e-109">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="5695e-110">Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="5695e-110">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="5695e-111">Ostrzeżenie: aby ukończyć ten samouczek, musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="5695e-111">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="5695e-112">Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC.</span><span class="sxs-lookup"><span data-stu-id="5695e-112">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="5695e-113">Formularze sieci Web obsługują również ASP.NET Identity, więc można wykonać podobne kroki w aplikacji formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5695e-113">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="5695e-114">Pozostaw domyślne uwierzytelnianie jako **pojedyncze konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="5695e-114">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="5695e-115">Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="5695e-115">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="5695e-116">W dalszej części samouczka zostanie wdrożona na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="5695e-116">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="5695e-117">Możesz [bezpłatnie otworzyć konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="5695e-117">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="5695e-118">Ustaw [dla projektu użycie protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="5695e-118">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="5695e-119">Uruchom aplikację, kliknij link **zarejestruj** i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5695e-119">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="5695e-120">W tym momencie jedynym sprawdzaniem poprawności w wiadomości e-mail jest atrybut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="5695e-120">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="5695e-121">W Eksplorator serwera przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz polecenie **Otwórz definicję tabeli**.</span><span class="sxs-lookup"><span data-stu-id="5695e-121">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="5695e-122">Na poniższej ilustracji przedstawiono schemat `AspNetUsers`:</span><span class="sxs-lookup"><span data-stu-id="5695e-122">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="5695e-123">Kliknij prawym przyciskiem myszy tabelę **AspNetUsers** , a następnie wybierz pozycję **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="5695e-123">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="5695e-124">W tym momencie wiadomość e-mail nie została potwierdzona.</span><span class="sxs-lookup"><span data-stu-id="5695e-124">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="5695e-125">Kliknij wiersz i wybierz pozycję Usuń.</span><span class="sxs-lookup"><span data-stu-id="5695e-125">Click on the row and select delete.</span></span> <span data-ttu-id="5695e-126">Ponownie dodasz tę wiadomość e-mail w następnym kroku i wyślesz wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="5695e-126">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="5695e-127">Potwierdzenie wiadomości e-mail</span><span class="sxs-lookup"><span data-stu-id="5695e-127">Email confirmation</span></span>

<span data-ttu-id="5695e-128">Najlepszym rozwiązaniem jest potwierdzenie wiadomości e-mail nowej rejestracji użytkownika w celu zweryfikowania, że nie są one personifikowane przez kogoś innego (oznacza to, że nie zostały zarejestrowane w wiadomości e-mail innej osoby).</span><span class="sxs-lookup"><span data-stu-id="5695e-128">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="5695e-129">Załóżmy, że masz forum dyskusyjne, aby zapobiec rejestrowaniu się `"bob@example.com"` w `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="5695e-129">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="5695e-130">Bez potwierdzenia wiadomości e-mail `"joe@contoso.com"` może pobrać niechcianą wiadomość e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5695e-130">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="5695e-131">Załóżmy, że Robert przypadkowo zarejestrowany jako `"bib@example.com"` i nie go, nie będzie mógł korzystać z odzyskiwania hasła, ponieważ aplikacja nie ma poprawnego adresu e-mail.</span><span class="sxs-lookup"><span data-stu-id="5695e-131">Suppose Bob accidentally registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="5695e-132">Potwierdzenie poczty e-mail zapewnia tylko ograniczoną ochronę z botów i nie zapewnia ochrony przed określonymi nadawców, którzy mają wiele roboczych aliasów poczty e-mail, których mogą używać do rejestracji.</span><span class="sxs-lookup"><span data-stu-id="5695e-132">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="5695e-133">Zazwyczaj chcesz uniemożliwić nowym użytkownikom ogłaszanie danych w witrynie sieci Web przed ich potwierdzeniem za pośrednictwem poczty e-mail, wiadomości SMS lub innego mechanizmu.</span><span class="sxs-lookup"><span data-stu-id="5695e-133">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="5695e-134">W poniższych sekcjach zostanie włączone potwierdzenie poczty e-mail i zmodyfikujesz kod, aby uniemożliwić nowo zarejestrowanym użytkownikom logowanie się do momentu potwierdzenia potwierdzenia wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="5695e-134">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="5695e-135">Podłącz do SendGrid</span><span class="sxs-lookup"><span data-stu-id="5695e-135">Hook up SendGrid</span></span>

<span data-ttu-id="5695e-136">Instrukcje w tej sekcji nie są aktualne.</span><span class="sxs-lookup"><span data-stu-id="5695e-136">The instructions in this section are not current.</span></span> <span data-ttu-id="5695e-137">Aby uzyskać zaktualizowane instrukcje, zobacz [Konfigurowanie dostawcy poczty E-mail SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) .</span><span class="sxs-lookup"><span data-stu-id="5695e-137">See [Configure SendGrid email provider](/aspnet/core/security/authentication/accconfirm#configure-email-provider) for updated instructions.</span></span>

<span data-ttu-id="5695e-138">Chociaż w tym samouczku pokazano, jak dodać powiadomienie e-mail za pośrednictwem usługi [SendGrid](http://sendgrid.com/), możesz wysłać wiadomość e-mail przy użyciu protokołu SMTP i innych mechanizmów (zobacz [dodatkowe zasoby](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="5695e-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="5695e-139">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="5695e-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="5695e-140">Przejdź do [strony rejestracji w usłudze Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) i zarejestruj się, aby skorzystać z bezpłatnego konta SendGrid.</span><span class="sxs-lookup"><span data-stu-id="5695e-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="5695e-141">Skonfiguruj SendGrid przez dodanie kodu podobnego do poniższego w *App_Start/identityconfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="5695e-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="5695e-142">Należy dodać następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="5695e-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="5695e-143">Aby zachować ten przykład, zapiszemy ustawienia aplikacji w pliku *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="5695e-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="5695e-144">Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="5695e-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="5695e-145">Konto i poświadczenia są przechowywane w element appSetting.</span><span class="sxs-lookup"><span data-stu-id="5695e-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="5695e-146">Na platformie Azure możesz bezpiecznie przechowywać te wartości na karcie **[Konfiguracja](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5695e-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="5695e-147">Zapoznaj [się z najlepszymi rozwiązaniami dotyczącymi wdrażania haseł i innych poufnych danych w ASP.NET i na platformie Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="5695e-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>

### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="5695e-148">Włącz potwierdzenie poczty e-mail na kontrolerze konta</span><span class="sxs-lookup"><span data-stu-id="5695e-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="5695e-149">Sprawdź, czy plik *Views\Account\ConfirmEmail.cshtml* ma poprawną składnię Razor.</span><span class="sxs-lookup"><span data-stu-id="5695e-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="5695e-150">(Znak @ w pierwszym wierszu może być niedostępny.</span><span class="sxs-lookup"><span data-stu-id="5695e-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="5695e-151">).</span><span class="sxs-lookup"><span data-stu-id="5695e-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="5695e-152">Uruchom aplikację i kliknij link Zarejestruj.</span><span class="sxs-lookup"><span data-stu-id="5695e-152">Run the app and click the Register link.</span></span> <span data-ttu-id="5695e-153">Po przesłaniu formularza rejestracji użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="5695e-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="5695e-154">Sprawdź konto e-mail i kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="5695e-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="5695e-155">Wymagaj potwierdzenia wiadomości e-mail przed zalogowaniem</span><span class="sxs-lookup"><span data-stu-id="5695e-155">Require email confirmation before log in</span></span>

<span data-ttu-id="5695e-156">Obecnie po zakończeniu procesu rejestracji użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="5695e-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="5695e-157">Zazwyczaj chcesz potwierdzić swój adres e-mail przed zarejestrowaniem ich w usłudze.</span><span class="sxs-lookup"><span data-stu-id="5695e-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="5695e-158">W poniższej sekcji zmodyfikujemy kod, aby wymagać od nowych użytkowników potwierdzenia wiadomości e-mail przed ich zalogowaniem się (uwierzytelniony).</span><span class="sxs-lookup"><span data-stu-id="5695e-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="5695e-159">Zaktualizuj metodę `HttpPost Register` przy użyciu następujących wyróżnionych zmian:</span><span class="sxs-lookup"><span data-stu-id="5695e-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="5695e-160">Poprzez komentowanie metody `SignInAsync` użytkownik nie będzie zalogowany przez rejestrację.</span><span class="sxs-lookup"><span data-stu-id="5695e-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="5695e-161">Wiersz `TempData["ViewBagLink"] = callbackUrl;` może służyć do [debugowania aplikacji](#dbg) i testowania rejestracji bez wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="5695e-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="5695e-162">`ViewBag.Message` jest używany do wyświetlania instrukcji Confirm.</span><span class="sxs-lookup"><span data-stu-id="5695e-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="5695e-163">[Przykład pobierania](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) zawiera kod służący do testowania potwierdzenia wiadomości e-mail bez konfigurowania poczty e-mail i może być również używany do debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5695e-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="5695e-164">Utwórz plik `Views\Shared\Info.cshtml` i Dodaj następujący znacznik Razor:</span><span class="sxs-lookup"><span data-stu-id="5695e-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="5695e-165">Dodaj [atrybut Autoryzuj](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) do metody akcji `Contact` kontrolera głównego.</span><span class="sxs-lookup"><span data-stu-id="5695e-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="5695e-166">Możesz kliknąć link **kontakt** , aby sprawdzić, czy użytkownicy anonimowi nie mają dostępu, a uwierzytelniony użytkownicy mają dostęp.</span><span class="sxs-lookup"><span data-stu-id="5695e-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="5695e-167">Należy również zaktualizować metodę `HttpPost Login` akcji:</span><span class="sxs-lookup"><span data-stu-id="5695e-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="5695e-168">Zaktualizuj widok *Views\Shared\Error.cshtml* , aby wyświetlić komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="5695e-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="5695e-169">Usuń wszystkie konta z tabeli **AspNetUsers** zawierające alias adresu e-mail, z którym chcesz przeprowadzić test.</span><span class="sxs-lookup"><span data-stu-id="5695e-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="5695e-170">Uruchom aplikację i sprawdź, czy nie możesz się zalogować, dopóki nie potwierdzisz adresu e-mail.</span><span class="sxs-lookup"><span data-stu-id="5695e-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="5695e-171">Po potwierdzeniu adresu e-mail kliknij link **kontaktowy** .</span><span class="sxs-lookup"><span data-stu-id="5695e-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="5695e-172">Odzyskiwanie/Resetowanie hasła</span><span class="sxs-lookup"><span data-stu-id="5695e-172">Password recovery/reset</span></span>

<span data-ttu-id="5695e-173">Usuń znaki komentarza z `HttpPost ForgotPassword`ej metody akcji na kontrolerze konta:</span><span class="sxs-lookup"><span data-stu-id="5695e-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="5695e-174">Usuń znaki komentarza z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) w pliku widoku Razor *Views\Account\Login.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5695e-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="5695e-175">Na stronie logowania zostanie teraz wyświetlony link służący do resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="5695e-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="5695e-176">Wyślij ponownie link do potwierdzenia wiadomości e-mail</span><span class="sxs-lookup"><span data-stu-id="5695e-176">Resend email confirmation link</span></span>

<span data-ttu-id="5695e-177">Po utworzeniu nowego konta lokalnego przez użytkownika są wysyłane pocztą e-mail link potwierdzający, którego należy użyć przed zalogowaniem się.</span><span class="sxs-lookup"><span data-stu-id="5695e-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="5695e-178">Jeśli użytkownik przypadkowo usunie wiadomość e-mail z potwierdzeniem lub wiadomość e-mail nie dotarła do Ciebie, będzie potrzebować linku potwierdzenia wysłanej ponownie.</span><span class="sxs-lookup"><span data-stu-id="5695e-178">If the user accidentally deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="5695e-179">Poniższe zmiany kodu pokazują, jak to włączyć.</span><span class="sxs-lookup"><span data-stu-id="5695e-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="5695e-180">Dodaj następującą metodę pomocnika na końcu pliku *Controllers\AccountController.cs* :</span><span class="sxs-lookup"><span data-stu-id="5695e-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="5695e-181">Zaktualizuj metodę Register, aby korzystała z nowego pomocnika:</span><span class="sxs-lookup"><span data-stu-id="5695e-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="5695e-182">Zaktualizuj metodę logowania, aby ponownie wysłać hasło, jeśli konto użytkownika nie zostało potwierdzone:</span><span class="sxs-lookup"><span data-stu-id="5695e-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="5695e-183">Łączenie kont logowania społecznościowych i lokalnych</span><span class="sxs-lookup"><span data-stu-id="5695e-183">Combine social and local login accounts</span></span>

<span data-ttu-id="5695e-184">Konta lokalne i społecznościowe można łączyć przez kliknięcie linku do poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="5695e-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="5695e-185">W następującej kolejności **RickAndMSFT@gmail.com** jest najpierw tworzony jako logowanie lokalne, ale w pierwszej kolejności można utworzyć konto w trybie społecznościowym, a następnie dodać lokalną nazwę logowania.</span><span class="sxs-lookup"><span data-stu-id="5695e-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="5695e-186">Kliknij link **Zarządzaj** .</span><span class="sxs-lookup"><span data-stu-id="5695e-186">Click on the **Manage** link.</span></span> <span data-ttu-id="5695e-187">Zwróć uwagę na **zewnętrzne nazwy logowania: 0** skojarzonych z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="5695e-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="5695e-188">Kliknij link do innej usługi logowania i zaakceptuj żądania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5695e-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="5695e-189">Te dwa konta zostały połączone, ale będzie można zalogować się przy użyciu dowolnego konta.</span><span class="sxs-lookup"><span data-stu-id="5695e-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="5695e-190">Możesz chcieć, aby użytkownicy mogli dodawać konta lokalne w przypadku, gdy ich usługa uwierzytelniania w sieci społecznościowej nie działa, lub prawdopodobnie utraciły dostęp do konta społecznościowego.</span><span class="sxs-lookup"><span data-stu-id="5695e-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="5695e-191">Na poniższej ilustracji, Tomasz to logowanie społecznościowe (które można zobaczyć od **zewnętrznych logowań: 1** pokazane na stronie).</span><span class="sxs-lookup"><span data-stu-id="5695e-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="5695e-192">Kliknięcie przycisku **Wybierz hasło** umożliwia dodanie lokalnego logowania skojarzonego z tym samym kontem.</span><span class="sxs-lookup"><span data-stu-id="5695e-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="5695e-193">Bardziej szczegółowe potwierdzenie wiadomości e-mail</span><span class="sxs-lookup"><span data-stu-id="5695e-193">Email confirmation in more depth</span></span>

<span data-ttu-id="5695e-194">W tym temacie znajdują się informacje o [potwierdzeniu konta i odzyskiwaniu hasła z użyciem ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) .</span><span class="sxs-lookup"><span data-stu-id="5695e-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="5695e-195">Debugowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5695e-195">Debugging the app</span></span>

<span data-ttu-id="5695e-196">Jeśli nie otrzymasz wiadomości e-mail zawierającej link:</span><span class="sxs-lookup"><span data-stu-id="5695e-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="5695e-197">Sprawdź folder wiadomości-śmieci lub spamu.</span><span class="sxs-lookup"><span data-stu-id="5695e-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="5695e-198">Zaloguj się do konta SendGrid, a następnie kliknij [link działania e-mail](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="5695e-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="5695e-199">Aby przetestować łącze weryfikacji bez poczty e-mail, Pobierz [ukończony przykład](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="5695e-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="5695e-200">Na stronie zostanie wyświetlony link potwierdzenia i kody potwierdzające.</span><span class="sxs-lookup"><span data-stu-id="5695e-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="5695e-201">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="5695e-201">Additional Resources</span></span>

- [<span data-ttu-id="5695e-202">Linki do ASP.NET Identity zalecanych zasobów</span><span class="sxs-lookup"><span data-stu-id="5695e-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="5695e-203">[Potwierdzenie konta i odzyskiwanie hasła przy użyciu ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Więcej szczegółów na temat odzyskiwania hasła i potwierdzania konta.</span><span class="sxs-lookup"><span data-stu-id="5695e-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="5695e-204">[Aplikacja MVC 5 z logowaniem w serwisach Facebook, Twitter, LinkedIn i Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) W tym samouczku pokazano, jak napisać aplikację ASP.NET MVC 5 z autoryzacją Facebook i usługą Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="5695e-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="5695e-205">Pokazano również, jak dodać dodatkowe dane do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="5695e-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="5695e-206">[Wdróż aplikację Secure ASP.NET MVC z członkostwem, uwierzytelnianiem OAuth i SQL Database na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="5695e-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="5695e-207">W tym samouczku dodano wdrożenie platformy Azure, sposób zabezpieczania aplikacji za pomocą ról, jak używać interfejsu API członkostwa do dodawania użytkowników i ról oraz dodatkowych funkcji zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="5695e-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="5695e-208">Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji z projektem</span><span class="sxs-lookup"><span data-stu-id="5695e-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="5695e-209">Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji z projektem</span><span class="sxs-lookup"><span data-stu-id="5695e-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="5695e-210">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="5695e-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)

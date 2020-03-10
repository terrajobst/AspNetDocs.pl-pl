---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Logowanie przy użyciu witryn zewnętrznych w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn — czyli sposobu obsługi...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638755"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="10128-103">Logowanie przy użyciu witryn zewnętrznych w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="10128-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="10128-104">Autor [FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="10128-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="10128-105">W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn — czyli sposobu obsługi uwierzytelniania OAuth i OpenID Connect w lokacji.</span><span class="sxs-lookup"><span data-stu-id="10128-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="10128-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="10128-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="10128-107">Jak włączyć logowanie z innych lokacji przy użyciu szablonu witryny WebMatrix Starter.</span><span class="sxs-lookup"><span data-stu-id="10128-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="10128-108">Jest to funkcja ASP.NET wprowadzona w artykule:</span><span class="sxs-lookup"><span data-stu-id="10128-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="10128-109">Pomocnik `OAuthWebSecurity`.</span><span class="sxs-lookup"><span data-stu-id="10128-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="10128-110">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="10128-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="10128-111">ASP.NET strony sieci Web (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="10128-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="10128-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="10128-112">WebMatrix 3</span></span>

<span data-ttu-id="10128-113">Strony sieci Web ASP.NET obejmują obsługę dostawców [OAuth](http://oauth.net/) i [OpenID Connect](http://openid.net/) .</span><span class="sxs-lookup"><span data-stu-id="10128-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="10128-114">Korzystając z tych dostawców, można zezwolić użytkownikom na logowanie się do swojej witryny przy użyciu istniejących poświadczeń z serwisu Facebook, Twitter, Microsoft i Google.</span><span class="sxs-lookup"><span data-stu-id="10128-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="10128-115">Na przykład, aby zalogować się przy użyciu konta w serwisie Facebook, użytkownicy mogą po prostu wybrać ikonę w serwisie Facebook, która przekierowuje je do strony logowania w serwisie Facebook, gdzie wprowadzają informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="10128-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="10128-116">Następnie mogą skojarzyć logowanie w serwisie Facebook ze swoimi kontami w swojej witrynie.</span><span class="sxs-lookup"><span data-stu-id="10128-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="10128-117">Powiązane ulepszenie funkcji członkostwa stron sieci Web polega na tym, że użytkownicy mogą kojarzyć wiele nazw logowania (w tym logowania z witryn sieci społecznościowych) przy użyciu jednego konta w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="10128-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="10128-118">Ten obraz przedstawia stronę logowania z szablonu **witryny startowej** , w którym użytkownik może wybrać ikonę Facebook, Twitter, Google lub Microsoft, aby włączyć logowanie przy użyciu konta zewnętrznego:</span><span class="sxs-lookup"><span data-stu-id="10128-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![dostawcy zewnętrzni](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="10128-120">Można włączyć członkowstwo OAuth i OpenID Connect przez usunięcie komentarza do kilku wierszy kodu w szablonie **witryny Starter** .</span><span class="sxs-lookup"><span data-stu-id="10128-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="10128-121">Metody i właściwości używane do pracy z dostawcami OAuth i OpenID Connect znajdują się w klasie `WebMatrix.Security.OAuthWebSecurity`.</span><span class="sxs-lookup"><span data-stu-id="10128-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="10128-122">Szablon **witryny Starter** obejmuje pełną infrastrukturę członkostwa, kompletną stronę logowania, bazę danych członkostwa i cały kod, który jest potrzebny użytkownikom do logowania się do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji.</span><span class="sxs-lookup"><span data-stu-id="10128-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="10128-123">Ta sekcja zawiera przykład sposobu pozwalania użytkownikom na logowanie się z witryn zewnętrznych do witryny opartej na szablonie **witryny Starter** .</span><span class="sxs-lookup"><span data-stu-id="10128-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="10128-124">Po utworzeniu witryny Starter należy wykonać tę czynność (szczegóły znajdują się poniżej):</span><span class="sxs-lookup"><span data-stu-id="10128-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="10128-125">W przypadku witryn korzystających z dostawcy OAuth (Facebook, Twitter i Microsoft) utworzysz aplikację w witrynie zewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="10128-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="10128-126">Dzięki temu klucze aplikacji będą potrzebne do wywołania funkcji logowania dla tych lokacji.</span><span class="sxs-lookup"><span data-stu-id="10128-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="10128-127">W przypadku witryn korzystających z dostawcy OpenID Connect (Google) nie ma potrzeby tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10128-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="10128-128">W przypadku wszystkich tych witryn musisz mieć konto, aby zalogować się i utworzyć aplikacje dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="10128-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10128-129">Aplikacje firmy Microsoft akceptują adres URL na żywo dla działającej witryny sieci Web, więc nie można używać adresu URL lokalnego witryny sieci Web do testowania logowania.</span><span class="sxs-lookup"><span data-stu-id="10128-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="10128-130">Edytuj kilka plików w witrynie sieci Web, aby określić odpowiedniego dostawcę uwierzytelniania i przesłać dane logowania do witryny, której chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="10128-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="10128-131">Ten artykuł zawiera oddzielne instrukcje dotyczące następujących zadań:</span><span class="sxs-lookup"><span data-stu-id="10128-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="10128-132">Włączanie logowania Google</span><span class="sxs-lookup"><span data-stu-id="10128-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="10128-133">Włączanie logowania do usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="10128-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="10128-134">Włączanie logowania do usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="10128-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="10128-135">Włączanie logowania Google</span><span class="sxs-lookup"><span data-stu-id="10128-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="10128-136">Utwórz lub Otwórz witrynę ASP.NET Web Pages, która jest oparta na szablonie witryny WebMatrix Starter.</span><span class="sxs-lookup"><span data-stu-id="10128-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="10128-137">Otwórz stronę *\_AppStart. cshtml* i Usuń komentarz z poniższego wiersza kodu.</span><span class="sxs-lookup"><span data-stu-id="10128-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="10128-138">Testowanie logowania Google</span><span class="sxs-lookup"><span data-stu-id="10128-138">Testing Google login</span></span>

1. <span data-ttu-id="10128-139">Uruchom *domyślną stronę. cshtml* witryny i wybierz przycisk **Zaloguj się** .</span><span class="sxs-lookup"><span data-stu-id="10128-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="10128-140">Na stronie *Logowanie* w sekcji Korzystanie z **innej usługi do logowania** wybierz pozycję **Google** lub **Yahoo** Submit (Prześlij).</span><span class="sxs-lookup"><span data-stu-id="10128-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="10128-141">W tym przykładzie używamy logowania Google.</span><span class="sxs-lookup"><span data-stu-id="10128-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="10128-142">Strona sieci Web przekierowuje żądanie do strony logowania Google.</span><span class="sxs-lookup"><span data-stu-id="10128-142">The web page redirects the request to the Google login page.</span></span>

    ![Logowanie Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="10128-144">Wprowadź poświadczenia dla istniejącego konta Google.</span><span class="sxs-lookup"><span data-stu-id="10128-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="10128-145">Jeśli zostanie wyświetlony monit z pytaniem, czy chcesz zezwolić na używanie przez *hosta lokalnego* informacji z konta, kliknij przycisk **Zezwalaj**.</span><span class="sxs-lookup"><span data-stu-id="10128-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="10128-146">Kod używa tokenu Google do uwierzytelniania użytkownika, a następnie wraca do tej strony w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="10128-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="10128-147">Ta strona umożliwia użytkownikom kojarzenie ich logowania Google z istniejącym kontem w witrynie sieci Web lub rejestrowanie nowego konta w lokacji w celu skojarzenia logowania zewnętrznego z.</span><span class="sxs-lookup"><span data-stu-id="10128-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="10128-149">Wybierz przycisk **Skojarz** .</span><span class="sxs-lookup"><span data-stu-id="10128-149">Choose the **Associate** button.</span></span> <span data-ttu-id="10128-150">Przeglądarka powraca do strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10128-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="10128-151">Włączanie logowania do usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="10128-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="10128-152">Przejdź do [witryny deweloperów serwisu Facebook](https://developers.facebook.com/apps) (Zaloguj się, jeśli jeszcze nie jest zalogowany).</span><span class="sxs-lookup"><span data-stu-id="10128-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="10128-153">Wybierz przycisk **Utwórz nową aplikację** , a następnie postępuj zgodnie z monitami, aby nazwa i utworzyć nową aplikację.</span><span class="sxs-lookup"><span data-stu-id="10128-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="10128-154">W sekcji Wybierz, w **jaki sposób aplikacja będzie integrowana z usługą Facebook**, wybierz sekcję **Witryna sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="10128-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="10128-155">Wprowadź wartość w polu **adres URL witryny** , podając adres URL witryny (na przykład `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="10128-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="10128-156">Pole **domena** jest opcjonalne; służy do zapewnienia uwierzytelniania dla całej domeny (na przykład *example.com*).</span><span class="sxs-lookup"><span data-stu-id="10128-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="10128-157">Jeśli na komputerze lokalnym jest uruchomiona witryna z adresem URL, takim jak `http://localhost:12345` (gdzie numer jest numerem portu lokalnego), można dodać tę wartość do pola **adres URL witryny** w celu przetestowania lokacji.</span><span class="sxs-lookup"><span data-stu-id="10128-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="10128-158">Jednak w przypadku zmiany numeru portu lokacji lokalnej należy zaktualizować pole **adres URL witryny** aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10128-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="10128-159">Wybierz przycisk **Zapisz zmiany** .</span><span class="sxs-lookup"><span data-stu-id="10128-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="10128-160">Ponownie wybierz kartę **aplikacje** , a następnie Wyświetl stronę początkową swojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10128-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="10128-161">Skopiuj wartości **Identyfikator aplikacji** i **klucz tajny** aplikacji dla aplikacji i wklej je do tymczasowego pliku tekstowego.</span><span class="sxs-lookup"><span data-stu-id="10128-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="10128-162">Te wartości zostaną przekazane do dostawcy usługi Facebook w kodzie witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="10128-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="10128-163">Zamknij witrynę dewelopera serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="10128-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="10128-164">Teraz wprowadzono zmiany w dwóch stronach w witrynie sieci Web, dzięki czemu użytkownicy będą mogli logować się do witryny przy użyciu swoich kont w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="10128-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="10128-165">Utwórz lub Otwórz witrynę ASP.NET Web Pages, która jest oparta na szablonie witryny WebMatrix Starter.</span><span class="sxs-lookup"><span data-stu-id="10128-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="10128-166">Otwórz stronę *\_AppStart. cshtml* i usuń znaczniki komentarza z kodu dla dostawcy protokołu OAuth w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="10128-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="10128-167">Niekomentowany blok kodu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="10128-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="10128-168">Skopiuj wartość **identyfikatora aplikacji** z aplikacji Facebook jako wartość parametru `appId` (wewnątrz cudzysłowów).</span><span class="sxs-lookup"><span data-stu-id="10128-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="10128-169">Skopiuj wartość **klucza tajnego aplikacji** z aplikacji Facebook jako wartość parametru `appSecret`.</span><span class="sxs-lookup"><span data-stu-id="10128-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="10128-170">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="10128-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="10128-171">Testowanie logowania w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="10128-171">Testing Facebook login</span></span>

1. <span data-ttu-id="10128-172">Uruchom *domyślną stronę. cshtml* witryny, a następnie wybierz przycisk **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="10128-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="10128-173">Na stronie *Logowanie* w sekcji Korzystanie z **innej usługi do logowania** wybierz ikonę **Facebook** .</span><span class="sxs-lookup"><span data-stu-id="10128-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="10128-174">Strona sieci Web przekierowuje żądanie do strony logowania w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="10128-174">The web page redirects the request to the Facebook login page.</span></span>

    ![Uwierzytelnianie OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="10128-176">Zaloguj się do konta w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="10128-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="10128-177">Kod używa tokenu Facebook do uwierzytelniania użytkownika, a następnie wraca do strony, na której można skojarzyć dane logowania w serwisie Facebook z logowaniem do witryny.</span><span class="sxs-lookup"><span data-stu-id="10128-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="10128-178">Nazwa użytkownika lub adres e-mail są wypełniane w polu **adres e-mail** w formularzu.</span><span class="sxs-lookup"><span data-stu-id="10128-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="10128-180">Wybierz przycisk **Skojarz** .</span><span class="sxs-lookup"><span data-stu-id="10128-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="10128-181">Przeglądarka powraca do strony głównej, a użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="10128-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="10128-182">Włączanie logowania do usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="10128-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="10128-183">Przejdź do [witryny Twitter Developers](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="10128-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="10128-184">Wybierz łącze **Utwórz aplikację** , a następnie zaloguj się do lokacji.</span><span class="sxs-lookup"><span data-stu-id="10128-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="10128-185">W formularzu **Utwórz aplikację** Wypełnij pola **Nazwa** i **Opis** .</span><span class="sxs-lookup"><span data-stu-id="10128-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="10128-186">W polu **Witryna sieci Web** wprowadź adres URL witryny (na przykład `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="10128-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="10128-187">Jeśli testujesz swoją lokację lokalnie (przy użyciu adresu URL, takiego jak `http://localhost:12345`), serwis Twitter może nie zaakceptować adresu URL.</span><span class="sxs-lookup"><span data-stu-id="10128-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="10128-188">Może jednak być możliwe użycie lokalnego adresu IP sprzężenia zwrotnego (na przykład `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="10128-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="10128-189">Upraszcza to proces testowania aplikacji lokalnie.</span><span class="sxs-lookup"><span data-stu-id="10128-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="10128-190">Jednak za każdym razem, gdy zmieniany jest numer portu lokacji lokalnej, należy zaktualizować pole **Witryna sieci Web** aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10128-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="10128-191">W polu **adres URL wywołania zwrotnego** wprowadź adres URL strony w witrynie sieci Web, do której użytkownicy będą mogli powrócić po zalogowaniu się do usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="10128-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="10128-192">Na przykład w celu wysłania użytkowników do strony głównej witryny startowej (która będzie rozpoznawać swój stan logowania) wprowadź ten sam adres URL wprowadzony w polu **Witryna sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="10128-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="10128-193">Zaakceptuj warunki i wybierz przycisk **Utwórz aplikację w usłudze Twitter** .</span><span class="sxs-lookup"><span data-stu-id="10128-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="10128-194">Na stronie spocznik **Moje aplikacje** Wybierz utworzoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="10128-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="10128-195">Na karcie **szczegóły** przewiń w dół i wybierz przycisk **Utwórz token dostępu** .</span><span class="sxs-lookup"><span data-stu-id="10128-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="10128-196">Na karcie **szczegóły** Skopiuj wartości **klucza klienta** i **wpisu tajnego konsumenta** dla aplikacji i wklej je w tymczasowym pliku tekstowym.</span><span class="sxs-lookup"><span data-stu-id="10128-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="10128-197">Te wartości zostaną przekazane do dostawcy usługi Twitter w kodzie witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="10128-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="10128-198">Zamknij witrynę usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="10128-198">Exit the Twitter site.</span></span>

<span data-ttu-id="10128-199">Teraz wprowadzono zmiany w dwóch stronach w witrynie sieci Web, dzięki czemu użytkownicy będą mogli logować się do witryny przy użyciu kont w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="10128-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="10128-200">Utwórz lub Otwórz witrynę ASP.NET Web Pages, która jest oparta na szablonie witryny WebMatrix Starter.</span><span class="sxs-lookup"><span data-stu-id="10128-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="10128-201">Otwórz stronę *\_AppStart. cshtml* i usuń znaczniki komentarza z kodu dla dostawcy protokołu OAuth w serwisie Twitter.</span><span class="sxs-lookup"><span data-stu-id="10128-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="10128-202">Niekomentowany blok kodu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="10128-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="10128-203">Skopiuj wartość **klucza klienta** z aplikacji Twitter jako wartość parametru `consumerKey` (wewnątrz cudzysłowów).</span><span class="sxs-lookup"><span data-stu-id="10128-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="10128-204">Skopiuj wartość **klucza tajnego klienta** z aplikacji Twitter jako wartość parametru `consumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="10128-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="10128-205">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="10128-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="10128-206">Testowanie logowania do usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="10128-206">Testing Twitter login</span></span>

1. <span data-ttu-id="10128-207">Uruchom stronę *default. cshtml* w witrynie i wybierz przycisk **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="10128-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="10128-208">Na stronie *Logowanie* w sekcji Korzystanie z **innej usługi do logowania** wybierz ikonę **Twitter** .</span><span class="sxs-lookup"><span data-stu-id="10128-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="10128-209">Strona sieci Web przekierowuje żądanie do strony logowania w usłudze Twitter dla utworzonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="10128-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="10128-211">Zaloguj się do konta usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="10128-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="10128-212">Kod używa tokenu usługi Twitter do uwierzytelniania użytkownika, a następnie wraca do strony, na której można skojarzyć dane logowania z kontem witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="10128-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="10128-213">Nazwa lub adres e-mail są wypełniane w polu **adres e-mail** w formularzu.</span><span class="sxs-lookup"><span data-stu-id="10128-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="10128-215">Wybierz przycisk **Skojarz** .</span><span class="sxs-lookup"><span data-stu-id="10128-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="10128-216">Przeglądarka powraca do strony głównej, a użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="10128-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="10128-217">Dodatkowe materiały</span><span class="sxs-lookup"><span data-stu-id="10128-217">Additional Resources</span></span>

- [<span data-ttu-id="10128-218">Dostosowywanie zachowania dla całej witryny</span><span class="sxs-lookup"><span data-stu-id="10128-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="10128-219">Dodawanie zabezpieczeń i członkostwa do witryny ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="10128-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)

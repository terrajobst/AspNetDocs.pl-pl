---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Logowanie się przy użyciu zewnętrznych witryn we wzorcu ASP.NET Web Pages lokacji (Razor) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) przy użyciu usługi Facebook, Google, Twitter, Yahoo i innych witryn — oznacza to, jak obsługiwać...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65124160"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="16606-103">Logowanie się przy użyciu zewnętrznych witryn w witrynie ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="16606-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="16606-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="16606-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="16606-105">W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) przy użyciu usługi Facebook, Google, Twitter, Yahoo i innych witryn — oznacza to, jak obsługiwać OAuth i OpenID w lokacji.</span><span class="sxs-lookup"><span data-stu-id="16606-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="16606-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="16606-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="16606-107">Jak włączyć logowania z innych witryn, w przypadku użycia szablonu witryny początkowej programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="16606-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="16606-108">Jest to ASP.NET wprowadzonej w artykule:</span><span class="sxs-lookup"><span data-stu-id="16606-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="16606-109">`OAuthWebSecurity` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="16606-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="16606-110">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="16606-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="16606-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="16606-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="16606-112">Program WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="16606-112">WebMatrix 3</span></span>

<span data-ttu-id="16606-113">ASP.NET Web Pages obsługuje [OAuth](http://oauth.net/) i [OpenID](http://openid.net/) dostawców.</span><span class="sxs-lookup"><span data-stu-id="16606-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="16606-114">Korzystając z tych dostawców, można pozwolić użytkownikom logowanie do witryny przy użyciu istniejących poświadczeń usług Facebook, Twitter, Microsoft i Google.</span><span class="sxs-lookup"><span data-stu-id="16606-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="16606-115">Na przykład aby zalogować się przy użyciu konta w serwisie Facebook, użytkowników po prostu wybrać ikonę usługi Facebook, który przekierowuje go do strony logowania usługi Facebook, gdzie użytkownik podał ich informacji o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="16606-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="16606-116">Można następnie skojarzyć logowania do usługi Facebook przy użyciu swojego konta w witrynie.</span><span class="sxs-lookup"><span data-stu-id="16606-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="16606-117">Powiązane rozszerzenie do funkcji przynależności stron sieci Web jest czy użytkownicy mogą powiązać logowania (w tym logowania z witrynami sieci społecznościowych) za pomocą jednego konta w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16606-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="16606-118">Ta ilustracja przedstawia strony logowania z **witryny początkowej** szablonu, w którym użytkownik może wybrać ikonę usługi Facebook, Twitter, Google i Microsoft, aby włączyć logowanie przy użyciu zewnętrznego konta:</span><span class="sxs-lookup"><span data-stu-id="16606-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![dostawców zewnętrznych](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="16606-120">Aby umożliwić członkostwa OAuth i OpenID, uncommenting kilku wierszy kodu w **witryny początkowej** szablonu.</span><span class="sxs-lookup"><span data-stu-id="16606-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="16606-121">Metody i właściwości można używać do pracy z uwierzytelniania OAuth i OpenID dostawców znajdują się w `WebMatrix.Security.OAuthWebSecurity` klasy.</span><span class="sxs-lookup"><span data-stu-id="16606-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="16606-122">**Witryny początkowej** szablon zawiera następujące elementy infrastruktury pełnego członkostwa, ze strony logowania, bazy danych członkostwa i cały kod należy umożliwić użytkownikom logowanie do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji .</span><span class="sxs-lookup"><span data-stu-id="16606-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="16606-123">Ta sekcja zawiera przykładowy sposób zezwolić użytkownikom na logowanie z zewnętrznych witryn, witryny, która opiera się na **witryny początkowej** szablonu.</span><span class="sxs-lookup"><span data-stu-id="16606-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="16606-124">Po utworzeniu witryny początkowej, należy wykonać ten (szczegóły poniżej):</span><span class="sxs-lookup"><span data-stu-id="16606-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="16606-125">Dla witryn, które korzystają z dostawcy uwierzytelniania OAuth (Facebook, Twitter i Microsoft) utworzyć aplikację w witrynie zewnętrznej.</span><span class="sxs-lookup"><span data-stu-id="16606-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="16606-126">Dzięki temu klucze aplikacji, które należy w celu wywołania funkcji logowania dla tych witryn.</span><span class="sxs-lookup"><span data-stu-id="16606-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="16606-127">Dla witryn, które korzystają z dostawcy openid o nazwie (Google) nie masz do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16606-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="16606-128">Dla wszystkich tych lokacji musi mieć konto, aby się zalogować i tworzenie aplikacji dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="16606-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="16606-129">Aplikacje firmy Microsoft Zaakceptuj, tylko adres URL na żywo roboczej witryny sieci Web, adres URL lokalną witrynę sieci Web nie można używać do testowania nazwy logowania.</span><span class="sxs-lookup"><span data-stu-id="16606-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="16606-130">Edytuj kilka plików w witrynie sieci Web, aby określić dostawcę uwierzytelniania i przesłanie logowania do witryny, do której chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="16606-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="16606-131">Ten artykuł zawiera instrukcje oddzielne w celu uwzględnienia poniższych zadań:</span><span class="sxs-lookup"><span data-stu-id="16606-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="16606-132">Włączanie logowania Google</span><span class="sxs-lookup"><span data-stu-id="16606-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="16606-133">Włączanie logowania do usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="16606-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="16606-134">Włączanie logowania do usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="16606-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="16606-135">Włączanie logowania Google</span><span class="sxs-lookup"><span data-stu-id="16606-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="16606-136">Utwórz lub Otwórz witrynę stron ASP.NET Web Pages, który jest oparty na szablonie witryny początkowej programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="16606-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="16606-137">Otwórz  *\_AppStart.cshtml* strony, a następnie usuń komentarz następujący wiersz kodu.</span><span class="sxs-lookup"><span data-stu-id="16606-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="16606-138">Testowanie logowania usługi Google</span><span class="sxs-lookup"><span data-stu-id="16606-138">Testing Google login</span></span>

1. <span data-ttu-id="16606-139">Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **Zaloguj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="16606-140">Na *logowania* stronie **Zaloguj się za pomocą innej usługi** sekcji, wybierają **Google** lub **Yahoo** przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="16606-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="16606-141">W tym przykładzie użyto logowania usługi Google.</span><span class="sxs-lookup"><span data-stu-id="16606-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="16606-142">Strony sieci web przekierowuje żądanie do strony logowania firmy Google.</span><span class="sxs-lookup"><span data-stu-id="16606-142">The web page redirects the request to the Google login page.</span></span>

    ![Konta Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="16606-144">Wprowadź poświadczenia dla istniejącego konta Google.</span><span class="sxs-lookup"><span data-stu-id="16606-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="16606-145">Jeśli Google zapyta, czy chcesz zezwolić na *Localhost* mają być używane informacje z konta, kliknij przycisk **Zezwalaj**.</span><span class="sxs-lookup"><span data-stu-id="16606-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="16606-146">Kod używa tokenu Google, aby uwierzytelnić użytkownika, a następnie wróci do tej strony w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16606-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="16606-147">Ta strona umożliwia kojarzenie ich logowania usługi Google przy użyciu istniejącego konta w witrynie sieci Web lub mogą rejestrować nowe konto w witrynie do skojarzenia zewnętrznych danych logowania za pomocą.</span><span class="sxs-lookup"><span data-stu-id="16606-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="16606-149">Wybierz **skojarzyć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-149">Choose the **Associate** button.</span></span> <span data-ttu-id="16606-150">Zwraca przeglądarce do strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16606-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="16606-151">Włączanie logowania do usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="16606-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="16606-152">Przejdź do [Facebook deweloperów witryny](https://developers.facebook.com/apps) (Zaloguj się, jeśli jeszcze nie zalogowano Cię).</span><span class="sxs-lookup"><span data-stu-id="16606-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="16606-153">Wybierz **Utwórz nową aplikację** przycisk, a następnie postępuj zgodnie z monitami, aby utworzyć nową aplikację.</span><span class="sxs-lookup"><span data-stu-id="16606-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="16606-154">W sekcji **wybierz, jak Twoja aplikacja integruje się z usługi Facebook**, wybierz **witryny sieci Web** sekcji.</span><span class="sxs-lookup"><span data-stu-id="16606-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="16606-155">Wypełnij **adres URL witryny** pole adres URL witryny (na przykład `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="16606-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="16606-156">**Domeny** pole jest opcjonalne; służy to do zapewnienia uwierzytelniania dla całej domeny (takie jak *example.com*).</span><span class="sxs-lookup"><span data-stu-id="16606-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="16606-157">Jeśli używasz witryny na komputerze lokalnym przy użyciu adresu URL, takich jak `http://localhost:12345` (których liczba jest numer portu lokalnego), można dodać tę wartość, aby **adres URL witryny** pola do testowania witryny.</span><span class="sxs-lookup"><span data-stu-id="16606-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="16606-158">Jednak każdy razem numer portu zmiany lokacji lokalnej, należy zaktualizować **adres URL witryny** pole aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16606-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="16606-159">Wybierz **Zapisz zmiany** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="16606-160">Wybierz **aplikacje** karcie ponownie, a następnie wyświetlić stronę początkową dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16606-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="16606-161">Kopiuj **Identyfikatora aplikacji** i **klucz tajny aplikacji** wartości dla swojej aplikacji i wklej je do pliku tekstowego tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="16606-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="16606-162">W kodzie witryny sieci Web przekazuje te wartości do dostawcy usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="16606-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="16606-163">Zakończ działanie witryny dewelopera usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="16606-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="16606-164">Teraz możesz dokonać zmian dwie strony w witrynie sieci Web tak, aby użytkownicy będą mogli zalogować się do witryny za pomocą swoich kont usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="16606-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="16606-165">Utwórz lub Otwórz witrynę stron ASP.NET Web Pages, który jest oparty na szablonie witryny początkowej programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="16606-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="16606-166">Otwórz  *\_AppStart.cshtml* strony, a następnie usuń komentarz kodu dla dostawcy uwierzytelniania Facebook OAuth.</span><span class="sxs-lookup"><span data-stu-id="16606-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="16606-167">Blok kodu pozbawionym znaków komentarza wierszu wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="16606-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="16606-168">Kopiuj **Identyfikatora aplikacji** wartości z aplikacji usługi Facebook jako wartość `appId` parametru (wewnątrz znaków cudzysłowu).</span><span class="sxs-lookup"><span data-stu-id="16606-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="16606-169">Kopiuj **klucz tajny aplikacji** wartość z aplikacji usługi Facebook jako `appSecret` wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="16606-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="16606-170">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="16606-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="16606-171">Testowanie logowania do usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="16606-171">Testing Facebook login</span></span>

1. <span data-ttu-id="16606-172">Uruchamianie witryny *default.cshtml* strony i wybierz **logowania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="16606-173">Na *logowania* stronie **Zaloguj się za pomocą innej usługi** wybierz pozycję **Facebook** ikony.</span><span class="sxs-lookup"><span data-stu-id="16606-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="16606-174">Strony sieci web przekierowuje żądanie do strony logowania usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="16606-174">The web page redirects the request to the Facebook login page.</span></span>

    ![protokołu OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="16606-176">Zaloguj się do konta w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="16606-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="16606-177">Kod używa tokenu usługi Facebook do uwierzytelniania, a następnie wróci do strony umożliwiające powiązanie logowania do usługi Facebook przy logowaniu do witryny.</span><span class="sxs-lookup"><span data-stu-id="16606-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="16606-178">Adres nazwy lub adresu e-mail użytkownika jest wypełniony do **E-mail** pola w formularzu.</span><span class="sxs-lookup"><span data-stu-id="16606-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="16606-180">Wybierz **skojarzyć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="16606-181">Przeglądarka zwraca do strony głównej, a użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="16606-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="16606-182">Włączanie logowania do usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="16606-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="16606-183">Przejdź do [Twitter deweloperów witryny](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="16606-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="16606-184">Wybierz **tworzenie aplikacji** łącze, a następnie zaloguj się do witryny.</span><span class="sxs-lookup"><span data-stu-id="16606-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="16606-185">Na **tworzenia aplikacji** formularza, wypełnij **nazwa** i **opis** pola.</span><span class="sxs-lookup"><span data-stu-id="16606-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="16606-186">W **witryny sieci Web** wprowadź adres URL witryny (na przykład `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="16606-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="16606-187">Jeśli testujesz witryny sieci lokalnie (przy użyciu adresu URL typu `http://localhost:12345`), Twitter, może nie zaakceptować adresu URL.</span><span class="sxs-lookup"><span data-stu-id="16606-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="16606-188">Jednak można używać adresu IP lokalnego sprzężenia zwrotnego (na przykład `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="16606-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="16606-189">Upraszcza to proces testowania aplikacji w środowisku lokalnym.</span><span class="sxs-lookup"><span data-stu-id="16606-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="16606-190">Jednak za każdym razem, gdy zmieni się numer portu lokacji lokalnej, należy zaktualizować **witryny sieci Web** pole aplikacji.</span><span class="sxs-lookup"><span data-stu-id="16606-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="16606-191">W **adresów URL wywołania zwrotnego** wprowadź adres URL dla strony witryny sieci Web, który ma użytkowników, aby powrócić do po zalogowaniu się do usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="16606-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="16606-192">Na przykład wysłać do użytkowników na stronę główną witryny Starter (który będzie także rozpoznawał ich stan zalogowany), wprowadź ten sam adres URL, które wprowadziłeś w **witryny sieci Web** pola.</span><span class="sxs-lookup"><span data-stu-id="16606-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="16606-193">Zaakceptuj warunki i wybierz polecenie **tworzenie aplikacji usługi Twitter** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="16606-194">Na **Moje aplikacje** początkowej strony, wybierz utworzoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="16606-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="16606-195">Na **szczegóły** karty, przewiń w dół i wybierz **Utwórz Mój Token dostępu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="16606-196">Na **szczegóły** kartę, skopiuj **konsumenta** i **klucz tajny klienta** wartości dla swojej aplikacji i wklej je do pliku tekstowego tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="16606-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="16606-197">W kodzie witryny sieci Web będzie przekazać te wartości do dostawcy usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="16606-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="16606-198">Zakończ pracę w witrynie Twitter.</span><span class="sxs-lookup"><span data-stu-id="16606-198">Exit the Twitter site.</span></span>

<span data-ttu-id="16606-199">Teraz możesz dokonać zmian dwie strony w witrynie sieci Web tak, aby użytkownicy będą mogli zalogować się do witryny za pomocą swoich kont usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="16606-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="16606-200">Utwórz lub Otwórz witrynę stron ASP.NET Web Pages, który jest oparty na szablonie witryny początkowej programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="16606-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="16606-201">Otwórz  *\_AppStart.cshtml* strony, a następnie usuń komentarz kodu dla dostawcy OAuth w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="16606-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="16606-202">Blok pozbawionym znaków komentarza wierszu kodu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="16606-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="16606-203">Kopiuj **konsumenta** wartość z aplikacji usługi Twitter jako wartość `consumerKey` parametru (wewnątrz znaków cudzysłowu).</span><span class="sxs-lookup"><span data-stu-id="16606-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="16606-204">Kopiuj **klucz tajny klienta** wartość z aplikacji usługi Twitter jako wartość `consumerSecret` parametru.</span><span class="sxs-lookup"><span data-stu-id="16606-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="16606-205">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="16606-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="16606-206">Testowanie logowania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="16606-206">Testing Twitter login</span></span>

1. <span data-ttu-id="16606-207">Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **logowania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="16606-208">Na *logowania* stronie **Zaloguj się za pomocą innej usługi** wybierz pozycję **Twitter** ikony.</span><span class="sxs-lookup"><span data-stu-id="16606-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="16606-209">Strony sieci web przekierowuje żądanie do strony logowania usługi Twitter dla aplikacji, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="16606-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="16606-211">Zaloguj się do konta w serwisie Twitter.</span><span class="sxs-lookup"><span data-stu-id="16606-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="16606-212">Kod używa token usługi Twitter w celu uwierzytelnienia użytkownika, a następnie powrót do strony umożliwiające powiązanie identyfikatora logowania przy użyciu konta z witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="16606-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="16606-213">Twoja nazwa lub adres e-mail jest wypełniana w **E-mail** pola w formularzu.</span><span class="sxs-lookup"><span data-stu-id="16606-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="16606-215">Wybierz **skojarzyć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="16606-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="16606-216">Przeglądarka zwraca do strony głównej, a użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="16606-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="16606-217">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="16606-217">Additional Resources</span></span>

- [<span data-ttu-id="16606-218">Dostosowywanie zachowania dla całej witryny</span><span class="sxs-lookup"><span data-stu-id="16606-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="16606-219">Dodawanie zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="16606-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)

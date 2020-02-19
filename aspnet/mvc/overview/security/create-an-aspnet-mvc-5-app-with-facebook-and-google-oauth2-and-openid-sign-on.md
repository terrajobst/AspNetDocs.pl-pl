---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Tworzenie aplikacji MVC 5 za pomocą usług Facebook, Twitter, LinkedIn i Google OAuth2 Sign-C#in () | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5, która umożliwia użytkownikom logowanie się przy użyciu protokołu OAuth 2,0 z poświadczeniami z zewnętrznego wstępnego...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457690"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="27ebf-103">Tworzenie aplikacji ASP.NET MVC 5 z logowaniem OAuth2 za pomocą poświadczeń usług Facebook, Twitter, LinkedIn i Google (C#)</span><span class="sxs-lookup"><span data-stu-id="27ebf-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>

<span data-ttu-id="27ebf-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="27ebf-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="27ebf-105">W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 5, która umożliwia użytkownikom logowanie się przy użyciu protokołu [OAuth 2,0](http://oauth.net/2/) z poświadczeniami od zewnętrznego dostawcy uwierzytelniania, takiego jak Facebook, Twitter, LinkedIn, Microsoft lub Google.</span><span class="sxs-lookup"><span data-stu-id="27ebf-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="27ebf-106">Dla uproszczenia ten samouczek koncentruje się na pracy z poświadczeniami z serwisu Facebook i Google.</span><span class="sxs-lookup"><span data-stu-id="27ebf-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="27ebf-107">Włączenie tych poświadczeń w witrynach sieci Web zapewnia znaczną korzyść, ponieważ miliony użytkowników mają już konta z dostawcami zewnętrznymi.</span><span class="sxs-lookup"><span data-stu-id="27ebf-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="27ebf-108">Mogą być bardziej nachylone użytkownicy, którzy nie muszą tworzyć i zapamiętać nowego zestawu poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="27ebf-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="27ebf-109">Zobacz też [ASP.NET aplikację MVC 5 za pomocą wiadomości SMS i uwierzytelniania dwuskładnikowego poczty e-mail](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="27ebf-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="27ebf-110">W tym samouczku pokazano również, jak dodać dane profilu dla użytkownika oraz jak dodawać role przy użyciu interfejsu API członkostwa.</span><span class="sxs-lookup"><span data-stu-id="27ebf-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="27ebf-111">Ten samouczek został utworzony przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (Obserwuj mnie w serwisie Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="27ebf-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="27ebf-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="27ebf-112">Getting Started</span></span>

<span data-ttu-id="27ebf-113">Zacznij od zainstalowania i uruchomienia [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="27ebf-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="27ebf-114">Zainstaluj program Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="27ebf-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="27ebf-115">Aby uzyskać pomoc dotyczącą usług Dropbox, GitHub, LinkedIn, usługi Instagram, buffer, Salesforce, PAROW, Stack Exchange, TripIt, Twitch, Twitter, Yahoo! i innych, zobacz ten [przykładowy projekt](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="27ebf-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="27ebf-116">Musisz zainstalować program Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszy, aby używać usługi Google OAuth 2 i do debugowania lokalnego bez ostrzeżeń dotyczących protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="27ebf-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>

<span data-ttu-id="27ebf-117">Na stronie **startowej** kliknij pozycję **Nowy projekt** lub możesz użyć menu i wybrać polecenie **plik**, a następnie pozycję **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="27ebf-118">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="27ebf-118">Creating Your First Application</span></span>

<span data-ttu-id="27ebf-119">Kliknij pozycję **Nowy projekt**, a następnie wybierz pozycję **Wizualizacja C#**  po lewej stronie, a następnie pozycję **Sieć Web** , a następnie wybierz opcję **aplikacja sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="27ebf-120">Nadaj projektowi nazwę "MvcAuth", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="27ebf-121">W oknie dialogowym **Nowy projekt ASP.NET** kliknij pozycję **MVC**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="27ebf-122">Jeśli uwierzytelnianie nie jest **poszczególnymi kontami użytkowników**, kliknij przycisk **Zmień uwierzytelnianie** i wybierz **poszczególne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="27ebf-123">Sprawdzając **host w chmurze**, aplikacja będzie bardzo łatwa do hostowania na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="27ebf-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="27ebf-124">Jeśli **w chmurze**wybrano opcję host, wypełnij okno dialogowe Konfigurowanie.</span><span class="sxs-lookup"><span data-stu-id="27ebf-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="27ebf-125">Aktualizowanie do najnowszej wersji oprogramowania pośredniczącego przy użyciu narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="27ebf-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="27ebf-126">Użyj Menedżera pakietów NuGet, aby zaktualizować [oprogramowanie pośredniczące Owin](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="27ebf-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="27ebf-127">W menu po lewej stronie wybierz pozycję **aktualizacje** .</span><span class="sxs-lookup"><span data-stu-id="27ebf-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="27ebf-128">Możesz kliknąć przycisk **Aktualizuj wszystko** lub wyszukać tylko pakiety Owin (pokazane na następnym obrazie):</span><span class="sxs-lookup"><span data-stu-id="27ebf-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="27ebf-129">Na poniższej ilustracji są wyświetlane tylko pakiety OWIN:</span><span class="sxs-lookup"><span data-stu-id="27ebf-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="27ebf-130">W konsoli Menedżera pakietów (PMC) można wprowadzić `Update-Package` polecenie, które zaktualizuje wszystkie pakiety.</span><span class="sxs-lookup"><span data-stu-id="27ebf-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="27ebf-131">Naciśnij klawisz **F5** lub **Ctrl + F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="27ebf-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="27ebf-132">Na poniższej ilustracji numer portu to 1234.</span><span class="sxs-lookup"><span data-stu-id="27ebf-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="27ebf-133">Po uruchomieniu aplikacji zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="27ebf-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="27ebf-134">W zależności od rozmiaru okna przeglądarki może być konieczne kliknięcie ikony nawigacji, aby wyświetlić **informacje dotyczące** **domu**, **kontaktu**, **rejestrowania** i **logowania** .</span><span class="sxs-lookup"><span data-stu-id="27ebf-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="27ebf-135">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="27ebf-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="27ebf-136">Aby połączyć się z dostawcami uwierzytelniania, takimi jak Google i Facebook, należy skonfigurować usługi IIS-Express do korzystania z protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="27ebf-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="27ebf-137">Ważne jest, aby nadal korzystać z protokołu SSL po zalogowaniu i nie wrócić do protokołu HTTP, plik cookie logowania jest równie tajny jak nazwa użytkownika i hasło oraz bez użycia protokołu SSL, który jest przesyłany w postaci zwykłego tekstu przez sieć.</span><span class="sxs-lookup"><span data-stu-id="27ebf-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="27ebf-138">Oprócz tego czas potrzebny do wykonania uzgadniania i zabezpieczenia kanału (czyli zbiorczego działania protokołu HTTPS wolniejszego niż HTTP) przed uruchomieniem potoku MVC, dlatego przekierowanie z powrotem do protokołu HTTP po zalogowaniu nie spowoduje wykonania bieżącego żądania lub przyszłego żądania znacznie szybsze.</span><span class="sxs-lookup"><span data-stu-id="27ebf-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="27ebf-139">W **Eksplorator rozwiązań**kliknij projekt **MvcAuth** .</span><span class="sxs-lookup"><span data-stu-id="27ebf-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="27ebf-140">Naciśnij klawisz F4, aby wyświetlić właściwości projektu.</span><span class="sxs-lookup"><span data-stu-id="27ebf-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="27ebf-141">Alternatywnie, z menu **Widok** można wybrać **okno właściwości**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="27ebf-142">Zmień wartość **SSL z włączony** na true.</span><span class="sxs-lookup"><span data-stu-id="27ebf-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="27ebf-143">Skopiuj adres URL protokołu SSL (który będzie `https://localhost:44300/`, chyba że utworzono inne projekty SSL).</span><span class="sxs-lookup"><span data-stu-id="27ebf-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="27ebf-144">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **MvcAuth** i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="27ebf-145">Wybierz kartę **Sieć Web** , a następnie wklej adres URL protokołu SSL do pola **adres URL projektu** .</span><span class="sxs-lookup"><span data-stu-id="27ebf-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="27ebf-146">Zapisz plik (CTL + S).</span><span class="sxs-lookup"><span data-stu-id="27ebf-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="27ebf-147">Ten adres URL będzie potrzebny do konfigurowania aplikacji Facebook i Google Authentication.</span><span class="sxs-lookup"><span data-stu-id="27ebf-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="27ebf-148">Dodaj atrybut [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) do kontrolera `Home`, aby wymagać wszystkie żądania muszą używać protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="27ebf-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="27ebf-149">Bardziej bezpiecznym podejściem jest dodanie filtra [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27ebf-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="27ebf-150">Zapoznaj się z sekcją &quot;chronić aplikację przy użyciu protokołu SSL, a&quot; atrybutu Autoryzuj w moim samouczku [Utwórz aplikację ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdróż ją w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="27ebf-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="27ebf-151">Poniżej przedstawiono część kontrolera macierzystego.</span><span class="sxs-lookup"><span data-stu-id="27ebf-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="27ebf-152">Naciśnij klawisze CTRL+F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="27ebf-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="27ebf-153">Jeśli certyfikat został zainstalowany w przeszłości, możesz pominąć resztę tej sekcji i przejść do [tworzenia aplikacji Google dla protokołu OAuth 2 i połączyć aplikację z projektem](#goog). w przeciwnym razie postępuj zgodnie z instrukcjami, aby zaufać certyfikatowi z podpisem własnym, który IIS Express wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="27ebf-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="27ebf-154">Przeczytaj okno dialogowe **Ostrzeżenie o zabezpieczeniach** , a następnie kliknij przycisk **tak** , jeśli chcesz zainstalować certyfikat reprezentujący localhost.</span><span class="sxs-lookup"><span data-stu-id="27ebf-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="27ebf-155">Program IE pokazuje stronę *główną* i nie ma ostrzeżeń dotyczących protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="27ebf-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="27ebf-156">Program Google Chrome akceptuje również certyfikat i wyświetla zawartość HTTPS bez ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="27ebf-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="27ebf-157">Program Firefox używa własnego magazynu certyfikatów, więc wyświetli ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="27ebf-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="27ebf-158">W przypadku naszej aplikacji możesz bezpiecznie kliknąć opcję **zapoznaj się z ryzykiem**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="27ebf-159">Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji z projektem</span><span class="sxs-lookup"><span data-stu-id="27ebf-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="27ebf-160">Bieżące instrukcje dotyczące usługi Google OAuth można znaleźć [w temacie Configuring Google Authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="27ebf-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="27ebf-161">Przejdź do [konsoli deweloperów firmy Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="27ebf-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="27ebf-162">Jeśli projekt nie został wcześniej utworzony, wybierz pozycję **poświadczenia** na karcie po lewej stronie, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="27ebf-163">Na karcie po lewej stronie kliknij pozycję **poświadczenia**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="27ebf-164">Kliknij przycisk **Utwórz poświadczenia** , a następnie **Identyfikator klienta uwierzytelniania OAuth**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="27ebf-165">W oknie dialogowym **Tworzenie identyfikatora klienta** Zachowaj domyślną **aplikację sieci Web** dla typu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27ebf-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="27ebf-166">Ustaw **autoryzowane źródła kodu JavaScript** na adres URL protokołu SSL użyty powyżej (`https://localhost:44300/`, chyba że utworzono inne projekty SSL)</span><span class="sxs-lookup"><span data-stu-id="27ebf-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="27ebf-167">Ustaw **Autoryzowany identyfikator URI przekierowania** na:</span><span class="sxs-lookup"><span data-stu-id="27ebf-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="27ebf-168">Kliknij pozycję menu ekran wyrażania zgody OAuth, a następnie ustaw adres e-mail i nazwę produktu.</span><span class="sxs-lookup"><span data-stu-id="27ebf-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="27ebf-169">Po zakończeniu formularza kliknij pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="27ebf-170">Kliknij element menu Biblioteka, Wyszukaj pozycję **Google + interfejs API**, kliknij go, a następnie naciśnij pozycję Włącz.</span><span class="sxs-lookup"><span data-stu-id="27ebf-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="27ebf-171">Na poniższej ilustracji przedstawiono włączone interfejsy API.</span><span class="sxs-lookup"><span data-stu-id="27ebf-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="27ebf-172">W Menedżerze API usługi Google API przejdź do karty **poświadczenia** , aby uzyskać **Identyfikator klienta**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="27ebf-173">Pobierz, aby zapisać plik JSON z wpisami tajnymi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="27ebf-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="27ebf-174">Skopiuj i wklej **ClientId** oraz **ClientSecret** do metody `UseGoogleAuthentication` znajdującej się w pliku *Startup.auth.cs* w folderze *App_Start* .</span><span class="sxs-lookup"><span data-stu-id="27ebf-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="27ebf-175">Podane poniżej wartości **ClientId** i **ClientSecret** są przykładami i nie działają.</span><span class="sxs-lookup"><span data-stu-id="27ebf-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="27ebf-176">Zabezpieczenia — nigdy nie należy przechowywać poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="27ebf-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="27ebf-177">Konto i poświadczenia są dodawane do powyższego kodu, aby zapewnić prostą przykład.</span><span class="sxs-lookup"><span data-stu-id="27ebf-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="27ebf-178">Zapoznaj [się z najlepszymi rozwiązaniami dotyczącymi wdrażania haseł i innych poufnych danych w ASP.NET i Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="27ebf-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="27ebf-179">Naciśnij **kombinację klawiszy CTRL + F5** , aby skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="27ebf-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="27ebf-180">Kliknij link **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="27ebf-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="27ebf-181">W obszarze **Użyj innej usługi do zalogowania**się kliknij pozycję **Google**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="27ebf-182">Jeśli pominięto którykolwiek z powyższych kroków, wystąpi błąd HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="27ebf-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="27ebf-183">Sprawdź ponownie powyższe kroki.</span><span class="sxs-lookup"><span data-stu-id="27ebf-183">Recheck your steps above.</span></span> <span data-ttu-id="27ebf-184">Jeśli pominięto wymagane ustawienie (na przykład **Nazwa produktu**), Dodaj brakujący element i Zapisz; uwierzytelnienie może potrwać kilka minut.</span><span class="sxs-lookup"><span data-stu-id="27ebf-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="27ebf-185">Nastąpi przekierowanie do witryny Google, w której będą wprowadzane poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="27ebf-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="27ebf-186">Po wprowadzeniu poświadczeń zostanie wyświetlony monit o przyznanie uprawnień do aplikacji sieci Web, która została właśnie utworzona:</span><span class="sxs-lookup"><span data-stu-id="27ebf-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="27ebf-187">Kliknij przycisk **Akceptuj**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-187">Click **Accept**.</span></span> <span data-ttu-id="27ebf-188">Nastąpi przekierowanie z powrotem do strony **rejestracji** w aplikacji MvcAuth, w której można zarejestrować swoje konto Google.</span><span class="sxs-lookup"><span data-stu-id="27ebf-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="27ebf-189">Istnieje możliwość zmiany nazwy rejestracji lokalnej poczty e-mail używanej na potrzeby konta usługi Gmail, ale zazwyczaj chcesz zachować domyślny alias poczty e-mail (czyli ten, który został użyty do uwierzytelniania).</span><span class="sxs-lookup"><span data-stu-id="27ebf-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="27ebf-190">Kliknij pozycję **zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="27ebf-191">Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji z projektem</span><span class="sxs-lookup"><span data-stu-id="27ebf-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="27ebf-192">Bieżące instrukcje dotyczące uwierzytelniania w usłudze Facebook OAuth2 można znaleźć w temacie [Konfigurowanie uwierzytelniania w serwisie Facebook](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="27ebf-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="27ebf-193">Sprawdzanie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="27ebf-193">Examine the Membership Data</span></span>

<span data-ttu-id="27ebf-194">W menu **Widok** kliknij **Eksplorator serwera**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="27ebf-195">Rozwiń węzeł **DefaultConnection (MvcAuth)** , rozwiń węzeł **tabele**, kliknij prawym przyciskiem myszy pozycję **AspNetUsers** i kliknij polecenie **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dane tabeli AspnetUsers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="27ebf-197">Dodawanie danych profilu do klasy użytkownika</span><span class="sxs-lookup"><span data-stu-id="27ebf-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="27ebf-198">W tej sekcji dodasz datę urodzenia i miejscowość domu do danych użytkownika podczas rejestracji, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="27ebf-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg z domem i bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="27ebf-200">Otwórz plik *Models\IdentityModels.cs* i Dodaj właściwości data urodzenia i miejscowość domu:</span><span class="sxs-lookup"><span data-stu-id="27ebf-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="27ebf-201">Otwórz plik *Models\AccountViewModels.cs* oraz właściwości ustaw datę urodzenia i miejscowość w `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="27ebf-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="27ebf-202">Otwórz plik *Controllers\AccountController.cs* i Dodaj kod dla daty urodzenia i miejscowości głównej w `ExternalLoginConfirmation`ej metodzie działania, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="27ebf-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="27ebf-203">Dodaj datę urodzenia i miejscowość domu do pliku *Views\Account\ExternalLoginConfirmation.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="27ebf-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="27ebf-204">Usuń bazę danych członkostwa, aby móc ponownie zarejestrować swoje konto w serwisie Facebook przy użyciu aplikacji, a następnie sprawdź, czy możesz dodać nową datę urodzenia i informacje o profilu miejscowości.</span><span class="sxs-lookup"><span data-stu-id="27ebf-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="27ebf-205">W **Eksplorator rozwiązań**kliknij ikonę **Pokaż wszystkie pliki** , a następnie kliknij prawym przyciskiem myszy pozycję *dodaj\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;. mdf* i kliknij polecenie **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="27ebf-206">W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="27ebf-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="27ebf-207">Wprowadź następujące polecenia w obszarze PMC.</span><span class="sxs-lookup"><span data-stu-id="27ebf-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="27ebf-208">Włącz-migracje</span><span class="sxs-lookup"><span data-stu-id="27ebf-208">Enable-Migrations</span></span>
2. <span data-ttu-id="27ebf-209">Inicjowanie dodawania do migracji</span><span class="sxs-lookup"><span data-stu-id="27ebf-209">Add-Migration Init</span></span>
3. <span data-ttu-id="27ebf-210">Update-Database</span><span class="sxs-lookup"><span data-stu-id="27ebf-210">Update-Database</span></span>

<span data-ttu-id="27ebf-211">Uruchom aplikację, a następnie zaloguj się i zarejestruj niektórych użytkowników za pomocą serwisu FaceBook i Google.</span><span class="sxs-lookup"><span data-stu-id="27ebf-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="27ebf-212">Sprawdzanie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="27ebf-212">Examine the Membership Data</span></span>

<span data-ttu-id="27ebf-213">W menu **Widok** kliknij **Eksplorator serwera**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="27ebf-214">Kliknij prawym przyciskiem myszy pozycję **AspNetUsers** , a następnie kliknij pozycję **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="27ebf-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="27ebf-215">Poniżej przedstawiono pola `HomeTown` i `BirthDate`.</span><span class="sxs-lookup"><span data-stu-id="27ebf-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="27ebf-216">Wylogowywanie aplikacji i logowanie przy użyciu innego konta</span><span class="sxs-lookup"><span data-stu-id="27ebf-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="27ebf-217">Jeśli zalogujesz się do aplikacji za pomocą usługi Facebook, a następnie wylogujesz się, a następnie spróbujesz ponownie zalogować się przy użyciu innego konta w serwisie Facebook (za pomocą tej samej przeglądarki), nastąpi natychmiastowe zalogowanie do poprzedniego konta w serwisie Facebook, z którego korzystasz.</span><span class="sxs-lookup"><span data-stu-id="27ebf-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="27ebf-218">Aby użyć innego konta, należy przejść do serwisu Facebook i wylogować się w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="27ebf-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="27ebf-219">Ta sama reguła ma zastosowanie do każdego innego dostawcy uwierzytelniania innej firmy.</span><span class="sxs-lookup"><span data-stu-id="27ebf-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="27ebf-220">Alternatywnie możesz zalogować się przy użyciu innego konta przy użyciu innej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="27ebf-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27ebf-221">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="27ebf-221">Next Steps</span></span>

<span data-ttu-id="27ebf-222">Zapoznaj [się z artykułem wprowadzenie do dostawców zabezpieczeń protokołu OAuth usługi Yahoo i LinkedIn dla Owin](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) przez Jerrie Pelser dla usług Yahoo i LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="27ebf-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="27ebf-223">Zobacz temat Jerrie dbsocial login Button for ASP.NET MVC 5, aby uzyskać przyciski logowania społecznościowego.</span><span class="sxs-lookup"><span data-stu-id="27ebf-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="27ebf-224">Postępuj zgodnie z moim samouczkiem [Utwórz aplikację ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdróż ją w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), która kontynuuje ten samouczek i pokaże następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="27ebf-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="27ebf-225">Jak wdrożyć aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="27ebf-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="27ebf-226">Jak zabezpieczyć aplikację za pomocą ról.</span><span class="sxs-lookup"><span data-stu-id="27ebf-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="27ebf-227">Jak zabezpieczyć aplikację przy użyciu metody [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) i [Autoryzuj](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtry.</span><span class="sxs-lookup"><span data-stu-id="27ebf-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="27ebf-228">Jak dodać użytkowników i role przy użyciu interfejsu API członkostwa.</span><span class="sxs-lookup"><span data-stu-id="27ebf-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="27ebf-229">Prosimy o opinię na temat sposobu, w jaki polubisz ten samouczek, i co możemy ulepszyć.</span><span class="sxs-lookup"><span data-stu-id="27ebf-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="27ebf-230">Możesz również zażądać nowych tematów w temacie [jak z kodem](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="27ebf-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="27ebf-231">Możesz nawet zadawać i głosować na nowe funkcje, które zostaną dodane do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="27ebf-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="27ebf-232">Można na przykład zagłosować na narzędzie do [tworzenia użytkowników i ról oraz zarządzania nimi.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="27ebf-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="27ebf-233">Aby uzyskać dobre wyjaśnienie, jak działają usługi uwierzytelniania zewnętrznego ASP.NET, zobacz [zewnętrzne usługi uwierzytelniania](https://asp.net/web-api/overview/security/external-authentication-services)firmy Roberta McMurraya.</span><span class="sxs-lookup"><span data-stu-id="27ebf-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="27ebf-234">Artykuł Roberta również szczegółowo opisuje włączenie uwierzytelniania firmy Microsoft i usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="27ebf-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="27ebf-235">[Samouczek doskonały EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) firmy Dykstra pokazuje, jak korzystać z Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="27ebf-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>

---
uid: web-api/overview/security/external-authentication-services
title: Zewnętrzne usługi uwierzytelniania z interfejsem API sieciC#Web ASP.NET () | Microsoft Docs
author: rmcmurray
description: Opisuje korzystanie z usług uwierzytelniania zewnętrznego w interfejsie API sieci Web ASP.NET.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555476"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="49559-103">Zewnętrzne usługi uwierzytelniania z interfejsem API sieciC#Web ASP.NET ()</span><span class="sxs-lookup"><span data-stu-id="49559-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="49559-104">Programy Visual Studio 2017 i ASP.NET 4.7.2 rozszerzają opcje zabezpieczeń dla [aplikacji jednostronicowych](../../../single-page-application/index.md) (Spa) i usług [internetowego interfejsu API](../../index.md) w celu integracji z zewnętrznymi usługami uwierzytelniania, które obejmują kilka usług uwierzytelniania OAuth/OpenID Connect i mediów społecznościowych: konta Microsoft, Twitter, Facebook i Google.</span><span class="sxs-lookup"><span data-stu-id="49559-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="49559-105">W tym instruktażu</span><span class="sxs-lookup"><span data-stu-id="49559-105">In this Walkthrough</span></span>

- [<span data-ttu-id="49559-106">Korzystanie z usług uwierzytelniania zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="49559-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="49559-107">Tworzenie przykładowej aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="49559-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="49559-108">Włączanie uwierzytelniania w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="49559-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="49559-109">Włączanie uwierzytelniania Google</span><span class="sxs-lookup"><span data-stu-id="49559-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="49559-110">Włączanie uwierzytelniania firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="49559-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="49559-111">Włączanie uwierzytelniania w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="49559-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="49559-112">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="49559-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="49559-113">Łączenie usług uwierzytelniania zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="49559-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="49559-114">Konfigurowanie IIS Express do używania w pełni kwalifikowanej nazwy domeny</span><span class="sxs-lookup"><span data-stu-id="49559-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="49559-115">Jak uzyskać ustawienia aplikacji na potrzeby uwierzytelniania firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="49559-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="49559-116">Opcjonalne: wyłącz rejestrację lokalną</span><span class="sxs-lookup"><span data-stu-id="49559-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="49559-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="49559-117">Prerequisites</span></span>

<span data-ttu-id="49559-118">Aby postępować zgodnie z przykładami w tym instruktażu, należy dysponować następującymi informacjami:</span><span class="sxs-lookup"><span data-stu-id="49559-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="49559-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="49559-119">Visual Studio 2017</span></span>
- <span data-ttu-id="49559-120">Konto dewelopera z identyfikatorem aplikacji i kluczem tajnym dla jednej z następujących usług uwierzytelniania mediów społecznościowych:</span><span class="sxs-lookup"><span data-stu-id="49559-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="49559-121">Konta Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="49559-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="49559-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="49559-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="49559-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="49559-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="49559-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="49559-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="49559-125">Korzystanie z usług uwierzytelniania zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="49559-125">Using External Authentication Services</span></span>

<span data-ttu-id="49559-126">Liczebność zewnętrznych usług uwierzytelniania, które są obecnie dostępne dla deweloperów sieci Web, ułatwia skrócenie czasu projektowania podczas tworzenia nowych aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="49559-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="49559-127">Użytkownicy sieci Web mają zwykle kilka istniejących kont dla popularnych usług sieci Web i witryn internetowych mediów społecznościowych, dlatego gdy aplikacja sieci Web implementuje usługi uwierzytelniania z zewnętrznej usługi sieci Web lub witryny sieci Web mediów społecznościowych, zapisuje czas projektowania, który wykorzystano Tworzenie implementacji uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="49559-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="49559-128">Korzystanie z zewnętrznej usługi uwierzytelniania powoduje, że użytkownicy końcowi nie muszą tworzyć innego konta dla aplikacji sieci Web, a także zapamiętać inną nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="49559-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="49559-129">W przeszłości deweloperzy mieli dwie opcje: Utwórz własną implementację uwierzytelniania lub Dowiedz się, jak zintegrować zewnętrzną usługę uwierzytelniania z aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="49559-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="49559-130">Na poniższym diagramie przedstawiono prosty przepływ żądania dla agenta użytkownika (przeglądarki sieci Web), który żąda informacji z aplikacji sieci Web skonfigurowanej do korzystania z zewnętrznej usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="49559-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

<span data-ttu-id="49559-131">Na powyższym diagramie agent użytkownika (lub przeglądarka sieci Web w tym przykładzie) wysyła żądanie do aplikacji sieci Web, która przekierowuje przeglądarkę internetową do zewnętrznej usługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="49559-131">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="49559-132">Agent użytkownika wysyła swoje poświadczenia do zewnętrznej usługi uwierzytelniania, a w przypadku pomyślnego uwierzytelnienia agenta użytkownika zewnętrzna usługa uwierzytelniania przekierowuje agenta użytkownika do oryginalnej aplikacji sieci Web przy użyciu jakiejś postaci tokenu, która Agent użytkownika wyśle do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="49559-132">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="49559-133">Aplikacja sieci Web użyje tokenu, aby sprawdzić, czy agent użytkownika został pomyślnie uwierzytelniony przez zewnętrzną usługę uwierzytelniania, a aplikacja sieci Web może użyć tokenu, aby zebrać więcej informacji na temat agenta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="49559-133">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="49559-134">Po zakończeniu przetwarzania informacji o agencie aplikacji aplikacja sieci Web zwróci odpowiednią odpowiedź do agenta użytkownika na podstawie jego ustawień autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="49559-134">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="49559-135">W drugim przykładzie agent użytkownika negocjuje z aplikacją sieci Web i zewnętrznym serwerem autoryzacji, a aplikacja sieci Web wykonuje dodatkową komunikację z zewnętrznym serwerem autoryzacji w celu pobrania dodatkowych informacji o użytkowniku Odczynnik</span><span class="sxs-lookup"><span data-stu-id="49559-135">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

<span data-ttu-id="49559-136">Programy Visual Studio 2017 i ASP.NET 4.7.2 zapewniają integrację z zewnętrznymi usługami uwierzytelniania dla deweloperów, zapewniając wbudowaną integrację dla następujących usług uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="49559-136">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="49559-137">Facebook</span><span class="sxs-lookup"><span data-stu-id="49559-137">Facebook</span></span>
- <span data-ttu-id="49559-138">Google</span><span class="sxs-lookup"><span data-stu-id="49559-138">Google</span></span>
- <span data-ttu-id="49559-139">Konta Microsoft (konta usługi Windows Live ID)</span><span class="sxs-lookup"><span data-stu-id="49559-139">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="49559-140">Twitter</span><span class="sxs-lookup"><span data-stu-id="49559-140">Twitter</span></span>

<span data-ttu-id="49559-141">W przykładach w tym instruktażu pokazano, jak skonfigurować poszczególne obsługiwane usługi uwierzytelniania zewnętrznego przy użyciu nowego szablonu aplikacji sieci Web ASP.NET, który jest dostarczany z programem Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="49559-141">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="49559-142">W razie potrzeby może być konieczne dodanie nazwy FQDN do ustawień usługi uwierzytelniania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="49559-142">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="49559-143">To wymaganie jest oparte na ograniczeniach zabezpieczeń niektórych zewnętrznych usług uwierzytelniania, które wymagają, aby nazwa FQDN w ustawieniach aplikacji była zgodna z nazwą FQDN używaną przez klientów.</span><span class="sxs-lookup"><span data-stu-id="49559-143">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="49559-144">(Kroki dla tego elementu różnią się znacznie dla każdej usługi uwierzytelniania zewnętrznego. należy zapoznać się z dokumentacją dla każdej usługi uwierzytelniania zewnętrznego, aby sprawdzić, czy jest to wymagane i jak skonfigurować te ustawienia.) Jeśli musisz skonfigurować IIS Express do korzystania z nazwy FQDN do testowania tego środowiska, zobacz sekcję [konfigurowanie IIS Express do używania w pełni kwalifikowanej nazwy domeny](#FQDN) w dalszej części tego przewodnika.</span><span class="sxs-lookup"><span data-stu-id="49559-144">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="49559-145">Tworzenie przykładowej aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="49559-145">Create a Sample Web Application</span></span>

<span data-ttu-id="49559-146">Poniższe kroki przeprowadzą Cię przez proces tworzenia przykładowej aplikacji przy użyciu szablonu aplikacji sieci Web ASP.NET. Ta przykładowa aplikacja zostanie użyta w dalszej części tego przewodnika.</span><span class="sxs-lookup"><span data-stu-id="49559-146">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="49559-147">Uruchom program Visual Studio 2017 i wybierz pozycję **Nowy projekt** na stronie początkowej.</span><span class="sxs-lookup"><span data-stu-id="49559-147">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="49559-148">Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="49559-148">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="49559-149">Gdy zostanie wyświetlone okno dialogowe **Nowy projekt** , wybierz pozycję **zainstalowane** i rozwiń **element C#Wizualizacja** .</span><span class="sxs-lookup"><span data-stu-id="49559-149">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="49559-150">W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="49559-150">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="49559-151">Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web ASP.NET (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="49559-151">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="49559-152">Wprowadź nazwę projektu i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="49559-152">Enter a name for your project and click **OK**.</span></span>

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

<span data-ttu-id="49559-153">Po wyświetleniu **nowego projektu ASP.NET** wybierz szablon **aplikacja jednostronicowa** i kliknij pozycję **Utwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="49559-153">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

<span data-ttu-id="49559-154">Poczekaj, aż program Visual Studio 2017 utworzy projekt.</span><span class="sxs-lookup"><span data-stu-id="49559-154">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="49559-155">Po zakończeniu tworzenia projektu przez program Visual Studio 2017 Otwórz plik *Startup.auth.cs* , który znajduje się w folderze **Start\_** .</span><span class="sxs-lookup"><span data-stu-id="49559-155">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="49559-156">Podczas pierwszego tworzenia projektu żadna z usług uwierzytelniania zewnętrznego nie jest włączona w pliku *Startup.auth.cs* ; Poniżej przedstawiono sposób, w jaki Twój kod może wyglądać, z sekcjami wyróżnionymi w celu włączenia zewnętrznej usługi uwierzytelniania oraz wszelkich odpowiednich ustawień w celu używania kont Microsoft, Twitter, Facebook lub Google w przypadku aplikacji ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="49559-156">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="49559-157">Po naciśnięciu klawisza F5 w celu skompilowania i debugowania aplikacji sieci Web zostanie wyświetlony ekran logowania, w którym zobaczysz, że żadne usługi uwierzytelniania zewnętrznego nie zostały zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="49559-157">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

<span data-ttu-id="49559-158">W poniższych sekcjach opisano sposób włączania poszczególnych usług uwierzytelniania zewnętrznego, które są dostarczane z ASP.NET w programie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="49559-158">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="49559-159">Włączanie uwierzytelniania w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="49559-159">Enabling Facebook authentication</span></span>

<span data-ttu-id="49559-160">Przy użyciu uwierzytelniania w serwisie Facebook wymagane jest utworzenie konta dewelopera w serwisie Facebook, a projekt będzie wymagał identyfikatora aplikacji i klucza tajnego z serwisu Facebook w celu zapełnienia działania.</span><span class="sxs-lookup"><span data-stu-id="49559-160">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="49559-161">Aby uzyskać informacje na temat tworzenia konta dewelopera w serwisie Facebook i uzyskiwania identyfikatora aplikacji i klucza tajnego, zobacz [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="49559-161">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="49559-162">Po uzyskaniu identyfikatora aplikacji i klucza tajnego wykonaj następujące kroki, aby włączyć uwierzytelnianie w serwisie Facebook dla aplikacji sieci Web:</span><span class="sxs-lookup"><span data-stu-id="49559-162">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="49559-163">Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="49559-163">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="49559-164">Znajdź sekcję uwierzytelnianie w serwisie Facebook kodu:</span><span class="sxs-lookup"><span data-stu-id="49559-164">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="49559-165">Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj identyfikator aplikacji i klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="49559-165">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="49559-166">Po dodaniu tych parametrów możesz ponownie skompilować projekt:</span><span class="sxs-lookup"><span data-stu-id="49559-166">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="49559-167">Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce sieci Web zobaczysz, że w serwisie Facebook została zdefiniowana usługa uwierzytelniania zewnętrznego:</span><span class="sxs-lookup"><span data-stu-id="49559-167">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. <span data-ttu-id="49559-168">Po kliknięciu przycisku **Facebook** przeglądarka zostanie przekierowana na stronę logowania w serwisie Facebook:</span><span class="sxs-lookup"><span data-stu-id="49559-168">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. <span data-ttu-id="49559-169">Po wprowadzeniu poświadczeń usługi Facebook i kliknięciu przycisku **Zaloguj**, przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z kontem w usłudze Facebook:</span><span class="sxs-lookup"><span data-stu-id="49559-169">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. <span data-ttu-id="49559-170">Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **rejestracji** aplikacja sieci Web wyświetli domyślną **stronę główną** dla Twojego konta w serwisie Facebook:</span><span class="sxs-lookup"><span data-stu-id="49559-170">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="49559-171">Włączanie uwierzytelniania Google</span><span class="sxs-lookup"><span data-stu-id="49559-171">Enabling Google Authentication</span></span>

<span data-ttu-id="49559-172">Korzystając z uwierzytelniania Google, należy utworzyć konto dla deweloperów firmy Google, a projekt będzie wymagał działania identyfikatora aplikacji i klucza tajnego z usługi Google.</span><span class="sxs-lookup"><span data-stu-id="49559-172">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="49559-173">Aby uzyskać informacje na temat tworzenia konta usługi Google Developer i uzyskiwania identyfikatora aplikacji i klucza tajnego, zobacz [https://developers.google.com](https://developers.google.com).</span><span class="sxs-lookup"><span data-stu-id="49559-173">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>

<span data-ttu-id="49559-174">Aby włączyć uwierzytelnianie Google dla aplikacji sieci Web, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="49559-174">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="49559-175">Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="49559-175">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="49559-176">Znajdź sekcję uwierzytelnianie Google w kodzie:</span><span class="sxs-lookup"><span data-stu-id="49559-176">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="49559-177">Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj identyfikator aplikacji i klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="49559-177">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="49559-178">Po dodaniu tych parametrów możesz ponownie skompilować projekt:</span><span class="sxs-lookup"><span data-stu-id="49559-178">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="49559-179">Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce sieci Web zobaczysz, że firma Google została zdefiniowana jako usługa uwierzytelniania zewnętrznego:</span><span class="sxs-lookup"><span data-stu-id="49559-179">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. <span data-ttu-id="49559-180">Po kliknięciu przycisku **Google** przeglądarka zostanie przekierowana na stronę logowania Google:</span><span class="sxs-lookup"><span data-stu-id="49559-180">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. <span data-ttu-id="49559-181">Po wprowadzeniu poświadczeń Google i kliknięciu przycisku **Zaloguj konto**Google wyświetli monit o zweryfikowanie, czy aplikacja sieci Web ma uprawnienia dostępu do konta Google:</span><span class="sxs-lookup"><span data-stu-id="49559-181">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. <span data-ttu-id="49559-182">Po kliknięciu przycisku **Akceptuj**przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z kontem Google:</span><span class="sxs-lookup"><span data-stu-id="49559-182">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. <span data-ttu-id="49559-183">Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **rejestracji** aplikacja sieci Web wyświetli domyślną **stronę główną** konta Google:</span><span class="sxs-lookup"><span data-stu-id="49559-183">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="49559-184">Włączanie uwierzytelniania firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="49559-184">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="49559-185">Uwierzytelnianie firmy Microsoft wymaga utworzenia konta dewelopera i wymaga do działania identyfikatora klienta i klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="49559-185">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="49559-186">Aby uzyskać informacje na temat tworzenia konta dewelopera firmy Microsoft i uzyskiwania identyfikatora klienta i klucza tajnego klienta, zobacz [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span><span class="sxs-lookup"><span data-stu-id="49559-186">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="49559-187">Po uzyskaniu klucza klienta i wpisu tajnego klienta wykonaj następujące kroki, aby włączyć uwierzytelnianie firmy Microsoft dla aplikacji sieci Web:</span><span class="sxs-lookup"><span data-stu-id="49559-187">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="49559-188">Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="49559-188">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="49559-189">Znajdź sekcję uwierzytelniania firmy Microsoft dotyczącą kodu:</span><span class="sxs-lookup"><span data-stu-id="49559-189">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="49559-190">Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj identyfikator klienta i klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="49559-190">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="49559-191">Po dodaniu tych parametrów możesz ponownie skompilować projekt:</span><span class="sxs-lookup"><span data-stu-id="49559-191">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="49559-192">Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce internetowej zobaczysz, że firma Microsoft została zdefiniowana jako zewnętrzna usługa uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="49559-192">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. <span data-ttu-id="49559-193">Po kliknięciu przycisku **Microsoft** przeglądarka zostanie przekierowana do strony logowania firmy Microsoft:</span><span class="sxs-lookup"><span data-stu-id="49559-193">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. <span data-ttu-id="49559-194">Po wprowadzeniu poświadczeń firmy Microsoft i kliknięciu przycisku **Zaloguj**zostanie wyświetlony monit o zweryfikowanie, czy aplikacja sieci Web ma uprawnienia dostępu do konto Microsoft:</span><span class="sxs-lookup"><span data-stu-id="49559-194">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. <span data-ttu-id="49559-195">Po kliknięciu przycisku **tak**przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z konto Microsoft:</span><span class="sxs-lookup"><span data-stu-id="49559-195">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. <span data-ttu-id="49559-196">Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **Utwórz konto** aplikacja sieci Web wyświetli domyślną **stronę główną** dla konto Microsoft:</span><span class="sxs-lookup"><span data-stu-id="49559-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="49559-197">Włączanie uwierzytelniania w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="49559-197">Enabling Twitter Authentication</span></span>

<span data-ttu-id="49559-198">Uwierzytelnianie w usłudze Twitter wymaga utworzenia konta dewelopera, które wymaga klucza klienta i wpisu tajnego klienta w celu działania programu.</span><span class="sxs-lookup"><span data-stu-id="49559-198">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="49559-199">Aby uzyskać informacje na temat tworzenia konta dewelopera usługi Twitter i uzyskiwania klucza klienta i wpisu tajnego klienta, zobacz [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="49559-199">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="49559-200">Po uzyskaniu klucza klienta i wpisu tajnego klienta wykonaj następujące kroki, aby włączyć uwierzytelnianie w usłudze Twitter dla aplikacji sieci Web:</span><span class="sxs-lookup"><span data-stu-id="49559-200">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="49559-201">Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="49559-201">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="49559-202">Znajdź sekcję uwierzytelnianie usługi Twitter w temacie Code:</span><span class="sxs-lookup"><span data-stu-id="49559-202">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="49559-203">Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj klucz klienta i wpis tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="49559-203">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="49559-204">Po dodaniu tych parametrów możesz ponownie skompilować projekt:</span><span class="sxs-lookup"><span data-stu-id="49559-204">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="49559-205">Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce internetowej zobaczysz, że usługa Twitter została zdefiniowana jako zewnętrzny element usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="49559-205">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. <span data-ttu-id="49559-206">Po kliknięciu przycisku **Twitter** przeglądarka zostanie przekierowana na stronę logowania do usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="49559-206">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. <span data-ttu-id="49559-207">Po wprowadzeniu poświadczeń usługi Twitter i kliknięciu przycisku **Autoryzuj aplikację**przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z kontem w usłudze Twitter:</span><span class="sxs-lookup"><span data-stu-id="49559-207">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. <span data-ttu-id="49559-208">Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **rejestracji** aplikacja sieci Web wyświetli domyślną **stronę główną** konta usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="49559-208">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="49559-209">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="49559-209">Additional Information</span></span>

<span data-ttu-id="49559-210">Aby uzyskać dodatkowe informacje na temat tworzenia aplikacji korzystających z protokołu OAuth i OpenID Connect, zobacz następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="49559-210">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="49559-211">Łączenie usług uwierzytelniania zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="49559-211">Combining External Authentication Services</span></span>

<span data-ttu-id="49559-212">Aby zapewnić większą elastyczność, można w tym samym czasie definiować wiele usług uwierzytelniania zewnętrznego — pozwala to użytkownikom aplikacji sieci Web na korzystanie z konta z dowolnej z włączonych usług uwierzytelniania zewnętrznego:</span><span class="sxs-lookup"><span data-stu-id="49559-212">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="49559-213">Konfigurowanie IIS Express do używania w pełni kwalifikowanej nazwy domeny</span><span class="sxs-lookup"><span data-stu-id="49559-213">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="49559-214">Niektórzy dostawcy uwierzytelniania zewnętrznego nie obsługują testowania aplikacji przy użyciu adresu HTTP, takiego jak `http://localhost:port/`.</span><span class="sxs-lookup"><span data-stu-id="49559-214">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="49559-215">Aby obejść ten problem, można dodać statyczne, w pełni kwalifikowane Mapowanie nazw domen (FQDN) do pliku HOSTs i skonfigurować opcje projektu w programie Visual Studio 2017, aby użyć nazwy FQDN do testowania/debugowania.</span><span class="sxs-lookup"><span data-stu-id="49559-215">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="49559-216">Aby to zrobić, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="49559-216">To do so, use the following steps:</span></span>

- <span data-ttu-id="49559-217">Dodawanie statycznej nazwy FQDN mapowania pliku HOSTs:</span><span class="sxs-lookup"><span data-stu-id="49559-217">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="49559-218">Otwórz wiersz polecenia z podwyższonym poziomem uprawnień w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="49559-218">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="49559-219">Wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="49559-219">Type the following command:</span></span>

      <span data-ttu-id="49559-220"><kbd>Notatnik%WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="49559-220"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="49559-221">Dodaj wpis podobny do następującego do pliku HOSTs:</span><span class="sxs-lookup"><span data-stu-id="49559-221">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="49559-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="49559-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="49559-223">Zapisz i zamknij plik HOSTs.</span><span class="sxs-lookup"><span data-stu-id="49559-223">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="49559-224">Skonfiguruj projekt programu Visual Studio tak, aby używał nazwy FQDN:</span><span class="sxs-lookup"><span data-stu-id="49559-224">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="49559-225">Gdy projekt jest otwarty w programie Visual Studio 2017, kliknij menu **projekt** , a następnie wybierz właściwości projektu.</span><span class="sxs-lookup"><span data-stu-id="49559-225">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="49559-226">Na przykład możesz wybrać **Właściwości WebApplication1**.</span><span class="sxs-lookup"><span data-stu-id="49559-226">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="49559-227">Wybierz kartę **Sieć Web** .</span><span class="sxs-lookup"><span data-stu-id="49559-227">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="49559-228">Wprowadź nazwę FQDN dla <strong>adresu URL projektu</strong>.</span><span class="sxs-lookup"><span data-stu-id="49559-228">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="49559-229">Można na przykład wprowadzić <kbd><http://www.wingtiptoys.com></kbd> , jeśli było to mapowanie nazwy FQDN dodane do pliku Hosts.</span><span class="sxs-lookup"><span data-stu-id="49559-229">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="49559-230">Skonfiguruj IIS Express do używania nazwy FQDN dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="49559-230">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="49559-231">Otwórz wiersz polecenia z podwyższonym poziomem uprawnień w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="49559-231">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="49559-232">Wpisz następujące polecenie, aby przejść do folderu IIS Express:</span><span class="sxs-lookup"><span data-stu-id="49559-232">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="49559-233"><kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="49559-233"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="49559-234">Wpisz następujące polecenie, aby dodać nazwę FQDN do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="49559-234">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="49559-235"><kbd>Appcmd. exe set config-Section: System. applicationHost/sites/+&quot;[name = "WebApplication1"]. powiązania. [Protocol = "http", bindingInformation = "\*: 80: www. wingtiptoys. com"]&quot;/commit: AppHost</kbd></span><span class="sxs-lookup"><span data-stu-id="49559-235"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="49559-236">Gdzie **WebApplication1** jest nazwą projektu, a **bindingInformation** zawiera numer portu i nazwę FQDN, których chcesz użyć do testowania.</span><span class="sxs-lookup"><span data-stu-id="49559-236">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="49559-237">Jak uzyskać ustawienia aplikacji na potrzeby uwierzytelniania firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="49559-237">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="49559-238">Łączenie aplikacji z usługą Windows Live na potrzeby uwierzytelniania firmy Microsoft jest procesem prostym.</span><span class="sxs-lookup"><span data-stu-id="49559-238">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="49559-239">Jeśli aplikacja nie została jeszcze połączona z usługą Windows Live, możesz wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="49559-239">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="49559-240">Po wyświetleniu monitu przejdź do [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) i wprowadź nazwę konto Microsoft i hasło, a następnie kliknij przycisk **Zaloguj się**:</span><span class="sxs-lookup"><span data-stu-id="49559-240">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="49559-241">Wybierz pozycję **Dodaj aplikację** i wprowadź nazwę aplikacji po wyświetleniu monitu, a następnie kliknij pozycję **Utwórz**:</span><span class="sxs-lookup"><span data-stu-id="49559-241">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. <span data-ttu-id="49559-242">Wybierz swoją aplikację w polu **Nazwa** , a zostanie wyświetlona strona właściwości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49559-242">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="49559-243">Wprowadź domenę przekierowania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49559-243">Enter the redirect domain for your application.</span></span> <span data-ttu-id="49559-244">Skopiuj **Identyfikator aplikacji** i w obszarze wpisy **tajne aplikacji**wybierz pozycję **Generuj hasło**.</span><span class="sxs-lookup"><span data-stu-id="49559-244">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="49559-245">Skopiuj wyświetlone hasło.</span><span class="sxs-lookup"><span data-stu-id="49559-245">Copy the password that appears.</span></span> <span data-ttu-id="49559-246">Identyfikator aplikacji i hasło to identyfikator klienta i klucz tajny klienta.</span><span class="sxs-lookup"><span data-stu-id="49559-246">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="49559-247">Wybierz przycisk **OK** , a następnie **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="49559-247">Select **Ok** and then **Save**.</span></span>

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="49559-248">Opcjonalne: wyłącz rejestrację lokalną</span><span class="sxs-lookup"><span data-stu-id="49559-248">Optional: Disable Local Registration</span></span>

<span data-ttu-id="49559-249">Bieżąca Funkcja rejestracji lokalnej ASP.NET nie uniemożliwia zautomatyzowanym programom (botów) tworzenia kont członków. na przykład za pomocą technologii zapobiegania bot i sprawdzania poprawności, takiej jak [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span><span class="sxs-lookup"><span data-stu-id="49559-249">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="49559-250">W związku z tym należy usunąć formularz logowania lokalnego i link rejestracji na stronie logowania.</span><span class="sxs-lookup"><span data-stu-id="49559-250">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="49559-251">Aby to zrobić, Otwórz stronę *\_login. cshtml* w projekcie, a następnie Skomentuj wiersze dla lokalnego panelu logowania i linku rejestracji.</span><span class="sxs-lookup"><span data-stu-id="49559-251">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="49559-252">Strona wyników powinna wyglądać podobnie do następującego przykładu kodu:</span><span class="sxs-lookup"><span data-stu-id="49559-252">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="49559-253">Po wyłączeniu lokalnego panelu logowania i usunięciu linku rejestracji na stronie logowania będą wyświetlane tylko zewnętrzni dostawcy uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="49559-253">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)

---
title: Ustawienia logowania zewnętrznego Google w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Google do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 5b6bfaafba68eaf15a60b7c512a9e7406e3112ee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069122"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="bbd84-103">Ustawienia logowania zewnętrznego Google w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bbd84-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="bbd84-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bbd84-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bbd84-105">W styczniu 2019 Google zaczęła [Zamknij](https://developers.google.com/+/api-shutdown) Google + Zaloguj się i deweloperów należy przenieść do nowego logowania Google w systemie marca.</span><span class="sxs-lookup"><span data-stu-id="bbd84-105">In January 2019 Google started to [shut down](https://developers.google.com/+/api-shutdown) Google+ sign in and developers must move to a new Google sign in system by March.</span></span> <span data-ttu-id="bbd84-106">W lutym w celu uwzględnienia zmiany zostaną zaktualizowane platformy ASP.NET Core 2.1 i 2,2 pakietów dla uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="bbd84-106">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication will be updated in February to accommodate the changes.</span></span> <span data-ttu-id="bbd84-107">Aby uzyskać więcej informacji i tymczasowe środki zaradcze dla platformy ASP.NET Core, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="bbd84-107">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="bbd84-108">W tym samouczku został zaktualizowany przy użyciu nowego procesu instalacji.</span><span class="sxs-lookup"><span data-stu-id="bbd84-108">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="bbd84-109">W tym samouczku dowiesz się, jak umożliwić użytkownikom logowanie za pomocą swojego konta Google przy użyciu projektu ASP.NET Core 2.2, utworzony na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="bbd84-109">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="bbd84-110">Utwórz identyfikator projektu i klienta konsoli interfejsu API Google</span><span class="sxs-lookup"><span data-stu-id="bbd84-110">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="bbd84-111">Przejdź do [integracji Google Zaloguj się do aplikacji sieci web](https://developers.google.com/identity/sign-in/web/devconsole-project) i wybierz **SKONFIGURUJ projekt**.</span><span class="sxs-lookup"><span data-stu-id="bbd84-111">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="bbd84-112">W **Konfigurowanie klienta OAuth** okno dialogowe, wybierz opcję **serwera sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="bbd84-112">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="bbd84-113">W **identyfikatory URI przekierowania autoryzowanych** pole wprowadzania tekstu, Ustaw identyfikator URI przekierowania.</span><span class="sxs-lookup"><span data-stu-id="bbd84-113">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="bbd84-114">Na przykład:`https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="bbd84-114">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="bbd84-115">Zapisz **identyfikator klienta** i **klucz tajny klienta**.</span><span class="sxs-lookup"><span data-stu-id="bbd84-115">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="bbd84-116">W przypadku wdrażania w witrynie, należy zarejestrować nowy publiczny adres url z **konsoli Google**.</span><span class="sxs-lookup"><span data-stu-id="bbd84-116">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="bbd84-117">Store Google ClientID i ClientSecret</span><span class="sxs-lookup"><span data-stu-id="bbd84-117">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="bbd84-118">Store ustawień poufnych, takich jak Google `Client ID` i `Client Secret` z [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bbd84-118">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="bbd84-119">Do celów tego samouczka, nazwa tokeny `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="bbd84-119">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

<span data-ttu-id="bbd84-120">Możesz zarządzać poświadczeniami interfejsu API i jego użycia w [Konsola interfejsu API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="bbd84-120">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="bbd84-121">Konfigurowanie uwierzytelniania serwisu Google</span><span class="sxs-lookup"><span data-stu-id="bbd84-121">Configure Google authentication</span></span>

<span data-ttu-id="bbd84-122">Dodaj usługę Google, aby `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bbd84-122">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="bbd84-123">Zaloguj się przy użyciu Google</span><span class="sxs-lookup"><span data-stu-id="bbd84-123">Sign in with Google</span></span>

* <span data-ttu-id="bbd84-124">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="bbd84-124">Run the app and click **Log in**.</span></span> <span data-ttu-id="bbd84-125">Pojawi się opcja Zaloguj się przy użyciu Google.</span><span class="sxs-lookup"><span data-stu-id="bbd84-125">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="bbd84-126">Kliknij przycisk **Google** przycisk, który przekierowuje do firmy Google do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="bbd84-126">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="bbd84-127">Po wprowadzeniu poświadczeń Google, nastąpi przekierowanie do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="bbd84-127">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="bbd84-128">Zobacz [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="bbd84-128">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="bbd84-129">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="bbd84-129">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="bbd84-130">Zmień domyślny identyfikator URI wywołania zwrotnego</span><span class="sxs-lookup"><span data-stu-id="bbd84-130">Change the default callback URI</span></span>

<span data-ttu-id="bbd84-131">Segmentem identyfikatora URI `/signin-google` jest ustawiony jako zwrotnym domyślnego dostawcę uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="bbd84-131">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="bbd84-132">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania Google za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="bbd84-132">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bbd84-133">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="bbd84-133">Troubleshooting</span></span>

* <span data-ttu-id="bbd84-134">Jeśli nie otrzymujesz błędy logowania nie działa, przełącz się do trybu opracowywania, aby ułatwić debugowanie problemu.</span><span class="sxs-lookup"><span data-stu-id="bbd84-134">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="bbd84-135">Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`podjęto próbę uwierzytelnienia powoduje *ArgumentException: Opcja "SignInScheme" musi być podana*.</span><span class="sxs-lookup"><span data-stu-id="bbd84-135">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="bbd84-136">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="bbd84-136">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="bbd84-137">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="bbd84-137">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="bbd84-138">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="bbd84-138">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbd84-139">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bbd84-139">Next steps</span></span>

* <span data-ttu-id="bbd84-140">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google.</span><span class="sxs-lookup"><span data-stu-id="bbd84-140">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="bbd84-141">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="bbd84-141">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="bbd84-142">Gdy opublikujesz aplikację na platformie Azure, zresetuj `ClientSecret` w konsoli interfejsu API Google.</span><span class="sxs-lookup"><span data-stu-id="bbd84-142">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="bbd84-143">Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="bbd84-143">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="bbd84-144">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="bbd84-144">The configuration system is set up to read keys from environment variables.</span></span>

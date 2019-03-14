---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: 'Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="cdb9e-103">Samouczek: Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cdb9e-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="cdb9e-104">W tym samouczku pokazano, jak utworzyć aplikację sieci web platformy ASP.NET Core przy użyciu interfejsu wiersza polecenia platformy .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="cdb9e-105">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cdb9e-106">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-106">Create a web app project.</span></span>
> * <span data-ttu-id="cdb9e-107">Włączanie obsługi protokołu HTTPS w lokalnym.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="cdb9e-108">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-108">Run the app.</span></span>
> * <span data-ttu-id="cdb9e-109">Edytuj stronę Razor.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-109">Edit a Razor page.</span></span>

<span data-ttu-id="cdb9e-110">Po zakończeniu będziesz mieć działającą aplikację sieci web uruchomiony na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Strona główna aplikacji sieci Web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="cdb9e-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="cdb9e-112">Prerequisites</span></span>

* [<span data-ttu-id="cdb9e-113">.NET core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="cdb9e-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="cdb9e-114">Tworzenie projektu aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="cdb9e-114">Create a web app project</span></span>

<span data-ttu-id="cdb9e-115">Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="cdb9e-116">Włączanie obsługi protokołu HTTPS w lokalnym</span><span class="sxs-lookup"><span data-stu-id="cdb9e-116">Enable local HTTPS</span></span>

<span data-ttu-id="cdb9e-117">Zaufanie certyfikatu deweloperskiego HTTPS:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="cdb9e-118">Windows</span><span class="sxs-lookup"><span data-stu-id="cdb9e-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="cdb9e-119">Poprzednie polecenie wyświetla następujące okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-119">The preceding command displays the following dialog:</span></span>

![Okno dialogowe ostrzeżenia o zabezpieczeniach](~/getting-started/_static/cert.png)

<span data-ttu-id="cdb9e-121">Wybierz **Tak**, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="cdb9e-122">macOS</span><span class="sxs-lookup"><span data-stu-id="cdb9e-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="cdb9e-123">Poprzednie polecenie wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="cdb9e-124">*Zażądano ufające certyfikatu deweloperskiego protokołu HTTPS. Jeśli certyfikat nie jest już zaufany możemy uruchom następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>
 
<span data-ttu-id="cdb9e-125">To polecenie może monitować o hasło zainstalować certyfikat w pęku kluczy systemu.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-125">This command command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="cdb9e-126">Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi programistycznemu.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="cdb9e-127">Linux</span><span class="sxs-lookup"><span data-stu-id="cdb9e-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="cdb9e-128">Sprawdź w dokumentacji dla Twojej dystrybucji systemu Linux informacje na temat dodania certyfikatu deweloperskiego do zaufanych certyfikatów protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-128">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="cdb9e-129">Aby uzyskać więcej informacji, zobacz [ufać certyfikatowi rozwoju platformy ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="cdb9e-129">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="cdb9e-130">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="cdb9e-130">Run the app</span></span>

<span data-ttu-id="cdb9e-131">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="cdb9e-132">Po powłoki poleceń wskazuje, że aplikacja została uruchomiona, przejdź do [ https://localhost:5001 ](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="cdb9e-132">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="cdb9e-133">Kliknij przycisk **Akceptuj**, aby zaakceptować zasady ochrony prywatności i plików cookie.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="cdb9e-134">Ta aplikacja nie zachowuje informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="cdb9e-135">Edytuj stronę Razor</span><span class="sxs-lookup"><span data-stu-id="cdb9e-135">Edit a Razor page</span></span>

<span data-ttu-id="cdb9e-136">Otwórz *Pages/Index.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="cdb9e-137">Przejdź do [ https://localhost:5001 ](https://localhost:5001)i Sprawdź zmiany są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdb9e-138">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="cdb9e-138">Next steps</span></span>

<span data-ttu-id="cdb9e-139">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cdb9e-140">Tworzenie projektu aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-140">Create a web app project.</span></span>
> * <span data-ttu-id="cdb9e-141">Włączanie obsługi protokołu HTTPS w lokalnym.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="cdb9e-142">Uruchom projekt.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-142">Run the project.</span></span>
> * <span data-ttu-id="cdb9e-143">Wprowadź zmiany.</span><span class="sxs-lookup"><span data-stu-id="cdb9e-143">Make a change.</span></span>

<span data-ttu-id="cdb9e-144">Aby dowiedzieć się więcej na temat platformy ASP.NET Core, zobacz ze ścieżką szkoleniową zalecany we wstępie:</span><span class="sxs-lookup"><span data-stu-id="cdb9e-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>

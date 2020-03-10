---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikowanie aplikacji na platformie Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622389"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="a100e-102">Publikowanie aplikacji w usłudze Azure Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a100e-102">Publish the App to Azure Azure App Service</span></span>

<span data-ttu-id="a100e-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a100e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a100e-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="a100e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="a100e-105">Ostatnim krokiem jest opublikowanie aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="a100e-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="a100e-106">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="a100e-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="a100e-107">Kliknięcie przycisku **Publikuj** wywołuje okno dialogowe **Publikowanie w sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="a100e-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="a100e-108">Jeśli zaznaczono opcję **host w chmurze** podczas pierwszego utworzenia projektu, połączenie i ustawienia są już skonfigurowane.</span><span class="sxs-lookup"><span data-stu-id="a100e-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="a100e-109">W takim przypadku po prostu kliknij kartę **Ustawienia** i sprawdź, &quot;wykonaj migracje Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="a100e-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="a100e-110">(Jeśli na początku nie sprawdzono **hosta w chmurze** , wykonaj kroki opisane w [następnej sekcji](#new-website)).</span><span class="sxs-lookup"><span data-stu-id="a100e-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="a100e-111">Aby wdrożyć aplikację, kliknij pozycję **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="a100e-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="a100e-112">Postęp publikowania można wyświetlić w oknie **działanie publikowania w sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="a100e-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="a100e-113">(Z menu **Widok** wybierz pozycję **inne okna**, a następnie wybierz pozycję **aktywność publikacji w sieci Web**).</span><span class="sxs-lookup"><span data-stu-id="a100e-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="a100e-114">Po zakończeniu wdrażania aplikacji w programie Visual Studio zostanie automatycznie uruchomiona przeglądarka domyślna, która przejdzie pod adres URL wdrożonej witryny sieci Web. Utworzona aplikacja jest teraz uruchomione w chmurze.</span><span class="sxs-lookup"><span data-stu-id="a100e-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="a100e-115">Adres URL w pasku adresu przeglądarki pokazuje, że witryna jest ładowana z Internetu.</span><span class="sxs-lookup"><span data-stu-id="a100e-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="a100e-116">Wdrażanie w nowej witrynie sieci Web</span><span class="sxs-lookup"><span data-stu-id="a100e-116">Deploying to a New Website</span></span>

<span data-ttu-id="a100e-117">Jeśli nie sprawdzono **hosta w chmurze** podczas pierwszego tworzenia projektu, można teraz skonfigurować nową aplikację sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a100e-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="a100e-118">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="a100e-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="a100e-119">Wybierz kartę **profil** , a następnie kliknij przycisk **Microsoft Azure Websites**.</span><span class="sxs-lookup"><span data-stu-id="a100e-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="a100e-120">Jeśli nie jesteś zalogowany do Azure, wyświetli się monit o zalogowanie.</span><span class="sxs-lookup"><span data-stu-id="a100e-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="a100e-121">W oknie dialogowym **istniejące witryny sieci Web** kliknij pozycję **Nowy**.</span><span class="sxs-lookup"><span data-stu-id="a100e-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="a100e-122">Wprowadź nazwę lokacji.</span><span class="sxs-lookup"><span data-stu-id="a100e-122">Enter a site name.</span></span> <span data-ttu-id="a100e-123">Wybierz swoją subskrypcję platformy Azure i region.</span><span class="sxs-lookup"><span data-stu-id="a100e-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="a100e-124">W obszarze **serwer bazy danych**wybierz pozycję **Utwórz nowy serwer**lub wybierz istniejący serwer.</span><span class="sxs-lookup"><span data-stu-id="a100e-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="a100e-125">Kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a100e-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="a100e-126">Kliknij kartę **Ustawienia** i sprawdź, &quot;wykonaj migracje Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="a100e-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="a100e-127">Następnie kliknij przycisk **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="a100e-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a100e-128">Wstecz</span><span class="sxs-lookup"><span data-stu-id="a100e-128">Previous</span></span>](part-9.md)

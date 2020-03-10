---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji kodu | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567404"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="52c22-103">ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji kodu</span><span class="sxs-lookup"><span data-stu-id="52c22-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>

<span data-ttu-id="52c22-104">Autor [Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="52c22-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="52c22-105">Pobierz projekt początkowy</span><span class="sxs-lookup"><span data-stu-id="52c22-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="52c22-106">W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="52c22-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="52c22-107">Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="52c22-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="52c22-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="52c22-108">Overview</span></span>

<span data-ttu-id="52c22-109">Po wdrożeniu wstępnym można nadal utrzymywać i opracowywać swoją witrynę sieci Web oraz przed długim wdrożeniem aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="52c22-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="52c22-110">Ten samouczek przeprowadzi Cię przez proces wdrażania aktualizacji w kodzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="52c22-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="52c22-111">Aktualizacja zaimplementowana i wdrażana w tym samouczku nie obejmuje zmiany bazy danych; w następnym samouczku znajdziesz informacje o sposobie wdrażania zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="52c22-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="52c22-112">Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="52c22-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="52c22-113">Wprowadzanie zmiany w kodzie</span><span class="sxs-lookup"><span data-stu-id="52c22-113">Make a code change</span></span>

<span data-ttu-id="52c22-114">Jako prosty przykład aktualizacji aplikacji, dodasz do strony **instruktorów** listę kursów według wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="52c22-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="52c22-115">Jeśli uruchomisz stronę **instruktorów** , zobaczysz, że w siatce znajdują **się linki,** ale nie wykonują żadnych innych czynności niż sprawia, że tło wierszy jest szare.</span><span class="sxs-lookup"><span data-stu-id="52c22-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Strona instruktorów z zaznaczeniem](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="52c22-117">Teraz dodasz kod, który jest uruchamiany, gdy zostanie kliknięty link **Wybierz** i zostanie wyświetlona lista szkoleń szkoleniowych według wybranego instruktora.</span><span class="sxs-lookup"><span data-stu-id="52c22-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="52c22-118">W oknie *instruktors. aspx*Dodaj następujące znaczniki bezpośrednio po kontrolce `Label` **ErrorMessageLabel** :</span><span class="sxs-lookup"><span data-stu-id="52c22-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="52c22-119">Uruchom stronę i wybierz instruktora.</span><span class="sxs-lookup"><span data-stu-id="52c22-119">Run the page and select an instructor.</span></span> <span data-ttu-id="52c22-120">Zostanie wyświetlona lista szkoleń szkoleniowych według tego instruktora.</span><span class="sxs-lookup"><span data-stu-id="52c22-120">You see a list of courses taught by that instructor.</span></span>

    ![Strona instruktorów z nauczaniem kursów](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="52c22-122">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="52c22-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="52c22-123">Wdrażanie aktualizacji kodu w środowisku testowym</span><span class="sxs-lookup"><span data-stu-id="52c22-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="52c22-124">Aby można było używać profilów publikowania do wdrożenia do testowania, przemieszczania i produkcji, należy zmienić opcje publikowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="52c22-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="52c22-125">W przypadku bazy danych członkostwa nie trzeba już uruchamiać skryptów grantu i wdrażania danych.</span><span class="sxs-lookup"><span data-stu-id="52c22-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="52c22-126">Otwórz kreatora **publikacji w sieci Web** , klikając prawym przyciskiem myszy projekt ContosoUniversity i klikając polecenie **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="52c22-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="52c22-127">Kliknij profil **testu** na liście rozwijanej **profil** .</span><span class="sxs-lookup"><span data-stu-id="52c22-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="52c22-128">Kliknij kartę **Ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="52c22-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="52c22-129">W obszarze **DefaultConnection** w sekcji **bazy danych** wyczyść pole wyboru **Aktualizuj bazę danych** .</span><span class="sxs-lookup"><span data-stu-id="52c22-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="52c22-130">Kliknij kartę **profil** , a następnie kliknij pozycję Profil **przemieszczania** na liście rozwijanej **profil** .</span><span class="sxs-lookup"><span data-stu-id="52c22-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="52c22-131">Gdy zostanie wyświetlony monit o zapisanie zmian wprowadzonych w profilu **testu** , kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="52c22-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="52c22-132">Wprowadź tę samą zmianę w profilu przemieszczania.</span><span class="sxs-lookup"><span data-stu-id="52c22-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="52c22-133">Powtórz ten proces, aby wprowadzić tę samą zmianę w profilu produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="52c22-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="52c22-134">Zamknij kreatora **publikacji w sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="52c22-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="52c22-135">Wdrażanie w środowisku testowym jest teraz prostą kwestią ponownego uruchamiania jednego kliknięcia.</span><span class="sxs-lookup"><span data-stu-id="52c22-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="52c22-136">Aby ten proces był szybszy, możesz skorzystać z paska narzędzi **Publikowanie w sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="52c22-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="52c22-137">W menu **Widok** wybierz **paski narzędzi** , a następnie wybierz pozycję **Sieć Web jeden kliknij pozycję Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="52c22-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="52c22-139">W **Eksplorator rozwiązań**wybierz projekt ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="52c22-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="52c22-140">na pasku narzędzi **Publikowanie w sieci Web** , wybierz profil publikacji **test** , a następnie kliknij pozycję **Publikuj sieć Web** (ikona z strzałkami wskazującymi w lewo i w prawo).</span><span class="sxs-lookup"><span data-stu-id="52c22-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="52c22-142">Program Visual Studio wdraża zaktualizowaną aplikację, a przeglądarka automatycznie otwiera stronę główną.</span><span class="sxs-lookup"><span data-stu-id="52c22-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="52c22-143">Uruchom stronę Instruktorzy i wybierz instruktora, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="52c22-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="52c22-144">Zwykle można także przeprowadzić testy regresji (to oznacza pozostałą część lokacji, aby upewnić się, że Nowa zmiana nie przerywa żadnej istniejącej funkcjonalności).</span><span class="sxs-lookup"><span data-stu-id="52c22-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="52c22-145">Jednak w tym samouczku pominiesz ten krok i zaczniesz wdrożyć aktualizację do środowiska przejściowego i produkcyjnego.</span><span class="sxs-lookup"><span data-stu-id="52c22-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="52c22-146">Podczas ponownego wdrażania Web Deploy automatycznie określa, które pliki uległy zmianie, i kopiuje tylko zmienione pliki na serwer.</span><span class="sxs-lookup"><span data-stu-id="52c22-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="52c22-147">Domyślnie Web Deploy używa ostatnich zmienionych dat dla plików, aby określić, które z nich uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="52c22-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="52c22-148">Niektóre systemy kontroli źródła zmieniają daty plików nawet wtedy, gdy nie zmienisz zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="52c22-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="52c22-149">W takim przypadku można skonfigurować Web Deploy do używania sum kontrolnych plików, aby określić, które pliki uległy zmianie.</span><span class="sxs-lookup"><span data-stu-id="52c22-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="52c22-150">Aby uzyskać więcej informacji, zobacz [dlaczego wszystkie moje pliki są ponownie wdrażane, mimo że nie zostały one zmienione?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) w ramach wdrożenia ASP.NET często zadawane pytania.</span><span class="sxs-lookup"><span data-stu-id="52c22-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="52c22-151">Przejmowanie aplikacji w tryb offline podczas wdrażania</span><span class="sxs-lookup"><span data-stu-id="52c22-151">Take the application offline during deployment</span></span>

<span data-ttu-id="52c22-152">Wdrażana zmiana to prosta zmiana jednej strony.</span><span class="sxs-lookup"><span data-stu-id="52c22-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="52c22-153">Czasami jednak wdrażane są większe zmiany lub wdrażane są zarówno zmiany kodu, jak i bazy danych, a lokacja może zachowywać się nieprawidłowo, jeśli użytkownik zażąda strony przed ukończeniem wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="52c22-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="52c22-154">Aby uniemożliwić użytkownikom dostęp do witryny w trakcie wdrażania, można użyć *aplikacji\_pliku offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="52c22-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="52c22-155">Po umieszczeniu pliku o nazwie *app\_offline. htm* w folderze głównym aplikacji usługi IIS automatycznie wyświetlają ten plik zamiast uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="52c22-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="52c22-156">Aby zapobiec uzyskiwaniu dostępu podczas wdrażania, należy umieścić *aplikację\_w trybie offline. htm* w folderze głównym, uruchomić proces wdrażania, a następnie usunąć *aplikację\_offline. htm* po pomyślnym wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="52c22-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="52c22-157">Web Deploy można skonfigurować do automatycznego umieszczania aplikacji domyślnej *\_pliku w trybie offline. htm* na serwerze podczas uruchamiania wdrażania i usuwania go po zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="52c22-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="52c22-158">Aby to zrobić, należy dodać następujący element XML do pliku profilu publikacji (. pubxml):</span><span class="sxs-lookup"><span data-stu-id="52c22-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="52c22-159">W tym samouczku przedstawiono sposób tworzenia i używania aplikacji niestandardowej\_pliku *offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="52c22-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="52c22-160">Korzystanie z *aplikacji\_w trybie offline. htm* w lokacji przemieszczania nie jest wymagane, ponieważ nie masz użytkowników uzyskujących dostęp do lokacji tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="52c22-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="52c22-161">Jest jednak dobrym sposobem na przetestowanie wszystkich sposobów planowania wdrożenia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="52c22-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-app_offlinehtm"></a><span data-ttu-id="52c22-162">Utwórz aplikację\_w trybie offline. htm</span><span class="sxs-lookup"><span data-stu-id="52c22-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="52c22-163">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="52c22-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="52c22-164">Utwórz **stronę HTML** o nazwie *App\_offline. htm* (Usuń ostateczną "l" w rozszerzeniu *HTML* , które domyślnie tworzy program Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="52c22-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="52c22-165">Zastąp znaczniki szablonu następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="52c22-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="52c22-166">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="52c22-166">Save and close the file.</span></span>

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="52c22-167">Kopiuj aplikację\_offline. htm do folderu głównego witryny sieci Web</span><span class="sxs-lookup"><span data-stu-id="52c22-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="52c22-168">Do kopiowania plików do witryny sieci Web można użyć dowolnego narzędzia FTP.</span><span class="sxs-lookup"><span data-stu-id="52c22-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="52c22-169">[FileZilla](http://filezilla-project.org/) to popularne narzędzie FTP, które jest wyświetlane na zrzutach ekranu.</span><span class="sxs-lookup"><span data-stu-id="52c22-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="52c22-170">Aby korzystać z narzędzia FTP, potrzebne są trzy rzeczy: adres URL FTP, nazwa użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="52c22-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="52c22-171">Adres URL jest pokazywany na stronie pulpitu nawigacyjnego witryny sieci Web w usłudze Azure portal zarządzania, a nazwa użytkownika i hasło dla usługi FTP można znaleźć w pliku *publishsettings* , który został wcześniej pobrany.</span><span class="sxs-lookup"><span data-stu-id="52c22-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="52c22-172">Poniższe kroki pokazują, jak uzyskać te wartości.</span><span class="sxs-lookup"><span data-stu-id="52c22-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="52c22-173">W portal zarządzania Azure kliknij kartę **witryny sieci Web** , a następnie kliknij tymczasową witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="52c22-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="52c22-174">Na stronie **pulpit nawigacyjny** przewiń w dół, aby znaleźć nazwę hosta FTP w sekcji **szybki przegląd** .</span><span class="sxs-lookup"><span data-stu-id="52c22-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nazwa hosta FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="52c22-176">Otwórz plik przemieszczania *publishsettings* w Notatniku lub innym edytorze tekstu.</span><span class="sxs-lookup"><span data-stu-id="52c22-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="52c22-177">Znajdź element `publishProfile` dla profilu FTP.</span><span class="sxs-lookup"><span data-stu-id="52c22-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="52c22-178">Skopiuj wartości `userName` i `userPWD`.</span><span class="sxs-lookup"><span data-stu-id="52c22-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Poświadczenia FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="52c22-180">Otwórz narzędzie FTP i zaloguj się do adresu URL FTP.</span><span class="sxs-lookup"><span data-stu-id="52c22-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="52c22-181">Kopiuj *aplikację\_offline. htm* z folderu rozwiązanie do folderu */site/wwwroot* w lokacji tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="52c22-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Kopiuj app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="52c22-183">Przejdź do adresu URL witryny tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="52c22-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="52c22-184">Zobaczysz, że *aplikacja\_w trybie offline. htm* jest teraz wyświetlana zamiast strony głównej.</span><span class="sxs-lookup"><span data-stu-id="52c22-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![app_offline. htm w oknie przeglądarki](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="52c22-186">Teraz można przystąpić do wdrażania do wdrożenia przejściowego.</span><span class="sxs-lookup"><span data-stu-id="52c22-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="52c22-187">Wdrażanie aktualizacji kodu do przemieszczania i produkcji</span><span class="sxs-lookup"><span data-stu-id="52c22-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="52c22-188">W **sieci Web kliknij przycisk Publikuj** na pasku narzędzi, wybierz profil publikowania **przejściowego** , a następnie kliknij pozycję **Publikuj sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="52c22-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="52c22-189">Program Visual Studio wdraża zaktualizowaną aplikację i otwiera przeglądarkę na stronie głównej witryny.</span><span class="sxs-lookup"><span data-stu-id="52c22-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="52c22-190">Zostanie wyświetlony plik *\_w trybie offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="52c22-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="52c22-191">Przed przeprowadzeniem testów w celu zweryfikowania pomyślnego wdrożenia należy usunąć *aplikację\_pliku offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="52c22-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="52c22-192">Wróć do narzędzia FTP i Usuń **aplikację\_w trybie offline. htm** z lokacji tymczasowej.</span><span class="sxs-lookup"><span data-stu-id="52c22-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="52c22-193">W przeglądarce Otwórz stronę instruktorzy w lokacji tymczasowej, a następnie wybierz instruktora, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.</span><span class="sxs-lookup"><span data-stu-id="52c22-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="52c22-194">Postępuj zgodnie z tą samą procedurą dla środowiska produkcyjnego, jak w przypadku przygotowania.</span><span class="sxs-lookup"><span data-stu-id="52c22-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="52c22-195">Przeglądanie zmian i wdrażanie określonych plików</span><span class="sxs-lookup"><span data-stu-id="52c22-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="52c22-196">Program Visual Studio 2012 daje również możliwość wdrażania pojedynczych plików.</span><span class="sxs-lookup"><span data-stu-id="52c22-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="52c22-197">Dla wybranego pliku można wyświetlić różnice między lokalną wersją i wdrożoną wersją, wdrożyć plik w środowisku docelowym lub skopiować plik ze środowiska docelowego do projektu lokalnego.</span><span class="sxs-lookup"><span data-stu-id="52c22-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="52c22-198">W tej części samouczka pokazano, jak korzystać z tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="52c22-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="52c22-199">Wprowadź zmianę do wdrożenia</span><span class="sxs-lookup"><span data-stu-id="52c22-199">Make a change to deploy</span></span>

1. <span data-ttu-id="52c22-200">Otwórz *zawartość/site. css*i Znajdź blok dla tagu `body`.</span><span class="sxs-lookup"><span data-stu-id="52c22-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="52c22-201">Zmień wartość `background-color` z `#fff` na `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="52c22-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="52c22-202">Wyświetl zmianę w oknie publikowania wersji zapoznawczej</span><span class="sxs-lookup"><span data-stu-id="52c22-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="52c22-203">W przypadku opublikowania projektu za pomocą kreatora **publikacji w sieci Web** można zobaczyć, jakie zmiany mają być publikowane przez dwukrotne kliknięcie pliku w oknie **podglądu** .</span><span class="sxs-lookup"><span data-stu-id="52c22-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="52c22-204">Kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij pozycję **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="52c22-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="52c22-205">Z listy rozwijanej **profil** wybierz profil publikacji **testowej** .</span><span class="sxs-lookup"><span data-stu-id="52c22-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="52c22-206">Kliknij pozycję **Podgląd**, a następnie kliknij pozycję **Uruchom podgląd**.</span><span class="sxs-lookup"><span data-stu-id="52c22-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="52c22-207">W okienku **podglądu** kliknij dwukrotnie pozycję **site. css**.</span><span class="sxs-lookup"><span data-stu-id="52c22-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Kliknij dwukrotnie pozycję site. css.](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="52c22-209">Okno dialogowe **Podgląd zmian** zawiera Podgląd zmian, które zostaną wdrożone.</span><span class="sxs-lookup"><span data-stu-id="52c22-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Podgląd zmian w pliku site. CSS](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="52c22-211">W przypadku dwukrotnego kliknięcia pliku *Web. config* okno dialogowe **Podgląd zmian** pokazuje efekt transformacji konfiguracji kompilacji i przekształceń profilu publikowania.</span><span class="sxs-lookup"><span data-stu-id="52c22-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="52c22-212">W tym momencie nie wykonano żadnych czynności, które mogłyby spowodować zmianę pliku *Web. config* na serwerze, aby zobaczyć, że nie wprowadzono żadnych zmian.</span><span class="sxs-lookup"><span data-stu-id="52c22-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="52c22-213">Jednak okno **Podgląd zmian** nieprawidłowo pokazuje dwie zmiany.</span><span class="sxs-lookup"><span data-stu-id="52c22-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="52c22-214">Wygląda na to, że dwa elementy XML zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="52c22-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="52c22-215">Te elementy są dodawane przez proces publikowania po wybraniu opcji **wykonaj migracje Code First przy uruchamianiu aplikacji** dla klasy kontekstu Code First.</span><span class="sxs-lookup"><span data-stu-id="52c22-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="52c22-216">Porównanie jest wykonywane przed dodaniem tych elementów przez proces publikowania, dlatego wygląda na to, że są usuwane, chociaż nie zostaną usunięte.</span><span class="sxs-lookup"><span data-stu-id="52c22-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="52c22-217">Ten błąd zostanie rozwiązany w przyszłej wersji.</span><span class="sxs-lookup"><span data-stu-id="52c22-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="52c22-218">Kliknij przycisk **Zamknij**.</span><span class="sxs-lookup"><span data-stu-id="52c22-218">Click **Close**.</span></span>
6. <span data-ttu-id="52c22-219">Kliknij przycisk **Opublikuj**.</span><span class="sxs-lookup"><span data-stu-id="52c22-219">Click **Publish**.</span></span>
7. <span data-ttu-id="52c22-220">Gdy przeglądarka zostanie otwarta na stronie głównej witryny testowej, naciśnij kombinację klawiszy CTRL + F5, aby wypróbować twarde odświeżanie, aby zobaczyć efekt zmiany w CSS.</span><span class="sxs-lookup"><span data-stu-id="52c22-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Efekt zmiany CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="52c22-222">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="52c22-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="52c22-223">Publikowanie określonych plików z Eksplorator rozwiązań</span><span class="sxs-lookup"><span data-stu-id="52c22-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="52c22-224">Załóżmy, że nie podoba Ci się niebieskie tło i chcesz przywrócić oryginalny kolor.</span><span class="sxs-lookup"><span data-stu-id="52c22-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="52c22-225">W tej sekcji zostaną przywrócone pierwotne ustawienia przez opublikowanie określonego pliku bezpośrednio z **Eksplorator rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="52c22-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="52c22-226">Otwórz *zawartość/site. css* i przywróć ustawienie `background-color`, aby `#fff`.</span><span class="sxs-lookup"><span data-stu-id="52c22-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="52c22-227">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy plik *Content/site. css* .</span><span class="sxs-lookup"><span data-stu-id="52c22-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="52c22-228">Menu kontekstowe zawiera trzy opcje publikacji.</span><span class="sxs-lookup"><span data-stu-id="52c22-228">The context menu shows three publish options.</span></span>

    ![Opcje publikowania z Eksplorator rozwiązań](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="52c22-230">Kliknij pozycję **Podgląd zmian w pliku site. css**.</span><span class="sxs-lookup"><span data-stu-id="52c22-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="52c22-231">Zostanie otwarte okno pokazujące różnice między plikiem lokalnym a jego wersją w środowisku docelowym.</span><span class="sxs-lookup"><span data-stu-id="52c22-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/site. CSS](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="52c22-233">W **Eksplorator rozwiązań**ponownie kliknij prawym przyciskiem myszy **witrynę site. css** , a następnie kliknij pozycję **Publikuj witrynę. css**.</span><span class="sxs-lookup"><span data-stu-id="52c22-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="52c22-234">Okno **działanie publikowania w sieci Web** pokazuje, że plik został opublikowany.</span><span class="sxs-lookup"><span data-stu-id="52c22-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Okno działania publikowania w sieci Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="52c22-236">Otwórz przeglądarkę, aby uzyskać adres URL `http://localhost/contosouniversity`, a następnie naciśnij klawisze CTRL + F5, aby wypróbować twarde odświeżanie, aby zobaczyć efekt zmiany w CSS.</span><span class="sxs-lookup"><span data-stu-id="52c22-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Strona główna z normalnym arkuszem stylów CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="52c22-238">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="52c22-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="52c22-239">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="52c22-239">Summary</span></span>

<span data-ttu-id="52c22-240">Zaobserwowano kilka sposobów wdrażania aktualizacji aplikacji, która nie obejmuje zmian w bazie danych, i przedstawiono sposób wyświetlania podglądu zmian w celu sprawdzenia, czy aktualizacje są aktualne, czego oczekujesz.</span><span class="sxs-lookup"><span data-stu-id="52c22-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="52c22-241">Strona instruktorzy ma teraz sekcję **szkolenia kursów** .</span><span class="sxs-lookup"><span data-stu-id="52c22-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Strona instruktorów z nauczaniem kursów](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="52c22-243">W następnym samouczku pokazano, jak wdrożyć zmianę bazy danych: należy dodać pole DataUrodzenia do bazy danych i do strony instruktorzy.</span><span class="sxs-lookup"><span data-stu-id="52c22-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="52c22-244">[Poprzednie](deploying-to-production.md)
> [dalej](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="52c22-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>

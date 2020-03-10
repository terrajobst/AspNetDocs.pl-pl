---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Samohostowanie interfejsu API sieci Web ASP.NETC#1 () — ASP.NET 4. x
author: MikeWasson
description: Samouczek z kodem pokazuje, jak hostować interfejs API sieci Web w aplikacji konsolowej.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525089"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="2028a-103">Samoobsługowy interfejs API sieci Web ASP.NETC#1 ()</span><span class="sxs-lookup"><span data-stu-id="2028a-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="2028a-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2028a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="2028a-105">W tym samouczku pokazano, jak hostować interfejs API sieci Web w aplikacji konsolowej.</span><span class="sxs-lookup"><span data-stu-id="2028a-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="2028a-106">Interfejs API sieci Web ASP.NET nie wymaga usług IIS.</span><span class="sxs-lookup"><span data-stu-id="2028a-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="2028a-107">Możesz samodzielnie hostować internetowy interfejs API w procesie hosta.</span><span class="sxs-lookup"><span data-stu-id="2028a-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="2028a-108">**Nowe aplikacje powinny używać OWIN do samoobsługowego interfejsu API sieci Web.**</span><span class="sxs-lookup"><span data-stu-id="2028a-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="2028a-109">Zobacz [Używanie Owin do samoobsługowego hostowania ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2028a-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2028a-110">Wersje oprogramowania używane w samouczku</span><span class="sxs-lookup"><span data-stu-id="2028a-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2028a-111">Interfejs API sieci Web 1</span><span class="sxs-lookup"><span data-stu-id="2028a-111">Web API 1</span></span>
> - <span data-ttu-id="2028a-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2028a-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="2028a-113">Tworzenie projektu aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="2028a-113">Create the Console Application Project</span></span>

<span data-ttu-id="2028a-114">Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** .</span><span class="sxs-lookup"><span data-stu-id="2028a-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2028a-115">Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.</span><span class="sxs-lookup"><span data-stu-id="2028a-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2028a-116">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  .</span><span class="sxs-lookup"><span data-stu-id="2028a-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2028a-117">W **obszarze C#Wizualizacja** wybierz pozycję **Windows**.</span><span class="sxs-lookup"><span data-stu-id="2028a-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="2028a-118">Na liście szablonów projektu wybierz pozycję **Aplikacja konsolowa**.</span><span class="sxs-lookup"><span data-stu-id="2028a-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="2028a-119">Nazwij projekt &quot;SelfHost&quot; a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2028a-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="2028a-120">Ustawianie platformy docelowej (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="2028a-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="2028a-121">W przypadku korzystania z programu Visual Studio 2010 Zmień platformę docelową na .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="2028a-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2028a-122">(Domyślnie szablon projektu jest przeznaczony dla [profilu klienta .NET Framework](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)).</span><span class="sxs-lookup"><span data-stu-id="2028a-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="2028a-123">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="2028a-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="2028a-124">Na liście rozwijanej **platforma docelowa** Zmień platformę docelową na .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="2028a-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2028a-125">Po wyświetleniu monitu o zastosowanie zmiany kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="2028a-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="2028a-126">Zainstaluj Menedżera pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="2028a-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="2028a-127">Menedżer pakietów NuGet to najprostszy sposób, aby dodać zestawy interfejsów API sieci Web do projektu non-ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2028a-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="2028a-128">Aby sprawdzić, czy zainstalowano Menedżera pakietów NuGet, kliknij menu **Narzędzia** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2028a-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="2028a-129">Jeśli zobaczysz element menu o nazwie **Menedżer pakietów NuGet**, będziesz mieć Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="2028a-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="2028a-130">Aby zainstalować Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="2028a-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="2028a-131">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2028a-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="2028a-132">W menu **Narzędzia** wybierz pozycję **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="2028a-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="2028a-133">W oknie dialogowym **rozszerzenia i aktualizacje** wybierz pozycję **online**.</span><span class="sxs-lookup"><span data-stu-id="2028a-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2028a-134">Jeśli nie widzisz "Menedżera pakietów NuGet", wpisz "Menedżer pakietów NuGet" w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="2028a-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="2028a-135">Wybierz pozycję Menedżer pakietów NuGet, a następnie kliknij pozycję **Pobierz**.</span><span class="sxs-lookup"><span data-stu-id="2028a-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="2028a-136">Po zakończeniu pobierania zostanie wyświetlony monit o zainstalowanie programu.</span><span class="sxs-lookup"><span data-stu-id="2028a-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="2028a-137">Po zakończeniu instalacji może zostać wyświetlony monit o ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2028a-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="2028a-138">Dodawanie pakietu NuGet interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="2028a-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="2028a-139">Po zainstalowaniu Menedżera pakietów NuGet Dodaj do projektu pakiet samoobsługowy interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="2028a-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="2028a-140">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2028a-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="2028a-141">*Uwaga*: Jeśli nie widzisz tego elementu menu, upewnij się, że Menedżer pakietów NuGet został prawidłowo zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="2028a-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="2028a-142">Wybierz pozycję **Zarządzaj pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="2028a-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="2028a-143">W oknie dialogowym **Zarządzanie pakietami część** wybierz pozycję **online**.</span><span class="sxs-lookup"><span data-stu-id="2028a-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2028a-144">W polu wyszukiwania wpisz &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="2028a-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="2028a-145">Wybierz pakiet internetowego interfejsu API ASP.NET, a następnie kliknij przycisk **Instaluj**.</span><span class="sxs-lookup"><span data-stu-id="2028a-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="2028a-146">Po zainstalowaniu pakietu kliknij przycisk **Zamknij** , aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="2028a-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="2028a-147">Upewnij się, że zainstalowano pakiet o nazwie Microsoft. AspNet. WebApi. SelfHost, a nie AspNetWebApi. SelfHost.</span><span class="sxs-lookup"><span data-stu-id="2028a-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="2028a-148">Tworzenie modelu i kontrolera</span><span class="sxs-lookup"><span data-stu-id="2028a-148">Create the Model and Controller</span></span>

<span data-ttu-id="2028a-149">Ten samouczek używa tych samych klas modelu i kontrolera co samouczek [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="2028a-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="2028a-150">Dodaj klasę publiczną o nazwie `Product`.</span><span class="sxs-lookup"><span data-stu-id="2028a-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="2028a-151">Dodaj klasę publiczną o nazwie `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="2028a-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="2028a-152">Utwórz tę klasę z klasy **System. Web. http. ApiController**.</span><span class="sxs-lookup"><span data-stu-id="2028a-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="2028a-153">Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz samouczek [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="2028a-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="2028a-154">Ten kontroler definiuje trzy akcje GET:</span><span class="sxs-lookup"><span data-stu-id="2028a-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="2028a-155">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="2028a-155">URI</span></span> | <span data-ttu-id="2028a-156">Opis</span><span class="sxs-lookup"><span data-stu-id="2028a-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2028a-157">/api/products</span><span class="sxs-lookup"><span data-stu-id="2028a-157">/api/products</span></span> | <span data-ttu-id="2028a-158">Pobierz listę wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="2028a-158">Get a list of all products.</span></span> |
| <span data-ttu-id="2028a-159">*Identyfikator* /API/Products/</span><span class="sxs-lookup"><span data-stu-id="2028a-159">/api/products/*id*</span></span> | <span data-ttu-id="2028a-160">Pobierz produkt według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="2028a-160">Get a product by ID.</span></span> |
| <span data-ttu-id="2028a-161">/API/Products/? Category =*Kategoria*</span><span class="sxs-lookup"><span data-stu-id="2028a-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="2028a-162">Pobierz listę produktów według kategorii.</span><span class="sxs-lookup"><span data-stu-id="2028a-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="2028a-163">Hostowanie internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="2028a-163">Host the Web API</span></span>

<span data-ttu-id="2028a-164">Otwórz plik Program.cs i Dodaj następujące instrukcje using:</span><span class="sxs-lookup"><span data-stu-id="2028a-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="2028a-165">Dodaj następujący kod do klasy **program** .</span><span class="sxs-lookup"><span data-stu-id="2028a-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="2028a-166">Obowiązkowe Dodawanie rezerwacji przestrzeni nazw adresu URL HTTP</span><span class="sxs-lookup"><span data-stu-id="2028a-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="2028a-167">Ta aplikacja nasłuchuje `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="2028a-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="2028a-168">Domyślnie nasłuchiwanie na określonym adresie HTTP wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="2028a-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="2028a-169">W związku z tym ten błąd może zostać wyświetlony podczas uruchamiania samouczka: "protokół HTTP nie może zarejestrować adresu URL http://+:8080/" Istnieją dwa sposoby uniknięcia tego błędu:</span><span class="sxs-lookup"><span data-stu-id="2028a-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="2028a-170">Uruchom program Visual Studio z podniesionymi uprawnieniami administratora lub</span><span class="sxs-lookup"><span data-stu-id="2028a-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="2028a-171">Użyj narzędzia Netsh. exe, aby nadać kontu uprawnienia do rezerwowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="2028a-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="2028a-172">Aby użyć narzędzia Netsh. exe, Otwórz wiersz polecenia z uprawnieniami administratora i wprowadź następujące polecenie: następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2028a-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="2028a-173">gdzie *machine\username* jest kontem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2028a-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="2028a-174">Po zakończeniu samoobsługowego udostępniania Pamiętaj o usunięciu rezerwacji:</span><span class="sxs-lookup"><span data-stu-id="2028a-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="2028a-175">Wywoływanie interfejsu API sieci Web z aplikacji klienckiej (C#)</span><span class="sxs-lookup"><span data-stu-id="2028a-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="2028a-176">Napiszmy prostą aplikację konsolową, która wywołuje internetowy interfejs API.</span><span class="sxs-lookup"><span data-stu-id="2028a-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="2028a-177">Dodaj nowy projekt aplikacji konsolowej do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="2028a-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="2028a-178">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie i wybierz polecenie **Dodaj nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="2028a-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="2028a-179">Utwórz nową aplikację konsolową o nazwie &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="2028a-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="2028a-180">Użyj Menedżera pakietów NuGet, aby dodać pakiet podstawowych bibliotek interfejsu API sieci Web ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="2028a-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="2028a-181">W menu Narzędzia wybierz pozycję **Menedżer pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2028a-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="2028a-182">Wybierz pozycję **Zarządzaj pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="2028a-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="2028a-183">W oknie dialogowym **Zarządzanie pakietami NuGet** wybierz pozycję **online**.</span><span class="sxs-lookup"><span data-stu-id="2028a-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="2028a-184">W polu wyszukiwania wpisz &quot;Microsoft. AspNet. WebApi. Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="2028a-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="2028a-185">Wybierz pakiet biblioteki klienta interfejsu Web API Microsoft ASP.NET i kliknij przycisk **Instaluj**.</span><span class="sxs-lookup"><span data-stu-id="2028a-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="2028a-186">Dodaj odwołanie w ClientApp do projektu SelfHost:</span><span class="sxs-lookup"><span data-stu-id="2028a-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="2028a-187">W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt ClientApp.</span><span class="sxs-lookup"><span data-stu-id="2028a-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="2028a-188">Wybierz pozycję **Dodaj odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="2028a-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="2028a-189">W oknie dialogowym **Menedżer odwołań** w obszarze **rozwiązanie**wybierz pozycję **projekty**.</span><span class="sxs-lookup"><span data-stu-id="2028a-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="2028a-190">Wybierz projekt SelfHost.</span><span class="sxs-lookup"><span data-stu-id="2028a-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="2028a-191">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="2028a-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="2028a-192">Otwórz plik Client/program. cs.</span><span class="sxs-lookup"><span data-stu-id="2028a-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="2028a-193">Dodaj następującą instrukcję **using** :</span><span class="sxs-lookup"><span data-stu-id="2028a-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="2028a-194">Dodaj statyczne wystąpienie **HttpClient** :</span><span class="sxs-lookup"><span data-stu-id="2028a-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="2028a-195">Dodaj następujące metody, aby wyświetlić listę wszystkich produktów, wyświetlić listę produktów według identyfikatora oraz listę produktów według kategorii.</span><span class="sxs-lookup"><span data-stu-id="2028a-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="2028a-196">Każda z tych metod jest zgodna z tym samym wzorcem:</span><span class="sxs-lookup"><span data-stu-id="2028a-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="2028a-197">Wywołaj **HttpClient. GetAsync** , aby wysłać żądanie Get do odpowiedniego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="2028a-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="2028a-198">Wywołaj metodę **HttpResponseMessage. EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="2028a-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="2028a-199">Ta metoda zgłasza wyjątek, jeśli stan odpowiedzi HTTP to kod błędu.</span><span class="sxs-lookup"><span data-stu-id="2028a-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="2028a-200">Wywołaj **ReadAsAsync&lt;t&gt;** , aby deserializować typ CLR z odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="2028a-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="2028a-201">Ta metoda jest metodą rozszerzającą zdefiniowaną w **systemie .NET. http. HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="2028a-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="2028a-202">Metody **GetAsync** i **ReadAsAsync** są asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="2028a-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="2028a-203">Zwracają one obiekty **zadań** , które reprezentują operację asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="2028a-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="2028a-204">Pobieranie właściwości **wynik** blokuje wątek do momentu zakończenia operacji.</span><span class="sxs-lookup"><span data-stu-id="2028a-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="2028a-205">Aby uzyskać więcej informacji o korzystaniu z programu HttpClient, w tym o sposobie wykonywania wywołań nieblokujących, zobacz [wywoływanie interfejsu API sieci Web z klienta platformy .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="2028a-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="2028a-206">Przed wywołaniem tych metod ustaw właściwość BaseAddress w wystąpieniu HttpClient na wartość "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="2028a-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="2028a-207">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2028a-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="2028a-208">Powinno to spowodować wyjście z poniższych danych.</span><span class="sxs-lookup"><span data-stu-id="2028a-208">This should output the following.</span></span> <span data-ttu-id="2028a-209">(Pamiętaj, aby najpierw uruchomić aplikację SelfHost).</span><span class="sxs-lookup"><span data-stu-id="2028a-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)

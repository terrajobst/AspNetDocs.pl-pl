---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Hosta samodzielnego ASP.NET Web API 1 (C#) — ASP.NET 4.x
author: MikeWasson
description: Samouczek przy użyciu kodu pokazuje, jak hostować interfejs API sieci web w aplikacji konsoli.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7c73bf4734f8ed8a1bf93595c0847f611ad9cc15
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59409606"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="e2639-103">Hosta samodzielnego ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="e2639-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="e2639-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e2639-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e2639-105">W tym samouczku pokazano, jak hostować interfejs API sieci web w aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="e2639-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="e2639-106">ASP.NET Web API nie wymaga usług IIS.</span><span class="sxs-lookup"><span data-stu-id="e2639-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="e2639-107">Interfejs API sieci web można hosta samodzielnego procesu hosta.</span><span class="sxs-lookup"><span data-stu-id="e2639-107">You can self-host a web API in your own host process.</span></span> 
> 
> **<span data-ttu-id="e2639-108">Nowe aplikacje powinny używać OWIN na potrzeby samodzielnego hostowania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e2639-108">New applications should use OWIN to self-host Web API.</span></span>** <span data-ttu-id="e2639-109">Zobacz [korzystanie z OWIN na potrzeby samodzielnego hostowania interfejsu Web API 2 platformy ASP.NET](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e2639-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e2639-110">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="e2639-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e2639-111">Składnik Web API 1</span><span class="sxs-lookup"><span data-stu-id="e2639-111">Web API 1</span></span>
> - <span data-ttu-id="e2639-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e2639-112">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="e2639-113">Utwórz projekt aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="e2639-113">Create the Console Application Project</span></span>

<span data-ttu-id="e2639-114">Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **Start**.</span><span class="sxs-lookup"><span data-stu-id="e2639-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="e2639-115">Możesz również z menu **Plik** wybrać pozycję **Nowy**, a następnie **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e2639-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="e2639-116">W okienku **Szablony** wybierz pozycję **Zainstalowane szablony** i rozwiń węzeł **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="e2639-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e2639-117">W obszarze **Visual C#**, wybierz opcję **Windows**.</span><span class="sxs-lookup"><span data-stu-id="e2639-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="e2639-118">Na liście szablonów projektu wybierz **aplikację Konsolową**.</span><span class="sxs-lookup"><span data-stu-id="e2639-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="e2639-119">Nadaj projektowi nazwę &quot;host własny&quot; i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2639-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="e2639-120">Ustaw docelową platformę (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="e2639-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="e2639-121">Jeśli używasz programu Visual Studio 2010, należy zmienić platformę docelową na .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="e2639-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="e2639-122">(Domyślnie projekt jest ukierunkowany szablonu [profilu programu .net Framework klienta](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="e2639-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="e2639-123">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="e2639-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="e2639-124">W **platformę docelową** listy rozwijanej listy, zmień platformę docelową na .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="e2639-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="e2639-125">Gdy zostanie wyświetlony monit, aby zastosować zmianę, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="e2639-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="e2639-126">Zainstaluj Menedżera pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="e2639-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="e2639-127">Menedżer pakietów NuGet jest najprostszym sposobem dodania zestawy interfejsu API sieci Web do projektu niedotyczący środowiska ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e2639-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="e2639-128">Aby sprawdzić, czy zainstalowano Menedżer pakietów NuGet, kliknij **narzędzia** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2639-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="e2639-129">Jeśli widzisz menu elementu o nazwie **Menedżera pakietów NuGet**, można skorzystać z Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="e2639-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="e2639-130">Aby zainstalować Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="e2639-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="e2639-131">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2639-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="e2639-132">Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="e2639-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="e2639-133">W **rozszerzenia i aktualizacje** okno dialogowe, wybierz opcję **Online**.</span><span class="sxs-lookup"><span data-stu-id="e2639-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="e2639-134">Jeśli nie widzisz "Menedżer pakietów NuGet", wpisz "Menedżer pakietów nuget", w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="e2639-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="e2639-135">Wybierz pozycję Menedżer pakietów NuGet, a następnie kliknij przycisk **Pobierz**.</span><span class="sxs-lookup"><span data-stu-id="e2639-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="e2639-136">Po ukończeniu pobierania pojawi do zainstalowania.</span><span class="sxs-lookup"><span data-stu-id="e2639-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="e2639-137">Po zakończeniu instalacji może być monitowany o ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2639-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="e2639-138">Dodaj pakiet NuGet interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="e2639-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="e2639-139">Po zainstalowaniu Menedżera pakietów NuGet do projektu należy dodać pakiet Self-Host interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="e2639-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="e2639-140">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e2639-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="e2639-141">*Uwaga*: Jeśli nie widzisz tego menu elementu, upewnij się, Menedżer pakietów NuGet, że poprawnie zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="e2639-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="e2639-142">Wybierz **Zarządzaj pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="e2639-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="e2639-143">W **Zarządzaj pakietami NugGet** okno dialogowe, wybierz opcję **Online**.</span><span class="sxs-lookup"><span data-stu-id="e2639-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="e2639-144">W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2639-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="e2639-145">Wybierz pakiet hosta Self interfejsu API sieci Web platformy ASP.NET, a następnie kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="e2639-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="e2639-146">Po zainstalowaniu pakietu kliknij **Zamknij** aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="e2639-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="e2639-147">Upewnij się zainstalować pakiet o nazwie Microsoft.AspNet.WebApi.SelfHost, nie AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="e2639-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="e2639-148">Tworzenie modelu i kontrolera</span><span class="sxs-lookup"><span data-stu-id="e2639-148">Create the Model and Controller</span></span>

<span data-ttu-id="e2639-149">W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="e2639-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="e2639-150">Dodaj publiczny klasę o nazwie `Product`.</span><span class="sxs-lookup"><span data-stu-id="e2639-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="e2639-151">Dodaj publiczny klasę o nazwie `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="e2639-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="e2639-152">Pochodzić z tej klasy z **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="e2639-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="e2639-153">Aby uzyskać więcej informacji o kodzie, w tym kontrolerze, zobacz [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="e2639-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="e2639-154">Ten kontroler definiuje trzy operacje GET:</span><span class="sxs-lookup"><span data-stu-id="e2639-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="e2639-155">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="e2639-155">URI</span></span> | <span data-ttu-id="e2639-156">Opis</span><span class="sxs-lookup"><span data-stu-id="e2639-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e2639-157">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="e2639-157">/api/products</span></span> | <span data-ttu-id="e2639-158">Zostanie wyświetlona lista wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="e2639-158">Get a list of all products.</span></span> |
| <span data-ttu-id="e2639-159">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="e2639-159">/api/products/*id*</span></span> | <span data-ttu-id="e2639-160">Pobieranie produktu według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="e2639-160">Get a product by ID.</span></span> |
| <span data-ttu-id="e2639-161">/ Interfejs API/produkty /? kategorii =*kategorii*</span><span class="sxs-lookup"><span data-stu-id="e2639-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="e2639-162">Zostanie wyświetlona lista produktów według kategorii.</span><span class="sxs-lookup"><span data-stu-id="e2639-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="e2639-163">Hostowanie interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="e2639-163">Host the Web API</span></span>

<span data-ttu-id="e2639-164">Otwórz plik Program.cs i dodaj następujące instrukcje using:</span><span class="sxs-lookup"><span data-stu-id="e2639-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="e2639-165">Dodaj następujący kod do **Program** klasy.</span><span class="sxs-lookup"><span data-stu-id="e2639-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="e2639-166">(Opcjonalnie) Dodaj rezerwacji Namespace adresu URL protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="e2639-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="e2639-167">Ta aplikacja nasłuchuje `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="e2639-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="e2639-168">Domyślna nasłuchiwać pod określonym adresem HTTP wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="e2639-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="e2639-169">Po uruchomieniu tego samouczka, w związku z tym, może wystąpić ten błąd: "HTTP nie można zarejestrować adres URL http://+:8080/" istnieją dwa sposoby, aby uniknąć tego błędu:</span><span class="sxs-lookup"><span data-stu-id="e2639-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="e2639-170">Uruchom program Visual Studio z uprawnieniami administratora z podniesionymi uprawnieniami lub</span><span class="sxs-lookup"><span data-stu-id="e2639-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="e2639-171">Użyj Netsh.exe, aby uzyskać uprawnienia do rezerwowania adresu URL konta.</span><span class="sxs-lookup"><span data-stu-id="e2639-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="e2639-172">Aby użyć Netsh.exe, otwórz wiersz polecenia z uprawnieniami administratora i wpisz następujące polecenie, polecenie: następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="e2639-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="e2639-173">gdzie *maszyna\nazwa_użytkownika* jest kontem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="e2639-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="e2639-174">Po zakończeniu hostingu samodzielnego, upewnij się usunąć zastrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="e2639-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="e2639-175">Wywoływanie interfejsu API sieci Web z aplikacji klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="e2639-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="e2639-176">Napiszmy prostą aplikację konsolową, która wywołuje interfejs API sieci web.</span><span class="sxs-lookup"><span data-stu-id="e2639-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="e2639-177">Dodaj nowy projekt aplikacji konsoli do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="e2639-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="e2639-178">W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy i wybierz **Dodaj nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="e2639-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="e2639-179">Utwórz nową aplikację konsoli o nazwie &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2639-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="e2639-180">Użyj Menedżera pakietów NuGet Dodaj pakiet biblioteki podstawowych interfejsów API sieci Web platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="e2639-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="e2639-181">Wybierz z menu narzędzia **Menedżera pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e2639-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="e2639-182">Wybierz **Zarządzaj pakietami NuGet dla rozwiązania**</span><span class="sxs-lookup"><span data-stu-id="e2639-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="e2639-183">W **Zarządzaj pakietami NuGet** okno dialogowe, wybierz opcję **Online**.</span><span class="sxs-lookup"><span data-stu-id="e2639-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="e2639-184">W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="e2639-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="e2639-185">Wybierz pakiet biblioteki klienta interfejsu API sieci Web platformy ASP.NET firmy Microsoft, a następnie kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="e2639-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="e2639-186">Dodaj odwołanie w ClientApp do projektu własnego hosta:</span><span class="sxs-lookup"><span data-stu-id="e2639-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="e2639-187">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ClientApp.</span><span class="sxs-lookup"><span data-stu-id="e2639-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="e2639-188">Wybierz **Dodaj odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="e2639-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="e2639-189">W **Menadżer odwołań** okna dialogowego, w obszarze **rozwiązania**, wybierz opcję **projektów**.</span><span class="sxs-lookup"><span data-stu-id="e2639-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="e2639-190">Wybierz projekt własnego hosta.</span><span class="sxs-lookup"><span data-stu-id="e2639-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="e2639-191">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2639-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="e2639-192">Otwórz plik Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e2639-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="e2639-193">Dodaj następujący kod **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="e2639-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="e2639-194">Dodaj statyczną **HttpClient** wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="e2639-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="e2639-195">Dodaj następujące metody, aby wyświetlić listę wszystkich produktów, listę produktów, według Identyfikatora i listę produktów według kategorii.</span><span class="sxs-lookup"><span data-stu-id="e2639-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="e2639-196">Każda z tych metod jest zgodna z ten sam wzorcem:</span><span class="sxs-lookup"><span data-stu-id="e2639-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="e2639-197">Wywołaj **HttpClient.GetAsync** wysłanie żądania GET do odpowiedniego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="e2639-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="e2639-198">Wywołaj **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="e2639-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="e2639-199">Ta metoda zgłasza wyjątek, jeśli kod błędu stanu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2639-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="e2639-200">Wywołaj **ReadAsAsync&lt;T&gt;**  deserializować typu CLR na podstawie odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2639-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="e2639-201">Ta metoda jest metodą rozszerzenia, zdefiniowane w **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="e2639-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="e2639-202">**GetAsync** i **ReadAsAsync** metody są asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="e2639-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="e2639-203">Zwracają **zadań** obiekty reprezentujące operację asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="e2639-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="e2639-204">Wprowadzenie **wynik** właściwość blokuje wątek, aż do zakończenia operacji.</span><span class="sxs-lookup"><span data-stu-id="e2639-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="e2639-205">Aby uzyskać więcej informacji na temat za pomocą elementu HttpClient, w tym sposobu wykonywania wywołań, nieblokującą, zobacz [wywołania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="e2639-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="e2639-206">Przed wywołaniem metody te, należy ustawić właściwość BaseAddress na wystąpienie klasy HttpClient "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="e2639-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="e2639-207">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="e2639-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="e2639-208">To powinno danych wyjściowych poniżej.</span><span class="sxs-lookup"><span data-stu-id="e2639-208">This should output the following.</span></span> <span data-ttu-id="e2639-209">(Pamiętaj, aby najpierw uruchom aplikację własnego hosta).</span><span class="sxs-lookup"><span data-stu-id="e2639-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)

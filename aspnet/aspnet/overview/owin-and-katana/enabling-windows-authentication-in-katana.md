---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Włączanie uwierzytelniania systemu Windows w Katana | Microsoft Docs
author: MikeWasson
description: 'W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w Katana. Obejmuje dwa scenariusze: Używanie usług IIS do hostowania Katana i używanie odbiornika HttpListener do samoobsługowego kat —...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617181"
---
# <a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="ce3d9-104">Włączanie uwierzytelniania systemu Windows w projekcie Katana</span><span class="sxs-lookup"><span data-stu-id="ce3d9-104">Enabling Windows Authentication in Katana</span></span>

<span data-ttu-id="ce3d9-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ce3d9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="ce3d9-106">W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w Katana.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="ce3d9-107">Obejmuje dwa scenariusze: użycie usług IIS do hostowania Katana i użycie odbiornika HttpListener do samodzielnego hostowania Katana w procesie niestandardowym.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="ce3d9-108">Dziękujemy za Dorrans, David Matson i Krzysztof Ross w celu przejrzenia tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>

<span data-ttu-id="ce3d9-109">Katana jest implementacją firmy Microsoft [Owin](http://owin.org/), otwartym interfejsem sieci Web dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="ce3d9-110">Wprowadzenie do OWIN i Katana można przeczytać [tutaj](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="ce3d9-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="ce3d9-111">Architektura OWIN ma kilka warstw:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="ce3d9-112">Host: zarządza procesem, w którym działa potok OWIN.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="ce3d9-113">Serwer: otwiera gniazdo sieciowe i nasłuchuje żądań.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="ce3d9-114">Oprogramowanie pośredniczące: przetwarza żądanie HTTP i odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="ce3d9-115">Katana obecnie udostępnia dwa serwery, z których oba obsługują zintegrowane uwierzytelnianie systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="ce3d9-116">**Microsoft. Owin. host. SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="ce3d9-117">Używa usług IIS z potokiem ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="ce3d9-118">**Microsoft. Owin. host. odbiornika HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="ce3d9-119">Używa [systemu .NET. odbiornika HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce3d9-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="ce3d9-120">Ten serwer jest obecnie opcją domyślną w przypadku samoobsługowego Katana.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="ce3d9-121">Usługa Katana obecnie nie udostępnia oprogramowania pośredniczącego OWIN do uwierzytelniania systemu Windows, ponieważ ta funkcja jest już dostępna na serwerach.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="ce3d9-122">Uwierzytelnianie systemu Windows w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="ce3d9-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="ce3d9-123">Korzystając z Microsoft. Owin. host. SystemWeb, można po prostu włączyć uwierzytelnianie systemu Windows w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="ce3d9-124">Zacznijmy od utworzenia nowej aplikacji ASP.NET przy użyciu szablonu projektu "ASP.NET Empty Web Application".</span><span class="sxs-lookup"><span data-stu-id="ce3d9-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="ce3d9-125">Następnie Dodaj pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-125">Next, add NuGet packages.</span></span> <span data-ttu-id="ce3d9-126">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ce3d9-127">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="ce3d9-128">Teraz Dodaj klasę o nazwie `Startup` przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="ce3d9-129">To wszystko, co należy zrobić, aby utworzyć aplikację "Hello World" dla OWIN, działającą w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="ce3d9-130">Naciśnij klawisz F5, aby debugować aplikację.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-130">Press F5 to debug the application.</span></span> <span data-ttu-id="ce3d9-131">Powinna zostać wyświetlona wartość "Hello world!"</span><span class="sxs-lookup"><span data-stu-id="ce3d9-131">You should see "Hello World!"</span></span> <span data-ttu-id="ce3d9-132">w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="ce3d9-133">Następnie włączysz uwierzytelnianie systemu Windows w IIS Express.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="ce3d9-134">Z menu **Widok** wybierz pozycję **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="ce3d9-135">Kliknij nazwę projektu w Eksplorator rozwiązań, aby wyświetlić właściwości projektu.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="ce3d9-136">W oknie **Właściwości** ustaw opcję **uwierzytelnianie anonimowe** na **wyłączone** i ustaw opcję **uwierzytelnianie systemu Windows** na **włączone**.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="ce3d9-137">Po uruchomieniu aplikacji z programu Visual Studio, IIS Express będą wymagały poświadczeń systemu Windows użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="ce3d9-138">Można to sprawdzić za pomocą [programu Fiddler](http://fiddler2.com/home) lub innego narzędzia debugowania http.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="ce3d9-139">Oto przykładowa odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="ce3d9-140">Nagłówki WWW-Authenticate w tej odpowiedzi wskazują, że serwer obsługuje protokół [negocjowania](http://www.ietf.org/rfc/rfc4559.txt) , który korzysta z protokołu Kerberos lub NTLM.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="ce3d9-141">Później podczas wdrażania aplikacji na serwerze wykonaj [następujące kroki](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) , aby włączyć uwierzytelnianie systemu Windows w usługach IIS na tym serwerze.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="ce3d9-142">Uwierzytelnianie systemu Windows w odbiornika HttpListener</span><span class="sxs-lookup"><span data-stu-id="ce3d9-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="ce3d9-143">Jeśli używasz programu Microsoft. Owin. host. odbiornika HttpListener do samoobsługowego Katana, możesz włączyć uwierzytelnianie systemu Windows bezpośrednio w wystąpieniu **odbiornika HttpListener** .</span><span class="sxs-lookup"><span data-stu-id="ce3d9-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="ce3d9-144">Najpierw utwórz nową aplikację konsolową.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-144">First, create a new console application.</span></span> <span data-ttu-id="ce3d9-145">Następnie Dodaj pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-145">Next, add NuGet packages.</span></span> <span data-ttu-id="ce3d9-146">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ce3d9-147">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="ce3d9-148">Teraz Dodaj klasę o nazwie `Startup` przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="ce3d9-149">Ta klasa implementuje ten sam przykład "Hello World" z wcześniej, ale ustawia również uwierzytelnianie systemu Windows jako schemat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="ce3d9-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="ce3d9-150">W funkcji `Main` Uruchom potok OWIN:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="ce3d9-151">Można wysłać żądanie w programu Fiddler, aby potwierdzić, że aplikacja korzysta z uwierzytelniania systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="ce3d9-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="ce3d9-152">Tematy pokrewne</span><span class="sxs-lookup"><span data-stu-id="ce3d9-152">Related Topics</span></span>

[<span data-ttu-id="ce3d9-153">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="ce3d9-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="ce3d9-154">System .NET. odbiornika HttpListener</span><span class="sxs-lookup"><span data-stu-id="ce3d9-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="ce3d9-155">Informacje o uwierzytelnianiu formularzy OWIN w MVC 5</span><span class="sxs-lookup"><span data-stu-id="ce3d9-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)

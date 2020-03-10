---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Praca z protokołem SSL w interfejsie API sieci Web | Microsoft Docs
author: MikeWasson
description: Pokazuje, jak używać protokołu SSL z interfejsem API sieci Web ASP.NET, w tym przy użyciu certyfikatów klienta SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598638"
---
# <a name="working-with-ssl-in-web-api"></a><span data-ttu-id="7be08-103">Praca z protokołem SSL w składniku Web API</span><span class="sxs-lookup"><span data-stu-id="7be08-103">Working with SSL in Web API</span></span>

<span data-ttu-id="7be08-104">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7be08-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7be08-105">Kilka typowych schematów uwierzytelniania nie jest zabezpieczonych za pośrednictwem zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7be08-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="7be08-106">W szczególności uwierzytelnianie podstawowe i uwierzytelnianie formularzy wysyłają nieszyfrowane poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="7be08-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="7be08-107">Aby zapewnić bezpieczeństwo, te schematy uwierzytelniania *muszą* używać protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="7be08-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="7be08-108">Ponadto certyfikaty klienta SSL mogą służyć do uwierzytelniania klientów.</span><span class="sxs-lookup"><span data-stu-id="7be08-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="7be08-109">Włączanie protokołu SSL na serwerze</span><span class="sxs-lookup"><span data-stu-id="7be08-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="7be08-110">Aby skonfigurować protokół SSL w usługach IIS 7 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="7be08-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="7be08-111">Utwórz lub Pobierz certyfikat.</span><span class="sxs-lookup"><span data-stu-id="7be08-111">Create or get a certificate.</span></span> <span data-ttu-id="7be08-112">W celu przetestowania można utworzyć certyfikat z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="7be08-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="7be08-113">Dodaj powiązanie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7be08-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="7be08-114">Aby uzyskać szczegółowe informacje, zobacz [jak skonfigurować protokół SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="7be08-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="7be08-115">W przypadku testowania lokalnego można włączyć protokół SSL w IIS Express z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7be08-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="7be08-116">W okno Właściwości należy ustawić **wartość true**dla **protokołu SSL** .</span><span class="sxs-lookup"><span data-stu-id="7be08-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="7be08-117">Zwróć uwagę na wartość **adresu URL protokołu SSL**; Użyj tego adresu URL do testowania połączeń HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7be08-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="7be08-118">Wymuszanie protokołu SSL w kontrolerze internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="7be08-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="7be08-119">Jeśli masz powiązanie HTTPS i HTTP, klienci mogą nadal korzystać z protokołu HTTP w celu uzyskania dostępu do lokacji.</span><span class="sxs-lookup"><span data-stu-id="7be08-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="7be08-120">Niektóre zasoby mogą być dostępne za pośrednictwem protokołu HTTP, podczas gdy inne zasoby wymagają protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="7be08-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="7be08-121">W takim przypadku należy użyć filtru akcji, aby wymagać protokołu SSL dla chronionych zasobów.</span><span class="sxs-lookup"><span data-stu-id="7be08-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="7be08-122">Poniższy kod przedstawia filtr uwierzytelniania interfejsu API sieci Web, który sprawdza, czy protokół SSL:</span><span class="sxs-lookup"><span data-stu-id="7be08-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="7be08-123">Dodaj ten filtr do wszystkich akcji internetowego interfejsu API, które wymagają protokołu SSL:</span><span class="sxs-lookup"><span data-stu-id="7be08-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="7be08-124">Certyfikaty klienta SSL</span><span class="sxs-lookup"><span data-stu-id="7be08-124">SSL Client Certificates</span></span>

<span data-ttu-id="7be08-125">Protokół SSL zapewnia uwierzytelnianie przy użyciu certyfikatów infrastruktury kluczy publicznych.</span><span class="sxs-lookup"><span data-stu-id="7be08-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="7be08-126">Serwer musi dostarczyć certyfikat uwierzytelniający serwer dla klienta.</span><span class="sxs-lookup"><span data-stu-id="7be08-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="7be08-127">Klient nie może dostarczyć certyfikatu do serwera, ale jest to jedna z opcji uwierzytelniania klientów.</span><span class="sxs-lookup"><span data-stu-id="7be08-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="7be08-128">Aby można było używać certyfikatów klienta z protokołem SSL, musisz mieć możliwość dystrybucji podpisanych certyfikatów do użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7be08-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="7be08-129">W przypadku wielu typów aplikacji nie jest to dobre środowisko użytkownika, ale w niektórych środowiskach (na przykład Enterprise) może być możliwe.</span><span class="sxs-lookup"><span data-stu-id="7be08-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="7be08-130">Zalety</span><span class="sxs-lookup"><span data-stu-id="7be08-130">Advantages</span></span> | <span data-ttu-id="7be08-131">Wady</span><span class="sxs-lookup"><span data-stu-id="7be08-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="7be08-132">-Poświadczenia certyfikatu są takie same, jak nazwa użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="7be08-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="7be08-133">— Protokół SSL zapewnia kompletny bezpieczny kanał, z uwierzytelnianiem, integralnością komunikatów i szyfrowaniem komunikatów.</span><span class="sxs-lookup"><span data-stu-id="7be08-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="7be08-134">— Należy uzyskać certyfikaty PKI i zarządzać nimi.</span><span class="sxs-lookup"><span data-stu-id="7be08-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="7be08-135">-Platforma kliencka musi obsługiwać certyfikaty klienta SSL.</span><span class="sxs-lookup"><span data-stu-id="7be08-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="7be08-136">Aby skonfigurować usługi IIS do akceptowania certyfikatów klienta, Otwórz Menedżera usług IIS i wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7be08-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="7be08-137">Kliknij węzeł lokacja w widoku drzewa.</span><span class="sxs-lookup"><span data-stu-id="7be08-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="7be08-138">Kliknij dwukrotnie funkcję **Ustawienia protokołu SSL** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="7be08-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="7be08-139">W obszarze **Certyfikaty klienta**wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="7be08-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="7be08-140">**Akceptuj**: program IIS zaakceptuje certyfikat od klienta, ale nie wymaga takiego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="7be08-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="7be08-141">**Wymagaj**: Wymagaj certyfikatu klienta.</span><span class="sxs-lookup"><span data-stu-id="7be08-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="7be08-142">(Aby włączyć tę opcję, należy również wybrać opcję "Wymagaj protokołu SSL").</span><span class="sxs-lookup"><span data-stu-id="7be08-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="7be08-143">Możesz również ustawić te opcje w pliku ApplicationHost. config:</span><span class="sxs-lookup"><span data-stu-id="7be08-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="7be08-144">Flaga **SslNegotiateCert** oznacza, że usługi IIS będą akceptować certyfikat od klienta, ale nie wymagają one (odpowiednik opcji "Akceptuj" w Menedżerze usług IIS).</span><span class="sxs-lookup"><span data-stu-id="7be08-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="7be08-145">Aby wymagać certyfikatu, Ustaw flagę **SslRequireCert** .</span><span class="sxs-lookup"><span data-stu-id="7be08-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="7be08-146">W celu przetestowania można również ustawić te opcje w IIS Express w lokalnym hoście aplikacji. Plik konfiguracji znajdujący się w lokalizacji "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="7be08-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="7be08-147">Tworzenie certyfikatu klienta do testowania</span><span class="sxs-lookup"><span data-stu-id="7be08-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="7be08-148">Do celów testowych można użyć programu [Makecert. exe](/windows/desktop/SecCrypto/makecert) , aby utworzyć certyfikat klienta.</span><span class="sxs-lookup"><span data-stu-id="7be08-148">For testing purposes, you can use [MakeCert.exe](/windows/desktop/SecCrypto/makecert) to create a client certificate.</span></span> <span data-ttu-id="7be08-149">Najpierw utwórz testowy główny urząd certyfikacji:</span><span class="sxs-lookup"><span data-stu-id="7be08-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="7be08-150">MakeCert wyświetli monit o wprowadzenie hasła dla klucza prywatnego.</span><span class="sxs-lookup"><span data-stu-id="7be08-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="7be08-151">Następnie Dodaj certyfikat do magazynu "Zaufane główne urzędy certyfikacji" serwera testowego w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="7be08-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="7be08-152">Otwórz program MMC.</span><span class="sxs-lookup"><span data-stu-id="7be08-152">Open MMC.</span></span>
2. <span data-ttu-id="7be08-153">W obszarze **plik**wybierz pozycję **Dodaj/Usuń przystawkę**.</span><span class="sxs-lookup"><span data-stu-id="7be08-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="7be08-154">Wybierz pozycję **konto komputera**.</span><span class="sxs-lookup"><span data-stu-id="7be08-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="7be08-155">Wybierz pozycję **komputer lokalny** i Ukończ pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="7be08-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="7be08-156">W okienku nawigacji rozwiń węzeł "Zaufane główne urzędy certyfikacji".</span><span class="sxs-lookup"><span data-stu-id="7be08-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="7be08-157">W menu **Akcja** wskaż polecenie **wszystkie zadania**, a następnie kliknij przycisk **Importuj** , aby uruchomić Kreatora importu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="7be08-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="7be08-158">Przejdź do pliku certyfikatu, TempCA. cer.</span><span class="sxs-lookup"><span data-stu-id="7be08-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="7be08-159">Kliknij przycisk **Otwórz**, a następnie kliknij przycisk **dalej** i Ukończ pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="7be08-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="7be08-160">(Zostanie wyświetlony monit o ponowne wprowadzenie hasła).</span><span class="sxs-lookup"><span data-stu-id="7be08-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="7be08-161">Teraz Utwórz certyfikat klienta podpisany przy użyciu pierwszego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="7be08-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="7be08-162">Używanie certyfikatów klienta w internetowym interfejsie API</span><span class="sxs-lookup"><span data-stu-id="7be08-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="7be08-163">Po stronie serwera można uzyskać certyfikat klienta, wywołując [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) w komunikacie żądania.</span><span class="sxs-lookup"><span data-stu-id="7be08-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="7be08-164">Metoda zwraca wartość null, jeśli nie ma certyfikatu klienta.</span><span class="sxs-lookup"><span data-stu-id="7be08-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="7be08-165">W przeciwnym razie zwraca wystąpienie **X509Certificate2** .</span><span class="sxs-lookup"><span data-stu-id="7be08-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="7be08-166">Użyj tego obiektu, aby uzyskać informacje z certyfikatu, takie jak wystawca i podmiot.</span><span class="sxs-lookup"><span data-stu-id="7be08-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="7be08-167">Następnie możesz użyć tych informacji do uwierzytelniania i/lub autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="7be08-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]

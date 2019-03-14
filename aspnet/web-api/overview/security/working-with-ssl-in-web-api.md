---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Praca z protokołem SSL w składniku Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: Pokazuje, jak używać protokołu SSL przy użyciu interfejsu API sieci Web platformy ASP.NET, w tym o korzystaniu z certyfikatów klientów SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 69c0d217f605096d968435c062ee9931f8dff75f
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073283"
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="107ca-103">Praca z protokołem SSL w składniku Web API</span><span class="sxs-lookup"><span data-stu-id="107ca-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="107ca-104">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="107ca-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="107ca-105">Kilka typowych schematów uwierzytelniania nie są bezpieczne przy użyciu zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="107ca-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="107ca-106">W szczególności uwierzytelnianie podstawowe i uwierzytelnianie formularzy Wyślij niezaszyfrowane poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="107ca-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="107ca-107">Do zabezpieczenia, te schematy uwierzytelniania *musi* używania protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="107ca-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="107ca-108">Ponadto certyfikaty klienta SSL może służyć do uwierzytelniania klientów.</span><span class="sxs-lookup"><span data-stu-id="107ca-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="107ca-109">Włączanie protokołu SSL na serwerze</span><span class="sxs-lookup"><span data-stu-id="107ca-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="107ca-110">Aby skonfigurować protokół SSL w usługach IIS 7 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="107ca-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="107ca-111">Utwórz lub uzyskać certyfikat.</span><span class="sxs-lookup"><span data-stu-id="107ca-111">Create or get a certificate.</span></span> <span data-ttu-id="107ca-112">W przypadku testowania można utworzyć certyfikatu z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="107ca-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="107ca-113">Dodaj powiązanie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="107ca-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="107ca-114">Aby uzyskać więcej informacji, zobacz [jak się protokołu SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="107ca-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="107ca-115">W przypadku lokalnego testowania można włączyć protokołu SSL w usługach IIS Express z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="107ca-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="107ca-116">W oknie właściwości ustaw **włączony protokół SSL** do **True**.</span><span class="sxs-lookup"><span data-stu-id="107ca-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="107ca-117">Zanotuj wartość ustawienia **adresu URL protokołu SSL**; Użyj tego adresu URL do testowania połączeń HTTPS.</span><span class="sxs-lookup"><span data-stu-id="107ca-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="107ca-118">Wymuszanie protokołu SSL w kontrolerze interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="107ca-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="107ca-119">Jeśli masz zarówno przy użyciu protokołu HTTPS i powiązanie HTTP, klienci nadal mogą używać protokołu HTTP dostęp do witryny.</span><span class="sxs-lookup"><span data-stu-id="107ca-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="107ca-120">Mogą zezwalać na niektóre zasoby, które mają być dostępne za pośrednictwem protokołu HTTP, podczas gdy inne zasoby wymagają protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="107ca-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="107ca-121">W takim przypadku umożliwia filtr akcji wymagać protokołu SSL dla chronionych zasobów.</span><span class="sxs-lookup"><span data-stu-id="107ca-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="107ca-122">Poniższy kod pokazuje filtr uwierzytelniania interfejsu API sieci Web, który sprawdza, czy protokół SSL:</span><span class="sxs-lookup"><span data-stu-id="107ca-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="107ca-123">Dodaj filtr do wszystkich działań interfejsu API sieci Web wymaga protokołu SSL:</span><span class="sxs-lookup"><span data-stu-id="107ca-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="107ca-124">Certyfikaty klienta SSL</span><span class="sxs-lookup"><span data-stu-id="107ca-124">SSL Client Certificates</span></span>

<span data-ttu-id="107ca-125">Protokół SSL umożliwia uwierzytelnianie za pomocą certyfikatów infrastruktury kluczy publicznych.</span><span class="sxs-lookup"><span data-stu-id="107ca-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="107ca-126">Serwer, musisz podać certyfikat, który uwierzytelnia serwer do klienta.</span><span class="sxs-lookup"><span data-stu-id="107ca-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="107ca-127">Mniej typowe dla od klienta zapewnienia certyfikatu do serwera, ale są jedną z opcji uwierzytelnianie klientów.</span><span class="sxs-lookup"><span data-stu-id="107ca-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="107ca-128">Aby używać certyfikatów klientów przy użyciu protokołu SSL, należy rozdystrybuować podpisanych certyfikatów dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="107ca-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="107ca-129">Dla wielu typów aplikacji nie będzie to środowisko użytkownika, ale w niektórych środowiskach (na przykład enterprise) może być możliwe.</span><span class="sxs-lookup"><span data-stu-id="107ca-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="107ca-130">Zalety</span><span class="sxs-lookup"><span data-stu-id="107ca-130">Advantages</span></span> | <span data-ttu-id="107ca-131">Wady</span><span class="sxs-lookup"><span data-stu-id="107ca-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="107ca-132">-Certyfikat poświadczeń są mocniejszych niż nazwy użytkownika i hasła.</span><span class="sxs-lookup"><span data-stu-id="107ca-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="107ca-133">— SSL zapewnia pełną bezpiecznego kanału, za pomocą uwierzytelniania, integralności i szyfrowania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="107ca-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="107ca-134">-Należy uzyskać i zarządzanie certyfikatami infrastruktury kluczy publicznych.</span><span class="sxs-lookup"><span data-stu-id="107ca-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="107ca-135">Platforma klienta muszą obsługiwać certyfikatów klientów SSL.</span><span class="sxs-lookup"><span data-stu-id="107ca-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="107ca-136">Aby skonfigurować usługi IIS, aby zaakceptować certyfikaty klienta, otwórz Menedżera usług IIS i wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="107ca-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="107ca-137">Kliknij węzeł witryny w widoku drzewa.</span><span class="sxs-lookup"><span data-stu-id="107ca-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="107ca-138">Kliknij dwukrotnie **ustawienia protokołu SSL** funkcji w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="107ca-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="107ca-139">W obszarze **certyfikaty klienta**, wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="107ca-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="107ca-140">**Zaakceptuj**: Usługi IIS będzie akceptować certyfikat klienta, ale nie wymaga jednego.</span><span class="sxs-lookup"><span data-stu-id="107ca-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="107ca-141">**Wymagaj**: Wymaga certyfikatu klienta.</span><span class="sxs-lookup"><span data-stu-id="107ca-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="107ca-142">(Aby włączyć tę opcję, należy również wybrać "Wymagaj protokołu SSL")</span><span class="sxs-lookup"><span data-stu-id="107ca-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="107ca-143">Te opcje można również ustawić w pliku ApplicationHost.config:</span><span class="sxs-lookup"><span data-stu-id="107ca-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="107ca-144">**SslNegotiateCert** flagi oznacza, że usługi IIS będzie akceptować certyfikat klienta, ale nie wymaga jednego (odpowiada opcji "Zaakceptuj" w Menedżerze usług IIS).</span><span class="sxs-lookup"><span data-stu-id="107ca-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="107ca-145">Aby jest wymagany certyfikat, należy ustawić **SslRequireCert** flagi.</span><span class="sxs-lookup"><span data-stu-id="107ca-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="107ca-146">W przypadku testowania można również ustawić te opcje w usługach IIS Express, w lokalnym hosta aplikacji. Plik konfiguracji znajduje się w "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="107ca-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="107ca-147">Tworzenie certyfikatu klienta do testowania</span><span class="sxs-lookup"><span data-stu-id="107ca-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="107ca-148">Do celów testowych możesz użyć [MakeCert.exe](/windows/desktop/SecCrypto/makecert) do utworzenia certyfikatu klienta.</span><span class="sxs-lookup"><span data-stu-id="107ca-148">For testing purposes, you can use [MakeCert.exe](/windows/desktop/SecCrypto/makecert) to create a client certificate.</span></span> <span data-ttu-id="107ca-149">Najpierw utwórz urząd główny testu:</span><span class="sxs-lookup"><span data-stu-id="107ca-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="107ca-150">Użycie narzędzia MakeCert wyświetli monit o podanie hasła klucza prywatnego.</span><span class="sxs-lookup"><span data-stu-id="107ca-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="107ca-151">Następnie Dodaj certyfikat do badania serwera "Zaufane główne urzędy certyfikacji" przechowywać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="107ca-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="107ca-152">Otwórz program MMC.</span><span class="sxs-lookup"><span data-stu-id="107ca-152">Open MMC.</span></span>
2. <span data-ttu-id="107ca-153">W obszarze **pliku**, wybierz opcję **Dodaj/Usuń przystawkę**.</span><span class="sxs-lookup"><span data-stu-id="107ca-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="107ca-154">Wybierz **konto komputera**.</span><span class="sxs-lookup"><span data-stu-id="107ca-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="107ca-155">Wybierz **komputera lokalnego** i Ukończ pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="107ca-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="107ca-156">W okienku nawigacji rozwiń węzeł "Zaufane główne urzędy certyfikacji".</span><span class="sxs-lookup"><span data-stu-id="107ca-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="107ca-157">Na **akcji** menu wskaż **wszystkie zadania**, a następnie kliknij przycisk **importu** można uruchomić Kreatora importu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="107ca-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="107ca-158">Przejdź do pliku certyfikatu, TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="107ca-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="107ca-159">Kliknij przycisk **Otwórz**, następnie kliknij przycisk **dalej** i Ukończ pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="107ca-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="107ca-160">(Możesz wyświetli monit o ponowne wprowadzenie hasła.)</span><span class="sxs-lookup"><span data-stu-id="107ca-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="107ca-161">Teraz można utworzyć certyfikatu klienta, który jest podpisany przez pierwszy certyfikat:</span><span class="sxs-lookup"><span data-stu-id="107ca-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="107ca-162">Przy użyciu certyfikatów klienta w interfejsie Web API</span><span class="sxs-lookup"><span data-stu-id="107ca-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="107ca-163">Po stronie serwera można uzyskać certyfikat klienta, przez wywołanie metody [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) komunikatu żądania.</span><span class="sxs-lookup"><span data-stu-id="107ca-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="107ca-164">Metoda zwraca wartość null, jeśli brak certyfikatu klienta.</span><span class="sxs-lookup"><span data-stu-id="107ca-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="107ca-165">W przeciwnym razie zwraca **X509Certificate2** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="107ca-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="107ca-166">Aby uzyskać informacje na podstawie certyfikatu, takie jak wystawcy i tematu, należy użyć tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="107ca-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="107ca-167">Następnie przy użyciu tych informacji do uwierzytelniania i/lub autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="107ca-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]

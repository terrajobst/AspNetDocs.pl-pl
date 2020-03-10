---
uid: whitepapers/aspnet-and-iis6
title: Uruchamianie ASP.NET 1,1 z usługami IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Mimo że system Windows Server 2003 obejmuje zarówno IIS 6,0, jak i ASP.NET 1,1, te składniki są domyślnie wyłączone. W tym dokumencie opisano sposób włączania usług IIS 6,0 a...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523311"
---
# <a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="4266a-104">Uruchamianie platformy ASP.NET 1.1 z usługami IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="4266a-104">Running ASP.NET 1.1 with IIS 6.0</span></span>

> <span data-ttu-id="4266a-105">Mimo że system Windows Server 2003 obejmuje zarówno IIS 6,0, jak i ASP.NET 1,1, te składniki są domyślnie wyłączone.</span><span class="sxs-lookup"><span data-stu-id="4266a-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="4266a-106">W tym dokumencie opisano sposób włączania usług IIS 6,0 i ASP.NET 1,1 oraz zalecane jest użycie kilku ustawień konfiguracyjnych w celu uzyskania optymalnej wydajności usług IIS i ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4266a-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="4266a-107">Dotyczy ASP.NET 1,1 i IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="4266a-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>

<span data-ttu-id="4266a-108">ASP.NET 1,1 jest dostarczany z systemem Windows Server 2003, który obejmuje również najnowszą wersję programu Internet Information Server (IIS) w wersji 6,0.</span><span class="sxs-lookup"><span data-stu-id="4266a-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="4266a-109">Usługi IIS 6,0 i ASP.NET 1,1 są przeznaczone do bezproblemowego integrowania i ASP.NET teraz domyślnie nowemu modelowi procesu roboczego usług IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="4266a-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="4266a-110">ASP.NET 1,1 nie jest instalowany domyślnie</span><span class="sxs-lookup"><span data-stu-id="4266a-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="4266a-111">W przeciwieństwie do poprzednich wersji systemów operacyjnych serwera firmy Microsoft program Internet Information Server (IIS) nie jest domyślnie włączony. nie jest ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="4266a-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="4266a-112">Dostępne są dwie opcje włączania usług IIS:</span><span class="sxs-lookup"><span data-stu-id="4266a-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="4266a-113">Włączanie usług IIS, opcja #1 — Konfigurowanie serwera</span><span class="sxs-lookup"><span data-stu-id="4266a-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="4266a-114">System Windows Server 2003 dostarcza nowego Kreatora konfiguracji serwera, aby pomóc w prawidłowym skonfigurowaniu serwera w żądanym trybie.</span><span class="sxs-lookup"><span data-stu-id="4266a-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="4266a-115">Aby uruchomić kreatora, należy zalogować się jako administrator — przejdź do: Start | Programy | Narzędzia administracyjne i wybierz pozycję "Konfiguruj serwer".</span><span class="sxs-lookup"><span data-stu-id="4266a-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="4266a-116">Po wybraniu powinien zostać wyświetlony ekran otwieranie Kreatora konfiguracji serwera:</span><span class="sxs-lookup"><span data-stu-id="4266a-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="4266a-117">Kliknij przycisk "dalej &gt;":</span><span class="sxs-lookup"><span data-stu-id="4266a-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="4266a-118">Kliknij przycisk "dalej &gt;"</span><span class="sxs-lookup"><span data-stu-id="4266a-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="4266a-119">Na tym ekranie należy wybrać opcję "serwer aplikacji (IIS, ASP.NET)" jako opcje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="4266a-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="4266a-120">Kliknij przycisk "dalej &gt;".</span><span class="sxs-lookup"><span data-stu-id="4266a-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="4266a-121">Po wybraniu opcji skonfigurowania serwera jako serwera aplikacji zostanie wyświetlony monit z pytaniem o to, jakie dodatkowe funkcje należy zainstalować.</span><span class="sxs-lookup"><span data-stu-id="4266a-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="4266a-122">Żadna opcja nie jest zaznaczona domyślnie.</span><span class="sxs-lookup"><span data-stu-id="4266a-122">Neither option is selected by default.</span></span> <span data-ttu-id="4266a-123">Aby włączyć ASP.NET automatycznie, należy wybrać opcję "Włącz ASP. SIEĆ.</span><span class="sxs-lookup"><span data-stu-id="4266a-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="4266a-124">Kliknij przycisk "dalej &gt;".</span><span class="sxs-lookup"><span data-stu-id="4266a-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="4266a-125">Na tym ekranie są wyświetlane opcje, które mają zostać zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="4266a-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="4266a-126">Kliknij przycisk "dalej &gt;".</span><span class="sxs-lookup"><span data-stu-id="4266a-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="4266a-127">Zobaczysz ten ekran podczas instalowania wybranych opcji.</span><span class="sxs-lookup"><span data-stu-id="4266a-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="4266a-128">Po zainstalowaniu usług są wyświetlane inne okna dialogowe.</span><span class="sxs-lookup"><span data-stu-id="4266a-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="4266a-129">Może być również wyświetlany monit o lokalizację instalacyjnego dysku CD systemu Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="4266a-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="4266a-130">Po zakończeniu kliknij przycisk "dalej &gt;".</span><span class="sxs-lookup"><span data-stu-id="4266a-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="4266a-131">Kliknij przycisk "Zakończ" — system Windows Server 2003 jest teraz skonfigurowany do obsługi usług IIS 6,0 i ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="4266a-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="4266a-132">Włączanie usług IIS, opcja #2 — Ręczne konfigurowanie usług IIS i ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4266a-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="4266a-133">Jeśli nie chcesz używać Kreatora konfiguracji serwera, możesz opcjonalnie zainstalować usługi IIS 6,0 i ASP.NET 1,1 przy użyciu apletu "Dodaj lub usuń programy" w panelu sterowania.</span><span class="sxs-lookup"><span data-stu-id="4266a-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="4266a-134">Najpierw otwórz Panel sterowania:</span><span class="sxs-lookup"><span data-stu-id="4266a-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="4266a-135">Następnie kliknij pozycję "Dodaj/Usuń składniki systemu Windows", co spowoduje otwarcie Kreatora składników systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="4266a-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="4266a-136">Zaznacz i zaznacz pole wyboru Serwer aplikacji, a następnie kliknij przycisk Szczegóły?</span><span class="sxs-lookup"><span data-stu-id="4266a-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="4266a-137">przycisk</span><span class="sxs-lookup"><span data-stu-id="4266a-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="4266a-138">Aby zainstalować ASP.NET, zaznacz opcję ASP. SIEĆ.</span><span class="sxs-lookup"><span data-stu-id="4266a-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="4266a-139">Kliknij przycisk OK, aby powrócić do Kreatora składników systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="4266a-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="4266a-140">Kliknij przycisk "dalej &gt;" w Kreatorze składników systemu Windows, aby rozpocząć instalowanie:</span><span class="sxs-lookup"><span data-stu-id="4266a-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="4266a-141">Po zainstalowaniu usług są wyświetlane inne okna dialogowe.</span><span class="sxs-lookup"><span data-stu-id="4266a-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="4266a-142">Może być również wyświetlany monit o lokalizację instalacyjnego dysku CD systemu Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="4266a-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="4266a-143">Po zakończeniu instalacji zostanie wyświetlony ostatni ekran Kreatora składników systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="4266a-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="4266a-144">Usługi IIS 6,0 i ASP.NET 1,1 są teraz skonfigurowane i dostępne.</span><span class="sxs-lookup"><span data-stu-id="4266a-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="4266a-145">Zalecane ustawienia</span><span class="sxs-lookup"><span data-stu-id="4266a-145">Recommended Settings</span></span>

<span data-ttu-id="4266a-146">Podczas pracy z programem ASP.NET 1,1 z usługami IIS 6,0 istnieje kilka ustawień konfiguracji, które są zalecane do uzyskania optymalnej wydajności z ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="4266a-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="4266a-147">Konfigurowanie limitów pamięci procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="4266a-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="4266a-148">Konfigurowanie odtwarzania procesów roboczych</span><span class="sxs-lookup"><span data-stu-id="4266a-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="4266a-149">Konfigurowanie limitów pamięci procesu roboczego</span><span class="sxs-lookup"><span data-stu-id="4266a-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="4266a-150">Domyślnie usługi IIS 6,0 nie ustawiają limitu ilości pamięci, która może być używana przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="4266a-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="4266a-151">ASP.NET. Funkcja pamięci podręcznej sieci polega na ograniczeniu ilości pamięci, aby pamięć podręczna mogła aktywnie usunąć nieużywane elementy z pamięci.</span><span class="sxs-lookup"><span data-stu-id="4266a-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="4266a-152">Zaleca się skonfigurowanie funkcji odtwarzania pamięci w usługach IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="4266a-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="4266a-153">Aby skonfigurować ten otwarty Internet Information Services Manager (Start | Programy | Narzędzia administracyjne | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="4266a-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="4266a-154">Po otwarciu rozwiń folder pule aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4266a-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="4266a-155">Dla każdej puli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4266a-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="4266a-156">Kliknij prawym przyciskiem myszy pulę aplikacji, na przykład "domyślna wartość", a następnie wybierz pozycję "właściwości":</span><span class="sxs-lookup"><span data-stu-id="4266a-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="4266a-157">Następnie Włącz odtwarzanie pamięci, klikając pozycję "Maksymalna użyta pamięć (w megabajtach):".</span><span class="sxs-lookup"><span data-stu-id="4266a-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="4266a-158">Wartość nie powinna być większa niż ilość pamięci fizycznej (nie wirtualnej) na serwerze, dlatego dobre przybliżenie wynosi 60% pamięci fizycznej, tj. dla serwera z 512 MB pamięci fizycznej wybierz 310.</span><span class="sxs-lookup"><span data-stu-id="4266a-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="4266a-159">Zaleca się również, aby maksymalna wartość nie przekraczała 800MB przy użyciu przestrzeni adresowej 2 GB.</span><span class="sxs-lookup"><span data-stu-id="4266a-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="4266a-160">Jeśli przestrzeń adresowa pamięci serwera jest WŁĄCZONĄ, maksymalny limit pamięci dla procesu roboczego może być równy 1, 800MB:</span><span class="sxs-lookup"><span data-stu-id="4266a-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="4266a-161">Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości.</span><span class="sxs-lookup"><span data-stu-id="4266a-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="4266a-162">Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4266a-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="4266a-163">Konfigurowanie odtwarzania procesów roboczych</span><span class="sxs-lookup"><span data-stu-id="4266a-163">Configuring worker recycling</span></span>

<span data-ttu-id="4266a-164">Domyślnie usługi IIS 6,0 są skonfigurowane do odtwarzania procesu roboczego co 29 godzin.</span><span class="sxs-lookup"><span data-stu-id="4266a-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="4266a-165">Jest to bardzo agresywne dla aplikacji z systemem ASP.NET i zaleca się, aby automatyczne odtwarzanie procesów roboczych było wyłączone.</span><span class="sxs-lookup"><span data-stu-id="4266a-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="4266a-166">Aby wyłączyć automatyczne odtwarzanie procesów roboczych, najpierw Otwórz Menedżera Internet Information Services (Start | Programy | Narzędzia administracyjne | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="4266a-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="4266a-167">Po otwarciu rozwiń folder pule aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4266a-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="4266a-168">Dla każdej puli aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4266a-168">For each application pool:</span></span>

1. <span data-ttu-id="4266a-169">Kliknij prawym przyciskiem myszy pulę aplikacji, na przykład "domyślna wartość", a następnie wybierz pozycję "właściwości":</span><span class="sxs-lookup"><span data-stu-id="4266a-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="4266a-170">Usuń zaznaczenie opcji "Odtwórz proces roboczy (w minutach):":</span><span class="sxs-lookup"><span data-stu-id="4266a-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="4266a-171">Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości.</span><span class="sxs-lookup"><span data-stu-id="4266a-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="4266a-172">Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4266a-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="4266a-173">Udzielanie dostępu do zapisu w systemie plików</span><span class="sxs-lookup"><span data-stu-id="4266a-173">Granting write access to the file system</span></span>

<span data-ttu-id="4266a-174">Jeśli aplikacja wymaga dostępu do zapisu w systemie plików i używasz systemu plików NTFS, należy zmodyfikować listę Access Control (ACL) dla folderu lub pliku, aby przyznać ASP.NET dostęp do programu.</span><span class="sxs-lookup"><span data-stu-id="4266a-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="4266a-175">Na przykład, aby udzielić ASP.NET dostępu do zapisu do programu c:\Inetpub\Wwwroot, otwórz Eksploratora i przejdź do katalogu:</span><span class="sxs-lookup"><span data-stu-id="4266a-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="4266a-176">Następnie kliknij prawym przyciskiem myszy katalog, np. "wwwroot" i wybierz polecenie Właściwości.</span><span class="sxs-lookup"><span data-stu-id="4266a-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="4266a-177">Po otwarciu okna dialogowego Właściwości wybierz kartę "zabezpieczenia":</span><span class="sxs-lookup"><span data-stu-id="4266a-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="4266a-178">Katalog c:\InetPub\Wwwroot\ jest specjalnym katalogiem, w którym Specjalna grupa usług IIS 6,0 "IIS\_WPG" ma już przyznane uprawnienia Odczyt &amp; wykonywanie, wyświetlanie zawartości folderu i odczyt.</span><span class="sxs-lookup"><span data-stu-id="4266a-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="4266a-179">Aby jednak udzielić uprawnienia do zapisu, należy kliknąć pole wyboru Zezwalaj na zapis:</span><span class="sxs-lookup"><span data-stu-id="4266a-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="4266a-180">Usługi IIS 6,0 mają teraz uprawnienie do zapisu w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="4266a-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="4266a-181">Aby udzielić uprawnień do zapisu w innych folderach, wykonaj następujące kroki — Uwaga. może być konieczne dodanie grupy IIS\_WPG, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="4266a-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="4266a-182">Przyznanie uprawnienia do zapisu dla usług IIS\_WPG zezwoli na zapisanie w tym katalogu dowolnej aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4266a-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="4266a-183">Obsługa zintegrowanego uwierzytelniania za pomocą SQL Server</span><span class="sxs-lookup"><span data-stu-id="4266a-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="4266a-184">Uwierzytelnianie zintegrowane umożliwia SQL Server do sprawdzania poprawności SQL Server kont logowania przy użyciu uwierzytelniania systemu Windows NT.</span><span class="sxs-lookup"><span data-stu-id="4266a-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="4266a-185">Dzięki temu użytkownik może ominąć standardowy proces logowania SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4266a-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="4266a-186">Dzięki temu użytkownik sieci może uzyskać dostęp do bazy danych SQL Server bez dostarczania oddzielnej identyfikacji logowania lub hasła, ponieważ SQL Server pobiera informacje o użytkowniku i haśle z procesu zabezpieczeń sieci systemu Windows NT.</span><span class="sxs-lookup"><span data-stu-id="4266a-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="4266a-187">Wybór uwierzytelniania zintegrowanego dla aplikacji ASP.NET jest dobrym rozwiązaniem, ponieważ żadne poświadczenia nie są nigdy przechowywane w ciągu połączenia dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4266a-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="4266a-188">Zamiast tego parametry połączenia używane do nawiązania połączenia z serwerem SQL będą wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="4266a-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="4266a-189">Te parametry połączenia mówią, SQL Server używać poświadczeń systemu Windows aplikacji próbującej uzyskać dostęp do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4266a-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="4266a-190">W przypadku ASP.NET/IIS 6 będzie to konto w grupie usługi IIS\_WPG.</span><span class="sxs-lookup"><span data-stu-id="4266a-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="4266a-191">Aby włączyć zintegrowane uwierzytelnianie między SQL Server i ASP.NET, należy najpierw upewnić się, że SQL Server jest skonfigurowany do uwierzytelniania zintegrowanego lub uwierzytelniania w trybie mieszanym — Sprawdź swoją administratora usługi, aby to określić.</span><span class="sxs-lookup"><span data-stu-id="4266a-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="4266a-192">Jeśli SQL Server jest w jednym z tych dwóch trybów, można użyć uwierzytelniania zintegrowanego.</span><span class="sxs-lookup"><span data-stu-id="4266a-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="4266a-193">Otwórz Menedżera SQL Server Enterprise (Start | Programy | Microsoft SQL Server | Enterprise Manager), wybierz odpowiedni serwer i rozwiń folder zabezpieczeń:</span><span class="sxs-lookup"><span data-stu-id="4266a-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="4266a-194">Jeśli grupa "BUILTINT\IIS\_WPG" nie znajduje się na liście, kliknij prawym przyciskiem myszy pozycję logowania i wybierz pozycję "nowy Login':</span><span class="sxs-lookup"><span data-stu-id="4266a-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="4266a-195">W polu tekstowym "Name:" wprowadź wartość "[nazwa serwera/domeny] \IIS\_WPG" lub kliknij przycisk wielokropka, aby otworzyć selektor użytkownika/grupy systemu Windows NT:</span><span class="sxs-lookup"><span data-stu-id="4266a-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="4266a-196">Wybierz grupę IIS\_WPG, a następnie kliknij przycisk "Dodaj" i OK, aby zamknąć selektor.</span><span class="sxs-lookup"><span data-stu-id="4266a-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="4266a-197">Następnie należy również ustawić domyślną bazę danych i uprawnienia dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4266a-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="4266a-198">Aby ustawić domyślną bazę danych, wybierz pozycję z listy rozwijanej, na przykład, poniżej opcji Northwind:</span><span class="sxs-lookup"><span data-stu-id="4266a-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="4266a-199">Następnie kliknij kartę dostęp do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="4266a-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="4266a-200">Kliknij pole wyboru Zezwalaj dla każdej bazy danych, do której chcesz zezwolić na dostęp.</span><span class="sxs-lookup"><span data-stu-id="4266a-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="4266a-201">Należy również wybrać role bazy danych, sprawdzając, czy właściciel\_ma mieć pewność, że logowanie będzie miało wszystkie niezbędne uprawnienia do zarządzania wybraną bazą danych i korzystania z niej.</span><span class="sxs-lookup"><span data-stu-id="4266a-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="4266a-202">Kliknij przycisk OK, aby zamknąć okno dialogowe właściwości.</span><span class="sxs-lookup"><span data-stu-id="4266a-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="4266a-203">Aplikacja ASP.NET jest teraz skonfigurowana do obsługi zintegrowanego uwierzytelniania SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4266a-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="4266a-204">Nie uruchamiaj ASP.NET 1,0 w trybie macierzystym usług IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="4266a-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="4266a-205">ASP.NET 1,0 w usługach IIS 6,0 jest obsługiwana tylko w trybie zgodności usług IIS 5.</span><span class="sxs-lookup"><span data-stu-id="4266a-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="4266a-206">Aby skonfigurować ASP.NET 1,0 do uruchamiania w trybie zgodności usług IIS 5,0, Otwórz menedżer usług internetowych i kliknij prawym przyciskiem myszy witrynę sieci Web i wybierz polecenie Właściwości:</span><span class="sxs-lookup"><span data-stu-id="4266a-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="4266a-207">Przejdź do karty usługa i sprawdź, czy? Czy uruchomić usługę WWW w trybie izolacji usług IIS 5,0?:</span><span class="sxs-lookup"><span data-stu-id="4266a-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)

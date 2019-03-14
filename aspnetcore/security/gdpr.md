---
title: Dane ogólne rozporządzenie o ochronie danych (GDPR) obsługi w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak uzyskać dostęp do punktów rozszerzenia RODO w aplikacji sieci web platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: 5f5ed96354b0b71961c122506602e60b95b809fa
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068201"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="2dab6-103">Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2dab6-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="2dab6-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2dab6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2dab6-105">Platformy ASP.NET Core udostępnia interfejsów API i szablony, które ułatwiają korzystanie z niektórych [Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](https://www.eugdpr.org/) wymagania:</span><span class="sxs-lookup"><span data-stu-id="2dab6-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="2dab6-106">Szablony projektów obejmują punktów rozszerzeń i przekształcone na wycinki kod znaczników, który można zastąpić prywatności i plików cookie zasady.</span><span class="sxs-lookup"><span data-stu-id="2dab6-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="2dab6-107">Funkcja zgody plik cookie umożliwia poprosić o (i śledzenia) zgody od użytkowników do przechowywania informacji osobistych.</span><span class="sxs-lookup"><span data-stu-id="2dab6-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="2dab6-108">Jeśli użytkownik nie wyraził zgodę na zbieranie danych, a aplikacja ma [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) równa `true`, inne niż niezbędne pliki cookie nie są wysyłane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2dab6-108">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="2dab6-109">Pliki cookie mogą być oznaczane jako podstawowe.</span><span class="sxs-lookup"><span data-stu-id="2dab6-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="2dab6-110">Podstawowe pliki cookie są wysyłane do przeglądarki, nawet wtedy, gdy użytkownik nie wyraził zgodę i śledzenie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="2dab6-110">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="2dab6-111">[Pliki cookie sesji i TempData](#tempdata) nie są funkcjonalności, gdy śledzenie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="2dab6-111">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="2dab6-112">[Zarządzanie tożsamościami](#pd) strona zawiera również link do pobierające i usuwające dane użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2dab6-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="2dab6-113">[Przykładową aplikację](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) umożliwia testowanie większość punktów rozszerzenia RODO i interfejsów API dodano szablony platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2dab6-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="2dab6-114">Zobacz [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) testowania instrukcje w pliku.</span><span class="sxs-lookup"><span data-stu-id="2dab6-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="2dab6-115">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2dab6-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="2dab6-116">Obsługa rozporządzenia RODO platformy ASP.NET Core w szablonie wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="2dab6-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="2dab6-117">Strony razor i MVC projekty utworzone za pomocą szablonów projektu obejmują obsługę RODO:</span><span class="sxs-lookup"><span data-stu-id="2dab6-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="2dab6-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) i [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) są ustawiane w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2dab6-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in `Startup`.</span></span>
* <span data-ttu-id="2dab6-119">*_CookieConsentPartial.cshtml* [widoku częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2dab6-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="2dab6-120">*Pages/Privacy.cshtml* strony lub *Views/Home/Privacy.cshtml* widok zawiera stronę, aby szczegółowo opisują zasady zachowania poufności informacji witryny.</span><span class="sxs-lookup"><span data-stu-id="2dab6-120">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="2dab6-121">*_CookieConsentPartial.cshtml* plik generuje łącze do strony ochrony prywatności.</span><span class="sxs-lookup"><span data-stu-id="2dab6-121">The *_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="2dab6-122">W przypadku aplikacji utworzonych za pomocą indywidualnych kont użytkowników, na stronie Zarządzanie zawiera łącza do pobierające i usuwające [dane osobiste użytkownika](#pd).</span><span class="sxs-lookup"><span data-stu-id="2dab6-122">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="2dab6-123">CookiePolicyOptions i UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="2dab6-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="2dab6-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) są inicjowane w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2dab6-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="2dab6-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) jest wywoływana w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2dab6-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="2dab6-126">_CookieConsentPartial.cshtml partial view</span><span class="sxs-lookup"><span data-stu-id="2dab6-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="2dab6-127">The *_CookieConsentPartial.cshtml* partial view:</span><span class="sxs-lookup"><span data-stu-id="2dab6-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="2dab6-128">Tej części:</span><span class="sxs-lookup"><span data-stu-id="2dab6-128">This partial:</span></span>

* <span data-ttu-id="2dab6-129">Pobiera stan śledzenia dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2dab6-129">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="2dab6-130">Jeśli aplikacja jest skonfigurowana do wymagają zgody, użytkownik musi wyrazić zgodę przed pliki cookie mogą być śledzone.</span><span class="sxs-lookup"><span data-stu-id="2dab6-130">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="2dab6-131">Jeśli wymagana jest zgoda, panel zgody plik cookie jest ustalony na górnej części paska nawigacyjnego, utworzone przez *_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="2dab6-131">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *_Layout.cshtml* file.</span></span>
* <span data-ttu-id="2dab6-132">Udostępnia kodu HTML `<p>` element, aby podsumować, prywatności i plików cookie przy użyciu zasad.</span><span class="sxs-lookup"><span data-stu-id="2dab6-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="2dab6-133">Zawiera również link do strony prywatności lub widok, gdzie możesz szczegółowo opisują zasady zachowania poufności informacji witryny.</span><span class="sxs-lookup"><span data-stu-id="2dab6-133">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="2dab6-134">Podstawowe pliki cookie</span><span class="sxs-lookup"><span data-stu-id="2dab6-134">Essential cookies</span></span>

<span data-ttu-id="2dab6-135">Jeśli nie udzielono zgody, tylko pliki cookie oznaczone niezbędne są wysyłane do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="2dab6-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="2dab6-136">Poniższy kod sprawia, że plik cookie jest podstawowe:</span><span class="sxs-lookup"><span data-stu-id="2dab6-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="2dab6-137">Tempdata dostawcy sesji stanu plików cookie i nie są istotne</span><span class="sxs-lookup"><span data-stu-id="2dab6-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="2dab6-138">[Dostawcy Tempdata](xref:fundamentals/app-state#tempdata) pliku cookie nie jest niezbędne.</span><span class="sxs-lookup"><span data-stu-id="2dab6-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="2dab6-139">Jeśli śledzenie jest wyłączone, dostawca Tempdata nie jest funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="2dab6-139">If tracking is disabled, the Tempdata provider isn't functional.</span></span> <span data-ttu-id="2dab6-140">Aby umożliwić dostawcy Tempdata, gdy śledzenie jest wyłączone, Oznacz plik cookie TempData za istotne w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2dab6-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="2dab6-141">[Stan sesji](xref:fundamentals/app-state) pliki cookie nie są istotne.</span><span class="sxs-lookup"><span data-stu-id="2dab6-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="2dab6-142">Stan sesji nie jest funkcjonalnych, jeśli śledzenie jest wyłączone.</span><span class="sxs-lookup"><span data-stu-id="2dab6-142">Session state isn't functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="2dab6-143">Dane osobowe</span><span class="sxs-lookup"><span data-stu-id="2dab6-143">Personal data</span></span>

<span data-ttu-id="2dab6-144">Aplikacje platformy ASP.NET Core utworzonych za pomocą indywidualnych kont użytkowników, to kod, aby pobrać i usunięcie danych osobowych.</span><span class="sxs-lookup"><span data-stu-id="2dab6-144">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="2dab6-145">Wybierz nazwę użytkownika, a następnie wybierz pozycję **danych osobowych**:</span><span class="sxs-lookup"><span data-stu-id="2dab6-145">Select the user name and then select **Personal data**:</span></span>

![Zarządzanie stroną danych osobowych](gdpr/_static/pd.png)

<span data-ttu-id="2dab6-147">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="2dab6-147">Notes:</span></span>

* <span data-ttu-id="2dab6-148">Aby wygenerować `Account/Manage` kod, zobacz [tożsamości szkieletu](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="2dab6-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="2dab6-149">**Usuń** i **Pobierz** łącza działać tylko w przypadku domyślnych danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="2dab6-149">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="2dab6-150">Aplikacje tworzone danych niestandardowych użytkownika musi zostać rozszerzony do usuwania/pobierania danych niestandardowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2dab6-150">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="2dab6-151">Aby uzyskać więcej informacji, zobacz [Dodawanie, pobieranie i usuwanie danych niestandardowych użytkownika tożsamości](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="2dab6-151">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="2dab6-152">Zapisano tokeny dla danego użytkownika, które są przechowywane w tabeli bazy danych tożsamości `AspNetUserTokens` są usuwane po usunięciu użytkownika za pomocą kaskadowych zachowanie dotyczące usuwania ze względu na [klucz obcy](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="2dab6-152">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="2dab6-153">[Uwierzytelnianie zewnętrznego dostawcy](xref:security/authentication/social/index), takie jak Facebook i Google, nie jest dostępna przed zaakceptowaniu zasad pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="2dab6-153">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="2dab6-154">Szyfrowanie w spoczynku</span><span class="sxs-lookup"><span data-stu-id="2dab6-154">Encryption at rest</span></span>

<span data-ttu-id="2dab6-155">Niektóre bazy danych i mechanizmy magazynu umożliwia szyfrowanie danych magazynowanych.</span><span class="sxs-lookup"><span data-stu-id="2dab6-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="2dab6-156">Szyfrowanie danych magazynowanych:</span><span class="sxs-lookup"><span data-stu-id="2dab6-156">Encryption at rest:</span></span>

* <span data-ttu-id="2dab6-157">Automatycznie szyfruje dane przechowywane.</span><span class="sxs-lookup"><span data-stu-id="2dab6-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="2dab6-158">Szyfruje bez konfiguracji, programowania lub innych zadań dotyczących oprogramowania, który uzyskuje dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="2dab6-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="2dab6-159">Jest to najprostszy i najbezpieczniejszy opcja.</span><span class="sxs-lookup"><span data-stu-id="2dab6-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="2dab6-160">Umożliwia zarządzanie kluczami i szyfrowania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="2dab6-160">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="2dab6-161">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2dab6-161">For example:</span></span>

* <span data-ttu-id="2dab6-162">Program Microsoft SQL i Azure SQL udostępniają [funkcji Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="2dab6-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="2dab6-163">Bazy danych w SQL Azure są szyfrowane domyślnie</span><span class="sxs-lookup"><span data-stu-id="2dab6-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="2dab6-164">[Azure obiektów blob, plików, tabela i Queue Storage są domyślnie szyfrowane](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="2dab6-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="2dab6-165">W przypadku baz danych, które nie udostępniają wbudowane szyfrowanie danych magazynowanych można udostępnić taką samą ochronę za pomocą szyfrowania dysków.</span><span class="sxs-lookup"><span data-stu-id="2dab6-165">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="2dab6-166">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2dab6-166">For example:</span></span>

* [<span data-ttu-id="2dab6-167">Funkcja BitLocker dla systemu Windows Server</span><span class="sxs-lookup"><span data-stu-id="2dab6-167">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="2dab6-168">W systemie Linux:</span><span class="sxs-lookup"><span data-stu-id="2dab6-168">Linux:</span></span>
  * [<span data-ttu-id="2dab6-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="2dab6-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="2dab6-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="2dab6-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2dab6-171">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2dab6-171">Additional resources</span></span>

* [<span data-ttu-id="2dab6-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="2dab6-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)

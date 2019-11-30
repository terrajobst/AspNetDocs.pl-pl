---
uid: config-builder
title: Konstruktory konfiguracji dla ASP.NET
author: rick-anderson
description: Dowiedz się, jak pobierać dane konfiguracji ze źródeł innych niż wartości Web. config ze źródeł zewnętrznych.
ms.author: riande
ms.date: 10/29/2018
msc.type: content
ms.openlocfilehash: 5299d9ab057c3096773955a7461e77a80673ebfe
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586764"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="f629e-103">Konstruktory konfiguracji dla ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f629e-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="f629e-104">Autorzy [Stephen Molloy](https://github.com/StephenMolloy) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f629e-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f629e-105">Konstruktory konfiguracji zapewniają nowoczesne i Agile mechanizm dla aplikacji ASP.NET, aby uzyskać wartości konfiguracyjne ze źródeł zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="f629e-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="f629e-106">Konstruktory konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="f629e-106">Configuration builders:</span></span>

* <span data-ttu-id="f629e-107">Są dostępne w .NET Framework 4.7.1 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="f629e-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="f629e-108">Zapewnianie elastycznego mechanizmu odczytywania wartości konfiguracyjnych.</span><span class="sxs-lookup"><span data-stu-id="f629e-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="f629e-109">Należy zająć się niektórymi podstawowymi potrzebami aplikacji, które są przenoszone do kontenera i środowiska ukierunkowanego na chmurę.</span><span class="sxs-lookup"><span data-stu-id="f629e-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="f629e-110">Może służyć do ulepszania ochrony danych konfiguracji przez rysowanie ze źródeł, które były wcześniej niedostępne (na przykład Azure Key Vault i zmienne środowiskowe) w systemie konfiguracji .NET.</span><span class="sxs-lookup"><span data-stu-id="f629e-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="f629e-111">Konstruktory konfiguracji klucza/wartości</span><span class="sxs-lookup"><span data-stu-id="f629e-111">Key/value configuration builders</span></span>

<span data-ttu-id="f629e-112">Typowym scenariuszem, który może być obsługiwany przez konstruktory konfiguracji, jest zapewnienie podstawowego mechanizmu wymiany klucza/wartości dla sekcji konfiguracyjnych, które są zgodne ze wzorcem klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="f629e-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="f629e-113">Koncepcja .NET Framework ConfigurationBuilders nie jest ograniczona do określonych sekcji konfiguracyjnych ani wzorców.</span><span class="sxs-lookup"><span data-stu-id="f629e-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="f629e-114">Jednak wiele konstruktorów konfiguracji w `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) pracuje w obrębie wzorca klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="f629e-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="f629e-115">Ustawienia konstruktorów konfiguracji klucza/wartości</span><span class="sxs-lookup"><span data-stu-id="f629e-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="f629e-116">Następujące ustawienia mają zastosowanie do wszystkich konstruktorów konfiguracji klucza/wartości w `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="f629e-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="f629e-117">Tryb</span><span class="sxs-lookup"><span data-stu-id="f629e-117">Mode</span></span>

<span data-ttu-id="f629e-118">Konstruktory konfiguracji używają zewnętrznego źródła informacji o klucz/wartość, aby wypełnić wybrane elementy klucza/wartości systemu konfiguracyjnego.</span><span class="sxs-lookup"><span data-stu-id="f629e-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="f629e-119">Szczególnie `<appSettings/>` i `<connectionStrings/>` sekcje otrzymują specjalne traktowanie od konstruktorów konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="f629e-120">Konstruktorzy pracują w trzech trybach:</span><span class="sxs-lookup"><span data-stu-id="f629e-120">The builders work in three modes:</span></span>

* <span data-ttu-id="f629e-121">`Strict` — tryb domyślny.</span><span class="sxs-lookup"><span data-stu-id="f629e-121">`Strict` - The default mode.</span></span> <span data-ttu-id="f629e-122">W tym trybie Konstruktor konfiguracji działa tylko w przypadku dobrze znanych sekcji konfiguracji klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="f629e-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="f629e-123">Tryb `Strict` wylicza każdy klucz w sekcji.</span><span class="sxs-lookup"><span data-stu-id="f629e-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="f629e-124">Jeśli w źródle zewnętrznym zostanie znaleziony pasujący klucz:</span><span class="sxs-lookup"><span data-stu-id="f629e-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="f629e-125">Konstruktory konfiguracji zastępują wartość w sekcji konfiguracji w wyniku wartością z zewnętrznego źródła.</span><span class="sxs-lookup"><span data-stu-id="f629e-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="f629e-126">`Greedy` — ten tryb jest ściśle powiązany z trybem `Strict`.</span><span class="sxs-lookup"><span data-stu-id="f629e-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="f629e-127">Zamiast ograniczać się do kluczy, które już istnieją w oryginalnej konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="f629e-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="f629e-128">Konstruktory konfiguracji dodaje wszystkie pary klucz/wartość ze źródła zewnętrznego do sekcji konfiguracji w wyniku.</span><span class="sxs-lookup"><span data-stu-id="f629e-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="f629e-129">`Expand` — działa na nieprzetworzonym kodzie XML przed przeanalizowaniem go do obiektu sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="f629e-130">Można go traktować jako rozszerzenie tokenów w ciągu.</span><span class="sxs-lookup"><span data-stu-id="f629e-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="f629e-131">Każda część nieprzetworzonego ciągu XML, która pasuje do wzorca `${token}` jest kandydatem do rozwinięcia tokenu.</span><span class="sxs-lookup"><span data-stu-id="f629e-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="f629e-132">Jeśli w zewnętrznym źródle nie zostanie znaleziona odpowiednia wartość, token nie zostanie zmieniony.</span><span class="sxs-lookup"><span data-stu-id="f629e-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="f629e-133">Konstruktory w tym trybie nie są ograniczone do sekcji `<appSettings/>` i `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="f629e-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="f629e-134">Poniższe znaczniki z *pliku Web. config* włączają [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) w trybie `Strict`:</span><span class="sxs-lookup"><span data-stu-id="f629e-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="f629e-135">Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` pokazane w poprzednim pliku *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="f629e-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="f629e-136">Poprzedni kod ustawi wartości właściwości na:</span><span class="sxs-lookup"><span data-stu-id="f629e-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="f629e-137">Wartości w pliku *Web. config* , jeśli klucze nie są ustawione w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="f629e-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="f629e-138">Wartości zmiennej środowiskowej, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f629e-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="f629e-139">Na przykład `ServiceID` będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="f629e-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="f629e-140">"ServiceID wartość z pliku Web. config", jeśli zmienna środowiskowa `ServiceID` nie jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f629e-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="f629e-141">Wartość zmiennej środowiskowej `ServiceID`, jeśli została ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f629e-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="f629e-142">Na poniższej ilustracji przedstawiono klucze `<appSettings/>`/wartości z poprzedniego zestawu plików *Web. config* w edytorze środowiska:</span><span class="sxs-lookup"><span data-stu-id="f629e-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Edytor środowiska](config-builder/static/env.png)

<span data-ttu-id="f629e-144">Uwaga: aby zobaczyć zmiany w zmiennych środowiskowych, może być konieczne zamknięcie i ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f629e-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="f629e-145">Obsługa prefiksów</span><span class="sxs-lookup"><span data-stu-id="f629e-145">Prefix handling</span></span>

<span data-ttu-id="f629e-146">Prefiksy kluczy mogą uprościć ustawienie kluczy, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="f629e-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="f629e-147">Konfiguracja .NET Framework jest złożona i zagnieżdżona.</span><span class="sxs-lookup"><span data-stu-id="f629e-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="f629e-148">Zewnętrzne źródła kluczy/wartości są zwykle podstawowe i płaskie według natury.</span><span class="sxs-lookup"><span data-stu-id="f629e-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="f629e-149">Na przykład zmienne środowiskowe nie są zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="f629e-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="f629e-150">Użyj dowolnych z poniższych metod, aby wstrzyknąć zarówno `<appSettings/>`, jak i `<connectionStrings/>` do konfiguracji za pomocą zmiennych środowiskowych:</span><span class="sxs-lookup"><span data-stu-id="f629e-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="f629e-151">Z `EnvironmentConfigBuilder` w domyślnym trybie `Strict` i odpowiednimi nazwami kluczy w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="f629e-152">Powyższy kod i znacznik przyjmuje takie podejście.</span><span class="sxs-lookup"><span data-stu-id="f629e-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="f629e-153">Korzystając z tej metody, **nie** można mieć identycznie nazwanych kluczy w obu `<appSettings/>` i `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="f629e-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="f629e-154">Użyj dwóch `EnvironmentConfigBuilder`s w trybie `Greedy` z unikatowymi prefiksami i `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="f629e-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="f629e-155">Dzięki temu aplikacja może odczytywać `<appSettings/>` i `<connectionStrings/>` bez konieczności aktualizowania pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="f629e-156">W następnej sekcji [stripPrefix](#stripprefix), pokazano, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="f629e-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="f629e-157">Użyj dwóch `EnvironmentConfigBuilder`s w trybie `Greedy` z unikatowymi prefiksami.</span><span class="sxs-lookup"><span data-stu-id="f629e-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="f629e-158">W tym podejściu nie można mieć zduplikowanych nazw kluczy, ponieważ nazwy kluczy muszą się różnić prefiksami.</span><span class="sxs-lookup"><span data-stu-id="f629e-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="f629e-159">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f629e-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="f629e-160">Przy poprzedzającym znaczniku można użyć tego samego prostego źródła klucza/wartości, aby wypełnić konfigurację dla dwóch różnych sekcji.</span><span class="sxs-lookup"><span data-stu-id="f629e-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="f629e-161">Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` klucze/wartości z poprzedniego zestawu plików *Web. config* w edytorze środowiska:</span><span class="sxs-lookup"><span data-stu-id="f629e-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Edytor środowiska](config-builder/static/prefix.png)

<span data-ttu-id="f629e-163">Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` klucze/wartości zawarte w poprzednim pliku *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="f629e-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="f629e-164">Poprzedni kod ustawi wartości właściwości na:</span><span class="sxs-lookup"><span data-stu-id="f629e-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="f629e-165">Wartości w pliku *Web. config* , jeśli klucze nie są ustawione w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="f629e-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="f629e-166">Wartości zmiennej środowiskowej, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f629e-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="f629e-167">Na przykład przy użyciu poprzedniego pliku *Web. config* klucze/wartości w poprzednim obrazie edytora środowiska i Poprzedni kod są ustawiane następujące wartości:</span><span class="sxs-lookup"><span data-stu-id="f629e-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="f629e-168">Key</span><span class="sxs-lookup"><span data-stu-id="f629e-168">Key</span></span>              | <span data-ttu-id="f629e-169">Wartość</span><span class="sxs-lookup"><span data-stu-id="f629e-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="f629e-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="f629e-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="f629e-171">AppSetting_ServiceID ze zmiennych ENV</span><span class="sxs-lookup"><span data-stu-id="f629e-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="f629e-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="f629e-172">AppSetting_default</span></span>            | <span data-ttu-id="f629e-173">AppSetting_default wartość z ENV</span><span class="sxs-lookup"><span data-stu-id="f629e-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="f629e-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="f629e-174">ConnStr_default</span></span>         | <span data-ttu-id="f629e-175">ConnStr_default Val z ENV</span><span class="sxs-lookup"><span data-stu-id="f629e-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="f629e-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="f629e-176">stripPrefix</span></span>

<span data-ttu-id="f629e-177">`stripPrefix`: wartość logiczna, wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="f629e-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="f629e-178">Poprzedzające znaczniki XML oddzielają ustawienia aplikacji od parametrów połączenia, ale wymagają wszystkich kluczy w pliku *Web. config* , aby użyć określonego prefiksu.</span><span class="sxs-lookup"><span data-stu-id="f629e-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="f629e-179">Na przykład prefiks `AppSetting` musi być dodany do klucza `ServiceID` ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="f629e-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="f629e-180">Przy `stripPrefix`, prefiks nie jest używany w pliku *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="f629e-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="f629e-181">Prefiks jest wymagany w źródle Configuration Builder (na przykład w środowisku). Oczekujemy, że większość deweloperów będzie używać `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="f629e-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="f629e-182">Aplikacje zwykle przełączają się na prefiks.</span><span class="sxs-lookup"><span data-stu-id="f629e-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="f629e-183">Poniższy prefiks *Web. config* paski:</span><span class="sxs-lookup"><span data-stu-id="f629e-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="f629e-184">W poprzednim pliku *Web. config* klucz `default` znajduje się zarówno w `<appSettings/>`, jak i `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="f629e-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="f629e-185">Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` klucze/wartości z poprzedniego zestawu plików *Web. config* w edytorze środowiska:</span><span class="sxs-lookup"><span data-stu-id="f629e-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![Edytor środowiska](config-builder/static/prefix.png)

<span data-ttu-id="f629e-187">Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` klucze/wartości zawarte w poprzednim pliku *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="f629e-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="f629e-188">Poprzedni kod ustawi wartości właściwości na:</span><span class="sxs-lookup"><span data-stu-id="f629e-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="f629e-189">Wartości w pliku *Web. config* , jeśli klucze nie są ustawione w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="f629e-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="f629e-190">Wartości zmiennej środowiskowej, jeśli jest ustawiona.</span><span class="sxs-lookup"><span data-stu-id="f629e-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="f629e-191">Na przykład przy użyciu poprzedniego pliku *Web. config* klucze/wartości w poprzednim obrazie edytora środowiska i Poprzedni kod są ustawiane następujące wartości:</span><span class="sxs-lookup"><span data-stu-id="f629e-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="f629e-192">Key</span><span class="sxs-lookup"><span data-stu-id="f629e-192">Key</span></span>              | <span data-ttu-id="f629e-193">Wartość</span><span class="sxs-lookup"><span data-stu-id="f629e-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="f629e-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="f629e-194">ServiceID</span></span>           | <span data-ttu-id="f629e-195">AppSetting_ServiceID ze zmiennych ENV</span><span class="sxs-lookup"><span data-stu-id="f629e-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="f629e-196">{1&gt;default&lt;1}</span><span class="sxs-lookup"><span data-stu-id="f629e-196">default</span></span>            | <span data-ttu-id="f629e-197">AppSetting_default wartość z ENV</span><span class="sxs-lookup"><span data-stu-id="f629e-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="f629e-198">{1&gt;default&lt;1}</span><span class="sxs-lookup"><span data-stu-id="f629e-198">default</span></span>         | <span data-ttu-id="f629e-199">ConnStr_default Val z ENV</span><span class="sxs-lookup"><span data-stu-id="f629e-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="f629e-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="f629e-200">tokenPattern</span></span>

<span data-ttu-id="f629e-201">`tokenPattern`: ciąg, wartość domyślna to `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="f629e-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="f629e-202">Zachowanie `Expand` dla konstruktorów przeszukuje nieprzetworzony kod XML pod kątem tokenów, które wyglądają jak `${token}`.</span><span class="sxs-lookup"><span data-stu-id="f629e-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="f629e-203">Wyszukiwanie jest wykonywane z domyślnym wyrażeniem regularnym `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="f629e-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="f629e-204">Zestaw znaków, które pasują do `\w`, jest bardziej rygorystyczny niż XML i wiele źródeł konfiguracji Zezwalaj.</span><span class="sxs-lookup"><span data-stu-id="f629e-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="f629e-205">Użyj `tokenPattern`, gdy nazwa tokenu wymaga więcej znaków niż `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="f629e-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="f629e-206">`tokenPattern`: ciąg:</span><span class="sxs-lookup"><span data-stu-id="f629e-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="f629e-207">Umożliwia deweloperom zmianę wyrażenia regularnego używanego do dopasowywania tokenu.</span><span class="sxs-lookup"><span data-stu-id="f629e-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="f629e-208">Sprawdzanie poprawności nie jest wykonywane, aby upewnić się, że jest to dobrze sformułowane wyrażenie regularne.</span><span class="sxs-lookup"><span data-stu-id="f629e-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="f629e-209">Musi zawierać grupę przechwytywania.</span><span class="sxs-lookup"><span data-stu-id="f629e-209">It must contain a capture group.</span></span> <span data-ttu-id="f629e-210">Całe wyrażenie regularne musi być zgodne z całym tokenem.</span><span class="sxs-lookup"><span data-stu-id="f629e-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="f629e-211">Pierwsze przechwytywanie musi być nazwą tokenu, aby wyszukać w źródle konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="f629e-212">Konstruktory konfiguracji w programie Microsoft. Configuration. ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="f629e-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="f629e-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f629e-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="f629e-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="f629e-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="f629e-215">Jest najprostszym z konstruktorów konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="f629e-216">Odczytuje wartości ze środowiska.</span><span class="sxs-lookup"><span data-stu-id="f629e-216">Reads values from the environment.</span></span>
* <span data-ttu-id="f629e-217">Nie ma żadnych dodatkowych opcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="f629e-218">Wartość atrybutu `name` jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="f629e-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="f629e-219">**Uwaga:** W środowisku kontenera systemu Windows zmienne ustawione w czasie wykonywania są wstrzykiwane tylko do środowiska procesu EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="f629e-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="f629e-220">Aplikacje uruchamiane jako usługa lub proces poza elementem EntryPoint nie pobierają tych zmiennych, chyba że są one wprowadzane przez mechanizm w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="f629e-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="f629e-221">W przypadku [usług IIS](https://github.com/Microsoft/iis-docker/pull/41)/kontenerów opartych na [ASP.NET](https://github.com/Microsoft/aspnet-docker), bieżąca wersja elementu [ServiceMonitor. exe](https://github.com/Microsoft/iis-docker/pull/41) obsługuje tę *opcję tylko w ramach tej* usługi.</span><span class="sxs-lookup"><span data-stu-id="f629e-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="f629e-222">Inne warianty kontenerów opartych na systemie Windows mogą wymagać opracowania własnego mechanizmu wstrzykiwania dla procesów poza punktami wejścia.</span><span class="sxs-lookup"><span data-stu-id="f629e-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="f629e-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f629e-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="f629e-224">Nigdy nie przechowuj haseł, poufnych parametrów połączenia lub innych poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="f629e-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="f629e-225">Wpisy tajne produkcji nie powinny być używane do celów deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="f629e-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="f629e-226">Ten konstruktor konfiguracji udostępnia funkcję podobną do [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f629e-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="f629e-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) można używać w projektach .NET Framework, ale musi być określony plik tajny.</span><span class="sxs-lookup"><span data-stu-id="f629e-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="f629e-228">Alternatywnie można zdefiniować Właściwość `UserSecretsId` w pliku projektu i utworzyć Nieprzetworzony plik tajny we właściwym miejscu do odczytu.</span><span class="sxs-lookup"><span data-stu-id="f629e-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="f629e-229">Aby zachować zależności zewnętrzne poza projektem, plik tajny jest sformatowany w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="f629e-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="f629e-230">Formatowanie XML jest szczegółami implementacji i nie powinno się opierać na tym formacie.</span><span class="sxs-lookup"><span data-stu-id="f629e-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="f629e-231">Jeśli musisz udostępnić plik *tajny. JSON* z projektami .NET Core, rozważ użycie [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span><span class="sxs-lookup"><span data-stu-id="f629e-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfigbuilder).</span></span> <span data-ttu-id="f629e-232">Format `SimpleJsonConfigBuilder` dla platformy .NET Core powinien również być traktowany jak szczegóły implementacji, aby zmienić.</span><span class="sxs-lookup"><span data-stu-id="f629e-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="f629e-233">Atrybuty konfiguracji dla `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f629e-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="f629e-234">`userSecretsId` — jest to preferowana metoda identyfikacji pliku tajnego XML.</span><span class="sxs-lookup"><span data-stu-id="f629e-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="f629e-235">Działa podobnie do programu .NET Core, który używa właściwości projektu `UserSecretsId` do przechowywania tego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="f629e-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="f629e-236">Ciąg musi być unikatowy, nie musi być identyfikatorem GUID.</span><span class="sxs-lookup"><span data-stu-id="f629e-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="f629e-237">W tym atrybucie `UserSecretsConfigBuilder` sprawdza dobrze znaną lokalizację lokalną (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) dla pliku tajnego należącego do tego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="f629e-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="f629e-238">`userSecretsFile` — opcjonalny atrybut określający plik zawierający wpisy tajne.</span><span class="sxs-lookup"><span data-stu-id="f629e-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="f629e-239">Znaku `~` można użyć na początku, aby odwołać się do katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f629e-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="f629e-240">Wymagany jest ten atrybut lub atrybut `userSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="f629e-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="f629e-241">Jeśli są określone oba te elementy, `userSecretsFile` ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="f629e-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="f629e-242">`optional`: Boolean, wartość domyślna `true` — uniemożliwia wyjątek, jeśli nie można znaleźć pliku tajnego.</span><span class="sxs-lookup"><span data-stu-id="f629e-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="f629e-243">Wartość atrybutu `name` jest dowolną.</span><span class="sxs-lookup"><span data-stu-id="f629e-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="f629e-244">Plik tajny ma następujący format:</span><span class="sxs-lookup"><span data-stu-id="f629e-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="f629e-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f629e-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="f629e-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) odczytuje wartości przechowywane w [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="f629e-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="f629e-247">`vaultName` jest wymagana (nazwa magazynu lub identyfikator URI magazynu).</span><span class="sxs-lookup"><span data-stu-id="f629e-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="f629e-248">Inne atrybuty umożliwiają kontrolę nad tym, z którym magazynem ma zostać nawiązane połączenie, ale tylko wtedy, gdy aplikacja nie działa w środowisku, które współpracuje z `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="f629e-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="f629e-249">Biblioteka uwierzytelniania usług platformy Azure służy do automatycznego pobierania informacji o połączeniu ze środowiska wykonywania, jeśli jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="f629e-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="f629e-250">Można przesłonić automatyczne pobranie informacji o połączeniu, podając parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="f629e-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="f629e-251">`vaultName` — wymagane, jeśli nie określono `uri`.</span><span class="sxs-lookup"><span data-stu-id="f629e-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="f629e-252">Określa nazwę magazynu w ramach subskrypcji platformy Azure, z którego ma zostać odczytana para klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="f629e-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="f629e-253">`connectionString` — parametry połączenia używane przez [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="f629e-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="f629e-254">`uri` — nawiązuje połączenie z innymi dostawcami Key Vault o określonej wartości `uri`.</span><span class="sxs-lookup"><span data-stu-id="f629e-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="f629e-255">Jeśli nie zostanie określony, platforma Azure (`vaultName`) jest dostawcą magazynu.</span><span class="sxs-lookup"><span data-stu-id="f629e-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="f629e-256">`version`-Azure Key Vault udostępnia funkcję przechowywania wersji dla wpisów tajnych.</span><span class="sxs-lookup"><span data-stu-id="f629e-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="f629e-257">Jeśli określono `version`, Konstruktor pobiera tylko wpisy tajne pasujące do tej wersji.</span><span class="sxs-lookup"><span data-stu-id="f629e-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="f629e-258">`preloadSecretNames` — domyślnie ten Konstruktor bada **wszystkie** nazwy kluczy w magazynie kluczy, gdy jest zainicjowany.</span><span class="sxs-lookup"><span data-stu-id="f629e-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="f629e-259">Aby uniemożliwić odczytywanie wszystkich wartości klucza, ustaw ten atrybut na `false`.</span><span class="sxs-lookup"><span data-stu-id="f629e-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="f629e-260">Ustawienie tej opcji na `false` odczytuje wpisy tajne po jednej naraz.</span><span class="sxs-lookup"><span data-stu-id="f629e-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="f629e-261">Odczytywanie wpisów tajnych po jednej naraz może być przydatne, jeśli magazyn zezwala na dostęp "Get", ale nie do "list".</span><span class="sxs-lookup"><span data-stu-id="f629e-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="f629e-262">**Uwaga:** W przypadku używania `Greedy` trybu `preloadSecretNames` musi być `true` (wartość domyślna).</span><span class="sxs-lookup"><span data-stu-id="f629e-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="f629e-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f629e-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="f629e-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) to podstawowy Konstruktor konfiguracji, który używa plików katalogu jako źródła wartości.</span><span class="sxs-lookup"><span data-stu-id="f629e-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="f629e-265">Nazwa pliku jest kluczem, a zawartość jest wartością.</span><span class="sxs-lookup"><span data-stu-id="f629e-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="f629e-266">Ten konstruktor konfiguracji może być przydatny w przypadku uruchamiania w środowisku organizowania kontenerów.</span><span class="sxs-lookup"><span data-stu-id="f629e-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="f629e-267">Systemy takie jak Docker Swarm i Kubernetes zapewniają `secrets` w zorganizowanych kontenerach systemu Windows w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="f629e-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="f629e-268">Szczegóły atrybutu:</span><span class="sxs-lookup"><span data-stu-id="f629e-268">Attribute details:</span></span>

* <span data-ttu-id="f629e-269">`directoryPath` — wymagane.</span><span class="sxs-lookup"><span data-stu-id="f629e-269">`directoryPath` - Required.</span></span> <span data-ttu-id="f629e-270">Określa ścieżkę do przeszukiwania wartości.</span><span class="sxs-lookup"><span data-stu-id="f629e-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="f629e-271">Wpisy tajne Docker for Windows są domyślnie przechowywane w katalogu *C:\ProgramData\Docker\secrets* .</span><span class="sxs-lookup"><span data-stu-id="f629e-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="f629e-272">`ignorePrefix` — pliki, które zaczynają się od tego prefiksu, są wykluczone.</span><span class="sxs-lookup"><span data-stu-id="f629e-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="f629e-273">Wartość domyślna to "Ignore.".</span><span class="sxs-lookup"><span data-stu-id="f629e-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="f629e-274">`keyDelimiter` — wartość domyślna to `null`.</span><span class="sxs-lookup"><span data-stu-id="f629e-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="f629e-275">Jeśli ta wartość jest określona, Konstruktor konfiguracji przechodzi przez wiele poziomów katalogu, tworząc nazwy kluczy przy użyciu tego ogranicznika.</span><span class="sxs-lookup"><span data-stu-id="f629e-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="f629e-276">Jeśli ta wartość jest `null`, Konstruktor konfiguracji będzie szukał tylko najwyższego poziomu katalogu.</span><span class="sxs-lookup"><span data-stu-id="f629e-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="f629e-277">`optional` — wartość domyślna to `false`.</span><span class="sxs-lookup"><span data-stu-id="f629e-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="f629e-278">Określa, czy Konstruktor konfiguracji powinien spowodować błędy, jeśli katalog źródłowy nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="f629e-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="f629e-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="f629e-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="f629e-280">Nigdy nie przechowuj haseł, poufnych parametrów połączenia lub innych poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="f629e-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="f629e-281">Wpisy tajne produkcji nie powinny być używane do celów deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="f629e-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="f629e-282">Projekty .NET Core często używają plików JSON do konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f629e-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="f629e-283">Konstruktor [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) umożliwia korzystanie z plików języka JSON platformy .NET Core w .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f629e-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="f629e-284">Ten konstruktor konfiguracji zapewnia podstawowe mapowanie ze źródła klucza prostego/wartości do określonych obszarów klucz/wartość konfiguracji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f629e-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="f629e-285">Ten program Configuration Builder **nie zapewnia konfiguracji** hierarchicznych.</span><span class="sxs-lookup"><span data-stu-id="f629e-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="f629e-286">Plik kopii zapasowej JSON jest podobny do słownika, a nie złożonego obiektu hierarchicznego.</span><span class="sxs-lookup"><span data-stu-id="f629e-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="f629e-287">Można użyć wielopoziomowego pliku hierarchicznego.</span><span class="sxs-lookup"><span data-stu-id="f629e-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="f629e-288">Ten dostawca `flatten`s głębokości poprzez dołączenie nazwy właściwości na każdym poziomie przy użyciu `:` jako ogranicznika.</span><span class="sxs-lookup"><span data-stu-id="f629e-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="f629e-289">Szczegóły atrybutu:</span><span class="sxs-lookup"><span data-stu-id="f629e-289">Attribute details:</span></span>

* <span data-ttu-id="f629e-290">`jsonFile` — wymagane.</span><span class="sxs-lookup"><span data-stu-id="f629e-290">`jsonFile` - Required.</span></span> <span data-ttu-id="f629e-291">Określa plik JSON, z którego ma zostać odczytany.</span><span class="sxs-lookup"><span data-stu-id="f629e-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="f629e-292">Znaku `~` można użyć na początku, aby odwołać się do katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f629e-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="f629e-293">`optional`-wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="f629e-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="f629e-294">Zapobiega zgłaszaniu wyjątków, jeśli nie można znaleźć pliku JSON.</span><span class="sxs-lookup"><span data-stu-id="f629e-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="f629e-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="f629e-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="f629e-296">`Flat` jest wartością domyślną.</span><span class="sxs-lookup"><span data-stu-id="f629e-296">`Flat` is the default.</span></span> <span data-ttu-id="f629e-297">Gdy `jsonMode` jest `Flat`, plik JSON jest jednym prostym źródłem klucza/wartości.</span><span class="sxs-lookup"><span data-stu-id="f629e-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="f629e-298">`EnvironmentConfigBuilder` i `AzureKeyVaultConfigBuilder` są również pojedynczymi płaskimi źródłami klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="f629e-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="f629e-299">Gdy `SimpleJsonConfigBuilder` jest skonfigurowany w trybie `Sectional`:</span><span class="sxs-lookup"><span data-stu-id="f629e-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="f629e-300">Plik JSON jest koncepcyjnie podzielony na najwyższego poziomu do wielu słowników.</span><span class="sxs-lookup"><span data-stu-id="f629e-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="f629e-301">Każdy słownik jest stosowany tylko do sekcji konfiguracji, która pasuje do dołączonej nazwy właściwości najwyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="f629e-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="f629e-302">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f629e-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="f629e-303">Implementowanie niestandardowego konstruktora konfiguracji klucza/wartości</span><span class="sxs-lookup"><span data-stu-id="f629e-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="f629e-304">Jeśli konstruktorzy konfiguracji nie spełnią Twoich potrzeb, można napisać ją niestandardową.</span><span class="sxs-lookup"><span data-stu-id="f629e-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="f629e-305">Klasa bazowa `KeyValueConfigBuilder` obsługuje tryby podstawienia i większość kwestii związanych z prefiksami.</span><span class="sxs-lookup"><span data-stu-id="f629e-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="f629e-306">Wdrożenie projektu wymaga tylko:</span><span class="sxs-lookup"><span data-stu-id="f629e-306">An implementing project need only:</span></span>

* <span data-ttu-id="f629e-307">Dziedzicz z klasy podstawowej i zaimplementuj podstawowe źródło par klucz/wartość za pośrednictwem `GetValue` i `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="f629e-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="f629e-308">Dodaj do projektu [Microsoft. Configuration. ConfigurationBuilders. Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) .</span><span class="sxs-lookup"><span data-stu-id="f629e-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="f629e-309">Klasa bazowa `KeyValueConfigBuilder` zapewnia większość pracy i spójne zachowanie między konstruktorami konfiguracji klucz/wartość.</span><span class="sxs-lookup"><span data-stu-id="f629e-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f629e-310">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f629e-310">Additional resources</span></span>

* [<span data-ttu-id="f629e-311">Repozytorium GitHub dla konstruktorów konfiguracji</span><span class="sxs-lookup"><span data-stu-id="f629e-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="f629e-312">Uwierzytelnianie między usługami Azure Key Vault przy użyciu platformy .NET</span><span class="sxs-lookup"><span data-stu-id="f629e-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)

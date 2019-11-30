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
# <a name="configuration-builders-for-aspnet"></a>Konstruktory konfiguracji dla ASP.NET

Autorzy [Stephen Molloy](https://github.com/StephenMolloy) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Konstruktory konfiguracji zapewniają nowoczesne i Agile mechanizm dla aplikacji ASP.NET, aby uzyskać wartości konfiguracyjne ze źródeł zewnętrznych.

Konstruktory konfiguracji:

* Są dostępne w .NET Framework 4.7.1 i nowszych.
* Zapewnianie elastycznego mechanizmu odczytywania wartości konfiguracyjnych.
* Należy zająć się niektórymi podstawowymi potrzebami aplikacji, które są przenoszone do kontenera i środowiska ukierunkowanego na chmurę.
* Może służyć do ulepszania ochrony danych konfiguracji przez rysowanie ze źródeł, które były wcześniej niedostępne (na przykład Azure Key Vault i zmienne środowiskowe) w systemie konfiguracji .NET.

## <a name="keyvalue-configuration-builders"></a>Konstruktory konfiguracji klucza/wartości

Typowym scenariuszem, który może być obsługiwany przez konstruktory konfiguracji, jest zapewnienie podstawowego mechanizmu wymiany klucza/wartości dla sekcji konfiguracyjnych, które są zgodne ze wzorcem klucz/wartość. Koncepcja .NET Framework ConfigurationBuilders nie jest ograniczona do określonych sekcji konfiguracyjnych ani wzorców. Jednak wiele konstruktorów konfiguracji w `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) pracuje w obrębie wzorca klucz/wartość.

## <a name="keyvalue-configuration-builders-settings"></a>Ustawienia konstruktorów konfiguracji klucza/wartości

Następujące ustawienia mają zastosowanie do wszystkich konstruktorów konfiguracji klucza/wartości w `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Tryb

Konstruktory konfiguracji używają zewnętrznego źródła informacji o klucz/wartość, aby wypełnić wybrane elementy klucza/wartości systemu konfiguracyjnego. Szczególnie `<appSettings/>` i `<connectionStrings/>` sekcje otrzymują specjalne traktowanie od konstruktorów konfiguracji. Konstruktorzy pracują w trzech trybach:

* `Strict` — tryb domyślny. W tym trybie Konstruktor konfiguracji działa tylko w przypadku dobrze znanych sekcji konfiguracji klucz/wartość. Tryb `Strict` wylicza każdy klucz w sekcji. Jeśli w źródle zewnętrznym zostanie znaleziony pasujący klucz:

   * Konstruktory konfiguracji zastępują wartość w sekcji konfiguracji w wyniku wartością z zewnętrznego źródła.
* `Greedy` — ten tryb jest ściśle powiązany z trybem `Strict`. Zamiast ograniczać się do kluczy, które już istnieją w oryginalnej konfiguracji:

  * Konstruktory konfiguracji dodaje wszystkie pary klucz/wartość ze źródła zewnętrznego do sekcji konfiguracji w wyniku.

* `Expand` — działa na nieprzetworzonym kodzie XML przed przeanalizowaniem go do obiektu sekcji konfiguracji. Można go traktować jako rozszerzenie tokenów w ciągu. Każda część nieprzetworzonego ciągu XML, która pasuje do wzorca `${token}` jest kandydatem do rozwinięcia tokenu. Jeśli w zewnętrznym źródle nie zostanie znaleziona odpowiednia wartość, token nie zostanie zmieniony. Konstruktory w tym trybie nie są ograniczone do sekcji `<appSettings/>` i `<connectionStrings/>`.

Poniższe znaczniki z *pliku Web. config* włączają [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) w trybie `Strict`:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` pokazane w poprzednim pliku *Web. config* :

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Poprzedni kod ustawi wartości właściwości na:

* Wartości w pliku *Web. config* , jeśli klucze nie są ustawione w zmiennych środowiskowych.
* Wartości zmiennej środowiskowej, jeśli jest ustawiona.

Na przykład `ServiceID` będzie zawierać:

* "ServiceID wartość z pliku Web. config", jeśli zmienna środowiskowa `ServiceID` nie jest ustawiona.
* Wartość zmiennej środowiskowej `ServiceID`, jeśli została ustawiona.

Na poniższej ilustracji przedstawiono klucze `<appSettings/>`/wartości z poprzedniego zestawu plików *Web. config* w edytorze środowiska:

![Edytor środowiska](config-builder/static/env.png)

Uwaga: aby zobaczyć zmiany w zmiennych środowiskowych, może być konieczne zamknięcie i ponowne uruchomienie programu Visual Studio.

### <a name="prefix-handling"></a>Obsługa prefiksów

Prefiksy kluczy mogą uprościć ustawienie kluczy, ponieważ:

* Konfiguracja .NET Framework jest złożona i zagnieżdżona.
* Zewnętrzne źródła kluczy/wartości są zwykle podstawowe i płaskie według natury. Na przykład zmienne środowiskowe nie są zagnieżdżone.

Użyj dowolnych z poniższych metod, aby wstrzyknąć zarówno `<appSettings/>`, jak i `<connectionStrings/>` do konfiguracji za pomocą zmiennych środowiskowych:

* Z `EnvironmentConfigBuilder` w domyślnym trybie `Strict` i odpowiednimi nazwami kluczy w pliku konfiguracji. Powyższy kod i znacznik przyjmuje takie podejście. Korzystając z tej metody, **nie** można mieć identycznie nazwanych kluczy w obu `<appSettings/>` i `<connectionStrings/>`.
* Użyj dwóch `EnvironmentConfigBuilder`s w trybie `Greedy` z unikatowymi prefiksami i `stripPrefix`. Dzięki temu aplikacja może odczytywać `<appSettings/>` i `<connectionStrings/>` bez konieczności aktualizowania pliku konfiguracji. W następnej sekcji [stripPrefix](#stripprefix), pokazano, jak to zrobić.
* Użyj dwóch `EnvironmentConfigBuilder`s w trybie `Greedy` z unikatowymi prefiksami. W tym podejściu nie można mieć zduplikowanych nazw kluczy, ponieważ nazwy kluczy muszą się różnić prefiksami.  Na przykład:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Przy poprzedzającym znaczniku można użyć tego samego prostego źródła klucza/wartości, aby wypełnić konfigurację dla dwóch różnych sekcji.

Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` klucze/wartości z poprzedniego zestawu plików *Web. config* w edytorze środowiska:

![Edytor środowiska](config-builder/static/prefix.png)

Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` klucze/wartości zawarte w poprzednim pliku *Web. config* :

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Poprzedni kod ustawi wartości właściwości na:

* Wartości w pliku *Web. config* , jeśli klucze nie są ustawione w zmiennych środowiskowych.
* Wartości zmiennej środowiskowej, jeśli jest ustawiona.

Na przykład przy użyciu poprzedniego pliku *Web. config* klucze/wartości w poprzednim obrazie edytora środowiska i Poprzedni kod są ustawiane następujące wartości:

|  Key              | Wartość |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID ze zmiennych ENV|
|    AppSetting_default            | AppSetting_default wartość z ENV |
|       ConnStr_default         | ConnStr_default Val z ENV|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: wartość logiczna, wartość domyślna to `false`. 

Poprzedzające znaczniki XML oddzielają ustawienia aplikacji od parametrów połączenia, ale wymagają wszystkich kluczy w pliku *Web. config* , aby użyć określonego prefiksu. Na przykład prefiks `AppSetting` musi być dodany do klucza `ServiceID` ("AppSetting_ServiceID"). Przy `stripPrefix`, prefiks nie jest używany w pliku *Web. config* . Prefiks jest wymagany w źródle Configuration Builder (na przykład w środowisku). Oczekujemy, że większość deweloperów będzie używać `stripPrefix`.

Aplikacje zwykle przełączają się na prefiks. Poniższy prefiks *Web. config* paski:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

W poprzednim pliku *Web. config* klucz `default` znajduje się zarówno w `<appSettings/>`, jak i `<connectionStrings/>`.

Na poniższej ilustracji przedstawiono `<appSettings/>` i `<connectionStrings/>` klucze/wartości z poprzedniego zestawu plików *Web. config* w edytorze środowiska:

![Edytor środowiska](config-builder/static/prefix.png)

Poniższy kod odczytuje `<appSettings/>` i `<connectionStrings/>` klucze/wartości zawarte w poprzednim pliku *Web. config* :

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Poprzedni kod ustawi wartości właściwości na:

* Wartości w pliku *Web. config* , jeśli klucze nie są ustawione w zmiennych środowiskowych.
* Wartości zmiennej środowiskowej, jeśli jest ustawiona.

Na przykład przy użyciu poprzedniego pliku *Web. config* klucze/wartości w poprzednim obrazie edytora środowiska i Poprzedni kod są ustawiane następujące wartości:

|  Key              | Wartość |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID ze zmiennych ENV|
|    {1&gt;default&lt;1}            | AppSetting_default wartość z ENV |
|    {1&gt;default&lt;1}         | ConnStr_default Val z ENV|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: ciąg, wartość domyślna to `@"\$\{(\w+)\}"`

Zachowanie `Expand` dla konstruktorów przeszukuje nieprzetworzony kod XML pod kątem tokenów, które wyglądają jak `${token}`. Wyszukiwanie jest wykonywane z domyślnym wyrażeniem regularnym `@"\$\{(\w+)\}"`. Zestaw znaków, które pasują do `\w`, jest bardziej rygorystyczny niż XML i wiele źródeł konfiguracji Zezwalaj. Użyj `tokenPattern`, gdy nazwa tokenu wymaga więcej znaków niż `@"\$\{(\w+)\}"`.

`tokenPattern`: ciąg:

* Umożliwia deweloperom zmianę wyrażenia regularnego używanego do dopasowywania tokenu.
* Sprawdzanie poprawności nie jest wykonywane, aby upewnić się, że jest to dobrze sformułowane wyrażenie regularne.
* Musi zawierać grupę przechwytywania. Całe wyrażenie regularne musi być zgodne z całym tokenem. Pierwsze przechwytywanie musi być nazwą tokenu, aby wyszukać w źródle konfiguracji.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Konstruktory konfiguracji w programie Microsoft. Configuration. ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Jest najprostszym z konstruktorów konfiguracji.
* Odczytuje wartości ze środowiska.
* Nie ma żadnych dodatkowych opcji konfiguracji.
* Wartość atrybutu `name` jest dowolną.

**Uwaga:** W środowisku kontenera systemu Windows zmienne ustawione w czasie wykonywania są wstrzykiwane tylko do środowiska procesu EntryPoint. Aplikacje uruchamiane jako usługa lub proces poza elementem EntryPoint nie pobierają tych zmiennych, chyba że są one wprowadzane przez mechanizm w kontenerze. W przypadku [usług IIS](https://github.com/Microsoft/iis-docker/pull/41)/kontenerów opartych na [ASP.NET](https://github.com/Microsoft/aspnet-docker), bieżąca wersja elementu [ServiceMonitor. exe](https://github.com/Microsoft/iis-docker/pull/41) obsługuje tę *opcję tylko w ramach tej* usługi. Inne warianty kontenerów opartych na systemie Windows mogą wymagać opracowania własnego mechanizmu wstrzykiwania dla procesów poza punktami wejścia.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nigdy nie przechowuj haseł, poufnych parametrów połączenia lub innych poufnych danych w kodzie źródłowym. Wpisy tajne produkcji nie powinny być używane do celów deweloperskich i testowych.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Ten konstruktor konfiguracji udostępnia funkcję podobną do [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) można używać w projektach .NET Framework, ale musi być określony plik tajny. Alternatywnie można zdefiniować Właściwość `UserSecretsId` w pliku projektu i utworzyć Nieprzetworzony plik tajny we właściwym miejscu do odczytu. Aby zachować zależności zewnętrzne poza projektem, plik tajny jest sformatowany w formacie XML. Formatowanie XML jest szczegółami implementacji i nie powinno się opierać na tym formacie. Jeśli musisz udostępnić plik *tajny. JSON* z projektami .NET Core, rozważ użycie [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). Format `SimpleJsonConfigBuilder` dla platformy .NET Core powinien również być traktowany jak szczegóły implementacji, aby zmienić.

Atrybuty konfiguracji dla `UserSecretsConfigBuilder`:

* `userSecretsId` — jest to preferowana metoda identyfikacji pliku tajnego XML. Działa podobnie do programu .NET Core, który używa właściwości projektu `UserSecretsId` do przechowywania tego identyfikatora. Ciąg musi być unikatowy, nie musi być identyfikatorem GUID. W tym atrybucie `UserSecretsConfigBuilder` sprawdza dobrze znaną lokalizację lokalną (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) dla pliku tajnego należącego do tego identyfikatora.
* `userSecretsFile` — opcjonalny atrybut określający plik zawierający wpisy tajne. Znaku `~` można użyć na początku, aby odwołać się do katalogu głównego aplikacji. Wymagany jest ten atrybut lub atrybut `userSecretsId`. Jeśli są określone oba te elementy, `userSecretsFile` ma pierwszeństwo.
* `optional`: Boolean, wartość domyślna `true` — uniemożliwia wyjątek, jeśli nie można znaleźć pliku tajnego. 
* Wartość atrybutu `name` jest dowolną.

Plik tajny ma następujący format:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

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

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) odczytuje wartości przechowywane w [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` jest wymagana (nazwa magazynu lub identyfikator URI magazynu). Inne atrybuty umożliwiają kontrolę nad tym, z którym magazynem ma zostać nawiązane połączenie, ale tylko wtedy, gdy aplikacja nie działa w środowisku, które współpracuje z `Microsoft.Azure.Services.AppAuthentication`. Biblioteka uwierzytelniania usług platformy Azure służy do automatycznego pobierania informacji o połączeniu ze środowiska wykonywania, jeśli jest to możliwe. Można przesłonić automatyczne pobranie informacji o połączeniu, podając parametry połączenia.

* `vaultName` — wymagane, jeśli nie określono `uri`. Określa nazwę magazynu w ramach subskrypcji platformy Azure, z którego ma zostać odczytana para klucz/wartość.
* `connectionString` — parametry połączenia używane przez [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` — nawiązuje połączenie z innymi dostawcami Key Vault o określonej wartości `uri`. Jeśli nie zostanie określony, platforma Azure (`vaultName`) jest dostawcą magazynu.
* `version`-Azure Key Vault udostępnia funkcję przechowywania wersji dla wpisów tajnych. Jeśli określono `version`, Konstruktor pobiera tylko wpisy tajne pasujące do tej wersji.
* `preloadSecretNames` — domyślnie ten Konstruktor bada **wszystkie** nazwy kluczy w magazynie kluczy, gdy jest zainicjowany. Aby uniemożliwić odczytywanie wszystkich wartości klucza, ustaw ten atrybut na `false`. Ustawienie tej opcji na `false` odczytuje wpisy tajne po jednej naraz. Odczytywanie wpisów tajnych po jednej naraz może być przydatne, jeśli magazyn zezwala na dostęp "Get", ale nie do "list". **Uwaga:** W przypadku używania `Greedy` trybu `preloadSecretNames` musi być `true` (wartość domyślna).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

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

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) to podstawowy Konstruktor konfiguracji, który używa plików katalogu jako źródła wartości. Nazwa pliku jest kluczem, a zawartość jest wartością. Ten konstruktor konfiguracji może być przydatny w przypadku uruchamiania w środowisku organizowania kontenerów. Systemy takie jak Docker Swarm i Kubernetes zapewniają `secrets` w zorganizowanych kontenerach systemu Windows w tym samym pliku.

Szczegóły atrybutu:

* `directoryPath` — wymagane. Określa ścieżkę do przeszukiwania wartości. Wpisy tajne Docker for Windows są domyślnie przechowywane w katalogu *C:\ProgramData\Docker\secrets* .
* `ignorePrefix` — pliki, które zaczynają się od tego prefiksu, są wykluczone. Wartość domyślna to "Ignore.".
* `keyDelimiter` — wartość domyślna to `null`. Jeśli ta wartość jest określona, Konstruktor konfiguracji przechodzi przez wiele poziomów katalogu, tworząc nazwy kluczy przy użyciu tego ogranicznika. Jeśli ta wartość jest `null`, Konstruktor konfiguracji będzie szukał tylko najwyższego poziomu katalogu.
* `optional` — wartość domyślna to `false`. Określa, czy Konstruktor konfiguracji powinien spowodować błędy, jeśli katalog źródłowy nie istnieje.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nigdy nie przechowuj haseł, poufnych parametrów połączenia lub innych poufnych danych w kodzie źródłowym. Wpisy tajne produkcji nie powinny być używane do celów deweloperskich i testowych.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Projekty .NET Core często używają plików JSON do konfiguracji. Konstruktor [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) umożliwia korzystanie z plików języka JSON platformy .NET Core w .NET Framework. Ten konstruktor konfiguracji zapewnia podstawowe mapowanie ze źródła klucza prostego/wartości do określonych obszarów klucz/wartość konfiguracji .NET Framework. Ten program Configuration Builder **nie zapewnia konfiguracji** hierarchicznych. Plik kopii zapasowej JSON jest podobny do słownika, a nie złożonego obiektu hierarchicznego. Można użyć wielopoziomowego pliku hierarchicznego. Ten dostawca `flatten`s głębokości poprzez dołączenie nazwy właściwości na każdym poziomie przy użyciu `:` jako ogranicznika.

Szczegóły atrybutu:

* `jsonFile` — wymagane. Określa plik JSON, z którego ma zostać odczytany. Znaku `~` można użyć na początku, aby odwołać się do katalogu głównego aplikacji.
* `optional`-wartość domyślna to `true`. Zapobiega zgłaszaniu wyjątków, jeśli nie można znaleźć pliku JSON.
* `jsonMode` - `[Flat|Sectional]`. `Flat` jest wartością domyślną. Gdy `jsonMode` jest `Flat`, plik JSON jest jednym prostym źródłem klucza/wartości. `EnvironmentConfigBuilder` i `AzureKeyVaultConfigBuilder` są również pojedynczymi płaskimi źródłami klucz/wartość. Gdy `SimpleJsonConfigBuilder` jest skonfigurowany w trybie `Sectional`:

  * Plik JSON jest koncepcyjnie podzielony na najwyższego poziomu do wielu słowników.
  * Każdy słownik jest stosowany tylko do sekcji konfiguracji, która pasuje do dołączonej nazwy właściwości najwyższego poziomu. Na przykład:

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

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementowanie niestandardowego konstruktora konfiguracji klucza/wartości

Jeśli konstruktorzy konfiguracji nie spełnią Twoich potrzeb, można napisać ją niestandardową. Klasa bazowa `KeyValueConfigBuilder` obsługuje tryby podstawienia i większość kwestii związanych z prefiksami. Wdrożenie projektu wymaga tylko:

* Dziedzicz z klasy podstawowej i zaimplementuj podstawowe źródło par klucz/wartość za pośrednictwem `GetValue` i `GetAllValues`:
* Dodaj do projektu [Microsoft. Configuration. ConfigurationBuilders. Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) .

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

Klasa bazowa `KeyValueConfigBuilder` zapewnia większość pracy i spójne zachowanie między konstruktorami konfiguracji klucz/wartość.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Repozytorium GitHub dla konstruktorów konfiguracji](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Uwierzytelnianie między usługami Azure Key Vault przy użyciu platformy .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)

---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Wdrażanie haseł i innych poufnych danych w ASP.NET i Azure App Service-ASP.NET 4. x
author: Rick-Anderson
description: Ten samouczek pokazuje, w jaki sposób kod może bezpiecznie przechowywać i uzyskiwać dostęp do zabezpieczonych informacji. Najważniejszym punktem jest nigdy nie należy przechowywać haseł ani innych dawcy...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8356a90611f791779cc4ff4730038d82cd76242f
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457053"
---
# <a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i w usłudze Azure App Service

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek pokazuje, w jaki sposób kod może bezpiecznie przechowywać i uzyskiwać dostęp do zabezpieczonych informacji. Najważniejszym punktem jest nigdy nie należy przechowywać haseł ani innych poufnych danych w kodzie źródłowym i nie należy korzystać z tajemnicy produkcyjnych w trybie tworzenia i testowania.
> 
> Przykładowy kod jest prostą aplikacją konsolową WebJob i aplikacją ASP.NET MVC, która wymaga dostępu do parametrów połączenia z bazą danych, Twilio, Google i SendGrid Secure Keys.
> 
> W ustawieniach lokalnych i PHP jest również wymieniony.

- [Praca z hasłami w środowisku programistycznym](#pwd)
- [Praca z parametrami połączenia w środowisku deweloperskim](#con)
- [Aplikacje konsolowe WebJobs](#wj)
- [Wdrażanie wpisów tajnych na platformie Azure](#da)
- [Uwagi dotyczące lokalnego i PHP](#not)
- [Dodatkowe zasoby](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Praca z hasłami w środowisku programistycznym

W samouczkach często są wyświetlane poufne dane w kodzie źródłowym, miejmy nadzieję z zastrzeżeniem, że nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Na przykład moja [aplikacja ASP.NET MVC 5 z programem SMS i z samouczkiem poczty E-mail funkcji 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) ilustruje następujące elementy w pliku *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Plik *Web. config* jest kodem źródłowym, dlatego te wpisy tajne nigdy nie powinny być przechowywane w tym pliku. Na szczęście element `<appSettings>` ma atrybut `file`, który umożliwia określenie pliku zewnętrznego zawierającego poufne ustawienia konfiguracji aplikacji. Wszystkie wpisy tajne można przenieść do zewnętrznego pliku, o ile plik zewnętrzny nie zostanie zaewidencjonowany w drzewie źródłowym. Na przykład w poniższym znaczniku plik *AppSettingsSecrets. config* zawiera wszystkie wpisy tajne aplikacji:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Znaczniki w pliku zewnętrznym (*AppSettingsSecrets. config* w tym przykładzie) są takie same, jak w pliku *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Środowisko uruchomieniowe ASP.NET Scala zawartość pliku zewnętrznego ze znacznikiem w &lt;appSettings&gt; elementu. Środowisko uruchomieniowe ignoruje atrybut pliku, jeśli nie można odnaleźć określonego pliku.

> [!WARNING]
> Zabezpieczenia — nie dodawaj pliku Secret *. config* do projektu ani nie należy go sprawdzać w kontroli źródła. Domyślnie program Visual Studio ustawia `Build Action` na `Content`, co oznacza, że plik jest wdrażany. Aby uzyskać więcej informacji [, zobacz Dlaczego nie wszystkie pliki w folderze Mój projekt zostały wdrożone?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Mimo że można użyć dowolnego rozszerzenia dla pliku Secret *. config* , najlepiej jest zachować plik *. config*, ponieważ pliki konfiguracji nie są obsługiwane przez usługi IIS. Zwróć również uwagę, że plik *AppSettingsSecrets. config* ma dwa poziomy katalogów w górę z pliku *Web. config* , więc jest całkowicie poza katalogiem rozwiązania. Przenosząc plik z katalogu rozwiązania, &quot;narzędzia Git Add \*&quot; nie dodamy go do repozytorium.

<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Praca z parametrami połączenia w środowisku deweloperskim

Program Visual Studio tworzy nowe projekty ASP.NET korzystające z [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB został utworzony w środowisku programistycznym. Nie wymaga hasła, dlatego nie trzeba wykonywać żadnych czynności, aby uniemożliwić sprawdzenie wpisów tajnych w kodzie źródłowym. Niektóre zespoły programistyczne używają pełnych wersji SQL Server (lub innych systemów DBMS), które wymagają hasła.

Możesz użyć atrybutu `configSource`, aby zamienić cały `<connectionStrings>` znacznik. W przeciwieństwie do atrybutu `file` `<appSettings>`, który scala znacznik, atrybut `configSource` zastępuje znacznik. Poniższy znacznik przedstawia atrybut `configSource` w pliku *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Jeśli używasz atrybutu `configSource`, jak pokazano powyżej, aby przenieść parametry połączenia do pliku zewnętrznego, a program Visual Studio utworzy nową witrynę sieci Web, nie będzie możliwe wykrycie korzystania z bazy danych i nie będzie można skonfigurować bazy danych podczas publikowania na platformie Azure z poziomu programu Visual Studio. Jeśli używasz atrybutu `configSource`, możesz użyć programu PowerShell do utworzenia i wdrożenia witryny sieci Web oraz bazy danych, a także utworzyć witrynę sieci Web i bazę danych w portalu przed opublikowaniem. Skrypt [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) utworzy nową witrynę sieci Web i bazę danych.

> [!WARNING]
> Zabezpieczenia — w przeciwieństwie do pliku *AppSettingsSecrets. config* , zewnętrzny plik parametrów połączenia musi znajdować się w tym samym katalogu, co główny plik *Web. config* , dlatego należy podjąć odpowiednie środki ostrożności, aby upewnić się, że nie jest on sprawdzany w repozytorium źródłowym.

> [!NOTE]
> **Ostrzeżenie o zabezpieczeniach w pliku tajnym:** Najlepszym rozwiązaniem jest nieużywanie produkcyjnych wpisów tajnych w testowaniu i programowaniu. Korzystanie z haseł produkcyjnych w środowisku testowym i programistycznym wycieka te klucze tajne.

<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplikacje konsolowe WebJobs

Plik *App. config* używany przez aplikację konsoli nie obsługuje ścieżek względnych, ale obsługuje ścieżki bezwzględne. Możesz użyć ścieżki bezwzględnej, aby przenieść wpisy tajne z katalogu projektu. Poniższe znaczniki przedstawiają wpisy tajne w pliku *C:\secrets\AppSettingsSecrets.config* i niepoufne dane w pliku *App. config* .

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Wdrażanie wpisów tajnych na platformie Azure

Po wdrożeniu aplikacji sieci Web na platformie Azure plik *AppSettingsSecrets. config* nie zostanie wdrożony (to czego potrzebujesz). Możesz przejść do [Portal zarządzania platformy Azure](https://azure.microsoft.com/services/management-portal/) i ustawić je ręcznie, aby wykonać następujące czynności:

1. Przejdź do [https://portal.azure.com](https://portal.azure.com)i zaloguj się przy użyciu poświadczeń platformy Azure.
2. Kliknij przycisk **przeglądaj &gt; Web Apps**, a następnie kliknij nazwę aplikacji sieci Web.
3. Kliknij pozycję **wszystkie ustawienia &gt; ustawienia aplikacji**.

**Ustawienia aplikacji** i wartości **parametrów połączenia** zastępują te same ustawienia w pliku *Web. config* . W naszym przykładzie te ustawienia nie zostały wdrożone na platformie Azure, ale jeśli te klucze znajdowały się w pliku *Web. config* , pierwszeństwo mają ustawienia wyświetlane w portalu.

Najlepszym rozwiązaniem jest wykonanie [przepływu pracy DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) i użycie [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (lub innej struktury, takiej jak [Chef](http://www.opscode.com/chef/) lub [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) w celu zautomatyzowania ustawiania tych wartości na platformie Azure. Poniższy skrypt programu PowerShell używa polecenia [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) do eksportowania zaszyfrowanych kluczy tajnych do dysku:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

W powyższym skrypcie "name" jest nazwą klucza tajnego, na przykład "&quot;FB\_AppSecret&quot; lub" TwitterSecret ". Plik ". Credential" utworzony przez skrypt można wyświetlić w przeglądarce. Poniższy fragment kodu sprawdza każdy plik poświadczeń i ustawia wpisy tajne dla nazwanej aplikacji sieci Web:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Zabezpieczenia — nie Uwzględniaj haseł ani innych wpisów tajnych w skrypcie programu PowerShell, dzięki czemu może to spowodować, że Użyj skryptu programu PowerShell do wdrożenia poufnych danych. Polecenie cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) zapewnia bezpieczny mechanizm uzyskiwania hasła. Użycie monitu interfejsu użytkownika może uniemożliwić wyciek hasła.

### <a name="deploying-db-connection-strings"></a>Wdrażanie parametrów połączenia bazy danych

Parametry połączenia bazy danych są obsługiwane w sposób analogiczny do ustawień aplikacji. Jeśli aplikacja sieci Web zostanie wdrożona z poziomu programu Visual Studio, parametry połączenia zostaną skonfigurowane. Możesz to sprawdzić w portalu. Zalecanym sposobem ustawienia parametrów połączenia jest program PowerShell. Przykładowy skrypt programu PowerShell tworzy witrynę sieci Web i bazę danych oraz ustawia parametry połączenia w witrynie sieci Web, pobierając [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [biblioteki skryptów platformy Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Uwagi dotyczące języka PHP

Ponieważ pary klucz-wartość dla **ustawień aplikacji** i **parametrów połączenia** są przechowywane w zmiennych środowiskowych w Azure App Service, deweloperzy korzystający z dowolnych struktur aplikacji sieci Web (takich jak php) mogą łatwo pobrać te wartości. Zapoznaj się z [witrynami sieci Web systemu Windows Azure w systemie Stefan Schackow: jak są umieszczane w blogu ciągi aplikacji i parametry połączenia](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) , które zawierają fragment kodu PHP umożliwiający odczytywanie ustawień aplikacji i parametrów połączenia.

## <a name="notes-for-on-premises-servers"></a>Uwagi dotyczące serwerów lokalnych

W przypadku wdrażania na lokalnych serwerach sieci Web można zabezpieczyć wpisy tajne, [szyfrując sekcje konfiguracji plików konfiguracji](https://msdn.microsoft.com/library/ff647398.aspx). Alternatywnie można użyć tego samego podejścia zalecanego dla usługi Azure Websites: Zachowaj ustawienia deweloperskie w plikach konfiguracji i użyj wartości zmiennych środowiskowych dla ustawień produkcyjnych. W takim przypadku należy jednak napisać kod aplikacji dla funkcji, które są automatyczne w usłudze Azure Websites: Pobierz ustawienia ze zmiennych środowiskowych i Użyj tych wartości zamiast ustawień pliku konfiguracji lub Użyj ustawień pliku konfiguracji, gdy nie znaleziono zmiennych środowiskowych.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

Przykładowy skrypt programu PowerShell służący do tworzenia aplikacji sieci Web i bazy danych, ustawia parametry połączenia + ustawienia aplikacji, pobiera [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [biblioteki skryptów platformy Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Zobacz [witryny sieci Web systemu Windows Azure w systemie Stefan Schackow: sposób działania ciągów aplikacji i parametrów połączenia](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)

Specjalne podziękowania dla Marcin Dorrans ( [@blowdart](https://twitter.com/blowdart) ) i Carlos Farre do recenzowania.

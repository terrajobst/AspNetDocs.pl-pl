---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatyzacja wszystkiego (Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: e741a753a36ebdaefbff8eee0b38911785c716ac
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457170"
---
# <a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatyzacja wszystkiego (Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby zapoznać się z wprowadzeniem do książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Pierwsze trzy wzorce, które będą wyglądały w rzeczywistości, mają zastosowanie do każdego projektu programistycznego oprogramowania, ale szczególnie dla projektów w chmurze. Ten wzorzec dotyczy automatyzowania zadań programistycznych. Jest to ważny temat, ponieważ procesy ręczne są powolne i podatne na błędy; Automatyzacja możliwie największej liczby możliwości umożliwia skonfigurowanie szybkiego, niezawodnego i elastycznego przepływu pracy. Jest to bardzo ważne w przypadku programowania w chmurze, ponieważ można łatwo zautomatyzować wiele zadań, które są trudne lub niemożliwe do automatyzacji w środowisku lokalnym. Można na przykład skonfigurować całe środowiska testowe, w tym nowy serwer sieci Web i maszyny wirtualne zaplecza, bazy danych, Magazyn obiektów BLOB (magazyn plików), kolejki itd.

## <a name="devops-workflow"></a>Przepływ pracy DevOps

Coraz więcej wyrazów "DevOps". Termin powstał z rozpoznawania, który należy zintegrować z zadaniami deweloperskimi i operacyjnymi w celu wydajnego opracowywania oprogramowania. Rodzaj przepływu pracy, który chcesz włączyć, to taki, w którym można opracowywać aplikację, wdrażać ją, dowiedzieć się, jak korzystać z produkcji, zmienić w odpowiedzi na to, co już znasz, i szybko i niezawodnie powtarzać cykl.

Niektóre pomyślne zespoły deweloperów w chmurze wdrażają wiele razy dziennie w środowisku działającym na żywo. Zespół platformy Azure używany do wdrożenia ważnej aktualizacji co 2-3 miesięcy, ale teraz zwalnia drobne aktualizacje co 2-3 dni i główne wydania co 2-3 tygodnie. Dowiesz się, że erze w rzeczywistości pozwala na reagowanie na Opinie klientów.

W tym celu należy włączyć cykl programowania i wdrażania, który jest powtarzalny, niezawodny, przewidywalny i ma czas niskiego cyklu.

![Przepływ pracy DevOps](automate-everything/_static/image1.png)

Innymi słowy, okres czasu między, gdy masz pomysł na daną funkcję i kiedy klient korzysta z niej, i przekazywanie opinii musi być tak krótki, jak to możliwe. Pierwsze trzy wzorce — automatyzuje wszystko, kontrolę źródła i ciągłą integrację i dostarczanie — wszystkie najlepsze rozwiązania, które zalecamy w celu umożliwienia tego rodzaju procesu.

## <a name="azure-management-scripts"></a>Skrypty zarządzania platformy Azure

W ramach [wprowadzenia do tej książki elektronicznej](introduction.md)wykorzystano konsolę sieci web, Portal zarządzania platformy Azure. Portal zarządzania umożliwia monitorowanie wszystkich zasobów wdrożonych na platformie Azure i zarządzanie nimi. Jest to prosty sposób na tworzenie i usuwanie usług, takich jak aplikacje sieci Web i maszyny wirtualne, Konfigurowanie tych usług, monitorowanie operacji usługi i tak dalej. To doskonałe narzędzie, ale korzystanie z niego jest procesem ręcznym. Jeśli zamierzasz opracowywać produkcyjną aplikację o dowolnym rozmiarze i szczególnie w środowisku zespołu, zalecamy przechodzenie przez interfejs użytkownika portalu, aby poznać i poznać platformę Azure, a następnie zautomatyzować procesy, które będziesz powtarzać.

Niemal wszystko, co można zrobić ręcznie w portalu zarządzania lub programie Visual Studio, można również wykonać, wywołując interfejs API zarządzania REST. Skrypty można pisać przy użyciu [programu Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)lub można użyć platformy typu open source, takiej jak [Chef](http://www.opscode.com/chef/) lub [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Można również użyć narzędzia wiersza polecenia bash w środowisku Mac lub Linux. Platforma Azure ma interfejsy API obsługi skryptów dla wszystkich różnych środowisk i ma [interfejs API zarządzania platformy .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) , w przypadku którego chcesz napisać kod zamiast skryptu.

Dla aplikacji Fix it utworzyliśmy niektóre skrypty środowiska Windows PowerShell, które automatyzują procesy tworzenia środowiska testowego i wdrażania projektu w tym środowisku, a my zapoznajemy część zawartości tych skryptów.

## <a name="environment-creation-script"></a>Skrypt tworzenia środowiska

Pierwszy skrypt, którego szukamy, to o nazwie *New-AzureWebsiteEnv. ps1*. Tworzy środowisko platformy Azure, w którym można wdrożyć aplikację Fix it na potrzeby testowania. Główne zadania wykonywane przez ten skrypt są następujące:

- Utwórz aplikację internetową.
- Tworzenie konta magazynu (Wymagane w przypadku obiektów blob i kolejek, jak widać w dalszej części rozdziału).
- Utwórz serwer SQL Database i dwie bazy danych: bazę danych aplikacji i bazę danych członkostwa.
- Ustawienia magazynu na platformie Azure, które będą używane przez aplikację w celu uzyskiwania dostępu do konta magazynu i baz danych.
- Utwórz pliki ustawień, które będą używane do automatyzowania wdrażania.

### <a name="run-the-script"></a>Uruchamianie skryptu

> [!NOTE]
> W tej części rozdziału przedstawiono przykłady skryptów i poleceń wprowadzanych w celu ich uruchomienia. Ta wersja demonstracyjna i nie zapewnia wszystkiego, czego potrzebujesz do uruchamiania skryptów. Aby uzyskać instrukcje krok po kroku, zobacz [dodatek: Poprawka Przykładowa aplikacji](the-fix-it-sample-application.md#deploybase).

Aby uruchomić skrypt programu PowerShell zarządzający usługami platformy Azure, należy zainstalować konsolę Azure PowerShell i skonfigurować ją do pracy z subskrypcją platformy Azure. Po skonfigurowaniu programu można uruchomić skrypt rozwiązywania problemów z tworzeniem środowiska IT przy użyciu polecenia takiego jak ten:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name` parametr określa nazwę, która ma być używana podczas tworzenia bazy danych i kont magazynu, a parametr `SqlDatabasePassword` określa hasło dla konta administratora, które zostanie utworzone dla SQL Database. Istnieją inne parametry, których można użyć w późniejszym czasie.

![Okno programu PowerShell](automate-everything/_static/image2.png)

Po zakończeniu działania skryptu można zobaczyć, co zostało utworzone w portalu zarządzania. Znajdziesz dwie bazy danych:

![Bazy danych](automate-everything/_static/image3.png)

Konto magazynu:

![Konto magazynu](automate-everything/_static/image4.png)

I aplikacja sieci Web:

![Witryna sieci Web](automate-everything/_static/image5.png)

Na karcie **Konfiguracja** aplikacji sieci Web można sprawdzić, czy ma ona ustawienia konta magazynu i parametry połączenia usługi SQL Database skonfigurowane dla aplikacji Fix it.

![appSettings i connectionStrings](automate-everything/_static/image6.png)

Folder *automatyzacji* zawiera teraz również plik *&lt;websitename&gt;. pubxml* . Ten plik przechowuje ustawienia używane przez program MSBuild do wdrażania aplikacji w środowisku platformy Azure, który został właśnie utworzony. Na przykład:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Jak widać, skrypt utworzył kompletne środowisko testowe, a cały proces odbywa się za około 90 sekund.

Jeśli ktoś inny w zespole chce utworzyć środowisko testowe, może po prostu uruchomić skrypt. Nie tylko jest to szybkie, ale mogą też mieć pewność, że korzystają z środowiska identycznego z tym, którego używasz. Nie można mieć pewności, że jeśli wszyscy mogli ręcznie skonfigurować elementy przy użyciu interfejsu użytkownika portalu zarządzania.

### <a name="a-look-at-the-scripts"></a>Zapoznaj się ze skryptami

Istnieją trzy skrypty, które wykonują tę czynność. Wywołujesz jeden z wiersza polecenia i automatycznie używamy dwóch pozostałych do wykonania niektórych zadań:

- *New-AzureWebSiteEnv. ps1* jest głównym skryptem.

    - *New-AzureStorage. ps1* tworzy konto magazynu.
    - *New-AzureSql. ps1* tworzy bazy danych.

### <a name="parameters-in-the-main-script"></a>Parametry w skrypcie głównym

Główny skrypt, *New-AzureWebSiteEnv. ps1*, definiuje kilka parametrów:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Wymagane są dwa parametry:

- Nazwa aplikacji sieci Web, którą tworzy skrypt. (Jest to również używane dla adresu URL: `<name>.azurewebsites.net`.)
- Hasło nowego użytkownika administracyjnego serwera bazy danych tworzonego przez skrypt.

Parametry opcjonalne umożliwiają określenie lokalizacji centrum danych (wartość domyślna to "zachodnie stany USA"), nazwę administratora serwera bazy danych (wartość domyślna to "dbuser") i regułę zapory dla serwera bazy danych.

### <a name="create-the-web-app"></a>Tworzenie aplikacji internetowej

Pierwszym krokiem jest utworzenie przez skrypt aplikacji sieci Web przez wywołanie polecenia cmdlet `New-AzureWebsite`, przekazanie do niego wartości parametru Nazwa i lokalizacja aplikacji sieci Web:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Tworzenie konta magazynu

Następnie główny skrypt uruchamia skrypt *New-AzureStorage. ps1* , określając " *&lt;websitename&gt;* Storage" dla nazwy konta magazynu oraz tę samą lokalizację centrum danych, w której znajduje się aplikacja internetowa.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*New-AzureStorage. ps1* wywołuje polecenie cmdlet `New-AzureStorageAccount`, aby utworzyć konto magazynu, i zwraca wartość Nazwa konta i klucz dostępu. Aplikacja będzie potrzebować tych wartości w celu uzyskania dostępu do obiektów blob i kolejek na koncie magazynu.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Być może nie zawsze chcesz utworzyć nowe konto magazynu; można poprawić skrypt przez dodanie parametru, który opcjonalnie kieruje go do korzystania z istniejącego konta magazynu.

### <a name="create-the-databases"></a>Tworzenie baz danych

Główny skrypt następnie uruchamia skrypt tworzenia bazy danych, *New-AzureSql. ps1*, po skonfigurowaniu domyślnej nazwy bazy danych i reguły zapory:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Skrypt tworzenia bazy danych Pobiera adres IP maszyny deweloperskiej i ustawia regułę zapory, aby komputer deweloperski mógł nawiązać połączenie z serwerem i zarządzać nim. Następnie skrypt tworzenia bazy danych przechodzi przez kilka kroków w celu skonfigurowania baz danych:

- Tworzy serwer przy użyciu polecenia cmdlet `New-AzureSqlDatabaseServer`.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Program tworzy reguły zapory, aby umożliwić komputerowi Deweloperskiemu zarządzanie serwerem i umożliwić aplikacji sieci Web łączenie się z nią. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Tworzy kontekst bazy danych, który zawiera nazwę serwera i poświadczenia, za pomocą polecenia cmdlet `New-AzureSqlDatabaseServerContext`.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` jest funkcją w skrypcie, która wywołuje polecenie cmdlet `ConvertTo-SecureString` w celu zaszyfrowania hasła i zwraca obiekt `PSCredential`, ten sam typ, który zwraca polecenie cmdlet `Get-Credential`.
- Tworzy bazę danych aplikacji i bazę danych członkostwa przy użyciu polecenia cmdlet `New-AzureSqlDatabase`.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Wywołuje funkcję zdefiniowaną lokalnie w celu utworzenia parametrów połączenia dla każdej bazy danych. Aplikacja będzie używać tych parametrów połączenia w celu uzyskiwania dostępu do baz danych. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString to funkcja zdefiniowana w skrypcie, która tworzy parametry połączenia z dostarczonych do niego wartości parametrów.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Zwraca tablicę skrótów z nazwą serwera bazy danych i parametrami połączenia.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Poprawka aplikacji IT używa oddzielnych baz danych członkostwa i aplikacji. Możliwe jest również umieszczenie zarówno danych członkostwa, jak i aplikacji w pojedynczej bazie danych.

### <a name="store-app-settings-and-connection-strings"></a>Ustawienia aplikacji ze sklepu i parametry połączenia

Platforma Azure zawiera funkcję, która umożliwia przechowywanie ustawień i parametrów połączenia, które automatycznie przesłaniają dane zwracane do aplikacji podczas próby odczytu `appSettings` lub `connectionStrings` kolekcji w pliku Web. config. Jest to alternatywa dla stosowania [transformacji Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) podczas wdrażania. Aby uzyskać więcej informacji, zobacz artykuł [przechowywanie poufnych danych na platformie Azure](source-control.md#appsettings) w dalszej części tej książki elektronicznej.

Skrypt tworzenia środowiska przechowuje na platformie Azure wszystkie `appSettings` i `connectionStrings` wartości, których aplikacja musi uzyskać dostęp do konta magazynu i baz danych, gdy działa na platformie Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) to struktura telemetrii, którą prezentujemy w rozdziale [monitorowanie i telemetrię](monitoring-and-telemetry.md) . Skrypt tworzenia środowiska również ponownie uruchamia aplikację sieci Web, aby upewnić się, że wybiera nowe ustawienia Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Przygotowywanie do wdrożenia

Na końcu procesu skrypt tworzenia środowiska wywołuje dwie funkcje do tworzenia plików, które będą używane przez skrypt wdrażania.

Jedna z tych funkcji tworzy profil publikacji *(&lt;websitename&gt;. pubxml* ). Kod wywołuje interfejs API REST platformy Azure w celu pobrania ustawień publikowania i zapisuje informacje w pliku *. publishsettings* . Następnie używa informacji z tego pliku wraz z plikiem szablonu (*pubxml. Template*), aby utworzyć plik *. pubxml* zawierający profil publikowania. Ten dwuetapowy proces symuluje działanie w programie Visual Studio: Pobierz plik *. publishsettings* i zaimportuj go w celu utworzenia profilu publikacji.

Druga funkcja używa innego pliku szablonu (witryna sieci Web-Environment. Template) do utworzenia pliku *Website-Environment. XML* zawierającego ustawienia, które będą używane przez skrypt wdrożenia wraz z plikiem *. pubxml* .

### <a name="troubleshooting-and-error-handling"></a>Rozwiązywanie problemów i obsługa błędów

Skrypty są podobne do programów: mogą kończyć się niepowodzeniem, a gdy użytkownik chce wiedzieć, jak to możliwe, i co spowodowało. Z tego powodu skrypt tworzenia środowiska zmienia wartość zmiennej `VerbosePreference` z `SilentlyContinue` na `Continue`, aby wyświetlić wszystkie pełne komunikaty. Zmienia również wartość zmiennej `ErrorActionPreference` z `Continue` na `Stop`, aby skrypt zatrzymał się nawet w przypadku napotkania błędów niepowodujących zakończenia:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Przed wykonaniem jakiejkolwiek pracy skrypt zapisuje czas rozpoczęcia, aby mógł obliczyć czas, który upłynął:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Po zakończeniu pracy w skrypcie zostanie wyświetlony czas, który upłynął:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

I dla każdej operacji na kluczu skrypt zapisuje pełne komunikaty, na przykład:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Skrypt wdrażania

Co robi skrypt *New-AzureWebsiteEnv. ps1* na potrzeby tworzenia środowiska, skrypt *Publish-AzureWebsite. ps1* służy do wdrażania aplikacji.

Skrypt wdrożenia Pobiera nazwę aplikacji sieci Web z pliku *Website-Environment. XML* utworzonego przez skrypt tworzenia środowiska.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Pobiera hasło użytkownika wdrożenia z pliku *. publishsettings* :

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Wykonuje polecenie [MSBuild](http://msbuildbook.com/) , które kompiluje i wdraża projekt:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

A jeśli w wierszu polecenia określono parametr `Launch`, program wywoła `Show-AzureWebsite` polecenie cmdlet w celu otworzenia domyślnej przeglądarki do adresu URL witryny sieci Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Skrypt wdrażania można uruchomić za pomocą polecenia takiego jak ten:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Po zakończeniu zostanie otwarta przeglądarka z witryną działającą w chmurze pod adresem URL `<websitename>.azurewebsites.net`.

![Napraw aplikację IT wdrożoną na platformie Microsoft Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Podsumowanie

Za pomocą tych skryptów można mieć pewność, że te same kroki będą zawsze wykonywane w tej samej kolejności, przy użyciu tych samych opcji. Pozwala to zagwarantować, że każdy deweloper w zespole nie pominie czegoś ani nie wyeliminuje czegoś lub nie wykonuje żadnych operacji niestandardowych na własnym komputerze, który faktycznie nie działa tak samo jak w środowisku innego członka zespołu lub w produkcji.

W podobny sposób można zautomatyzować większość funkcji zarządzania platformy Azure, które można wykonać w portalu zarządzania, za pomocą interfejsu API REST, skryptów programu Windows PowerShell, interfejsu API języka .NET lub narzędzia bash, które można uruchomić w systemie Linux lub Mac.

W [następnym rozdziale](source-control.md) zaobserwujemy kod źródłowy i wyjaśnimy, dlaczego warto uwzględnić skrypty w repozytorium kodu źródłowego.

## <a name="resources"></a>Zasoby

- [Zainstaluj i skonfiguruj program Windows PowerShell dla platformy Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). W tym artykule wyjaśniono, jak zainstalować Azure PowerShell polecenia cmdlet oraz jak zainstalować certyfikat wymagany na komputerze, aby zarządzać kontem platformy Azure. Jest to doskonałe miejsce do rozpoczęcia pracy, ponieważ ma także linki do zasobów dla programu PowerShell.
- [Centrum skryptów platformy Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portal WindowsAzure.com do zasobów na potrzeby tworzenia skryptów, które zarządzają usługami platformy Azure, z linkami do samouczków z wprowadzeniem, dokumentacją i kodem źródłowym oraz przykładowym skryptem.
- [Skrypt weekendowy: wprowadzenie z platformą Azure i programem PowerShell](https://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). W blogu przeznaczonym dla programu Windows PowerShell ten wpis zawiera doskonałe wprowadzenie do korzystania z programu PowerShell dla funkcji zarządzania platformy Azure.
- [Zainstaluj i skonfiguruj Międzyplatformowy interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Samouczek ułatwiający rozpoczęcie pracy z platformą Azure Scripting, która działa w systemach Mac i Linux oraz na komputerach z systemem Windows.
- [Sekcja narzędzi wiersza polecenia w temacie Pobieranie zestawów SDK i narzędzi platformy Azure](https://azure.microsoft.com/downloads/). Strona portalu dla dokumentacji i plików do pobrania związanych z narzędziami wiersza polecenia dla systemu Azure.
- [Automatyzacja wszystkiego przy użyciu bibliotek zarządzania platformy Azure i platformy .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman wprowadza interfejs API zarządzania .NET dla platformy Azure.
- [Publikowanie w środowiskach deweloperskich i testowych przy użyciu skryptów środowiska Windows PowerShell](https://msdn.microsoft.com/library/azure/dn642480.aspx). Dokumentacja MSDN, która wyjaśnia, jak używać skryptów publikowania, które program Visual Studio automatycznie generuje dla projektów sieci Web.
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Rozszerzenie programu Visual Studio, które dodaje obsługę języka dla środowiska Windows PowerShell w programie Visual Studio.

> [!div class="step-by-step"]
> [Poprzednie](introduction.md)
> [dalej](source-control.md)

---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET odmowa dostępu do katalogów usług IIS | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji ASP.NET zwróci błąd, "odmowa dostępu do katalogu DirectoryName. Nie powiodło się...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638503"
---
# <a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="8f9a4-104">Platforma ASP.NET nie ma dostępu do katalogów usług IIS</span><span class="sxs-lookup"><span data-stu-id="8f9a4-104">ASP.NET Denied Access to IIS Directories</span></span>

> <span data-ttu-id="8f9a4-105">W tym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji ASP.NET zwróci błąd, "odmowa dostępu do katalogu *DirectoryName* .</span><span class="sxs-lookup"><span data-stu-id="8f9a4-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="8f9a4-106">Nie można rozpocząć monitorowania zmian w katalogu. "</span><span class="sxs-lookup"><span data-stu-id="8f9a4-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="8f9a4-107">Dotyczy ASP.NET 1,0 i ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="8f9a4-108">ASP.NET v1 RTM jest teraz uruchamiany przy użyciu mniej uprzywilejowanego konta systemu Windows, które jest rejestrowane jako konto "ASPNET" na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="8f9a4-109">W niektórych zablokowanych systemach to konto może nie mieć domyślnie uprawnień do odczytu do katalogów zawartości witryny sieci Web, katalogu głównego aplikacji lub katalogu głównego witryny internetowej.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="8f9a4-110">W takim przypadku podczas żądania stron z danej aplikacji sieci Web zostanie wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="8f9a4-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="8f9a4-111">Aby rozwiązać ten problem, należy zmienić uprawnienia zabezpieczeń do odpowiednich katalogów.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="8f9a4-112">W odniesieniu do ASP.NET wymaga dostępu do odczytu, wykonywania i wyświetlania listy dla konta ASPNET dla katalogu głównego witryny sieci Web (na przykład: c:\Inetpub\Wwwroot lub dowolnego alternatywnego katalogu witryn, który mógł zostać skonfigurowany w usługach IIS), katalogu zawartości i katalogu głównego aplikacji. w celu monitorowania zmian w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="8f9a4-113">Katalog główny aplikacji odpowiada ścieżce folderu skojarzonej z katalogiem wirtualnym aplikacji w narzędziu administracyjnym usług IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="8f9a4-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="8f9a4-114">Rozważmy na przykład następującą hierarchię aplikacji w folderze wwwroot.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="8f9a4-115">Na potrzeby tego przykładu konto ASPNET wymaga uprawnień do odczytu zdefiniowanych powyżej dla zawartości w katalogu MojaApl i wwwroot.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="8f9a4-116">Pojedyncza dziedziczona lista kontroli dostępu w folderze głównym może być również opcjonalnie użyta dla obu katalogów, jeśli są one zagnieżdżone.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="8f9a4-117">Aby dodać uprawnienia do katalogu, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8f9a4-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="8f9a4-118">Korzystając z Eksploratora Windows, przejdź do katalogu</span><span class="sxs-lookup"><span data-stu-id="8f9a4-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="8f9a4-119">Kliknij prawym przyciskiem myszy folder katalogu i wybierz pozycję "właściwości".</span><span class="sxs-lookup"><span data-stu-id="8f9a4-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="8f9a4-120">Przejdź do karty "zabezpieczenia" w oknie dialogowym właściwości</span><span class="sxs-lookup"><span data-stu-id="8f9a4-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="8f9a4-121">Kliknij przycisk "Dodaj" i wprowadź nazwę komputera, a po nim nazwę konta ASPNET.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="8f9a4-122">Na przykład na komputerze o nazwie "webdev" wpisz webdev\ASPNET i naciśnij przycisk "OK".</span><span class="sxs-lookup"><span data-stu-id="8f9a4-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="8f9a4-123">Upewnij się, że konto ASPNET ma zaznaczone pola wyboru "Czytaj &amp; Execute", "Wyświetl zawartość folderu" i "read".</span><span class="sxs-lookup"><span data-stu-id="8f9a4-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="8f9a4-124">Naciśnij przycisk OK, aby zamknąć okno dialogowe i zapisać zmiany.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="8f9a4-125">W razie potrzeby te zmiany można zautomatyzować za pomocą skryptów lub narzędzia "cacls. exe", które jest dostarczane z systemem Windows.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="8f9a4-126">Aby uzyskać więcej informacji na temat konta ASPNET, zapoznaj się z [dokumentem często zadawanych pytań](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="8f9a4-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="8f9a4-127">Jeśli dana aplikacja sieci Web opiera się na uprawnieniach zapisu lub modyfikacji określonego folderu lub pliku, można to udzielić, wykonując tę samą procedurę i sprawdzając pola wyboru "Write" i/lub "Modify".</span><span class="sxs-lookup"><span data-stu-id="8f9a4-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="8f9a4-128">Na maszynach zezwalających wszystkim lub użytkownikom na dostęp do odczytu do tych katalogów (co jest konfiguracją domyślną) nie zostaną napotkane żadne problemy i powyższe kroki nie będą wymagane.</span><span class="sxs-lookup"><span data-stu-id="8f9a4-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>

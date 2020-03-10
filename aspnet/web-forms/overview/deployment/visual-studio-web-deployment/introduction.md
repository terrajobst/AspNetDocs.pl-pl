---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'ASP.NET Web Deployment przy użyciu programu Visual Studio: wprowadzenie | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub dostawcy hostingu innej firmy przy użyciu programu V...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 96dd31d949633e001fc595621bedbf74e98000fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642220"
---
# <a name="aspnet-web-deployment-using-visual-studio-introduction"></a>ASP.NET Web Deployment przy użyciu programu Visual Studio: wprowadzenie

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) Azure App Service aplikację sieci Web ASP.NET Web Apps lub dostawcę hostingu innej firmy przy użyciu programu Visual Studio 2012 i zestawu Azure SDK dla platformy .NET. Większość procedur jest podobna do Visual Studio 2013.
> 
> Tworzysz aplikację sieci Web, aby była dostępna dla osób za pośrednictwem Internetu. Jednak samouczki programistyczne dla sieci Web zwykle zatrzymują się bezpośrednio po pokazaniu, jak uzyskać coś pracy na komputerze deweloperskim. W tej serii samouczków zaczynają się inne osoby: utworzono aplikację internetową, przetestowano ją i wszystko jest gotowe do użycia. Co dalej? W tych samouczkach pokazano, jak najpierw wdrożyć usługi IIS na lokalnym komputerze deweloperskim do testowania, a następnie do platformy Azure lub dostawcy hostingu innej firmy na potrzeby przemieszczania i produkcji. Wdrażana aplikacja jest projektem aplikacji sieci Web, który używa Entity Framework, SQL Server i systemu członkostwa ASP.NET. Przykładowa aplikacja używa formularzy sieci Web ASP.NET, ale przedstawione procedury dotyczą również ASP.NET MVC i Web API.
> 
> W tych samouczkach założono, że wiesz, jak korzystać z ASP.NET w programie Visual Studio. Jeśli nie, dobrym miejscem do rozpoczęcia jest [podstawowy samouczek dotyczący formularzy sieci Web w ASP.NET](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) lub [podstawowy samouczek MVC ASP.NET](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum wdrażania ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) lub [StackOverflow](http://stackoverflow.com).
> 
> Ta zawartość jest również dostępna jako bezpłatna książka elektroniczna w [galerii książek elektronicznych TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).

## <a name="overview"></a>Omówienie

Te samouczki przeprowadzą Cię przez proces wdrażania aplikacji sieci Web ASP.NET, która zawiera bazy danych SQL Server. Najpierw wdrażasz usługi IIS na lokalnym komputerze deweloperskim do testowania, a następnie Web Apps w Azure App Service i Azure SQL Database na potrzeby przygotowania i produkcji. Aby dowiedzieć się, jak wdrożyć usługę za pomocą programu Visual Studio, kliknij pozycję Publikuj, aby zobaczyć, jak wdrożyć program przy użyciu wiersza polecenia.

Liczba samouczków może sprawiać, że proces wdrażania będzie zniechęcające. W rzeczywistości podstawowe procedury są proste. Jednak w rzeczywistych sytuacjach często trzeba wykonać dodatkowe zadania wdrażania, na przykład ustawiając uprawnienia do folderu na serwerze docelowym. Zostały przedstawione niektóre z tych dodatkowych zadań, co gwarantuje, że samouczki nie pozostawiają informacji, które mogą uniemożliwić pomyślne wdrożenie rzeczywistej aplikacji.

Samouczki zostały zaprojektowane tak, aby były uruchamiane w kolejności, a każda z nich kompiluje się w poprzedniej części. Możesz pominąć części, które nie są związane z twoją sytuacją, ale może być konieczne dostosowanie procedur w kolejnych samouczkach.

## <a name="intended-audience"></a>Docelowi odbiorcy

Samouczki mają na celu ASP.NET deweloperów, którzy pracują w środowiskach, w których:

- Środowisko produkcyjne jest Azure App Service Web Apps lub dostawcy hostingu innej firmy.
- Wdrożenie nie jest ograniczone do procesu ciągłej integracji, ale może być wykonywane bezpośrednio z programu Visual Studio.

Te samouczki nie obejmują wdrożenia z [kontroli źródła](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) przy użyciu procesu [ciągłego dostarczania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) , z wyjątkiem jednego samouczka, w którym pokazano, jak wdrożyć program z wiersza polecenia. Aby uzyskać informacje na temat ciągłego dostarczania, zobacz następujące zasoby:

- [Ciągła integracja i ciągłe dostarczanie (Tworzenie aplikacji w chmurze w rzeczywistych warunkach w systemie Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Wdrażanie aplikacji sieci Web w programie Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (starszy zestaw samouczków pisanych dla programu Visual Studio 2010, który nadal ma przydatne informacje dla środowisk przedsiębiorstwa).

## <a name="using-a-third-party-hosting-provider"></a>Korzystanie z dostawcy hostingu innej firmy

Samouczki przeprowadzi Cię przez proces konfigurowania konta platformy Azure i wdrażania aplikacji do Web Apps w Azure App Service na potrzeby przemieszczania i produkcji. Można jednak zastosować te same procedury podstawowe do wdrożenia w wybranym dostawcy hostingu innej firmy. W przypadku, gdy samouczki przechodzą procesy unikatowe na platformę Azure, wyjaśniają one, że i doradzają, jakie różnice można oczekiwać od dostawcy hostingu innej firmy.

## <a name="deploying-web-app-projects"></a>Wdrażanie projektów aplikacji sieci Web

Przykładowa aplikacja pobierana i wdrażana na potrzeby tych samouczków jest projektem aplikacji sieci Web programu Visual Studio. Jeśli jednak instalujesz najnowszą aktualizację publikacji w sieci Web dla programu Visual Studio, możesz użyć tych samych metod wdrażania i narzędzi dla projektów aplikacji sieci Web.

## <a name="deploying-aspnet-mvc-projects"></a>Wdrażanie projektów ASP.NET MVC

Przykładową aplikacją jest projekt ASP.NET Web Forms, ale wszystko, co można zrobić, ma zastosowanie również do ASP.NET MVC. Projekt programu Visual Studio MVC jest po prostu kolejną formą projektu aplikacji sieci Web. Jedyną różnicą jest to, że jeśli wdrażasz program w ramach dostawcy hostingu, który nie obsługuje ASP.NET MVC lub wersji docelowej, musisz upewnić się, że zainstalowano odpowiedni pakiet NuGet ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)lub [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) w projekcie.

## <a name="programming-language"></a>Język programowania

Aplikacja Przykładowa używa C# programu C#, ale samouczki nie wymagają znajomości, a techniki wdrażania wyświetlane w samouczkach nie są specyficzne dla języka.

## <a name="database-deployment-methods"></a>Metody wdrażania bazy danych

Istnieją trzy sposoby wdrażania bazy danych SQL Server wraz z wdrażaniem w sieci Web w programie Visual Studio:

- migracje Code First platformy Entity Framework
- Dostawca Web Deploy dbDacFx
- Dostawca Web Deploy dbFullSql

W tym samouczku zostaną użyte pierwsze dwie z tych metod. Dostawca Web Deploy dbFullSql jest starszą metodą, która nie jest już zalecana, z wyjątkiem określonych scenariuszy, takich jak Migrowanie z SQL Server Compact do SQL Server.

Metody przedstawione w tym samouczku dotyczą SQL Server baz danych, a nie SQL Server Compact. Informacje o sposobie wdrażania bazy danych SQL Server Compact można znaleźć w temacie [wdrażanie w sieci Web w programie Visual Studio z SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Metody przedstawione w tym samouczku wymagają użycia metody publikowania Web Deploy. Jeśli wolisz inną metodę publikowania, taką jak FTP, system plików lub FPSE, zobacz [wdrażanie bazy danych oddzielnie od wdrażania aplikacji sieci Web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) na mapie zawartości wdrożenia sieci Web dla programu Visual Studio i ASP.NET.

### <a name="entity-framework-code-first-migrations"></a>migracje Code First platformy Entity Framework

W Entity Framework w wersji 4,3, firma Microsoft wprowadziła Migracje Code First. Migracje Code First automatyzuje proces dokonywania zmian przyrostowych w modelu danych i propagowania tych zmian do bazy danych. We wcześniejszych wersjach Code First zwykle pozwala Entity Framework porzucić i ponownie utworzyć bazę danych przy każdej zmianie modelu danych. Nie jest to problem podczas opracowywania, ponieważ dane testowe można łatwo ponownie utworzyć, ale w środowisku produkcyjnym zwykle chcesz zaktualizować schemat bazy danych bez porzucania bazy danych. Funkcja migracji umożliwia Code First aktualizowanie bazy danych bez porzucania i ponownego tworzenia. Można zezwolić Code First automatycznie decydować, jak wprowadzić wymagane zmiany schematu, lub napisać kod, który dostosowuje zmiany. Aby zapoznać się z wprowadzeniem do Migracje Code First, zobacz [migracje Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Podczas wdrażania projektu sieci Web program Visual Studio może zautomatyzować proces wdrażania bazy danych, która jest zarządzana przez Migracje Code First. Podczas tworzenia profilu publikowania należy zaznaczyć pole wyboru z etykietą wykonaj Migracje Code First (uruchamiany przy uruchamianiu aplikacji). To ustawienie powoduje, że proces wdrażania automatycznie skonfiguruje plik Web. config aplikacji na serwerze docelowym, tak aby Code First używa klasy inicjatora `MigrateDatabaseToLatestVersion`.

Program Visual Studio nie robi niczego w przypadku bazy danych podczas procesu wdrażania. Gdy wdrożona aplikacja uzyskuje dostęp do bazy danych po raz pierwszy po wdrożeniu, Code First automatycznie tworzy bazę danych lub aktualizuje schemat bazy danych do najnowszej wersji. Jeśli aplikacja implementuje metodę inicjatora migracji, metoda jest uruchamiana po utworzeniu bazy danych lub zaktualizowaniu schematu.

W tym samouczku użyjesz Migracje Code First do wdrożenia bazy danych aplikacji.

### <a name="the-dbdacfx-web-deploy-provider"></a>Dostawca Web Deploy dbDacFx

W przypadku bazy danych SQL Server, która nie jest zarządzana przez Entity Framework Code First, można zaznaczyć pole wyboru z etykietą Aktualizuj bazę danych podczas konfigurowania profilu publikowania. Podczas początkowego wdrażania dostawca dbDacFx tworzy tabele i inne obiekty bazy danych w docelowej bazie danych, aby odpowiadały źródłowej bazie danych. W przypadku kolejnych wdrożeń dostawca określa, co jest inne niż źródłowa i docelowa baza danych, i aktualizuje schemat docelowej bazy danych w celu dopasowania do źródłowej bazy danych. Domyślnie dostawca nie wprowadza żadnych zmian, które powodują utratę danych, na przykład gdy tabela lub kolumna zostanie porzucona.

Ta metoda nie automatyzuje wdrażania danych w tabelach bazy danych, ale można utworzyć skrypty, aby to zrobić, i skonfigurować program Visual Studio do uruchamiania ich podczas wdrażania. Innym powodem uruchamiania skryptów podczas wdrażania jest wprowadzenie zmian w schemacie, które nie mogą być wykonywane automatycznie, ponieważ spowodują utratę danych.

W tym samouczku użyjesz dostawcy dbDacFx do wdrożenia bazy danych członkostwa ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Rozwiązywanie problemów w ramach tego samouczka

Jeśli podczas wdrażania wystąpi błąd lub wdrożona witryna nie działa prawidłowo, komunikaty o błędach nie zawsze zapewniają wyraźne rozwiązanie. Aby ułatwić Ci typowe scenariusze problemu, dostępna jest [Strona referencyjna dotycząca rozwiązywania problemów](troubleshooting.md) . Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie zadziała podczas wykonywania samouczków, pamiętaj, aby sprawdzić stronę rozwiązywania problemów.

## <a name="comments-welcome"></a>Komentarze — Zapraszamy!

Komentarze samouczków są powitane, a po zaktualizowaniu samouczka zostanie podjęta potrzeba wprowadzenia poprawek lub sugestii dotyczących ulepszeń, które są dostępne w komentarzach samouczka.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek został zapisany dla następujących produktów:

- Windows 8 lub Windows 7.
- Program Visual Studio 2012 lub Visual Studio 2012 Express for Web z [najnowszą aktualizacją](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Zestaw Azure SDK dla programu Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Możesz wykonać czynności opisane w samouczku za pomocą programu Visual Studio 2010 SP1 lub Visual Studio 2013, ale niektóre zrzuty ekranu będą różne, a niektóre funkcje będą różne.

Jeśli używasz Visual Studio 2013, zainstaluj [zestaw Azure SDK dla Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Jeśli używasz programu Visual Studio 2010 z dodatkiem SP1, Zainstaluj następujące oprogramowanie:

- [Zestaw Azure SDK dla programu Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [Narzędzia danych SQL Server](https://msdn.microsoft.com/library/hh500335.aspx).

W zależności od liczby zależności zestawu SDK, które znajdują się już na komputerze, Instalowanie zestawu Azure SDK może zająć dużo czasu od kilku minut do pół godziny lub dłużej. Zestaw Azure SDK jest potrzebny nawet wtedy, gdy planujesz publikowanie do dostawcy hostingu innej firmy zamiast na platformie Azure, ponieważ zestaw SDK zawiera najnowsze aktualizacje funkcji publikowania w sieci Web programu Visual Studio.

> [!NOTE]
> Ten samouczek został zapisany w wersji 1.8.1 zestawu Azure SDK. Od tego czasu wydano nowsze wersje z dodatkowymi funkcjami. Samouczki zostały zaktualizowane, aby wymienić te funkcje i linki do zasobów, które zawierają więcej informacji na ich temat.

Instrukcje i zrzuty ekranu są oparte na systemie Windows 8, ale samouczki objaśniają różnice dotyczące systemu Windows 7.

Inne oprogramowanie jest wymagane w celu ukończenia tego samouczka, ale nie trzeba go jeszcze instalować. Ten samouczek przeprowadzi Cię przez kroki instalacji tego programu w razie potrzeby.

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

Wdrażana aplikacja ma nazwę Contoso University i została już utworzona. Jest to uproszczona wersja witryny sieci Web University, oparta na luźnej aplikacji firmy Contoso University opisana w [samouczkach Entity Framework w witrynie ASP.NET](https://asp.net/entity-framework/tutorials).

Po zainstalowaniu wymagań wstępnych Pobierz [aplikację sieci Web firmy Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). Plik *. zip* zawiera wiele wersji projektu. Aby wykonać kroki samouczka, Zacznij od projektu znajdującego się w C# folderze. Aby zobaczyć, jak wygląda projekt na końcu samouczków, Otwórz projekt w folderze ContosoUniversity-end.

Aby przygotować projekt do pracy w ramach kroków samouczka, wykonaj następujące czynności:

1. Zapisz pliki rozwiązań ContosoUniversity z C# folderu w folderze o nazwie ContosoUniversity w dowolnym folderze używanym do pracy z projektami programu Visual Studio.

    Domyślnie jest to następujący folder dla programu Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (W przypadku zrzutów ekranu w tym samouczku folder projektu znajduje się w katalogu głównym na `C`: dysk).
2. Uruchom program Visual Studio i Otwórz projekt.
3. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **przywracanie pakietu EnableNuGet**.
4. Skompiluj rozwiązanie.
5. W przypadku wystąpienia błędów kompilacji ręcznie Przywróć pakiety NuGet:

    1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Zarządzaj pakietami NuGet dla rozwiązania**.
    2. W górnej części okna dialogowego **Zarządzanie pakietami NuGet** w **tym rozwiązaniu brakuje niektórych pakietów NuGet. Kliknij, aby przywrócić.** Kliknij przycisk **Przywróć** .
    3. Ponownie skompiluj rozwiązanie.
6. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

    Aplikacja zostanie otwarta na stronie głównej programu Contoso University.

    ![Strona główna — Tworzenie](introduction/_static/image1.png)

    (Może wystąpić czas oczekiwania, gdy program Visual Studio uruchomi wystąpienie SQL Server Express LocalDB i jeśli ten proces trwa zbyt długo, może wystąpić błąd limitu czasu. W takim przypadku po prostu Rozpocznij projekt ponownie).

Strony witryny sieci Web są dostępne z paska menu i umożliwiają wykonywanie następujących zadań:

- Wyświetlanie statystyk uczniów (strona informacje).
- Wyświetlanie, edytowanie, usuwanie i Dodawanie uczniów.
- Wyświetlanie i edytowanie kursów.
- Wyświetlanie i edytowanie instruktorów.
- Wyświetlanie i edytowanie działów.

Poniżej przedstawiono zrzuty ekranu kilku reprezentatywnych stron.

![Strona Students dev](introduction/_static/image2.png)

![Dodawanie stron uczniów](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Przeglądanie funkcji aplikacji, które mają wpływ na wdrażanie

Poniższe funkcje aplikacji mają wpływ na sposób ich wdrażania lub czynności, które należy wykonać w celu wdrożenia. Każdy z tych informacji został szczegółowo opisany w następujących samouczkach w serii.

- Firma Contoso University używa bazy danych SQL Server do przechowywania danych aplikacji, takich jak uczniowie i nazwiska instruktorów. Baza danych zawiera kombinację danych testowych i danych produkcyjnych, a podczas wdrażania programu w środowisku produkcyjnym należy wykluczyć dane testowe.
- Aplikacja używa systemu członkostwa ASP.NET, który przechowuje informacje o koncie użytkownika w bazie danych SQL Server. Aplikacja definiuje użytkownika administratora, który ma dostęp do niektórych informacji z ograniczeniami. Należy wdrożyć bazę danych członkostwa bez kont testowych, ale przy użyciu konta administratora.
- Aplikacja używa narzędzia do rejestrowania błędów innych firm i raportowania. To narzędzie jest dostępne w zestawie, który musi zostać wdrożony z aplikacją.
- Narzędzie rejestrowania błędów zapisuje informacje o błędach w plikach XML do folderu plików. Musisz się upewnić, że konto, które ASP.NET jest uruchamiane w ramach wdrożonej lokacji, ma uprawnienie do zapisu w tym folderze, a ty musisz wykluczyć ten folder z wdrożenia. (W przeciwnym razie dane dziennika błędów ze środowiska testowego mogą zostać wdrożone w produkcji i/lub pliki dzienników błędów produkcyjnych mogą zostać usunięte).
- Aplikacja zawiera pewne ustawienia, które należy zmienić we wdrożonym pliku *Web. config* w zależności od środowiska docelowego (test, przemieszczanie lub produkcja), a także inne ustawienia, które należy zmienić, w zależności od konfiguracji kompilacji (debugowanie lub wydanie).
- Rozwiązanie programu Visual Studio zawiera projekt biblioteki klas. Należy wdrożyć tylko zestaw, który zostanie wygenerowany przez ten projekt, a nie sam projekt.

## <a name="summary"></a>Podsumowanie

W tym pierwszym samouczku w serii pobrano przykładowy projekt programu Visual Studio i przegląd funkcji witryny, które mają wpływ na sposób wdrażania aplikacji. W poniższych samouczkach przygotowano do wdrożenia przez skonfigurowanie niektórych z tych elementów do obsługi automatycznej. Inne osoby, które postanowisz ręcznie.

> [!div class="step-by-step"]
> [Dalej](preparing-databases.md)

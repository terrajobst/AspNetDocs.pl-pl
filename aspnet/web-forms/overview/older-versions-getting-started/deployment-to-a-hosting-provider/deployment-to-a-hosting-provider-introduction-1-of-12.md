---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio: wprowadzenie-1 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: ea88da1e6d510f706fc7ca370cfa32974c1243f8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587741"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio: wprowadzenie-1 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web.
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Te samouczki przeprowadzą Cię przez proces wdrażania usług IIS na lokalnym komputerze deweloperskim do testowania, a następnie do dostawcy hostingu innej firmy. Wdrażana aplikacja korzysta z bazy danych aplikacji i bazy danych członkostwa ASP.NET. Zacznij korzystać z usługi SQL Server Compact i wdrażania na SQL Server Compact, a w dalszych samouczkach pokazano, jak wdrożyć zmiany bazy danych i jak przeprowadzić migrację do SQL Server.
> 
> W samouczkach założono, że wiesz, jak korzystać z ASP.NET w programie Visual Studio. Jeśli nie, dobrym miejscem do rozpoczęcia jest [podstawowy samouczek dotyczący formularzy sieci Web w ASP.NET](../tailspin-spyworks/tailspin-spyworks-part-1.md) lub [podstawowy samouczek MVC ASP.NET](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum wdrażania ASP.NET](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).

## <a name="overview"></a>Omówienie

Te samouczki przeprowadzą Cię przez proces wdrażania usług IIS na lokalnym komputerze deweloperskim do testowania, a następnie do dostawcy hostingu innej firmy. Wdrażana aplikacja korzysta z bazy danych aplikacji i bazy danych członkostwa ASP.NET. Zacznij korzystać z usługi SQL Server Compact i wdrażania na SQL Server Compact, a w dalszych samouczkach pokazano, jak wdrożyć zmiany bazy danych i jak przeprowadzić migrację do SQL Server.

Liczba samouczków — 11 we wszystkich i stronach rozwiązywania problemów — może spowodować, że proces wdrażania będzie zniechęcające. W rzeczywistości podstawowe procedury wdrażania lokacji składają się na stosunkowo małą część zestawu samouczków. Jednak w rzeczywistych sytuacjach często potrzebne są informacje o niewielkim, ale istotnym dodatkowym aspekcie wdrożenia — na przykład ustawiając uprawnienia do folderu na serwerze docelowym. W samouczkach dodaliśmy wiele z tych dodatkowych technik, z tą różnicą, że samouczki nie pozostawiają informacji, które mogą uniemożliwić pomyślne wdrożenie rzeczywistej aplikacji.

Samouczki zostały zaprojektowane tak, aby były uruchamiane w kolejności, a każda z nich kompiluje się w poprzedniej części. Można jednak pominąć części, które nie są związane z twoją sytuacją. (Części pomijające mogą wymagać dostosowania procedur w kolejnych samouczkach).

## <a name="intended-audience"></a>Zamierzone odbiorcy

Samouczki mają na celu ASP.NET deweloperów, którzy pracują w małych organizacjach i innych środowiskach, w których:

- Proces ciągłej integracji (zautomatyzowane kompilacje i wdrażanie) nie jest używany.
- Środowisko produkcyjne jest dostawcą hostingu innej firmy.
- Jedna osoba zwykle wypełnia wiele ról (ta sama osoba opracowuje, testuje i wdraża).

W środowiskach korporacyjnych jest bardziej typowym wdrożeniem procesów ciągłej integracji, a środowisko produkcyjne jest zwykle hostowane przez własne serwery firmy. Różne osoby również zwykle wykonują różne role. Aby uzyskać informacje o wdrażaniu w przedsiębiorstwie, zobacz [wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Organizacje wszystkich rozmiarów mogą również wdrażać aplikacje sieci Web na platformie Azure, a większość procedur przedstawionych w tych samouczkach ma zastosowanie również do usługi Azure App Services Web Apps. Aby zapoznać się z wprowadzeniem do platformy Azure, zobacz [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Dostawca hostingu widoczny w samouczkach

Samouczki przeprowadzi Cię przez proces konfigurowania konta z firmą hostingową i wdrażania aplikacji dla tego dostawcy usług hostingowych. Określona firma hostingu została wybrana, aby samouczki mogły zilustrować pełne środowisko wdrażania w działającej witrynie sieci Web. Każda firma hostingu oferuje różne funkcje, a środowisko wdrażania na ich serwerach różni się nieco. Jednak proces opisany w tym samouczku jest typowy dla całego procesu.

Dostawca hostingu używany w tym samouczku, Cytanium.com, jest jednym z dostępnych, a jego użycie w tym samouczku nie stanowi adnotacji ani rekomendacji.

## <a name="deploying-web-site-projects"></a>Wdrażanie projektów witryny sieci Web

Firma Contoso University to projekt aplikacji sieci Web programu Visual Studio. Większość metod wdrażania i narzędzi w tym samouczku nie ma zastosowania do [projektów witryny sieci Web](https://msdn.microsoft.com/library/dd547590.aspx). Aby uzyskać informacje na temat sposobu wdrażania projektów witryn sieci Web, zobacz [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Wdrażanie projektów ASP.NET MVC

W tym samouczku wdrażany jest projekt ASP.NET Web Forms, ale wszystko, co należy zrobić, ma zastosowanie również do ASP.NET MVC. Projekt programu Visual Studio MVC jest po prostu kolejną formą projektu aplikacji sieci Web. Jedyną różnicą jest to, że jeśli wdrażasz program w ramach dostawcy hostingu, który nie obsługuje ASP.NET MVC lub wersji docelowej, musisz upewnić się, że zainstalowano odpowiedni pakiet NuGet ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) lub [MVC 4](http://nuget.org/packages/aspnetmvc)) w projekcie.

## <a name="programming-language"></a>Język programowania

Aplikacja Przykładowa używa C# programu C#, ale samouczki nie wymagają znajomości, a techniki wdrażania wyświetlane w samouczkach nie są specyficzne dla języka.

## <a name="troubleshooting-during-this-tutorial"></a>Rozwiązywanie problemów w ramach tego samouczka

Gdy wystąpi błąd podczas wdrażania lub wdrożona witryna nie działa prawidłowo, komunikaty o błędach nie zawsze zapewniają rozwiązanie. Aby ułatwić Ci typowe scenariusze problemu, dostępna jest [Strona referencyjna dotycząca rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) . Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie zadziała podczas wykonywania samouczków, pamiętaj, aby sprawdzić stronę rozwiązywania problemów.

## <a name="comments-welcome"></a>Komentarze — Zapraszamy!

Komentarze samouczków są powitane, a po zaktualizowaniu samouczka zostanie podjęta potrzeba wprowadzenia poprawek lub sugestii dotyczących ulepszeń, które są dostępne w komentarzach samouczka.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że na komputerze jest zainstalowany system Windows 7 lub nowszy oraz jeden z następujących produktów:

- [Program Visual Studio 2010 z dodatkiem SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 z dodatkiem SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Jeśli masz program Visual Studio 2010 z dodatkiem SP1 lub Visual Web Developer Express 2010 SP1, Zainstaluj następujące produkty również:

- [Zestaw Azure SDK dla programu .NET (VS 2010 z dodatkiem SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (obejmuje aktualizację publikacji w sieci Web)
- [Narzędzia Microsoft Visual Studio 2010 SP1 dla SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Inne oprogramowanie jest wymagane w celu ukończenia tego samouczka, ale nie trzeba go jeszcze ładować. Ten samouczek przeprowadzi Cię przez kroki instalacji tego programu w razie potrzeby.

## <a name="downloading-the-sample-application"></a>Pobieranie przykładowej aplikacji

Wdrażana aplikacja ma nazwę Contoso University i została już utworzona. Jest to uproszczona wersja witryny sieci Web University, oparta na luźnej aplikacji firmy Contoso University opisana w [samouczkach Entity Framework w witrynie ASP.NET](https://asp.net/entity-framework/tutorials).

Po zainstalowaniu wymagań wstępnych Pobierz [aplikację sieci Web firmy Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Plik *. zip* zawiera wiele wersji projektu i plik PDF zawierający wszystkie 12 samouczków. Aby wykonać kroki samouczka, Zacznij od ContosoUniversity — BEGIN. Aby zobaczyć, jak wygląda projekt na końcu samouczków, Otwórz ContosoUniversity-end. Aby zobaczyć, jak wygląda projekt przed migracją do pełnego SQL Server w samouczku 10, Otwórz ContosoUniversity-AfterTutorial09.

Aby przygotować się do wykonania kroków samouczka, Zapisz ContosoUniversity — Rozpocznij pracę z dowolnym folderem używanym do pracy z projektami programu Visual Studio. Domyślnie jest to następujący folder:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(W przypadku zrzutów ekranu w tym samouczku folder projektu znajduje się w katalogu głównym na `C`: dysk).

Uruchom program Visual Studio, Otwórz projekt i naciśnij klawisze CTRL-F5, aby go uruchomić.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Strony witryny sieci Web są dostępne z paska menu i umożliwiają wykonywanie następujących zadań:

- Wyświetlanie statystyk uczniów (strona informacje).
- Wyświetlanie, edytowanie, usuwanie i Dodawanie uczniów.
- Wyświetlanie i edytowanie kursów.
- Wyświetlanie i edytowanie instruktorów.
- Wyświetlanie i edytowanie działów.

Poniżej przedstawiono zrzuty ekranu kilku reprezentatywnych stron.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Przeglądanie funkcji aplikacji, które mają wpływ na wdrażanie

Poniższe funkcje aplikacji mają wpływ na sposób ich wdrażania lub czynności, które należy wykonać w celu wdrożenia. Każdy z tych informacji został szczegółowo opisany w następujących samouczkach w serii.

- Firma Contoso University używa bazy danych SQL Server Compact do przechowywania danych aplikacji, takich jak uczniowie i nazwiska instruktorów. Baza danych zawiera kombinację danych testowych i danych produkcyjnych, a podczas wdrażania programu w środowisku produkcyjnym należy wykluczyć dane testowe. W dalszej części samouczka przeprowadzisz migrację z SQL Server Compact do SQL Server.
- Aplikacja używa systemu członkostwa ASP.NET, który przechowuje informacje o koncie użytkownika w bazie danych SQL Server Compact. Aplikacja definiuje użytkownika administratora, który ma dostęp do niektórych informacji z ograniczeniami. Należy wdrożyć bazę danych członkostwa bez kont testowych, ale przy użyciu jednego konta administratora.
- Ponieważ baza danych aplikacji i baza danych członkostwa korzystają SQL Server Compact jako aparat bazy danych, należy wdrożyć aparat bazy danych dla dostawcy hostingu, a także same bazy danych.
- Aplikacja korzysta z uniwersalnych dostawców członkostwa ASP.NET, aby system członkostwa mógł przechowywać swoje dane w bazie danych SQL Server Compact. Zestaw zawierający dostawców uniwersalnego członkostwa musi być wdrożony z aplikacją.
- Aplikacja używa Entity Framework 5,0 do uzyskiwania dostępu do danych w bazie danych aplikacji. Zestaw zawierający Entity Framework 5,0 musi być wdrożony razem z aplikacją.
- Aplikacja używa narzędzia do rejestrowania błędów innych firm i raportowania. To narzędzie jest dostępne w zestawie, który musi zostać wdrożony z aplikacją.
- Narzędzie rejestrowania błędów zapisuje informacje o błędach w plikach XML do folderu plików. Musisz się upewnić, że konto, które ASP.NET jest uruchamiane w ramach wdrożonej lokacji, ma uprawnienie do zapisu w tym folderze, a ty musisz wykluczyć ten folder z wdrożenia. (W przeciwnym razie dane dziennika błędów ze środowiska testowego mogą zostać wdrożone w produkcji i/lub pliki dzienników błędów produkcyjnych mogą zostać usunięte).
- Aplikacja zawiera pewne ustawienia, które należy zmienić we wdrożonym pliku *Web. config* w zależności od środowiska docelowego (test lub produkcja), a także inne ustawienia, które należy zmienić w zależności od konfiguracji kompilacji (debugowanie lub wydanie).
- Rozwiązanie programu Visual Studio zawiera projekt biblioteki klas. Należy wdrożyć tylko zestaw, który zostanie wygenerowany przez ten projekt, a nie sam projekt.

W tym pierwszym samouczku w serii pobrano przykładowy projekt programu Visual Studio i przegląd funkcji witryny, które mają wpływ na sposób wdrażania aplikacji. W poniższych samouczkach przygotowano do wdrożenia przez skonfigurowanie niektórych z tych elementów do obsługi automatycznej. Inne osoby, które postanowisz ręcznie.

> [!div class="step-by-step"]
> [Next](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)

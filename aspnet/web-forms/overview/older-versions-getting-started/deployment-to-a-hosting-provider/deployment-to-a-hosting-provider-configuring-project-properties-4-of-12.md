---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Konfigurowanie właściwości projektu — 4 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6e63e75dca3d776fb9a1bd7e420ef48891daac69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625861"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Konfigurowanie właściwości projektu — 4 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

Niektóre opcje wdrażania są konfigurowane we właściwościach projektu, które są przechowywane w pliku projektu (plik *. csproj* lub *. vbproj* ). W większości przypadków wartości domyślne tych ustawień są odpowiednie, ale można użyć interfejsu użytkownika **właściwości projektu** wbudowanego w program Visual Studio, aby korzystać z tych ustawień, jeśli trzeba je zmienić. W tym samouczku zawarto przegląd ustawień wdrożenia we **właściwościach projektu**. Tworzony jest również plik zastępczy, który powoduje wdrożenie pustego folderu.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurowanie ustawień wdrożenia w oknie właściwości projektu

Większość ustawień wpływających na to, co się dzieje podczas wdrażania, jest uwzględnionych w profilu publikowania, jak widać w następujących samouczkach. Kilka ustawień, z którymi należy się zapoznać, znajduje się na kartach **pakowanie/publikowanie** okna **właściwości projektu** . Te ustawienia są określone dla każdej konfiguracji kompilacji — to znaczy, że można mieć różne ustawienia dla kompilacji wydania niż w przypadku kompilacji debugowania.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **ContosoUniversity** , wybierz polecenie **Właściwości**, a następnie wybierz kartę **pakowanie/publikowanie w sieci Web** .

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Gdy okno jest wyświetlane, domyślnie wyświetlane są ustawienia dla każdej konfiguracji kompilacji, która jest obecnie aktywna dla rozwiązania. Jeśli pole **konfiguracji** nie wskazuje **aktywnego (wydanie)** , wybierz pozycję **Zwolnij** w celu wyświetlenia ustawień konfiguracji kompilacji wydania. Wdrażasz kompilacje wydania w środowisku testowym i produkcyjnym.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

W przypadku wybrania opcji **aktywne (wydanie)** **lub** wydawania zobaczysz wartości, które są efektywne podczas wdrażania przy użyciu konfiguracji kompilacji wydania:

- W polu **elementy do wdrożenia** wybrano **tylko pliki, które są konieczne do uruchomienia aplikacji** . Inne opcje to **wszystkie pliki w tym projekcie** lub **wszystkie pliki w tym folderze projektu**. Pozostawienie domyślnego zaznaczenia bez zmian pozwala uniknąć wdrażania plików kodu źródłowego, na przykład. To ustawienie jest przyczyną, dlaczego foldery zawierające SQL Server Compact pliki binarne musiały zostać uwzględnione w projekcie. Aby uzyskać więcej informacji na temat tego ustawienia, zobacz **dlaczego nie wszystkie pliki w folderze Mój projekt są wdrożone?** w artykule [ASP.NET wdrażanie projektu aplikacji sieci Web — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx).
- Wybrano opcję **Wyklucz wygenerowane symbole debugowania** . W przypadku korzystania z tej konfiguracji kompilacji nie będzie można debugować.
- Nie wybrano **plików do wykluczenia z\_aplikacji** . Plik SQL Server Compact dla bazy danych członkostwa znajduje się w tym folderze i trzeba go wdrożyć. W przypadku wdrażania aktualizacji, które nie obejmują zmian w bazie danych, zaznacz to pole wyboru.
- **Przed opublikowaniem** nie wybrano wstępnie skompilowanej aplikacji. W większości scenariuszy nie ma potrzeby wstępnego kompilowania projektów aplikacji sieci Web. Aby uzyskać więcej informacji na temat tej opcji, zobacz [okno dialogowe](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx) [pakowanie/publikowanie w sieci Web, właściwości projektu](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) i zaawansowane ustawienia wstępnej kompilacji.
- Zaznaczono **wszystkie bazy danych skonfigurowane na karcie pakowanie/publikowanie SQL** , ale ta opcja nie ma żadnego efektu, ponieważ nie jest konfigurowana karta **pakowanie/publikowanie SQL** . Ta karta jest dla starszej metody wdrażania bazy danych, która była używana jako jedyną opcją wdrażania SQL Server baz danych. Użyjesz karty **pakiet/publikowanie SQL** w samouczku [migrowanie do SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) .
- Sekcja **ustawień pakietu wdrażania internetowego** nie ma zastosowania, ponieważ w tych samouczkach jest używany przycisk Publikuj w jednym miejscu.

Zmień pole listy rozwijanej **Konfiguracja** na Debuguj, aby wyświetlić ustawienia domyślne dla kompilacji debugowania. Wartości są takie same, z wyjątkiem **wykluczania wygenerowanych symboli debugowania** , aby można było debugować podczas wdrażania kompilacji debugowania.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Upewnienie się, że folder ELMAH zostanie wdrożony

Jak zostało to opisane w poprzednim samouczku, [pakiet NuGet programu ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) zawiera funkcje rejestrowania błędów i raportowania. W aplikacji z uczelnią w firmie Contoso został skonfigurowany do przechowywania szczegółów błędu w folderze o nazwie *ELMAH*:

![Folder ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Wykluczenie określonych plików lub folderów z wdrożenia jest typowym wymaganiem; Innym przykładem jest folder, do którego użytkownicy mogą przekazywać pliki. Nie chcesz, aby pliki dzienników ani pliki przekazane, które zostały utworzone w środowisku deweloperskim, nie zostały wdrożone do środowiska produkcyjnego. A Jeśli wdrażasz aktualizację do środowiska produkcyjnego, nie chcesz, aby proces wdrażania usunął pliki, które istnieją w środowisku produkcyjnym. (W zależności od sposobu ustawienia opcji wdrożenia plik istnieje w lokacji docelowej, ale nie w lokacji źródłowej podczas wdrażania, Web Deploy usuwa go z lokalizacji docelowej).

Jak przedstawiono wcześniej w tym samouczku, opcja **elementy do wdrożenia** na karcie **pakowanie/publikowanie w sieci Web** jest ustawiona na **tylko pliki potrzebne do uruchomienia tej aplikacji**. W związku z tym pliki dzienników tworzone przez program ELMAH w trakcie programowania nie zostaną wdrożone, co ma miejsce. (Do wdrożenia muszą być zawarte w projekcie, a ich Właściwość **Akcja kompilacji** musi być ustawiona na wartość **zawartość**. Aby uzyskać więcej informacji, zobacz **dlaczego nie wszystkie pliki w folderze Mój projekt są wdrożone?** w [ASP.NET wdrażanie projektu aplikacji sieci Web — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx). Jednak Web Deploy nie utworzy folderu w lokacji docelowej, chyba że istnieje co najmniej jeden plik do skopiowania do niego. W związku z tym należy dodać plik *txt* do folderu, aby działał jako symbol zastępczy, aby folder został skopiowany.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *ELMAH* , wybierz polecenie **Dodaj nowy element**i Utwórz plik o nazwie *placeholder. txt*. Umieść w nim następujący tekst: "jest to plik zastępczy, aby upewnić się, że folder zostanie wdrożony". i Zapisz plik. To wszystko, co należy zrobić, aby upewnić się, że program Visual Studio wdroży ten plik i folder, w którym znajduje się w nim Właściwość **Akcja kompilacji** plików *txt* jest domyślnie ustawiona na **zawartość** .

Ukończono wszystkie zadania konfiguracji wdrażania. W następnym samouczku utworzysz witrynę firmy Contoso University w środowisku testowym i przetestujemy ją tam.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)

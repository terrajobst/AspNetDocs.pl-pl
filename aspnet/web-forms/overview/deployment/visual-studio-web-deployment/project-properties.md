---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'ASP.NET Web Deployment przy użyciu programu Visual Studio: właściwości projektu | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: b2811791a897c9166f6222c23dddc6921e5267ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643599"
---
# <a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>ASP.NET Web Deployment przy użyciu programu Visual Studio: właściwości projektu

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

Niektóre opcje wdrażania są konfigurowane we właściwościach projektu, które są przechowywane w pliku projektu (plik *. csproj* lub *. vbproj* ). W większości przypadków wartości domyślne tych ustawień są odpowiednie, ale można użyć interfejsu użytkownika **właściwości projektu** wbudowanego w program Visual Studio, aby korzystać z tych ustawień, jeśli trzeba je zmienić. W tym samouczku zawarto przegląd ustawień wdrożenia we **właściwościach projektu**. Tworzony jest również plik zastępczy, który powoduje wdrożenie pustego folderu.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfigurowanie ustawień wdrożenia w oknie właściwości projektu

Większość ustawień wpływających na to, co się dzieje podczas wdrażania, jest uwzględnionych w profilu publikowania, jak widać w następujących samouczkach. Kilka ustawień, z którymi należy się zapoznać, znajduje się na kartach **pakowanie/publikowanie** okna **właściwości projektu** . Te ustawienia są określone dla każdej konfiguracji kompilacji — to znaczy, że można mieć różne ustawienia dla kompilacji wydania niż w przypadku kompilacji debugowania.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **ContosoUniversity** , wybierz polecenie **Właściwości**, a następnie wybierz kartę **pakowanie/publikowanie w sieci Web** .

![Karta Sieć Web pakowanie/publikowanie](project-properties/_static/image1.png)

Gdy okno jest wyświetlane, domyślnie wyświetlane są ustawienia dla każdej konfiguracji kompilacji, która jest obecnie aktywna dla rozwiązania. Jeśli pole **konfiguracji** nie wskazuje **aktywnego (wydanie)** , wybierz pozycję **Zwolnij** w celu wyświetlenia ustawień konfiguracji kompilacji wydania. Wdrażasz kompilacje wydania w środowisku testowym i produkcyjnym.

![Wybieranie konfiguracji kompilacji wydania](project-properties/_static/image2.png)

W przypadku wybrania opcji **aktywne (wydanie)** **lub** wydawania zobaczysz wartości, które są efektywne podczas wdrażania przy użyciu konfiguracji kompilacji wydania:

- W polu **elementy do wdrożenia** wybrano **tylko pliki, które są konieczne do uruchomienia aplikacji** . Inne opcje to **wszystkie pliki w tym projekcie** lub **wszystkie pliki w tym folderze projektu**. Pozostawienie domyślnego zaznaczenia bez zmian pozwala uniknąć wdrażania plików kodu źródłowego, na przykład. To ustawienie jest przyczyną, dlaczego foldery zawierające SQL Server Compact pliki binarne musiały zostać uwzględnione w projekcie. Aby uzyskać więcej informacji na temat tego ustawienia, zobacz **dlaczego nie wszystkie pliki w folderze Mój projekt są wdrożone?** w artykule [ASP.NET wdrażanie projektu aplikacji sieci Web — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx).
- Wybrano opcję **Wyklucz wygenerowane symbole debugowania** . W przypadku korzystania z tej konfiguracji kompilacji nie będzie można debugować.
- Zaznaczono **wszystkie bazy danych skonfigurowane na karcie pakowanie/publikowanie SQL** . Określa, czy program Visual Studio będzie wdrażał bazy danych oraz pliki. Mimo że etykieta pola wyboru zawiera wyłącznie kartę **pakiet/publikacja SQL** , wyczyszczenie tego pola wyboru spowoduje również wyłączenie wdrożenia bazy danych skonfigurowanego w profilu publikowania. Zostanie to zrobione później, więc pole wyboru musi pozostać zaznaczone. Karta **pakowanie/publikowanie SQL** jest używana w przypadku starszej metody publikowania bazy danych, która nie jest używana w tych samouczkach.
- Sekcja **ustawień pakietu wdrażania internetowego** nie ma zastosowania, ponieważ w tych samouczkach jest używany przycisk Publikuj w jednym miejscu.

Zmień pole listy rozwijanej **Konfiguracja** na Debuguj, aby wyświetlić ustawienia domyślne dla kompilacji debugowania. Wartości są takie same, z wyjątkiem **wykluczania wygenerowanych symboli debugowania** , aby można było debugować podczas wdrażania kompilacji debugowania.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Upewnij się, że folder ELMAH zostanie wdrożony

Jak zostało to opisane w poprzednim samouczku, [pakiet NuGet programu ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) zawiera funkcje rejestrowania błędów i raportowania. W aplikacji z uczelnią w firmie Contoso został skonfigurowany do przechowywania szczegółów błędu w folderze o nazwie *ELMAH*:

![Folder ELMAH](project-properties/_static/image3.png)

Wykluczenie określonych plików lub folderów z wdrożenia jest typowym wymaganiem; Innym przykładem jest folder, do którego użytkownicy mogą przekazywać pliki. Nie chcesz, aby pliki dzienników ani pliki przekazane, które zostały utworzone w środowisku deweloperskim, nie zostały wdrożone do środowiska produkcyjnego. A Jeśli wdrażasz aktualizację do środowiska produkcyjnego, nie chcesz, aby proces wdrażania usunął pliki, które istnieją w środowisku produkcyjnym. (W zależności od sposobu ustawienia opcji wdrożenia plik istnieje w lokacji docelowej, ale nie w lokacji źródłowej podczas wdrażania, Web Deploy usuwa go z lokalizacji docelowej).

Jak przedstawiono wcześniej w tym samouczku, opcja **elementy do wdrożenia** na karcie **pakowanie/publikowanie w sieci Web** jest ustawiona na **tylko pliki potrzebne do uruchomienia tej aplikacji**. W związku z tym pliki dzienników tworzone przez program ELMAH w trakcie programowania nie zostaną wdrożone, co ma miejsce. (Do wdrożenia muszą być zawarte w projekcie, a ich Właściwość **Akcja kompilacji** musi być ustawiona na wartość **zawartość**. Aby uzyskać więcej informacji, zobacz **dlaczego nie wszystkie pliki w folderze Mój projekt są wdrożone?** w [ASP.NET wdrażanie projektu aplikacji sieci Web — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx). Jednak Web Deploy nie utworzy folderu w lokacji docelowej, chyba że istnieje co najmniej jeden plik do skopiowania do niego. W związku z tym należy dodać plik *txt* do folderu, aby działał jako symbol zastępczy, aby folder został skopiowany.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *ELMAH* , wybierz polecenie **Dodaj nowy element**i Utwórz plik tekstowy o nazwie *placeholder. txt*. Umieść w nim następujący tekst: "jest to plik zastępczy, aby upewnić się, że folder zostanie wdrożony". i Zapisz plik. To wszystko, co należy zrobić, aby upewnić się, że program Visual Studio wdroży ten plik i folder, w którym znajduje się w nim Właściwość **Akcja kompilacji** plików *txt* jest domyślnie ustawiona na **zawartość** .

## <a name="summary"></a>Podsumowanie

Ukończono wszystkie zadania konfiguracji wdrażania. W następnym samouczku utworzysz witrynę firmy Contoso University w środowisku testowym i przetestujemy ją tam.

> [!div class="step-by-step"]
> [Poprzednie](web-config-transformations.md)
> [dalej](deploying-to-iis.md)

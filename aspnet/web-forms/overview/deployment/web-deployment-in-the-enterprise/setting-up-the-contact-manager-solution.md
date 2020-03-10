---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Konfigurowanie rozwiązania Contact Manager | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób pobierania i konfigurowania rozwiązania Contact Manager do uruchamiania lokalnego na stacji roboczej dewelopera.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629858"
---
# <a name="setting-up-the-contact-manager-solution"></a>Konfigurowanie rozwiązania Contact Manager

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób pobierania i konfigurowania rozwiązania Contact Manager do uruchamiania lokalnego na stacji roboczej dewelopera.

## <a name="system-requirements"></a>Wymagania systemu

Aby uruchomić rozwiązanie Contact Manager lokalnie i wykonać inne zadania opisane w tym samouczku, musisz zainstalować to oprogramowanie na stacji roboczej dewelopera:

- Visual Studio 2010 z dodatkiem Service Pack 1, Premium lub Ultimate
- Internet Information Services (IIS) 7,5 Express
- SQL Server Express 2008 R2
- Narzędzie do wdrażania w sieci Web usług IIS (Web Deploy) 2,1 lub nowsze
- ASP.NET 4.0
- ASP.NET MVC 3
- Program .NET Framework 4
- .NET Framework 3.5 SP1

Z wyjątkiem programu Visual Studio 2010 można pobrać i zainstalować najnowsze wersje wszystkich tych produktów i składników za pomocą [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Pobierz i Wyodrębnij rozwiązanie

Możesz pobrać przykładową aplikację Contact Manager z galerii kodu MSDN [tutaj](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Konfigurowanie i uruchamianie rozwiązania

Aby skonfigurować i uruchomić rozwiązanie Contact Manager na komputerze lokalnym, należy wykonać następujące czynności wysokiego poziomu:

1. Jeśli jeszcze tego nie zrobiono, utwórz lokalną bazę danych usług aplikacji ASP.NET z włączoną funkcją członkostwa i zarządzania rolami.
2. Edytuj parametry połączenia w plikach *Web. config* , aby wskazywały lokalne wystąpienie SQL Server Express.
3. Uruchom rozwiązanie z programu Visual Studio 2010.

Pozostała część tej sekcji zawiera więcej wskazówek na temat wykonywania poszczególnych zadań.

**Aby utworzyć bazę danych usług aplikacji**

1. Otwórz wiersz polecenia programu Visual Studio 2010. W tym celu w menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft Visual Studio 2010**, kliknij pozycję **Visual Studio Tools**, a następnie kliknij pozycję **wiersz polecenia programu Visual Studio (2010)** .
2. W wierszu polecenia wpisz następujące polecenie, a następnie naciśnij klawisz ENTER:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Użyj przełącznika **– C** , aby określić parametry połączenia dla serwera bazy danych.
    2. Użyj przełącznika **– A** , aby określić funkcje usług aplikacji, które chcesz dodać do bazy danych. W takim przypadku **m** wskazuje, że chcesz dodać obsługę dostawcy członkostwa i **r** wskazuje, że chcesz dodać obsługę dla menedżera ról.
    3. Użyj przełącznika **– d** , aby określić nazwę bazy danych usług aplikacji. Pominięcie tego przełącznika spowoduje utworzenie bazy danych o nazwie domyślnej **aspnetdb**.
3. Po pomyślnym utworzeniu bazy danych wiersz polecenia wyświetli potwierdzenie.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Aby uzyskać więcej informacji na temat narzędzia regsql ASPNET\_, zobacz [ASP.NET SQL Server Registration Tool (aspnet\_regsql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).

Następnym krokiem jest upewnienie się, że parametry połączenia w rozwiązaniu Contact Manager wskazują lokalne wystąpienie SQL Server Express.

**Aby zaktualizować parametry połączenia**

1. Otwórz rozwiązanie Contact Manager w programie Visual Studio 2010.
2. W oknie **Eksplorator rozwiązań** rozwiń projekt **ContactManager. MVC** , a następnie kliknij dwukrotnie węzeł **Web. config** .

    > [!NOTE]
    > Projekt ContactManager. MVC zawiera dwa pliki *Web. config* . Należy edytować plik poziomu projektu.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. W elemencie **connectionStrings** Sprawdź, czy parametry połączenia o nazwie **ApplicationServices** wskazują lokalną bazę danych usług aplikacji ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. W oknie **Eksplorator rozwiązań** rozwiń projekt **ContactManager. Service** , a następnie kliknij dwukrotnie węzeł **Web. config** .

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. W elemencie **connectionStrings** w parametrach połączenia o nazwie **ContactManagerContext**upewnij się, że właściwość **Źródło danych** jest ustawiona na lokalne wystąpienie SQL Server Express. Nie trzeba zmieniać żadnych innych parametrów połączenia.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Zapisz wszystkie otwarte pliki.

Teraz możesz być gotowy do uruchomienia rozwiązania Contact Manager na komputerze lokalnym.

> [!NOTE]
> Jeśli wykonasz te kroki bez wcześniejszego tworzenia bazy danych usług aplikacji, ASP.NET utworzy bazę danych przy pierwszej próbie utworzenia użytkownika. Jednak ręczne tworzenie bazy danych zapewnia znacznie większą kontrolę nad zestawem funkcji usług aplikacji, który ma być obsługiwany.

**Aby uruchomić rozwiązanie Contact Manager**

1. W programie Visual Studio 2010 naciśnij klawisz F5.
2. Program Internet Explorer uruchamia i żąda adresu URL aplikacji Contact Manager ASP.NET MVC 3. Domyślnie aplikacja wyświetla stronę **wszystkie kontakty** .

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Dodaj kilka kontaktów, a następnie sprawdź, czy aplikacja działa zgodnie z oczekiwaniami.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Przejdź do `http://localhost:50114/Account/Register` (Dostosuj adres URL, jeśli aplikacja jest obsługiwana na innym porcie). Dodaj nazwę użytkownika, adres e-mail i hasło, a następnie sprawdź, czy możliwe jest pomyślne zarejestrowanie konta.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Przejdź do `http://localhost:50114/Account/LogOn` (Dostosuj adres URL, jeśli aplikacja jest obsługiwana na innym porcie). Sprawdź, czy możesz się zalogować, używając właśnie utworzonego konta.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Zamknij program Internet Explorer, aby zatrzymać debugowanie.

## <a name="conclusion"></a>Podsumowanie

W tym momencie rozwiązanie Contact Manager powinno być w pełni skonfigurowane do uruchamiania na komputerze lokalnym. Możesz użyć rozwiązania jako odniesienia podczas pracy w innych tematach w tym samouczku.

W następnym temacie, [opisującym plik projektu](understanding-the-project-file.md), wyjaśniono, jak można użyć plików projektu niestandardowych Microsoft Build Engine (MSBuild) w rozwiązaniu Contact Manager do sterowania procesem wdrażania.

> [!div class="step-by-step"]
> [Poprzednie](the-contact-manager-solution.md)
> [dalej](understanding-the-project-file.md)

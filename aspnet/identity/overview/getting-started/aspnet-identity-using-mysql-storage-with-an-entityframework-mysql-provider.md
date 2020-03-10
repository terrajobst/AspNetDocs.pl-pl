---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: korzystanie z magazynu MySQL za pomocą dostawcy EntityFramework MySQLC#() — ASP.NET 4. x'
author: maumar
description: W tym samouczku pokazano, jak zastąpić domyślny mechanizm przechowywania danych dla ASP.NET Identity z EntityFramework (dostawca klienta SQL) za pomocą programu MySQL provid...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: e89ed139657c5ce9ddcc56879946c62038919483
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584008"
---
# <a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: używanie usługi MySQL Storage z dostawcą MySQL programu EntityFramework (C#)

[Maurycy Markowski](https://github.com/maumar), [Raquel Soares de Almeida](https://github.com/raquelsa), [Robert McMurraya](https://github.com/rmcmurray)

> W tym samouczku pokazano, jak zastąpić domyślny mechanizm przechowywania danych dla [**ASP.NET Identity**](introduction-to-aspnet-identity.md) z EntityFramework (dostawca klienta SQL) za pomocą dostawcy programu MySQL.

Następujące tematy zostaną omówione w tym samouczku:

- Tworzenie bazy danych MySQL na platformie Azure
- Tworzenie aplikacji MVC przy użyciu Visual Studio 2013 szablonu MVC
- Konfigurowanie EntityFramework do pracy z dostawcą bazy danych MySQL
- Uruchamianie aplikacji w celu sprawdzenia wyników

Na końcu tego samouczka będziesz mieć aplikację MVC z magazynem ASP.NET Identity, który korzysta z bazy danych MySQL hostowanej na platformie Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Tworzenie wystąpienia bazy danych MySQL na platformie Azure

1. Zaloguj się do [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Kliknij pozycję **Nowy** w dolnej części strony, a następnie wybierz pozycję **sklep**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. W kreatorze **Wybierz i dodatek** wybierz pozycję **baza danych MySQL ClearDB**, a następnie kliknij strzałkę **dalej** w dolnej części ramki:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Zachowaj domyślny plan **bezpłatny** , Zmień **nazwę** na **IdentityMySQLDatabase**, zaznacz region, który znajduje się najbliżej ty, a następnie **kliknij strzałkę w dół w dolnej** części ramki:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Kliknij znacznik wyboru **zakupu** , aby zakończyć tworzenie bazy danych.

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Po utworzeniu bazy danych można nią zarządzać z poziomu karty **Dodatki** w portalu zarządzania. Aby pobrać informacje o połączeniu dla bazy danych, kliknij pozycję **Informacje o połączeniu** w dolnej części strony:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Skopiuj parametry połączenia, klikając przycisk Kopiuj przez pole **CONNECTIONSTRING** i zapisując je; te informacje będą używane w dalszej części tego samouczka dla aplikacji MVC:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Tworzenie projektu aplikacji MVC

Aby wykonać kroki opisane w tej sekcji samouczka, należy najpierw zainstalować [Visual Studio Express 2013 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Po zainstalowaniu programu Visual Studio wykonaj następujące kroki, aby utworzyć nowy projekt aplikacji MVC:

1. Otwórz program Visual Studio 2103.
2. Na stronie **startowej** kliknij pozycję **Nowy projekt** lub kliknij menu **plik** , a następnie pozycję **Nowy projekt**:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Gdy zostanie wyświetlone okno dialogowe **Nowy projekt** , rozwiń pozycję **Wizualizacja C#**  na liście szablonów, a następnie kliknij pozycję **Sieć Web**i wybierz pozycję **aplikacja sieci Web ASP.NET**. Nazwij projekt **IdentityMySQLDemo** , a następnie kliknij przycisk **OK**:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. W oknie dialogowym **Nowy projekt ASP.NET** wybierz opcję **MVC** templatewith opcje domyślne; spowoduje to skonfigurowanie **poszczególnych kont użytkowników** jako metody uwierzytelniania. Kliknij przycisk **OK**:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Konfigurowanie EntityFramework do pracy z bazą danych MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aktualizowanie zestawu Entity Framework dla projektu

Aplikacja MVC, która została utworzona na podstawie szablonu Visual Studio 2013, zawiera odwołanie do pakietu [6.0.0 EntityFramework](http://www.nuget.org/packages/EntityFramework) , ale do tego zestawu zostały zaktualizowane, ponieważ jego wersja zawiera znaczące ulepszenia wydajności. Aby móc używać najnowszych aktualizacji w aplikacji, wykonaj następujące kroki.

1. Otwórz projekt MVC w programie Visual Studio.
2. Kliknij pozycję **Narzędzia**, a następnie kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. W dolnej części programu Visual Studio zostanie wyświetlona **konsola Menedżera pakietów** . Wpisz &quot;**Update-Package EntityFramework**&quot; i naciśnij klawisz ENTER:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Zainstaluj dostawcę MySQL dla usługi EntityFramework

Aby program EntityFramework mógł nawiązać połączenie z bazą danych MySQL, należy zainstalować dostawcę programu MySQL. Aby to zrobić, Otwórz **konsolę Menedżera pakietów** i wpisz &quot;**install-package MySQL. Data. Entity-pre**&quot;, a następnie naciśnij klawisz ENTER.

> [!NOTE]
> Jest to wersja wstępna zestawu i może zawierać błędy. Nie należy używać wersji wstępnej dostawcy w środowisku produkcyjnym.

[Kliknij poniższy obraz, aby go rozwinąć.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Wprowadzanie zmian w konfiguracji projektu do pliku Web. config aplikacji

W tej sekcji skonfigurujesz Entity Framework tak, aby korzystał z zainstalowanego dostawcy MySQL, zarejestrować fabrykę dostawców MySQL i dodać parametry połączenia z platformy Azure.

> [!NOTE]
> Poniższe przykłady zawierają określoną wersję zestawu dla programu MySql. Data. dll. Jeśli wersja zestawu ulegnie zmianie, należy zmodyfikować odpowiednie ustawienia konfiguracji w poprawnej wersji.

1. Otwórz plik Web. config dla projektu w Visual Studio 2013.
2. Znajdź następujące ustawienia konfiguracji, które definiują domyślnego dostawcę i fabrykę baz danych dla Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Zamień te ustawienia konfiguracji na następujące, które spowodują skonfigurowanie Entity Framework do korzystania z dostawcy MySQL:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Znajdź sekcję &lt;connectionStrings&gt; i Zastąp ją następującym kodem, który określi parametry połączenia dla bazy danych MySQL hostowanej na platformie Azure (Pamiętaj, że wartość ProviderName została również zmieniona z oryginału):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Dodawanie niestandardowego kontekstu MigrationHistory

Entity Framework Code First używa tabeli **MigrationHistory** do śledzenia zmian modelu i zapewnienia spójności między schematem bazy danych i schematem koncepcyjnym. Jednak ta tabela nie działa domyślnie dla programu MySQL, ponieważ klucz podstawowy jest zbyt duży. Aby naprawić tę sytuację, należy zmniejszyć rozmiar klucza dla tej tabeli. Aby to zrobić, wykonaj następujące kroki:

1. Informacje o schemacie dla tej tabeli są przechwytywane w **HistoryContext**, które mogą być modyfikowane jako wszystkie inne **DbContext**. Aby to zrobić, Dodaj nowy plik klasy o nazwie **MySqlHistoryContext.cs** do projektu i Zastąp jego zawartość następującym kodem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Następnie należy skonfigurować Entity Framework, aby używały zmodyfikowanego **HistoryContext**zamiast domyślnego. Można to zrobić, korzystając z funkcji konfiguracji opartych na kodzie. Aby to zrobić, Dodaj nowy plik klasy o nazwie **MySqlConfiguration.cs** do projektu i Zastąp jego zawartość:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Tworzenie niestandardowego inicjatora EntityFramework dla ApplicationDbContext

Dostawca MySQL proponowany w tym samouczku obecnie nie obsługuje migracji Entity Framework, więc należy użyć inicjatorów modelu w celu nawiązania połączenia z bazą danych. Ponieważ ten samouczek używa wystąpienia MySQL na platformie Azure, konieczne będzie utworzenie niestandardowego inicjatora Entity Framework.

> [!NOTE]
> Ten krok nie jest wymagany, jeśli łączysz się z wystąpieniem SQL Server na platformie Azure lub używasz bazy danych hostowanej lokalnie.

Aby utworzyć niestandardowy inicjator Entity Framework dla programu MySQL, wykonaj następujące czynności:

1. Dodaj nowy plik klasy o nazwie **MySqlInitializer.cs** do projektu i Zastąp jego zawartość następującym kodem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Otwórz plik **IdentityModels.cs** dla projektu, który znajduje się w katalogu **models** , i Zastąp jego zawartość następującym:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Uruchamianie aplikacji i weryfikowanie bazy danych

Po wykonaniu kroków opisanych w poprzednich sekcjach należy przetestować bazę danych. Aby to zrobić, wykonaj następujące kroki:

1. Naciśnij **kombinację klawiszy CTRL + F5** , aby skompilować i uruchomić aplikację sieci Web.
2. Kliknij kartę **Rejestr** w górnej części strony:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij pozycję **zarejestruj**:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. W tym momencie tabele ASP.NET Identity są tworzone w bazie danych MySQL, a użytkownik jest zarejestrowany i zalogowany do aplikacji:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalowanie narzędzia MySQL Workbench do weryfikowania danych

1. Zainstaluj narzędzie **MySQL Workbench** na [stronie pobierania programu MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. W Kreatorze instalacji: **wybór funkcji** wybierz pozycję **MySQL Workbench** w sekcji **aplikacje** .
3. Uruchom aplikację i Dodaj nowe połączenie, używając danych parametrów połączenia z bazy danych Azure MySQL utworzonej w BEGGING tego samouczka.
4. Po ustanowieniu połączenia Sprawdź tabele **ASP.NET Identity** utworzone w **IdentityMySQLDatabase.**
5. Zobaczysz, że wszystkie ASP.NET Identity wymagane tabele są tworzone, jak pokazano na poniższej ilustracji:

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Sprawdź tabelę **AspnetUsers** , aby sprawdzić obecność wpisów podczas rejestrowania nowych użytkowników.

   [Kliknij poniższy obraz, aby go rozwinąć. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)

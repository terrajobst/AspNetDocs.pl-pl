---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji bazy danych | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636831"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji bazy danych

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku wprowadzisz zmiany w bazie danych i powiązane zmiany kodu, testujesz zmiany w programie Visual Studio, a następnie wdrażasz aktualizację w środowiskach testowych, przejściowych i produkcyjnych.

W tym samouczku pokazano, jak zaktualizować bazę danych zarządzaną przez program Migracje Code First, a później pokazano, jak zaktualizować bazę danych za pomocą dostawcy dbDacFx.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Wdrażanie aktualizacji bazy danych za pomocą Migracje Code First

W tej sekcji dodasz kolumnę Data urodzenia do `Person` klasy bazowej dla jednostek `Student` i `Instructor`. Następnie zaktualizujesz stronę wyświetlającą dane instruktora, aby wyświetlić nową kolumnę. Na koniec należy wdrożyć zmiany w testach, przemieszczaniu i produkcji.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Dodawanie kolumny do tabeli w bazie danych aplikacji

1. W projekcie *ContosoUniversity. dal* Otwórz *Person.cs* i Dodaj następującą właściwość na końcu klasy `Person` (po niej powinny znajdować się dwa zamykające nawiasy klamrowe):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Następnie zaktualizuj metodę `Seed`, aby zapewnić wartość nowej kolumny. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następującym blokiem kodu, który zawiera informacje o dacie urodzenia:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** . Upewnij się, że ContosoUniversity. DAL jest nadal wybrane jako **Projekt domyślny**.
3. W oknie **konsola Menedżera pakietów** wybierz pozycję **ContosoUniversity. dal** jako **Projekt domyślny**, a następnie wprowadź następujące polecenie:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę `DbMigration`, a w metodzie `Up` można zobaczyć kod, który tworzy nową kolumnę. Metoda `Up` tworzy kolumnę podczas implementowania zmiany, a metoda `Down` usuwa kolumnę podczas wycofywania zmiany.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w oknie **konsola Menedżera pakietów** (Upewnij się, że projekt CONTOSOUNIVERSITY. dal jest nadal zaznaczony):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework uruchamia metodę `Up`, a następnie uruchamia metodę `Seed`.

### <a name="display-the-new-column-in-the-instructors-page"></a>Wyświetlanie nowej kolumny na stronie instruktorów

1. W projekcie ContosoUniversity Otwórz program *instruktors. aspx* i Dodaj nowe pole szablonu, aby wyświetlić datę urodzenia. Dodaj ją między tymi elementami dla daty zatrudnienia i przypisania pakietu Office:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Jeśli wcięcia kodu nie są zsynchronizowane, możesz nacisnąć klawisze CTRL-K, a następnie CTRL-D, aby automatycznie ponownie sformatować plik).
2. Uruchom aplikację, a następnie kliknij link **Instruktorzy** .

    Po załadowaniu strony zobaczysz, że ma ona nowe pole Data urodzenia.

    ![Strona instruktorów z dataurodzeniem](deploying-a-database-update/_static/image2.png)
3. Zamknij przeglądarkę.

### <a name="deploy-the-database-update"></a>Wdróż aktualizację bazy danych

1. W **Eksplorator rozwiązań** wybierz projekt ContosoUniversity.
2. Na pasku narzędzi **kliknij przycisk Publikuj w sieci Web** , kliknij profil publikacji **test** , a następnie kliknij pozycję **Publikuj sieć Web**. (Jeśli pasek narzędzi jest wyłączony, wybierz projekt ContosoUniversity w **Eksplorator rozwiązań**).

    Program Visual Studio wdroży zaktualizowaną aplikację, a w przeglądarce zostanie otwarta strona główna.
3. Uruchom stronę **instruktorów** , aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

    Gdy aplikacja próbuje uzyskać dostęp do bazy danych na tej stronie, Code First aktualizuje schemat bazy danych i uruchamia metodę `Seed`. Gdy zostanie wyświetlona strona, zobaczysz kolumnę oczekiwanej **daty urodzenia** z datami w nim.
4. W **sieci Web kliknij przycisk Publikuj** na pasku narzędzi, kliknij profil publikowania **przemieszczania** , a następnie kliknij przycisk **Publikuj sieć Web**.
5. Uruchom stronę **instruktorów** w obszarze przygotowanie, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.
6. W **sieci Web kliknij przycisk Publikuj** na pasku narzędzi, kliknij profil publikowania **produkcyjnego** , a następnie kliknij przycisk **Publikuj sieć Web**.
7. Uruchom stronę **Instruktorzy** w środowisku produkcyjnym, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

    W przypadku rzeczywistej aktualizacji aplikacji produkcyjnej, która obejmuje zmianę bazy danych, przeważnie przeprowadzisz aplikację w tryb offline podczas wdrażania przy użyciu *aplikacji\_offline. htm*, jak zostało to opisane w poprzednim samouczku.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Wdrażanie aktualizacji bazy danych za pomocą dostawcy dbDacFx

W tej sekcji dodasz kolumnę *komentarzy* do tabeli *User* w bazie danych członkostwa i utworzysz stronę, która umożliwia wyświetlanie i edytowanie komentarzy dla każdego użytkownika. Następnie wdrażasz zmiany w testach, przemieszczaniu i produkcji.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Dodawanie kolumny do tabeli w bazie danych członkostwa

1. W programie Visual Studio Otwórz **Eksplorator obiektów SQL Server**.
2. Rozwiń **(LocalDB) \v11.0**, rozwiń węzeł **bazy danych**, rozwiń pozycję **ASPNET-ContosoUniversity** (nie **ASPNET-ContosoUniversity-prod**), a następnie rozwiń węzeł **tabele**.

    Jeśli nie widzisz **(LocalDB) \v11.0** w węźle **SQL Server** , kliknij prawym przyciskiem myszy węzeł **SQL Server** i kliknij polecenie **Dodaj SQL Server**. W oknie dialogowym **łączenie z serwerem** wprowadź *(LocalDB) \V11.0* jako **nazwę serwera**, a następnie kliknij przycisk **Połącz**.

    Jeśli nie widzisz **ASPNET-ContosoUniversity**, Uruchom projekt i zaloguj się przy użyciu poświadczeń *administratora* (hasło to *devpwd*), a następnie Odśwież okno **Eksplorator obiektów SQL Server** .
3. Kliknij prawym przyciskiem myszy tabelę **Użytkownicy** , a następnie kliknij pozycję **Projektant widoków**.

    ![Projektant widoków SSOX](deploying-a-database-update/_static/image3.png)
4. W projektancie Dodaj kolumnę *Komentarze* i ustaw ją jako *nvarchar (128)* i nullable, a następnie kliknij przycisk **Aktualizuj**.

    ![Dodawanie kolumny komentarzy](deploying-a-database-update/_static/image4.png)
5. W oknie **Podgląd aktualizacji bazy danych** kliknij pozycję **Aktualizuj bazę danych**.

    ![Podgląd aktualizacji bazy danych](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Utwórz stronę, aby wyświetlić i edytować nową kolumnę

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **konta** w projekcie ContosoUniversity, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **nowy element**.
2. Utwórz nowy **formularz sieci Web przy użyciu strony wzorcowej** i nadaj mu nazwę *UserInfo. aspx*. Zaakceptuj domyślny plik *site. Master* jako stronę wzorcową.
3. Skopiuj następujące znaczniki do `MainContent` elementu `Content` (ostatni z 3 elementów `Content`):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Kliknij prawym przyciskiem myszy stronę *UserInfo. aspx* , a następnie kliknij pozycję **Wyświetl w przeglądarce**.
5. Zaloguj się przy użyciu poświadczeń użytkownika *administratora* (hasło to *devpwd*) i Dodaj do użytkownika pewne komentarze, aby sprawdzić, czy strona działa poprawnie.

    ![Strona UserInfo](deploying-a-database-update/_static/image6.png)
6. Zamknij przeglądarkę.

## <a name="deploy-the-database-update"></a>Wdróż aktualizację bazy danych

Aby wdrożyć program przy użyciu dostawcy dbDacFx, wystarczy wybrać opcję **Aktualizuj bazę danych** w profilu publikowania. Jednak w przypadku wstępnego wdrożenia przy użyciu tej opcji można również skonfigurować kilka dodatkowych skryptów SQL: te są nadal w profilu i należy uniemożliwić ich ponowne uruchomienie.

1. Otwórz kreatora **publikacji w sieci Web** , klikając prawym przyciskiem myszy projekt ContosoUniversity i klikając polecenie **Publikuj**.
2. Wybierz profil **testu** .
3. Kliknij kartę **Ustawienia** .
4. W obszarze **DefaultConnection**wybierz pozycję **Aktualizuj bazę danych**.
5. Wyłącz dodatkowe skrypty skonfigurowane do uruchamiania w ramach początkowego wdrożenia:

    1. Kliknij pozycję **Konfiguruj aktualizacje bazy danych**.
    2. W oknie dialogowym **Konfiguruj aktualizacje bazy danych** wyczyść pola wyboru obok pozycji *Grant. SQL* i *ASPNET-Data-dev. SQL*.
    3. Kliknij przycisk **Zamknij**.
6. Kliknij kartę **Podgląd** .
7. W obszarze **bazy danych** i po prawej stronie **DefaultConnection**kliknij link **Podgląd bazy danych** .

    ![Podgląd bazy danych](deploying-a-database-update/_static/image7.png)

    W oknie podglądu zostanie wyświetlony skrypt, który zostanie uruchomiony w docelowej bazie danych, aby uczynić schemat bazy danych zgodny ze schematem źródłowej bazy danych. Skrypt zawiera polecenie ALTER TABLE, które dodaje nową kolumnę.
8. Zamknij okno dialogowe **Podgląd bazy danych** , a następnie kliknij przycisk **Publikuj**.

    Program Visual Studio wdroży zaktualizowaną aplikację, a w przeglądarce zostanie otwarta strona główna.
9. Uruchom stronę UserInfo (Dodaj *konto/UserInfo. aspx* do adresu URL strony głównej), aby sprawdzić, czy aktualizacja została pomyślnie wdrożona. Musisz się zalogować, wprowadzając ciąg *admin* i *devpwd*.

    Dane w tabelach nie są wdrażane domyślnie, a skrypt wdrażania danych nie został skonfigurowany do uruchamiania, dlatego nie można znaleźć komentarza dodanego podczas opracowywania. Możesz teraz dodać nowy komentarz w obszarze przygotowywanie, aby sprawdzić, czy zmiana została wdrożona w bazie danych, a strona działa poprawnie.
10. Postępuj zgodnie z tą samą procedurą, aby wdrożyć do etapów przejściowych i produkcyjnych.

    Nie zapomnij wyłączyć dodatkowych skryptów. Jedyną różnicą w porównaniu z profilem testowym jest wyłączenie tylko jednego skryptu w profilach przejściowych i produkcyjnych, ponieważ zostały one skonfigurowane do uruchamiania tylko *ASPNET-prod-Data. SQL*.

    Poświadczenia do przemieszczania i produkcji to admin i prodpwd.

    W przypadku rzeczywistej aktualizacji aplikacji produkcyjnej, która obejmuje zmianę bazy danych, można również przełączać aplikację w tryb offline podczas wdrażania, przekazując *aplikację\_offline. htm* przed opublikowaniem i usunięciem jej później, jak pokazano w [poprzednim samouczku](deploying-a-code-update.md).

## <a name="summary"></a>Podsumowanie

Wdrożono aktualizację aplikacji, która obejmowała zmianę bazy danych przy użyciu zarówno Migracje Code First, jak i dostawcy dbDacFx.

![Strona instruktorów z dataurodzeniem](deploying-a-database-update/_static/image8.png)

![Strona UserInfo](deploying-a-database-update/_static/image9.png)

W następnym samouczku pokazano, jak wykonywać wdrożenia przy użyciu wiersza polecenia.

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-code-update.md)
> [dalej](command-line-deployment.md)

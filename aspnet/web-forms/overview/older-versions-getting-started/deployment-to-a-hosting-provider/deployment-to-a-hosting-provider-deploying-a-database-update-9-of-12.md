---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji bazy danych 9 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3385e1019d9e7a9263fd9513c39b25eb85febbb5
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582014"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji bazy danych 9 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku wprowadzisz zmiany w bazie danych i powiązane zmiany kodu, testujesz zmiany w programie Visual Studio, a następnie wdrażasz aktualizację zarówno w środowisku testowym, jak i produkcyjnym.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Dodawanie nowej kolumny do tabeli

W tej sekcji dodasz kolumnę Data urodzenia do `Person` klasy bazowej dla jednostek `Student` i `Instructor`. Następnie zaktualizujesz stronę wyświetlającą dane instruktora, aby wyświetlić nową kolumnę.

W projekcie *ContosoUniversity. dal* Otwórz *Person.cs* i Dodaj następującą właściwość na końcu klasy `Person` (po niej powinny znajdować się dwa zamykające nawiasy klamrowe):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Następnie zaktualizuj metodę inicjatora, aby zapewnić wartość nowej kolumny. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następującym blokiem kodu, który zawiera informacje o dacie urodzenia:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

W projekcie ContosoUniversity Otwórz program *instruktors. aspx* i Dodaj nowe pole szablonu, aby wyświetlić datę urodzenia. Dodaj ją między tymi elementami dla daty zatrudnienia i przypisania pakietu Office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Jeśli wcięcia kodu nie są zsynchronizowane, możesz nacisnąć klawisze CTRL-K, a następnie CTRL-D, aby automatycznie ponownie sformatować plik).

Skompiluj rozwiązanie, a następnie otwórz okno **konsoli Menedżera pakietów** . Upewnij się, że ContosoUniversity. DAL jest nadal wybrane jako **Projekt domyślny**.

W oknie **konsola Menedżera pakietów** wybierz pozycję **ContosoUniversity. dal** jako **Projekt domyślny**, a następnie wprowadź następujące polecenie:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Po zakończeniu tego polecenia program Visual Studio otwiera plik klasy, który definiuje nową klasę `DbMigration`, a w metodzie `Up` można zobaczyć kod, który tworzy nową kolumnę.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w oknie **konsola Menedżera pakietów** (Upewnij się, że projekt CONTOSOUNIVERSITY. dal jest nadal zaznaczony):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Po zakończeniu wykonywania polecenia Uruchom aplikację i wybierz stronę instruktorzy. Po załadowaniu strony zobaczysz, że ma ona nowe pole Data urodzenia.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Wdrażanie aktualizacji bazy danych w środowisku testowym

W **Eksplorator rozwiązań** wybierz projekt ContosoUniversity.

Na pasku narzędzi **kliknij przycisk Publikuj w sieci Web** , wybierz profil publikowania **testowego** , a następnie kliknij pozycję **Publikuj sieć Web**. (Jeśli pasek narzędzi jest wyłączony, wybierz projekt ContosoUniversity w **Eksplorator rozwiązań**).

Program Visual Studio wdroży zaktualizowaną aplikację, a w przeglądarce zostanie otwarta strona główna. Uruchom stronę instruktorów, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona. Gdy aplikacja próbuje uzyskać dostęp do bazy danych na tej stronie, Code First aktualizuje schemat bazy danych i uruchamia metodę `Seed`. Gdy zostanie wyświetlona strona, zobaczysz kolumnę oczekiwanej **daty urodzenia** z datami w nim.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Wdrażanie aktualizacji bazy danych w środowisku produkcyjnym

Teraz można wdrażać w środowisku produkcyjnym. Jedyną różnicą jest to, że użyjesz *aplikacji\_w trybie offline. htm* , aby uniemożliwić użytkownikom uzyskiwanie dostępu do witryny i w ten sposób aktualizować bazę danych podczas wdrażania zmian. W przypadku wdrożenia produkcyjnego wykonaj następujące czynności:

- Przekaż *aplikację\_pliku offline. htm* do lokacji produkcyjnej.
- W programie Visual Studio wybierz profil produkcyjny w **sieci Web** , a następnie **kliknij przycisk Publikuj.**
- Usuń *aplikację\_pliku offline. htm* z lokacji produkcyjnej.

> [!NOTE]
> Gdy aplikacja jest używana w środowisku produkcyjnym, należy zaimplementować plan tworzenia kopii zapasowych. Oznacza to, że należy okresowo kopiować pliki *School-prod. sdf* i *ASPNET-prod. sdf* z lokacji produkcyjnej do bezpiecznej lokalizacji magazynu i należy zachować kilka generacji takich kopii zapasowych. Podczas aktualizowania bazy danych należy wykonać kopię zapasową od razu przed zmianą. Następnie, jeśli wystąpi błąd i nie wykryjesz go, dopóki nie zostanie wdrożony w środowisku produkcyjnym, nadal będzie można odzyskać bazę danych do stanu, w którym była przed uszkodzeniem.

Gdy program Visual Studio otworzy w przeglądarce adres URL strony głównej, zostanie wyświetlona strona *\_offline. htm* . Po usunięciu *aplikacji\_pliku offline. htm* można ponownie przejść do strony głównej, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Teraz wdrożono aktualizację aplikacji, która obejmuje zmianę bazy danych na test i środowisko produkcyjne. W następnym samouczku pokazano, jak przeprowadzić migrację bazy danych z SQL Server Compact do SQL Server Express i SQL Server.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [dalej](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)

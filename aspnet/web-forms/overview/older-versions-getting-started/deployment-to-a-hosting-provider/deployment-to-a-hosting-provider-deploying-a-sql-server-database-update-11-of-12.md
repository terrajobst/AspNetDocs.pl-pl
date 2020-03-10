---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji bazy danych SQL Server Database-11 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528127"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie aktualizacji SQL Server Database — 11 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, pokazano, jak wdrożyć wersje SQL Server inne niż SQL Server Compact i jak wdrożyć je w witrynach sieci Web systemu Windows Azure, zobacz [ASP.NET Web Deployment za pomocą programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak wdrożyć aktualizację bazy danych w pełnej SQL Server bazie danych. Ponieważ Migracje Code First wykonuje wszystkie czynności związane z aktualizacją bazy danych, proces jest niemal identyczny z tym, co zostało zrobione SQL Server Compact w samouczku [wdrażanie aktualizacji bazy danych](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Dodawanie nowej kolumny do tabeli

W tej części samouczka wprowadzisz zmiany w bazie danych i odpowiednie zmiany w kodzie, a następnie przetestuj je w programie Visual Studio w celu wdrożenia ich w środowisku testowym i produkcyjnym. Zmiana obejmuje Dodawanie kolumny `OfficeHours` do jednostki `Instructor` i wyświetlanie nowych informacji na stronie sieci Web **instruktorów** .

W projekcie ContosoUniversity. DAL Otwórz *Instructor.cs* i Dodaj następującą właściwość między `HireDate` i `Courses` właściwości:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Zaktualizuj klasę inicjatora, aby umożliwić jej odsiewanie nowej kolumny przy użyciu danych testowych. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następującym blokiem kodu, który zawiera nową kolumnę:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

W projekcie ContosoUniversity Otwórz *instruktors. aspx* i Dodaj nowe pole szablonu dla godzin pracy tuż przed tagiem zamykającym `</Columns>` w pierwszej kontrolce `GridView`:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Skompiluj rozwiązanie.

Otwórz okno **konsoli Menedżera pakietów** i wybierz pozycję CONTOSOUNIVERSITY. dal jako **Projekt domyślny**.

Wprowadź następujące polecenia:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Uruchom aplikację i wybierz stronę **Instruktorzy** . Ładowanie strony trwa nieco dłużej niż zwykle, ponieważ usługa Entity Framework ponownie tworzy bazę danych i wystawia ją z danymi testowymi.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Wdrażanie aktualizacji bazy danych w środowisku testowym

W przypadku korzystania z Migracje Code First Metoda wdrażania bazy danych zmiana na SQL Server jest taka sama jak w przypadku SQL Server Compact. Należy jednak zmienić profil publikowania testowego, ponieważ nadal jest on skonfigurowany do migracji z SQL Server Compact do SQL Server.

Pierwszym krokiem jest usunięcie przekształceń parametrów połączenia utworzonych w poprzednim samouczku. Te nie są już potrzebne, ponieważ w profilu publikacji określisz przekształcenia parametrów połączenia, tak jak wcześniej przed skonfigurowaniem na karcie **pakiet/publikowanie danych SQL** na potrzeby migracji do SQL Server.

Otwórz plik *Web. test. config* i usuń element `connectionStrings`. Jedyna pozostała transformacja w pliku *Web. test. config* dotyczy wartości `Environment` w elemencie `appSettings`.

Teraz można zaktualizować profil publikowania i opublikować go w środowisku testowym.

Otwórz kreatora **publikacji w sieci Web** , a następnie przejdź do karty **profil** .

Wybierz profil publikowania **testowego** .

Wybierz kartę **Ustawienia** .

Kliknij pozycję **Włącz nowe ulepszenia publikowania bazy danych**.

W polu ciąg połączenia dla **SchoolContext**wprowadź tę samą wartość, która była używana w pliku transformacji *Web. test. config* w poprzednim samouczku:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Wybierz pozycję **wykonaj migracje Code First (uruchamiaj przy uruchamianiu aplikacji)** . (W używanej wersji programu Visual Studio, pole wyboru może mieć etykietę **zastosuj migracje Code First**.)

W polu ciąg połączenia dla **DefaultConnection**wprowadź tę samą wartość, która była używana w pliku transformacji *Web. test. config* w poprzednim samouczku:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Pozostaw wyczyszczone **aktualizacje bazy danych** .

Kliknij przycisk **Opublikuj**.

Program Visual Studio wdraża zmiany kodu w środowisku testowym i otwiera przeglądarkę na stronie głównej firmy Contoso University.

Wybierz stronę instruktorów.

Gdy aplikacja uruchamia Tę stronę, próbuje uzyskać dostęp do bazy danych. Migracje Code First sprawdza, czy baza danych jest aktualna, i stwierdza, że nie zastosowano jeszcze najnowszej migracji. Migracje Code First stosuje najnowszą migrację, uruchamia metodę `Seed`, a następnie strona działa normalnie. Zostanie wyświetlona kolumna nowe godziny pracy z danymi z rozrzutu.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Wdrażanie aktualizacji bazy danych w środowisku produkcyjnym

Należy również zmienić profil publikowania dla środowiska produkcyjnego. W takim przypadku należy usunąć istniejący profil i utworzyć nowy, importując zaktualizowany plik. publishsettings. Zaktualizowany plik będzie zawierać parametry połączenia dla bazy danych SQL Server w Cytanium.

Podczas wdrażania w środowisku testowym nie są już potrzebne transformacje parametrów połączenia w pliku transformacji *Web. Production. config* . Otwórz ten plik i Usuń element `connectionStrings`. Pozostałe przekształcenia dotyczą wartości `Environment` w elemencie `appSettings` i elementu `location`, który ogranicza dostęp do raportów o błędach ELMAH.

Przed utworzeniem nowego profilu publikowania dla środowiska produkcyjnego Pobierz zaktualizowany plik. publishsettings w taki sam sposób jak wcześniej w samouczku [wdrażanie w środowisku produkcyjnym](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . (W panelu sterowania Cytanium kliknij pozycję **witryny sieci Web**, a następnie kliknij witrynę internetową **contosouniversity.com** . Wybierz kartę **Publikowanie w sieci Web** , a następnie kliknij pozycję **Pobierz profil publikowania dla tej witryny sieci Web**. Przyczyną tego problemu jest pobranie parametrów połączenia bazy danych w pliku. publishsettings. Parametry połączenia nie były dostępne przy pierwszym pobraniu pliku, ponieważ nadal używasz SQL Server Compact i nie utworzyć SQL Server bazę danych w Cytanium.

Teraz można zaktualizować profil publikowania i opublikować go w środowisku produkcyjnym.

Otwórz kreatora **publikacji w sieci Web** , a następnie przejdź do karty **profil** .

Kliknij pozycję **Zarządzaj profilami**, a następnie Usuń profil produkcyjny.

Zamknij kreatora **publikacji w sieci Web** , aby zapisać tę zmianę.

Otwórz ponownie kreatora **publikacji sieci Web** , a następnie kliknij przycisk **Importuj**.

Na karcie **połączenie** Zmień **docelowy adres URL** na odpowiednią wartość, jeśli używasz tymczasowego adresu URL.

Kliknij przycisk **Dalej**.

Na karcie **Ustawienia** kliknij pozycję **Włącz nowe ulepszenia publikowania bazy danych**.

Z listy rozwijanej parametry połączenia dla **SchoolContext**Wybierz parametry połączenia Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Wybierz pozycję **Wykonaj migracje Code First (działa przy uruchamianiu aplikacji)** .

Z listy rozwijanej parametry połączenia dla **DefaultConnection**Wybierz parametry połączenia Cytanium.

Wybierz kartę **profil** , kliknij pozycję **Zarządzaj**profilami, a następnie zmień nazwę profilu z "contosouniversity.com Web Deploy" na "produkcja".

Zamknij profil publikowania, aby zapisać zmiany, a następnie otwórz je ponownie.

Kliknij przycisk **Opublikuj**. (W przypadku rzeczywistej produkcyjnej witryny sieci Web należy skopiować *aplikację\_offline. htm* do środowiska produkcyjnego i umieścić ją w folderze projektu przed opublikowaniem, a następnie usunąć ją po zakończeniu wdrażania.)

Program Visual Studio wdraża zmiany kodu w środowisku testowym i otwiera przeglądarkę na stronie głównej firmy Contoso University.

Wybierz stronę instruktorów.

Migracje Code First aktualizuje bazę danych w taki sam sposób jak w środowisku testowym. Zostanie wyświetlona kolumna nowe godziny pracy z danymi z rozrzutu.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Pomyślnie wdrożono aktualizację aplikacji, która obejmowała zmianę bazy danych, przy użyciu bazy danych SQL Server.

## <a name="more-information"></a>Więcej informacji

Ta seria samouczków dotyczy wdrażania aplikacji sieci Web ASP.NET w ramach dostawcy hostingu innej firmy. Aby uzyskać więcej informacji na temat tematów omówionych w tych samouczkach, zobacz [mapę zawartości wdrożenia ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) w witrynie MSDN w sieci Web.

## <a name="acknowledgements"></a>Podziękowania

Chcę podziękowanie następujące osoby, które złożyły znaczące wkłady w zawartość tej serii samouczków:

- [Alberto Poblacion, MVP &amp; MCT, Hiszpania](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, SPECJALISTa ds. projektowania platformy danych, Stany Zjednoczone
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Jan Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Włochy](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott myśliwy, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [dalej](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)

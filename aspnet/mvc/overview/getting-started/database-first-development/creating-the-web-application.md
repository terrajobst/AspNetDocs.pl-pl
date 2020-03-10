---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Samouczek: Tworzenie aplikacji sieci Web i modeli danych dla EF Database First za pomocą ASP.NET MVC'
description: Ten samouczek koncentruje się na tworzeniu aplikacji sieci Web i generowaniu modeli danych opartych na tabelach bazy danych.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616271"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a>Samouczek: Tworzenie aplikacji sieci Web i modeli danych dla EF Database First za pomocą ASP.NET MVC

 Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych. Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.

Ten samouczek koncentruje się na tworzeniu aplikacji sieci Web i generowaniu modeli danych opartych na tabelach bazy danych.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Tworzenie aplikacji internetowej platformy ASP.NET
> * Generuj modele

## <a name="prerequisites"></a>Wymagania wstępne

* [Wprowadzenie do Entity Framework 6 Database First przy użyciu MVC 5](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a>Tworzenie aplikacji internetowej platformy ASP.NET

W nowym rozwiązaniu lub w tym samym rozwiązaniu co projekt bazy danych Utwórz nowy projekt w programie Visual Studio i wybierz szablon **aplikacji sieci Web ASP.NET** . Nazwij projekt **ContosoSite**.

![Utwórz projekt](creating-the-web-application/_static/image1.png)

Kliknij przycisk **OK**.

W oknie Nowy projekt ASP.NET wybierz szablon **MVC** . Możesz teraz wyczyścić opcję **host w chmurze** , ponieważ później zostanie wdrożona aplikacja w chmurze. Kliknij przycisk **OK** , aby utworzyć aplikację.

Projekt jest tworzony z domyślnymi plikami i folderami.

W tym samouczku użyjesz Entity Framework 6. Możesz dwukrotnie sprawdzić wersję Entity Framework w projekcie za pomocą okna Zarządzanie pakietami NuGet. W razie potrzeby Zaktualizuj swoją wersję Entity Framework.

![Pokaż wersję](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generuj modele

Teraz utworzysz modele Entity Framework z tabel bazy danych. Te modele są klasami, które będą używane do pracy z danymi. Każdy model odzwierciedla tabelę w bazie danych i zawiera właściwości, które odnoszą się do kolumn w tabeli.

Kliknij prawym przyciskiem myszy folder **modele** , a następnie wybierz polecenie **Dodaj** i **nowy element**.

W oknie Dodawanie nowego elementu wybierz pozycję **dane** w lewym okienku i **ADO.NET Entity Data Model** z opcji w środkowym okienku. Nazwij nowy plik modelu **ContosoModel**.

Kliknij pozycję **Add** (Dodaj).

W Kreatorze Entity Data Model wybierz pozycję **Dr Designer z bazy danych**.

Kliknij przycisk **Dalej**.

Jeśli masz połączenia bazy danych zdefiniowane w środowisku deweloperskim, może zostać wyświetlony jeden z tych połączeń wstępnie wybranych. Jednak chcesz utworzyć nowe połączenie z bazą danych utworzoną w pierwszej części tego samouczka. Kliknij przycisk **nowe połączenie** .

W okno Właściwości połączenia Podaj nazwę lokalnego serwera, na którym została utworzona baza danych (w tym przypadku **(LocalDB) \ProjectsV13**). Po podania nazwy serwera wybierz ContosoUniversityData z dostępnych baz danych.

![Ustawianie właściwości połączenia](creating-the-web-application/_static/image8.png)

Kliknij przycisk **OK**.

Zostaną wyświetlone poprawne właściwości połączenia. Możesz użyć nazwy domyślnej dla połączenia w pliku Web. config.

Kliknij przycisk **Dalej**.

Wybierz najnowszą wersję Entity Framework.

Kliknij przycisk **Dalej**.

Wybierz pozycję **tabele** , aby wygenerować modele dla wszystkich trzech tabel.

Kliknij przycisk **Finish** (Zakończ).

Jeśli zostanie wyświetlone ostrzeżenie o zabezpieczeniach, wybierz **przycisk OK** , aby kontynuować uruchamianie szablonu.

Modele są generowane na podstawie tabel bazy danych i wyświetlany jest diagram, który pokazuje właściwości i relacje między tabelami.

![Diagram modelu](creating-the-web-application/_static/image11.png)

Folder modele zawiera teraz wiele nowych plików związanych z modelami, które zostały wygenerowane na podstawie bazy danych programu.

Plik **ContosoModel.Context.cs** zawiera klasę, która dziedziczy z klasy **DbContext** , i udostępnia właściwość dla każdej klasy modelu, która odpowiada tabeli bazy danych. Pliki **Course.cs**, **Enrollment.cs**i **student.cs** zawierają klasy modelu, które reprezentują tabele baz danych. Podczas pracy z szkieletami należy używać zarówno klasy kontekstu, jak i klas modelu.

Przed kontynuowaniem pracy z tym samouczkiem Skompiluj projekt. W następnej sekcji zostanie wygenerowany kod oparty na modelach danych, ale ta sekcja nie będzie działała, jeśli projekt nie został skompilowany.

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Utworzono aplikację sieci Web ASP.NET
> * Wygenerowane modele

Przejdź do następnego samouczka, aby dowiedzieć się, jak utworzyć generowanie kodu na podstawie modeli danych.
> [!div class="nextstepaction"]
> [Generowanie widoków](generating-views.md)

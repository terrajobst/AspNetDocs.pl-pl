---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'Samouczek: wprowadzenie do programu EF Database First przy użyciu MVC 5'
description: W tym samouczku przedstawiono sposób rozpoczynania pracy z istniejącą bazą danych i szybkiego tworzenia aplikacji sieci Web, która umożliwia użytkownikom współdziałanie z danymi.
author: Rick-Anderson
ms.author: riande
ms.date: 01/15/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: a760767839a834a9c7e9fe358a3fd806a833261f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583525"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a>Samouczek: wprowadzenie do programu EF Database First przy użyciu MVC 5

Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych. Wygenerowany kod odpowiada kolumnom w tabeli bazy danych. W ostatniej części serii dowiesz się, jak dodać adnotacje danych do modelu danych, aby określić wymagania dotyczące weryfikacji i formatowanie wyświetlania. Gdy skończysz, możesz przejść do artykułu platformy Azure, aby dowiedzieć się, jak wdrożyć aplikację .NET i bazę danych SQL w celu Azure App Service.

W tym samouczku przedstawiono sposób rozpoczynania pracy z istniejącą bazą danych i szybkiego tworzenia aplikacji sieci Web, która umożliwia użytkownikom współdziałanie z danymi. Używa Entity Framework 6 i MVC 5 do kompilowania aplikacji sieci Web. Funkcja tworzenia szkieletów ASP.NET umożliwia automatyczne generowanie kodu do wyświetlania, aktualizowania, tworzenia i usuwania danych. Korzystając z narzędzi do publikowania w programie Visual Studio, można łatwo wdrożyć lokację i bazę danych na platformie Azure.

Ta część serii koncentruje się na tworzeniu bazy danych i wypełnianiu jej danymi.

Ta seria została zapisywana z użyciem wkładów z Dykstra i Rick Anderson. Ulepszono je na podstawie opinii użytkowników w sekcji komentarzy.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Konfigurowanie bazy danych

## <a name="prerequisites"></a>Wymagania wstępne

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="set-up-the-database"></a>Konfigurowanie bazy danych

Aby naśladować środowisko mające istniejącą bazę danych, należy najpierw utworzyć bazę danych z wstępnie wypełnionymi danymi, a następnie utworzyć aplikację sieci Web, która łączy się z bazą danych.

Ten samouczek został opracowany przy użyciu LocalDB z programem Visual Studio 2017. Możesz użyć istniejącego serwera bazy danych zamiast LocalDB, ale w zależności od używanej wersji programu Visual Studio i typu bazy danych, wszystkie narzędzia danych w programie Visual Studio mogą nie być obsługiwane. Jeśli narzędzia nie są dostępne dla bazy danych, może być konieczne wykonanie niektórych kroków specyficznych dla bazy danych w ramach pakietu administracyjnego dla bazy danych programu.

Jeśli masz problem z narzędziami bazy danych w używanej wersji programu Visual Studio, upewnij się, że zainstalowano najnowszą wersję narzędzi bazy danych. Informacje o aktualizowaniu lub instalowaniu narzędzi bazy danych znajdują się w temacie [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Uruchom program Visual Studio i Utwórz **projekt bazy danych SQL Server**. Nazwij projekt **ContosoUniversityData**.

![Utwórz projekt bazy danych](setting-up-database/_static/image1.png)

Masz teraz pusty projekt bazy danych. Aby upewnić się, że możesz wdrożyć tę bazę danych na platformie Azure, Azure SQL Database jako platformę docelową dla projektu. Ustawienie platformy docelowej nie powoduje rzeczywistego wdrożenia bazy danych; oznacza to, że projekt bazy danych sprawdzi, czy projekt bazy danych jest zgodny z platformą docelową. Aby ustawić platformę docelową, Otwórz **Właściwości** projektu i wybierz **Microsoft Azure SQL Database** dla platformy docelowej.

Tabele potrzebne do tego samouczka można utworzyć, dodając skrypty SQL, które definiują tabele. Kliknij prawym przyciskiem myszy projekt i Dodaj nowy element. Wybierz **tabele i widoki** > **tabelę** i nadaj jej nazwę *student*.

W pliku tabeli Zastąp polecenie T-SQL następującym kodem, aby utworzyć tabelę.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Należy zauważyć, że okno projektowania jest automatycznie synchronizowane z kodem. Możesz współpracować z kodem lub projektantem.

![Pokaż kod i projekt](setting-up-database/_static/image5.png)

Dodaj kolejną tabelę. Tym razem Nazwij ten kurs i użyj poniższego polecenia T-SQL.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

I Powtórz jeszcze raz, aby utworzyć tabelę o nazwie Rejestracja.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Bazę danych można wypełniać danymi za pomocą skryptu uruchamianego po wdrożeniu bazy danych. Dodaj skrypt powdrożeniowy do projektu. Kliknij prawym przyciskiem myszy projekt i Dodaj nowy element. Wybierz pozycję **skrypty użytkownika** > **skrypt po wdrożeniu**. Możesz użyć nazwy domyślnej.

Dodaj następujący kod T-SQL do skryptu powdrożeniowego. Ten skrypt po prostu dodaje dane do bazy danych, gdy nie znaleziono zgodnego rekordu. Nie zastępuje ani nie usuwa żadnych danych, które mogły zostać wprowadzone do bazy danych.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Należy pamiętać, że skrypt powdrożeniowy jest uruchamiany za każdym razem, gdy wdrażasz projekt bazy danych. W związku z tym należy uważnie uwzględnić wymagania podczas pisania tego skryptu. W niektórych przypadkach warto zacząć od znanego zestawu danych za każdym razem, gdy projekt zostanie wdrożony. W innych przypadkach możesz nie chcieć zmieniać istniejących danych w dowolny sposób. Na podstawie Twoich wymagań możesz zdecydować, czy potrzebny jest skrypt po wdrożeniu, czy też co należy uwzględnić w skrypcie. Aby uzyskać więcej informacji na temat wypełniania bazy danych za pomocą skryptu powdrożeniowego, zobacz [Dołączanie danych do projektu bazy danych SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Masz teraz 4 pliki skryptów SQL, ale nie rzeczywiste tabele. Możesz przystąpić do wdrażania projektu bazy danych w programie LocalDB. W programie Visual Studio kliknij przycisk Start (lub F5), aby skompilować i wdrożyć projekt bazy danych. Sprawdź kartę **dane wyjściowe** , aby sprawdzić, czy kompilacja i wdrożenie powiodło się.

Aby zobaczyć, że została utworzona nowa baza danych, Otwórz **Eksplorator obiektów SQL Server** i poszukaj nazwy projektu na poprawnym lokalnym serwerze bazy danych (w tym przypadku **(LocalDB) \ProjectsV13**).

Aby zobaczyć, że tabele są wypełnione danymi, kliknij prawym przyciskiem myszy tabelę i wybierz polecenie **Wyświetl dane**.

![Pokaż dane tabeli](setting-up-database/_static/image9.png)

Wyświetlany jest edytowalny widok danych tabeli. Na przykład po wybraniu opcji **tabele** > **dbo. kurs** > **Wyświetlanie danych**zostanie wyświetlona tabela z trzema kolumnami (**kurs**, **tytuł**i **kredyty**) i cztery wiersze.

## <a name="additional-resources"></a>Dodatkowe zasoby

Przykładowy przykład tworzenia Code First można znaleźć [w temacie Wprowadzenie with ASP.NET MVC 5](../introduction/getting-started.md). Aby zapoznać się z bardziej zaawansowanym przykładem, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Aby uzyskać wskazówki dotyczące wybierania Entity Framework podejścia do użycia, zobacz [Entity Framework podejścia deweloperskiego](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Konfigurowanie bazy danych

Przejdź do następnego samouczka, aby dowiedzieć się, jak utworzyć aplikację sieci Web i modele danych.
> [!div class="nextstepaction"]
> [Tworzenie aplikacji sieci Web i modeli danych](creating-the-web-application.md)

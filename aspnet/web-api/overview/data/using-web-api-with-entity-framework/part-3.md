---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Użyj Migracje Code First do wypełniania bazy danych | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557457"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a>Używanie Migracje Code First do wypełniania bazy danych

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) w EF do wypełniania bazy danych danymi testowymi.

W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](part-3/samples/sample1.cmd)]

To polecenie dodaje folder o nazwie migrations do projektu oraz plik kodu o nazwie Configuration.cs w folderze migrations.

![](part-3/_static/image1.png)

Otwórz plik Configuration.cs. Dodaj następującą instrukcję **using** .

[!code-csharp[Main](part-3/samples/sample2.cs)]

Następnie Dodaj następujący kod do metody **Configuration. inicjator** :

[!code-csharp[Main](part-3/samples/sample3.cs)]

W oknie Konsola Menedżera pakietów wpisz następujące polecenia:

[!code-console[Main](part-3/samples/sample4.cmd)]

Pierwsze polecenie generuje kod, który tworzy bazę danych, a drugie polecenie wykonuje ten kod. Baza danych jest tworzona lokalnie, przy użyciu [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Eksplorowanie interfejsu API (opcjonalnie)

Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania. Program Visual Studio uruchamia się IIS Express i uruchamia aplikację sieci Web. Program Visual Studio uruchomi przeglądarkę i otworzy stronę główną aplikacji.

Gdy program Visual Studio uruchamia projekt sieci Web, przypisuje numer portu. Na poniższej ilustracji numer portu to 50524. Po uruchomieniu aplikacji zobaczysz inny numer portu.

![](part-3/_static/image3.png)

Strona główna jest implementowana przy użyciu ASP.NET MVC. W górnej części strony znajduje się link "API". Ten link umożliwia przełączenie do automatycznie generowanej strony pomocy dla internetowego interfejsu API. (Aby dowiedzieć się, jak ta strona pomocy jest generowana i jak można dodać własną dokumentację do strony, zobacz [Tworzenie stron pomocy dla interfejsu API sieci Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)). Możesz kliknąć linki na stronie pomocy, aby wyświetlić szczegółowe informacje o interfejsie API, w tym format żądania i odpowiedzi.

![](part-3/_static/image4.png)

Interfejs API umożliwia wykonywanie operacji CRUD w bazie danych. Poniżej przedstawiono podsumowanie interfejsu API.

| Autorzy |  |
| --- | -- |
| GET api/authors | Pobierz wszystkich autorów. |
| GET api/authors/{id} | Pobierz autora według identyfikatora. |
| POST /api/authors | Utwórz nowego autora. |
| PUT /api/authors/{id} | Zaktualizuj istniejącego autora. |
| DELETE /api/authors/{id} | Usuń autora. |

| Książki |  |
| --- | -- |
| GET /api/books | Pobierz wszystkie książki. |
| GET /api/books/{id} | Pobierz książkę według identyfikatora. |
| POST /api/books | Utwórz nową książkę. |
| PUT /api/books/{id} | Aktualizowanie istniejącej książki. |
| DELETE /api/books/{id} | Usuń książkę. |

## <a name="view-the-database-optional"></a>Wyświetl bazę danych (opcjonalnie)

Po uruchomieniu polecenia Update-Database, EF utworzył bazę danych i wywołała metodę `Seed`. Gdy aplikacja jest uruchamiana lokalnie, EF używa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Bazę danych można wyświetlić w programie Visual Studio. Z menu **Widok** wybierz opcję **Eksplorator obiektów SQL Server**.

![](part-3/_static/image5.png)

W oknie dialogowym **łączenie z serwerem** w polu **Nazwa serwera** wpisz "(LocalDB) \v11.0". Pozostaw opcję **uwierzytelniania** "uwierzytelnianie systemu Windows". Kliknij przycisk **Connect** (Połącz).

![](part-3/_static/image6.png)

Program Visual Studio nawiązuje połączenie z usługą LocalDB i wyświetla istniejące bazy danych w oknie Eksplorator obiektów SQL Server. Można rozwinąć węzły, aby wyświetlić tabele, które zostały utworzone przez EF.

![](part-3/_static/image7.png)

Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybierz polecenie **Wyświetl dane**.

![](part-3/_static/image8.png)

Poniższy zrzut ekranu przedstawia wyniki tabeli Books. Zwróć uwagę, że EF zapełnił bazę danych danymi o inicjatorze, a tabela zawiera klucz obcy do tabeli autorów.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Poprzednie](part-2.md)
> [dalej](part-4.md)

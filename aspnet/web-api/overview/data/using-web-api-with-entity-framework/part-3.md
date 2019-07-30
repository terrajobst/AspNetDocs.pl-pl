---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Użyć migracje Code First na inicjowanie bazy danych | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59421670"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a>Użyć migracje Code First na inicjowanie bazy danych

przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) w programie EF w celu umieszczenia w bazie danych testowych.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](part-3/samples/sample1.cmd)]

To polecenie dodaje folder o nazwie migracji do projektu, a także o nazwie Configuration.cs w folderze migracje pliku z kodem.

![](part-3/_static/image1.png)

Otwórz plik Configuration.cs. Dodaj następujący kod **przy użyciu** instrukcji.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Następnie dodaj następujący kod do **Configuration.Seed** metody:

[!code-csharp[Main](part-3/samples/sample3.cs)]

W oknie Konsola Menedżera pakietów wpisz następujące polecenia:

[!code-console[Main](part-3/samples/sample4.cmd)]

Pierwsze polecenie generuje kod, który tworzy bazę danych, a drugie polecenie wykonuje kod. Baza danych jest tworzona lokalnie, za pomocą [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Poznaj interfejs API (opcjonalnie)

Naciśnij klawisz F5, aby uruchomić aplikację w trybie debugowania. Visual Studio uruchamia usług IIS Express i uruchamia aplikację sieci web. Następnie, Visual Studio otworzy w przeglądarce i otwiera strony głównej aplikacji.

Po uruchomieniu projektu sieci web programu Visual Studio przypisuje numer portu. Na poniższej ilustracji numer portu to 50524. Po uruchomieniu aplikacji, zobaczysz inny numer portu.

![](part-3/_static/image3.png)

Strona główna jest implementowany przy użyciu platformy ASP.NET MVC. W górnej części strony istnieje link, który jest wyświetlany komunikat "Interfejs API". Ten link oferuje do strony pomocy generowane automatycznie dla internetowego interfejsu API. (Aby dowiedzieć się, jak jest generowany stronę pomocy i dodawania własnych dokumentacji do strony, zobacz [Tworzenie strony pomocy dla interfejsu API sieci Web platformy ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Możesz kliknąć pomoc łączy strony, aby wyświetlić szczegóły dotyczące interfejsu API, w tym formacie żądań i odpowiedzi.

![](part-3/_static/image4.png)

Interfejs API umożliwia wykonywanie operacji CRUD na bazie danych. Poniżej przedstawiono podsumowanie interfejsu API.

| Autorzy |  |
| --- | -- |
| GET api/authors | Pobieranie wszystkich autorów. |
| GET api/authors/{id} | Pobierz Autor według identyfikatora. |
| POST /api/authors | Utwórz nowy autora. |
| PUT /api/authors/{id} | Zaktualizuj istniejący autora. |
| DELETE /api/authors/{id} | Usuń autora. |

| Książki |  |
| --- | -- |
| GET /api/books | Pobierz wszystkie książki. |
| GET /api/books/{id} | Pobierz książkę według identyfikatora. |
| POST /api/books | Tworzenie nowej książki. |
| PUT /api/books/{id} | Aktualizowanie istniejącej. |
| DELETE /api/books/{id} | Usuń książki. |

## <a name="view-the-database-optional"></a>Widok bazy danych (opcjonalnie)

Po uruchomieniu polecenia Update-Database, EF, tworzenia bazy danych oraz o nazwie `Seed` metody. Po uruchomieniu aplikacji lokalnie EF używa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Bazy danych można wyświetlić w programie Visual Studio. Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server**.

![](part-3/_static/image5.png)

W **Połącz z serwerem** okna dialogowego w **nazwy serwera** pole edycji, wpisz "(localdb) \v11.0". Pozostaw **uwierzytelniania** opcji "Uwierzytelnianie Windows". Kliknij przycisk **Połącz**.

![](part-3/_static/image6.png)

Program Visual Studio połączy się LocalDB i pokazuje istniejących baz danych w oknie Eksplorator obiektów SQL Server. Można rozwinąć węzły, aby zobaczyć tabele utworzone EF.

![](part-3/_static/image7.png)

Aby wyświetlić dane, kliknij prawym przyciskiem myszy tabelę i wybrać **dane widoku**.

![](part-3/_static/image8.png)

Poniższy zrzut ekranu przedstawia wyniki dla tabeli książki. Należy zauważyć, że EF wypełnione w bazie danych inicjatora, a tabela zawiera klucz obcy tabeli autorów.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Poprzednie](part-2.md)
> [dalej](part-4.md)

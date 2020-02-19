---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Tworzenie parametrów połączenia i praca z SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 20781ad760d3a0e4559ec4c7e18528f3686dcc02
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456520"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i praca z bazą danych SQL Server LocalDB

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Tworzenie parametrów połączenia i praca z bazą danych SQL Server LocalDB

Utworzona Klasa `MovieDBContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych. Jednym z pytań, które można zadać, jest określenie, jak należy określić bazę danych, z którą zostanie nawiązane połączenie. Nie trzeba określać bazy danych, która ma być używana, Entity Framework domyślnie będzie używać [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). W tej sekcji jawnie dodamy parametry połączenia w pliku *Web. config* aplikacji.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) to uproszczona wersja aparatu bazy danych SQL Server Express uruchamiana na żądanie i uruchamiana w trybie użytkownika. LocalDB działa w specjalnym trybie wykonywania SQL Server Express, który umożliwia współpracę z bazami danych jako plikami *MDF* . Zazwyczaj pliki bazy danych LocalDB są przechowywane w folderze *danych\_aplikacji* projektu sieci Web.

Nie zaleca się stosowania SQL Server Express w produkcyjnych aplikacjach sieci Web. LocalDB w szczególności nie należy używać w środowisku produkcyjnym z aplikacją sieci Web, ponieważ nie jest ona przeznaczona do pracy z usługami IIS. Jednak baza danych LocalDB może być łatwo migrowana do SQL Server lub SQL Azure.

W programie Visual Studio 2017 LocalDB jest instalowany domyślnie z programem Visual Studio.

Domyślnie Entity Framework szuka parametrów połączenia o nazwie identycznej z klasą kontekstu obiektu (`MovieDBContext` dla tego projektu). Aby uzyskać więcej informacji, zobacz [SQL Server parametry połączenia dla aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Otwórz główny plik *Web. config* aplikacji przedstawiony poniżej. (Plik *Web. config* znajduje się w folderze *widoki* ).

![](creating-a-connection-string/_static/image1.png)

Znajdź `<connectionStrings>` element:

![](creating-a-connection-string/_static/image2.png)

Dodaj następujące parametry połączenia do elementu `<connectionStrings>` w pliku *Web. config* .

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Poniższy przykład przedstawia część pliku *Web. config* z nowymi dodanymi parametrami połączenia:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Dwa parametry połączenia są bardzo podobne. Pierwsze parametry połączenia mają nazwę `DefaultConnection` i służy do kontrolowania, kto może uzyskać dostęp do aplikacji. Dodane parametry połączenia określają LocalDB bazę danych o nazwie *Movie. mdf* znajdującą się w folderze *danych\_aplikacji* . W tym samouczku nie będziemy korzystać z bazy danych członkostwa, aby uzyskać więcej informacji na temat członkostwa, uwierzytelniania i zabezpieczeń, zobacz mój samouczek [Tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdrażanie jej w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Nazwa parametrów połączenia musi być zgodna z nazwą klasy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Nie trzeba faktycznie dodawać parametrów połączenia `MovieDBContext`. Jeśli nie określisz parametrów połączenia, Entity Framework utworzy bazę danych LocalDB w katalogu Users z w pełni kwalifikowaną nazwą klasy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (w tym przypadku `MvcMovie.Models.MovieDBContext`). Możesz nazywać dowolną bazę danych, o ile ma *. Sufiks MDF* . Załóżmy na przykład, że nazwa bazy danych to *. mdf*.

Następnie utworzysz nową klasę `MoviesController`, która może być używana do wyświetlania danych filmowych i umożliwia użytkownikom tworzenie nowych list filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-model.md)
> [dalej](accessing-your-models-data-from-a-controller.md)

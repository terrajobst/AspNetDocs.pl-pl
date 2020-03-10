---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Konfigurowanie produkcyjnej aplikacji sieci Web do korzystania z produkcyjnejC#bazy danych () | Microsoft Docs
author: rick-anderson
description: Jak opisano w poprzednich samouczkach, informacje konfiguracyjne różnią się w zależności od środowiska deweloperskiego i produkcyjnego. To są es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 89941bb6db52316a259ad5f5577721e36f19bd84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631531"
---
# <a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Konfigurowanie produkcyjnej aplikacji internetowej do korzystania z produkcyjnej bazy danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Jak opisano w poprzednich samouczkach, informacje konfiguracyjne różnią się w zależności od środowiska deweloperskiego i produkcyjnego. Jest to szczególnie istotne w przypadku aplikacji sieci Web opartych na danych, ponieważ parametry połączenia bazy danych różnią się w środowisku deweloperskim i produkcyjnym. W tym samouczku przedstawiono sposoby konfigurowania środowiska produkcyjnego w celu uwzględnienia odpowiednich parametrów połączenia w bardziej szczegółowy sposób.

## <a name="introduction"></a>Wprowadzenie

Aplikacje sieci Web oparte na danych zazwyczaj korzystają z innej bazy danych niż w środowisku produkcyjnym. W przypadku aplikacji hostowanych przez dostawcę hosta sieci Web i opracowanych lokalnie baza danych programistycznych zazwyczaj znajduje się na komputerze dewelopera, podczas gdy produkcyjna baza danych jest hostowana na serwerze bazy danych w funkcji udostępniania w sieci Web. Wdrożenie aplikacji sieci Web opartej na danych wiąże się z kopiowaniem bazy danych programistycznych na produkcyjny serwer bazy danych. W poprzednim samouczku przedstawiono sposoby wykonywania tego kroku.

Aplikacja sieci Web używa informacji w *parametrach połączenia* w celu nawiązania połączenia z bazą danych. Parametry połączenia, które są zwykle przechowywane w `Web.config`, określa nazwę serwera bazy danych, nazwę bazy danych, kontekst zabezpieczeń i inne informacje. Ponieważ baza danych używana przez aplikację sieci Web zależy od tego, czy aplikacja sieci Web działa w środowisku deweloperskim czy produkcyjnym, parametry połączenia muszą się różnić między tymi dwoma środowiskami.

Informacje o konfiguracji różnią się w zależności od środowiska deweloperskiego i produkcyjnego. *Typowe różnice konfiguracji między samouczkiem opracowywania i produkcji* omawiają metody obsługi osobnych informacji konfiguracyjnych między tymi dwoma środowiskami, a także krótkie omówienie parametrów połączenia bazy danych. W tym samouczku przedstawiono sposoby konfigurowania środowiska produkcyjnego w celu uwzględnienia odpowiednich parametrów połączenia w bardziej szczegółowy sposób.

## <a name="examining-the-connection-string-information"></a>Badanie informacji o parametrach połączenia

Parametry połączenia używane przez aplikację sieci Web przeglądów książek są przechowywane w pliku konfiguracji aplikacji `Web.config`. `Web.config` zawiera specjalną sekcję do przechowywania parametrów połączenia, aptly o nazwie [&lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Plik `Web.config` witryny sieci Web przeglądający książki ma jeden parametr połączenia zdefiniowany w tej sekcji o nazwie `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

Parametry połączenia — źródło danych = .\SQLEXPRESS; AttachDbFilename = | Katalog DataDirectory | \Reviews.mdf; zabezpieczenia zintegrowane = true; Wystąpienie użytkownika = true — składa się z szeregu opcji i wartości, a pary Option/wartość są rozdzielane średnikami i każdej opcji i wartości rozdzielane znakiem równości. Cztery opcje używane w tych parametrach połączenia to:

- `Data Source` — określa lokalizację serwera bazy danych i nazwę wystąpienia serwera bazy danych (jeśli istnieje). Wartość, `.\SQLEXPRESS`, to przykład, w którym występuje serwer bazy danych i nazwa wystąpienia. Okres określa, że serwer bazy danych znajduje się na tym samym komputerze co aplikacja; Nazwa wystąpienia jest `SQLEXPRESS`.
- `AttachDbFilename` — określa lokalizację pliku bazy danych. Wartość zawiera `|DataDirectory|`symbolu zastępczego, który jest rozpoznawany jako pełna ścieżka do folderu `App_Data` aplikacji w czasie wykonywania.
- `Integrated Security` — wartość logiczna wskazująca, czy podczas nawiązywania połączenia z bazą danych (false) lub bieżącym poświadczenia konta systemu Windows ma być używana określona nazwa użytkownika/hasło.
- `User Instance` — opcja konfiguracji specyficzna dla wersji SQL Server Express, która wskazuje, czy zezwolić użytkownikom nieadministracyjnym na komputerze lokalnym na dołączanie do bazy danych SQL Server Express Edition i nawiązywanie z nią połączenia. Aby uzyskać więcej informacji na temat tego ustawienia, zobacz [SQL Server Express wystąpień użytkownika](https://msdn.microsoft.com/library/ms254504.aspx) .

Opcje parametrów połączenia dozwolone są zależne od bazy danych, z którą nawiązujesz połączenie, oraz używanego dostawcy bazy danych ADO.NET. Na przykład parametry połączenia w celu nawiązania połączenia z bazą danych Microsoft SQL Server różnią się od używane do nawiązywania połączeń z bazą danych Oracle. Podobnie połączenie z bazą danych Microsoft SQL Server przy użyciu dostawcy SqlClient używa innych parametrów połączenia niż w przypadku korzystania z dostawcy OLE DB.

Możesz ręcznie utworzyć parametry połączenia z bazą danych za pomocą witryny, takiej jak [connectionStrings.com](http://www.connectionstrings.com/) , jako zasobu dla opcji dostępnych. Łatwiej jest jednak dodać bazę danych do Eksplorator serwera w programie Visual Studio, a następnie pobrać parametry połączenia z okno Właściwości. Niech s użyje tej ostatniej metody tworzenia parametrów połączenia do produkcyjnego serwera bazy danych.

Otwórz program Visual Studio, a następnie przejdź do okna Eksplorator serwera (w programie Visual Web Developer to okno nosi nazwę Eksplorator bazy danych). Kliknij prawym przyciskiem myszy opcję połączenia danych, a następnie wybierz opcję Dodaj połączenie z menu kontekstowego. Spowoduje to wyświetlenie Kreatora pokazanego na rysunku 1. Wybierz odpowiednie źródło danych i kliknij przycisk Kontynuuj.

[![wybrać dodanie nowej bazy danych do Eksplorator serwera](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Rysunek 1**. Wybierz, aby dodać nową bazę danych do Eksplorator serwera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))

Następnie określ różne informacje o połączeniu z bazą danych (patrz rysunek 2). Po zarejestrowaniu się w firmie hostingowej w sieci Web powinny zostać podane informacje dotyczące sposobu łączenia się z bazą danych programu — nazwy serwera bazy danych, nazwy bazy danych i hasła, które ma być używane do łączenia się z bazą danych i tak dalej. Po wprowadzeniu tych informacji kliknij przycisk OK, aby zakończyć pracę kreatora i dodać bazę danych do Eksplorator serwera.

[![określić informacje o połączeniu z bazą danych](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Rysunek 2**. Określanie informacji o połączeniu z bazą danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))

Baza danych środowiska produkcyjnego powinna być teraz wymieniona w Eksplorator serwera. Wybierz bazę danych z Eksplorator serwera i przejdź do okno Właściwości. Znaleziono właściwość o nazwie Connection parametrów z parametrami połączenia bazy danych. Przy założeniu, że używasz bazy danych Microsoft SQL Server w środowisku produkcyjnym i dostawcy SqlClient, parametry połączenia powinny wyglądać podobnie do następujących:

<strong>Źródło danych =<em>servername</em>; Katalog początkowy =<em>DatabaseName</em>; Utrwalanie informacji o zabezpieczeniach = true; Identyfikator użytkownika =<em>username</em>; Hasło =*hasło</strong>*

Gdzie *servername*, *DatabaseName*, *username*i *Password* ma wartości dla nazwy serwera bazy danych, nazwy bazy danych oraz nazwy użytkownika i hasła dostarczonej przez firmę hosta sieci Web.

## <a name="deploying-the-book-reviews-web-application"></a>Wdrażanie aplikacji sieci Web do przeglądu książki

Poprzedni samouczek przeszedł przez Kopiowanie bazy danych programistycznych do środowiska produkcyjnego, ale nie eksploruje wdrażania aplikacji opartej na danych. W tym momencie środowisko produkcyjne zawiera bazę danych, ale korzysta z wersji programu książka przeglądający w aplikacji ze statycznymi przeglądami. Musimy wdrożyć nową, opartą na danych aplikację na serwerze produkcyjnym wraz ze zaktualizowanymi informacjami o konfiguracji.

Poświęć chwilę na wdrożenie aplikacji opartej na danych ze środowiska programistycznego w środowisku produkcyjnym. Ten proces został szczegółowo omówiony w poprzednich samouczkach. Jeśli potrzebujesz odświeżacza, zobacz *wdrażanie witryny sieci Web przy użyciu klienta FTP* lub *wdrażanie witryny sieci Web przy użyciu samouczków programu Visual Studio* . Należy upewnić się, że parametry połączenia produkcyjnej bazy danych są używane w środowisku produkcyjnym, co oznacza, że należy wdrożyć alternatywny plik `Web.config`. W związku z tym zmodyfikowany element `Web.config` pliku `<connectionStrings>` musi zawierać parametry połączenia produkcyjnej bazy danych i powinien wyglądać podobnie do poniższego:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Należy zauważyć, że parametry połączenia w elemencie `<connectionStrings>` mają takie same (`ReviewsConnectionString`), ale teraz zawierają parametry połączenia produkcyjnej bazy danych, a nie parametry połączenia z bazą danych deweloperskich.

Jeśli nie masz bardziej formalnego przepływu pracy wdrożenia, ręcznie zmodyfikuj plik `Web.config`, aby używał parametrów połączenia produkcyjnej bazy danych przed wdrożeniem (zapamiętywanie w celu przywrócenia z powrotem przy użyciu parametrów połączenia z bazą danych) lub Zachowaj osobny plik `Web.config` z informacjami o konfiguracji środowiska produkcyjnego, które są przekazywane do środowiska produkcyjnego w ramach procesu wdrażania.

> [!NOTE]
> Jeśli przypadkowo wdrożono plik `Web.config` zawierający parametry połączenia z bazą danych deweloperskich, wystąpi błąd podczas próby nawiązania połączenia z bazą danych przez aplikację w środowisku produkcyjnym. Ten błąd manifestuje jako `SqlException` z komunikatem informującym o tym, że serwer nie został odnaleziony lub był niedostępny.

Po wdrożeniu lokacji w środowisku produkcyjnym odwiedź witrynę produkcyjną za pomocą przeglądarki. W przypadku lokalnego uruchamiania aplikacji opartej na danych należy zobaczyć i korzystać z tego samego środowiska użytkownika. Oczywiście po odwiedzeniu witryny sieci Web w środowisku produkcyjnym lokacja jest obsługiwana przez produkcyjny serwer bazy danych, podczas gdy witryna sieci Web jest wykorzystywana do tworzenia aplikacji. Rysunek 3 przedstawia samouczek *ASP.NET 3,5 na stronie Przegląd 24-godzinny* z witryny sieci Web w środowisku produkcyjnym (należy zwrócić uwagę na adres URL na pasku adresu przeglądarki).

[![aplikacja oparta na danych jest teraz dostępna w środowisku produkcyjnym!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Rysunek 3**. aplikacja oparta na danych jest teraz dostępna w środowisku produkcyjnym. ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))

### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Przechowywanie parametrów połączenia w osobnym pliku konfiguracji

Typową techniką utrzymywania oddzielnych informacji konfiguracyjnych w środowisku deweloperskim i produkcyjnym jest posiadanie dwóch wersji `Web.config`: jeden dla środowiska programistycznego i jeden do produkcji. W czasie wdrażania odpowiednia wersja `Web.config` może zostać skopiowana do środowiska produkcyjnego. Najlepiej, aby ten proces był zautomatyzowany w ramach przepływu pracy wdrażania.

Zamiast utrzymywać dwa osobne pliki `Web.config`, opcjonalnie możesz uzyskać bardziej szczegółowe różnice. Elementy wchodzące w skład pliku `Web.config` można zdefiniować w zewnętrznych plikach konfiguracji, do których następnie odwołuje się plik `Web.config`. W Nutshell można mieć jeden plik `Web.config` dla obu środowisk odwołujących się do pliku databaseConnectionStrings. config, który będzie zawierać parametry połączenia używane przez aplikację i będą unikatowe dla każdego środowiska. Wiemy, że oddzielając różne informacje o konfiguracji do oddzielnych plików, tidier i prostszy plik `Web.config` i bardziej jasno podkreśla różnice konfiguracji w środowiskach deweloperskich i produkcyjnych.

Aby użyć tej techniki, Zacznij od utworzenia nowego folderu w aplikacji sieci Web o nazwie `ConfigSections`. Następnie Dodaj dwa pliki do tego nowego folderu o nazwie databaseConnectionStrings. dev. config i databaseConnectionStrings. Product. config. Następnie skopiuj element `<connectionStrings>` z `Web.config` do plików databaseConnectionStrings. dev. config i databaseConnectionStrings. Production. config, a następnie zmodyfikuj parametry połączenia w pliku databaseConnectionStrings. Production. config, aby określić parametry połączenia z produkcyjną bazą danych. Na przykład plik databaseConnectionStrings. dev. config powinien zawierać tylko `<connectionStrings>` element z parametrami połączenia, które odwołują się do bazy danych programu Development:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Podobnie plik databaseConnectionStrings. Product. config powinien zawierać tylko `<connectionStrings>` element, ale taki, który ma parametry połączenia produkcyjnej bazy danych.

Utwórz kopię pliku databaseConnectionStrings. dev. config i nadaj jej nazwę databaseConnectionStrings. config.

> [!NOTE]
> Plik konfiguracji można nazwać coś innego niż databaseConnectionStrings. config, jeśli lubisz, na przykład `connectionStrings.config` lub `dbInfo.config`. Pamiętaj jednak, aby nazwać plik z rozszerzeniem `.config`, ponieważ pliki `.config` są domyślnie nieobsługiwane przez aparat ASP.NET. Jeśli nazwa pliku jest inna, np. `connectionStrings.txt`, użytkownik może wskazać, aby przeglądarka [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) i wyświetlić zawartość pliku!

W tym momencie folder `ConfigSections` powinien zawierać trzy pliki (zobacz rysunek 4). Pliki databaseConnectionStrings. dev. config i databaseConnectionStrings. Production. config zawierają odpowiednio parametry połączenia dla środowisk deweloperskich i produkcyjnych. Plik databaseConnectionStrings. config zawiera informacje o parametrach połączenia, które będą używane przez aplikację sieci Web w czasie wykonywania. W związku z tym plik databaseConnectionStrings. config powinien być taki sam jak plik databaseConnectionStrings. dev. config w środowisku deweloperskim, natomiast w przypadku produkcji plik databaseConnectionStrings. config powinien być taki sam jak databaseConnectionStrings. Product. config.

[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Rysunek 4**. ConfigSections ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))

Teraz musimy wydać `Web.config`, aby użyć pliku databaseConnectionStrings. config dla swojego magazynu parametrów połączenia. Otwórz `Web.config` i Zastąp istniejący element `<connectionStrings>` następującym:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

Atrybut `configSource` określa ścieżkę fizyczną względną do pliku `Web.config`. Jeśli zewnętrzny plik `.config` znajduje się w tym samym katalogu, co `Web.config` następnie ustaw ten atrybut na nazwę pliku `.config`. Jeśli znajduje się w podkatalogu, tak jak w przypadku databaseConnectionStrings. config, określ podfolder przy użyciu ukośnika odwrotnego, aby rozdzielić nazwy folderów i plików, takie jak ConfigSections\databaseConnectionStrings.config.

W przypadku tej modyfikacji środowiska deweloperskie i produkcyjne zawierają ten sam plik `Web.config`. Teraz jedyną różnicą jest plik databaseConnectionStrings. config. Skopiuj plik databaseConnectionStrings. Product. config do środowiska produkcyjnego i zmień jego nazwę na databaseConnectionStrings. config. Jeśli w przyszłości istnieją zmiany parametrów połączenia produkcyjnej bazy danych, należy je wprowadzić do pliku databaseConnectionStrings. Product. config, a następnie przekazać go do środowiska produkcyjnego, zmieniając nazwę pliku databaseConnectionStrings. config.

> [!NOTE]
> Możesz określić informacje dla każdego elementu `Web.config` w osobnym pliku i użyć atrybutu `configSource`, aby odwołać się do tego pliku z poziomu `Web.config`.

## <a name="summary"></a>Podsumowanie

Aplikacje oparte na danych zazwyczaj korzystają z różnych baz danych w środowiskach deweloperskich i produkcyjnych. W związku z tym parametry połączenia bazy danych przechowywane w konfiguracji aplikacji sieci Web muszą być unikatowe dla danego środowiska. W tym samouczku pokazano, jak określić parametry połączenia z produkcyjną bazą danych i sposoby zachować unikatowe informacje o parametrach połączenia w dwóch środowiskach.

Szczęśliwe programowanie!

#### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Parametry połączenia i pliki konfiguracji](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informacje o ciągach konfiguracji bazy danych @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Przenoszenie ustawień z pliku Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Dokumentacja techniczna elementu &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-database-cs.md)
> [dalej](configuring-a-website-that-uses-application-services-cs.md)

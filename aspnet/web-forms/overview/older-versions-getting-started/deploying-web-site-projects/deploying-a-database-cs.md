---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Wdrażanie bazy danych (C#) | Microsoft Docs
author: rick-anderson
description: Wdrożenie aplikacji sieci Web ASP.NET wymaga pobrania niezbędnych plików i zasobów ze środowiska programistycznego do środowiska produkcyjnego. Dla da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 83657be794e1ea31f6ad2f2b4adc274724d60cf2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638209"
---
# <a name="deploying-a-database-c"></a>Wdrażanie bazy danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Wdrożenie aplikacji sieci Web ASP.NET wymaga pobrania niezbędnych plików i zasobów ze środowiska programistycznego do środowiska produkcyjnego. W przypadku aplikacji sieci Web opartych na danych obejmuje to schemat bazy danych i dane. Ten samouczek jest pierwszą częścią serii, która bada kroki niezbędne do pomyślnego wdrożenia bazy danych ze środowiska programistycznego w środowisku produkcyjnym.

## <a name="introduction"></a>Wprowadzenie

Wdrożenie aplikacji sieci Web ASP.NET wymaga pobrania niezbędnych plików i zasobów ze środowiska programistycznego do środowiska produkcyjnego. W ciągu ostatnich sześciu samouczków oglądamy wdrażanie prostej aplikacji sieci Web. Ta witryna demonstracyjna obejmowała wiele zasobów po stronie serwera — ASP.NET stron, pliki konfiguracji, plik `Web.sitemap` i tak dalej — wraz z zasobami po stronie klienta, takimi jak obrazy i pliki CSS. Ale co z aplikacjami sieci Web opartymi na danych? Jakie dodatkowe kroki należy wykonać w celu wdrożenia aplikacji sieci Web, która korzysta z bazy danych?

W następnym samouczku opisano kroki niezbędne do wdrożenia aplikacji sieci Web opartej na danych. Ten samouczek rozpoczyna się od sprawdzenia, jak uzyskać schemat bazy danych i zawartość ze środowiska programistycznego w środowisku produkcyjnym, podczas gdy kolejny samouczek przegląda wymaganą zmianę konfiguracji. Poniżej przedstawiono informacje na temat problemów związanych z wdrażaniem bazy danych korzystającej z Usługi aplikacji (członkostwo, role, profil itd.).

## <a name="examining-the-updated-book-reviews-web-application"></a>Sprawdzanie zaktualizowanej aplikacji sieci Web przeglądów książki

Aby zademonstrować wdrożenie aplikacji sieci Web opartej na danych, Zaktualizowano aplikację sieci Web Recenzja książki z prostej, statycznej witryny sieci Web do jednego opartego na danych. Tak jak wcześniej, w tym samouczku s można pobrać dwie wersje aplikacji: jedną, która używa modelu projektu aplikacji sieci Web i jednego, który używa modelu projektu witryny sieci Web.

Aktualizacja aplikacji sieci Web, która przegląda książkę, używa bazy danych [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) , która jest przechowywana w folderze `App_Data` lokacji (`~/App_Data/Reviews.mdf`). Jeśli na komputerze zainstalowano SQL Server 2008, demonstracja powinna zostać uruchomiona bez błędu. Jeśli masz starszą wersję SQL Server możesz zainstalować bezpłatną SQL Server 2008 Express Edition lub użyć skryptów bazy danych dostępnych w tym samouczku, aby samodzielnie utworzyć bazę danych.

Baza danych `Reviews.mdf` zawiera cztery tabele:

- `Genres` — zawiera rekord dla każdego gatunku, taki jak technologia, fikcja i firma.
- `Books` — zawiera rekord dla każdego przeglądu, z kolumnami, takimi jak `Title`, `GenreId`, `ReviewDate`i `Review`, między innymi.
- `Authors` — zawiera informacje o każdym autorze, który utworzył zrecenzowaną książkę.
- `BooksAuthors` — tabela sprzężenia wiele-do-wielu określająca, które autorów zapisują książki.

Rysunek 1 przedstawia diagram ER tych czterech tabel.

[![baza danych aplikacji sieci Web w programie Books składa się z czterech tabel](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Rysunek 1**. książka przegląda bazę danych aplikacji sieci Web, składającą się z czterech tabel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image3.jpg))

Poprzednia wersja witryny sieci Web przeglądający książki miała osobną stronę ASP.NET dla każdej książki. Na przykład wykryto stronę o nazwie `~/Tech/TYASP35.aspx`, która zawiera przegląd *ASP.NET 3,5 w ciągu 24 godzin*. Ta nowa wersja witryny sieci Web oparta na danych zawiera przeglądy przechowywane w bazie danych i pojedynczej stronie ASP.NET, przeglądanie. aspx? ID =*bookId*, który wyświetla przegląd określonej książki. Podobnie istnieje strona o nazwie gatunek. aspx? ID =*genreId* , która zawiera listę zrecenzowanych ksiąg w określonym gatunku.

Rysunki 2 i 3 pokazują `Genre.aspx` i `Review.aspx` strony w akcji. Zwróć uwagę na adres URL na pasku adresu dla każdej strony. Na rysunku 2 to s gatunek. aspx? ID = 85d164ba-1123-4c47-82a0-c8ec75de7e0e. Ponieważ 85d164ba-1123-4c47-82a0-c8ec75de7e0e jest wartością `GenreId` dla gatunku technologii, w nagłówku strony są odczytywane "Przeglądy technologii", a lista punktowana wylicza te przeglądy w witrynie, które znajdują się pod tym gatunkiem.

[![strony gatunek technologii](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Rysunek 2**: Strona gatunek technologii ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image6.jpg))

[![przegląd, aby nauczyć sobie ASP.NET 3,5 w ciągu 24 godzin](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Rysunek 3**: przegląd do *nauki sobie ASP.NET 3,5 w ciągu 24 godzin* ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image9.jpg))

Aplikacja sieci Web przegląda książkę zawiera również sekcję administracyjną, w której administratorzy mogą dodawać, edytować i usuwać gatunki, przeglądy i informacje o autorze. Obecnie każdy użytkownik może uzyskać dostęp do sekcji Administracja. W przyszłym samouczku dodamy obsługę kont użytkowników i zezwolisz użytkownikom na dostęp tylko dla autoryzowanych użytkowników.

W przypadku pobrania aplikacji do przeglądu książek należy pamiętać, że celem jest zademonstrowanie wdrożenia aplikacji opartej na danych. Nie wykazuje najlepszych praktyk w zakresie projektowania aplikacji. Na przykład nie istnieje osobna warstwa dostępu do danych (DAL); strony ASP.NET komunikują się bezpośrednio z bazą danych za pomocą kontrolki kontrolki SqlDataSource lub kodu ADO.NET w swoich klasach znajdujących się w kodzie. Aby uzyskać bardziej szczegółowe informacje na temat tworzenia aplikacji opartych na danych przy użyciu architektury warstwowej, zobacz moje [ *prace z samouczkami dotyczącymi danych* ](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bazy danych na etapie programowania i produkcji

Po rozpoczęciu programowania w aplikacji sieci Web opartej na danych należy określić parametry połączenia z bazą danych, która zawiera szczegółowe informacje dotyczące sposobu łączenia się z bazą danych. Te parametry połączenia określają między innymi, serwer bazy danych, nazwę bazy danych i informacje o zabezpieczeniach. Najczęściej baza danych używana przez aplikację podczas opracowywania różni się od bazy danych używanej w środowisku produkcyjnym. Istnieje wiele korzyści z używania różnych baz danych do programowania i produkcji. Inna baza danych w programowaniu oznacza, że nie trzeba martwić się o przypadkowe modyfikacje lub usunięcie danych na żywo. Umożliwia także umieszczanie danych fikcyjnych testów lub wprowadzanie istotnych zmian w modelu danych bez konieczności przejmowania się wpływem na działanie aplikacji w środowisku produkcyjnym. Minusem istnienia innej bazy danych w środowiskach programistycznych i produkcyjnych polega na tym, że po wdrożeniu aplikacji baza danych programu i wszelkie istotne zmiany schematu lub danych bazy danych muszą również zostać wdrożone.

Przed pierwszym wdrożeniem istnieje tylko jedno wystąpienie bazy danych, a to wystąpienie znajduje się w środowisku deweloperskim. Podczas wdrażania aplikacji w środowisku produkcyjnym po raz pierwszy nie należy kopiować wymaganych plików po stronie serwera i po stronie klienta, ale również kopiować bazę danych ze środowiska deweloperskiego do środowiska produkcyjnego. Jest to miejsce, w którym już teraz korzystamy z aplikacji sieci Web przeglądający książki — baza danych znajduje się w folderze `App_Data` w naszym środowisku programistycznym, ale nie została jeszcze wypchnięci do środowiska produkcyjnego.

Po wdrożeniu aplikacji istnieją dwie kopie bazy danych. W miarę dojrzewania aplikacji mogą zostać dodane nowe funkcje, co wymaga zmiany w modelu danych (np. dodanie nowych kolumn do istniejących tabel, wprowadzanie zmian w istniejących kolumnach, dodawanie nowych tabel itd.). Po wdrożeniu aplikacji sieci Web zmiany zastosowane do bazy danych w środowisku programistycznym od momentu ostatniego wdrożenia muszą zostać zastosowane do produkcyjnej bazy danych programu. Niektóre strategie zarządzania tym procesem zostały omówione w przyszłym samouczku. Ten samouczek koncentruje się na wdrażaniu całej bazy danych ze środowiska deweloperskiego w środowisku produkcyjnym.

## <a name="deploying-the-database-to-the-production-environment"></a>Wdrażanie bazy danych w środowisku produkcyjnym

W pozostałej części tego samouczka pokazano, jak wdrożyć bazę danych ze środowiska programistycznego w środowisku produkcyjnym. Jeśli jesteś następnym, musisz się upewnić, że konto z dostawcą hosta sieci Web zawiera Microsoft SQL Server obsługi bazy danych. Konieczne będzie również posiadanie pewnych informacji, takich jak nazwa serwera bazy danych, nazwa bazy danych oraz nazwa użytkownika i hasło używane do nawiązania połączenia z bazą danych.

Jak wspomniano wcześniej w tym samouczku, baza danych witryny sieci Web programu Reviewing Books jest bazą danych SQL Server 2008 Express Edition przechowywaną w folderze `App_Data`. Mogłoby to spowodować, że wdrożenie takiej bazy danych byłoby proste, jak w przypadku kopiowania folderu `App_Data` ze środowiska deweloperskiego do środowiska produkcyjnego. Większość dostawców hosta sieci Web nie obsługuje jednak hostingu baz danych w folderze `App_Data` ze względów bezpieczeństwa. Zamiast tego hosty sieci Web udostępniają konto na serwerze bazy danych SQL Server w środowisku. Wdrożenie bazy danych ze środowiska deweloperskiego w środowisku produkcyjnym wymaga zarejestrowania bazy danych na serwerze bazy danych hosta sieci Web.

Jak uzyskać bazę danych ze środowiska programistycznego w środowisku produkcyjnym? Istnieje kilka sposobów, aby to zrobić, w zależności od tego, jakie usługi oferuje Twój host sieci Web. Z niektórymi hostami, takimi jak DiscountASP.NET, można za pomocą usługi FTP utworzyć kopię zapasową bazy danych lub `.mdf` pliku w witrynie sieci Web, a następnie w panelu sterowania przywrócić plik kopii zapasowej lub dołączyć plik `.mdf` do serwera bazy danych SQL Server. Dzięki tym narzędziom wdrażanie bazy danych jest tak proste jak skopiowanie folderu `App_Data` do środowiska produkcyjnego, a następnie dołączenie go za pomocą panelu sterowania. Jest to prawdopodobnie najłatwiejszym i najszybszym sposobem na opublikowanie bazy danych po raz pierwszy.

Innym rozwiązaniem jest użycie Kreatora publikacji bazy danych. Kreator publikowania bazy danych to aplikacja klasyczna systemu Windows, która generuje polecenia SQL w celu utworzenia schematu bazy danych — tabele, procedury składowane, widoki, funkcje zdefiniowane przez użytkownika i tak dalej — i opcjonalnie dane w swoich tabelach. Następnie można nawiązać połączenie z serwerem bazy danych dostawcy hosta sieci Web za pomocą SQL Server Management Studio a następnie wykonać ten skrypt w celu zduplikowania bazy danych w środowisku produkcyjnym. Jeszcze lepiej, jeśli dostawca hosta sieci Web obsługuje [usługi publikowania baz danych](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) firmy Microsoft, skrypt wygenerowany przez Kreatora publikacji bazy danych jest automatycznie wykonywany na serwerze bazy danych w Twoim imieniu. Ponieważ Kreator publikowania bazy danych generuje skrypt, który tworzy schemat bazy danych i dane, będzie działał niezależnie od tego, czy dostawca hosta sieci Web oferuje funkcje, takie jak dołączanie przekazanego pliku `.mdf`.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generowanie poleceń SQL w celu utworzenia schematu bazy danych i danych przy użyciu Kreatora publikacji bazy danych

Pozwól, aby przeanalizować bazę danych przeglądów książki w środowisku produkcyjnym za pomocą Kreatora publikowania bazy danych. Jeśli używasz programu Visual Studio 2008 lub więcej, Kreator publikowania bazy danych jest już zainstalowany. W przypadku korzystania z programu Visual Studio 2005 należy najpierw [pobrać i zainstalować](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) kreatora.

Otwórz program Visual Studio i przejdź do bazy danych `Reviews.mdf`. Jeśli używasz programu Visual Web Developer, przejdź do Eksplorator bazy danych; Jeśli używasz programu Visual Studio, użyj Eksplorator serwera. Na rysunku 4 przedstawiono bazę danych `Reviews.mdf` w Eksplorator bazy danych w programie Visual Web Developer. Jak pokazano na rysunku 4, baza danych `Reviews.mdf` składa się z czterech tabel, trzech procedur składowanych i funkcji zdefiniowanej przez użytkownika.

[![zlokalizować bazy danych w Eksplorator bazy danych lub Eksplorator serwera](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Rysunek 4**. Odszukaj bazę danych w Eksplorator bazy danych lub Eksplorator serwera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image12.jpg))

Kliknij prawym przyciskiem myszy nazwę bazy danych i wybierz opcję "Publikuj w dostawcy" z menu kontekstowego. Spowoduje to uruchomienie Kreatora publikacji bazy danych (patrz rysunek 5). Kliknij przycisk Dalej, aby przejść do poprzedniego ekranu powitalnego.

[![ekranu powitalnego Kreatora publikacji bazy danych](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Rysunek 5**. ekran powitalny Kreatora publikacji bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image15.jpg))

Drugi ekran kreatora zawiera listę baz danych dostępnych dla Kreatora publikacji bazy danych i pozwala określić, czy mają być wyświetlane skrypty wszystkich obiektów w wybranej bazie danych, czy też do wybierania obiektów do skryptu. Wybierz odpowiednią bazę danych i pozostaw zaznaczone pole wyboru "skrypt wszystkie obiekty w wybranej bazie danych".

> [!NOTE]
> Jeśli zostanie wyświetlony komunikat o błędzie "nie ma obiektów w bazie danych *DatabaseName* typów, które mogą być inicjowane przez ten kreator" po kliknięciu przycisku Dalej na rysunku 6, upewnij się, że ścieżka do pliku bazy danych nie jest zbyt długa. Wykryto, że ten błąd może wystąpić, jeśli ścieżka do pliku bazy danych jest za długa.

[![ekranu powitalnego Kreatora publikacji bazy danych](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Ilustracja 6**. ekran powitalny Kreatora publikacji bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image18.jpg))

Na następnym ekranie można wygenerować plik skryptu lub, jeśli obsługuje go host sieci Web, opublikować bazę danych bezpośrednio na serwerze bazy danych dostawcy hosta sieci Web. Jak pokazano na rysunku 7, mam skrypt zapisany w pliku `C:\REVIEWS.MDF.sql`.

[![skrypt bazy danych do pliku lub publikowanie go bezpośrednio dla dostawcy hosta sieci Web](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Rysunek 7**. wykonywanie skryptu bazy danych do pliku lub publikowanie jej bezpośrednio dla dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image21.jpg))

Na następnym ekranie zostanie wyświetlony komunikat z wieloma opcjami tworzenia skryptów. Można określić, czy skrypt powinien zawierać instrukcje Drop, aby usunąć te istniejące obiekty. Ta wartość domyślna to true, co jest bardzo precyzyjne podczas wdrażania bazy danych po raz pierwszy. Możesz również określić, czy docelowa baza danych ma SQL Server 2000, SQL Server 2005 czy SQL Server 2008. Na koniec możesz wskazać, czy ma być skryptować schemat i dane, tylko dane, czy tylko schemat. Schemat jest kolekcją obiektów bazy danych, tabel, procedur składowanych, widoków itd. Dane są zawarte w tabelach.

Jak pokazano na rysunku 8, mam kreatora skonfigurowany do usuwania istniejących obiektów bazy danych, generowania skryptu dla bazy danych SQL Server 2008 i publikowania zarówno schematu, jak i danych.

[![określić opcje publikowania](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Ilustracja 8**. Określanie opcji publikowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](deploying-a-database-cs/_static/image24.jpg))

Ostatnie dwa ekrany podsumowują akcje, które mają zostać wykonane, a następnie wyświetlają stan skryptów. Wynikiem działania kreatora jest plik skryptu, który zawiera polecenia SQL, które są niezbędne do utworzenia bazy danych w środowisku produkcyjnym, i wypełnienie go tymi samymi danymi co na etapie projektowania.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Wykonywanie poleceń SQL w bazie danych środowiska produkcyjnego

Teraz, gdy mamy skrypt, który zawiera polecenia SQL służące do tworzenia bazy danych i jej wszystkich danych, które pozostaną, są wykonywane w produkcyjnej bazie danych. Niektórzy dostawcy hosta sieci Web oferują w panelu sterowania pole tekstowe, w którym można wprowadzać polecenia SQL do wykonania w bazie danych. Jeśli masz bardzo duży plik skryptu, ta opcja może nie działać (na przykład rozmiar pliku skryptu `REVIEWS.MDF.sql` przekracza 425 KB).

Lepszym rozwiązaniem jest bezpośrednie połączenie z serwerem produkcyjnej bazy danych przy użyciu programu SQL Server Management Studio (SSMS). Jeśli na komputerze jest zainstalowana wersja nieexpress SQL Server, najkorzystniej jest już zainstalowany program SSMS. W przeciwnym razie możesz [pobrać i zainstalować](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) bezpłatną kopię programu SQL Server Management Studio Express Edition.

Uruchom program SSMS i Połącz się z serwerem bazy danych hosta sieci Web, korzystając z informacji podanych przez dostawcę hosta sieci Web.

[![połączyć się z serwerem bazy danych dostawcy hosta sieci Web](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Rysunek 9**. Łączenie się z serwerem bazy danych dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image27.jpg))

Rozwiń kartę bazy danych i Znajdź bazę danych. Kliknij przycisk Nowa kwerenda w lewym górnym rogu paska narzędzi, Wklej w poleceniach SQL z pliku skryptu utworzonego przez Kreatora publikacji bazy danych, a następnie kliknij przycisk Wykonaj, aby uruchomić te polecenia na serwerze produkcyjnej bazy danych. Jeśli plik skryptu jest szczególnie duży, wykonanie poleceń może potrwać kilka minut.

[![połączyć się z serwerem bazy danych dostawcy hosta sieci Web](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Rysunek 10**. Łączenie się z serwerem bazy danych dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image30.jpg))

Wszystko, co wszystko jest gotowe! W tym momencie baza danych programu Development została zduplikowana w środowisku produkcyjnym. W przypadku odświeżenia bazy danych w programie SSMS powinny zostać wyświetlone nowe obiekty bazy danych. Rysunek 11 przedstawia produkcyjne tabele, procedury składowane i funkcje zdefiniowane przez użytkownika, które są dublowane w bazie danych programistycznych. Ze względu na to, że nastąpiło opublikowanie danych przez Kreatora publikacji bazy danych, tabele produkcyjnej bazy danych mają takie same dane jak tabele programistycznej bazy danych w czasie, gdy Kreator został wykonany. Rysunek 12 przedstawia dane w tabeli `Books` w produkcyjnej bazie danych.

[![obiekty bazy danych zostały zduplikowane w produkcyjnej bazie danych](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Ilustracja 11**. obiekty bazy danych zostały zduplikowane w produkcyjnej bazie danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image33.jpg))

[![produkcyjna baza danych zawiera te same dane co w przypadku programu Development Database](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Ilustracja 12**. produkcyjna baza danych zawiera te same dane, co w przypadku programu Development Database ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-a-database-cs/_static/image36.jpg))

W tym momencie wdrożono tylko programistyczną bazę danych w środowisku produkcyjnym. Nie przeprowadzono jeszcze wdrożenia samej aplikacji sieci Web lub Zbadaj, jakie zmiany konfiguracji są potrzebne, aby aplikacja w środowisku produkcyjnym korzystała z produkcyjnej bazy danych. Będziemy omawiać te problemy w następnym samouczku.

## <a name="summary"></a>Podsumowanie

Wdrożenie aplikacji sieci Web opartej na danych wymaga kopiowania bazy danych używanej podczas programowania do środowiska produkcyjnego. Wielu dostawców hosta sieci Web oferuje narzędzia upraszczające proces wdrażania bazy danych. Na przykład za pomocą DiscountASP.NET można `.mdf` FTP pliku bazy danych (lub kopii zapasowej), a następnie dołączyć bazę danych do serwera bazy danych z panelu sterowania. Inna opcja, która działa niezależnie od tego, jakie funkcje są oferowane przez dostawcę hosta sieci Web, to narzędzie Kreatora publikacji bazy danych firmy Microsoft, które generuje skrypt poleceń SQL, aby utworzyć schemat i dane programistycznej bazy danych. Po wygenerowaniu tego skryptu można wykonać go w produkcyjnej bazie danych.

Teraz, gdy książka przegląda bazę danych aplikacji sieci Web w środowisku produkcyjnym, możemy wdrożyć aplikację. Informacje o konfiguracji aplikacji sieci Web s określają jednak parametry połączenia z bazą danych, a parametry połączenia odwołują się do bazy danych programistycznych. Należy zaktualizować te informacje o parametrach połączenia podczas wdrażania lokacji w środowisku produkcyjnym. W następnym samouczku pokazano, jak te różnice w konfiguracji, i przedstawiono kroki wymagane do opublikowania witryny przeglądy książki opartej na danych w środowisku produkcyjnym.

Szczęśliwe programowanie!

#### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Pobierz Kreatora publikacji bazy danych Microsoft SQL Server 1,1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Pobierz Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Poprzednie](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [dalej](configuring-the-production-web-application-to-use-the-production-database-cs.md)

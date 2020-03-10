---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Przekazywanie plików (C#) | Microsoft Docs
author: rick-anderson
description: Dowiedz się, jak zezwolić użytkownikom na przekazywanie plików binarnych (takich jak dokumenty programu Word lub PDF) do witryny sieci Web, gdzie mogą być przechowywane w systemie plików serwera...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e3e32a829de386a681504c8d5d61dd258b8b2e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548798"
---
# <a name="uploading-files-c"></a>Przekazywanie plików (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) lub [Pobierz plik PDF](uploading-files-cs/_static/datatutorial54cs1.pdf)

> Dowiedz się, jak umożliwić użytkownikom przekazywanie plików binarnych (takich jak dokumenty programu Word lub PDF) do witryny sieci Web, gdzie mogą być przechowywane w systemie plików serwera lub w bazie danych programu.

## <a name="introduction"></a>Wprowadzenie

Wszystkie samouczki, które chcielibyśmy zbadać, są przeznaczone wyłącznie dla danych tekstowych. Jednak wiele aplikacji ma modele danych, które przechwytują dane tekstowe i binarne. Witryna Datowanie online może zezwalać użytkownikom na przekazywanie zdjęć w celu skojarzenia ich z ich profilem. Witryna internetowa rekrutacji może pozwolić użytkownikom na przekazywanie ich życiorysów jako dokumentu programu Microsoft Word lub PDF.

Praca z danymi binarnymi dodaje nowy zestaw wyzwań. Należy zdecydować, jak dane binarne są przechowywane w aplikacji. Interfejs używany do wstawiania nowych rekordów należy zaktualizować, aby umożliwić użytkownikowi przekazanie pliku z komputera, a dodatkowe czynności muszą zostać wykonane, aby wyświetlić lub udostępnić metodę pobierania rekordu s skojarzonych danych binarnych. W tym samouczku i następnych trzech zapoznajemy się z tym, jak obsłużyć te wyzwania. Na końcu tych samouczków zostanie utworzona w pełni funkcjonalna aplikacja, która kojarzy obraz i broszurę PDF z każdą kategorią. W tym konkretnym samouczku Przyjrzyjmy się różnym technikom do przechowywania danych binarnych i Dowiedz się, jak umożliwić użytkownikom przekazywanie plików z ich komputerów i zapisywanie ich w systemie plików serwera sieci Web.

> [!NOTE]
> Dane binarne, które są częścią modelu danych aplikacji, są czasami określane jako [obiekt BLOB](http://en.wikipedia.org/wiki/Binary_large_object), akronim dla dużego obiektu binarnego. W tych samouczkach zostały wybrane użycie danych binarnych terminologii, chociaż termin BLOB jest synonimem.

## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Krok 1. Tworzenie pracy z danymi binarnymi na stronach sieci Web

Przed rozpoczęciem eksplorowania wyzwań związanych z dodaniem obsługi danych binarnych Poświęć chwilę na utworzenie stron ASP.NET w naszym projekcie witryny sieci Web, które będziemy potrzebować do tego samouczka i trzech następnych. Zacznij od dodania nowego folderu o nazwie `BinaryData`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`

![Dodaj strony ASP.NET dla samouczków dotyczących danych binarnych](uploading-files-cs/_static/image1.gif)

**Rysunek 1**. dodawanie stron ASP.NET dla samouczków dotyczących danych binarnych

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `BinaryData` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym należy dodać tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-cs/_static/image2.png))

Na koniec Dodaj te strony jako wpisy do pliku `Web.sitemap`. Przed zwiększeniem `<siteMapNode>`GridView należy dodać następujące znaczniki:

[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do pracy z samouczkami danych binarnych.

![Mapa witryny zawiera teraz wpisy dotyczące pracy z samouczkami danych binarnych](uploading-files-cs/_static/image3.gif)

**Rysunek 3**. Mapa witryny zawiera teraz wpisy dotyczące pracy z samouczkami danych binarnych

## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Krok 2. decydowanie o miejscu przechowywania danych binarnych

Dane binarne skojarzone z modelem danych aplikacji mogą być przechowywane w jednym z dwóch miejsc: w systemie plików serwera sieci Web z odwołaniem do pliku przechowywanego w bazie danych programu; lub bezpośrednio w samej bazie danych (zobacz rysunek 4). Każde podejście ma swój własny zestaw informatyków i wad i zawiera bardziej szczegółowe omówienie.

[![dane binarne mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Rysunek 4**. dane binarne mogą być przechowywane w systemie plików lub bezpośrednio w bazie danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](uploading-files-cs/_static/image4.png))

Załóżmy, że chcemy zwiększyć bazę danych Northwind, aby skojarzyć obraz z każdym produktem. Jedną z opcji jest zapisanie tych plików obrazów w systemie plików serwera sieci Web i zapisanie ścieżki w tabeli `Products`. W tym podejściu d dodajemy kolumnę `ImagePath` do tabeli `Products` typu `varchar(200)`, być może. Gdy użytkownik przekazał zdjęcie do Chai, ten obraz może być przechowywany w systemie plików s serwera sieci Web w `~/Images/Tea.jpg`, gdzie `~` reprezentuje ścieżkę fizyczną aplikacji. Oznacza to, że jeśli witryna sieci Web jest odblokowana przy użyciu ścieżki fizycznej `C:\Websites\Northwind\`, `~/Images/Tea.jpg` byłoby równoważne `C:\Websites\Northwind\Images\Tea.jpg`. Po przekazaniu pliku obrazu firma Microsoft aktualizuje rekord Chai w tabeli `Products` tak, aby jej `ImagePath` kolumna odwołuje się do ścieżki nowego obrazu. Możemy użyć `~/Images/Tea.jpg` lub tylko `Tea.jpg`, jeśli postanowiono, że wszystkie obrazy produktów zostaną umieszczone w folderze `Images` aplikacji.

Główne zalety przechowywania danych binarnych w systemie plików są następujące:

- **Łatwość wdrażania** , ponieważ wkrótce zobaczymy, przechowywanie i pobieranie danych binarnych przechowywanych bezpośrednio w bazie danych programu obejmuje nieco więcej kodu niż podczas pracy z danymi w systemie plików. Ponadto, aby użytkownik mógł wyświetlać lub pobierać dane binarne, muszą być przedstawiane przy użyciu adresu URL dla tych danych. Jeśli dane znajdują się w systemie plików serwera sieci Web, adres URL jest prosty. Jeśli dane są przechowywane w bazie danych programu, należy utworzyć stronę sieci Web, która będzie pobierać i zwracać dane z bazy danych.
- **Szerszy dostęp do danych binarnych** może być konieczne udostępnienie danych binarnych innym usługom lub aplikacjom, które nie mogą pobierać danych z bazy danych. Na przykład obrazy skojarzone z każdym produktem mogą również być dostępne dla użytkowników za pomocą [protokołu FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), w tym przypadku d chcemy przechowywać dane binarne w systemie plików.
- **Wydajność** Jeśli dane binarne są przechowywane w systemie plików, Przeciążenie popytu i sieci między serwerem bazy danych i serwerem sieci Web będzie mniejsze niż w przypadku przechowywania danych binarnych bezpośrednio w bazie danych.

Główną wadą przechowywania danych binarnych w systemie plików jest oddzielenie danych z bazy danych. Jeśli rekord zostanie usunięty z tabeli `Products`, skojarzony plik w systemie plików serwera sieci Web nie zostanie automatycznie usunięty. Musimy napisać dodatkowy kod, aby usunąć plik lub system plików stanie się nieużywanymi, oddzielonymi plikami. Ponadto podczas tworzenia kopii zapasowej bazy danych należy koniecznie utworzyć kopie zapasowe skojarzonych danych binarnych w systemie plików. Przeniesienie bazy danych do innej lokacji lub serwera stwarza podobne wyzwania.

Alternatywnie dane binarne mogą być przechowywane bezpośrednio w bazie danych Microsoft SQL Server 2005 przez utworzenie kolumny typu `varbinary`. Podobnie jak w przypadku innych typów danych o zmiennej długości, można określić maksymalną długość danych binarnych, które mogą być przechowywane w tej kolumnie. Na przykład, aby zarezerwować maksymalnie 5 000 bajtów, użyj `varbinary(5000)`; `varbinary(MAX)` pozwala na maksymalny rozmiar magazynu (około 2 GB).

Główną zaletą przechowywania danych binarnych bezpośrednio w bazie danych jest ścisłe sprzężenie między danymi binarnymi i rekordem bazy danych. Znacznie upraszcza to zadania zarządzania bazami danych, takie jak kopie zapasowe lub przeniesienie bazy danych do innej lokacji lub serwera programu. Ponadto usunięcie rekordu powoduje automatyczne usunięcie odpowiednich danych binarnych. Istnieją również bardziej subtelne zalety przechowywania danych binarnych w bazie danych. Więcej szczegółowych informacji można znaleźć w artykule [przechowywanie plików binarnych bezpośrednio w bazie danych przy użyciu ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) .

> [!NOTE]
> W Microsoft SQL Server 2000 i starszych wersjach typ danych `varbinary` miał maksymalny limit 8 000 bajtów. Aby przechowywać do 2 GB danych binarnych, należy zamiast tego użyć [typu danych`image`](https://msdn.microsoft.com/library/ms187993.aspx) . Jednak dodanie `MAX` w SQL Server 2005 oznacza, że typ danych `image` został uznany za przestarzały. Jest ona nadal obsługiwana w celu zapewnienia zgodności z poprzednimi wersjami, ale firma Microsoft ogłosiła, że typ danych `image` zostanie usunięty w przyszłych wersjach SQL Server.

W przypadku korzystania z starszego modelu danych może zostać wyświetlony `image` typ danych. Tabela `Categories` bazy danych Northwind zawiera kolumnę `Picture`, która może służyć do przechowywania danych binarnych pliku obrazu dla kategorii. Ponieważ baza danych Northwind ma swoje elementy główne w programie Microsoft Access i starszych wersjach SQL Server, ta kolumna jest typu `image`.

Na potrzeby tego samouczka i następnych trzech będziemy używać obu tych metod. Tabela `Categories` ma już kolumnę `Picture` do przechowywania zawartości binarnej obrazu dla kategorii. Dodamy dodatkową kolumnę `BrochurePath`, aby przechowywać ścieżkę do pliku PDF w systemie plików serwera sieci Web, który może służyć do zapewnienia jakości wydruku i dopracowanych przeglądów kategorii.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Krok 3. Dodawanie kolumny`BrochurePath`do tabeli`Categories`

Obecnie tabela kategorii ma tylko cztery kolumny: `CategoryID`, `CategoryName`, `Description`i `Picture`. Oprócz tych pól należy dodać nową, która będzie wskazywała broszurę kategorii s (jeśli taka istnieje). Aby dodać tę kolumnę, przejdź do Eksplorator serwera, przechodzenie do szczegółów tabel, kliknij prawym przyciskiem myszy tabelę `Categories` i wybierz Otwórz definicję tabeli (patrz rysunek 5). Jeśli nie widzisz Eksplorator serwera, przenieś ją, wybierając opcję Eksplorator serwera z menu Widok lub naciśnij klawisze CTRL + ALT + S.

Dodaj nową kolumnę `varchar(200)` do tabeli `Categories` o nazwie `BrochurePath` i umożliwi `NULL` s i kliknij ikonę Zapisz (lub naciśnij klawisze CTRL + S).

[![dodać kolumny BrochurePath do tabeli Categories](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Rysunek 5**. dodawanie kolumny `BrochurePath` do tabeli `Categories` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](uploading-files-cs/_static/image6.png))

## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Krok 4. aktualizowanie architektury w celu używania kolumn`Picture`i`BrochurePath`

`CategoriesDataTable` w warstwie dostępu do danych (DAL) ma obecnie zdefiniowaną cztery `DataColumn` s: `CategoryID`, `CategoryName`, `Description`i `NumberOfProducts`. Po pierwotnie zaprojektowaniu tego elementu DataTable w samouczku [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) `CategoriesDataTable` miał tylko trzy pierwsze kolumny; kolumna `NumberOfProducts` została dodana w bazie danych [master/detail przy użyciu listy punktowanej rekordów głównych z szczegółowym samouczkiem elementu DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) .

Zgodnie z opisem w temacie *Tworzenie warstwy dostępu do danych*, dane tabel w określonym zestawie danych tworzą obiekty biznesowe. TableAdapters są odpowiedzialne za komunikowanie się z bazą danych i wypełnianie obiektów firmy wynikami zapytania. `CategoriesDataTable` jest wypełniana przez `CategoriesTableAdapter`, która ma trzy metody pobierania danych:

- `GetCategories()` wykonuje głównego zapytania TableAdapter s i zwraca pola `CategoryID`, `CategoryName`i `Description` wszystkich rekordów w tabeli `Categories`. Główne zapytanie jest używane przez automatycznie generowane metody `Insert` i `Update`.
- `GetCategoryByCategoryID(categoryID)` zwraca pola `CategoryID`, `CategoryName`i `Description` kategorii, których `CategoryID` jest równa *IDkategorii*.
- `GetCategoriesAndNumberOfProducts()` — zwraca pola `CategoryID`, `CategoryName`i `Description` dla wszystkich rekordów w tabeli `Categories`. Używa również podzapytania, aby zwrócić liczbę produktów skojarzonych z każdą kategorią.

Zwróć uwagę, że żadna z tych zapytań nie zwróci kolumn `Categories` tabeli `Picture` lub `BrochurePath` nie `CategoriesDataTable` `DataColumn` s dla tych pól. Aby można było korzystać z właściwości obrazu i `BrochurePath`, należy najpierw dodać je do `CategoriesDataTable` a następnie zaktualizować klasę `CategoriesTableAdapter`, aby zwracała te kolumny.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Dodawanie`Picture`i`BrochurePath``DataColumn` s

Zacznij od dodania tych dwóch kolumn do `CategoriesDataTable`. Kliknij prawym przyciskiem myszy nagłówek `CategoriesDataTable` s, wybierz polecenie Dodaj z menu kontekstowego, a następnie wybierz opcję kolumny. Spowoduje to utworzenie nowego `DataColumn` w elemencie DataTable o nazwie `Column1`. Zmień nazwę tej kolumny na `Picture`. Na okno Właściwości ustaw właściwość `DataType` `DataColumn` s na `System.Byte[]` (nie jest to opcja na liście rozwijanej, należy wpisać ją w).

[![utworzyć elementu DataColumn o nazwie Picture, którego typem danych jest system. Byte []](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Ilustracja 6**. Utwórz `DataColumn` o nazwie `Picture`, której `DataType` jest `System.Byte[]` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-cs/_static/image8.png))

Dodaj kolejne `DataColumn` do elementu DataTable, nadając mu nazwę `BrochurePath` przy użyciu domyślnej wartości `DataType` (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Zwracanie wartości`Picture`i`BrochurePath`z TableAdapter

Po dodaniu tych dwóch `DataColumn` s do `CategoriesDataTable`, będziemy gotowi do zaktualizowania `CategoriesTableAdapter`. Możliwe, że obie te wartości kolumn zostały zwrócone w głównym zapytaniu TableAdapter, ale spowoduje to przywrócenie danych binarnych przy każdej wywołaniu metody `GetCategories()`. Zamiast tego należy zaktualizować główne zapytanie TableAdapter, aby przywrócić `BrochurePath` i utworzyć dodatkową metodę pobierania danych zwracającą określoną kolumnę `Picture` kategorii.

Aby zaktualizować główne zapytanie TableAdapter, kliknij prawym przyciskiem myszy nagłówek `CategoriesTableAdapter` s i wybierz opcję Konfiguruj z menu kontekstowego. Spowoduje to wyświetlenie Kreatora konfiguracji karty spisu, który został zaobserwowany w wielu wcześniejszych samouczkach. Zaktualizuj zapytanie, aby przywrócić `BrochurePath` i kliknij przycisk Zakończ.

[![zaktualizować listę kolumn w instrukcji SELECT, aby zwracała również BrochurePath](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Rysunek 7**. Aktualizacja listy kolumn w instrukcji `SELECT`, aby zwracała również `BrochurePath` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-cs/_static/image10.png))

W przypadku używania instrukcji SQL ad hoc dla TableAdapter, aktualizowanie listy kolumn w zapytaniu głównym aktualizuje listę kolumn dla wszystkich metod zapytań `SELECT` w TableAdapter. Oznacza to, że metoda `GetCategoryByCategoryID(categoryID)` została zaktualizowana w celu zwrócenia kolumny `BrochurePath`, co może być zamierzone. Jednak Zaktualizowano także listę kolumn w metodzie `GetCategoriesAndNumberOfProducts()`, usuwając podzapytanie zwracające liczbę produktów dla każdej kategorii. W związku z tym musimy zaktualizować tę metodę s `SELECT` Query. Kliknij prawym przyciskiem myszy metodę `GetCategoriesAndNumberOfProducts()`, wybierz polecenie Konfiguruj i Przywróć pierwotną wartość zapytania `SELECT`:

[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

Następnie utwórz nową metodę TableAdapter, która zwraca określoną kategorię `Picture` wartość kolumny. Kliknij prawym przyciskiem myszy nagłówek `CategoriesTableAdapter` s i wybierz opcję Dodaj zapytanie, aby uruchomić Kreatora konfiguracji zapytania TableAdapter. Pierwszy krok tego kreatora prosi nas, jeśli chcemy wysyłać zapytania o dane przy użyciu instrukcji SQL ad hoc, nowej procedury składowanej lub istniejącej. Wybierz pozycję Użyj instrukcji SQL i kliknij przycisk Dalej. Ponieważ zwrócimy wiersz, wybierz opcję Wybierz, która zwraca wiersze z drugiego kroku.

[![wybrać opcję Użyj instrukcji SQL](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Ilustracja 8**. Wybierz opcję Użyj instrukcji SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-cs/_static/image12.png))

[![, ponieważ zapytanie zwróci rekord z tabeli Categories, wybierz pozycję Zaznacz, która zwraca wiersze](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Ilustracja 9**. ponieważ zapytanie zwróci rekord z tabeli Categories, wybierz pozycję Zaznacz, która zwraca wiersze ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](uploading-files-cs/_static/image14.png))

W trzecim kroku wprowadź następujące zapytanie SQL i kliknij przycisk Dalej:

[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

Ostatnim krokiem jest wybranie nazwy nowej metody. Użyj `FillCategoryWithBinaryDataByCategoryID` i `GetCategoryWithBinaryDataByCategoryID` do wypełnienia elementu DataTable i zwrócenia odpowiednio wzorców DataTable. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![wybrać nazwy metod TableAdapter](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Ilustracja 10**. Wybieranie nazw metod TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](uploading-files-cs/_static/image16.png))

> [!NOTE]
> Po zakończeniu działania Kreatora konfiguracji zapytania karty tabeli może zostać wyświetlone okno dialogowe z informacją o tym, że nowy tekst polecenia zwraca dane ze schematem innym niż schemat głównej kwerendy. W skrócie Kreator nie ma zanotowania, że główne `GetCategories()` zapytanie TableAdapter s zwróci inny schemat niż ten, który został właśnie utworzony. Jednak jest to co jest potrzebne, więc możesz zignorować ten komunikat.

Należy również pamiętać, że jeśli używasz instrukcji SQL ad hoc i używasz Kreatora do zmiany głównego zapytania TableAdapter s w późniejszym czasie, spowoduje to Zmodyfikowanie listy kolumn `GetCategoryWithBinaryDataByCategoryID` Metoda s `SELECT` w celu uwzględnienia tylko tych kolumn z głównego zapytania (oznacza to, że usunie kolumnę `Picture` z zapytania). Konieczne będzie ręczne zaktualizowanie listy kolumn, aby zwracała `Picture` kolumnę, podobnie jak w przypadku metody `GetCategoriesAndNumberOfProducts()` wcześniej w tym kroku.

Po dodaniu dwóch `DataColumn` s do `CategoriesDataTable` i metody `GetCategoryWithBinaryDataByCategoryID` do `CategoriesTableAdapter`, te klasy w projektancie zestawu danych powinny wyglądać podobnie jak zrzut ekranu na rysunku 11.

![Projektant obiektów DataSet zawiera nowe kolumny i metodę](uploading-files-cs/_static/image11.gif)

**Ilustracja 11**. Projektant obiektów DataSet zawiera nowe kolumny i metodę

## <a name="updating-the-business-logic-layer-bll"></a>Aktualizowanie warstwy logiki biznesowej (LOGIKI biznesowej)

Po zaktualizowaniu DAL wszystko to, co pozostanie, aby rozszerzyć warstwę logiki biznesowej (LOGIKI biznesowej) w celu uwzględnienia metody nowej metody `CategoriesTableAdapter`. Dodaj następującą metodę do klasy `CategoriesBLL`:

[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Krok 5. Przekazywanie pliku z klienta na serwer sieci Web

Podczas zbierania danych binarnych często te dane są dostarczane przez użytkownika końcowego. Aby przechwycić te informacje, użytkownik musi mieć możliwość przekazania pliku z komputera do serwera sieci Web. Przekazane dane muszą być zintegrowane z modelem danych, co może oznaczać zapisanie pliku w systemie plików serwera sieci Web i dodanie ścieżki do pliku w bazie danych lub zapisanie zawartości binarnej bezpośrednio w bazie danych. W tym kroku dowiesz się, jak zezwolić użytkownikowi na przekazywanie plików z komputera na serwer programu. W następnym samouczku będziemy zachęcać do integracji przekazanego pliku z modelem danych.

ASP.NET 2,0 s New [FileUpload Web Control](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) udostępnia mechanizm umożliwiający użytkownikom wysyłanie plików z komputera do serwera sieci Web. Formant FileUpload renderuje jako element `<input>`, którego atrybut `type` jest ustawiony na plik, które przeglądarki są wyświetlane jako pole tekstowe z przyciskiem przeglądania. Kliknięcie przycisku Przeglądaj powoduje wyświetlenie okna dialogowego, w którym użytkownik może wybrać plik. Po opublikowaniu formularza zawartość wybranego pliku s jest wysyłana wraz z ogłaszaniem zwrotnym. Po stronie serwera informacje o przekazanym pliku są dostępne za pomocą właściwości kontrolki FileUpload s.

Aby zademonstrować przekazywanie plików, Otwórz stronę `FileUpload.aspx` w folderze `BinaryData`, przeciągnij kontrolkę FileUpload z przybornika do projektanta i ustaw właściwość Control s `ID` na `UploadTest`. Następnie Dodaj kontrolkę sieci Web przycisk Ustawienia `ID` i `Text` właściwości, aby `UploadButton` i przekazać wybrany plik odpowiednio. Na koniec Umieść kontrolkę sieci Web etykieta pod przyciskiem, wyczyść jej Właściwość `Text` i ustaw jej Właściwość `ID` na `UploadDetails`.

[![dodać kontrolkę FileUpload do strony ASP.NET](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Ilustracja 12**. Dodawanie kontrolki FileUpload do strony ASP.NET ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](uploading-files-cs/_static/image18.png))

Na rysunku 13 przedstawiono Tę stronę, gdy oglądasz za pomocą przeglądarki. Należy pamiętać, że kliknięcie przycisku Przeglądaj powoduje wyświetlenie okna dialogowego wyboru pliku, umożliwiające użytkownikowi wybranie pliku z komputera. Po wybraniu pliku kliknij przycisk Przekaż wybrany plik, co spowoduje wysłanie do serwera sieci Web wybranej zawartości binarnej plików.

[![użytkownik może wybrać plik do przekazania z komputera na serwer](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Ilustracja 13**. Użytkownik może wybrać plik do przekazania z komputera na serwer ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](uploading-files-cs/_static/image20.png))

W przypadku ogłaszania zwrotnego przekazany plik może być zapisany w systemie plików lub jego dane binarne mogą pracować bezpośrednio za pomocą strumienia. Na potrzeby tego przykładu należy utworzyć folder `~/Brochures` i zapisać w nim przekazany plik. Zacznij od dodania folderu `Brochures` do lokacji jako podfolderu katalogu głównego. Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `UploadButton` s `Click` i Dodaj następujący kod:

[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

Formant FileUpload zawiera różne właściwości do pracy z przekazanymi danymi. Na przykład [właściwość`HasFile`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) wskazuje, czy plik został przekazany przez użytkownika, podczas gdy [Właściwość`FileBytes`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) zapewnia dostęp do przekazanych danych binarnych jako tablicę bajtów. Procedura obsługi zdarzeń `Click` uruchamia się, upewniając się, że plik został przekazany. Jeśli plik został przekazany, etykieta pokazuje nazwę przekazanego pliku, jego rozmiar w bajtach i jego typ zawartości.

> [!NOTE]
> Aby upewnić się, że użytkownik przekazuje plik, możesz sprawdzić Właściwość `HasFile` i wyświetlić ostrzeżenie, jeśli `false`, lub zamiast tego można użyć [formantu RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) .

FileUpload s `SaveAs(filePath)` zapisuje przekazany plik do określonej *ścieżki*pliku. *ścieżka pliku musi* być *ścieżką fizyczną* (`C:\Websites\Brochures\SomeFile.pdf`), a nie *ścieżką* *wirtualną* (`/Brochures/SomeFile.pdf`). [Metoda`Server.MapPath(virtPath)`](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) przyjmuje ścieżkę wirtualną i zwraca odpowiadającą jej ścieżkę fizyczną. Tutaj ścieżka wirtualna jest `~/Brochures/fileName`, gdzie *filename* jest nazwą przekazanego pliku. Aby uzyskać więcej informacji na temat ścieżek wirtualnych i fizycznych przy użyciu `Server.MapPath`, zobacz temat [using Server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) .

Po zakończeniu procedury obsługi zdarzeń `Click` Poświęć chwilę na przetestowanie strony w przeglądarce. Kliknij przycisk Przeglądaj i wybierz plik z dysku twardego, a następnie kliknij przycisk Przekaż wybrany plik. Ogłaszanie zwrotne wyśle zawartość wybranego pliku na serwer sieci Web, co spowoduje wyświetlenie informacji o pliku przed zapisaniem go w folderze `~/Brochures`. Po przekazaniu pliku Wróć do programu Visual Studio i kliknij przycisk Odśwież w Eksplorator rozwiązań. Powinien zostać wyświetlony plik przekazany w folderze ~/Brochures.

[![plik EvolutionValley. jpg został przekazany do serwera sieci Web](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Ilustracja 14**: plik `EvolutionValley.jpg` został przekazany do serwera sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](uploading-files-cs/_static/image22.png))

![EvolutionValley. jpg zostało zapisane w folderze ~/Brochures](uploading-files-cs/_static/image15.gif)

**Ilustracja 15**. `EvolutionValley.jpg` zostało zapisane w folderze `~/Brochures`

## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Subtleties z zapisywaniem przekazanych plików w systemie plików

Istnieje kilka subtleties, które należy rozwiązać podczas zapisywania przekazywania plików do systemu plików serwera sieci Web. Najpierw wystąpił problem z bezpieczeństwem. Aby zapisać plik w systemie plików, kontekst zabezpieczeń, w którym jest wykonywana strona ASP.NET musi mieć uprawnienia do zapisu. Serwer sieci Web ASP.NET Development działa w kontekście bieżącego konta użytkownika. Jeśli używasz programu Microsoft s Internet Information Services (IIS) jako serwera sieci Web, kontekst zabezpieczeń zależy od wersji usług IIS i jej konfiguracji.

Inne wyzwanie związane z zapisywaniem plików w systemie plików są obracane wokół nazw plików. Obecnie nasza strona zapisuje wszystkie przekazane pliki do katalogu `~/Brochures` przy użyciu takiej samej nazwy, jak plik na komputerze klienta s. Jeśli użytkownik przekazuje broszurę o nazwie `Brochure.pdf`, plik zostanie zapisany jako `~/Brochure/Brochure.pdf`. Ale co zrobić, jeśli później użytkownik B przekaże inny plik broszury, który ma taką samą nazwę pliku (`Brochure.pdf`)? W przypadku kodu mamy teraz plik A użytkownika zostanie nadpisany przy użyciu przekazywania użytkownika B s.

Istnieje kilka technik rozwiązywania konfliktów nazw plików. Jedną z opcji jest Zabroń przekazywania pliku, jeśli istnieje już taki o tej samej nazwie. W ramach tego podejścia, gdy użytkownik B próbuje przekazać plik o nazwie `Brochure.pdf`, system nie zapisze pliku i zamiast tego wyświetli komunikat informujący użytkownika B o zmianie nazwy pliku i spróbuj ponownie. Innym podejściem jest zapisanie pliku przy użyciu unikatowej nazwy pliku, która może być [globalnie unikatowym identyfikatorem (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) lub wartością z odpowiednich kolumn klucza podstawowego rekordu bazy danych (przy założeniu, że przekazywanie jest skojarzone z określonym wierszem w modelu danych). W następnym samouczku opisano te opcje bardziej szczegółowo.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Wyzwania związane z bardzo dużą ilością danych binarnych

W tych samouczkach przyjęto założenie, że przechwycone dane binarne mają Nieumiarkowany rozmiar. Praca z bardzo dużą ilością plików danych binarnych, które mają kilka megabajtów lub większa, wprowadza nowe wyzwania, które wykraczają poza zakres tych samouczków. Na przykład domyślnie ASP.NET będzie odrzucać operacje przekazywania większe niż 4 MB, chociaż można je skonfigurować za pomocą [elementu`<httpRuntime>`](https://msdn.microsoft.com/library/e1f13641.aspx) w `Web.config`. Usługi IIS nakładają także własne ograniczenia rozmiaru przekazywania plików. Więcej informacji można znaleźć w temacie [przekazywanie plików przez usługi IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) . Ponadto czas poświęcony na przekazanie dużych plików może przekroczyć wartość domyślną 110 sekund ASP.NET na żądanie. Występują również problemy z pamięcią i wydajnością, które powstają podczas pracy z dużymi plikami.

Formant FileUpload jest niepraktyczny w przypadku dużych operacji przekazywania plików. Gdy zawartość pliku jest publikowana na serwerze, użytkownik końcowy musi cierpliwie zaczekać bez potwierdzenia, że ich przekazywanie jest postępu. To nie jest to problem występujący w przypadku mniejszych plików, które można przekazać w ciągu kilku sekund, ale może to być problem związany z podawaniem większych plików, które mogą upłynąć do przeładowania. Istnieją różne kontrolki przekazywania plików innych firm, które są lepiej dopasowane do obsługi dużych przeciążeń i wielu z tych dostawców zapewniają wskaźniki postępu i menedżerowie przekazywania ActiveX, które stanowią znacznie bardziej dopracowane środowisko użytkownika.

Jeśli aplikacja musi obsługiwać duże pliki, należy uważnie zbadać wyzwania i znaleźć odpowiednie rozwiązania dla konkretnych potrzeb.

## <a name="summary"></a>Podsumowanie

Kompilowanie aplikacji, która musi przechwycić dane binarne, zawiera szereg wyzwań. W tym samouczku przedstawiono pierwsze dwa: Decydowanie o miejscu przechowywania danych binarnych i umożliwieniu użytkownikowi przekazywania zawartości binarnej za pomocą strony sieci Web. W ciągu następnych trzech samouczków zobaczymy, jak skojarzyć przekazane dane z rekordem w bazie danych, a także jak wyświetlać dane binarne obok pól danych tekstowych.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Korzystanie z typów danych o dużej wartości](https://msdn.microsoft.com/library/ms178158.aspx)
- [Przewodniki Szybki Start dotyczące kontrolek FileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Kontrolka serwera ASP.NET 2,0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Ciemny bok przekazywania plików](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Teresa Murphy i Bernadette Leigh. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Dalej](displaying-binary-data-in-the-data-web-controls-cs.md)

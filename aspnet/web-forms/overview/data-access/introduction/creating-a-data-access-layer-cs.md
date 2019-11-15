---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Tworzenie warstwy dostępu do danych (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku rozpocznie się rozpoczęcie od bardzo początku i utworzenie warstwy dostępu do danych (DAL), przy użyciu wpisanych zestawów danych, aby uzyskać dostęp do informacji w bazie danych.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 5aaf97dc8448dcb7b94ef2e4e23f34fd37ac4426
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115188"
---
# <a name="creating-a-data-access-layer-c"></a>Tworzenie warstwy dostępu do danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> W tym samouczku rozpocznie się rozpoczęcie od bardzo początku i utworzenie warstwy dostępu do danych (DAL), przy użyciu wpisanych zestawów danych, aby uzyskać dostęp do informacji w bazie danych.

## <a name="introduction"></a>Wprowadzenie

Jako Deweloperzy sieci Web, nasza praca zaczyna się na pracy z danymi. Firma Microsoft tworzy bazy danych, aby przechowywać dane, kod do pobrania i modyfikacji oraz strony sieci Web, które mają być zbierane i podsumowywane. Jest to pierwszy samouczek w seriach krótkich, który będzie eksplorować techniki wdrażania tych wspólnych wzorców w ASP.NET 2,0. Zaczniemy od utworzenia [architektury oprogramowania](http://en.wikipedia.org/wiki/Software_architecture) składającej się z warstwy dostępu do danych (dal) przy użyciu zestawów DataSet z określonym typem, warstwy logiki biznesowej (logiki biznesowej), która wymusza niestandardowe reguły biznesowe, a warstwa prezentacji złożona ze stron ASP.NET, które współużytkują wspólny układ strony. Po podstawę tego zaplecza przejdziemy do raportowania, pokazujący, jak wyświetlać, podsumowywać, zbierać i sprawdzać poprawność danych z aplikacji sieci Web. Te samouczki są zwięzłe i zawierają instrukcje krok po kroku dotyczące dużej ilości zrzutów ekranu, aby przeprowadzić Cię przez proces wizualnie. Każdy samouczek jest dostępny w C# systemach i Visual Basic wersje i zawiera pobieranie pełnego użytego kodu. (Pierwszy samouczek jest dość długi, ale pozostałe fragmenty są prezentowane w znacznie większej części digestible).

W przypadku tych samouczków będziemy używać wersji Microsoft SQL Server 2005 Express z bazą danych Northwind umieszczonej w **aplikacji\_katalogu danych** . Oprócz pliku bazy danych, folder **danych\_aplikacji** zawiera również skrypty SQL służące do tworzenia bazy danych, jeśli chcesz użyć innej wersji bazy danych. Te skrypty można także [pobrać bezpośrednio z firmy Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), jeśli wolisz. W przypadku korzystania z innej wersji SQL Server bazy danych Northwind należy zaktualizować ustawienie **NORTHWNDConnectionString** w pliku **Web. config** aplikacji. Aplikacja sieci Web została skompilowana przy użyciu programu Visual Studio 2005 Professional Edition jako projektu witryny sieci Web opartej na systemie plików. Jednak wszystkie samouczki będą pracować równie dobrze z bezpłatną wersją programu Visual Studio 2005 i [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
W tym samouczku rozpocznie się rozpoczęcie od bardzo początku i utworzenie warstwy dostępu do danych (DAL), a następnie utworzenie warstwy logiki biznesowej (LOGIKI biznesowej) w drugim samouczku i pracę nad układem strony i nawigacją w trzecim. Samouczki po trzecim z nich zostaną skompilowane w oparciu o podstawę określoną w pierwszych trzech. W tym pierwszym samouczku mamy dużą ilość danych, więc Uruchom program Visual Studio i zacznijmy pracę!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Krok 1. Tworzenie projektu sieci Web i nawiązywanie połączenia z bazą danych

Zanim będziemy mogli utworzyć naszą warstwę dostępu do danych (DAL), najpierw musimy utworzyć witrynę sieci Web i skonfigurować naszą bazę danych. Zacznij od utworzenia nowej witryny sieci Web ASP.NET opartej na systemie plików. Aby to osiągnąć, przejdź do menu plik i wybierz polecenie Nowa witryna sieci Web, wyświetlając okno dialogowe Nowa witryna sieci Web. Wybierz szablon witryna sieci Web ASP.NET, Ustaw listę rozwijaną lokalizacja na system plików, wybierz folder, w którym ma zostać umieszczona witryna sieci Web, i Ustaw język C#na.

[![utworzyć nową witrynę sieci Web opartą na systemie plików](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Rysunek 1**. Tworzenie nowej witryny sieci Web opartej na systemie plików ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image3.png))

Spowoduje to utworzenie nowej witryny sieci Web z domyślną stroną ASP.NET **. aspx** i folderem **danych aplikacji\_** .

Po utworzeniu witryny sieci Web następnym krokiem jest dodanie odwołania do bazy danych w Eksplorator serwera programu Visual Studio. Dzięki dodaniu bazy danych do Eksplorator serwera można dodawać tabele, procedury składowane, widoki i tak dalej, jak w programie Visual Studio. Możesz również wyświetlać dane tabeli lub tworzyć własne zapytania ręcznie lub graficznie za pośrednictwem Konstruktor zapytań. Ponadto podczas kompilowania wpisanych zestawów danych dla DAL musimy wskazać program Visual Studio do bazy danych, z której mają być zbudowane określone zestawy danych. Chociaż możemy podać te informacje o połączeniu w tym momencie, program Visual Studio automatycznie wypełni listę rozwijaną baz danych, które są już zarejestrowane w Eksplorator serwera.

Kroki umożliwiające dodanie bazy danych Northwind do Eksplorator serwera zależą od tego, czy chcesz użyć bazy danych SQL Server 2005 Express Edition w folderze **aplikacji\_danych** , czy też w przypadku instalacji serwera bazy danych o Microsoft SQL Server 2000 lub 2005.

## <a name="using-a-database-in-the-app_data-folder"></a>Korzystanie z bazy danych w folderze danych\_aplikacji

Jeśli nie masz serwera bazy danych SQL Server 2000 lub 2005 do nawiązania połączenia lub chcesz, aby nie trzeba było dodawać bazy danych do serwera bazy danych, możesz użyć wersji SQL Server 2005 Express Edition bazy danych Northwind, która znajduje się w folderze **danych\_** z pobranej witryny sieci Web (**NORTHWND. MDF**).

Baza danych umieszczona w folderze **danych\_aplikacji** jest automatycznie dodawana do Eksplorator serwera. Przy założeniu, że na maszynie zainstalowano SQL Server 2005 Express Edition, powinien zostać wyświetlony węzeł o nazwie NORTHWND. MDF w Eksplorator serwera, które można rozwijać i eksplorować tabele, widoki, procedury składowane i tak dalej (patrz rysunek 2).

Folder **danych\_aplikacji** może również zawierać pliki Microsoft Access **. mdb** , takie jak ich SQL Server odpowiedniki, są automatycznie dodawane do Eksplorator serwera. Jeśli nie chcesz używać żadnej z SQL Server opcji, zawsze możesz [pobrać wersję pliku bazy danych Northwind z programu Microsoft Access](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) i upuścić ją do **aplikacji\_** Directory. Należy jednak pamiętać, że bazy danych programu Access nie są tak rozbudowane jako SQL Server i nie są przeznaczone do użycia w scenariuszach witryny sieci Web. Ponadto kilka samouczków 35 + będzie korzystać z pewnych funkcji na poziomie bazy danych, które nie są obsługiwane przez program.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Łączenie z bazą danych na serwerze bazy danych Microsoft SQL Server 2000 lub 2005

Alternatywnie można nawiązać połączenie z bazą danych Northwind zainstalowaną na serwerze bazy danych. Jeśli na serwerze bazy danych nie ma jeszcze zainstalowanej bazy danych Northwind, należy najpierw dodać ją do serwera bazy danych, uruchamiając skrypt instalacyjny zawarty w tym samouczku, pobierając lub wyświetlając [wersję SQL Server 2000 wersji Northwind i skrypt instalacyjny](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) bezpośrednio z witryny sieci Web firmy Microsoft.

Po zainstalowaniu bazy danych przejdź do Eksplorator serwera w programie Visual Studio, kliknij prawym przyciskiem myszy węzeł połączenia danych, a następnie wybierz polecenie Dodaj połączenie. Jeśli nie widzisz Eksplorator serwera przejdź do widoku/Eksplorator serwera lub naciśnij kombinację klawiszy CTRL + ALT + S. Spowoduje to wyświetlenie okna dialogowego Dodawanie połączenia, w którym można określić serwer, z którym ma zostać nawiązane połączenie, informacje o uwierzytelnianiu i nazwę bazy danych. Po pomyślnym skonfigurowaniu informacji o połączeniu z bazą danych i kliknięciu przycisku OK baza danych zostanie dodana jako węzeł pod węzłem połączenia danych. Węzeł bazy danych można rozwinąć, aby eksplorować tabele, widoki, procedury składowane i tak dalej.

![Dodawanie połączenia do bazy danych Northwind serwera bazy danych](creating-a-data-access-layer-cs/_static/image4.png)

**Rysunek 2**. Dodawanie połączenia do bazy danych Northwind serwera bazy danych

## <a name="step-2-creating-the-data-access-layer"></a>Krok 2. Tworzenie warstwy dostępu do danych

Podczas pracy z danymi jedną z opcji jest osadzenie logiki specyficznej dla danych bezpośrednio do warstwy prezentacji (w aplikacji sieci Web, strony ASP.NET tworzą warstwę prezentacji). Może to potrwać pisanie kodu ADO.NET w części kodu strony ASP.NET lub przy użyciu formantu kontrolki SqlDataSource z części znacznika. W obu przypadkach to podejście ściśle Couples logikę dostępu do danych za pomocą warstwy prezentacji. Zalecanym podejściem jest jednak oddzielenie logiki dostępu do danych od warstwy prezentacji. Ta osobna warstwa jest nazywana warstwą dostępu do danych, DAL na krótkie i jest zwykle implementowana jako oddzielny projekt biblioteki klas. Zalety tej architektury warstwowej są dobrze udokumentowane (zobacz sekcję "dalsze odczytywanie" na końcu tego samouczka, aby uzyskać informacje o tych zaletach) i to podejście, które zajmiemy się w tej serii.

Cały kod, który jest specyficzny dla bazowego źródła danych, taki jak tworzenie połączenia z bazą danych, wygenerowanie poleceń **SELECT**, **INSERT**, **Update**i **delete** , i tak dalej powinien znajdować się w dal. Warstwa prezentacji nie powinna zawierać żadnych odwołań do tego kodu dostępu do danych, ale zamiast tego należy wykonać wywołania do DAL dla dowolnych i wszystkich żądań danych. Warstwy dostępu do danych zazwyczaj zawierają metody uzyskiwania dostępu do danych źródłowych bazy danych. Na przykład baza danych Northwind zawiera tabele **produkty** i **Kategorie** , które rejestrują produkty do sprzedaży i kategorie, do których należą. W naszym DAL będziemy korzystać z metod takich jak:

- **GetCategories (),** które będą zwracać informacje o wszystkich kategoriach
- **GetProducts ()** , które zwracają informacje dotyczące wszystkich produktów
- **GetProductsByCategoryID (*IDkategorii*)** , która zwróci wszystkie produkty należące do określonej kategorii
- **GetProductByProductID (*ProductID*)** , który zwróci informacje o określonym produkcie

Te metody, gdy zostanie wywołane, będą łączyć się z bazą danych, wystawić odpowiednie zapytanie i zwrócić wyniki. Jak zwracamy te wyniki. Metody te mogą po prostu zwracać zestaw danych lub element DataReader wypełniany przez kwerendę bazy danych, ale najlepiej zwrócić te wyniki przy użyciu *obiektów o jednoznacznie określonym typie*. Obiekt o jednoznacznie określonym typie to jeden, którego schemat jest sztywno zdefiniowany w czasie kompilacji, natomiast przeciwieństwo do obiektu o swobodnym typie, jest to jeden, którego schemat nie jest znany do czasu wykonania.

Na przykład element DataReader i zestaw danych (domyślnie) są obiektami z swobodnym typem, ponieważ ich schemat jest zdefiniowany przez kolumny zwracane przez zapytanie bazy danych używane do ich wypełniania. Aby uzyskać dostęp do określonej kolumny z luźno wpisanej tabeli, należy użyć składni takiej jak:  <strong><em>DataTable</em>. Wiersze [<em>index</em>] ["<em>ColumnName</em>"]</strong>. Luźne wpisywanie danych w tym przykładzie jest zgodne z faktem, że musimy uzyskać dostęp do nazwy kolumny przy użyciu indeksu ciągu lub liczby porządkowej. Z drugiej strony element DataTable o jednoznacznie określonym typie będzie miał wszystkie kolumny zaimplementowane jako właściwości, w wyniku czego kod wygląda następująco:  <strong><em>DataTable</em>. Wiersze [<em>index</em>]. element *ColumnName</strong>* .

Aby można było zwracać obiekty o jednoznacznie określonym typie, deweloperzy mogą tworzyć własne niestandardowe obiekty biznesowe lub korzystać z wpisanych zestawów danych. Obiekt biznesowy jest implementowany przez dewelopera jako Klasa, której właściwości zwykle odzwierciedlają kolumny bazowej tabeli bazy danych, reprezentowanego przez obiekt biznesowy. Określony zestaw danych jest klasą wygenerowaną przez program Visual Studio w oparciu o schemat bazy danych, której elementy członkowskie są jednoznacznie wpisane zgodnie z tym schematem. Sam zestaw danych składa się z klas, które poszerzają klasy zestawu danych ADO.NET, DataTable i DataRow. Oprócz jednoznacznie wpisanych tabeli danych, typy zestawów danych zawierają teraz również TableAdapters, które są klasami z metodami służącymi do wypełniania tabel DataTables i propagowania modyfikacji w tabelach DataTables z powrotem do bazy.

> [!NOTE]
> Aby uzyskać więcej informacji o zaletach i wadach korzystania z wpisanych zestawów danych a niestandardowymi obiektami biznesowymi, zapoznaj się z tematem [projektowanie składników warstwy i przekazywanie danych za pośrednictwem warstw](https://msdn.microsoft.com/library/ms978496.aspx).

Będziemy używać zestawów danych o jednoznacznie określonym typie dla architektury samouczków. Rysunek 3 ilustruje przepływ pracy między różnymi warstwami aplikacji, które korzystają z wpisanych zestawów danych.

[![cały kod dostępu do danych jest Relegated DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Rysunek 3**. cały kod dostępu do danych jest RELEGATED do dal ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image7.png))

## <a name="creating-a-typed-dataset-and-table-adapter"></a>Tworzenie zestawu danych o określonym typie i karty tabeli

Aby rozpocząć tworzenie naszych DAL, Zacznijmy od dodania do projektu zestawu danych o określonym typie. Aby to osiągnąć, kliknij prawym przyciskiem myszy węzeł projektu w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy element. Wybierz opcję DataSet z listy szablonów i nadaj jej nazwę **Northwind. xsd**.

[![wybrać, aby dodać nowy zestaw danych do projektu](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Rysunek 4**. Wybierz, aby dodać nowy zestaw danych do projektu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image10.png))

Po kliknięciu przycisku Dodaj po wyświetleniu monitu o dodanie zestawu danych do folderu **\_aplikacji** kliknij przycisk Tak. Zostanie wyświetlony Projektant określonego zestawu danych i zostanie uruchomiony Kreator konfiguracji TableAdapter, który umożliwi dodanie pierwszego TableAdapter do określonego zestawu danych.

Typ DataSet służy jako kolekcja danych o jednoznacznie określonym typie. składa się z wystąpień typu DataTable o jednoznacznie określonym typie, z których każdy jest z kolei składający się z silnie typu instancji DataRow. Utworzymy silnie typu DataTable dla każdej źródłowej tabeli bazy danych, z którą będziemy musieli współpracować w tej serii samouczków. Zacznijmy od utworzenia elementu DataTable dla tabeli **Products** .

Należy pamiętać, że jednoznacznie wpisane tabele danych nie zawierają żadnych informacji na temat sposobu uzyskiwania dostępu do danych z ich źródłowej tabeli. Aby można było pobrać dane w celu wypełnienia DataTable, używamy klasy TableAdapter, która działa jako warstwa dostępu do danych. W przypadku naszych **produktów** DataTable TableAdapter będzie zawierać metody **getProducts ()** , **GetProductByCategoryID (*IDkategorii*)** i tak dalej, że wywołamy z warstwy prezentacji. Rola elementu DataTable służy jako obiekty o jednoznacznie określonym typie służące do przekazywania danych między warstwami.

Kreator konfiguracji TableAdapter rozpoczyna się od wyświetlenia monitu o wybranie bazy danych, z którą chcesz współpracować. Lista rozwijana zawiera te bazy danych w Eksplorator serwera. Jeśli baza danych Northwind nie została dodana do Eksplorator serwera, w tym momencie można kliknąć przycisk nowe połączenie.

[![wybrać bazę danych Northwind z listy rozwijanej](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Rysunek 5**. Wybierz z listy rozwijanej bazę danych Northwind (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image13.png))

Po wybraniu bazy danych i kliknięciu przycisku Dalej zostanie wyświetlony monit z prośbą o zapisanie parametrów połączenia w pliku **Web. config** . Przez zapisanie parametrów połączenia, należy unikać, aby był on zakodowany w klasach TableAdapter, co upraszcza działania, jeśli informacje o parametrach połączenia zmieniają się w przyszłości. Jeśli chcesz zapisać parametry połączenia w pliku konfiguracji, zostanie on umieszczony w sekcji **&lt;connectionStrings&gt;** , która może być [Opcjonalnie zaszyfrowana](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) w celu zwiększenia bezpieczeństwa lub zmodyfikowana później za pomocą nowej strony właściwości ASP.NET 2,0 w ramach narzędzia administratora graficznego interfejsu użytkownika usług IIS, co jest bardziej idealne dla administratorów.

[![zapisać parametry połączenia w pliku Web. config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Ilustracja 6**. Zapisz parametry połączenia w **pliku Web. config** ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image16.png))

Następnie musimy zdefiniować schemat dla pierwszej jednoznacznie wpisanej tabeli DataTable i podać pierwszą metodę TableAdapter, która będzie używana podczas wypełniania silnie określonego zestawu danych. Te dwa kroki są wykonywane równocześnie przez utworzenie zapytania zwracającego kolumny z tabeli, która ma być odzwierciedlona w naszym elemencie DataTable. Na końcu kreatora przekażemy nazwę metody do tego zapytania. Po wykonaniu tej metody można wywołać ją z naszej warstwy prezentacji. Metoda wykonuje zdefiniowane zapytanie i wypełnia silnie wpisaną wartość DataTable.

Aby rozpocząć Definiowanie zapytania SQL, musimy najpierw określić, jak chcemy, aby TableAdapter wystawić zapytanie. Możemy użyć instrukcji SQL ad hoc, utworzyć nową procedurę składowaną lub użyć istniejącej procedury składowanej. Dla tych samouczków będziemy używać instrukcji SQL ad hoc. Zapoznaj się z artykułem [Brian Noyes](http://briannoyes.net/), [Utwórz warstwę dostępu do danych za pomocą projektanta zestawu danych programu Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) , aby zapoznać się z przykładem korzystania z procedur składowanych.

[![wykonywanie zapytań dotyczących danych za pomocą instrukcji SQL ad hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Rysunek 7**. wykonywanie zapytania dotyczącego danych za pomocą instrukcji SQL ad hoc ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image19.png))

W tym momencie możemy ręcznie wpisać zapytanie SQL. Podczas tworzenia pierwszej metody w TableAdapter zazwyczaj chcesz, aby zapytanie zwracało te kolumny, które muszą być wyrażone w odpowiedniej tabeli DataTable. W tym celu można utworzyć zapytanie, które zwraca wszystkie kolumny i wszystkie wiersze z tabeli **Products** :

[![wprowadzić zapytanie SQL do pola tekstowego](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Ilustracja 8**. wprowadzanie zapytania SQL do pola tekstowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image22.png))

Alternatywnie, użyj Konstruktor zapytań i graficznego konstruowania zapytania, jak pokazano na rysunku 9.

[![utworzyć zapytanie graficznie, za pomocą edytora zapytań](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Rysunek 9**. Tworzenie zapytania graficznie, za pomocą edytora zapytań ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image25.png))

Po utworzeniu zapytania, ale przed przechodzeniem na następny ekran, kliknij przycisk Opcje zaawansowane. W projektach witryn sieci Web "Generuj instrukcje INSERT, Update i Delete" to jedyną zaawansowaną opcją wybraną domyślnie. Jeśli uruchomisz tego kreatora z biblioteki klas lub projektu systemu Windows, również zostanie wybrana opcja "Użyj optymistycznej współbieżności". Pozostaw opcję "Użyj optymistycznej współbieżności", która nie jest zaznaczona. Sprawdzimy optymistyczną współbieżność w przyszłych samouczkach.

[![wybrać tylko opcję Generuj instrukcje INSERT, Update i DELETE](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Ilustracja 10**. Wybierz tylko opcję Generuj instrukcje INSERT, Update i Delete (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image28.png))

Po sprawdzeniu opcji zaawansowanych kliknij przycisk Dalej, aby przejoć do końcowego ekranu. Oto prośba o wybranie metod do dodania do TableAdapter. Istnieją dwa wzorce do wypełniania danych:

- **Wypełnianie DataTable** przy użyciu tego podejścia Metoda jest tworzona, która przyjmuje element DataTable jako parametr i wypełnia je na podstawie wyników zapytania. Klasa ADO.NET DataAdapter, na przykład implementuje ten wzorzec za pomocą jego metody **Fill ()** .
- **Zwróć element DataTable** z tym podejściem Metoda tworzy i wypełnia element DataTable dla użytkownika i zwraca go jako metody zwraca wartość.

TableAdapter może zaimplementować jeden lub oba te wzorce. Możesz również zmienić nazwy metod dostarczonych tutaj. Pozostaw wszystkie zaznaczone pola wyboru, nawet jeśli będziemy używać tylko tego samego wzorca w ramach tych samouczków. Ponadto Zmieńmy **nazwę metody "** zamiast generycznej" na **getProducts**.

W przypadku zaznaczenia tego pola wyboru, "GenerateDBDirectMethods", tworzy metody **Insert ()** , **Update ()** i **delete (** ) dla TableAdapter. Jeśli ta opcja nie zostanie zaznaczona, wszystkie aktualizacje będą musiały zostać wykonane za pomocą metody **aktualizacji TableAdapter ()** , która przyjmuje określony zestaw danych, element DataTable, pojedynczy element DataRow lub tablicę DataRows. (Jeśli usunięto zaznaczenie opcji "Generuj instrukcje INSERT, Update i Delete" z właściwości zaawansowanych na rysunku 9, to ustawienie pola wyboru nie będzie miało żadnego efektu). Pozostaw to pole wyboru.

[![zmienić nazwy metody z GetData na getproductss](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Ilustracja 11**. zmiana nazwy metody z **GetData** na **getproductss** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image31.png))

Aby zakończyć pracę kreatora, kliknij przycisk Zakończ. Po zamknięciu kreatora zostaną zwrócone do projektanta obiektów DataSet, który pokazuje właśnie utworzoną tabelę. Można wyświetlić listę kolumn w tabeli **Products** (**ProductID**, **ProductName**itd.), a także metody **ProductsTableAdapter** (**Fill ()** i **getProducts ()** ).

[![produkty DataTable i ProductsTableAdapter zostały dodane do określonego zestawu danych](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Ilustracja 12**. **produkty** DataTable i **ProductsTableAdapter** zostały dodane do określonego zestawu danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image34.png))

W tym momencie mamy zestaw danych z jednym elementem DataTable (**Northwind. Products**) i z silnie wpisaną klasą DataAdapter (**NorthwindTableAdapters. ProductsTableAdapter**) za pomocą metody **getproductss ()** . Te obiekty mogą służyć do uzyskiwania dostępu do listy wszystkich produktów z kodu, takich jak:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Ten kod nie wymaga od nas zapisania jednego bitu kodu specyficznego dla dostępu do danych. Nie trzeba tworzyć wystąpień żadnych klas ADO.NET, nie musimy odwoływać się do żadnych parametrów połączenia, zapytań SQL ani procedur składowanych. Zamiast tego TableAdapter zapewnia kod dostępu do danych niskiego poziomu.

Każdy obiekt użyty w tym przykładzie jest również silnie wpisany, co pozwala programowi Visual Studio zapewnić sprawdzanie typów funkcji IntelliSense i czasu kompilacji. I najlepsze dla wszystkich tabel danych zwracanych przez TableAdapter można powiązać z kontrolkami sieci Web z danymi ASP.NET, takimi jak GridView, DetailsView, DropDownList, formant CheckBoxList i kilka innych. Poniższy przykład ilustruje powiązanie elementu DataTable zwróconego przez metodę **getProducts ()** z elementem GridView w zaledwie scant trzy wiersze kodu na **stronie,\_** obsłudze zdarzeń ładowania.

AllProducts. aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]

[![Lista produktów zostanie wyświetlona w widoku GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Ilustracja 13**. Lista produktów jest wyświetlana w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image37.png))

Chociaż ten przykład wymagał zapisu trzech wierszy kodu na naszej **\_stronie** ASP.net stronie programu obsługi zdarzeń, w przyszłych samouczkach sprawdzimy, jak użyć elementu ObjectDataSource, aby deklaratywnie pobrać dane z dal. Przy użyciu elementu ObjectDataSource nie będziemy musieli pisać żadnego kodu, a także uzyskać pomocy technicznej dotyczącej stronicowania i sortowania.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Krok 3. Dodawanie sparametryzowanych metod do warstwy dostępu do danych

W tym momencie Nasza Klasa **ProductsTableAdapter** ma, ale jedną metodę, **getProducts ()** , która zwraca wszystkie produkty z bazy danych. Chociaż możliwe jest korzystanie z wszystkich produktów, jest to bardzo przydatne, gdy będziemy chciały pobrać informacje o określonym produkcie lub wszystkie produkty należące do określonej kategorii. Aby dodać te funkcje do warstwy dostępu do danych, można dodać sparametryzowane metody do TableAdapter.

Dodajmy metodę **GetProductsByCategoryID (*IDkategorii*)** . Aby dodać nową metodę do DAL, Wróć do projektanta obiektów DataSet, kliknij prawym przyciskiem myszy w sekcji **ProductsTableAdapter** i wybierz polecenie Dodaj zapytanie.

![Kliknij prawym przyciskiem myszy TableAdapter i wybierz polecenie Dodaj zapytanie](creating-a-data-access-layer-cs/_static/image38.png)

**Ilustracja 14**. Kliknij prawym przyciskiem myszy TableAdapter i wybierz polecenie Dodaj zapytanie

Pierwszy monit o to, czy chcemy uzyskać dostęp do bazy danych przy użyciu instrukcji SQL ad hoc, czy nowej lub istniejącej procedury składowanej. Zdecydujmy się ponownie użyć instrukcji SQL ad hoc. Następnie pytamy, jakiego typu zapytania SQL chcesz użyć. Ponieważ chcemy zwrócić wszystkie produkty należące do określonej kategorii, chcemy napisać instrukcję **SELECT** , która zwraca wiersze.

[![utworzyć instrukcji SELECT, która zwraca wiersze](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Ilustracja 15**. Wybierz, aby utworzyć instrukcję **SELECT** , która zwraca wiersze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image41.png))

Następnym krokiem jest zdefiniowanie zapytania SQL służącego do uzyskiwania dostępu do danych. Ponieważ chcemy zwrócić tylko te produkty, które należą do określonej kategorii, używam tej samej instrukcji <strong>SELECT</strong> z elementu <strong>getproductss ()</strong>, ale Dodaj następującą klauzulę <strong>WHERE</strong> : <strong>IDkategorii = @CategoryID</strong>. Parametr <strong>@CategoryID</strong> wskazuje kreatorowi TableAdapter, że tworzona Metoda będzie wymagała parametru wejściowego odpowiedniego typu (tj. wartości null Integer).

[![wprowadzić zapytanie, aby zwracało tylko produkty w określonej kategorii](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Rysunek 16**. wprowadź zapytanie, aby zwrócić tylko produkty w określonej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image44.png))

W ostatnim kroku możemy wybrać wzorce dostępu do danych, które mają być używane, a także dostosować nazwy wygenerowanych metod. Dla deseniu wypełnienia Zmień nazwę na <strong>FillByCategoryID</strong> i dla zwracanego wzorca elementu DataTable Return (metody <strong>get*X</strong>*  ), użyjmy <strong>GetProductsByCategoryID</strong>.

[![wybrać nazwy dla metod TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Ilustracja 17**. Wybieranie nazw metod TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image47.png))

Po zakończeniu działania kreatora Projektant zestawu danych zawiera nowe metody TableAdapter.

![Teraz można wykonywać zapytania o produkty według kategorii](creating-a-data-access-layer-cs/_static/image48.png)

**Ilustracja 18**: produkty mogą teraz być odpytane według kategorii

Poświęć chwilę na dodanie metody **GetProductByProductID (*ProductID*)** za pomocą tej samej techniki.

Te zapytania parametryczne można przetestować bezpośrednio z projektanta obiektów DataSet. Kliknij prawym przyciskiem myszy metodę w TableAdapter i wybierz pozycję Podgląd danych. Następnie wprowadź wartości, które mają być używane dla parametrów, a następnie kliknij przycisk Podgląd.

[![są wyświetlane te produkty należące do kategorii napoje](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Ilustracja 19**. pokazywane są te produkty należące do kategorii napoje ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image51.png))

Za pomocą metody **GetProductsByCategoryID (*IDkategorii*)** w naszym dal możemy teraz utworzyć stronę ASP.NET, która wyświetla tylko te produkty w określonej kategorii. W poniższym przykładzie pokazano wszystkie produkty, które znajdują się w kategorii napoje, które mają **IDkategorii** 1.

Napoje. ASP

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]

[![są wyświetlane te produkty w kategorii napoje](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Ilustracja 20**. wyświetlane są te produkty w kategorii napoje ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image54.png))

## <a name="step-4-inserting-updating-and-deleting-data"></a>Krok 4. Wstawianie, aktualizowanie i usuwanie danych

Istnieją dwa wzorce często używane do wstawiania, aktualizowania i usuwania danych. Pierwszy wzorzec, który wywoła wzorzec bezpośredniej bazy danych, obejmuje tworzenie metod, które po wywołaniu, wydać polecenie **INSERT**, **Update**lub **delete** do bazy danych, która działa w ramach pojedynczego rekordu bazy danych. Takie metody są zwykle przesyłane w szeregu wartości skalarnych (liczb całkowitych, ciągów, wartości logicznych, DateTimes itd.), które odpowiadają wartościom do wstawienia, aktualizacji lub usunięcia. Na przykład, z tym wzorcem tabeli **Products** , Metoda Delete przyjmuje parametr w postaci liczby całkowitej, wskazujący **ProductID** rekordu do usunięcia, podczas gdy metoda Insert przyjmuje ciąg dla **NazwaProduktu**, wartość dziesiętną dla wartości **CenaJednostkowa**, liczbę całkowitą dla **UnitsOnStock**i tak dalej.

[![każde żądanie INSERT, Update i DELETE jest natychmiast wysyłane do bazy danych](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Ilustracja 21**. każde żądanie INSERT, Update i DELETE jest natychmiast wysyłane do bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image57.png))

Drugim wzorcem, do którego odwołuje się jako wzorzec aktualizacji wsadowej, jest aktualizacja całego zestawu danych, DataTable lub kolekcji obiektów DataRows w jednym wywołaniu metody. W tym wzorcu deweloper usuwa, wstawia i modyfikuje wiersze w elemencie DataTable, a następnie przekazuje te wiersze lub element DataTable do metody Update. Ta metoda wylicza następnie przekazane dane, określa, czy zostały zmodyfikowane, dodane lub usunięte (za pośrednictwem wartości [Właściwości RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) elementu DataRow) i wystawia odpowiednie żądania bazy danych dla każdego rekordu.

[![wszystkie zmiany są synchronizowane z bazą danych po wywołaniu metody Update](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Ilustracja 22**. wszystkie zmiany są synchronizowane z bazą danych po wywołaniu metody Update ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image60.png))

TableAdapter domyślnie używa wzorca aktualizacji wsadowych, ale również obsługuje wzorzec DB Direct. Ponieważ wybrano opcję "Generuj instrukcje INSERT, Update i Delete" z właściwości zaawansowanych podczas tworzenia naszego TableAdapter, **ProductsTableAdapter** zawiera metodę **Update ()** , która implementuje wzorzec aktualizacji wsadowych. W celu TableAdapter zawiera metodę **Update ()** , która może być przekazywać określony zestaw danych, niejawnie wpisaną wartość DataTable lub co najmniej jeden wiersz DataRows. Jeśli zaznaczono pole wyboru "GenerateDBDirectMethods" podczas pierwszego tworzenia TableAdapter wzorzec bazy danych Direct zostanie również wdrożony za pośrednictwem metod **Insert ()** , **Update ()** i **delete ()** .

Obie wzorce modyfikacji danych używają właściwości **InsertCommand**, **UpdateCommand**i **DeleteCommand** elementu TableAdapter do wydawania do bazy danych poleceń **INSERT**, **Update**i **delete** . Właściwości **InsertCommand**, **UpdateCommand**i **DeleteCommand** można sprawdzić i zmodyfikować, klikając TableAdapter w Projektancie obiektów DataSet, a następnie przechodząc do okno właściwości. (Upewnij się, że wybrano TableAdapter i że obiekt **ProductsTableAdapter** jest zaznaczony na liście rozwijanej w okno właściwości).

[![TableAdapter ma właściwości InsertCommand, UpdateCommand i DeleteCommand](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Ilustracja 23**. TableAdapter ma właściwości **InsertCommand**, **UpdateCommand**i **DeleteCommand** ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image63.png))

Aby przejrzeć lub zmodyfikować wszystkie te właściwości polecenia bazy danych, kliknij właściwość **CommandText** , która spowoduje wyświetlenie Konstruktor zapytań.

[![skonfigurować instrukcje INSERT, UPDATE i DELETE w Konstruktor zapytań](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Ilustracja 24**. Konfigurowanie instrukcji **INSERT**, **Update**i **delete** w Konstruktor zapytań ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image66.png))

Poniższy przykład kodu pokazuje, jak używać wzorca aktualizacji wsadowej do podwójnej ceny wszystkich produktów, które nie zostały wycofane i które mają 25 jednostek w magazynie lub mniej:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Poniższy kod ilustruje sposób używania wzorca DB Direct do programistycznego usuwania określonego produktu, a następnie aktualizacji, a następnie dodania nowego:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Tworzenie niestandardowych metod INSERT, Update i DELETE

Metody **Insert ()** , **Update ()** i **delete (** ) utworzone przez metodę Direct DB mogą być nieskomplikowaną, szczególnie w przypadku tabel z wieloma kolumnami. Spojrzenie na poprzedni przykład kodu, bez pomocy IntelliSense, nie jest szczególnie jasne, które kolumny tabeli **produkty** są mapowane na każdy parametr wejściowy do metod **Update ()** i **Insert ()** . Mogą wystąpić sytuacje, w których chcemy tylko zaktualizować pojedynczą kolumnę lub dwie lub użyć dostosowanej metody **Insert ()** , która prawdopodobnie zwróci wartość pola **Identity** nowo wstawionego rekordu (autoprzyrost).

Aby utworzyć taką metodę niestandardową, Wróć do projektanta obiektów DataSet. Kliknij prawym przyciskiem myszy TableAdapter i wybierz polecenie Dodaj zapytanie, zwracając do kreatora TableAdapter. Na drugim ekranie możemy wskazać typ zapytania do utworzenia. Utwórzmy metodę, która dodaje nowy produkt, a następnie zwróci wartość **identyfikatora**nowo dodanego rekordu. W związku z tym należy wybrać opcję Utwórz zapytanie **wstawiające** .

[![utworzyć metodę, aby dodać nowy wiersz do tabeli Products](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Ilustracja 25**. Utwórz metodę, aby dodać nowy wiersz do tabeli **Products** ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image69.png))

Na następnym ekranie zostanie wyświetlona pozycja **CommandText** **InsertCommand**. Rozszerzyć to zapytanie, dodając **zakres\_Identity ()** na końcu zapytania, co spowoduje zwrócenie ostatniej wartości tożsamości wstawionej do kolumny **tożsamości** w tym samym zakresie. (Zapoznaj się z [dokumentacją techniczną](https://msdn.microsoft.com/library/ms190315.aspx) , aby uzyskać więcej informacji na temat **zakresu\_Identity ()** i dlaczego prawdopodobnie chcesz [użyć zakresu\_identity () zamiast @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Przed dodaniem instrukcji **SELECT** upewnij się, że została zakończona instrukcja **INSERT** z średnikiem.

[![rozszerzyć zapytanie w celu zwrócenia wartości SCOPE_IDENTITY ()](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Ilustracja 26**. rozszerzanie zapytania w celu zwrócenia **zakresu\_wartość Identity ()** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image72.png))

Na koniec Nazwij nową metodę **InsertProduct**.

[![ustawić nową nazwę metody na InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Ilustracja 27**. Ustaw nową nazwę metody na **InsertProduct** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image75.png))

Po powrocie do projektanta obiektów DataSet zobaczysz, że **ProductsTableAdapter** zawiera nową metodę **InsertProduct**. Jeśli ta nowa metoda nie ma parametru dla każdej kolumny w tabeli **Products** , prawdopodobnie nie pamiętasz o przerwaniu instrukcji **INSERT** z średnikiem. Skonfiguruj metodę **InsertProduct** i upewnij się, że istnieje średnik oddzielający instrukcje **INSERT** i **SELECT** .

Domyślnie metody INSERT nie generują metod niezwiązanych z badaniem, co oznacza, że zwracają liczbę odnośnych wierszy. Jednak chcemy, aby Metoda **InsertProduct** zwracała wartość zwróconą przez zapytanie, a nie liczbę wierszy, których to dotyczy. Aby to osiągnąć, Dostosuj Właściwość **executemode** metody **InsertProduct** na wartość **skalarną**.

[![zmienić właściwość Executemode na wartość skalarną](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Ilustracja 28**. Zmiana właściwości **Executemode** na **skalarną** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image78.png))

Poniższy kod przedstawia tę nową metodę **InsertProduct** w działaniu:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Krok 5. wykonywanie warstwy dostępu do danych

Należy zauważyć, że Klasa **ProductsTableAdapters** zwraca wartości **IDkategorii** i **IDDostawcy** z tabeli **Products** , ale nie zawiera kolumny **CategoryName** z tabeli **Categories** ani kolumny **NazwaFirmy** z tabeli **dostawcy** , chociaż prawdopodobnie są to kolumny, które mają być wyświetlane podczas wyświetlania informacji o produkcie. Możemy rozszerzyć metodę początkową TableAdapter, **getProducts ()** , aby uwzględnić zarówno wartości kolumn **CategoryName** , jak i **NazwaFirmy** , co spowoduje zaktualizowanie tabeli DataTable o jednoznacznie określonym typie w celu uwzględnienia tych nowych kolumn.

Może to stwarzać problem, jednak ponieważ metody TableAdapter do wstawiania, aktualizowania i usuwania danych są oparte na tej metodzie początkowej. Na szczęście w klauzuli **SELECT** nie wpływają na automatycznie generowane metody wstawiania, aktualizowania i usuwania. Pamiętając, aby dodawać zapytania do **kategorii** i **dostawców** jako podzapytania, a nie do **przyłączania** do usługi s, unikamy konieczności ponownego wykonywania tych metod w celu modyfikowania danych. Kliknij prawym przyciskiem myszy metodę **getProducts ()** w **ProductsTableAdapter** i wybierz pozycję Konfiguruj. Następnie dostosuj klauzulę **SELECT** , aby wyglądać następująco:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]

[![zaktualizować instrukcji SELECT dla metody getProducts ()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Ilustracja 29**. Aktualizowanie instrukcji **SELECT** dla metody **getProducts ()** ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image81.png))

Po zaktualizowaniu metody **getProducts ()** w celu użycia nowego zapytania tabela DataTable będzie zawierać dwie nowe kolumny: **CategoryName** i **NazwaDostawcy**.

![Element DataTable produkty ma dwie nowe kolumny](creating-a-data-access-layer-cs/_static/image82.png)

**Ilustracja nr 30**: **produkty** DataTable mają dwie nowe kolumny

Poświęć chwilę na aktualizację klauzuli **SELECT** w metodzie **GetProductsByCategoryID (*IDkategorii*)** .

W przypadku aktualizacji funkcji **Getproductss ()** **Wybierz opcję** using **Join** Projektant zestawu danych nie będzie w stanie automatycznie generować metod wstawiania, aktualizowania i usuwania danych bazy danych przy użyciu wzorca DB Direct. Zamiast tego trzeba będzie ręcznie utworzyć je podobnie jak w przypadku metody **InsertProduct** we wcześniejszej części tego samouczka. Ponadto należy ręcznie podać wartości właściwości **InsertCommand**, **UpdateCommand**i **DeleteCommand** , jeśli chcesz użyć wzorca aktualizacji wsadowych.

## <a name="adding-the-remaining-tableadapters"></a>Dodawanie pozostałej TableAdapters

Do tej pory oglądamy tylko pracę z pojedynczym TableAdapter dla pojedynczej tabeli bazy danych. Jednak baza danych Northwind zawiera kilka powiązanych tabel, z którymi będziemy musieli współpracować w naszej aplikacji sieci Web. Typ DataSet może zawierać wiele powiązanych tabel danych. W związku z tym, aby ukończyć nasze DAL, musimy dodać tabelę DataTables dla innych tabel, z których będziemy korzystać w tych samouczkach. Aby dodać nowy TableAdapter do określonego zestawu danych, Otwórz projektanta obiektów DataSet, kliknij prawym przyciskiem myszy w projektancie, a następnie wybierz polecenie Dodaj/TableAdapter. Spowoduje to utworzenie nowego elementu DataTable i TableAdapter, a następnie przechodzenie przez kreatora w tym samouczku.

Utworzenie następujących TableAdapters i metod przy użyciu następujących zapytań potrwa kilka minut. Należy zauważyć, że zapytania w **ProductsTableAdapter** obejmują podzapytania, aby uzyskać nazwę kategorii i dostawców każdego produktu. Ponadto, jeśli wcześniej zostały już dodane metody **getProducts ()** i **GetProductsByCategoryID (*IDkategorii*)** klasy **ProductsTableAdapter** .

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **Getdostawcy**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]

[![projektanta obiektów DataSet po dodaniu czterech TableAdapters](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Rysunek 31**: Projektant zestawu danych po dodaniu czterech TableAdapters ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image85.png))

## <a name="adding-custom-code-to-the-dal"></a>Dodawanie niestandardowego kodu do DAL

TableAdapters i tabele danych dodawane do określonego typu danych są wyrażane jako plik definicji schematu XML (**Northwind. xsd**). Informacje o schemacie można wyświetlić, klikając prawym przyciskiem myszy plik **Northwind. xsd** w Eksplorator rozwiązań i wybierając polecenie Wyświetl kod.

[![plik definicji schematu XML (XSD) dla zestawu danych Northwinds](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Rysunek 32**: plik definicji schematu XML (XSD) dla zestawu danych Northwinds ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image88.png))

Informacje o schemacie są tłumaczone C# na kod lub Visual Basic w czasie projektowania podczas kompilowania lub w czasie wykonywania (jeśli jest to konieczne), po którym można wykonać krokowo za pomocą debugera. Aby wyświetlić ten automatycznie wygenerowany kod, przejdź do Widok klasy i przechodzenie do szczegółów klasy TableAdapter lub typy zestawów danych. Jeśli nie widzisz Widok klasy na ekranie, przejdź do menu Widok i wybierz go z tego miejsca lub naciśnij klawisze CTRL + SHIFT + C. Z Widok klasy można zobaczyć właściwości, metody i zdarzenia dla wpisanych zestawów danych i klas TableAdapter. Aby wyświetlić kod dla konkretnej metody, kliknij dwukrotnie nazwę metody w Widok klasy lub kliknij ją prawym przyciskiem myszy i wybierz polecenie Przejdź do definicji.

![Zbadaj automatycznie wygenerowany kod, wybierając pozycję Przejdź do definicji z Widok klasy](creating-a-data-access-layer-cs/_static/image89.png)

**Rysunek 33**: Zbadaj automatycznie wygenerowany kod, wybierając pozycję Przejdź do definicji z widok klasy

Chociaż wygenerowany automatycznie kod może być doskonałym oszczędzaniem czasu, kod jest często bardzo ogólny i należy go dostosować, aby spełniał unikatowe potrzeby aplikacji. Niemniej ryzyko rozszerzania automatycznie generowanego kodu, że narzędzie, które wygenerowało kod, może zdecydować się na ponowne wygenerowanie i zastąpienie dostosowań. Dzięki nowej koncepcji klasy częściowej .NET 2.0 można łatwo podzielić klasę na wiele plików. Dzięki temu możemy dodawać własne metody, właściwości i zdarzenia do automatycznie generowanych klas bez konieczności przejmowania się informacjami o programie Visual Studio zastępującym dostosowania.

Aby zademonstrować sposób dostosowywania DAL, dodajmy metodę **getProducts ()** do klasy **SuppliersRow** . Klasa **SuppliersRow** reprezentuje pojedynczy rekord w tabeli **dostawcy** ; Każdy dostawca może dostawca zero dla wielu produktów, więc **getProducts ()** zwróci te produkty określonego dostawcy. Aby to zrobić, Utwórz nowy plik klasy w **aplikacji\_folderze kodu** o nazwie **SuppliersRow.cs** i Dodaj następujący kod:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Ta klasa częściowa instruuje kompilator, że podczas kompilowania klasy **Northwind. SuppliersRow** w celu uwzględnienia właśnie zdefiniowanej metody **getProducts ()** . Jeśli kompilujesz projekt, a następnie wrócisz do Widok klasy zobaczysz element **getProducts ()** teraz na liście jako metoda **Northwind. SuppliersRow**.

![Metoda getProducts () jest teraz częścią klasy Northwind. SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**Ilustracja 34**: Metoda **getProducts ()** jest teraz częścią klasy **Northwind. SuppliersRow**

Metody **getProducts ()** można teraz użyć do wyliczenia zestawu produktów dla określonego dostawcy, jak pokazano w poniższym kodzie:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Te dane mogą być również wyświetlane w dowolnym z ASP. Kontrolki sieci Web danych netto. Na poniższej stronie jest wykorzystywana kontrolka GridView z dwoma polami:

- BoundField, który wyświetla nazwę każdego dostawcy, i
- Element TemplateField, który zawiera kontrolkę BulletedList, która jest powiązana z wynikami zwróconymi przez metodę **getProducts ()** dla każdego dostawcy.

Sprawdzimy, jak wyświetlać takie raporty o szczegółach głównych w przyszłych samouczkach. Na razie ten przykład został zaprojektowany tak, aby zilustrować użycie metody niestandardowej dodanej do klasy **Northwind. SuppliersRow** .

SuppliersAndProducts. aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]

[![nazwa firmy dostawcy znajduje się w lewej kolumnie, w której znajdują się produkty z prawej strony](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Ilustracja 35**: Nazwa firmy dostawcy jest wymieniona w lewej kolumnie, ich produkty w prawym okienku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image93.png))

## <a name="summary"></a>Podsumowanie

Podczas kompilowania aplikacji sieci Web tworzącej DAL powinien być jednym z pierwszych kroków, które należy wykonać przed rozpoczęciem tworzenia warstwy prezentacji. W programie Visual Studio tworzenie DAL na podstawie wpisanych zestawów danych to zadanie, które można wykonać w ciągu 10-15 minut bez konieczności pisania wiersza kodu. Samouczki przenoszone do przodu zostaną skompilowane po tym DAL. W [następnym samouczku](creating-a-business-logic-layer-cs.md) zdefiniujemy kilka reguł firmy i zapoznaj się ze sposobem ich implementacji w oddzielnej warstwie logiki biznesowej.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Tworzenie DAL przy użyciu jednoznacznie wpisanych TableAdapters i tabel DataTables w programie VS 2005 i ASP.NET 2,0](https://weblogs.asp.net/scottgu/435498)
- [Projektowanie składników warstwy danych i przekazywanie danych przy użyciu warstw](https://msdn.microsoft.com/library/ms978496.aspx)
- [Tworzenie warstwy dostępu do danych za pomocą projektanta obiektów DataSet programu Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Szyfrowanie informacji o konfiguracji w aplikacjach ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter — Omówienie](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Praca z określonym zestawem danych](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Używanie dostępu do danych o jednoznacznie określonym typie w programie Visual Studio 2005 i ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Jak zwiększyć metody TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Pobieranie danych skalarnych z procedury składowanej](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenia wideo dotyczące tematów zawartych w tym samouczku

- [Warstwy dostępu do danych w aplikacjach ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Jak ręcznie powiązać zestaw danych z elementem DataGrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Jak korzystać z zestawów danych i filtrów z aplikacji ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci ołowiu dla tego samouczka były Piotr Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez i Carlos Santos. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](creating-a-business-logic-layer-cs.md)

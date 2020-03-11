---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: Wydajne stronicowanie za dużą ilością danych (VB) | Microsoft Docs
author: rick-anderson
description: Domyślna opcja stronicowania kontrolki prezentacja danych jest nieodpowiednia podczas pracy z dużymi ilościami danych.
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 0c788c4109d0d2839de969c628399290376a1ccd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78589944"
---
# <a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Efektywne stronicowanie dużych ilości danych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) lub [Pobierz plik PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> Domyślna opcja stronicowania kontrolki prezentacja danych jest nieodpowiednia podczas pracy z dużymi ilościami danych, ponieważ jej źródłowa kontrola źródła danych pobiera wszystkie rekordy, nawet gdy jest wyświetlany tylko podzestaw danych. W takim przypadku należy zmienić niestandardowe stronicowanie.

## <a name="introduction"></a>Wprowadzenie

Jak opisano w poprzednim samouczku, stronicowanie można zaimplementować na jeden z dwóch sposobów:

- **Domyślne stronicowanie** można zaimplementować, zaznaczając opcję Włącz stronicowanie w tagu inteligentnym kontrolki sieci Web danych. Jednak za każdym razem, gdy oglądasz stronę danych, element ObjectDataSource pobiera *wszystkie* rekordy, nawet jeśli na stronie są wyświetlane tylko ich podzbiór.
- **Niestandardowe stronicowanie** poprawia wydajność domyślnego stronicowania przez pobranie tylko tych rekordów z bazy danych, które mają być wyświetlane dla określonej strony danych żądanych przez użytkownika; Niemniej jednak stronicowanie niestandardowe obejmuje zwiększenie nakładu pracy niż domyślne stronicowanie

Ze względu na prostotę implementacji wystarczy zaznaczyć pole wyboru i ponownie wykonać tę czynność. domyślne stronicowanie jest atrakcyjną opcją. Podejście na przyszłość przy pobieraniu wszystkich rekordów, chociaż sprawia, że jest to niewiarygodne rozwiązanie podczas stronicowania za pomocą wystarczająco dużych ilości danych lub dla witryn z wieloma współbieżnymi użytkownikami. W takiej sytuacji należy włączyć niestandardowe stronicowanie w celu zapewnienia systemu, który odpowiada.

Wyzwanie niestandardowego stronicowania jest w stanie zapisać zapytanie, które zwraca dokładny zestaw rekordów wymaganych przez określoną stronę danych. Na szczęście Microsoft SQL Server 2005 udostępnia nowe słowo kluczowe dla wyników rankingu, co pozwala nam napisać zapytanie, które może efektywnie pobrać odpowiedni podzbiór rekordów. W tym samouczku dowiesz się, jak używać nowego słowa kluczowego SQL Server 2005 w celu zaimplementowania niestandardowego stronicowania w kontrolce GridView. Interfejs użytkownika dla niestandardowego stronicowania jest identyczny z tym, że dla domyślnego stronicowania, krokowe przechodzenie z jednej strony do następnego przy użyciu stronicowania niestandardowego może mieć kilka zamówień o wielkości szybciej niż domyślne stronicowanie.

> [!NOTE]
> Dokładny wpływ na wydajność wystawiony przez niestandardowe stronicowanie zależy od łącznej liczby rekordów, które są stronicowane, i obciążenia umieszczanego na serwerze bazy danych. Na końcu tego samouczka Przyjrzyjmy się niektórym metrykom, które przedstawiają korzyści wynikające z wydajności uzyskanej za pomocą niestandardowego stronicowania.

## <a name="step-1-understanding-the-custom-paging-process"></a>Krok 1: zrozumienie niestandardowego procesu stronicowania

Podczas stronicowania za pomocą danych dokładne rekordy wyświetlane na stronie są zależne od strony żądanych danych i liczby rekordów wyświetlanych na stronie. Załóżmy na przykład, że chcielibyśmy uzyskać stronę dotyczącą produktów 81, wyświetlając 10 produktów na stronę. Podczas wyświetlania pierwszej strony chcemy mieć produkty od 1 do 10. Gdy przeglądasz drugą stronę, chcemy w produktach 11 do 20 itd.

Istnieją trzy zmienne, które określają, jakie rekordy muszą zostać pobrane oraz jak ma być renderowany interfejs stronicowania:

- **Początkowy indeks wiersza** indeks pierwszego wiersza na stronie danych do wyświetlenia; Ten indeks można obliczyć przez pomnożenie indeksu strony przez rekordy, które mają być wyświetlane na stronie i dodawane do nich. Na przykład podczas stronicowania za pomocą rekordów 10 na pierwszej stronie (którego indeks strony ma wartość 0) indeks wierszy początkowych to 0 \* 10 + 1, lub 1; dla drugiej strony (którego indeks strony to 1) indeks wierszy początkowych to 1 \* 10 + 1 lub 11.
- **Maksymalna liczba wierszy** , które mają być wyświetlane na stronie w maksymalnej liczbie rekordów. Ta zmienna jest określana jako maksymalna liczba wierszy od ostatniej strony może być mniej rekordów zwracanych niż rozmiar strony. Na przykład podczas stronicowania przez 81 rekordów produktów na stronie dziewiąta i końcowa będzie mieć tylko jeden rekord. Żadna strona nie zawiera więcej rekordów niż wartość maksymalna liczba wierszy.
- **Całkowita liczba rekordów** , które łączą się ze stroną. Chociaż ta zmienna jest t jest niezbędna do określenia, które rekordy mają być pobierane dla danej strony, wymuszają interfejs stronicowania. Na przykład jeśli na stronie są dostępne 81 produkty, interfejs stronicowania wie o wyświetlaniu dziewięciu numerów stron w interfejsie użytkownika stronicowania.

Przy domyślnym stronicowaniu indeks wierszy początkowych jest obliczany jako iloczyn indeksu strony i rozmiaru strony plus jeden, natomiast Maksymalna liczba wierszy jest po prostu rozmiar strony. Ponieważ domyślne stronicowanie pobiera wszystkie rekordy z bazy danych podczas renderowania dowolnej strony danych, indeks dla każdego wiersza jest znany, co sprawia, że przejście do początku wiersza indeksu wierszy jest proste. Ponadto całkowita liczba rekordów jest łatwo dostępna, ponieważ s po prostu liczbę rekordów w elemencie DataTable (lub dowolny obiekt jest używany do przechowywania wyników bazy danych).

W przypadku zmiennych początkowych indeks wiersza i Maksymalna liczba wierszy implementacja stronicowania niestandardowego musi zwracać tylko dokładny podzbiór rekordów, zaczynając od indeksu wierszy początkowych i maksymalnie maksymalną liczbę wierszy rekordów. Niestandardowe stronicowanie zawiera dwa wyzwania:

- Musimy mieć możliwość wydajnego kojarzenia indeksu wierszy z każdym wierszem na wszystkich danych, które są stronicowane w taki sposób, aby można było rozpocząć zwracanie rekordów w określonym indeksie początkowych wierszy.
- Musimy podać łączną liczbę rekordów na stronie

W dwóch następnych krokach sprawdzimy skrypt SQL potrzebny do odpowiedzi na te dwa wyzwania. Oprócz skryptu SQL należy również zaimplementować metody w DAL i LOGIKI biznesowej.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Krok 2. Zwracanie łącznej liczby rekordów na stronie

Zanim sprawdzimy, jak pobrać dokładny podzestaw rekordów dla wyświetlanej strony, poczekaj, jak zwrócić łączną liczbę rekordów na stronie. Te informacje są konieczne w celu poprawnego skonfigurowania interfejsu użytkownika stronicowania. Łączną liczbę rekordów zwracanych przez określone zapytanie SQL można uzyskać za pomocą [funkcji agregującej`COUNT`](https://msdn.microsoft.com/library/ms175997.aspx). Na przykład aby określić łączną liczbę rekordów w tabeli `Products`, można użyć następującego zapytania:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Zezwól usłudze s na dodanie metody do DAL, która zwraca te informacje. W szczególności utworzysz metodę DAL o nazwie `TotalNumberOfProducts()`, która wykonuje instrukcję `SELECT` wskazaną powyżej.

Zacznij od otwarcia pliku zestawu danych z `Northwind.xsd` określonym w folderze `App_Code/DAL`. Następnie kliknij prawym przyciskiem myszy `ProductsTableAdapter` w projektancie, a następnie wybierz polecenie Dodaj zapytanie. Jak widać w poprzednich samouczkach, umożliwimy nam Dodawanie nowej metody do DAL, która po wywołaniu spowoduje wykonanie określonej instrukcji SQL lub procedury składowanej. Podobnie jak w przypadku naszych metod TableAdapter w poprzednich samouczkach, aby to zrobić, należy użyć instrukcji SQL ad hoc.

![Korzystanie z instrukcji SQL ad hoc](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Rysunek 1**. używanie instrukcji SQL ad hoc

Na następnym ekranie możemy określić typ zapytania, który ma zostać utworzony. Ponieważ to zapytanie zwróci pojedynczą wartość skalarną, Łączna liczba rekordów w tabeli `Products` wybierz `SELECT`, która zwraca opcję "wartość odrębna".

![Konfigurowanie zapytania do użycia instrukcji SELECT, która zwraca pojedynczą wartość](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Rysunek 2**. Konfigurowanie zapytania do użycia instrukcji SELECT, która zwraca pojedynczą wartość

Po wskazaniu typu zapytania, który ma być używany, musimy dalej określić zapytanie.

![Użyj zapytania SELECT COUNT (*) FROM Products](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Rysunek 3**. użycie zapytania SELECT COUNT (\*) from Products

Na koniec Określ nazwę metody. Jak wspomniano powyżej, użyj `TotalNumberOfProducts`.

![Nazwij metodę DAL TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Rysunek 4**. Nazwa metody dal TotalNumberOfProducts

Po kliknięciu przycisku Zakończ Kreator doda metodę `TotalNumberOfProducts` do DAL. Metody skalarne zwracające w DAL zwracające Typy dopuszczające wartość null w przypadku, gdy wynik zapytania SQL jest `NULL`. Nasza kwerenda `COUNT`, jednak zawsze zwróci wartość nie`NULL`ną; niezależnie od tego Metoda DAL zwraca wartość null Integer.

Oprócz metody DAL potrzebujemy również metody w LOGIKI biznesowej. Otwórz plik klasy `ProductsBLL` i Dodaj metodę `TotalNumberOfProducts`, która po prostu wywołuje metodę `TotalNumberOfProducts` DAL s:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

Metoda DAL `TotalNumberOfProducts` zwraca wartość null Integer; Jednakże tworzymy metodę `ProductsBLL` klasy s `TotalNumberOfProducts`, tak aby zwracała standardową liczbę całkowitą. W związku z tym musimy mieć metodę `ProductsBLL` klasy s `TotalNumberOfProducts` zwrócić część wartości null liczby całkowitej zwracanej przez metodę `TotalNumberOfProducts` DAL s. Wywołanie `GetValueOrDefault()` zwraca wartość null Integer (jeśli istnieje); Jeśli wartość null jest nie`null`, jednak zwraca domyślną wartość całkowitą 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Krok 3. Zwracanie dokładnego podzbioru rekordów

Następnym zadaniem jest utworzenie metod w DAL i LOGIKI biznesowej, które akceptują wcześniej wymienione zmienne początkowy indeks wiersza i Maksymalna liczba wierszy i zwracają odpowiednie rekordy. Przed wykonaniem tej czynności poinformuj najpierw o wymaganym skrypcie SQL. Wyzwaniem na bieżąco jest to, że musimy być w stanie efektywnie przypisywać indeks do każdego wiersza w całych wynikach, tak aby można było zwrócić tylko te rekordy, zaczynając od indeksu wierszy początkowych (oraz do maksymalnej liczby rekordów rekordów).

Nie jest to wyzwanie, jeśli w tabeli bazy danych istnieje już kolumna, która służy jako indeks wiersza. Na pierwszy rzut oka możemy wziąć pod uwagę, że `Products` tabeli `ProductID`, ponieważ pierwszy produkt ma `ProductID` 1, drugi a 2 itd. Jednak usunięcie produktu pozostawia przerwy w sekwencji, nullifying to podejście.

Istnieją dwie ogólne techniki służące do wydajnego kojarzenia indeksu wierszy z danymi do stron przez, co pozwala na pobranie precyzyjnego podzestawu rekordów:

- **Przy użyciu SQL Server 2005 s `ROW_NUMBER()` słowo kluczowe** new do SQL Server 2005, słowo kluczowe `ROW_NUMBER()` kojarzy klasyfikację z każdym zwróconym rekordem na podstawie pewnej kolejności. Ta klasyfikacja może służyć jako indeks wierszy dla każdego wiersza.
- **Używanie zmiennej tabeli i `SET ROWCOUNT`** Instrukcji SQL Server s [`SET ROWCOUNT`](https://msdn.microsoft.com/library/ms188774.aspx) można użyć, aby określić, ile razem rekordów ma przetwarzać zapytanie przed zakończeniem; [zmienne tabeli](http://www.sqlteam.com/item.asp?ItemID=9454) są lokalnymi zmiennymi T-SQL, które mogą przechowywać dane tabelaryczne, zbliżone do [tabel tymczasowych](http://www.sqlteam.com/item.asp?ItemID=2029). Takie podejście działa równie dobrze w przypadku obu Microsoft SQL Server 2005 i SQL Server 2000 (natomiast podejście `ROW_NUMBER()` działa tylko z SQL Server 2005).  
  
  Dobrym pomysłem jest utworzenie zmiennej tabeli, która ma `IDENTITY` kolumny i kolumny dla kluczy podstawowych tabeli, których dane są stronicowane. Następnie zawartość tabeli, której dane są stronicowane, jest zrzucana do zmiennej tabeli, w związku z czym kojarzy sekwencyjny indeks wierszy (za pośrednictwem kolumny `IDENTITY`) dla każdego rekordu w tabeli. Po wypełnieniu zmiennej tabeli należy wykonać instrukcję `SELECT` na zmiennej tabeli, która jest dołączona do źródłowej tabeli, aby pobrać określone rekordy. Instrukcja `SET ROWCOUNT` służy do inteligentnego ograniczania liczby rekordów, które należy zrzucić do zmiennej tabeli.  
  
  Ta metoda efektywności s jest oparta na żądanym numerze strony, ponieważ `SET ROWCOUNT` wartością jest przypisana wartość początkowego indeksu wierszy i Maksymalna liczba wierszy. Podczas stronicowania za pomocą stron o niskich numerach, takich jak pierwsze kilka stron danych, to podejście jest bardzo wydajne. Jednak podczas pobierania strony blisko końca wykazuje domyślną wydajność podobną do stronicowania.

Ten samouczek implementuje niestandardowe stronicowanie za pomocą słowa kluczowego `ROW_NUMBER()`. Aby uzyskać więcej informacji na temat używania zmiennej tabeli i techniki `SET ROWCOUNT`, zobacz [bardziej wydajną metodę stronicowania w dużych zestawach wyników](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

Słowo kluczowe `ROW_NUMBER()` skojarzono klasyfikację z każdym rekordem zwracanym w ramach określonej kolejności przy użyciu następującej składni:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` zwraca wartość liczbową, która określa rangę każdego rekordu w odniesieniu do wskazanej kolejności. Na przykład, aby zobaczyć rangę dla każdego produktu, uporządkowaną od najtańszych do najmniejszych, możemy użyć następującej kwerendy:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Rysunek 5 przedstawia wyniki zapytania s w przypadku uruchomienia za pomocą okna zapytania w programie Visual Studio. Należy zwrócić uwagę, że produkty są uporządkowane według ceny i rangi cenowej dla każdego wiersza.

![Ranga ceny jest uwzględniana dla każdego zwróconego rekordu](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Rysunek 5**. ranga ceny jest uwzględniana dla każdego zwróconego rekordu

> [!NOTE]
> `ROW_NUMBER()` to tylko jedna z wielu nowych funkcji klasyfikacji dostępnych w SQL Server 2005. Dokładniejsze Omówienie `ROW_NUMBER()`, wraz z innymi funkcjami rankingu, można znaleźć, [zwracając wyniki rankingu do Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).

Podczas klasyfikowania wyników według określonej `ORDER BY` kolumnie w klauzuli `OVER` (`UnitPrice`, w powyższym przykładzie), SQL Server musi sortować wyniki. Jest to szybka operacja, jeśli istnieje klastrowany indeks względem kolumn, w których wyniki są uporządkowane przez program, lub jeśli istnieje indeks obejmujący, ale może być bardziej kosztowny w inny sposób. Aby zwiększyć wydajność dla dostatecznie dużych zapytań, należy rozważyć dodanie indeksu nieklastrowanego dla kolumny, według której mają być uporządkowane wyniki. Zobacz [funkcje klasyfikacji i wydajność w SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) , aby uzyskać bardziej szczegółowy opis zagadnień dotyczących wydajności.

Informacji o klasyfikowaniu zwracanych przez `ROW_NUMBER()` nie można bezpośrednio użyć w klauzuli `WHERE`. Można jednak użyć tabeli pochodnej do zwrócenia wyniku `ROW_NUMBER()`, który następnie może być wyświetlany w klauzuli `WHERE`. Na przykład następujące zapytanie używa tabeli pochodnej do zwrócenia kolumn ProductName i CenaJednostkowa, wraz z wynikiem `ROW_NUMBER()`, a następnie używa klauzuli `WHERE` w celu zwrócenia tylko tych produktów, których ranga ceny wynosi od 11 do 20:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Przedłużenie tego pojęcia w ten sposób, możemy wykorzystać to podejście do pobrania określonej strony danych z uwzględnieniem żądanego indeksu wierszy początkowych i wartości maksymalnej liczby wierszy:

[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Ponieważ w tym samouczku zobaczymy, że *`StartRowIndex`* dostarczone przez element ObjectDataSource jest indeksowany, rozpoczynając od zera, podczas gdy wartość `ROW_NUMBER()` zwracana przez SQL Server 2005 jest indeksowana od 1. W związku z tym, klauzula `WHERE` zwraca te rekordy, w których `PriceRank` jest ściśle większa niż *`StartRowIndex`* i mniejsze niż lub równe *`StartRowIndex`*  +  *`MaximumRows`* .

Teraz, gdy omówiono, jak `ROW_NUMBER()` można użyć do pobrania określonej strony danych, w której znajduje się indeks wierszy początkowych i wartości maksymalne wierszy, teraz musimy zaimplementować tę logikę jako metody w DAL i LOGIKI biznesowej.

Podczas tworzenia tego zapytania musimy określić kolejność, według której będą klasyfikowane wyniki. Posortuj produkty według ich nazwy w kolejności alfabetycznej. Oznacza to, że w przypadku niestandardowej implementacji stronicowania w tym samouczku nie będzie można utworzyć niestandardowego raportu stronicowanego niż można również posortować. W następnym samouczku zobaczymy, jak można zapewnić takie funkcje.

W poprzedniej sekcji utworzyliśmy metodę DAL jako instrukcję ad-hoc języka SQL. Niestety, parser T-SQL w programie Visual Studio używany przez kreatora TableAdapter jest nietak podobny do składni `OVER` używanej przez funkcję `ROW_NUMBER()`. W związku z tym należy utworzyć tę metodę DAL jako procedurę składowaną. Wybierz Eksplorator serwera z menu Widok (lub naciśnij kombinację klawiszy CTRL + ALT + S) i rozwiń węzeł `NORTHWND.MDF`. Aby dodać nową procedurę składowaną, kliknij prawym przyciskiem myszy węzeł procedury składowane i wybierz polecenie Dodaj nową procedurę składowaną (zobacz rysunek 6).

![Dodaj nową procedurę składowaną na potrzeby stronicowania za pomocą produktów](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Ilustracja 6**. Dodawanie nowej procedury składowanej w celu stronicowania za pomocą produktów

Ta procedura składowana powinna akceptować dwa parametry wejściowe liczby całkowitej — `@startRowIndex` i `@maximumRows` i korzystać z funkcji `ROW_NUMBER()` uporządkowanej według pola `ProductName`, zwracając tylko te wiersze, które są większe niż określona `@startRowIndex` i mniejsze niż lub równe `@startRowIndex` + `@maximumRow` s. Wprowadź następujący skrypt do nowej procedury składowanej, a następnie kliknij ikonę Zapisz, aby dodać procedurę przechowywaną do bazy danych.

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Po utworzeniu procedury składowanej Poświęć chwilę na jej przetestowanie. Kliknij prawym przyciskiem myszy nazwę `GetProductsPaged` procedury składowanej w Eksplorator serwera i wybierz opcję Execute (wykonaj). Program Visual Studio wyświetli monit o podanie parametrów wejściowych, `@startRowIndex` i `@maximumRow` s (patrz rysunek 7). Spróbuj użyć różnych wartości i przeanalizować wyniki.

![Wprowadź wartość parametrów @startRowIndex i @maximumRows](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>Rysunek 7</strong>. wprowadzanie wartości @startRowIndex i @maximumRows parametrów

Po wybraniu tych wartości parametrów wejściowych w oknie danych wyjściowych zostaną wyświetlone wyniki. Rysunek 8 pokazuje wyniki podczas przekazywania 10 dla parametrów `@startRowIndex` i `@maximumRows`.

[![rekordy, które pojawią się na drugiej stronie danych są zwracane](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Ilustracja 8**. zwracane są rekordy, które pojawią się na drugiej stronie danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))

Po utworzeniu tej procedury składowanej zostanie ona ponownie gotowa do utworzenia metody `ProductsTableAdapter`. Otwórz zestaw danych o określonym `Northwind.xsd`, kliknij prawym przyciskiem myszy w `ProductsTableAdapter`, a następnie wybierz opcję Dodaj zapytanie. Zamiast tworzyć zapytania przy użyciu instrukcji SQL ad hoc, utwórz ją za pomocą istniejącej procedury składowanej.

![Utwórz metodę DAL za pomocą istniejącej procedury składowanej](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Ilustracja 9**. Tworzenie metody dal za pomocą istniejącej procedury składowanej

Następnie zostanie wyświetlony monit o wybranie procedury składowanej do wywołania. Wybierz procedurę składowaną `GetProductsPaged` z listy rozwijanej.

![Wybierz procedurę składowaną GetProductsPaged z listy rozwijanej](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Ilustracja 10**. Wybierz procedurę składowaną GetProductsPaged z listy rozwijanej

Na następnym ekranie pojawi się monit z pytaniem o rodzaj danych zwracanych przez procedurę składowaną: dane tabelaryczne, pojedynczą wartość lub brak wartości. Ponieważ `GetProductsPaged` procedury składowanej może zwracać wiele rekordów, wskaż, że zwraca dane tabelaryczne.

![Wskaż, że procedura składowana zwraca dane tabelaryczne](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Ilustracja 11**. wskazuje, że procedura składowana zwraca dane tabelaryczne

Na koniec wskaż nazwy metod, które chcesz utworzyć. Tak jak w przypadku wcześniejszych samouczków, przejdź dalej i twórz metody przy użyciu wypełniania elementu DataTable i zwracają element DataTable. Nazwij pierwszą metodę `FillPaged` i drugi `GetProductsPaged`.

![Nazwij metody FillPaged i GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Ilustracja 12**. nazwy metod FillPaged i GetProductsPaged

Oprócz utworzenia metody DAL w celu zwrócenia określonej strony produktów, należy również podać takie funkcje w LOGIKI biznesowej. Podobnie jak w przypadku metody DAL, Metoda GetProductsPaged LOGIKI biznesowej, musi akceptować dwie liczby całkowite, aby określić indeks wierszy początkowych i maksymalną liczbę wierszy, i musi zwrócić tylko te rekordy, które mieszczą się w określonym zakresie. Utwórz taką metodę LOGIKI biznesowej w klasie ProductsBLL, która powoduje jedynie wywołanie metody DAL s GetProductsPaged, na przykład:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Możesz użyć dowolnej nazwy dla parametrów danych wejściowych metody LOGIKI biznesowej, ale w miarę jak zobaczymy, że wybierzesz opcję użycia `startRowIndex` i `maximumRows` zapisuje z dodatkowej liczby pracy podczas konfigurowania elementu ObjectDataSource do korzystania z tej metody.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Krok 4. Konfigurowanie elementu ObjectDataSource do używania niestandardowego stronicowania

Przy użyciu metod LOGIKI biznesowej i DAL w celu uzyskania dostępu do określonego podzbioru rekordów jest gotowy do utworzenia kontrolki GridView, która umożliwia wykonywanie stron za pośrednictwem własnych rekordów. Aby rozpocząć, Otwórz stronę `EfficientPaging.aspx` w folderze `PagingAndSorting`, Dodaj widok GridView do strony i skonfiguruj go tak, aby korzystał z nowej kontrolki ObjectDataSource. We wcześniejszych samouczkach często mamy skonfigurowany element ObjectDataSource do korzystania z metody `GetProducts` `ProductsBLL` Class. Tym razem zamiast tego chcemy użyć metody `GetProductsPaged`, ponieważ metoda `GetProducts` zwraca *wszystkie* produkty z bazy danych, a `GetProductsPaged` zwraca tylko określony podzbiór rekordów.

![Skonfiguruj element ObjectDataSource do używania metody GetProductsPaged klasy ProductsBLL](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Ilustracja 13**. Konfigurowanie elementu ObjectDataSource do używania metody GetProductsPaged klasy ProductsBLL

Ponieważ ponownie utworzysz widok GridView tylko do odczytu, poświęć chwilę, aby ustawić listę rozwijaną metody na karcie Wstawianie, aktualizowanie i usuwanie do (brak).

Następnie Kreator ObjectDataSource monituje nas o źródła `GetProductsPaged` metody `startRowIndex` i `maximumRows` wartości parametrów wejściowych. Te parametry wejściowe będą faktycznie ustawiane automatycznie przez GridView, więc po prostu pozostaw Źródło ustawioną na None i kliknij przycisk Zakończ.

![Pozostaw źródła parametrów wejściowych jako brak](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Ilustracja 14**. pozostawienie źródła parametru wejściowego jako None

Po ukończeniu działania kreatora ObjectDataSource, GridView będzie zawierać BoundField lub CheckBoxField dla każdego z pól danych produktu. Możesz swobodnie dopasować wygląd widoku GridView w miarę dopasowania. Wybiorę opcję wyświetlania tylko `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`i `UnitPrice` BoundFields. Ponadto skonfiguruj widok GridView, aby obsługiwał stronicowanie, zaznaczając pole wyboru Włącz stronicowanie w jego tagu inteligentnym. Po wprowadzeniu tych zmian znaczniki deklaratywne GridView i ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

W przypadku odwiedzania strony za pomocą przeglądarki, w widoku GridView nie ma miejsca do znalezienia.

![Widok GridView nie jest wyświetlany](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Ilustracja 15**. widok GridView nie jest wyświetlany

Brak elementu GridView, ponieważ element ObjectDataSource używa obecnie 0 jako wartości dla obu `startRowIndex` `GetProductsPaged` i `maximumRows` parametrów wejściowych. W związku z tym wyniki zapytania SQL nie zwracają żadnych rekordów i w związku z tym widok GridView nie jest wyświetlany.

Aby to naprawić, należy skonfigurować element ObjectDataSource do używania niestandardowego stronicowania. Można to zrobić w następujących krokach:

1. **Ustaw właściwość `EnablePaging` ObjectDataSource s tak, aby `true`** wskazywała na element ObjectDataSource, który musi zostać przekazany do `SelectMethod` dwa dodatkowe parametry: jeden do określenia początkowy indeks wiersza ([`StartRowIndexParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) i drugi, aby określić maksymalną liczbę wierszy ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Ustaw właściwości `StartRowIndexParameterName` i `MaximumRowsParameterName` elementów ObjectDataSource odpowiednio** `StartRowIndexParameterName` i `MaximumRowsParameterName` właściwości wskazują nazwy parametrów wejściowych, które są przesyłane do `SelectMethod` do celów stronicowania niestandardowego. Domyślnie te nazwy parametrów są `startIndexRow` i `maximumRows`, co oznacza, że podczas tworzenia metody `GetProductsPaged` w LOGIKI biznesowej użyto tych wartości parametrów wejściowych. W przypadku wybrania opcji używania innych nazw parametrów dla metody LOGIKI biznesowej s `GetProductsPaged`, takiej jak `startIndex` i `maxRows`, na przykład trzeba będzie odpowiednio ustawić właściwości ObjectDataSource s `StartRowIndexParameterName` i `MaximumRowsParameterName` (na przykład startIndex dla `StartRowIndexParameterName` i maxRows dla `MaximumRowsParameterName`).
3. **Ustaw właściwość ObjectDataSource s [`SelectCountMethod`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) na nazwę metody, która zwraca łączną liczbę rekordów, które są stronicowane za pośrednictwem (`TotalNumberOfProducts`)** , aby Metoda `TotalNumberOfProducts` `ProductsBLL` klasy s zwracała łączną liczbę rekordów, które są stronicowane za pomocą metody dal, która wykonuje kwerendę `SELECT COUNT(*) FROM Products`. Te informacje są konieczne przez element ObjectDataSource w celu poprawnego renderowania interfejsu stronicowania.
4. **Usuń `startRowIndex` i `maximumRows` `<asp:Parameter>` elementów z znacznika deklaracyjnego ObjectDataSource** podczas konfigurowania elementu ObjectDataSource za pośrednictwem kreatora, Visual Studio automatycznie dodaliśmy dwa elementy `<asp:Parameter>` dla parametrów wejściowych metody `GetProductsPaged`. Ustawienie `EnablePaging` na `true`powoduje automatyczne przekazanie tych parametrów; Jeśli pojawiają się one również we składni deklaratywnej, element ObjectDataSource podejmie próbę przekazania *czterech* parametrów do metody `GetProductsPaged` i dwóch parametrów do metody `TotalNumberOfProducts`. Jeśli zapomnisz usunąć te `<asp:Parameter>` elementy, podczas odwiedzania strony przez przeglądarkę zostanie wyświetlony komunikat o błędzie: element *ObjectDataSource "ObjectDataSource1" nie może znaleźć niegenerycznej metody "TotalNumberOfProducts", która ma parametry: StartRowIndex, MaximumRows*.

Po wprowadzeniu tych zmian składnia deklaracyjnego elementu ObjectDataSource powinna wyglądać następująco:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Należy zauważyć, że właściwości `EnablePaging` i `SelectCountMethod` zostały ustawione, a elementy `<asp:Parameter>` zostały usunięte. Rysunek 16 przedstawia zrzut ekranu okno Właściwości po wprowadzeniu tych zmian.

![Aby użyć stronicowania niestandardowego, skonfiguruj kontrolkę ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Ilustracja 16**. Aby użyć stronicowania niestandardowego, skonfiguruj kontrolkę ObjectDataSource

Po wprowadzeniu tych zmian odwiedź tę stronę za pomocą przeglądarki. Powinny pojawić się na liście 10 produktów uporządkowane alfabetycznie. Poświęć chwilę na przechodzenie między danymi pojedynczo. Chociaż nie ma różnic wizualnych od perspektywy użytkownika końcowego między domyślnym stronicowaniem a stronicowaniem niestandardowym, niestandardowe stronicowanie bardziej wydajne stron za pośrednictwem dużych ilości danych, ponieważ pobiera tylko te rekordy, które muszą być wyświetlane dla danej strony.

[![dane uporządkowane według nazwy produktu, są stronicowane przy użyciu niestandardowego stronicowania](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Ilustracja 17**. dane uporządkowane według nazwy produktu są stronicowane przy użyciu niestandardowego stronicowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))

> [!NOTE]
> Dzięki stronicowaniu niestandardowym wartość liczby stron zwracana przez `SelectCountMethod` ObjectDataSource s jest przechowywana w stanie widoku GridView. Inne zmienne GridView `PageIndex`, `EditIndex`, `SelectedIndex`, kolekcja `DataKeys` i tak dalej są przechowywane w *stanie kontroli*, który jest utrwalany niezależnie od wartości właściwości GridView-`EnableViewState`. Ponieważ wartość `PageCount` jest utrwalana w przypadku ogłaszania zwrotnego przy użyciu stanu widoku, podczas korzystania z interfejsu stronicowania, który zawiera link umożliwiający przejście do ostatniej strony, należy sprawdzić, czy stan widoku GridView jest włączony. (Jeśli interfejs stronicowania nie zawiera bezpośredniego linku do ostatniej strony, można wyłączyć stan widoku).

Kliknięcie linku Ostatnia strona powoduje ogłoszenie zwrotne, a następnie wyświetlenie widoku GridView w celu zaktualizowania jego właściwości `PageIndex`. Jeśli zostanie kliknięty link Ostatnia strona, GridView przypisze jej Właściwość `PageIndex` do wartości mniejszej niż jej Właściwość `PageCount`. Po wyłączeniu stanu widoku wartość `PageCount` zostanie utracona na stronach ogłaszania zwrotnego, a `PageIndex` w zamian zostanie przypisana wartość maksymalna liczba całkowita. Następnie GridView próbuje określić początkowy indeks wiersza, mnożąc `PageSize` i `PageCount` właściwości. Powoduje to `OverflowException`, ponieważ produkt przekracza maksymalny dozwolony rozmiar w postaci liczby całkowitej.

## <a name="implement-custom-paging-and-sorting"></a>Implementowanie niestandardowego stronicowania i sortowania

Nasza bieżąca implementacja stronicowania niestandardowego wymaga, aby kolejność, w której dane są stronicowane, była określana statycznie podczas tworzenia procedury składowanej `GetProductsPaged`. Jednak można zauważyć, że tag "GridView" w widoku inteligentnym zawiera pole wyboru Włącz sortowanie obok opcji Włącz stronicowanie. Niestety dodanie obsługi sortowania do widoku GridView przy użyciu bieżącej implementacji stronicowania niestandardowego spowoduje posortowanie rekordów tylko na aktualnie wyświetlanej stronie danych. Na przykład w przypadku skonfigurowania widoku GridView do obsługi stronicowania, a następnie podczas wyświetlania pierwszej strony danych Sortuj według nazwy produktu w kolejności malejącej, spowoduje to odwrócenie kolejności produktów na stronie 1. Jak pokazano na rysunku 18, takie pokazuje Carnarvon Tigers jako pierwszy produkt podczas sortowania w odwrotnej kolejności alfabetycznej, która ignoruje 71 inne produkty, które są dostępne po Carnarvon Tigers, alfabetycznie; tylko rekordy na pierwszej stronie są brane pod uwagę podczas sortowania.

[posortowane są tylko dane wyświetlane na bieżącej stronie ![](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Ilustracja 18**. posortowane są tylko dane wyświetlane na bieżącej stronie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))

Sortowanie ma zastosowanie tylko do bieżącej strony danych, ponieważ sortowanie jest wykonywane po pobraniu danych z metody LOGIKI biznesowej s `GetProductsPaged` i ta metoda zwraca tylko te rekordy dla określonej strony. Aby poprawnie zaimplementować sortowanie, musimy przekazać wyrażenie sortowania do metody `GetProductsPaged`, tak aby dane mogły być odpowiednio klasyfikowane przed zwróceniem określonej strony danych. Zobaczymy, jak to zrobić w następnym samouczku.

## <a name="implementing-custom-paging-and-deleting"></a>Implementowanie niestandardowego stronicowania i usuwanie

W przypadku włączenia funkcji usuwania w widoku GridView, którego dane są stronicowane przy użyciu niestandardowych technik stronicowania, podczas usuwania ostatniego rekordu z ostatniej strony, zobaczysz, że nie zostanie odpowiednio zmniejszony `PageIndex`GridView. Aby odtworzyć tę usterkę, Włącz opcję usuwania dla właśnie utworzonego samouczka. Przejdź do ostatniej strony (strona 9), w której powinien zostać wyświetlony pojedynczy produkt, ponieważ jest stronicowany przez 81 produktów, 10 produktów jednocześnie. Usuń ten produkt.

Po usunięciu ostatniego produktu, widok GridView *powinien* automatycznie przechodzić do ósmej strony i takie funkcje są przedstawiane jako domyślne stronicowanie. Po usunięciu niestandardowego stronicowania, gdy usuniesz ten ostatni produkt na ostatniej stronie, widok GridView po prostu znika z ekranu. Dokładną przyczyną tego, że dzieje *się tak,* jest bit wykraczający poza zakres tego samouczka; Zobacz [Usuwanie ostatniego rekordu na ostatniej stronie z widoku GridView z niestandardowym stronicowaniem](http://scottonwriting.net/sowblog/posts/7326.aspx) , aby poznać szczegóły niskiego poziomu dotyczące źródła tego problemu. Podsumowując to s z powodu następującej sekwencji kroków, które są wykonywane przez GridView po kliknięciu przycisku Usuń:

1. Usuń rekord
2. Pobierz odpowiednie rekordy do wyświetlenia dla określonych `PageIndex` i `PageSize`
3. Upewnij się, że `PageIndex` nie przekracza liczby stron danych w źródle danych. Jeśli tak, automatycznie Zmniejsz Właściwość `PageIndex` GridView
4. Powiąż odpowiednią stronę danych z GridView przy użyciu rekordów uzyskanych w kroku 2

Problem wynika z faktu, że w kroku 2 `PageIndex` używany podczas przechwyconia rekordów do wyświetlenia jest w dalszym ciągu `PageIndex` ostatniej strony, której jedyny rekord został właśnie usunięty. W związku z tym w kroku 2 *żadne rekordy nie* są zwracane, ponieważ Ostatnia strona danych nie zawiera już żadnych rekordów. Następnie w kroku 3 widok GridView realizuje, że jego właściwość `PageIndex` jest większa niż łączna liczba stron w źródle danych (od momentu usunięcia ostatniego rekordu z ostatniej strony) i w związku z tym zmniejsza jego właściwość `PageIndex`. W kroku 4 GridView próbuje powiązać się z danymi pobranymi w kroku 2. Jednakże w kroku 2 żadne rekordy nie zostały zwrócone, w związku z czym wynikiem jest pusty widok GridView. W przypadku domyślnego stronicowania ten problem nie występuje, ponieważ w kroku 2 *wszystkie* rekordy są pobierane ze źródła danych.

Aby rozwiązać ten problem, dostępne są dwie opcje. Pierwszym z nich jest utworzenie procedury obsługi zdarzeń dla programu obsługi zdarzeń `RowDeleted` GridView, która określa, ile rekordów było wyświetlanych na stronie, która została właśnie usunięta. Jeśli istnieje tylko jeden rekord, usunięty rekord musi być ostatnim z nich i musimy zmniejszyć `PageIndex`GridView. Oczywiście chcemy tylko zaktualizować `PageIndex`, jeśli operacja usuwania zakończyła się pomyślnie, co można ustalić, upewniając się, że właściwość `e.Exception` jest `null`.

Takie podejście działa, ponieważ aktualizuje `PageIndex` po kroku 1, ale przed krok 2. W związku z tym, w kroku 2, zwracany jest odpowiedni zestaw rekordów. Aby to osiągnąć, użyj następującego kodu:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Alternatywnym obejściem jest utworzenie procedury obsługi zdarzeń dla zdarzenia ObjectDataSource s `RowDeleted` i ustawienie właściwości `AffectedRows` na wartość 1. Po usunięciu rekordu w kroku 1 (ale przed ponownym pobraniem danych w kroku 2) widok GridView aktualizuje swoją właściwość `PageIndex`, jeśli operacja ma wpływ na co najmniej jeden wiersz. Jednak Właściwość `AffectedRows` nie jest ustawiana przez element ObjectDataSource i w związku z tym ten krok zostanie pominięty. Jednym ze sposobów wykonania tego kroku jest ręczne ustawienie właściwości `AffectedRows`, jeśli operacja usuwania zostanie zakończona pomyślnie. Można to zrobić przy użyciu kodu, takiego jak następujące:

[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Kod dla obu tych programów obsługi zdarzeń można znaleźć w klasie związanej z kodem `EfficientPaging.aspx` przykładu.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Porównywanie wydajności domyślnego i niestandardowego stronicowania

Ponieważ stronicowanie niestandardowe pobiera tylko niezbędne rekordy, natomiast domyślne stronicowanie zwraca *wszystkie* rekordy dla każdej przeglądanej strony, co oznacza, że niestandardowe stronicowanie jest bardziej wydajne niż domyślne stronicowanie. Ale tylko wtedy, gdy jest to niestandardowe stronicowanie? Jakiego rodzaju zyski wydajności mogą być widoczne przez przechodzenie z domyślnego stronicowania do niestandardowego stronicowania?

Niestety, żadna odpowiedź nie pasuje do żadnej z nich. Wzrost wydajności zależy od wielu czynników, a najprawdopodobniej dwie to liczba rekordów, które są umieszczane w stronicowaniu, a obciążenie umieszczone na serwerze bazy danych i kanałach komunikacji między serwerem sieci Web a serwerem baz danych. W przypadku małych tabel zawierających zaledwie kilka dziesiątych rekordów różnica wydajności może być niewielka. W przypadku dużych tabel, w przypadku tysięcy do setek tysięcy wierszy, różnica wydajności jest ostra.

Artykuł kopalni, [niestandardowe stronicowanie w ASP.NET 2,0 z SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), zawiera pewne testy wydajności, które zostały uruchomione, aby wyróżnić różnice wydajności między tymi dwiema technikami stronicowania podczas stronicowania za pomocą tabeli bazy danych z 50 000 rekordy. W tych testach przebadałem czas wykonywania zapytania na poziomie SQL Server (za pomocą [programu SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) i na stronie ASP.NET przy użyciu [funkcji śledzenia ASP.NET s](https://msdn.microsoft.com/library/y13fw6we.aspx). Należy pamiętać, że te testy zostały uruchomione w moim pudełku deweloperskim z pojedynczym aktywnym użytkownikiem i dlatego są nienaukowe i nie są naśladować typowych wzorców obciążenia witryny sieci Web. Niezależnie od tego wyniki ilustrują względne różnice w czasie wykonywania dla domyślnego i niestandardowego stronicowania podczas pracy z dostatecznie dużymi ilościami danych.

|  | **Średni czas trwania (s)** | **Odczytywan** |
| --- | --- | --- |
| **Domyślny Profiler SQL stronicowania** | 1.411 | 383 |
| **Profiler SQL stronicowania niestandardowego** | 0.002 | 29 |
| **Domyślny ślad ASP.NET stronicowania** | 2.379 | *Nie dotyczy* |
| **Niestandardowe stronicowanie ASP.NET śledzenia** | 0.029 | *Nie dotyczy* |

Jak widać, pobieranie określonej strony danych wymaga 354 mniej odczytów średnio i zakończonych w części czasu. Na stronie ASP.NET niestandardowa strona była w stanie renderować w pobliżu o 1/100% czasu, jaki zajęło<sup>Korzystanie z domyślnego</sup> stronicowania. Zobacz [mój artykuł](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) , aby uzyskać więcej informacji na temat tych wyników wraz z kodem i bazą danych, którą można pobrać, aby odtworzyć te testy w Twoim środowisku.

## <a name="summary"></a>Podsumowanie

Domyślne stronicowanie to cinch do wdrożenia po prostu zaznacz pole wyboru Włącz stronicowanie w tagu inteligentnym formant sieci Web danych, ale taka prostota ma koszt wydajności. Za pomocą domyślnego stronicowania, gdy użytkownik zażąda dowolnej strony danych, *wszystkie* rekordy są zwracane, nawet jeśli można pokazać tylko niewielki ułamek. Aby zwalczać ten koszt wydajności, element ObjectDataSource oferuje alternatywne niestandardowe stronicowanie opcji stronicowania.

Chociaż niestandardowe stronicowanie zwiększa się w przypadku domyślnych problemów z wydajnością stronicowania, pobierając tylko te rekordy, które mają być wyświetlane, to nie jest więcej związane z implementacją niestandardowego stronicowania. Najpierw należy napisać zapytanie, które poprawnie (i efektywnie) uzyskuje dostęp do określonego podzestawu żądanych rekordów. Można to zrobić na wiele sposobów. Ten samouczek został sprawdzony w tym samouczku, aby użyć SQL Server 2005 s nowej funkcji `ROW_NUMBER()` do określania rangi wyników, a następnie zwrócenia tylko tych wyników, których klasyfikacja mieści się w określonym zakresie. Ponadto musimy dodać metodę, aby określić łączną liczbę rekordów, które są stronicowane. Po utworzeniu tych metod DAL i LOGIKI biznesowej należy również skonfigurować element ObjectDataSource, aby można było określić, ile łącznych rekordów jest stronicowanych, i można prawidłowo przekazać indeks wierszy początkowych i wartości maksymalne wierszy do LOGIKI biznesowej.

Podczas implementowania niestandardowego stronicowania jest wymagane wykonanie kilku kroków i nie jest niemal proste jako stronicowanie domyślne, niestandardowe stronicowanie jest konieczność podczas stronicowania za pomocą wystarczająco dużych ilości danych. W miarę jak pokazano, niestandardowe stronicowanie może wychodzić kilka sekund od czasu renderowania strony ASP.NET i zwiększyć obciążenie serwera bazy danych o jedną rudy większej liczby zamówień.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](paging-and-sorting-report-data-vb.md)
> [dalej](sorting-custom-paged-data-vb.md)

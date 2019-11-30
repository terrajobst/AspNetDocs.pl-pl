---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Korzystanie z strony wzorcowej ze strony zawartości (VB) | Microsoft Docs
author: rick-anderson
description: Bada, jak wywoływać metody, ustawiać właściwości, itd. strony wzorcowej z kodu na stronie zawartości.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: fe1f7b80b26ff25c1ce744e4f823e3fb35eea074
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74577288"
---
# <a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interakcja ze stroną wzorcową z poziomu strony zawartości (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Bada, jak wywoływać metody, ustawiać właściwości, itd. strony wzorcowej z kodu na stronie zawartości.

## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich pięciu samouczków oglądamy sposób tworzenia strony wzorcowej, definiowania obszarów zawartości, wiązania ASP.NET stron ze stroną wzorcową i definiowania zawartości specyficznej dla strony. Gdy osoba odwiedzająca żąda określonej strony zawartości, znaczniki zawartości i stron wzorcowych są odrzucane w czasie wykonywania, co spowodowało renderowanie ujednoliconej hierarchii formantów. W związku z tym już widzimy, w jaki sposób Strona wzorcowa i jedna z jej stron zawartości mogą się z nią współdziałać: Strona zawartości sprawdza adiustację, aby odmówić, że jest to formant ContentPlaceHolder na stronie wzorcowej.

Co jeszcze sprawdzimy, jak strona wzorcowa i strona zawartości mogą programistycznie współdziałać. Oprócz definiowania znaczników dla kontrolek ContentPlaceHolder na stronie wzorcowej, strona zawartości może również przypisywać wartości do właściwości publicznych swojej strony wzorcowej i wywoływać metody publiczne. Podobnie Strona wzorcowa może współdziałać ze swoimi stronami zawartości. Gdy interakcja programowa między wzorcem a stroną zawartości jest mniej wspólna niż interakcja między nimi, istnieje wiele scenariuszy, w których ta interakcja programowa jest wymagana.

W tym samouczku sprawdzimy, jak strona zawartości może programowo współdziałać z jej stroną wzorcową. w następnym samouczku dowiesz się, jak strona wzorcowa może w podobny sposób korzystać z jej stron zawartości.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Przykłady interakcji programistycznej między stroną zawartości a jej stroną wzorcową

Gdy określony region strony musi być skonfigurowany na podstawie strony na stronie, używamy kontrolki ContentPlaceHolder. Ale co się stało z sytuacją, w której większość stron musi emitować niektóre dane wyjściowe, ale niewielka liczba stron, którą trzeba dostosować, aby pokazać coś innego? Taki przykład, który został zbadany w samouczku [*wielu elementów ContentPlaceHolders i domyślnej zawartości*](multiple-contentplaceholders-and-default-content-vb.md) , obejmuje wyświetlanie interfejsu logowania na każdej stronie. Chociaż większość stron powinna zawierać interfejs logowania, należy go pominąć dla kilku stron, takich jak: główna strona logowania (`Login.aspx`); na stronie Tworzenie konta; i inne strony, które są dostępne tylko dla uwierzytelnionych użytkowników. Samouczek [*wielu elementów ContentPlaceHolders i domyślnej zawartości*](multiple-contentplaceholders-and-default-content-vb.md) przedstawia sposób definiowania zawartości domyślnej dla elementu ContentPlaceHolder na stronie wzorcowej, a następnie sposobu przesłonięcia go na tych stronach, w których zawartość domyślna nie była pożądana.

Innym rozwiązaniem jest utworzenie publicznej właściwości lub metody na stronie głównej, która wskazuje, czy ma być wyświetlany lub ukryty interfejs logowania. Na przykład strona wzorcowa może zawierać właściwość publiczną o nazwie `ShowLoginUI` której wartość została użyta do ustawienia właściwości `Visible` kontrolki logowania na stronie wzorcowej. Te strony zawartości, w których powinien być pominięty interfejs użytkownika logowania, mogą następnie programowo ustawić właściwość `ShowLoginUI` na `False`.

Prawdopodobnie najbardziej typowym przykładem interakcji dotyczącej zawartości i strony głównej występuje, gdy dane wyświetlane na stronie wzorcowej muszą zostać odświeżone po transpired na stronie zawartości. Rozważmy stronę wzorcową zawierającą widok GridView, który wyświetla pięć ostatnio dodanych rekordów z określonej tabeli bazy danych i że jedna z jej stron zawartości zawiera interfejs do dodawania nowych rekordów do tej samej tabeli.

Gdy użytkownik odwiedzi stronę, aby dodać nowy rekord, zobaczy pięć ostatnio dodanych rekordów wyświetlanych na stronie wzorcowej. Po wypełnieniu wartości kolumn nowego rekordu przesyła formularz. Przy założeniu, że element GridView na stronie wzorcowej ma właściwość `EnableViewState` ustawioną na wartość true (domyślnie), jego zawartość jest ponownie ładowana ze stanu widoku i w związku z tym pięć identycznych rekordów jest wyświetlany, nawet jeśli nowszy rekord został właśnie dodany do bazy danych. Może to spowodować odmylić użytkownika.

> [!NOTE]
> Nawet Jeśli wyłączysz stan widoku GridView, tak aby ponownie powiązać się z jego podstawowym źródłem danych przy każdym ogłaszaniu zwrotnym, nadal nie będzie można wyświetlić właśnie dodanego rekordu, ponieważ dane są powiązane z widokiem GridView wcześniej w cyklu życia strony niż w przypadku dodania nowego rekordu do DataB ASE.

Aby rozwiązać ten sposób, tak że dodany rekord zostanie wyświetlony w widoku GridView strony głównej na stronie ogłaszania zwrotnego, musimy poinstruować element GridView o konieczności ponownego powiązania ze źródłem danych *po* dodaniu nowego rekordu do bazy danych. Wymaga to interakcji między stronami zawartości i wzorca, ponieważ interfejs do dodawania nowego rekordu (i jego programów obsługi zdarzeń) znajduje się na stronie zawartości, ale widok GridView, który należy odświeżyć, znajduje się na stronie wzorcowej.

Ponieważ odświeżenie wyświetlania strony wzorcowej z programu obsługi zdarzeń na stronie zawartości jest jednym z najpopularniejszych potrzeb dotyczących interakcji z zawartością i stroną wzorcową, zapoznaj się z tym tematem bardziej szczegółowo. Pobranie dla tego samouczka obejmuje bazę danych Microsoft SQL Server 2005 Express Edition o nazwie `NORTHWIND.MDF` w folderze `App_Data` witryny sieci Web. Baza danych Northwind przechowuje informacje o produktach, pracownikach i sprzedaży fikcyjnej firmy, Northwind Traders.

Krok 1 przeprowadzi Cię przez wyświetlanie pięciu ostatnio dodanych produktów w widoku GridView na stronie wzorcowej. Krok 2 powoduje utworzenie strony zawartości służącej do dodawania nowych produktów. Krok 3. zawarto informacje na temat tworzenia publicznych właściwości i metod na stronie głównej, a krok 4 ilustruje programistyczny interfejs z tymi właściwościami i metodami ze strony zawartość.

> [!NOTE]
> Ten samouczek nie zawiera elementów roboczych związanych z pracą z danymi w ASP.NET. Procedura konfigurowania strony wzorcowej w celu wyświetlania danych i strony zawartości służącej do wstawiania danych jest kompletna, ale Breezy. Aby uzyskać bardziej szczegółowe informacje na temat wyświetlania i wstawiania danych i korzystania z kontrolek kontrolki SqlDataSource i GridView, zapoznaj się z zasobami w sekcji dalsze informacje na końcu tego samouczka.

## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Krok 1. Wyświetlanie pięciu ostatnio dodanych produktów na stronie wzorcowej

Otwórz stronę wzorcową site. Master i Dodaj etykietę i kontrolkę GridView do `<div>``leftContent`. Wyczyść Właściwość `Text` etykiety, ustaw jej Właściwość `EnableViewState` na `False`i Właściwość `ID` na `GridMessage`. Ustaw właściwość `ID` GridView na `RecentProducts`. Następnie z projektanta rozwiń tag inteligentny GridView i wybierz powiązanie go z nowym źródłem danych. Spowoduje to uruchomienie Kreatora konfiguracji źródła danych. Ponieważ baza danych Northwind w folderze `App_Data` jest bazą danych Microsoft SQL Server, wybierz opcję utworzenia kontrolki SqlDataSource, wybierając pozycję (patrz rysunek 1). Nazwij `RecentProductsDataSource`kontrolki SqlDataSource.

[![powiązać widoku GridView z kontrolką kontrolki SqlDataSource o nazwie RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Ilustracja 01**. powiązanie widoku GridView z kontrolką kontrolki SqlDataSource o nazwie `RecentProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))

Następny krok prosi nas o określenie bazy danych, z którą ma zostać nawiązane połączenie. Z listy rozwijanej wybierz plik bazy danych `NORTHWIND.MDF` i kliknij przycisk Dalej. Ponieważ ta baza danych jest używana po raz pierwszy, Kreator będzie oferować przechowywanie parametrów połączenia w `Web.config`. Przechowuj parametry połączenia przy użyciu nazwy `NorthwindConnectionString`.

[![połączyć się z bazą danych Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Ilustracja 02**. Łączenie się z bazą danych Northwind ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))

Kreator konfiguracji źródła danych udostępnia dwie metody, za pomocą których można określić zapytanie używane do pobierania danych:

- Określenie niestandardowej instrukcji SQL lub procedury składowanej lub
- Wybierając tabelę lub widok, a następnie określając kolumny do zwrócenia

Ponieważ chcemy zwrócić tylko pięć ostatnio dodanych produktów, musimy określić niestandardową instrukcję SQL. Użyj następującego `SELECT` kwerendy:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

Słowo kluczowe `TOP 5` zwraca tylko pięć pierwszych rekordów z zapytania. Klucz podstawowy tabeli `Products`, `ProductID`, jest kolumną `IDENTITY`, która zapewnia nam, że każdy nowy produkt dodany do tabeli będzie miał większą wartość niż poprzedni wpis. W związku z tym sortowanie wyników według `ProductID` w kolejności malejącej zwraca Produkty zaczynające się od ostatnio utworzonych.

[![zwrócić pięć ostatnio dodanych produktów](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Rysunek 03**: Zwróć pięć ostatnio dodanych produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))

Po zakończeniu działania kreatora program Visual Studio generuje dwie BoundFieldsy dla widoku GridView w celu wyświetlenia `ProductName` i `UnitPrice` pól zwróconych z bazy danych. W tym momencie deklaracyjne znaczniki strony wzorcowej powinny zawierać znaczniki podobne do następujących:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Jak widać, znacznik zawiera: kontrolka sieci Web etykiety (`GridMessage`); `RecentProducts`GridView, z dwoma BoundFields; i kontrolka kontrolki SqlDataSource, która zwraca pięć ostatnio dodanych produktów.

Po utworzeniu tego widoku GridView i skonfigurowaniu jego kontrolki kontrolki SqlDataSource odwiedź witrynę internetową za pomocą przeglądarki. Jak pokazano na rysunku 4, w lewym dolnym rogu zobaczysz siatkę zawierającą pięć ostatnio dodanych produktów.

[![GridView wyświetla pięć ostatnio dodanych produktów](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Ilustracja 04**: w widoku GridView są wyświetlane pięć ostatnio dodanych produktów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))

> [!NOTE]
> Możesz wyczyścić wygląd GridView. Niektóre sugestie obejmują formatowanie wyświetlanej wartości `UnitPrice` jako waluty i używanie kolorów i czcionek tła w celu poprawienia wyglądu siatki.

## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Krok 2. Tworzenie strony zawartości w celu dodania nowych produktów

Następnym zadaniem jest utworzenie strony zawartości, za pomocą której użytkownik może dodać nowy produkt do tabeli `Products`. Dodaj nową stronę zawartości do folderu `Admin` o nazwie `AddProduct.aspx`, aby upewnić się, że należy powiązać ją ze stroną wzorcową `Site.master`. Rysunek 5 przedstawia Eksplorator rozwiązań po dodaniu tej strony do witryny sieci Web.

[![dodać nową stronę ASP.NET do folderu Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Ilustracja 05**. Dodawanie nowej strony ASP.NET do folderu `Admin` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))

Odwołaj ten element w [*temacie Określanie tytułu, Meta tagów i innych nagłówków HTML na stronie wzorcowej*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) — samouczek został utworzony za pomocą niestandardowej klasy strony podstawowej o nazwie `BasePage`, która wygenerowała tytuł strony, jeśli nie została ustawiona jawnie. Przejdź do klasy powiązanej z kodem `AddProduct.aspx` strony i utwórz ją od `BasePage` (zamiast z `System.Web.UI.Page`).

Na koniec Zaktualizuj plik `Web.sitemap`, aby zawierał wpis do tej lekcji. Dodaj następujące oznakowanie poniżej `<siteMapNode>` dla błędów nazewnictwa identyfikatora formantu lekcja:

[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Jak pokazano na rysunku 6, Dodawanie tego elementu `<siteMapNode>` jest odzwierciedlone na liście lekcji.

Wróć do `AddProduct.aspx`. W kontrolce zawartości dla `MainContent` ContentPlaceHolder Dodaj kontrolkę DetailsView i nadaj jej nazwę `NewProduct`. Powiąż element DetailsView z nową kontrolką kontrolki SqlDataSource o nazwie `NewProductDataSource`. Podobnie jak w przypadku kontrolki SqlDataSource w kroku 1, należy skonfigurować kreatora tak, aby używał bazy danych Northwind, i wybrać opcję określenia niestandardowej instrukcji SQL. Ponieważ DetailsView zostanie użyty do dodawania elementów do bazy danych, musimy określić zarówno instrukcję `SELECT`, jak i instrukcję `INSERT`. Użyj następującego `SELECT` kwerendy:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Następnie na karcie Wstawianie Dodaj następującą instrukcję `INSERT`:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Po zakończeniu działania kreatora przejdź do tagu inteligentnego DetailsView i zaznacz pole wyboru "Włącz wstawianie". Spowoduje to dodanie CommandField do widoku DetailsView z właściwością `ShowInsertButton` ustawioną na wartość true. Ponieważ ten DetailsView zostanie użyty wyłącznie do wstawiania danych, ustaw właściwość `DefaultMode` DetailsView na `Insert`.

To wszystko. Przetestujmy Tę stronę. Odwiedź witrynę `AddProduct.aspx` za pomocą przeglądarki, wprowadź nazwę i cenę (patrz rysunek 6).

[![dodać nowego produktu do bazy danych](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Ilustracja 06**. Dodawanie nowego produktu do bazy danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))

Po wpisaniu nazwy i ceny nowego produktu kliknij przycisk Wstaw. Powoduje to odświeżenie formularza. Na stronie ogłaszania zwrotnego jest wykonywana instrukcja `INSERT` kontrolki kontrolki SqlDataSource. dwa parametry są wypełniane wartościami wprowadzonymi przez użytkownika w dwóch kontrolkach pola tekstowego DetailsView. Niestety, nie ma wizualizacji wizualnej, która wystąpiła operacja wstawiania. Najprawdopodobniej będzie wyświetlany komunikat z potwierdzeniem, że został dodany nowy rekord. Jestem tym ćwiczeniem dla czytnika. Ponadto po dodaniu nowego rekordu z widoku DetailsView widok GridView na stronie wzorcowej nadal pokazuje te same pięć rekordów jak poprzednio; nie obejmuje to właśnie dodanego rekordu. Sprawdzimy, jak to naprawić w nadchodzących krokach.

> [!NOTE]
> Oprócz dodania formy wizualnej opinii, że Wstawianie zakończyło się pomyślnie, zachęcamy do aktualizowania interfejsu wstawiania w widoku DetailsView w celu uwzględnienia weryfikacji. Obecnie nie jest przeprowadzana Walidacja. Jeśli użytkownik wprowadzi nieprawidłową wartość pola `UnitPrice`, na przykład "zbyt kosztowna", zostanie zgłoszony wyjątek na stronie ogłaszania zwrotnego, gdy system próbuje skonwertować ten ciąg na wartość dziesiętną. Aby uzyskać więcej informacji na temat dostosowywania interfejsu wstawiania, zapoznaj się z [samouczkiem *Dostosowywanie interfejsu modyfikacji danych* ](https://asp.net/learn/data-access/tutorial-20-vb.aspx) z [serii praca z samouczkiem dotyczącym danych](../../data-access/index.md).

## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Krok 3. Tworzenie publicznych właściwości i metod na stronie wzorcowej

W kroku 1 dodaliśmy kontrolkę sieci Web etykieta o nazwie `GridMessage` nad elementem GridView na stronie wzorcowej. Ta etykieta jest przeznaczona do opcjonalnego wyświetlania komunikatu. Na przykład po dodaniu nowego rekordu do tabeli `Products` może być konieczne wyświetlenie komunikatu z komunikatem "*NazwaProduktu* został dodany do bazy danych". Zamiast nadawać twardemu kodowi tekst dla tej etykiety na stronie wzorcowej, możemy chcieć, aby wiadomość była dostosowywana na stronie zawartości.

Ponieważ kontrolka Label jest zaimplementowana jako chroniona zmienna składowa na stronie wzorcowej, nie można uzyskać do niej dostępu bezpośrednio z poziomu stron zawartości. Aby można było korzystać z etykiety na stronie wzorcowej ze strony zawartości (lub, w tym przypadku, każda kontrolka sieci Web na stronie głównej) musi utworzyć właściwość publiczną na stronie wzorcowej, która uwidacznia formant sieci Web lub służy jako serwer proxy, za pomocą którego jedna z jego właściwości może być  otworzyć. Dodaj następującą składnię do klasy powiązanej z kodem strony wzorcowej, aby uwidocznić Właściwość `Text` etykiety:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Gdy nowy rekord zostanie dodany do tabeli `Products` ze strony zawartości, `RecentProducts` GridView na stronie wzorcowej musi ponownie powiązać się ze swoim podstawowym źródłem danych. Aby ponownie powiązać element GridView, wywołaj jego metodę `DataBind`. Ponieważ GridView na stronie wzorcowej nie jest programistycznie dostępny dla stron zawartości, musimy utworzyć metodę publiczną na stronie wzorcowej, która po wywołaniu powoduje ponowne powiązanie danych z elementem GridView. Dodaj następującą metodę do klasy z kodem powiązanej ze stroną wzorcową:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Mając Właściwość `GridMessageText` i metodę `RefreshRecentProductsGrid` na miejscu, każda strona zawartości może programowo ustawić lub odczytać wartość właściwości `Text` etykiety `GridMessage` lub ponownie powiązać dane z `RecentProducts` GridView. Krok 4. badanie, jak uzyskać dostęp do publicznych właściwości i metod strony wzorcowej ze strony zawartości.

> [!NOTE]
> Nie zapomnij oznaczyć właściwości i metod strony wzorcowej jako `Public`. Jeśli te właściwości i metody nie są jawnie oznaczone jako `Public`, nie będą dostępne ze strony zawartość.

## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Krok 4. wywoływanie publicznych członków strony wzorcowej ze strony zawartości

Teraz, gdy Strona główna ma niezbędne właściwości i metody publiczne, możemy wywoływać te właściwości i metody ze strony zawartości `AddProduct.aspx`. W szczególnych przypadkach musimy ustawić właściwość `GridMessageText` strony głównej i wywołać jej metodę `RefreshRecentProductsGrid` po dodaniu nowego produktu do bazy danych. Wszystkie kontrolki sieci Web danych ASP.NET wyzwalają zdarzenia bezpośrednio przed i po wykonaniu różnych zadań, które ułatwiają deweloperom stron wykonywanie niektórych działań programistycznych przed lub po zadaniu. Na przykład gdy użytkownik końcowy kliknie przycisk wstawiania w widoku DetailsView, w przypadku ogłaszania zwrotnego, w trakcie odświeżania, DetailsView wygeneruje zdarzenie `ItemInserting` przed rozpoczęciem wstawiania przepływu pracy. Następnie wstawia rekord do bazy danych programu. Poniżej znajduje się zdarzenie `ItemInserted` w widoku DetailsView. Dlatego w celu pracy z stroną wzorcową po dodaniu nowego produktu Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemInserted` DetailsView.

Istnieją dwa sposoby programistycznego interfejsu strony zawartości z jej stroną wzorcową:

- Za pomocą właściwości `Page.Master`, która zwraca swobodnie wpisane odwołanie do strony głównej lub
- Określ typ strony głównej strony lub ścieżkę do pliku za pomocą dyrektywy `@MasterType`. spowoduje to automatyczne dodanie właściwości o jednoznacznie określonym typie do strony o nazwie `Master`.

Przeanalizujmy oba podejścia.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Korzystanie z właściwości`Page.Master`ze swobodnym typem

Wszystkie strony sieci Web ASP.NET muszą pochodzić od klasy `Page`, która znajduje się w przestrzeni nazw `System.Web.UI`. Klasa `Page` zawiera [właściwość`Master`](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) , która zwraca odwołanie do strony wzorcowej strony. Jeśli strona nie zawiera strony wzorcowej `Master` zwraca `Nothing`.

Właściwość `Master` zwraca obiekt typu [`MasterPage`](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (znajdujący się również w `System.Web.UI` przestrzeni nazw), który jest typem podstawowym, z którego pochodzą wszystkie strony wzorcowe. W związku z tym, aby użyć publicznych właściwości lub metod zdefiniowanych na stronie głównej witryny sieci Web, należy rzutować `MasterPage` obiektu zwróconego z właściwości `Master` na odpowiedni typ. Ze względu na to, że nazywamy nasze `Site.master`pliku strony głównej, Klasa z kodem jest nazywana `Site`. W związku z tym Poniższy kod rzutuje Właściwość `Page.Master` na wystąpienie klasy `Site`.

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Teraz, gdy do typu witryny została powiązana właściwość `Page.Master` o swobodnym typie, możemy odwoływać się do właściwości i metod specyficznych dla lokacji. Jak pokazano na rysunku 7, właściwość publiczna `GridMessageText` pojawia się na liście rozwijanej IntelliSense.

[![technologia IntelliSense wyświetla publiczne właściwości i metody strony głównej](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Ilustracja 07**. Technologia IntelliSense wyświetla publiczne właściwości i metody strony głównej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))

> [!NOTE]
> Jeśli nazwa pliku strony głównej jest nazywana `MasterPage.master`, nazwa klasy na stronie wzorcowej jest `MasterPage`. Może to prowadzić do niejednoznaczności kodu podczas rzutowania z typu `System.Web.UI.MasterPage` do klasy `MasterPage`. Krótko mówiąc, należy w pełni zakwalifikować typ, do którego jest dokonywane rzutowanie, co może być nieco trudne w przypadku korzystania z modelu projektu witryny sieci Web. Moja sugestia powinna mieć pewność, że podczas tworzenia strony wzorcowej należy podać nazwę inną niż `MasterPage.master` lub, a nawet lepiej, utworzyć odwołanie o jednoznacznie określonym typie do strony głównej.

### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Tworzenie odwołania o jednoznacznie określonym typie za pomocą dyrektywy`@MasterType`

Jeśli dokładnie widzisz, że Klasa ASP.NET strony z kodem jest klasą częściową (należy zwrócić uwagę na słowo kluczowe `Partial` w definicji klasy). Klasy częściowe zostały wprowadzone w C# systemach i Visual Basic with.NET Framework 2,0 i w Nutshell umożliwiają definiowanie elementów członkowskich klasy w wielu plikach. Plik klasy związanej z kodem — `AddProduct.aspx.vb`, na przykład — zawiera kod, który został utworzony przez nas, a my. Oprócz naszego kodu aparat ASP.NET automatycznie tworzy oddzielny plik klasy z właściwościami i obsługą zdarzeń w programie, który tłumaczy deklaratywne znaczniki na hierarchię klas strony.

Automatyczne generowanie kodu, które odbywa się za każdym razem, gdy strona ASP.NET jest odwiedzana paves w sposób nieinteresujący i przydatny. W przypadku stron wzorcowych, jeśli poinformujemy aparat ASP.NET o tym, która strona wzorcowa jest używana przez naszą stronę zawartości, generuje dla nas silnie wpisaną Właściwość `Master`.

Użyj [dyrektywy`@MasterType`](https://msdn.microsoft.com/library/ms228274.aspx) , aby poinformować aparat ASP.NET o typie strony głównej strony zawartości. Dyrektywa `@MasterType` może zaakceptować nazwę typu strony wzorcowej lub ścieżki pliku. Aby określić, że strona `AddProduct.aspx` używa `Site.master` jako strony głównej, Dodaj następującą dyrektywę na początku `AddProduct.aspx`:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Ta dyrektywa nakazuje aparatowi ASP.NET Dodawanie odwołania o jednoznacznie określonym typie do strony wzorcowej za pomocą właściwości o nazwie `Master`. W przypadku dyrektywy `@MasterType` na miejscu możemy wywoływać publiczne właściwości i metody strony wzorcowej `Site.master` bezpośrednio przez właściwość `Master` bez rzutowania.

> [!NOTE]
> W przypadku pominięcia dyrektywy `@MasterType` składnia `Page.Master` i `Master` zwrócić te same czynności: obiekt z swobodnym typem na stronę wzorcową strony. Jeśli dołączysz dyrektywę `@MasterType`, `Master` zwraca odwołanie o jednoznacznie określonym typie do określonej strony głównej. `Page.Master`jednak nadal zwraca odwołanie o swobodnym typie. Aby uzyskać bardziej szczegółowe informacje na temat tego, jak jest to przypadek i jak tworzona jest właściwość `Master`, gdy dyrektywa `@MasterType` jest uwzględniona, zobacz artykuł [K. Scott](http://odetocode.com/blogs/scott/default.aspx) [2,0 ASP.NET —`@MasterType` wpis w](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)blogu.

### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualizowanie strony wzorcowej po dodaniu nowego produktu

Teraz, gdy wiemy, jak wywoływać publiczne właściwości i metody strony wzorcowej ze strony zawartości, jesteśmy gotowi do zaktualizowania strony `AddProduct.aspx`, aby Strona główna była odświeżana po dodaniu nowego produktu. Na początku kroku 4 utworzyliśmy procedurę obsługi zdarzeń dla zdarzenia `ItemInserting` formantu DetailsView, które jest wykonywane natychmiast po dodaniu nowego produktu do bazy danych. Dodaj następujący kod do tego programu obsługi zdarzeń:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

Powyższy kod używa zarówno właściwości `Page.Master` o swobodnym typie i właściwości `Master` o jednoznacznie określonym typie. Należy pamiętać, że właściwość `GridMessageText` jest ustawiona na wartość "*ProductName* dodana do siatki..." Wartości dodanego produktu są dostępne za pomocą kolekcji `e.Values`; Jak widać, dodaliśmy wartość `ProductName` można uzyskać za pośrednictwem `e.Values("ProductName")`.

Na rysunku nr 8 przedstawiono stronę `AddProduct.aspx` natychmiast po nowym produkcie — Scott, który został dodany do bazy danych. Należy zauważyć, że nazwa dodanego produktu jest zapisywana na etykiecie strony wzorcowej i że widok GridView został odświeżony w celu uwzględnienia produktu i jego ceny.

[![etykietę strony wzorcowej i widok GridView Pokaż produkt dodany](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Ilustracja 08**: etykieta strony wzorcowej i widok GridView pokazują produkt dodany ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))

## <a name="summary"></a>Podsumowanie

Najlepiej, gdy strona wzorcowa i jej strony zawartości są całkowicie oddzielone od siebie i nie wymagają interakcji. Gdy strony wzorcowe i strony zawartości powinny być zaprojektowane z tym celem, istnieje kilka typowych scenariuszy, w których strona zawartości musi mieć interfejs ze stroną wzorcową. Jednym z najczęstszych centrów przyczyn dotyczących aktualizowania określonej części ekranu głównego na podstawie niektórych akcji, które transpired na stronie zawartości.

Dobra wiadomość polega na tym, że jest stosunkowo prosta, aby strona zawartości była programowo współdziałać ze stroną wzorcową. Zacznij od utworzenia publicznych właściwości lub metod na stronie wzorcowej, które hermetyzują funkcje, które muszą być wywoływane przez stronę zawartości. Następnie na stronie zawartość Uzyskuj dostęp do właściwości i metod strony wzorcowej za pomocą właściwości `Page.Master` o swobodnym typie lub użyj dyrektywy `@MasterType`, aby utworzyć odwołanie z jednoznacznie określonymi typami do strony głównej.

W następnym samouczku sprawdzimy, jak strona wzorcowa może programistycznie korzystać z jednej z jej stron zawartości.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Uzyskiwanie dostępu do danych i aktualizowanie ich w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET strony wzorcowe: porady, wskazówki i pułapki](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` w ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Przekazywanie informacji między zawartością a stronami wzorcowymi](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Praca z danymi w samouczkach ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Zack Nowak. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](control-id-naming-in-content-pages-vb.md)
> [dalej](interacting-with-the-content-page-from-the-master-page-vb.md)

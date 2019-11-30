---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Obsługa wyjątków na poziomie LOGIKI biznesowej i DAL na stronie ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku zobaczysz, jak wyświetlić przyjazny, informacyjny komunikat o błędzie, jeśli wystąpi wyjątek podczas operacji INSERT, Update lub DELETE...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: ee277596ade18d2603892d134b47c2c8697836bb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621046"
---
# <a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Obsługa wyjątków na poziomie warstwy logiki biznesowej i warstwy dostępu do danych na stronie platformy ASP.NET (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) lub [Pobierz plik PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> W tym samouczku zobaczysz, jak wyświetlić przyjazny, informacyjny komunikat o błędzie, jeśli wystąpi wyjątek podczas operacji wstawiania, aktualizowania lub usuwania formantu sieci Web danych ASP.NET.

## <a name="introduction"></a>Wprowadzenie

Praca z danymi z aplikacji sieci Web ASP.NET przy użyciu architektury aplikacji warstwowej obejmuje następujące trzy ogólne czynności:

1. Określ, która metoda warstwy logiki biznesowej ma być wywoływana i jakie wartości parametrów należy przekazać. Wartości parametrów mogą być sztywno kodowane, przypisane programowo lub wprowadzane przez użytkownika.
2. Wywołaj metodę.
3. Przetwórz wyniki. Podczas wywoływania metody LOGIKI biznesowej, która zwraca dane, może to oznaczać, że dane są powiązane z formantem sieci Web danych. W przypadku metod LOGIKI biznesowej, które modyfikują dane, może to obejmować wykonywanie niektórych akcji na podstawie zwracanej wartości lub bezpieczne obsługiwanie dowolnego wyjątku, który powstał w kroku 2.

Jak zostało to opisane w [poprzednim samouczku](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), zarówno element ObjectDataSource, jak i kontrolki sieci Web danych udostępniają punkty rozszerzalności dla kroków 1 i 3. Widok GridView, na przykład, wyzwala zdarzenie `RowUpdating` przed przypisaniem wartości pól do kolekcji `UpdateParameters` elementu ObjectDataSource; `RowUpdated` zdarzenie jest wywoływane po zakończeniu operacji przez element ObjectDataSource.

Zbadamy już zdarzenia wyzwalane w kroku 1 i dowiesz się, jak można je użyć do dostosowania parametrów wejściowych lub anulowania operacji. W tym samouczku powrócimy do zdarzeń wyzwalanych po zakończeniu operacji. Korzystając z tych programów obsługi zdarzeń na poziomie, możemy między innymi ustalić, czy wystąpił wyjątek podczas operacji i obsłużyć go bezpiecznie, wyświetlając przyjazny, informacyjny komunikat o błędzie na ekranie zamiast domyślnego standardu ASP.NET Strona wyjątków.

Aby zilustrować pracę z tymi zdarzeniami pocztowymi, Utwórzmy stronę, która wyświetla listę produktów w edytowalnym widoku GridView. Podczas aktualizowania produktu, jeśli zostanie zgłoszony wyjątek, na stronie ASP.NET zostanie wyświetlony krótki komunikat powyżej widoku GridView wyjaśniającego, że wystąpił problem. Zacznijmy!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Krok 1. Tworzenie edytowalnego widoku GridView produktów

W poprzednim samouczku utworzyliśmy edytowalny widok GridView zawierający tylko dwa pola, `ProductName` i `UnitPrice`. Wymaga to utworzenia dodatkowego przeciążenia dla metody `UpdateProduct` klasy `ProductsBLL`, która akceptuje tylko trzy parametry wejściowe (Nazwa produktu, Cena jednostkowa i identyfikator), jako przeciwieństwo parametru dla każdego pola produktu. Na potrzeby tego samouczka należy ponownie wykonać tę technikę, tworząc edytowalny widok GridView, który wyświetla nazwę produktu, ilość na jednostkę, cenę jednostkową i jednostki w magazynie, ale tylko umożliwia edytowanie nazwy, ceny jednostkowej i jednostek w magazynie.

Aby obsłużyć ten scenariusz, konieczne będzie inne Przeciążenie metody `UpdateProduct`, która akceptuje cztery parametry: nazwę produktu, cenę jednostkową, jednostki w magazynie i identyfikator. Dodaj następującą metodę do klasy `ProductsBLL`:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Po ukończeniu tej metody wszystko jest gotowe do utworzenia strony ASP.NET, która umożliwia edytowanie tych czterech konkretnych pól produktu. Otwórz stronę `ErrorHandling.aspx` w folderze `EditInsertDelete` i Dodaj widok GridView do strony za pomocą narzędzia Projektant. Powiąż widok GridView z nowym elementem ObjectDataSource, mapując metodę `Select()` na metodę `GetProducts()` klasy `ProductsBLL` i metodę `Update()` do właśnie utworzonego przeciążenia `UpdateProduct`.

[![użyć przeciążenia metody UpdateProduct, które akceptuje cztery parametry wejściowe](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Rysunek 1**. Użyj przeciążenia metody `UpdateProduct`, która akceptuje cztery parametry wejściowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))

Spowoduje to utworzenie elementu ObjectDataSource z kolekcją `UpdateParameters` z czterema parametrami i elementem GridView zawierającym pole dla każdego pola produktu. Deklaratywne znaczniki elementu ObjectDataSource przypisuje Właściwość `OldValuesParameterFormatString` wartość `original_{0}`, co spowoduje wyjątek, ponieważ nasze klasy LOGIKI biznesowej nie oczekują parametru wejściowego o nazwie `original_productID` do przekazania. Nie zapomnij usunąć tego ustawienia całkowicie ze składni deklaratywnej (lub ustaw ją na wartość domyślną, `{0}`).

Następnie dostosowanie widok GridView, aby uwzględnić tylko `ProductName`, `QuantityPerUnit`, `UnitPrice`i `UnitsInStock` BoundFields. Możesz również zastosować dowolne formatowanie na poziomie pola, które jest uznawane za niezbędne (na przykład zmiana właściwości `HeaderText`).

W poprzednim samouczku przedstawiono sposób formatowania `UnitPrice` BoundField jako waluty zarówno w trybie tylko do odczytu, jak i w trybie edycji. Zróbmy to teraz. Odwołaj to wymagające ustawienia właściwości `DataFormatString` BoundField na `{0:c}`, jej Właściwość `HtmlEncode` na `false`i `ApplyFormatInEditMode` do `true`, jak pokazano na rysunku 2.

[![skonfigurować BoundField CenaJednostkowa do wyświetlania jako waluta](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Rysunek 2**. Konfigurowanie `UnitPrice` BoundField do wyświetlania jako waluta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))

Formatowanie `UnitPrice` jako waluty w interfejsie edycji wymaga utworzenia programu obsługi zdarzeń dla zdarzenia `RowUpdating` GridView, które analizuje ciąg w formacie waluty w wartości `decimal`. Należy odwołać się do procedury obsługi zdarzeń `RowUpdating` z ostatniego samouczka, aby upewnić się, że użytkownik podał `UnitPrice` wartość. Jednak w tym samouczku zezwolisz użytkownikowi na pominięcie ceny.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Nasz widok GridView zawiera `QuantityPerUnit` BoundField, ale ten BoundField powinien być tylko do wyświetlania i nie powinien być edytowalny przez użytkownika. Aby to rozmieścić, po prostu ustaw właściwość `ReadOnly` BoundFields ' na `true`.

[![QuantityPerUnit BoundField tylko do odczytu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Rysunek 3**. uczyń `QuantityPerUnit` BoundField tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))

Na koniec zaznacz pole wyboru Włącz edytowanie w tagu inteligentnym GridView. Po wykonaniu tych kroków Projektant strony `ErrorHandling.aspx` powinien wyglądać podobnie do rysunku 4.

[![usunąć wszystkie oprócz wymaganych BoundFields i zaznacz pole wyboru Włącz edycję](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Ilustracja 4**. Usuń wszystkie oprócz wymaganych BoundFields i zaznacz pole wyboru Włącz edycję ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))

W tym momencie mamy listę wszystkich pól `ProductName`, `QuantityPerUnit`, `UnitPrice`i `UnitsInStock` produktów. można jednak edytować tylko pola `ProductName`, `UnitPrice`i `UnitsInStock`.

[![użytkownicy mogą teraz łatwo edytować nazwy produktów, ceny i jednostki w polach zasobów](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Rysunek 5**. Użytkownicy mogą teraz łatwo edytować nazwy produktów, ceny i jednostki w polach podstawowych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))

## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Krok 2. bezpieczne obsługiwanie wyjątków na poziomie DAL

Mimo że ten edytowalny widok GridView działa bardzo dobrze, gdy użytkownicy wprowadzają wartości prawne dotyczące nazwy, ceny i jednostek edytowanego produktu, wprowadzenie niedozwolonych wartości spowoduje wyjątek. Na przykład pominięcie wartości `ProductName` powoduje zgłoszenie [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) , ponieważ właściwość `ProductName` w klasie `ProductsRow` ma właściwość `AllowDBNull` ustawioną na `false`; Jeśli baza danych nie działa, podczas próby nawiązania połączenia z bazą danych `SqlException` zostanie wygenerowany przez TableAdapter. Bez podejmowania żadnych działań te wyjątki są bąbelkowe w warstwie dostępu do danych do warstwy logiki biznesowej, a następnie do strony ASP.NET, a wreszcie do środowiska uruchomieniowego ASP.NET.

W zależności od sposobu skonfigurowania aplikacji sieci Web i bez względu na to, czy odwiedzasz aplikację z `localhost`, nieobsługiwany wyjątek może skutkować ogólną stroną serwera — błąd, szczegółowy raport o błędach lub stronę sieci Web przyjazną dla użytkownika. Zobacz [Obsługa błędów aplikacji sieci Web w ASP.NET](http://www.15seconds.com/issue/030102.htm) i [element customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) , aby uzyskać więcej informacji na temat sposobu, w jaki środowisko uruchomieniowe ASP.NET reaguje na nieprzechwycony wyjątek.

Rysunek 6 przedstawia ekran napotkany podczas próby zaktualizowania produktu bez określania wartości `ProductName`. Jest to domyślny szczegółowy raport o błędach wyświetlany podczas przechodzenia przez `localhost`.

[![pominięcie nazwy produktu spowoduje wyświetlenie szczegółów wyjątku](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Ilustracja 6**. pominięcie nazwy produktu spowoduje wyświetlenie szczegółów wyjątku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))

Te szczegóły wyjątku są przydatne podczas testowania aplikacji, ale użytkownik końcowy z takim ekranem w celu zaprezentowania wyjątku jest mniejszy niż idealny. Użytkownik końcowy prawdopodobnie nie wie, co to jest `NoNullAllowedException` lub dlaczego został on spowodowany. Lepszym rozwiązaniem jest zaprezentowanie użytkownikowi informacji o bardziej przyjaznym dla użytkownika komunikacie wyjaśniającym, że wystąpiły problemy podczas próby zaktualizowania produktu.

Jeśli wystąpi wyjątek podczas wykonywania operacji, zdarzenia na poziomie źródłowym zarówno w elemencie ObjectDataSource, jak i w kontrolce sieci Web danych zapewniają metodę wykrywania i anulowania wyjątku z propagacji do środowiska uruchomieniowego ASP.NET. W naszym przykładzie utworzymy procedurę obsługi zdarzeń dla zdarzenia `RowUpdated` GridView, które określa, czy wyjątek został wywołany i, jeśli tak, wyświetla szczegóły wyjątku w kontrolce sieci Web etykieta.

Zacznij od dodania etykiety do strony ASP.NET, ustawiając jej Właściwość `ID` na `ExceptionDetails` i czyszcząc jej Właściwość `Text`. Aby narysować oczy użytkownika do tej wiadomości, ustaw jej Właściwość `CssClass` na `Warning`, która jest klasą CSS dodaną do pliku `Styles.css` w poprzednim samouczku. Odwołaj, że ta Klasa CSS powoduje, że tekst etykiety ma być wyświetlany w kolorze czerwonym, kursywą, pogrubieniu, bardzo dużą czcionką.

[![dodać kontrolkę sieci Web etykieta do strony](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Rysunek 7**. Dodawanie kontrolki sieci Web etykieta do strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))

Ponieważ chcę, aby formant sieci Web etykiety był widoczny tylko natychmiast po wystąpieniu wyjątku, ustaw dla jego właściwości `Visible` wartość false w programie obsługi zdarzeń `Page_Load`:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Przy użyciu tego kodu na pierwszej stronie odwiedzasz i kolejne ogłaszanie zwrotne kontrolki `ExceptionDetails` będą miały Właściwość `Visible` ustawioną na `false`. W przypadku wyjątku na poziomie DAL lub LOGIKI biznesowej, który możemy wykryć w programie obsługi zdarzeń `RowUpdated` GridView, ustawimy Właściwość `Visible` kontrolki `ExceptionDetails` na true. Ponieważ procedury obsługi zdarzeń kontrolki sieci Web występują po obsłudze zdarzeń `Page_Load` w cyklu życia strony, etykieta zostanie wyświetlona. Jednak przy następnym ogłaszaniu zwrotnym `Page_Load` obsługi zdarzeń przywraca Właściwość `Visible` z powrotem do `false`, ukrywając ją z widoku ponownie.

> [!NOTE]
> Alternatywnie możemy usunąć konieczność ustawienia właściwości `Visible` kontrolki `ExceptionDetails` w `Page_Load`, przypisując jej Właściwość `Visible` `false` we składni deklaratywnej i wyłączając jej stan widoku (ustawiając jej Właściwość `EnableViewState` na `false`). Ta alternatywna metoda zostanie użyta w przyszłym samouczku.

Po dodaniu kontrolki etykieta następnym krokiem jest utworzenie programu obsługi zdarzeń dla zdarzenia `RowUpdated` GridView. Wybierz widok GridView w projektancie, przejdź do okno Właściwości i kliknij ikonę błyskawicy, aby wyświetlić listę zdarzeń GridView. Istnieje już wpis dla zdarzenia `RowUpdating` GridView, ponieważ utworzyliśmy procedurę obsługi zdarzeń dla tego zdarzenia wcześniej w tym samouczku. Utwórz również procedurę obsługi zdarzeń dla zdarzenia `RowUpdated`.

![Utwórz procedurę obsługi zdarzeń dla zdarzenia RowUpdated GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Ilustracja 8**. Tworzenie obsługi zdarzeń dla zdarzenia `RowUpdated` GridView

> [!NOTE]
> Obsługę zdarzeń można również utworzyć za pomocą listy rozwijanej w górnej części pliku klasy związanej z kodem. Z listy rozwijanej po lewej stronie `RowUpdated` i po prawej stronie wybierz pozycję GridView.

Utworzenie tego programu obsługi zdarzeń spowoduje dodanie następującego kodu do klasy związanej z kodem ASP.NET strony:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

Drugi parametr wejściowy programu obsługi zdarzeń jest obiektem typu [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), który ma trzy właściwości istotne dla obsługi wyjątków:

- `Exception` odwołanie do zgłoszonego wyjątku; Jeśli żaden wyjątek nie został zgłoszony, ta właściwość będzie mieć wartość `null`
- `ExceptionHandled` wartość logiczna, która wskazuje, czy wyjątek został obsłużony w programie obsługi zdarzeń `RowUpdated`; Jeśli `false` (wartość domyślna), wyjątek jest ponownie zgłaszany, percolating do środowiska uruchomieniowego ASP.NET
- `KeepInEditMode`, jeśli ustawiono, `true` edytowany wiersz GridView pozostaje w trybie edycji; Jeśli `false` (wartość domyślna), wiersz GridView wraca do swojego trybu tylko do odczytu

Nasz kod, a następnie powinien sprawdzić, czy `Exception` nie jest `null`, co oznacza, że wyjątek został zgłoszony podczas wykonywania operacji. W takim przypadku chcemy:

- Wyświetl przyjazny dla użytkownika komunikat w etykiecie `ExceptionDetails`
- Wskaż, że wyjątek został obsłużony
- Zachowaj wiersz GridView w trybie edycji

Ten kod realizuje następujące cele:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Ta procedura obsługi zdarzeń rozpoczyna się od sprawdzenia, czy `e.Exception` jest `null`. Jeśli tak nie jest, właściwość `Visible` `ExceptionDetails` etykiety jest ustawiona na `true` i jej Właściwość `Text` na "Wystąpił problem podczas aktualizowania produktu". Szczegóły faktycznego wyjątku, który został zgłoszony, znajdują się we właściwości `InnerException` obiektu `e.Exception`. Ten wyjątek wewnętrzny jest sprawdzany i, jeśli jest określonego typu, dodatkowy, przydatny komunikat jest dołączany do właściwości `Text` `ExceptionDetails` etykiety. Na koniec właściwości `ExceptionHandled` i `KeepInEditMode` są ustawione na `true`.

Rysunek 9 przedstawia zrzut ekranu na tej stronie podczas pomijania nazwy produktu. Rysunek 10 pokazuje wyniki podczas wprowadzania niedozwolonej wartości `UnitPrice` (-50).

[![ProductName BoundField musi zawierać wartość](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Rysunek 9**: BoundField `ProductName` musi zawierać wartość ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))

[![ujemne wartości CenaJednostkowa są niedozwolone](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Rysunek 10**. ujemne wartości `UnitPrice` są niedozwolone ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))

Ustawiając właściwość `e.ExceptionHandled` na `true`, program obsługi zdarzeń `RowUpdated` wskazuje, że obsłużył wyjątek. W związku z tym wyjątek nie zostanie propagowany do środowiska uruchomieniowego ASP.NET.

> [!NOTE]
> Rysunki 9 i 10 przedstawiają płynny sposób obsługi wyjątków zgłoszonych z powodu nieprawidłowych danych wejściowych użytkownika. W idealnym przypadku takie nieprawidłowe dane wejściowe nigdy nie docierają do warstwy logiki biznesowej w pierwszym miejscu, ponieważ strona ASP.NET powinna upewnić się, że dane wejściowe użytkownika są prawidłowe przed wywołaniem metody `UpdateProduct`j klasy `ProductsBLL`. W naszym następnym samouczku dowiesz się, jak dodać kontrolki weryfikacji do edycji i wstawiania interfejsów, aby upewnić się, że dane przesyłane do warstwy logiki biznesowej są zgodne z regułami biznesowymi. Kontrolki walidacji nie tylko uniemożliwiają wywołanie metody `UpdateProduct`, dopóki dane podane przez użytkownika nie będą prawidłowe, ale również zapewniają bardziej szczegółowe środowisko użytkownika do identyfikowania problemów z wprowadzaniem danych.

## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Krok 3. bezpieczne obsługiwanie wyjątków na poziomie LOGIKI biznesowej

Podczas wstawiania, aktualizowania lub usuwania danych, warstwa dostępu do danych może zgłosić wyjątek w wyniku błędu związanego z danymi. Baza danych może być w trybie offline, wymagana kolumna tabeli bazy danych może nie mieć określonej wartości lub ograniczenie poziomu tabeli mogło zostać naruszone. Oprócz nieścisłych wyjątków związanych z danymi, Warstwa logiki biznesowej może używać wyjątków, aby wskazać, kiedy reguły biznesowe zostały naruszone. W samouczku [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md) na przykład dodaliśmy sprawdzanie reguły biznesowej do oryginalnego przeciążenia `UpdateProduct`. W przypadku, gdy użytkownik oznaczy produkt jako niegotowy, firma Microsoft zobowiązana, że produkt nie należy do jedynej dostarczonej przez niego dostawcy. Jeśli ten warunek został naruszony, zgłoszono `ApplicationException`.

W przypadku przeciążenia `UpdateProduct` utworzonego w ramach tego samouczka dodamy regułę biznesową, która zabrania, aby pole `UnitPrice` było ustawione na nową wartość, która jest większa niż dwukrotnie oryginalna wartość `UnitPrice`. Aby to osiągnąć, Dostosuj Przeciążenie `UpdateProduct` tak, aby wykonywało to sprawdzenie i zgłosi `ApplicationException`, jeśli reguła zostanie naruszona. Zaktualizowana Metoda jest następująca:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Wprowadzenie tej zmiany spowoduje, że wszystkie aktualizacje cen, które są większe niż dwa razy w istniejącej cenie, spowodują zgłoszenie `ApplicationException`. Podobnie jak wyjątek wywoływany z DAL, ten LOGIKI biznesowej podniesione `ApplicationException` można wykryć i obsłużyć w programie obsługi zdarzeń `RowUpdated` GridView. W rzeczywistości kod programu obsługi zdarzeń `RowUpdated`, zgodnie z zapisaniem, prawidłowo wykryje ten wyjątek i wyświetli wartość właściwości `Message` `ApplicationException`. Rysunek 11 przedstawia zrzut ekranu, gdy użytkownik próbuje zaktualizować cenę Chai do $50,00, która jest większa niż podwójna cena w wysokości $19,95.

[![reguły biznesowe nie zezwalają na zwiększenie cen, które przekraczają cenę produktu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Ilustracja 11**. reguły biznesowe nie zezwalają na zwiększenie cen, które przekraczają cenę produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))

> [!NOTE]
> Najlepiej, gdy nasze reguły logiki biznesowej byłyby refaktoryzacji z przeciążeń metody `UpdateProduct` i we wspólnej metodzie. Jest to pozostawione jako ćwiczenie dla czytnika.

## <a name="summary"></a>Podsumowanie

Podczas wstawiania, aktualizowania i usuwania operacji zarówno formant sieci Web danych, jak i element ObjectDataSource są wyzwalane przez zdarzenia poprzedzające i końcowe, które bookendją rzeczywistą operację. Jak już wspomniano w tym samouczku i powyższym, podczas pracy z edytowalnym elementem GridView `RowUpdating` zdarzenie wyzwalane, a następnie zdarzenie `Updating` elementu ObjectDataSource, w którym jest wykonywane polecenie Update w obiekcie źródłowym elementu ObjectDataSource. Po zakończeniu operacji zdarzenie `Updated` elementu ObjectDataSource zostanie wyzwolone, a następnie zdarzenie `RowUpdated` GridView.

Możemy utworzyć programy obsługi zdarzeń dla zdarzeń przedpoziomu, aby dostosować parametry wejściowe lub dla zdarzeń na poziomie, aby móc sprawdzać wyniki operacji i odpowiadać na nie. Programy obsługi zdarzeń na poziomie są najczęściej używane do wykrywania, czy wystąpił wyjątek podczas operacji. W przypadku wyjątku te programy obsługi zdarzeń na poziomie mogą opcjonalnie obsłużyć wyjątek samodzielnie. W tym samouczku pokazano, jak obsłużyć taki wyjątek, wyświetlając przyjazny komunikat o błędzie.

W następnym samouczku dowiesz się, jak zmniejszyć prawdopodobieństwo wystąpienia wyjątków wynikających z problemów z formatowaniem danych (takich jak wprowadzanie negatywnej `UnitPrice`). W celu dowiesz się, jak dodać kontrolki weryfikacji do edycji i wstawiania interfejsów.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Liz Shulok. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [dalej](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)

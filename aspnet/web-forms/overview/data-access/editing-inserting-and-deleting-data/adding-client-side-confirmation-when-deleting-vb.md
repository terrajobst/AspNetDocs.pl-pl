---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Dodawanie potwierdzenia po stronie klienta podczas usuwania (VB) | Microsoft Docs
author: rick-anderson
description: W interfejsach, które zostały już utworzone, użytkownik może przypadkowo usunąć dane, klikając przycisk Usuń po kliknięciu przycisku Edytuj. W tym t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: addb5a1fdc5793309388c5f06b44fb3b145bc102
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78610517"
---
# <a name="adding-client-side-confirmation-when-deleting-vb"></a>Dodawanie potwierdzenia po stronie klienta podczas usuwania (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) lub [Pobierz plik PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> W interfejsach, które zostały już utworzone, użytkownik może przypadkowo usunąć dane, klikając przycisk Usuń po kliknięciu przycisku Edytuj. W tym samouczku dodamy okno dialogowe potwierdzenia po stronie klienta, które pojawia się po kliknięciu przycisku Usuń.

## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich kilku samouczków zaobserwowano, jak korzystać z naszej architektury aplikacji, elementu ObjectDataSource i kontrolek sieci Web danych w ramach uzgodnienia, aby zapewnić możliwość wstawiania, edytowania i usuwania. Usunięte interfejsy, które w tej chwili zostały wykonane, zostały złożone z przycisku usuwania, który po kliknięciu powoduje odświeżenie i wywołanie metody ObjectDataSource s `Delete()`. Metoda `Delete()` następnie wywołuje skonfigurowaną metodę z warstwy logiki biznesowej, która propaguje wywołanie do warstwy dostępu do danych, wydając rzeczywistą instrukcję `DELETE` do bazy danych.

Ten interfejs użytkownika umożliwia osobom odwiedzającym usuwanie rekordów za pomocą kontrolek GridView, DetailsView lub FormView, ale nie ma żadnych potwierdzeń, gdy użytkownik kliknie przycisk Usuń. Jeśli użytkownik przypadkowo kliknie przycisk Usuń, gdy klikniesz przycisk Edytuj, rekord, który ma zostać zaktualizowany, zostanie usunięty. Aby temu zapobiec, w tym samouczku dodamy okno dialogowe potwierdzenia po stronie klienta, które pojawia się po kliknięciu przycisku Usuń.

Funkcja JavaScript `confirm(string)` wyświetla swój parametr wejściowy ciągu jako tekst wewnątrz modalnego okna dialogowego, które zawiera dwa przyciski — OK i Anuluj (patrz rysunek 1). Funkcja `confirm(string)` zwraca wartość logiczną w zależności od tego, jaki przycisk jest kliknięty (`true`, jeśli użytkownik kliknie OK, i `false` Jeśli kliknie przycisk Anuluj).

![Metoda Confirm (String) języka JavaScript wyświetla modalne, po stronie klienta](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Rysunek 1**. Metoda `confirm(string)` JavaScript wyświetla modalne, po stronie klienta

W trakcie przekazywania formularza, jeśli wartość `false` jest zwracana z programu obsługi zdarzeń po stronie klienta, przesyłanie formularza zostanie anulowane. Korzystając z tej funkcji, można użyć przycisku Usuń s po stronie klienta `onclick` obsługi zdarzeń zwraca wartość wywołania do `confirm("Are you sure you want to delete this product?")`. Jeśli użytkownik kliknie przycisk Anuluj, `confirm(string)` zwróci wartość false, co spowoduje anulowanie przesłania formularza. Bez ogłaszania zwrotnego produkt, którego przycisk usuwania został kliknięty, nie zostanie usunięty. Jeśli jednak użytkownik kliknie przycisk OK w oknie dialogowym potwierdzenia, ogłaszanie zwrotne będzie kontynuowane unabated i produkt zostanie usunięty. Aby uzyskać więcej informacji na temat tej techniki, należy zapoznać się z tematem [Używanie metody `confirm()` JavaScript s do sterowania przesyłaniem formularzy](http://www.webreference.com/programming/javascript/confirm/) .

Dodawanie niezbędnego skryptu po stronie klienta różni się nieco w przypadku używania szablonów niż w przypadku używania CommandField. Dlatego w tym samouczku zobaczymy zarówno widok FormView, jak i widok GridView.

> [!NOTE]
> Przy użyciu technik potwierdzeń po stronie klienta, takich jak te omówione w tym samouczku, zakłada się, że użytkownicy odwiedzają przeglądarki obsługujące język JavaScript i obsługujące JavaScript. Jeśli jedno z tych założeń nie jest prawdziwe dla określonego użytkownika, kliknięcie przycisku Usuń spowoduje natychmiastowe zakończenie ogłaszania zwrotnego (bez wyświetlania potwierdzenia MessageBox).

## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Krok 1. Tworzenie widoku FormView, który obsługuje usuwanie

Zacznij od dodania widoku FormView do strony `ConfirmationOnDelete.aspx` w folderze `EditInsertDelete`, aby powiązać ją z nowym elementem ObjectDataSource, który pobiera informacje o produkcie za pomocą metody `GetProducts()` `ProductsBLL` Class s. Należy również skonfigurować element ObjectDataSource tak, aby Metoda `DeleteProduct(productID)` `ProductsBLL` klasy s została zamapowana na metodę `Delete()` ObjectDataSource s; Upewnij się, że listy rozwijane kart Wstawianie i aktualizacja są ustawione na wartość (brak). Na koniec zaznacz pole wyboru Włącz stronicowanie w tagu inteligentnym FormView.

Po wykonaniu tych kroków nowe oznakowanie deklaracyjne języka ObjectDataSource będzie wyglądać następująco:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Jak w poprzednich przykładach, które nie korzystały z optymistycznej współbieżności, poświęć chwilę na wyczyszczenie właściwości `OldValuesParameterFormatString` ObjectDataSource s.

Ponieważ został on powiązany z kontrolką ObjectDataSource, która obsługuje tylko usuwanie, tylko `ItemTemplate` FormView oferuje tylko przycisk Usuń, bez przycisków New i Update. Znaczniki deklaratywne FormView, jednak zawierają zbędne `EditItemTemplate` i `InsertItemTemplate`, które mogą zostać usunięte. Poświęć chwilę na dostosowanie `ItemTemplate` tak, aby pokazywała tylko podzestaw pól danych produktu. Chcę, aby nazwa produktu była wyświetlana w `<h3>` nagłówku powyżej jego dostawcy i nazwy kategorii (wraz z przyciskiem Usuń).

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Dzięki tym zmianom mamy w pełni funkcjonalną stronę sieci Web, która umożliwia użytkownikowi jednokrotne przełączanie się między produktami, z możliwością usunięcia produktu przez kliknięcie przycisku Usuń. Rysunek 2 przedstawia zrzut ekranu dotyczący postępu z tego względu, gdy jest wyświetlany za pomocą przeglądarki.

[![FormView pokazuje informacje o pojedynczym produkcie](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Rysunek 2**: FormView pokazuje informacje o pojedynczym produkcie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))

## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Krok 2: wywoływanie funkcji Confirm (String) z poziomu zdarzenia onkliknięcia po stronie klienta

Po utworzeniu FormView, ostatnim krokiem jest skonfigurowanie przycisku Usuń, tak aby po kliknięciu jego przez osobę odwiedzającą została wywołana funkcja JavaScript `confirm(string)`. Dodawanie skryptu po stronie klienta do przycisku, element LinkButton lub kliknięto element ImageButton s zdarzenia `onclick` po stronie klienta można wykonać przy użyciu `OnClientClick property`, który jest nowy dla ASP.NET 2,0. Ponieważ chcemy mieć wartość zwracaną przez funkcję `confirm(string)`, po prostu ustaw tę właściwość na: `return confirm('Are you certain that you want to delete this product?');`

Po wprowadzeniu tej zmiany Składnia deklaratywna usuwania element LinkButton powinna wyglądać następująco:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Wszystko, co wszystko jest gotowe! Rysunek 3 pokazuje zrzut ekranu przedstawiający to potwierdzenie w akcji. Kliknięcie przycisku Usuń powoduje wyświetlenie okna dialogowego potwierdzenia. Jeśli użytkownik kliknie przycisk Anuluj, ogłaszanie zwrotne zostanie anulowane, a produkt nie zostanie usunięty. Jeśli jednak użytkownik kliknie przycisk OK, ogłaszanie zwrotne będzie kontynuowane, a metoda ObjectDataSource s `Delete()` zostanie wywołana, culminating w usuwanym rekordzie bazy danych.

> [!NOTE]
> Ciąg przesłany do funkcji JavaScript `confirm(string)` jest rozdzielany znakami apostrofów (a nie cudzysłowów). W języku JavaScript ciągi mogą być rozdzielane przy użyciu dowolnego znaku. W tym miejscu są używane apostrofy, dzięki czemu ograniczniki ciągu przechodzą do `confirm(string)` nie wprowadzają niejednoznaczności ograniczników używanych dla wartości właściwości `OnClientClick`.

[![potwierdzenie jest teraz wyświetlane po kliknięciu przycisku Usuń](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Rysunek 3**. potwierdzenie jest teraz wyświetlane po kliknięciu przycisku Usuń (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))

## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Krok 3. Konfigurowanie właściwości OnClientClick przycisku Usuń w CommandField

Podczas pracy z przyciskiem, element LinkButton lub kliknięto element ImageButton bezpośrednio w szablonie, można z nim skojarzyć okno dialogowe potwierdzenia przez skonfigurowanie jego właściwości `OnClientClick`, aby zwracała wyniki funkcji `confirm(string)` JavaScript. Jednak CommandField, który dodaje pole usuwania przycisków do widoku GridView lub DetailsView-nie ma właściwości `OnClientClick`, która może być ustawiona deklaratywnie. Zamiast tego należy programistycznie odwoływać się do przycisku Usuń w obszarze GridView lub DetailsView s odpowiednie `DataBound` obsługi zdarzeń, a następnie ustawić jego właściwość `OnClientClick`.

> [!NOTE]
> Podczas ustawiania właściwości `OnClientClick` przycisku Usuń w odpowiedniej procedurze obsługi zdarzeń `DataBound`, mamy dostęp do danych powiązanych z bieżącym rekordem. Oznacza to, że możemy rozłożyć komunikat potwierdzający, aby uwzględnić szczegóły dotyczące konkretnego rekordu, takiego jak "czy na pewno chcesz usunąć produkt Chai?". Takie dostosowanie jest również możliwe w szablonach przy użyciu składni wiązania danych.

Aby dopuścić do ustawienia właściwości `OnClientClick` przycisków usuwania w CommandField, Dodaj widok GridView do strony. Skonfiguruj ten widok GridView, aby używał tego samego formantu ObjectDataSource, który jest używany przez FormView. Ogranicz również widok GridView BoundFields, aby zawierał tylko nazwę produktu, kategorię i dostawcę. Na koniec zaznacz pole wyboru Włącz usuwanie w tagu inteligentnym GridView. Spowoduje to dodanie CommandField do kolekcji GridView s `Columns` z właściwością `ShowDeleteButton` ustawioną na `true`.

Po wprowadzeniu tych zmian znaczniki deklaratywne GridView powinny wyglądać następująco:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

CommandField zawiera pojedyncze wystąpienie klasy Delete element LinkButton, do którego można uzyskać dostęp programowo przy użyciu programu obsługi zdarzeń `RowDataBound` GridView. Po odwołaniu można ustawić odpowiednio jej Właściwość `OnClientClick`. Utwórz procedurę obsługi zdarzeń dla zdarzenia `RowDataBound` przy użyciu następującego kodu:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Ta procedura obsługi zdarzeń działa z wierszami danych (tymi, które będą miały przycisk Usuń) i rozpoczyna się programowo, odwołując się do przycisku Usuń. Ogólnie rzecz biorąc, użyj następującego wzorca:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* jest typem przycisku używanym przez CommandField-Button, element LinkButton lub kliknięto element ImageButton. Domyślnie CommandField używa LinkButtons, ale można go dostosować za pośrednictwem CommandField s `ButtonType property`. *CommandFieldIndex* jest indeksem porządkowym CommandField w kolekcji `Columns` GridView, podczas gdy *controlIndex* jest indeksem przycisku usuwania w kolekcji CommandField s `Controls`. Wartość *controlIndex* zależy od pozycji przycisku w odniesieniu do innych przycisków w CommandField. Na przykład, jeśli jedynym przyciskiem wyświetlanym w CommandField jest przycisk Usuń, Użyj indeksu 0. Jeśli jednak przycisk Edytuj, który poprzedza przycisk Usuń, Użyj indeksu 2. Przyczyną jest użycie indeksu 2 jest spowodowane tym, że dwie kontrolki są dodawane przez CommandField przed przyciskiem Usuń: przycisk Edytuj i LiteralControl, które służy do dodawania miejsca między przyciskami Edytuj i Usuń.

W naszym konkretnym przykładzie CommandField korzysta z LinkButtons i, że pole z lewej strony ma wartość *commandFieldIndex* równą 0. Ponieważ nie ma żadnych innych przycisków, ale przycisk Usuń w CommandField, używamy *controlIndex* 0.

Po odwołaniu się do przycisku Usuń w CommandField następnym zapoznaj się z informacjami o produkcie powiązanym z bieżącym wierszem GridView. Na koniec ustawimy Właściwość `OnClientClick` przycisku Usuń na odpowiednią JavaScript, która zawiera nazwę produktu. Ponieważ ciąg JavaScript przesłany do funkcji `confirm(string)` jest rozdzielany przy użyciu apostrofów, musimy wprowadzić wszelkie apostrofy, które pojawiają się w ramach nazwy produktu. W szczególności wszystkie apostrofy w nazwie produktu są oznaczone jako "`\'`".

Po zakończeniu tych zmian kliknięcie przycisku Usuń w widoku GridView spowoduje wyświetlenie niestandardowego okna dialogowego potwierdzenia (patrz rysunek 4). Podobnie jak w przypadku potwierdzenia MessageBox z widoku FormView, jeśli użytkownik kliknie przycisk Anuluj ogłaszanie zwrotne zostanie anulowane, co uniemożliwi usunięcie.

> [!NOTE]
> Tej techniki można również użyć do programistycznego dostępu do przycisku Usuń w CommandField w widoku DetailsView. Dla widoku DetailsView można jednak utworzyć procedurę obsługi zdarzeń dla zdarzenia `DataBound`, ponieważ w widoku DetailsView nie ma zdarzenia `RowDataBound`.

[![kliknięcie przycisku Usuń w widoku GridView powoduje wyświetlenie niestandardowego okna dialogowego potwierdzenia](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Ilustracja 4**. kliknięcie przycisku Usuń w widoku GridView powoduje wyświetlenie niestandardowego okna dialogowego potwierdzenia ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))

## <a name="using-templatefields"></a>Korzystanie z używanie TemplateField

Jedną z wad CommandField jest to, że jego przyciski muszą być dostępne za pomocą indeksowania i że obiekt wyniku musi być rzutowany do odpowiedniego typu przycisku (Button, element LinkButton lub kliknięto element ImageButton). Korzystanie z typów "Magic Numbers" i "zakodowanych" zaprasza problemy, których nie można odnaleźć do czasu wykonania. Na przykład, jeśli ty lub inny programista, dodaje nowe przyciski do CommandField w pewnym momencie w przyszłości (na przykład przycisk Edytuj) lub zmienia właściwość `ButtonType`, istniejący kod nadal będzie kompilować bez błędu, ale odwiedzanie strony może spowodować wyjątek lub nieoczekiwane zachowanie, w zależności od tego, jak zapisano kod i jakie zmiany zostały wprowadzone.

Alternatywnym podejściem jest konwertowanie widoku GridView i widoku DetailsView s CommandFields na używanie TemplateField. Spowoduje to wygenerowanie TemplateField z `ItemTemplate`, który ma element LinkButton (lub kliknięto element ImageButton) dla każdego przycisku w CommandField. Te przyciski `OnClientClick` właściwości mogą być przypisywane deklaratywnie, jak w widoku FormView lub można programistycznie uzyskać `DataBound` do nich dostęp przy użyciu następującego wzorca:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Gdzie *controlID* jest wartością właściwości `ID` przycisku. Chociaż ten wzorzec nadal wymaga ustalonego typu dla rzutowania, eliminuje konieczność indeksowania, co pozwala na zmianę układu bez powodu błędu czasu wykonywania.

## <a name="summary"></a>Podsumowanie

Funkcja JavaScript `confirm(string)` jest powszechnie stosowaną techniką służącą do sterowania przepływem pracy tworzenia formularzy. Po wykonaniu funkcja wyświetla modalne okno dialogowe po stronie klienta, które zawiera dwa przyciski, OK i Anuluj. Jeśli użytkownik kliknie przycisk OK, funkcja `confirm(string)` zwróci `true`; Kliknięcie przycisku Anuluj zwraca `false`. Ta funkcja, w połączeniu z zachowaniem przeglądarki, umożliwia anulowanie przesłania formularza, jeśli program obsługi zdarzeń w procesie przesłaniania zwróci `false`, może być używany do wyświetlania elementu MessageBox potwierdzenia podczas usuwania rekordu.

Funkcja `confirm(string)` może być skojarzona z obsługą zdarzeń `onclick` po stronie klienta i kontrolki sieci Web na przycisku, korzystając z właściwości Control s `OnClientClick`. Podczas pracy z przyciskiem Usuń w szablonie — w jednym z szablonów FormView lub w TemplateField w widoku DetailsView lub GridView — tę właściwość można ustawić jako deklaratywną lub programowo, jak pokazano w tym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](implementing-optimistic-concurrency-vb.md)
> [dalej](limiting-data-modification-functionality-based-on-the-user-vb.md)

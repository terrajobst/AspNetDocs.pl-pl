---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Posługiwanie się stroną zawartości z poziomu strony wzorcowej (C#) | Microsoft Docs
author: rick-anderson
description: Bada sposób wywoływania metod, ustawiania właściwości itp. strony zawartości z kodu na stronie wzorcowej.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 2cf57665aa584285351d874267949d61db69c7fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78618196"
---
# <a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interakcja ze stroną zawartości z poziomu strony wzorcowej (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Bada sposób wywoływania metod, ustawiania właściwości itp. strony zawartości z kodu na stronie wzorcowej.

## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku sprawdzono sposób, w jaki strona zawartości może programowo współdziałać ze stroną wzorcową. Odwołaj, że zaktualizowaliśmy stronę wzorcową w celu uwzględnienia kontrolki GridView, która wystawiła pięć ostatnio dodanych produktów. Następnie utworzyliśmy stronę zawartości, za pomocą której użytkownik może dodać nowy produkt. Po dodaniu nowego produktu strona zawartości musi nakazać stronie wzorcowej odświeżenie widoku GridView w taki sposób, aby zawierała tylko produkt dodany. Ta funkcja została osiągnięta przez dodanie metody publicznej do strony wzorcowej, która odświeży dane powiązane z elementem GridView, a następnie wywołuje tę metodę ze strony zawartości.

Najbardziej typowa interakcja z zawartością i stroną wzorcową pochodzi ze strony zawartość. Jednak jest możliwe, aby strona wzorcowa Rouse bieżącą stronę zawartości do działania. może to być konieczne, jeśli strona wzorcowa zawiera elementy interfejsu użytkownika, które umożliwiają użytkownikom modyfikowanie danych, które są również wyświetlane na stronie zawartość. Rozważ użycie strony zawartości, która wyświetla informacje o produktach w kontrolce GridView i na stronie wzorcowej zawierającej kontrolkę Button, która po kliknięciu powoduje podwojenie cen wszystkich produktów. Podobnie jak w przypadku przykładu w poprzednim samouczku, widok GridView musi zostać odświeżony po kliknięciu przycisku podwójnej ceny, tak aby wyświetlał nowe ceny, ale w tym scenariuszu jest to strona wzorcowa, która musi Rouse stronę zawartości do działania.

W tym samouczku przedstawiono sposób, w jaki Strona główna wywołała funkcje zdefiniowane na stronie zawartości.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Obsługa interakcji programistycznej za pośrednictwem obsługi zdarzeń i zdarzeń

Wywoływanie funkcji strony zawartości z poziomu strony wzorcowej jest trudniejsze niż inne. Ze względu na to, że strona zawartości ma pojedynczą stronę wzorcową, podczas zawieszania interakcji programowanej ze strony zawartości wiemy, jakie metody i właściwości publiczne są dostępne w naszym usunięciu. Na stronie wzorcowej można jednak wybrać wiele różnych stron zawartości z własnymi zestawami właściwości i metod. W jaki sposób można napisać kod na stronie wzorcowej, aby wykonać jakąś akcję na stronie zawartości, gdy nie wiesz, jaka strona zawartości zostanie wywołana do czasu wykonania?

Rozważmy kontrolkę sieci Web ASP.NET, taką jak kontrolka Button. Kontrolka przycisku może być wyświetlana na dowolnej liczbie stron ASP.NET i wymaga mechanizmu, za pomocą którego można ostrzec o klikniętej stronie. Jest to realizowane przy użyciu *zdarzeń*. W szczególności formant Button wywołuje jego zdarzenie `Click`, gdy zostanie kliknięty; na stronie ASP.NET zawierającej przycisk można opcjonalnie odpowiedzieć na to powiadomienie za pośrednictwem *programu obsługi zdarzeń*.

Ten sam wzorzec może służyć do wyzwolenia funkcji wyzwalacza strony głównej na jej stronach zawartości:

1. Dodaj zdarzenie do strony głównej.
2. Zgłoś zdarzenie, gdy strona wzorcowa musi komunikować się ze swoją stroną zawartości. Jeśli na przykład Strona główna musi wysyłać alerty do strony zawartości, na której użytkownik ma podwójne ceny, jego zdarzenie zostanie zgłoszone natychmiast po podwójnym cenach.
3. Utwórz procedurę obsługi zdarzeń na tych stronach zawartości, które muszą wykonać pewne czynności.

W pozostałej części tego samouczka zaimplementowano przykład opisany we wprowadzeniu; oznacza to, że strona zawartości, która wyświetla listę produktów w bazie danych i stronę wzorcową, która zawiera przycisk kontrolki do dwukrotnego uzyskania cen.

## <a name="step-1-displaying-products-in-a-content-page"></a>Krok 1. Wyświetlanie produktów na stronie zawartości

Pierwszą kolejnością biznesową jest utworzenie strony zawartości, która zawiera produkty z bazy danych Northwind. (Baza danych Northwind została dodana do projektu w poprzednim samouczku, [*korzystając z strony wzorcowej ze strony zawartość*](interacting-with-the-master-page-from-the-content-page-cs.md)). Zacznij od dodania nowej strony ASP.NET do folderu `~/Admin` o nazwie `Products.aspx`, aby upewnić się, że należy powiązać ją ze stroną wzorcową `Site.master`. Rysunek 1 przedstawia Eksplorator rozwiązań po dodaniu tej strony do witryny sieci Web.

[![dodać nową stronę ASP.NET do folderu Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Ilustracja 01**. Dodawanie nowej strony ASP.NET do folderu `Admin` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))

Odwołaj się do tego w [*temacie Określanie tytułu, meta tagi i innych nagłówków HTML na stronie wzorcowej*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) — samouczek został utworzony za pomocą niestandardowej klasy strony podstawowej o nazwie `BasePage`, która generuje tytuł strony, jeśli nie jest jawnie ustawiona. Przejdź do klasy powiązanej z kodem `Products.aspx` strony i utwórz ją od `BasePage` (zamiast z `System.Web.UI.Page`).

Na koniec Zaktualizuj plik `Web.sitemap`, aby zawierał wpis do tej lekcji. Dodaj następującą adiustację poniżej `<siteMapNode>`, aby uzyskać informacje na temat interakcji ze stroną wzorcową:

[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

Dodanie tego elementu `<siteMapNode>` jest odzwierciedlone na liście lekcji (patrz rysunek 5).

Wróć do `Products.aspx`. W kontrolce zawartości dla `MainContent`Dodaj kontrolkę GridView i nadaj jej nazwę `ProductsGrid`. Powiąż element GridView z nową kontrolką kontrolki SqlDataSource o nazwie `ProductsDataSource`.

[![powiązać widoku GridView z nową kontrolką kontrolki SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Ilustracja 02**. powiązanie widoku GridView z nową kontrolką kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))

Skonfiguruj kreatora tak, aby używał bazy danych Northwind. Jeśli pracujesz w poprzednim samouczku, musisz mieć już parametry połączenia o nazwie `NorthwindConnectionString` w `Web.config`. Wybierz te parametry połączenia z listy rozwijanej, jak pokazano na rysunku 3.

[![skonfigurować kontrolki SqlDataSource do korzystania z bazy danych Northwind](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Rysunek 03**: Konfigurowanie kontrolki SqlDataSource do korzystania z bazy danych Northwind ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))

Następnie określ instrukcję `SELECT` formantu źródła danych, wybierając tabelę produkty z listy rozwijanej i zwracając kolumny `ProductName` i `UnitPrice` (zobacz rysunek 4). Kliknij przycisk Dalej, a następnie Zakończ, aby zakończyć pracę Kreatora konfiguracji źródła danych.

[![zwrócić pola NazwaProduktu i CenaJednostkowa z tabeli Products](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Rysunek 04**: zwróć pola `ProductName` i `UnitPrice` z tabeli `Products` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))

To wszystko! Po zakończeniu działania kreatora program Visual Studio dodaje dwa BoundFields do widoku GridView, aby zdublować dwa pola zwracane przez formant kontrolki SqlDataSource. Poniższe znaczniki formantów GridView i kontrolki SqlDataSource. Rysunek 5 przedstawia wyniki wyświetlane w przeglądarce.

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]

[![każdego produktu, a jego cena jest wymieniona w widoku GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Ilustracja 05**: każdy produkt i jego cena są wymienione w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))

> [!NOTE]
> Możesz wyczyścić wygląd GridView. Niektóre sugestie obejmują formatowanie wyświetlanej wartości CenaJednostkowa jako waluty i użycie kolorów i czcionek tła w celu poprawienia wyglądu siatki. Aby uzyskać więcej informacji o wyświetlaniu i formatowaniu danych w programie ASP.NET, zapoznaj się z tematem [Praca z serią samouczków dotyczących danych](../../data-access/index.md).

## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Krok 2. Dodawanie przycisku podwójnej ceny do strony głównej

Następnym zadaniem jest dodanie kontrolki sieci Web przycisku do strony wzorcowej, która po kliknięciu spowoduje podwójną cenę wszystkich produktów w bazie danych. Otwórz stronę wzorcową `Site.master` i przeciągnij przycisk z przybornika do projektanta, umieszczając go pod `RecentProductsDataSource` kontrolką kontrolki sqldatasourceą dodaną w poprzednim samouczku. Ustaw właściwość `ID` przycisku na `DoublePrice` i jej Właściwość `Text` na wartość "podwójne ceny produktu".

Następnie Dodaj kontrolkę kontrolki SqlDataSource do strony wzorcowej, nadając jej nazwę `DoublePricesDataSource`. Ta kontrolki SqlDataSource zostanie użyta do wykonania instrukcji `UPDATE` w celu podwójnej realizacji wszystkich cen. W związku z tym musimy ustawić właściwości `ConnectionString` i `UpdateCommand` do odpowiednich parametrów połączenia i instrukcji `UPDATE`. Następnie musimy wywołać metodę `Update` formantu kontrolki SqlDataSource po kliknięciu przycisku `DoublePrice`. Aby ustawić właściwości `ConnectionString` i `UpdateCommand`, wybierz kontrolkę kontrolki SqlDataSource, a następnie przejdź do okno Właściwości. Właściwość `ConnectionString` wyświetla listę tych parametrów połączenia, które są już przechowywane w `Web.config` na liście rozwijanej. Wybierz opcję `NorthwindConnectionString`, jak pokazano na rysunku 6.

[![skonfigurować kontrolki SqlDataSource do korzystania z NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Ilustracja 06**. Konfigurowanie kontrolki SqlDataSource do korzystania z `NorthwindConnectionString` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))

Aby ustawić właściwość `UpdateCommand`, Znajdź w okno Właściwości opcję UpdateQuery. Ta właściwość, po wybraniu, wyświetla przycisk z wielokropkiem; Kliknij ten przycisk, aby wyświetlić okno dialogowe Edytor parametrów i polecenie pokazane na rysunku 7. Wpisz następującą instrukcję `UPDATE` w polu tekstowym okna dialogowego:

[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Ta instrukcja, gdy jest wykonywana, spowoduje podwójne `UnitPrice` wartość dla każdego rekordu w tabeli `Products`.

[![ustaw właściwość UpdateCommand elementu kontrolki SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Ilustracja 07**: ustaw właściwość `UpdateCommand` kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))

Po ustawieniu tych właściwości, przycisk i znaczniki deklaratywne formantów kontrolki SqlDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Wszystko to, które pozostanie, wywołuje metodę `Update` po kliknięciu przycisku `DoublePrice`. Utwórz procedurę obsługi zdarzeń `Click` dla przycisku `DoublePrice` i Dodaj następujący kod:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Aby przetestować tę funkcję, odwiedź stronę `~/Admin/Products.aspx` utworzoną w kroku 1 i kliknij przycisk "podwójne ceny produktu". Kliknięcie przycisku powoduje odświeżenie i wykonanie `DoublePrice` przycisku `Click` obsługi zdarzeń, Podwajanie cen wszystkich produktów. Strona jest następnie ponownie renderowana, a znacznik jest zwracany i ponownie wyświetlany w przeglądarce. W widoku GridView na stronie zawartość są wyświetlane te same ceny jak przed kliknięciem przycisku "ceny podwójnego produktu". Wynika to z faktu, że dane początkowo załadowane w widoku GridView były przechowywane w stanie widoku, więc nie są ponownie ładowane w przypadku ogłaszania zwrotnego, chyba że zostanie to zrobione inaczej. Jeśli odwiedzasz na innej stronie, a następnie wrócisz do strony `~/Admin/Products.aspx`, zobaczysz zaktualizowane ceny.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Krok 3. Podnoszenie poziomu zdarzenia, gdy ceny są podwójne

Ponieważ GridView na stronie `~/Admin/Products.aspx` nie odzwierciedla bezpośrednio podwajania cen, użytkownik może zrozumieć, że nie klika przycisku "podwójne ceny produktu" lub że nie działa. Mogą próbować jeszcze raz kliknąć przycisk jeszcze raz, Podwajanie cen i ponownie. Aby rozwiązać ten problem, musimy mieć siatkę na stronie zawartości, która będzie zawierała nowe ceny natychmiast po ich podwójnym wyjściu.

Jak opisano wcześniej w tym samouczku, musimy zgłosić zdarzenie na stronie wzorcowej za każdym razem, gdy użytkownik kliknie przycisk `DoublePrice`. Zdarzenie to metoda dla jednej klasy (wydawcy zdarzeń), która powiadamia inny zestaw innych klas (subskrybentów zdarzeń) o wystąpieniu interesujących. W tym przykładzie Strona główna jest wydawcą zdarzeń. te strony zawartości, które należy zadbać o to, gdy kliknięto przycisk `DoublePrice`, to Subskrybenci.

Klasa subskrybuje zdarzenie przez utworzenie *programu obsługi zdarzeń*, który jest metodą wykonywaną w odpowiedzi na zgłoszone zdarzenie. Wydawca definiuje zdarzenia, które wywołuje przez zdefiniowanie *delegata zdarzenia*. Delegat zdarzeń określa, jakie parametry wejściowe muszą być akceptowane przez program obsługi zdarzeń. W .NET Framework, Delegaty zdarzeń nie zwracają żadnej wartości i akceptują dwa parametry wejściowe:

- `Object`, który identyfikuje źródło zdarzenia i
- Klasa pochodna `System.EventArgs`

Drugi parametr przesłany do procedury obsługi zdarzeń może zawierać dodatkowe informacje o zdarzeniu. Chociaż Klasa bazowa `EventArgs` nie przekazuje żadnych informacji, .NET Framework zawiera szereg klas, które zwiększają `EventArgs` i obejmują dodatkowe właściwości. Na przykład wystąpienie `CommandEventArgs` jest przesyłane do programów obsługi zdarzeń, które odpowiadają na zdarzenie `Command` i zawiera dwie właściwości informacyjne: `CommandArgument` i `CommandName`.

> [!NOTE]
> Aby uzyskać więcej informacji na temat tworzenia, wywoływania i obsługi zdarzeń, zobacz [zdarzenia i delegatów](https://msdn.microsoft.com/library/17sde2xt.aspx) oraz [delegatów zdarzeń w prostym języku angielskim](http://www.codeproject.com/KB/cs/eventdelegates.aspx).

Aby zdefiniować zdarzenie, użyj następującej składni:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Ze względu na to, że po kliknięciu przycisku `DoublePrice` ma być wysyłany alert tylko na stronie zawartości, można użyć `EventHandler`delegata zdarzenia, który definiuje procedurę obsługi zdarzeń, która akceptuje jako drugi parametr obiekt typu `System.EventArgs`. Aby utworzyć zdarzenie na stronie wzorcowej, Dodaj następujący wiersz kodu do klasy powiązanej z kodem strony wzorcowej:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Powyższy kod dodaje zdarzenie publiczne do strony wzorcowej o nazwie `PricesDoubled`. Teraz będziemy musieli zgłosić to zdarzenie po rozpoczęciu cen. Aby zgłosić zdarzenie, użyj następującej składni:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Gdzie *Sender* i *EventArgs* są wartościami, które chcesz przekazać do programu obsługi zdarzeń subskrybenta.

Zaktualizuj procedurę obsługi zdarzeń `Click` `DoublePrice` przy użyciu następującego kodu:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Tak jak wcześniej program obsługi zdarzeń `Click` jest uruchamiany przez wywołanie metody `Update` `DoublePricesDataSource` formantu kontrolki SqlDataSource, aby dwukrotnie ceny wszystkich produktów. Po doistnieniu dwóch dodatków do programu obsługi zdarzeń. Najpierw odświeżane są dane `RecentProducts` GridView. Ten element GridView został dodany do strony wzorcowej w poprzednim samouczku i zawiera pięć ostatnio dodanych produktów. Musimy odświeżyć tę siatkę, aby w przypadku tych pięciu produktów była widoczna Podwójna cena. Poniżej zostanie zgłoszone zdarzenie `PricesDoubled`. Odwołanie do samej strony wzorcowej (`this`) jest wysyłane do programu obsługi zdarzeń jako źródło zdarzenia i pusty obiekt `EventArgs` jest wysyłany jako argumenty zdarzenia.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Krok 4. obsługa zdarzenia na stronie zawartości

Na tym etapie strona wzorcowa generuje swoje zdarzenie `PricesDoubled` przy każdym kliknięciu kontrolki przycisku `DoublePrice`. Jest to jednak tylko połowa sprawdzonej — nadal musimy obsłużyć zdarzenie na subskrybencie. Obejmuje to dwa kroki: Tworzenie programu obsługi zdarzeń i Dodawanie kodu okablowania zdarzeń, aby po podniesieniu zdarzenia zostanie wykonany program obsługi zdarzeń.

Zacznij od utworzenia procedury obsługi zdarzeń o nazwie `Master_PricesDoubled`. Ze względu na to, jak definiujemy zdarzenie `PricesDoubled` na stronie wzorcowej dwa parametry wejściowe programu obsługi zdarzeń muszą być odpowiednio typu `Object` i `EventArgs`. W programie obsługi zdarzeń Wywołaj metodę `DataBind` `ProductsGrid` GridView, aby ponownie powiązać dane z siatką.

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Kod dla programu obsługi zdarzeń został ukończony, ale jeszcze nie można obsłużyć zdarzenia `PricesDoubled` strony głównej do tej procedury obsługi zdarzeń. Abonent może nawiązać zdarzenie z obsługą zdarzeń za pomocą następującej składni:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*Wydawca* jest odwołaniem do obiektu, który oferuje *zdarzenie EventName, a* *MethodName* to nazwa programu obsługi zdarzeń zdefiniowanego na subskrybencie, który ma sygnaturę odpowiadającą *eventDelegate*. Innymi słowy, jeśli delegowanie zdarzenia jest `EventHandler`, *MethodName* musi być nazwą metody na subskrybencie, która nie zwraca wartości i akceptuje dwa parametry wejściowe typów `Object` i `EventArgs`.

Ten kod okablowania zdarzeń musi być wykonywany na pierwszej stronie odwiedzania i kolejnych ogłaszania zwrotnego i powinien wystąpić w punkcie cyklu życia strony, który poprzedza, gdy zdarzenie może zostać zgłoszone. Dobry moment na dodanie kodu okablowania zdarzeń znajduje się w etapie wstępnego inicjowania, który występuje bardzo wcześnie w cyklu życia strony.

Otwórz `~/Admin/Products.aspx` i Utwórz procedurę obsługi zdarzeń `Page_PreInit`:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Aby można było ukończyć ten kod okablowania, potrzebujemy programistycznego odwołania do strony wzorcowej ze strony zawartość. Jak wspomniano w poprzednim samouczku, można to zrobić na dwa sposoby:

- Poprzez rzutowanie właściwości `Page.Master` o swobodnej postaci na odpowiedni typ strony głównej lub
- Przez dodanie dyrektywy `@MasterType` na stronie `.aspx`, a następnie użycie właściwości `Master` o jednoznacznie określonym typie.

Użyjmy tej ostatniej metody. Dodaj następującą `@MasterType` dyrektywy na górze znacznika deklaracyjnego strony:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Następnie Dodaj następujący kod okablowania zdarzeń w programie obsługi zdarzeń `Page_PreInit`:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Po tym kodzie widok GridView na stronie zawartość jest odświeżany za każdym razem, gdy zostanie kliknięty przycisk `DoublePrice`.

Ilustracje 8 i 9 ilustrują to zachowanie. Rysunek 8 pokazuje stronę podczas pierwszej wizyty. Zwróć uwagę, że wartości cen w `RecentProducts` GridView (w lewej kolumnie strony głównej) i `ProductsGrid` GridView (na stronie zawartość). Rysunek 9 przedstawia ten sam ekran bezpośrednio po kliknięciu przycisku `DoublePrice`. Jak widać, nowe ceny są natychmiast odzwierciedlane w obu elementach GridView.

[![początkowych wartości cen](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Ilustracja 08**: początkowe wartości cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))

[![w widoku GridView są wyświetlane ceny podwójnie podwójne.](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Ilustracja 09**: ceny dokładnie podwójne są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))

## <a name="summary"></a>Podsumowanie

Najlepiej, gdy strona wzorcowa i jej strony zawartości są całkowicie oddzielone od siebie i nie wymagają interakcji. Jeśli jednak masz stronę wzorcową lub stronę zawartości, która wyświetla dane, które można zmodyfikować ze strony głównej lub strony zawartości, może być konieczne wyświetlenie strony głównej alertu na stronie zawartości (lub na odwrót), gdy dane są modyfikowane, aby można było zaktualizować ekran. W poprzednim samouczku przedstawiono sposób, w jaki strona zawartości jest programowo współdziałać z jej stroną wzorcową. w tym samouczku pokazano, jak mam zainicjowanie interakcji przez stronę wzorcową.

Gdy interakcja programowa między treścią a stroną wzorcową może pochodzić z zawartości lub strony wzorcowej, użyty wzorzec interakcji zależy od pochodzenia. Różnice wynikają z faktu, że strona zawartości ma pojedynczą stronę wzorcową, ale strona wzorcowa może mieć wiele różnych stron zawartości. Zamiast konieczności bezpośredniego działania strony wzorcowej ze stroną zawartości, lepszym rozwiązaniem jest wygenerowanie zdarzenia przez stronę wzorcową, aby sygnalizować, że wykonano pewne czynności. Te strony zawartości, które zadbają o akcję, mogą tworzyć programy obsługi zdarzeń.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Uzyskiwanie dostępu do danych i aktualizowanie ich w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Zdarzenia i Delegaty](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Przekazywanie informacji między zawartością a stronami wzorcowymi](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Praca z danymi w samouczkach ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został suchi Banerjee. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](interacting-with-the-master-page-from-the-content-page-cs.md)
> [dalej](master-pages-and-asp-net-ajax-cs.md)

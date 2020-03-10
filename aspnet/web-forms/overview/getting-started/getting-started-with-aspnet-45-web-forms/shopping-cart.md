---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Koszyk | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: d3b619ebd9448d30857ffbaf17fd245b1d54a662
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641471"
---
# <a name="shopping-cart"></a>Koszyk

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku opisano logikę biznesową wymaganą do dodania koszyka do przykładowej aplikacji Wingtip zabawki ASP.NET Web Forms. W tym samouczku przedstawiono poprzednie samouczki "Wyświetlanie elementów danych i szczegółów" oraz jest częścią serii samouczków dotyczących sklepu Wingtip. Po ukończeniu tego samouczka użytkownicy przykładowej aplikacji będą mogli dodawać, usuwać i modyfikować produkty w koszyku zakupów.

## <a name="what-youll-learn"></a>Zawartość:

1. Jak utworzyć koszyk dla aplikacji sieci Web.
2. Jak umożliwić użytkownikom dodawanie elementów do koszyka.
3. Jak dodać kontrolkę [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) , aby wyświetlić szczegóły koszyka zakupów.
4. Obliczanie i wyświetlanie sumy zamówienia.
5. Jak usuwać i aktualizować elementy w koszyku zakupów.
6. Jak uwzględnić licznik koszyka zakupów.

## <a name="code-features-in-this-tutorial"></a>Funkcje kodu w tym samouczku:

1. Entity Framework Code First
2. Adnotacje danych
3. Kontrolki danych o jednoznacznie określonym typie
4. Powiązanie modelu

## <a name="creating-a-shopping-cart"></a>Tworzenie koszyka

Wcześniej w tej serii samouczków dodano strony i kod umożliwiający wyświetlenie danych produktu z bazy danych. W tym samouczku utworzysz koszyk do zarządzania produktami, które użytkownicy chcą kupić. Użytkownicy będą mogli przeglądać i dodawać elementy do koszyka, nawet jeśli nie są zarejestrowani lub zarejestrowani. Aby zarządzać dostępem do koszyka zakupów, użytkownicy są przypisani unikatowymi `ID` przy użyciu unikatowego identyfikatora globalnego (GUID), gdy użytkownik uzyskuje dostęp do koszyka po raz pierwszy. Ten `ID` będzie przechowywany przy użyciu stanu sesji ASP.NET.

> [!NOTE] 
> 
> Stan sesji ASP.NET jest wygodnym miejscem do przechowywania informacji specyficznych dla użytkownika, które wygasną po opuszczeniu witryny przez użytkownika. W przypadku nieprawidłowego stanu sesji może to mieć wpływ na wydajność większych lokacji, jednak w celach demonstracyjnych dobrze jest korzystać ze stanu sesji. Przykładowy projekt Wingtip zabawki pokazuje, jak używać stanu sesji bez zewnętrznego dostawcy, w którym stan sesji jest przechowywany w procesie na serwerze sieci Web hostującym lokację. W przypadku większych lokacji, które udostępniają wiele wystąpień aplikacji lub dla lokacji, które uruchamiają wiele wystąpień aplikacji na różnych serwerach, należy rozważyć użycie **usługi Windows Azure cache Service**. Ta Cache Service zapewnia usługę rozproszonego buforowania, która jest zewnętrzna względem witryny sieci Web i rozwiązuje problem związany z używaniem stanu sesji w procesie. Aby uzyskać więcej informacji, zobacz [jak używać stanu sesji ASP.NET z witrynami sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).

### <a name="add-cartitem-as-a-model-class"></a>Dodaj CartItem jako klasę modelu

Wcześniej w tej serii samouczków został zdefiniowany schemat dla kategorii i danych produktu, tworząc klasy `Category` i `Product` w folderze *modele* . Teraz Dodaj nową klasę, aby zdefiniować schemat dla koszyka. W dalszej części tego samouczka dodasz klasę do obsługi dostępu do danych do tabeli `CartItem`. Ta klasa udostępnia logikę biznesową, która umożliwia dodawanie, usuwanie i aktualizowanie elementów w koszyku.

1. Kliknij prawym przyciskiem myszy folder *modele* i wybierz polecenie **Dodaj** -&gt; **nowy element**. 

    ![Koszyk — nowy element](shopping-cart/_static/image1.png)
2. Zostanie wyświetlone okno dialogowe **Dodaj nowy element** . Wybierz pozycję **kod**, a następnie wybierz pozycję **Klasa**. 

    ![Koszyk — okno dialogowe Dodawanie nowego elementu](shopping-cart/_static/image2.png)
3. Nadaj tej nowej klasie nazwę *CartItem.cs*.
4. Kliknij pozycję **Add** (Dodaj).  
   Nowy plik klasy zostanie wyświetlony w edytorze.
5. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

Klasa `CartItem` zawiera schemat definiujący każdy produkt dodawany przez użytkownika do koszyka zakupów. Ta klasa jest podobna do innych klas schematu utworzonych wcześniej w tej serii samouczków. Zgodnie z Konwencją Entity Framework Code First oczekuje, że klucz podstawowy dla tabeli `CartItem` będzie `CartItemId` lub `ID`. Jednak kod zastępuje zachowanie domyślne za pomocą atrybutu `[Key]` adnotacji danych. Atrybut `Key` właściwości ItemId określa, że właściwość `ItemID` jest kluczem podstawowym.

Właściwość `CartId` określa `ID` użytkownika, który jest skojarzony z elementem do zakupu. Dodasz kod w celu utworzenia tego użytkownika `ID`, gdy użytkownik uzyskuje dostęp do koszyka. Ten `ID` będzie również przechowywany jako zmienna sesji ASP.NET.

### <a name="update-the-product-context"></a>Aktualizowanie kontekstu produktu

Oprócz dodawania klasy `CartItem` należy zaktualizować klasę kontekstu bazy danych, która zarządza klasami jednostki i która zapewnia dostęp do danych w bazie danych. W tym celu należy dodać nowo utworzoną klasę `CartItem` modelu do klasy `ProductContext`.

1. W **Eksplorator rozwiązań**Znajdź i otwórz plik *ProductContext.cs* w folderze *modele* .
2. Dodaj wyróżniony kod do pliku *ProductContext.cs* w następujący sposób:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Jak wspomniano wcześniej w tej serii samouczków, kod w pliku *ProductContext.cs* dodaje przestrzeń nazw `System.Data.Entity`, dzięki czemu masz dostęp do wszystkich podstawowych funkcji Entity Framework. Ta funkcja obejmuje możliwość wykonywania zapytań, wstawiania, aktualizowania i usuwania danych przez pracę z silnie określonymi obiektami. Klasa `ProductContext` dodaje dostęp do nowo dodanej klasy modelu `CartItem`.

### <a name="managing-the-shopping-cart-business-logic"></a>Zarządzanie logiką biznesową koszyka zakupów

Następnie utworzysz klasę `ShoppingCart` w nowym folderze *logiki* . Klasa `ShoppingCart` obsługuje dostęp do danych do tabeli `CartItem`. Klasa zawiera również logikę biznesową, która umożliwia dodawanie, usuwanie i aktualizowanie elementów w koszyku.

Logika koszyka zakupów, którą dodasz, będzie zawierać funkcje do zarządzania następującymi akcjami:

1. Dodawanie elementów do koszyka zakupów
2. Usuwanie elementów z koszyka zakupów
3. Pobieranie identyfikatora koszyka zakupów
4. Pobieranie elementów z koszyka zakupów
5. Łączna ilość wszystkich elementów koszyka zakupów
6. Aktualizowanie danych koszyka zakupów

Strona koszyka zakupów (*ShoppingCart. aspx*) i koszyka zakupów zostaną użyte razem w celu uzyskania dostępu do danych koszyka zakupów. Na stronie koszyka zakupów zostaną wyświetlone wszystkie elementy dodawane przez użytkownika do koszyka zakupów. Poza stroną koszyka i klasą zakupów utworzysz stronę (*AddToCart. aspx*), aby dodać produkty do koszyka. Dodasz również kod do strony *ProductList. aspx* oraz strony *ProductDetails. aspx* , która będzie udostępniać link do strony *AddToCart. aspx* , dzięki czemu użytkownik może dodawać produkty do koszyka.

Na poniższym diagramie przedstawiono proces podstawowy, który występuje, gdy użytkownik dodaje produkt do koszyka.

![Koszyk — Dodawanie do koszyka zakupów](shopping-cart/_static/image3.png)

Gdy użytkownik kliknie link **Dodaj do koszyka** na stronie *ProductList. aspx* lub *ProductDetails. aspx* , aplikacja przejdzie do strony *AddToCart. aspx* , a następnie automatycznie do strony *ShoppingCart. aspx* . Na stronie *AddToCart. aspx* zostanie dodany wybór produktu do koszyka przez wywołanie metody w klasie ShoppingCart. Na stronie *ShoppingCart. aspx* zostaną wyświetlone produkty, które zostały dodane do koszyka.

#### <a name="creating-the-shopping-cart-class"></a>Tworzenie klasy koszyka zakupów

Klasa `ShoppingCart` zostanie dodana do oddzielnego folderu w aplikacji, dzięki czemu będzie jasne rozróżnienie między modelem (folderem modeli), stronami (folderem głównym) i logiką (folderem logiki).

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **WingtipToys**i wybierz pozycję **Dodaj**-&gt;**Nowy folder**. Nadaj nazwę nowej *logiki*folderu.
2. Kliknij prawym przyciskiem myszy folder *logiki* , a następnie wybierz pozycję **Dodaj** -&gt; **nowy element**.
3. Dodaj nowy plik klasy o nazwie *ShoppingCartActions.cs*.
4. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Metoda `AddToCart` umożliwia uwzględnienie poszczególnych produktów w koszyku na podstawie `ID`produktu. Produkt zostanie dodany do koszyka lub jeśli koszyk zawiera już element dla tego produktu, ilość jest zwiększana.

Metoda `GetCartId` zwraca koszyk `ID` dla użytkownika. `ID` koszyka służy do śledzenia elementów, które użytkownik ma w swoim koszyku. Jeśli użytkownik nie ma istniejącego koszyka `ID`, dla nich zostanie utworzony nowy koszyk `ID`. Jeśli użytkownik jest zalogowany jako zarejestrowanego użytkownika, `ID` koszyk zostanie ustawiony na nazwę użytkownika. Jeśli jednak użytkownik nie jest zalogowany, `ID` koszyka jest ustawiona na unikatową wartość (identyfikator GUID). Identyfikator GUID zapewnia, że dla każdego użytkownika jest tworzony tylko jeden koszyk, w oparciu o sesję.

Metoda `GetCartItems` zwraca listę elementów koszyka zakupów dla użytkownika. W dalszej części tego samouczka zobaczysz, że powiązanie modelu służy do wyświetlania elementów koszyka w koszyku przy użyciu metody `GetCartItems`.

### <a name="creating-the-add-to-cart-functionality"></a>Tworzenie funkcji dodawania do koszyka

Jak wspomniano wcześniej, utworzysz stronę przetwarzania o nazwie *AddToCart. aspx* , która będzie używana do dodawania nowych produktów do koszyka zakupów użytkownika. Ta strona wywoła metodę `AddToCart` w klasie `ShoppingCart`, która została właśnie utworzona. Strona *AddToCart. aspx* będzie oczekiwać, że `ID` produktu zostanie do niego przeniesiona. Ten `ID` produktu będzie używany podczas wywoływania metody `AddToCart` w klasie `ShoppingCart`.

> [!NOTE] 
> 
> Zmodyfikujesz kod w tle (*AddToCart.aspx.cs*) na tej stronie, a nie interfejs użytkownika strony (*AddToCart. aspx*).

#### <a name="to-create-the-add-to-cart-functionality"></a>Aby utworzyć funkcję dodawania do koszyka:

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **WingtipToys**, kliknij polecenie **Dodaj** -&gt; **nowy element**.  
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Dodawanie standardowej nowej strony (formularz sieci Web) do aplikacji o nazwie *AddToCart. aspx*. 

    ![Koszyk — Dodaj formularz sieci Web](shopping-cart/_static/image4.png)
3. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy stronę *AddToCart. aspx* , a następnie kliknij pozycję **Wyświetl kod**. Plik związany z kodem *AddToCart.aspx.cs* jest otwarty w edytorze.
4. Zastąp istniejący kod w kodzie *AddToCart.aspx.cs* następującym:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Po załadowaniu strony *AddToCart. aspx* produkt `ID` jest pobierany z ciągu zapytania. Następnie wystąpienie klasy koszyka zakupów jest tworzone i używane do wywołania metody `AddToCart`, która została dodana wcześniej w tym samouczku. Metoda `AddToCart`, zawarta w pliku *ShoppingCartActions.cs* , zawiera logikę umożliwiającą dodanie wybranego produktu do koszyka zakupów lub zwiększenie ilości produktu wybranego produktu. Jeśli produkt nie został dodany do koszyka zakupów, produkt zostanie dodany do tabeli `CartItem` bazy danych. Jeśli produkt został już dodany do koszyka, a użytkownik dodaje dodatkowy element tego samego produktu, ilość produktu jest zwiększana w tabeli `CartItem`. Na koniec Strona przekierowuje z powrotem do strony *ShoppingCart. aspx* , którą dodasz w następnym kroku, gdzie użytkownik zobaczy zaktualizowaną listę elementów w koszyku.

Jak wspomniano wcześniej, `ID` użytkownika służy do identyfikowania produktów, które są skojarzone z określonym użytkownikiem. Ta `ID` jest dodawana do wiersza w tabeli `CartItem` za każdym razem, gdy użytkownik doda produkt do koszyka.

### <a name="creating-the-shopping-cart-ui"></a>Tworzenie interfejsu użytkownika koszyka zakupów

Na stronie *ShoppingCart. aspx* zostaną wyświetlone produkty dodane przez użytkownika do koszyka zakupów. Umożliwia także dodawanie, usuwanie i aktualizowanie elementów w koszyku.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WingtipToys**, a następnie kliknij pozycję **Dodaj** -&gt; **nowy element**.  
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Dodaj nową stronę (formularz sieci Web), która zawiera stronę wzorcową, wybierając **formularz sieci Web przy użyciu strony wzorcowej**. Nadaj nowej stronie nazwę *ShoppingCart. aspx*.
3. Wybierz pozycję **site. Master** , aby dołączyć stronę wzorcową do nowo utworzonej strony *. aspx* .
4. Na stronie *ShoppingCart. aspx* Zastąp istniejący znacznik następującym znacznikiem:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

Strona *ShoppingCart. aspx* zawiera kontrolkę **GridView** o nazwie `CartList`. Ta kontrolka używa powiązania modelu, aby powiązać dane koszyka zakupów z bazy danych z formantem **GridView** . Po ustawieniu właściwości `ItemType` formantu **GridView** , wyrażenie powiązania danych `Item` jest dostępne w znaczniku kontrolki, a kontrolka zostanie silnie wpisana. Jak wspomniano wcześniej w tej serii samouczków, możesz wybrać szczegóły obiektu `Item` przy użyciu funkcji IntelliSense. Aby skonfigurować kontrolkę danych do używania powiązania modelu w celu wybrania danych, należy ustawić właściwość `SelectMethod` formantu. W powyższym znaczniku należy ustawić `SelectMethod`, aby użyć metody GetShoppingCartItems, która zwraca listę obiektów `CartItem`. Kontrolka danych **GridView** wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwrócone dane. Należy nadal dodać metodę `GetShoppingCartItems`.

#### <a name="retrieving-the-shopping-cart-items"></a>Pobieranie elementów koszyka zakupów

Następnie Dodaj kod do kodu *ShoppingCart.aspx.cs* , aby pobrać i wypełnić interfejs użytkownika koszyka zakupów.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy stronę *ShoppingCart. aspx* , a następnie kliknij pozycję **Wyświetl kod**. Plik związany z kodem *ShoppingCart.aspx.cs* jest otwarty w edytorze.
2. Zastąp istniejący kod następującym:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Jak wspomniano powyżej, Kontrola danych `GridView` wywołuje metodę `GetShoppingCartItems` w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwrócone dane. Metoda `GetShoppingCartItems` tworzy wystąpienie obiektu `ShoppingCartActions`. Następnie kod używa tego wystąpienia do zwrócenia elementów w koszyku, wywołując metodę `GetCartItems`.

### <a name="adding-products-to-the-shopping-cart"></a>Dodawanie produktów do koszyka zakupów

Gdy zostanie wyświetlona strona *ProductList. aspx* lub *ProductDetails. aspx* , użytkownik będzie mógł dodać produkt do koszyka przy użyciu linku. Po kliknięciu linku aplikacja przechodzi do strony przetwarzania o nazwie *AddToCart. aspx*. Strona *AddToCart. aspx* wywoła metodę `AddToCart` w klasie `ShoppingCart`, która została dodana wcześniej w tym samouczku.

Teraz dodasz link **Dodaj do koszyka** do strony *ProductList. aspx* i *ProductDetails. aspx* . Ten link będzie zawierać `ID` produktu pobrany z bazy danych programu.

1. W **Eksplorator rozwiązań**Znajdź i Otwórz stronę o nazwie *ProductList. aspx*.
2. Dodaj znaczniki wyróżnione kolorem żółtym do strony *ProductList. aspx* , aby cała strona była wyświetlana w następujący sposób:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testowanie koszyka

Uruchom aplikację, aby dowiedzieć się, jak dodać produkty do koszyka.

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.  
 Po utworzeniu bazy danych przez projekt zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .
2. Wybierz pozycję **samochody** z menu nawigacji kategorii.  
 Zostanie wyświetlona strona *ProductList. aspx* zawierająca tylko produkty należące do kategorii "samochody". 

    ![Koszyk samochodów](shopping-cart/_static/image5.png)
3. Kliknij link **Dodaj do koszyka** obok pierwszego produktu na liście (samochód wymienialny).   
 Zostanie wyświetlona strona *ShoppingCart. aspx* pokazująca wybór w koszyku. 

    ![Koszyk — koszyk](shopping-cart/_static/image6.png)
4. Wyświetl dodatkowe produkty, wybierając **płaszczyzny** z menu nawigacji kategorii.
5. Kliknij link **Dodaj do koszyka** obok pierwszego wymienionego produktu.  
 Zostanie wyświetlona strona *ShoppingCart. aspx* z dodatkowym elementem.
6. Zamknij okno przeglądarki.

### <a name="calculating-and-displaying-the-order-total"></a>Obliczanie i wyświetlanie sumy zamówienia

Oprócz dodawania produktów do koszyka zakupów należy dodać metodę `GetTotal` do klasy `ShoppingCart` i wyświetlić łączną kwotę zamówienia na stronie koszyka zakupów.

1. W **Eksplorator rozwiązań**otwórz plik *ShoppingCartActions.cs* w folderze *logiki* .
2. Dodaj następującą metodę `GetTotal` wyróżnioną żółtym do klasy `ShoppingCart`, tak aby Klasa była wyświetlana w następujący sposób:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Najpierw Metoda `GetTotal` Pobiera identyfikator koszyka dla użytkownika. Następnie Metoda pobiera kwotę koszyka przez pomnożenie ceny produktu przez ilość produktu dla każdego produktu wymienionego w koszyku.

> [!NOTE] 
> 
> Powyższy kod używa typu dopuszczającego wartość null "`int?`". Typy dopuszczające wartości null mogą reprezentować wszystkie wartości typu podstawowego, a także jako wartość null. Aby uzyskać więcej informacji, zobacz [używanie typów dopuszczających wartość null](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).

### <a name="modify-the-shopping-cart-display"></a>Modyfikowanie wyświetlania koszyka zakupów

Następnie zmodyfikujesz kod dla strony *ShoppingCart. aspx* , aby wywołać metodę `GetTotal` i wyświetlić tę sumę na stronie *ShoppingCart. aspx* podczas ładowania strony.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy stronę *ShoppingCart. aspx* i wybierz polecenie **Wyświetl kod**.
2. W pliku *ShoppingCart.aspx.cs* zaktualizuj procedurę obsługi `Page_Load`, dodając następujący kod w żółtym:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Po załadowaniu strony *ShoppingCart. aspx* załaduje ona obiekt koszyka zakupów, a następnie pobiera sumę koszyka zakupów, wywołując metodę `GetTotal` klasy `ShoppingCart`. Jeśli koszyk jest pusty, zostanie wyświetlony komunikat z tym efektem.

### <a name="testing-the-shopping-cart-total"></a>Testowanie całkowitego koszyka zakupów

Uruchom aplikację teraz, aby zobaczyć, jak nie tylko dodać produkt do koszyka, ale możesz zobaczyć łączną część koszyka zakupów.

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.  
 Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .
2. Wybierz pozycję **samochody** z menu nawigacji kategorii.
3. Kliknij link **Dodaj do koszyka** obok pierwszego produktu.   
 Zostanie wyświetlona strona *ShoppingCart. aspx* z sumą zamówień. 

    ![Koszyk — suma dla koszyka](shopping-cart/_static/image7.png)
4. Dodaj inne produkty (na przykład płaszczyznę) do koszyka.
5. Na stronie *ShoppingCart. aspx* zostanie wyświetlona zaktualizowana suma dla wszystkich produktów, które zostały dodane. 

    ![Koszyk — wiele produktów](shopping-cart/_static/image8.png)
6. Zatrzymaj uruchomioną aplikację, zamykając okno przeglądarki.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Dodawanie przycisków aktualizacji i wyewidencjonowania do koszyka zakupów

Aby umożliwić użytkownikom modyfikowanie koszyka, dodasz przycisk **aktualizacji** i przycisk **wyewidencjonowania** do strony koszyka zakupów. Przycisk **wyewidencjonowywania** nie jest używany do dalszej części tej serii samouczków.

1. W **Eksplorator rozwiązań**Otwórz stronę *ShoppingCart. aspx* w folderze głównym projektu aplikacji sieci Web.
2. Aby dodać przycisk **Aktualizuj** i przycisk **wyewidencjonowania** do strony *ShoppingCart. aspx* , Dodaj znacznik wyróżniony żółtym do istniejącego znacznika, jak pokazano w poniższym kodzie:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Gdy użytkownik kliknie przycisk **Aktualizuj** , zostanie wywołana procedura obsługi zdarzeń `UpdateBtn_Click`. Ta procedura obsługi zdarzeń wywoła kod, który zostanie dodany w następnym kroku.

Następnie można zaktualizować kod zawarty w pliku *ShoppingCart.aspx.cs* , aby przechodzić do pętli przez elementy koszyka i wywoływać metody `RemoveItem` i `UpdateItem`.

1. W **Eksplorator rozwiązań**otwórz plik *ShoppingCart.aspx.cs* w folderze głównym projektu aplikacji sieci Web.
2. Dodaj następujące sekcje kodu wyróżnione żółtymi do pliku *ShoppingCart.aspx.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Gdy użytkownik kliknie przycisk **Aktualizuj** na stronie *ShoppingCart. aspx* , wywoływana jest metoda UpdateCartItems. Metoda UpdateCartItems pobiera zaktualizowane wartości dla każdego elementu w koszyku. Następnie Metoda UpdateCartItems wywołuje metodę `UpdateShoppingCartDatabase` (dodano i wyjaśniono w następnym kroku) w celu dodania lub usunięcia elementów z koszyka. Gdy baza danych zostanie zaktualizowana w celu odzwierciedlenia aktualizacji koszyka, formant **GridView** zostanie zaktualizowany na stronie koszyka zakupów, wywołując metodę `DataBind` dla **widoku GridView**. Ponadto całkowita kwota zamówienia na stronie koszyka zakupów jest aktualizowana w celu odzwierciedlenia zaktualizowanej listy elementów.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualizowanie i usuwanie elementów koszyka zakupów

Na stronie *ShoppingCart. aspx* można zobaczyć kontrolki, które zostały dodane do aktualizowania ilości elementu i usuwania elementu. Teraz Dodaj kod, który umożliwi działanie tych kontrolek.

1. W **Eksplorator rozwiązań**otwórz plik *ShoppingCartActions.cs* w folderze *logiki* .
2. Dodaj następujący kod wyróżniony kolorem żółtym do pliku klasy *ShoppingCartActions.cs* :   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Metoda `UpdateShoppingCartDatabase` wywołana z metody `UpdateCartItems` na stronie *ShoppingCart.aspx.cs* zawiera logikę do aktualizowania lub usuwania elementów z koszyka. Metoda `UpdateShoppingCartDatabase` wykonuje iterację wszystkich wierszy na liście koszyków zakupów. Jeśli element koszyka zakupów został oznaczony jako usunięty lub ilość jest mniejsza od 1, wywoływana jest metoda `RemoveItem`. W przeciwnym razie element koszyka zakupów jest sprawdzany pod kątem aktualizacji, gdy wywoływana jest metoda `UpdateItem`. Po usunięciu lub zaktualizowaniu elementu koszyka zostaną zapisane zmiany w bazie danych.

Struktura `ShoppingCartUpdates` jest używana do przechowywania wszystkich elementów koszyka zakupów. Metoda `UpdateShoppingCartDatabase` używa struktury `ShoppingCartUpdates`, aby określić, czy którykolwiek z elementów ma zostać zaktualizowany czy usunięty.

W następnym samouczku będziesz używać metody `EmptyCart`, aby wyczyścić koszyk po zakupie produktów. Jednak teraz użyjemy `GetCount` metody, która została właśnie dodana do pliku *ShoppingCartActions.cs* , aby określić, ile elementów znajduje się w koszyku zakupów.

### <a name="adding-a-shopping-cart-counter"></a>Dodawanie licznika koszyka zakupów

Aby umożliwić użytkownikowi wyświetlanie łącznej liczby elementów w koszyku, należy dodać licznik do strony *site. Master* . Ten licznik również działa jako link do koszyka zakupów.

1. W **Eksplorator rozwiązań**Otwórz stronę *site. Master* .
2. Zmodyfikuj znacznik przez dodanie linku do koszyka zakupów, jak pokazano w sekcji żółtej, tak aby pojawił się w następujący sposób:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Następnie zaktualizuj kod związany z plikiem *site.Master.cs* , dodając kod wyróżniony kolorem żółtym w następujący sposób:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Przed renderowaniem strony jako HTML zostanie zgłoszone zdarzenie `Page_PreRender`. W obsłudze `Page_PreRender`, Łączna liczba koszyków zakupów jest określana przez wywołanie metody `GetCount`. Zwrócona wartość jest dodawana do zakresu `cartCount` zawartego w znaczniku strony *.* Tagi `<span>` umożliwiają prawidłowe renderowanie elementów wewnętrznych. Gdy zostanie wyświetlona dowolna strona witryny, zostanie wyświetlona suma koszyka zakupów. Użytkownik może również kliknąć opcję koszyka zakupów, aby wyświetlić koszyk.

## <a name="testing-the-completed-shopping-cart"></a>Testowanie ukończonego koszyka zakupów

Teraz możesz uruchomić aplikację, aby zobaczyć, jak można dodawać, usuwać i aktualizować elementy w koszyku. Kwota koszyka zakupów będzie odzwierciedlać łączny koszt wszystkich elementów w koszyku.

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.  
 Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .
2. Wybierz pozycję **samochody** z menu nawigacji kategorii.
3. Kliknij link **Dodaj do koszyka** obok pierwszego produktu.   
 Zostanie wyświetlona strona *ShoppingCart. aspx* z sumą zamówień.
4. Wybierz **płaszczyzny** z menu nawigacji kategorii.
5. Kliknij link **Dodaj do koszyka** obok pierwszego produktu.
6. Ustaw liczbę pierwszego elementu w koszyku na 3, a następnie zaznacz pole wyboru **Usuń element** drugiego elementu.<a id="a"></a>
7. Kliknij przycisk **Aktualizuj** , aby zaktualizować stronę koszyka zakupów i wyświetlić nową sumę zamówień. 

    ![Aktualizacja koszyka koszyka](shopping-cart/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku utworzono koszyk dla przykładowej aplikacji Wingtip zabawki Web Forms. W tym samouczku użyto Entity Framework Code First, adnotacji danych, kontrolek danych o jednoznacznie określonym typie i powiązania modelu.

Koszyk obsługuje dodawanie, usuwanie i aktualizowanie elementów wybranych przez użytkownika do zakupu. Oprócz implementacji funkcji koszyka zakupów wiesz już, jak wyświetlać elementy koszyka zakupów w kontrolce **GridView** i obliczać sumę zamówień.

Aby zrozumieć, jak opisana funkcja działa w prawdziwej aplikacji biznesowej, możesz zapoznać się z przykładem ASP.NET typu Open Source ( [witryny systemu NopCommerce](https://github.com/nopSolutions/nopCommerce) ). Pierwotnie został utworzony na formularzach sieci Web i w ciągu lat przenoszonych do MVC i teraz ASP.NET Core.

## <a name="addition-information"></a>Dodatkowe informacje

[ASP.NET Session State Overview](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Poprzednie](display_data_items_and_details.md)
> [dalej](checkout-and-payment-with-paypal.md)

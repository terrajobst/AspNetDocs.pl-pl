---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Część 8: koszyk z aktualizacjami AJAX | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 8 obejmuje koszyk z aktualizacjami AJAX.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539257"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a>Część 8. koszyk z aktualizacjami AJAX

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 8 obejmuje koszyk z aktualizacjami AJAX.

Zezwolimy użytkownikom na umieszczenie albumów w ich koszyku bez rejestrowania się, ale konieczne będzie zarejestrowanie się jako gościa w celu ukończenia wyewidencjonowania. Proces zakupów i wyewidencjonowywania zostanie podzielony na dwa kontrolery: kontroler ShoppingCart, który umożliwia anonimowe Dodawanie elementów do koszyka, oraz kontroler wyewidencjonowania obsługujący proces wyewidencjonowania. Zaczniemy od koszyka w tej sekcji, a następnie kompilować proces wyewidencjonowania w poniższej sekcji.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Dodawanie klas modeli koszyk, zamówienie i OrderDetail

Nasze koszyki i procesy wyewidencjonowywania będą korzystać z nowych klas. Kliknij prawym przyciskiem myszy folder modele i Dodaj klasę koszyka (Cart.cs) z poniższym kodem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Ta klasa jest całkiem podobna do innych, z których korzystamy dotąd, z wyjątkiem atrybutu [Key] dla właściwości RecordId. Nasze elementy koszyka będą miały identyfikator ciągu o nazwie CartID, aby umożliwić anonimowe zakupy, ale tabela zawiera klucz podstawowy Integer o nazwie RecordId. Zgodnie z Konwencją, Entity Framework kod — najpierw oczekuje, że klucz podstawowy dla tabeli o nazwie Koszyk będzie miał wartość CartId lub identyfikator, ale możemy łatwo przesłonić ten element za pośrednictwem adnotacji lub kodu, jeśli chcemy. Jest to przykład, w jaki sposób możemy używać prostych Konwencji w Entity Framework kodzie — najpierw w przypadku, gdy są one zgodne, ale nie są ograniczone przez nie.

Następnie Dodaj klasę Order (Order.cs) z poniższym kodem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Ta klasa śledzi podsumowanie i informacje o dostarczaniu dla zamówienia. **Nie będzie jeszcze kompilować**, ponieważ ma właściwość nawigacji OrderDetails, która zależy od klasy, która nie została jeszcze utworzona. Naprawmy, że teraz dodając klasę o nazwie OrderDetail.cs, dodając następujący kod.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Wprowadzimy jedną ostatnią aktualizację do naszej klasy MusicStoreEntities, która będzie zawierać zestawy dbsets, które uwidaczniają te nowe klasy modelu, łącznie z&gt;wykonawcą&lt;Nieogólnymi. Zaktualizowana Klasa MusicStoreEntities zostanie wyświetlona poniżej.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Zarządzanie logiką biznesową koszyka zakupów

Następnie utworzymy klasę ShoppingCart w folderze models. Model ShoppingCart obsługuje dostęp do danych do tabeli koszyka. Ponadto będzie obsługiwał logikę biznesową w celu dodawania i usuwania elementów z koszyka.

Ponieważ nie chcemy wymagać od użytkowników rejestrowania się w celu dodania elementów do koszyka zakupów, przypiszemy użytkownikom tymczasowy unikatowy identyfikator (przy użyciu identyfikatora GUID lub unikatowego identyfikatora globalnego) podczas uzyskiwania dostępu do koszyka. Ten identyfikator zostanie zapisany przy użyciu klasy sesji ASP.NET.

*Uwaga: sesja ASP.NET jest wygodnym miejscem do przechowywania informacji specyficznych dla użytkownika, które wygasną po opuszczeniu witryny. W przypadku nieprawidłowego stanu sesji może to mieć wpływ na wydajność większych lokacji. nasze jasne użycie będzie dobrze działało do celów demonstracyjnych.*

Klasa ShoppingCart uwidacznia następujące metody:

**AddToCart** pobiera album jako parametr i dodaje go do koszyka użytkownika. Ponieważ tabela koszyka śledzi liczbę dla każdego albumu, zawiera logikę umożliwiającą utworzenie nowego wiersza w razie potrzeby lub po prostu zwiększenie ilości, jeśli użytkownik zamówił już jedną kopię albumu.

**RemoveFromCart** Pobiera identyfikator albumu i usuwa go z koszyka użytkownika. Jeśli użytkownik ma tylko jedną kopię albumu w swoim koszyku, wiersz zostanie usunięty.

**EmptyCart** usuwa wszystkie elementy z koszyka użytkownika.

**GetCartItems** pobiera listę CartItems do wyświetlania lub przetwarzania.

**GetCount** pobiera łączną liczbę albumów, które użytkownik ma w swoim koszyku.

Wartość **gettotal** oblicza łączny koszt wszystkich elementów w koszyku.

**Porządek** jest konwertowany na zamówienie w ramach fazy wyewidencjonowania.

**Getkoszyk** to metoda statyczna, która umożliwia naszym kontrolerom uzyskanie obiektu koszyka. Używa metody **GetCartId** , aby obsłużyć odczyt CartId z sesji użytkownika. Metoda GetCartId wymaga HttpContextBase, aby mógł odczytać CartId użytkownika z sesji użytkownika.

Oto kompletna **Klasa ShoppingCart**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>Modele widoków

Nasz kontroler koszyka zakupów będzie musiał komunikować się ze swoimi widokami, które nie są prawidłowo mapowane do naszych obiektów modelu. Nie chcemy modyfikować naszych modeli zgodnie z naszymi widokami. Klasy modelu powinny reprezentować naszą domenę, a nie interfejs użytkownika. Jednym z rozwiązań jest przekazanie informacji do naszych widoków przy użyciu klasy ViewBag, podobnie jak w przypadku listy rozwijanej Menedżera sklepu, ale przekazywanie wielu informacji za pośrednictwem ViewBag jest trudne do zarządzania.

Rozwiązaniem do tego jest użycie wzorca *ViewModel* . W przypadku korzystania z tego wzorca tworzymy klasy o jednoznacznie określonym typie, które są zoptymalizowane pod kątem naszych scenariuszy widoku, i które uwidaczniają właściwości wartości dynamicznych/zawartości wymaganej przez nasze szablony widoków. Nasze klasy kontrolera mogą następnie wypełnić i przekazać te klasy zoptymalizowane pod kątem widoku do naszego szablonu widoku. Zapewnia to bezpieczeństwo typu, sprawdzanie czasu kompilacji i IntelliSense w edytorze w szablonach widoków.

Utworzymy dwa modele widoku do użycia w naszym kontrolerze koszyka: ShoppingCartViewModel będzie przechowywać zawartość koszyka zakupów użytkownika, a ShoppingCartRemoveViewModel będzie służyć do wyświetlania informacji o potwierdzeniach, gdy użytkownik usunie coś z koszyka.

Utwórzmy nowy folder modele widoków w katalogu głównym naszego projektu, aby zachować zorganizowane elementy. Kliknij prawym przyciskiem myszy projekt, wybierz polecenie Dodaj/nowy folder.

![](mvc-music-store-part-8/_static/image1.jpg)

Nazwij folder modele widoków.

![](mvc-music-store-part-8/_static/image1.png)

Następnie Dodaj klasę ShoppingCartViewModel w folderze modele widoków. Ma dwie właściwości: listę elementów koszyka oraz wartość dziesiętną, aby pomieścić całkowitą cenę dla wszystkich elementów w koszyku.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Teraz Dodaj ShoppingCartRemoveViewModel do folderu modele widoków, używając następujących czterech właściwości.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Kontroler koszyka zakupów

Kontroler koszyka zakupów ma trzy główne cele: Dodawanie elementów do koszyka, usuwanie elementów z koszyka i wyświetlanie elementów w koszyku. Spowoduje to użycie trzech klas, które właśnie utworzyliśmy: ShoppingCartViewModel, ShoppingCartRemoveViewModel i ShoppingCart. Podobnie jak w przypadku StoreController i StoreManagerController, dodamy pole do przechowywania wystąpienia MusicStoreEntities.

Dodaj nowy kontroler koszyka zakupów do projektu przy użyciu szablonu pustego kontrolera.

![](mvc-music-store-part-8/_static/image2.png)

Oto kompletny kontroler ShoppingCart. Akcje indeks i Dodaj kontroler powinny wyglądać bardzo dobrze. Operacje kontrolera Remove i CartSummary obsługują dwa specjalne przypadki, które omówiono w poniższej sekcji.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aktualizacje AJAX z użyciem jQuery

Będziemy dalej tworzyć stronę indeksu koszyka zakupów, która jest silnie wpisana do ShoppingCartViewModel i używa szablonu widoku listy przy użyciu tej samej metody jak wcześniej.

![](mvc-music-store-part-8/_static/image3.png)

Jednak zamiast używania pliku HTML. ActionLink do usuwania elementów z koszyka będziemy używać platformy jQuery do "podłączania" dla wszystkich linków w tym widoku, które mają klasę HTML RemoveLink. Zamiast ogłaszać formularz, ten program obsługi zdarzeń kliknij po prostu wywołanie zwrotne AJAX do naszej akcji kontrolera RemoveFromCart. RemoveFromCart zwraca wynik Zserializowany w formacie JSON, który następnie jest analizowany przez wywołanie zwrotne jQuery i wykonuje cztery szybkie aktualizacje strony przy użyciu jQuery:

- 1. Usuwa z listy usunięty album
- 2. Aktualizuje liczbę koszyków w nagłówku
- 3. Wyświetla komunikat aktualizacji dla użytkownika
- 4. Aktualizuje łączną cenę koszyka

Ze względu na to, że scenariusz usuwania jest obsługiwany przez wywołanie zwrotne AJAX w widoku indeksu, nie jest potrzebny dodatkowy widok dla akcji RemoveFromCart. Oto kompletny kod widoku/ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Aby przetestować ten limit, musimy móc dodać elementy do naszego koszyka zakupów. Zaktualizujemy nasz widok **szczegółów sklepu** , aby uwzględnić przycisk "Dodaj do koszyka". Na bieżąco możemy uwzględnić niektóre informacje dodatkowe dotyczące albumu, które zostały dodane od czasu ostatniej aktualizacji tego widoku: gatunek, wykonawca, Cena i album. Zostanie wyświetlony zaktualizowany kod widoku szczegółów sklepu, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Teraz możemy klikać w sklepie i testować Dodawanie i usuwanie albumów do i z naszego koszyka zakupów. Uruchom aplikację i przejdź do indeksu magazynu.

![](mvc-music-store-part-8/_static/image4.png)

Następnie kliknij gatunek, aby wyświetlić listę albumów.

![](mvc-music-store-part-8/_static/image5.png)

Kliknięcie tytułu albumu zawiera teraz nasz zaktualizowany widok szczegółów albumu, w tym przycisk "Dodaj do koszyka".

![](mvc-music-store-part-8/_static/image6.png)

Kliknięcie przycisku "Dodaj do koszyka" pokazuje widok indeksu koszyka zakupów z listą podsumowania koszyka zakupów.

![](mvc-music-store-part-8/_static/image7.png)

Po załadowaniu koszyka zakupów możesz kliknąć link Usuń z koszyka, aby zobaczyć aktualizację AJAX do koszyka.

![](mvc-music-store-part-8/_static/image8.png)

Utworzyliśmy roboczy koszyk, który umożliwia niezarejestrowanim użytkownikom dodawanie elementów do ich koszyka. W poniższej sekcji zezwolimy im na zarejestrowanie i zakończenie procesu wyewidencjonowania.

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-7.md)
> [dalej](mvc-music-store-part-9.md)

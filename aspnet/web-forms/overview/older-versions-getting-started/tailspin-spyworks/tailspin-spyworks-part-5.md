---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Część 5: logika biznesowa | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 5 dodaje logikę biznesową.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630306"
---
# <a name="part-5-business-logic"></a>Część 5. Logika biznesowa

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 5 dodaje logikę biznesową.

## <a id="_Toc260221671"></a>Dodawanie jakiejś logiki biznesowej

Chcemy, aby nasze środowisko zakupów było dostępne za każdym razem, gdy ktoś odwiedzi naszą witrynę sieci Web. Osoby odwiedzające będą mogły przeglądać i dodawać elementy do koszyka, nawet jeśli nie są zarejestrowane lub zarejestrowani. Gdy użytkownicy są gotowi do wyewidencjonowania, otrzymają możliwość uwierzytelnienia, a jeśli nie są jeszcze członkami, będą mogli utworzyć konto.

Oznacza to, że konieczne będzie wdrożenie logiki w celu przekonwertowania koszyka ze stanu anonimowego na stan zarejestrowanego użytkownika.

Utwórzmy katalog o nazwie "classes", a następnie kliknij prawym przyciskiem myszy folder i Utwórz nowy plik "Class" o nazwie MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Jak wspomniano wcześniej, rozszerzamy klasę, która implementuje stronę MyShoppingCart. aspx i będziemy używać. Zaawansowana konstrukcja sieciowa "klasy częściowej".

Wygenerowane wywołanie naszego pliku MyShoppingCart.aspx.cf wygląda następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Zwróć uwagę na użycie słowa kluczowego "częściowe".

Właśnie wygenerowany plik klasy wygląda następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Będziemy scalać nasze implementacje, dodając słowo kluczowe części do tego pliku.

Nasz nowy plik klasy będzie teraz wyglądać następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Pierwszą metodą dodawaną do naszej klasy jest metoda "AddItem". Jest to metoda, która ostatecznie będzie wywoływana, gdy użytkownik kliknie linki "Dodaj do grafiki" na liście produktów i na stronach szczegółów produktu.

Dołącz następujące elementy do instrukcji using w górnej części strony.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

I Dodaj tę metodę do klasy MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Używamy LINQ to Entities, aby sprawdzić, czy element znajduje się już w koszyku. W takim przypadku aktualizujemy ilość zamówienia elementu, w przeciwnym razie utworzymy nowy wpis dla wybranego elementu

Aby wywołać tę metodę, zaimplementujemy stronę AddToCart. aspx, która nie tylko jest klasą tej metody, ale zamiast tego zostanie wyświetlona bieżąca zakupy a = koszyk po dodaniu elementu.

Kliknij prawym przyciskiem myszy nazwę rozwiązania w Eksploratorze rozwiązań i Dodaj nową stronę o nazwie AddToCart. aspx, która została wcześniej ukończona.

Chociaż możemy użyć tej strony do wyświetlania wyników pośrednich, takich jak problemy ze stanem niskiego stanu zasobów itp., Strona nie będzie w rzeczywistości renderowana, ale zamiast tego wywołaj logikę "Dodaj" i Przekieruj.

Aby to osiągnąć, dodamy następujący kod do strony\_zdarzenie ładowania.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Należy zauważyć, że pobieramy produkt do dodania do koszyka z parametru QueryString i wywołując metodę addItem klasy.

Przy założeniu, że żadne błędy nie wystąpią, kontrola jest przenoszona do strony SHoppingCart. aspx, która zostanie w pełni zaimplementowana. Jeśli wystąpi błąd, wystąpił wyjątek.

Obecnie nie została jeszcze zaimplementowana globalna procedura obsługi błędów, więc ten wyjątek zostałby nieobsłużony przez naszą aplikację, ale wkrótce nastąpi rozwiązanie tego problemu.

Należy również pamiętać, że użycie instrukcji Debug. Fail () (dostępne za pośrednictwem `using System.Diagnostics;)`

Czy aplikacja działa w debugerze, ta metoda wyświetli szczegółowe okno dialogowe z informacjami o stanie aplikacji wraz z określonym przez nas komunikatem o błędzie.

Podczas wykonywania w środowisku produkcyjnym instrukcja Debug. Fail () jest ignorowana.

Należy zwrócić uwagę na kod powyżej wywołania metody z naszych nazw klas koszyka zakupów "GetShoppingCartId".

Dodaj kod, aby zaimplementować metodę w następujący sposób.

Należy zauważyć, że dodano również przyciski aktualizacji i wyewidencjonowania oraz etykietę, w której można wyświetlić koszyk "łącznie".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Teraz możemy dodać elementy do naszego koszyka zakupów, ale nie wprowadziliśmy logiki, aby wyświetlić koszyk po dodaniu produktu.

Dlatego na stronie MyShoppingCart. aspx zostanie dodana kontrolka EntityDataSource i kontrolka GridVire w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Zadzwoń do formularza w projektancie, aby można było dwukrotnie kliknąć przycisk Aktualizuj koszyk i wygenerować procedurę obsługi zdarzeń kliknięcia określoną w deklaracji w znaczniku.

Zaimplementujemy szczegóły później, ale pozwoli to nam na skompilowanie i uruchomienie naszej aplikacji bez błędów.

Gdy uruchamiasz aplikację i dodasz element do koszyka zakupów, zobaczysz.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Zwróć uwagę na to, że od "domyślnego" wyświetlania siatki są one stosowane przez implementację trzech kolumn niestandardowych.

Pierwszym z nich jest edytowalne pole "Bound" dla danej ilości:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Następna jest kolumna "obliczeniowa", w której wyświetlana jest suma elementu wiersza (element jest kosztem do uporządkowania):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Na koniec mamy kolumnę niestandardową, która zawiera kontrolkę CheckBox, która będzie używana przez użytkownika w celu wskazania, że element powinien zostać usunięty z wykresu zakupów.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Jak widać, wiersz sumy zamówienia jest pusty, dlatego dodamy kilka logiki w celu obliczenia sumy zamówienia.

Najpierw zaimplementujmy metodę "gettotal" dla naszej klasy MyShoppingCart.

W pliku MyShoppingCart.cs Dodaj następujący kod.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Następnie na stronie\_Załaduj procedurę obsługi zdarzeń możemy wywołać metodę gettotal. W tym samym czasie dodamy test, aby sprawdzić, czy koszyk jest pusty i odpowiednio dostosować wyświetlanie.

Jeśli koszyk jest pusty, możemy to zrobić:

![](tailspin-spyworks-part-5/_static/image4.jpg)

A jeśli nie, zobaczymy nasze podsumowanie.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Jednak ta strona nie została jeszcze ukończona.

Potrzebujemy dodatkowej logiki, aby ponownie obliczyć koszyk, usuwając elementy oznaczone do usunięcia i określając nowe wartości ilości, które mogły zostać zmienione w siatce przez użytkownika.

Umożliwia dodanie metody "RemoveItem" do naszej klasy koszyka zakupów w MyShoppingCart.cs, aby obsłużyć przypadek, gdy użytkownik oznaczy element do usunięcia.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Teraz przejdźmy do usługi AD, aby obsłużyć okoliczności, gdy użytkownik po prostu zmieni jakość do uporządkowania w GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Korzystając z podstawowych funkcji usuwania i aktualizowania w miejscu, możemy wdrożyć logikę, która rzeczywiście aktualizuje koszyk w bazie danych. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Należy zauważyć, że ta metoda oczekuje dwóch parametrów. Jednym z nich jest identyfikator koszyka zakupów, a drugi to tablica obiektów typu zdefiniowanego przez użytkownika.

Aby zminimalizować zależność naszej logiki od specyficznych dla interfejsu użytkownika, definiujemy strukturę danych, za pomocą której możemy przekazać elementy koszyka zakupów do naszego kodu bez konieczności bezpośredniego dostępu do formantu GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

W naszym pliku MyShoppingCart.aspx.cs możemy użyć tej struktury na naszym przycisku aktualizacji kliknij program obsługi zdarzeń w następujący sposób. Pamiętaj, że oprócz aktualizowania koszyka będziemy również aktualizować sumę koszyka.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Należy pamiętać o szczególnym znaczeniu dla tego wiersza kodu:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues () to specjalna funkcja pomocnicza, która zostanie zaimplementowana w MyShoppingCart.aspx.cs w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Zapewnia to czysty sposób uzyskiwania dostępu do wartości elementów powiązanych w naszym formancie GridView. Ponieważ nasz formant CheckBox "Usuń element" nie jest powiązany, będziemy uzyskiwać do niego dostęp za pośrednictwem metody FindControl ().

Na tym etapie opracowywania projektu jesteśmy gotowi do wdrożenia procesu wyewidencjonowania.

Przed wykonaniem tej czynności Użyj programu Visual Studio do wygenerowania bazy danych członkostwa i dodania użytkownika do repozytorium członkostwa.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-4.md)
> [dalej](tailspin-spyworks-part-6.md)

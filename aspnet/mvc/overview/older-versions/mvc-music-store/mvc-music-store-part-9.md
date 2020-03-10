---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Część 9: Rejestracja i wyewidencjonowywanie | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 9 obejmuje rejestrację i wyewidencjonowanie.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559536"
---
# <a name="part-9-registration-and-checkout"></a>Część 9. Rejestracja i finalizacja zakupu

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 9 obejmuje rejestrację i wyewidencjonowanie.

W tej sekcji zostanie utworzona CheckoutController, która będzie zbierać informacje o adresie i płatnościach klientów. Wymagamy od użytkowników zarejestrowania się w naszej witrynie przed wyewidencjonowaniem, więc ten kontroler będzie wymagał autoryzacji.

Użytkownicy przejdą do procesu wyewidencjonowania w koszyku, klikając przycisk "Wyewidencjonuj".

![](mvc-music-store-part-9/_static/image1.jpg)

Jeśli użytkownik nie jest zalogowany, zostanie wyświetlony monit o podanie.

![](mvc-music-store-part-9/_static/image1.png)

Po pomyślnym zalogowaniu użytkownika zostanie wyświetlony widok adres i płatność.

![](mvc-music-store-part-9/_static/image2.png)

Po wypełnieniu formularza i przesłaniu zamówienia zostanie wyświetlony ekran potwierdzenie zamówienia.

![](mvc-music-store-part-9/_static/image3.png)

Próba wyświetlenia nieistniejącej kolejności lub zamówienia, które nie należy do Ciebie, spowoduje wyświetlenie widoku błędów.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrowanie koszyka

Gdy proces zakupów jest anonimowy, gdy użytkownik kliknie przycisk wyewidencjonowania, będzie wymagane zarejestrowanie i zalogowanie się. Użytkownicy będą oczekiwać, że firma Microsoft będzie obsługiwać informacje o koszyku zakupów między wizytami, więc należy skojarzyć informacje o koszyku z użytkownikiem po zakończeniu rejestracji lub zalogowaniu się.

Jest to bardzo proste, ponieważ Klasa ShoppingCart ma już metodę, która spowoduje skojarzenie wszystkich elementów w bieżącym koszyku z nazwą użytkownika. Po ukończeniu rejestracji lub zalogowania użytkownik będzie musiał wywołać tę metodę.

Otwórz klasę **elementu AccountController** , która została dodana podczas konfigurowania członkostwa i autoryzacji. Dodaj instrukcję using odwołującą się do MvcMusicStore. Models, a następnie Dodaj następującą metodę MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Następnie zmodyfikuj akcję post logowania w celu wywołania MigrateShoppingCart po sprawdzeniu poprawności użytkownika, jak pokazano poniżej:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Wprowadź tę samą zmianę w akcji rejestracji po pomyślnym utworzeniu konta użytkownika:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

To teraz — anonimowy koszyk do zakupów zostanie automatycznie przeniesiony na konto użytkownika po pomyślnej rejestracji lub zalogowaniu się.

## <a name="creating-the-checkoutcontroller"></a>Tworzenie CheckoutController

Kliknij prawym przyciskiem myszy folder controllers i Dodaj nowy kontroler do projektu o nazwie CheckoutController przy użyciu szablonu pustego kontrolera.

![](mvc-music-store-part-9/_static/image5.png)

Najpierw Dodaj atrybut Autoryzuj powyżej deklaracji klasy kontrolera, aby wymagać od użytkowników rejestrowania przed wyewidencjonowaniem:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Uwaga: jest to podobne do wprowadzonej wcześniej zmiany w StoreManagerController, ale w takim przypadku atrybut Autoryzuj wymaga, aby użytkownik miał rolę administratora. W kontrolerze wyewidencjonowania wymagamy, aby użytkownik był zalogowany, ale nie musi być administratorem.*

Ze względu na prostotę nie będziemy omawiać informacji o płatnościach w tym samouczku. Zamiast tego zezwalamy użytkownikom na wyewidencjonowywanie przy użyciu kodu promocyjnego. Ten kod promocyjny będzie przechowywany przy użyciu stałej o nazwie PromoCode.

Podobnie jak w StoreController, zadeklarujemy pole, aby pomieścić wystąpienie klasy MusicStoreEntities o nazwie storeDB. Aby można było korzystać z klasy MusicStoreEntities, konieczne będzie dodanie instrukcji using dla przestrzeni nazw MvcMusicStore. models. Poniżej znajduje się Górna część naszego kontrolera wyewidencjonowania.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController będą miały następujące akcje kontrolera:

**AddressAndPayment (metoda Get)** wyświetli formularz umożliwiający użytkownikowi wprowadzanie informacji.

**AddressAndPayment (Metoda post)** sprawdzi dane wejściowe i przetworzy zamówienie.

**Zakończenie** zostanie wyświetlone po pomyślnym zakończeniu procesu wyewidencjonowania przez użytkownika. Ten widok będzie zawierać numer zamówienia użytkownika w postaci potwierdzenia.

Najpierw Zmień nazwę akcji kontrolera indeksu (która została wygenerowana podczas tworzenia kontrolera) do AddressAndPayment. Ta akcja kontrolera po prostu wyświetla formularz wyewidencjonowania, dlatego nie wymaga żadnych informacji o modelu.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Nasza metoda POST AddressAndPayment będzie zgodna z tym samym wzorcem, który został użyty w StoreManagerController: spróbuje zaakceptować przesłane formularz i zakończyć zamówienie i ponownie wyświetlić formularz, jeśli zakończy się niepowodzeniem.

Po sprawdzeniu poprawności formularza spełnia wymagania dotyczące weryfikacji dla zamówienia, wartość formularza PromoCode zostanie sprawdzona bezpośrednio. Przy założeniu, że wszystko jest poprawne, będziemy zapisywać zaktualizowane informacje z kolejnością, poinstruować obiekt ShoppingCart, aby dokończył proces zamówienia, i przekierować do kompletnej akcji.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Po pomyślnym zakończeniu procesu wyewidencjonowania użytkownicy zostaną przekierowani do akcji Ukończ kontroler. Ta akcja spowoduje wykonanie prostej kontroli w celu sprawdzenia, czy zamówienie rzeczywiście należy do zalogowanego użytkownika przed pokazywaniem numeru zamówienia jako potwierdzenia.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Uwaga: widok błędów został automatycznie utworzony dla nas w folderze/Views/Shared po rozpoczęciu projektu.*

Kompletny kod CheckoutController jest następujący:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Dodawanie widoku AddressAndPayment

Teraz Utwórzmy widok AddressAndPayment. Kliknij prawym przyciskiem myszy jedną z akcji kontrolera AddressAndPayment i Dodaj widok o nazwie AddressAndPayment, który jednoznacznie wpisano jako zamówienie i używa szablonu edycji, jak pokazano poniżej.

![](mvc-music-store-part-9/_static/image6.png)

Ten widok umożliwia korzystanie z dwóch technik, które oglądamy podczas tworzenia widoku StoreManagerEdit:

- Użyjemy html. EditorForModel () do wyświetlania pól formularza dla modelu zamówienia
- Będziemy korzystać z reguł walidacji przy użyciu klasy Order z atrybutami walidacji

Zaczniemy od zaktualizowania kodu formularza do używania HTML. EditorForModel (), a następnie dodatkowego pola tekstowego dla kodu promocyjnego. Poniżej przedstawiono pełen kod widoku AddressAndPayment.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definiowanie reguł walidacji dla zamówienia

Teraz, gdy nasz widok jest skonfigurowany, skonfigurujemy reguły sprawdzania poprawności dla modelu zamówienia, tak jak wcześniej dla modelu albumu. Kliknij prawym przyciskiem myszy folder modele i Dodaj klasę o nazwie Order. Oprócz atrybutów weryfikacji użytych wcześniej dla albumu należy również użyć wyrażenia regularnego do zweryfikowania adresu e-mail użytkownika.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Próba przesłania formularza z brakującymi lub nieprawidłowymi informacjami spowoduje wyświetlenie komunikatu o błędzie przy użyciu walidacji po stronie klienta.

![](mvc-music-store-part-9/_static/image7.png)

Tak, większość pracy w procesie wyewidencjonowania została wykonana. Mamy tylko kilka szanse i zakończy się. Musimy dodać dwa proste widoki i będziemy musieli zadbać o przekazanie informacji o koszyku podczas procesu logowania.

## <a name="adding-the-checkout-complete-view"></a>Dodawanie widoku ukończenia wyewidencjonowania

Widok Kończenie wyewidencjonowywania jest bardzo prosty, ponieważ wymaga tylko wyświetlenia identyfikatora zamówienia. Kliknij prawym przyciskiem myszy akcję Ukończ kontroler i Dodaj widok o nazwie Complete, który jest silnie określony jako int.

![](mvc-music-store-part-9/_static/image8.png)

Teraz będziemy aktualizować kod widoku w celu wyświetlenia identyfikatora zamówienia, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aktualizowanie widoku błędów

Szablon domyślny zawiera widok błędów w folderze widoki udostępnione, dzięki czemu można go ponownie używać w innym miejscu w witrynie. Ten widok błędów zawiera bardzo prosty błąd i nie używa naszego układu witryny, dlatego zaktualizujemy go.

Ponieważ jest to ogólna strona błędu, zawartość jest bardzo prosta. Jeśli użytkownik chce ponownie spróbować wykonać tę akcję, zostanie uwzględniony komunikat i link do przechodzenia do poprzedniej strony w historii.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-8.md)
> [dalej](mvc-music-store-part-10.md)

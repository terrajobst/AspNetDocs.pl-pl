---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iteracja #3 — Dodawanie walidacji formularza (VB) | Microsoft Docs'
author: microsoft
description: W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 73b307f53875abe84b592c75b1ff614ffd9d8b82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544451"
---
# <a name="iteration-3--add-form-validation-vb"></a>Iteracja #3 — Dodawanie walidacji formularza (VB)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji Contact Management ASP.NET MVC (VB)

W tej serii samouczków tworzymy całą aplikację do zarządzania kontaktami od początku do końca. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazw, numerów telefonów i adresów e-mail — w celu uzyskania listy osób.

Aplikacja została utworzona przez wiele iteracji. W przypadku każdej iteracji stopniowo ulepszamy aplikację. Celem tej wielu iteracji jest umożliwienie zrozumienia przyczyny każdej zmiany.

- #1 iteracji — Utwórz aplikację. W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

- Iteracja #2 — Zwiększ wygląd aplikacji. W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

- Iteracja #4 — możliwość swobodnego łączenia aplikacji. W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

- #5 iteracji — Utwórz testy jednostkowe. W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

- Iteracja #6 — Użyj programowania opartego na testach. W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

- Iteracja #7 — Dodawanie funkcji AJAX. W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Ta iteracja

W drugiej iteracji aplikacji Contact Manager dodawana jest podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie kontaktów bez wprowadzania wartości dla wymaganych pól formularza. Sprawdzamy również numery telefonów i adresy e-mail (patrz rysunek 1).

[![okno dialogowe Nowy projekt](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Ilustracja 01**. formularz z walidacją ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](iteration-3-add-form-validation-vb/_static/image2.png))

W tej iteracji dodawana jest logika walidacji bezpośrednio do akcji kontrolera. Ogólnie rzecz biorąc, nie jest to zalecany sposób dodawania walidacji do aplikacji ASP.NET MVC. Lepszym rozwiązaniem jest umieszczenie logiki walidacji aplikacji w oddzielnej [warstwie usług](http://martinfowler.com/eaaCatalog/serviceLayer.html). W następnej iteracji będziemy refaktoryzacji aplikacji menedżera kontaktów, aby zwiększyć łatwość obsługi aplikacji.

W tej iteracji, aby zachować prostotę, należy ręcznie napisać cały kod sprawdzania poprawności. Zamiast pisać kod weryfikacyjny wypróbujemy, możemy skorzystać z platformy walidacji. Na przykład można użyć bloku Application Validation Library (VAB) firmy Microsoft, aby zaimplementować logikę sprawdzania poprawności dla aplikacji ASP.NET MVC. Aby dowiedzieć się więcej o bloku aplikacji walidacji, zobacz:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Dodawanie walidacji do widoku tworzenia

Aby rozpocząć, Dodaj logikę walidacji do widoku tworzenia. Ze względu na to, że Wygenerowano widok Tworzenie za pomocą programu Visual Studio, widok tworzenie zawiera już wszystkie niezbędne logiki interfejsu użytkownika do wyświetlania komunikatów sprawdzania poprawności. Widok tworzenie znajduje się na liście 1.

**Lista 1 — \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

Klasa Field-Validation-Error służy do nadawania stylu dane wyjściowe renderowane przez pomocnika html. ValidationMessage (). Klasa Input-Validation-Error służy do nadawania stylu TextBox (Input) renderowanego przez pomocnika html. TextBox (). Klasa walidacji-Summary-Errors służy do nadawania stylu listę nieuporządkowaną renderowaną przez pomocnika html. podsumowania walidacji ().

> [!NOTE] 
> 
> Możesz zmodyfikować klasy arkuszy stylów opisane w tej sekcji, aby dostosować wygląd komunikatów o błędach walidacji.

## <a name="adding-validation-logic-to-the-create-action"></a>Dodawanie logiki walidacji do akcji tworzenia

Teraz widok Tworzenie nigdy nie wyświetla walidacji komunikatów o błędach, ponieważ nie zapisano logiki w celu wygenerowania żadnych komunikatów. Aby wyświetlić komunikaty o błędach walidacji, należy dodać komunikaty o błędach do ModelState.

> [!NOTE] 
> 
> Metoda UpdateModel () dodaje komunikaty o błędach do ModelState automatycznie, gdy wystąpi błąd podczas przypisywania wartości pola formularza do właściwości. Na przykład, jeśli spróbujesz przypisać ciąg "Apple" do właściwości DataUrodzenia, która akceptuje wartości DateTime, a następnie Metoda UpdateModel () dodaje błąd do ModelState.

Zmodyfikowana Metoda Create () na liście 2 zawiera nową sekcję, która sprawdza poprawność właściwości klasy Contact przed wstawieniem nowego kontaktu do bazy danych.

**Lista 2 — Controllers\ContactController.vb (tworzenie z walidacją)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

Sekcja Validate wymusza cztery unikatowe reguły sprawdzania poprawności:

- Właściwość FirstName musi mieć długość większą od zera (i nie może składać się z samych spacji)
- Właściwość LastName musi mieć długość większą od zera (i nie może składać się z samych spacji)
- Jeśli właściwość telefonu ma wartość (ma długość większą niż 0), właściwość telefon musi być zgodna z wyrażeniem regularnym.
- Jeśli właściwość wiadomości E-mail ma wartość (ma długość większą niż 0), Właściwość wiadomości E-mail musi być zgodna z wyrażeniem regularnym.

W przypadku naruszenia reguły walidacji komunikat o błędzie zostanie dodany do ModelState za pomocą metody AddModelError (). Po dodaniu komunikatu do ModelState należy podać nazwę właściwości i tekst komunikatu o błędzie walidacji. Ten komunikat o błędzie jest wyświetlany w widoku przez metody pomocnika html. podsumowania walidacji () i HTML. ValidationMessage ().

Po wykonaniu reguł walidacji jest sprawdzana Właściwość IsValid elementu ModelState. Właściwość IsValid zwraca wartość false, jeśli wszystkie komunikaty o błędach walidacji zostały dodane do ModelState. Jeśli walidacja nie powiedzie się, formularz Utwórz zostanie wyświetlony ponownie przy użyciu komunikatów o błędach.

> [!NOTE] 
> 
> Otrzymałem wyrażenia regularne do sprawdzania poprawności numeru telefonu i adresu e-mail z repozytorium wyrażeń regularnych w [ *http://regexlib.com* ](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Dodawanie logiki walidacji do akcji Edytuj

Akcja Edytuj () aktualizuje kontakt. Akcja Edit () wymaga wykonania dokładnie tej samej walidacji co akcja Utwórz (). Zamiast duplikowania tego samego kodu sprawdzania poprawności należy ponownie sprawdzić kontroler kontaktu, aby akcje create () i Edit () wywoływały tę samą metodę walidacji.

Zmodyfikowana Klasa kontrolera kontaktów jest zawarta w liście 3. Ta klasa ma nową metodę ValidateContact (), która jest wywoływana w ramach akcji Create () i Edit ().

**Lista 3 — Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Podsumowanie

W tej iteracji dodaliśmy podstawowe sprawdzanie poprawności formularza do naszej aplikacji menedżera kontaktów. Nasza logika walidacji uniemożliwia użytkownikom przesyłanie nowych kontaktów lub edytowanie istniejącej osoby kontaktowej bez podawania wartości właściwości FirstName i LastName. Ponadto użytkownicy muszą podać prawidłowe numery telefonów i adresy e-mail.

W tej iteracji dodaliśmy logikę walidacji do naszej aplikacji z Menedżerem kontaktów w najprostszym możliwym zakresie. Jednak mieszanie naszej logiki walidacji z naszą logiką kontrolera spowoduje utworzenie w długim czasie problemów dla Stanów Zjednoczonych. Nasza aplikacja będzie trudniejsza do utrzymania i modyfikacji w czasie.

W następnej iteracji będziemy refaktoryzacji logika weryfikacji dostępu do bazy danych z naszych kontrolerów. Będziemy korzystać z kilku zasad projektowania oprogramowania w celu umożliwienia nam tworzenia bardziej luźno powiązanych i łatwiejszego w obsłudze aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](iteration-2-make-the-application-look-nice-vb.md)
> [dalej](iteration-4-make-the-application-loosely-coupled-vb.md)

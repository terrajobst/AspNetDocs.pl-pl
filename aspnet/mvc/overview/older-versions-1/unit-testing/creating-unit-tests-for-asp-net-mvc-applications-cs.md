---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Tworzenie testów jednostkowych dla aplikacji ASP.NET MVCC#() | Microsoft Docs
author: StephenWalther
description: Dowiedz się, jak tworzyć testy jednostkowe dla akcji kontrolera. W tym samouczku Stephen Walther demonstruje, w jaki sposób sprawdzić, czy akcja kontrolera zwraca element Parti...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 35fd0d85c63e5bd196394ce11b851c822a6405d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623943"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

[Pobierz plik PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> Dowiedz się, jak tworzyć testy jednostkowe dla akcji kontrolera. W tym samouczku Stephen Walther demonstruje, jak sprawdzić, czy akcja kontrolera zwraca określony widok, zwraca określony zestaw danych lub zwraca inny typ wyniku działania.

Celem tego samouczka jest przedstawienie, jak można napisać testy jednostkowe dla kontrolerów w aplikacjach ASP.NET MVC. Omawiamy, jak utworzyć trzy różne typy testów jednostkowych. Dowiesz się, jak przetestować widok zwracany przez akcję kontrolera, jak przetestować dane widoku zwrócone przez akcję kontrolera i jak sprawdzić, czy jedna akcja kontrolera przekierowuje użytkownika do drugiej akcji kontrolera.

## <a name="creating-the-controller-under-test"></a>Tworzenie testowanego kontrolera

Zacznijmy od utworzenia kontrolera, który ma zostać przetestowany. Kontroler o nazwie `ProductController`znajduje się na liście 1.

**Lista 1 — `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

`ProductController` zawiera dwie metody akcji o nazwach `Index()` i `Details()`. Obie metody akcji zwracają widok. Zwróć uwagę, że akcja `Details()` akceptuje parametr o nazwie ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Testowanie widoku zwróconego przez kontroler

Załóżmy, że chcemy sprawdzić, czy `ProductController` zwraca odpowiedni widok. Chcemy upewnić się, że po wywołaniu akcji `ProductController.Details()` zostanie zwrócony widok szczegółów. Klasa testowa na liście 2 zawiera test jednostkowy do testowania widoku zwróconego przez akcję `ProductController.Details()`.

**Lista 2 — `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

Klasa na liście 2 zawiera metodę testową o nazwie `TestDetailsView()`. Ta metoda zawiera trzy wiersze kodu. Pierwszy wiersz kodu tworzy nowe wystąpienie klasy `ProductController`. Drugi wiersz kodu wywołuje metodę akcji `Details()` kontrolera. Na koniec ostatni wiersz kodu sprawdza, czy widok zwrócony przez akcję `Details()` jest widokiem szczegółowym.

Właściwość `ViewResult.ViewName` reprezentuje nazwę widoku zwróconego przez kontroler. Jedno duże ostrzeżenie dotyczące testowania tej właściwości. Istnieją dwa sposoby, aby kontroler mógł zwrócić widok. Kontroler może jawnie zwrócić widok podobny do tego:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

Alternatywnie można wywnioskować nazwę widoku na podstawie nazwy akcji kontrolera podobnej do:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

Ta akcja kontrolera zwraca również widok o nazwie `Details`. Jednak nazwa widoku jest wywnioskowana na podstawie nazwy akcji. Jeśli chcesz przetestować nazwę widoku, należy jawnie zwrócić nazwę widoku z akcji kontrolera.

Test jednostkowy można uruchomić na liście 2, wprowadzając kombinację **klawiszy Ctrl-R, A** lub klikając przycisk **Uruchom wszystkie testy w rozwiązaniu** (patrz rysunek 1). Jeśli test zakończy się powodzeniem, zobaczysz okno Wyniki testów na rysunku 2.

[![uruchomić wszystkie testy w rozwiązaniu](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**Ilustracja 01**. uruchamianie wszystkich testów w rozwiązaniu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))

[![powodzenie!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**Ilustracja 02**: sukces! ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))

## <a name="testing-the-view-data-returned-by-a-controller"></a>Testowanie danych widoku zwróconych przez kontroler

Kontroler MVC przekazuje dane do widoku przy użyciu czegoś o nazwie *`View Data`* . Załóżmy na przykład, że chcesz wyświetlić szczegóły dotyczące określonego produktu po wywołaniu akcji `ProductController Details()`. W takim przypadku można utworzyć wystąpienie klasy `Product` (zdefiniowane w modelu) i przekazać wystąpienie do widoku `Details`, wykorzystując zalety `View Data`.

Zmodyfikowane `ProductController` na liście 3 zawiera zaktualizowaną akcję `Details()`, która zwraca produkt.

**Lista 3 — `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

Najpierw akcja `Details()` tworzy nowe wystąpienie klasy `Product`, która reprezentuje komputer przenośny. Następnie wystąpienie klasy `Product` jest przesyłane jako drugi parametr do metody `View()`.

Można napisać testy jednostkowe, aby sprawdzić, czy oczekiwane dane są zawarte w danych widoku. Test jednostkowy na liście 4 testuje, czy produkt reprezentujący komputer przenośny jest zwracany po wywołaniu metody akcji `ProductController Details()`.

**Lista 4 — `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

W przypadku listy 4 Metoda `TestDetailsView()` testuje dane widoku zwrócone przez wywołanie metody `Details()`. `ViewData` jest uwidoczniona jako właściwość na `ViewResult` zwrócone przez wywołanie metody `Details()`. Właściwość `ViewData.Model` zawiera produkt przesłany do widoku. Test po prostu weryfikuje, czy produkt zawarty w danych widoku ma nazwę laptop.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testowanie wyniku działania zwróconego przez kontroler

Bardziej złożona akcja kontrolera może zwracać różne typy wyników akcji w zależności od wartości parametrów przesyłanych do akcji kontrolera. Akcja kontrolera może zwracać różne typy wyników akcji, w tym `ViewResult`, `RedirectToRouteResult`lub `JsonResult`.

Na przykład zmodyfikowana akcja `Details()` na liście 5 zwraca widok `Details`, gdy do akcji zostanie przekazany prawidłowy identyfikator produktu. Jeśli przekażesz nieprawidłowy identyfikator produktu — identyfikator o wartości mniejszej niż 1, nastąpi przekierowanie do akcji `Index()`.

**Lista 5 — `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

Możesz przetestować zachowanie akcji `Details()` z testem jednostkowym na liście 6. Test jednostkowy na liście 6 sprawdza, czy nastąpiło przekierowanie do widoku `Index`, gdy identyfikator o wartości-1 jest przesyłany do metody `Details()`.

**Lista 6 — `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

Po wywołaniu metody `RedirectToAction()` w akcji kontrolera akcja kontrolera zwraca `RedirectToRouteResult`. Test sprawdza, czy `RedirectToRouteResult` przekieruje użytkownika do akcji kontrolera o nazwie `Index`.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób kompilowania testów jednostkowych dla akcji kontrolera MVC. Po pierwsze przedstawiono sposób sprawdzania, czy prawy Widok jest zwracany przez akcję kontrolera. Wiesz już, jak użyć właściwości `ViewResult.ViewName` do zweryfikowania nazwy widoku.

Następnie zbadamy, jak można testować zawartość `View Data`. Wiesz już, jak sprawdzić, czy odpowiedni produkt został zwrócony w `View Data` po wywołaniu akcji kontrolera.

Wreszcie omówione zostało, jak można sprawdzić, czy w akcji kontrolera są zwracane różne typy wyników akcji. Wiesz już, jak sprawdzić, czy kontroler zwraca `ViewResult` lub `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Dalej](creating-unit-tests-for-asp-net-mvc-applications-vb.md)

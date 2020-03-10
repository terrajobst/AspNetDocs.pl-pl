---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Część 4: Tworzenie listy produktów | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 4 omawia produkty z licytacją...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566984"
---
# <a name="part-4-listing-products"></a>Część 4. Tworzenie listy produktów

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 4 obejmuje listę produktów z kontrolką GridView.

## <a id="_Toc260221670"></a>Wyświetlanie listy produktów za pomocą kontrolki GridView

Zacznijmy od wdrożenia naszej strony ProductsList. aspx, "kliknij prawym przyciskiem myszy" w naszym rozwiązaniu i wybierając pozycję "Dodaj" i "nowy element".

![](tailspin-spyworks-part-4/_static/image1.jpg)

Wybierz pozycję "formularz sieci Web przy użyciu strony głównej" i wprowadź nazwę strony ProductsList. aspx ".

Kliknij pozycję "Dodaj".

![](tailspin-spyworks-part-4/_static/image2.jpg)

Następnie wybierz folder "Style", w którym została umieszczona strona site. Master, a następnie wybierz ją z okna "zawartość folderu".

![](tailspin-spyworks-part-4/_static/image3.jpg)

Kliknij przycisk OK, aby utworzyć stronę.

Baza danych jest wypełniana danymi produktu, jak pokazano poniżej.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Po utworzeniu strony będziemy ponownie używać źródła danych jednostki w celu uzyskania dostępu do danych produktu, ale w tym wystąpieniu musimy wybrać jednostki produktu i musimy ograniczyć liczbę zwracanych elementów do wybranych kategorii.

W tym celu poinformujemy EntityDataSource o tym, aby automatycznie generował klauzulę WHERE, i określimy WhereParameter.

Przywrócisz, że po utworzeniu elementów menu w naszym menu "kategoria produktów" automatycznie utworzysz link, dodając IDKategorii do kwerendy QueryString dla każdego łącza. Poinformujemy źródło danych jednostki, aby wyprowadził parametr WHERE z tego parametru QueryString.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Następnie skonfigurujemy kontrolkę ListView do wyświetlania listy produktów. Aby utworzyć optymalne środowisko zakupów, będziemy kompaktować kilka zwięzłych funkcji do każdego pojedynczego produktu wyświetlanego w naszym ListVew.

- Nazwa produktu będzie łączem do widoku szczegółowego produktu.
- Zostanie wyświetlona cena produktu.
- Zostanie wyświetlony obraz produktu, który zostanie dynamicznie wybrany z katalogu obrazów wykazu w naszej aplikacji.
- Zostanie dołączony link umożliwiający natychmiastowe dodanie określonego produktu do koszyka zakupów.

Oto znaczniki dla naszego wystąpienia formantu ListView.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Dynamicznie tworzysz kilka linków dla każdego wyświetlanego produktu.

Ponadto przed przetestem nowej strony musimy utworzyć strukturę katalogów dla obrazów wykazu produktów w następujący sposób.

![](tailspin-spyworks-part-4/_static/image1.png)

Gdy nasze obrazy produktu są dostępne, możemy przetestować naszą stronę z listą produktów.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Na stronie głównej witryny kliknij jeden z linków listy kategorii.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Teraz musimy zaimplementować stronę ProductDetails. aspx i funkcje AddToCart.

Użyj opcji plik —&gt;nowy, aby utworzyć nazwę strony ProductDetails. aspx przy użyciu strony wzorca witryny, tak jak wcześniej.

Będziemy ponownie używać kontrolki EntityDataSource do uzyskiwania dostępu do określonego rekordu produktu w bazie danych i użyjemy kontrolki FormView ASP.NET, aby wyświetlić dane produktu w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Nie martw się, jeśli formatowanie nie będzie wyglądało na nieco zabawne. Znacznik powyżej pozostawia miejsce w układzie wyświetlania dla kilku funkcji, które zostaną zaimplementowane później.

Koszyk będzie reprezentował bardziej złożoną logikę w naszej aplikacji. Aby rozpocząć, użyj pliku&gt;New, aby utworzyć stronę o nazwie MyShoppingCart. aspx.

Pamiętaj, że nie wybieramy nazwy ShoppingCart. aspx.

Nasza baza danych zawiera tabelę o nazwie "ShoppingCart". Wygenerowany Entity Data Model Klasa została utworzona dla każdej tabeli w bazie danych. W związku z tym Entity Data Model wygenerowała klasy jednostki o nazwie "ShoppingCart". Możemy edytować model, aby można było użyć tej nazwy dla implementacji koszyka zakupów lub przełożyć ją na nasze potrzeby, ale zamiast tego wybieramy nazwę, która pozwoli uniknąć konfliktu.

Warto również zauważyć, że będziemy tworzyć prosty koszyk i osadzać logikę koszyka zakupów z uwzględnieniem koszyka zakupów. Możemy również zdecydować się na wdrożenie naszego koszyka w całkowicie oddzielnej warstwie biznesowej.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-3.md)
> [dalej](tailspin-spyworks-part-5.md)

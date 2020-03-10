---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: Część 6. Używanie adnotacji danych na potrzeby walidacji modelu | Microsoft Docs
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 6 obejmuje używanie adnotacji danych dla modelu V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539278"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a>Część 6. Używanie adnotacji do danych na potrzeby walidacji modelu

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 6 obejmuje używanie adnotacji danych na potrzeby walidacji modelu.

Mamy ważny problem z naszymi formularzami tworzenia i edytowania: nie wykonuje żadnych weryfikacji. Możemy to zrobić w taki sposób, aby pozostawić wymagane pola jako puste lub wpisać litery w polu Cena, a pierwszy z nich zobaczymy z bazy danych.

Możemy łatwo dodać weryfikację do naszej aplikacji, dodając adnotacje danych do naszych klas modelu. Adnotacje danych pozwalają nam opisać reguły, które chcemy zastosować do naszych właściwości modelu, a ASP.NET MVC zajmie się ich wymuszeniem i wyświetleniem odpowiednich komunikatów dla naszych użytkowników.

## <a name="adding-validation-to-our-album-forms"></a>Dodawanie walidacji do naszych formularzy albumu

Będziemy używać następujących atrybutów adnotacji danych:

- **Wymagane** — wskazuje, że właściwość jest polem wymaganym
- **DisplayName** — definiuje tekst, który ma być używany w polach formularzy i komunikatów weryfikacji
- **StringLength** — definiuje maksymalną długość pola ciągu
- **Range** — zapewnia maksymalną i minimalną wartość pola liczbowego
- **Bind** — wyświetla listę pól do wykluczenia lub dołączenia podczas wiązania parametrów lub wartości formularza z właściwościami modelu
- **ScaffoldColumn** — umożliwia ukrywanie pól z formularzy edytora

*Uwaga: Aby uzyskać więcej informacji na temat weryfikacji modelu przy użyciu atrybutów adnotacji danych, zapoznaj się z dokumentacją MSDN w witrynie* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Otwórz klasę albumu i Dodaj następujące instrukcje *using* na początku.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Następnie zaktualizuj właściwości, aby dodać atrybuty wyświetlania i walidacji, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

W tym czasie zmieniono także gatunek i wykonawcę na właściwości wirtualne. Dzięki temu Entity Framework w razie potrzeby Ładuj z opóźnieniem.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Po dodaniu tych atrybutów do modelu albumu, nasz ekran Utwórz i edytuj natychmiast rozpocznie sprawdzanie poprawności pól i używa wybranych przez siebie nazw wyświetlanych (np. adres URL grafiki albumu zamiast AlbumArtUrl). Uruchom aplikację i przejdź do/StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

Następnie zostaną rozdzielone pewne reguły walidacji. Wprowadź wartość w polu cena 0 i pozostaw pole puste. Po kliknięciu przycisku Utwórz zostanie wyświetlony formularz z komunikatami o błędach walidacji pokazujący, które pola nie spełniały zdefiniowanej reguły sprawdzania poprawności.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Testowanie weryfikacji po stronie klienta

Sprawdzanie poprawności po stronie serwera jest bardzo ważne w perspektywie aplikacji, ponieważ użytkownicy mogą obejść weryfikację po stronie klienta. Jednak na stronie sieci Web są wdrażane tylko te, które implementują walidację po stronie serwera.

1. Użytkownik musi czekać na opublikowanie formularza, zweryfikowanie go na serwerze i wysłanie odpowiedzi do przeglądarki.
2. Użytkownik nie otrzymuje natychmiastowej opinii, gdy poprawi pole, aby przeszedł teraz reguły walidacji.
3. Przed użyciem przeglądarki użytkownika wykorzystamy zasoby serwera do przeprowadzenia logiki walidacji.

Na szczęście szablony szkieletu ASP.NET MVC 3 mają wbudowaną funkcję weryfikacji po stronie klienta, co nie wymaga żadnych dodatkowych czynności.

Wpisanie pojedynczej litery w polu title spełnia wymagania dotyczące walidacji, dlatego komunikat o weryfikacji zostanie natychmiast usunięty.

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-5.md)
> [dalej](mvc-music-store-part-7.md)

---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Weryfikowanie z użyciem warstwy usług (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Dowiedz się, jak przenieść swoją logikę weryfikacji poza akcji kontrolera, a także do oddzielnej usługi warstwy. W tym samouczku wyjaśniono, Autor: Stephen Walther jak możesz...'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 9b2a7e00b3c50a946ad0f2518880892f103a5c1b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59387116"
---
# <a name="validating-with-a-service-layer-c"></a>Walidacja z użyciem warstwy usług (C#)

przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Dowiedz się, jak przenieść swoją logikę weryfikacji poza akcji kontrolera, a także do oddzielnej usługi warstwy. W tym samouczku Walther Autor: Stephen wyjaśnia, jak możesz zachować sharp separacji zagadnień przez izolowanie swojej warstwy usług z warstwą kontrolera.


Celem tego samouczka jest opisują jedną z metod wykonywania weryfikacji w aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak przenieść swoją logikę weryfikacji poza kontrolerach i do oddzielnej usługi warstwy.

## <a name="separating-concerns"></a>Oddzielenie obaw

W przypadku tworzenia aplikacji ASP.NET MVC, nie należy umieszczać logikę bazy danych wewnątrz akcji kontrolera. Mieszanie logikę bazy danych i kontroler utrudnia aplikacji do obsługi wraz z upływem czasu. Zaleca się umieścić wszystkie logiki bazy danych w warstwie osobnym repozytorium.

Na przykład wyświetlanie listy 1 zawiera prostego repozytorium o nazwie ProductRepository. Repozytorium produktu zawiera wszystkie kod dostępu do danych dla aplikacji. Listę obejmuje również interfejs IProductRepository, który implementuje repozytorium produktu.

**Wyświetlanie listy 1 — Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Kontroler w ofercie 2 używa warstwy repozytorium w jego indeks() i Create() akcji. Zwróć uwagę, czy ten kontroler nie zawiera wszelka logika bazy danych. Tworzenie warstwy repozytorium pozwala zachować czyste rozdzielenie problemów. Kontrolery są odpowiedzialne za logiki kontroli przepływu aplikacji i repozytorium jest odpowiedzialny za logiką dostępu do danych.

**Wyświetlanie listy 2 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Tworzenie warstwy usług

Tak logiki kontroli przepływu w aplikacji należy w kontrolerze i logiką dostępu do danych, należy w repozytorium. W takiej sytuacji, gdzie należy umieścić logikę weryfikacji? Jedną z opcji jest umieszczenie logikę weryfikacji w *warstwy usług*.

Warstwy usług to dodatkowa warstwa w aplikacji ASP.NET MVC, która pośredniczy komunikacji między kontrolerem i warstwy repozytorium. Warstwy usługi zawiera logikę biznesową. W szczególności zawiera logikę weryfikacji.

Na przykład warstwy usług produktu w ofercie 3 ma metodę CreateProduct(). Metoda CreateProduct() wywołuje metodę ValidateProduct() do sprawdzania poprawności nowego produktu przed przekazaniem produktu do repozytorium produktu.

**Wyświetlanie listy 3 - Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Kontroler produktu została zaktualizowana w ofercie 4, aby używać warstwy usług zamiast warstwy repozytorium. Warstwa kontrolera rozmawia z warstwy usług. Warstwy usług rozmawia z warstwy repozytorium. Każda warstwa ma oddzielne odpowiedzialności.

**Wyświetlanie listy 4 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Należy zauważyć, że usługa produktu jest tworzona w Konstruktorze kontroler produktu. Po utworzeniu usługi produktów słownik stanów modelu jest przekazywany do usługi. Usługa produktu używa stan modelu do przekazywania komunikatów o błędach weryfikacji odsyłane do kontrolera.

## <a name="decoupling-the-service-layer"></a>Oddzielenie warstwy usług

Firma Microsoft nie powiodło się izolować kontrolera i warstwy usługi, w związku z jednego. Kontroler i warstwy usługi komunikują się za pośrednictwem stanu modelu. Innymi słowy warstwy usług zależny od określonej funkcji platformę ASP.NET MVC.

Chcemy wyizolować warstwy usług z naszych możliwie warstwy kontrolera. Teoretycznie możemy powinno być możliwe do warstwy usług za pomocą dowolnego typu aplikacji i nie tylko aplikację ASP.NET MVC. Na przykład w przyszłości może być chcemy tworzenie aplikacji WPF frontonu dla naszej aplikacji. Należy znaleźliśmy sposobem usunięcia zależności na platformie ASP.NET MVC stan modelu z naszych warstwy usług.

W ofercie 5 warstwy usług został zaktualizowany tak, aby nie używał już stanu modelu. Zamiast tego używa każdej klasy, która implementuje interfejs IValidationDictionary.

**Wyświetlanie listy 5 - Models\ProductService.cs (odłączone)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

Interfejs IValidationDictionary jest zdefiniowany w ofercie 6. Ten prosty interfejs zawiera jedną metodę i jedną właściwość.

**Wyświetlanie listy 6 - Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

Klasa w ofercie 7 o nazwie klasy ModelStateWrapper implementuje interfejs IValidationDictionary. Można utworzyć wystąpienie klasy ModelStateWrapper, przekazując słownik stanów modelu do konstruktora.

**Wyświetlanie listy 7 - Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Ponadto zaktualizowano kontroler w ofercie 8 używa ModelStateWrapper podczas tworzenia warstwy usług w jej konstruktora.

**Wyświetlanie listy 8 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Za pomocą IValidationDictionary interfejsu i klasa ModelStateWrapper pozwala nam całkowicie odizolować nasze warstwy usług z naszych warstwy kontrolera. Warstwy usług nie jest już zależny od stanu modelu. Można przekazać każdej klasy, która implementuje interfejs IValidationDictionary do warstwy usług. Na przykład aplikacja WPF może implementować interfejs IValidationDictionary klasa prostych kolekcji.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było w celu omówienia jedno z podejść do przeprowadzania weryfikacji w aplikacji ASP.NET MVC. W tym samouczku przedstawiono sposób przenieść całą logikę weryfikacji poza kontrolerach i do oddzielnej usługi warstwy. Przedstawiono również sposób izolowania swojej warstwy usług z warstwą kontrolera, tworząc klasę ModelStateWrapper.

> [!div class="step-by-step"]
> [Poprzednie](validating-with-the-idataerrorinfo-interface-cs.md)
> [dalej](validation-with-the-data-annotation-validators-cs.md)

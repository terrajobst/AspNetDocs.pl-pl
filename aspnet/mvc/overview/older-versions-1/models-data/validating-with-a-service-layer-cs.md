---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Sprawdzanie poprawności przy użyciu warstwy usługC#() | Microsoft Docs
author: StephenWalther
description: Dowiedz się, jak przenieść logikę walidacji z akcji kontrolera i do oddzielnej warstwy usług. W tym samouczku Stephen Walther wyjaśnia, jak to zrobić...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: da1a1c9cc79a452eb0d7597810e86f7bcf6cd179
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542862"
---
# <a name="validating-with-a-service-layer-c"></a>Walidacja z użyciem warstwy usług (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> Dowiedz się, jak przenieść logikę walidacji z akcji kontrolera i do oddzielnej warstwy usług. W tym samouczku Stephen Walther wyjaśnia, jak można zachować wyraziste rozdzielenie problemów przez odizolowanie warstwy usług od warstwy kontrolera.

Celem tego samouczka jest opisywanie jednej metody weryfikacji w aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak przenieść logikę walidacji z kontrolerów i do oddzielnej warstwy usług.

## <a name="separating-concerns"></a>Rozdzielenie problemów

Podczas kompilowania aplikacji ASP.NET MVC nie należy umieszczać logiki bazy danych wewnątrz akcji kontrolera. Mieszanie logiki bazy danych i kontrolera sprawia, że aplikacja jest trudniejsza do utrzymania w czasie. Zalecenie polega na umieszczeniu całej logiki bazy danych w osobnej warstwie repozytorium.

Na przykład lista 1 zawiera proste repozytorium o nazwie ProductRepository. Repozytorium produktu zawiera cały kod dostępu do danych dla aplikacji. Ta lista zawiera również interfejs IProductRepository, który implementuje repozytorium produktu.

**Lista 1--Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Kontroler na liście 2 używa warstwy repozytorium w ramach akcji index () i Create (). Należy zauważyć, że ten kontroler nie zawiera żadnych logiki bazy danych. Utworzenie warstwy repozytorium pozwala zachować czyste rozdzielenie problemów. Kontrolery są odpowiedzialni za logikę kontroli przepływu aplikacji i repozytorium są odpowiedzialne za logikę dostępu do danych.

**Lista 2 — Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Tworzenie warstwy usług

W związku z tym logika kontroli przepływu aplikacji należy do kontrolera, a logika dostępu do danych należy do repozytorium. W takim przypadku należy umieścić logikę walidacji? Jedną z opcji jest umieszczenie logiki walidacji w *warstwie usług*.

Warstwa usługi jest dodatkową warstwą w aplikacji ASP.NET MVC, która koryguje komunikację między kontrolerem a warstwą repozytorium. Warstwa usługi zawiera logikę biznesową. W szczególności zawiera logikę walidacji.

Na przykład warstwa usługi produktu na liście 3 ma metodę/produkt (). Metoda do produkcji () wywołuje metodę ValidateProduct (), aby zweryfikować nowy produkt przed przekazaniem produktu do repozytorium produktu.

**Lista 3 — Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Kontroler produktu został zaktualizowany na liście 4, aby użyć warstwy usługi zamiast warstwy repozytorium. Warstwa kontrolera jest wychodząca do warstwy usługi. Warstwa usługi jest wychodząca do warstwy repozytorium. Każda warstwa ma osobną odpowiedzialność.

**Lista 4 — Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Należy zauważyć, że usługa produktu jest tworzona w konstruktorze produktu. Po utworzeniu usługi produktu słownik stanu modelu jest przesyłany do usługi. Usługa produktu używa stanu modelu do przekazywania komunikatów o błędach walidacji z powrotem do kontrolera.

## <a name="decoupling-the-service-layer"></a>Oddzielenie warstwy usług

Nie powiodło się izolowanie warstw kontrolera i usług. Warstwy kontrolera i usługi komunikują się za pomocą stanu modelu. Innymi słowy, warstwa usługi ma zależność od określonej funkcji struktury MVC ASP.NET.

Chcemy odizolować warstwę usług od naszej warstwy kontrolera tak dużo, jak to możliwe. Teoretycznie powinno być możliwe użycie warstwy usług z dowolnym typem aplikacji, a nie tylko aplikacji ASP.NET MVC. Na przykład w przyszłości możemy chcieć utworzyć fronton platformy WPF dla naszej aplikacji. Powinienmy znaleźć sposób usunięcia zależności od stanu modelu MVC ASP.NET z naszej warstwy usług.

Na liście 5 warstwa usługi została zaktualizowana, aby nie korzystała już z stanu modelu. Zamiast tego używa klasy, która implementuje interfejs IValidationDictionary.

**Lista 5-Models\ProductService.cs (odłączony)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

Interfejs IValidationDictionary został zdefiniowany na liście 6. Ten prosty interfejs ma pojedynczą metodę i pojedynczą właściwość.

**Lista 6 — Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

Klasa na liście 7 o nazwie Klasa ModelStateWrapper implementuje interfejs IValidationDictionary. Można utworzyć wystąpienie klasy ModelStateWrapper przez przekazanie słownika stanu modelu do konstruktora.

**Lista 7 — Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Na koniec zaktualizowany kontroler na liście 8 używa ModelStateWrapper podczas tworzenia warstwy usług w jej konstruktorze.

**Lista 8-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Użycie interfejsu IValidationDictionary i klasy ModelStateWrapper umożliwia nam całkowite odizolowanie naszej warstwy usług od naszej warstwy kontrolera. Warstwa usługi nie jest już zależna od stanu modelu. Do warstwy usług można przekazać dowolną klasę, która implementuje interfejs IValidationDictionary. Na przykład aplikacja WPF może zaimplementować interfejs IValidationDictionary przy użyciu prostej klasy kolekcji.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było omawianie jednego podejścia do wykonywania walidacji w aplikacji ASP.NET MVC. W tym samouczku przedstawiono sposób przenoszenia wszystkich logiki walidacji z kontrolerów i do oddzielnej warstwy usług. Poznasz również sposób izolowania warstwy usług od warstwy kontrolera przez utworzenie klasy ModelStateWrapper.

> [!div class="step-by-step"]
> [Poprzednie](validating-with-the-idataerrorinfo-interface-cs.md)
> [dalej](validation-with-the-data-annotation-validators-cs.md)

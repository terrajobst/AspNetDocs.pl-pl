---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Wskazówki dotyczące zabezpieczeń dla ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Opisuje problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas udostępniania zestawu danych za pomocą protokołu OData dla ASP.NET Web API 2 w ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556498"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a>Wskazówki dotyczące zabezpieczeń dla usługi ASP.NET Web API 2 OData

według [Jan Wasson](https://github.com/MikeWasson)

W tym temacie opisano niektóre problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas udostępniania zestawu danych za pomocą protokołu OData dla ASP.NET Web API 2 w ASP.NET 4. x.

## <a name="edm-security"></a>Zabezpieczenia modelu EDM

Semantyka zapytań jest oparta na modelu Entity Data Model (EDM), a nie na źródłowych typach modeli. Można wykluczyć właściwość z modelu EDM i nie będzie ona widoczna dla zapytania. Załóżmy na przykład, że model zawiera typ pracownika z właściwością wynagrodzeń. Możesz chcieć wykluczyć tę właściwość z modelu EDM, aby ukryć ją od klientów.

Istnieją dwa sposoby wykluczenia właściwości z modelu EDM. Można ustawić atrybut **[IgnoreDataMember]** dla właściwości w klasie modelu:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Możesz również usunąć właściwość z modelu EDM programowo:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Zabezpieczenia zapytań

Złośliwy lub algorytmie klient może utworzyć zapytanie, które zajmuje bardzo dużo czasu na wykonanie. W najgorszym przypadku może to przerwać dostęp do usługi.

Atrybut **[Queryable]** jest filtrem akcji, który analizuje, weryfikuje i stosuje zapytanie. Filtr konwertuje opcje zapytania na wyrażenie LINQ. Gdy kontroler OData zwraca typ **IQueryable** , dostawca **IQueryable** LINQ konwertuje wyrażenie LINQ na zapytanie. W związku z tym wydajność jest zależna od dostawcy LINQ, który jest używany, a także dla określonych cech zestawu danych lub schematu bazy danych.

Aby uzyskać więcej informacji na temat korzystania z opcji zapytania OData w interfejsie Web API ASP.NET, zobacz [Obsługa opcji zapytania OData](supporting-odata-query-options.md).

Jeśli wiesz, że wszyscy klienci są zaufani (na przykład w środowisku przedsiębiorstwa) lub jeśli zestaw danych jest mały, wydajność zapytań może nie być problemem. W przeciwnym razie należy wziąć pod uwagę następujące zalecenia.

- Przetestuj swoją usługę przy użyciu różnych zapytań i Profiluj bazę danych.
- Włącz stronicowanie oparte na serwerze, aby uniknąć powrotu dużego zestawu danych w jednym zapytaniu. Aby uzyskać więcej informacji, zobacz [stronicowanie oparte na serwerze](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- Potrzebujesz $filter i $orderby? Niektóre aplikacje mogą zezwalać na stronicowanie klienta przy użyciu $top i $skip, ale wyłączyć inne opcje zapytania. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Rozważ ograniczenie $orderby do właściwości w indeksie klastrowanym. Sortowanie dużych danych bez indeksu klastrowanego jest powolne. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Maksymalna liczba węzłów: Właściwość **MaxNodeCount** na **[Queryable]** ustawia maksymalną liczbę węzłów dozwoloną w drzewie składni $Filter. Wartość domyślna to 100, ale można ustawić niższą wartość, ponieważ może być konieczne spowolnienie kompilowania dużej liczby węzłów. Jest to szczególnie istotne, jeśli używasz LINQ to Objects (tj., zapytania LINQ w kolekcji w pamięci, bez użycia pośredniego dostawcy LINQ). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Rozważ wyłączenie dowolnej funkcji () i wszystkich (), ponieważ mogą one być wolne. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Jeśli dowolne właściwości ciągu zawierają duże ciągi&#8212;na przykład, opis produktu lub wpis&#8212;w blogu rozważ wyłączenie funkcji ciągu. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Należy rozważyć niezezwalanie na filtrowanie właściwości nawigacji. Filtrowanie właściwości nawigacji może skutkować przyłączeniem, co może potrwać, w zależności od schematu bazy danych. Poniższy kod przedstawia moduł sprawdzania poprawności zapytań, który uniemożliwia filtrowanie właściwości nawigacji. Aby uzyskać więcej informacji na temat modułów sprawdzania poprawności zapytań, zobacz [Walidacja zapytań](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Rozważ ograniczenie $filter zapytań, pisząc moduł sprawdzania poprawności, który jest dostosowany do bazy danych. Rozważmy na przykład te dwa zapytania: 

  - Wszystkie filmy z aktorami, których nazwisko zaczyna się od "A".
  - Wszystkie filmy wydane w 1994.

    Jeśli nie są indeksowane przez aktorów, pierwsze zapytanie może wymagać aparatu bazy danych do skanowania całej listy filmów. Drugie zapytanie może być akceptowalne, przy założeniu, że filmy są indeksowane przez rok wydania.

    Poniższy kod przedstawia moduł sprawdzania poprawności, który umożliwia filtrowanie właściwości "ReleaseYear" i "title", ale nie innych właściwości.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Ogólnie rzecz biorąc, należy rozważyć, które funkcje $filter są potrzebne. Jeśli klienci nie potrzebują pełnego wyrazistości $filter, można ograniczyć dozwolone funkcje.

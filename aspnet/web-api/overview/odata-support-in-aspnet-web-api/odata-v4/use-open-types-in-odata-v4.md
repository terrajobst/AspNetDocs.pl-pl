---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Otwieranie typów w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET | Microsoft Docs
author: microsoft
description: W protokole OData v4 typem otwartym jest typ strukturalny, który zawiera właściwości dynamiczne, a także wszystkie właściwości, które są zadeklarowane w definicji typu. Otwórz...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622179"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Typy otwarte w protokole OData v4 z interfejsem API sieci Web ASP.NET

przez [firmę Microsoft](https://github.com/microsoft)

> W protokole OData v4 *typem otwartym* jest typ strukturalny, który zawiera właściwości dynamiczne, a także wszystkie właściwości, które są zadeklarowane w definicji typu. Typy otwarte umożliwiają dodawanie elastyczności do modeli danych. W tym samouczku pokazano, jak używać typów otwartych w usłudze ASP.NET Web API OData.
> 
> W tym samouczku założono, że wiesz już, jak utworzyć punkt końcowy OData w interfejsie API sieci Web ASP.NET. W przeciwnym razie Zacznij od odczytywania najpierw [Utwórz punkt końcowy OData v4](create-an-odata-v4-endpoint.md) .
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Internetowy interfejs API OData 5,3
> - OData 4

Najpierw niektóre terminologia OData:

- Typ jednostki: typ strukturalny z kluczem.
- Typ złożony: typ strukturalny bez klucza.
- Typ otwarcia: typ z właściwościami dynamicznymi. Można otworzyć zarówno typy jednostek, jak i typy złożone.

Wartość właściwości dynamicznej może być typu pierwotnego, typu złożonego lub typu wyliczeniowego; lub kolekcja dowolnego z tych typów. Aby uzyskać więcej informacji na temat typów otwartych, zobacz [specyfikację OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instalowanie bibliotek usługi Web OData

Zainstaluj najnowsze biblioteki OData interfejsu API sieci Web za pomocą Menedżera pakietów NuGet. W oknie Konsola Menedżera pakietów:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definiowanie typów CLR

Zacznij od zdefiniowania modeli modelu EDM jako typów CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Po utworzeniu Entity Data Model (EDM)

- `Category` jest typem wyliczenia.
- `Address` jest typem złożonym. (Nie ma klucza, dlatego nie jest typem jednostki).
- `Customer` to typ jednostki. (Ma klucz).
- `Press` jest otwartym typem złożonym.
- `Book` to otwarty typ jednostki.

Aby utworzyć typ otwarty, typ CLR musi mieć właściwość typu `IDictionary<string, object>`, która zawiera właściwości dynamiczne.

## <a name="build-the-edm-model"></a>Kompiluj model modelu EDM

Jeśli używasz **ODataConventionModelBuilder** do tworzenia modelu EDM, `Press` i `Book` są automatycznie dodawane jako typy otwarte, na podstawie obecności właściwości `IDictionary<string, object>`.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Możesz również jawnie skompilować modelu EDM przy użyciu **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Dodawanie kontrolera OData

Następnie Dodaj kontroler OData. Na potrzeby tego samouczka użyjesz uproszczonego kontrolera, który obsługuje tylko żądania GET i POST, i używa listy znajdującej się w pamięci do przechowywania jednostek.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Zwróć uwagę, że pierwsze wystąpienie `Book` nie ma właściwości dynamicznych. Drugie wystąpienie `Book` ma następujące właściwości dynamiczne:

- "Opublikowany": typ pierwotny
- "Authors": Kolekcja typów pierwotnych
- "OtherCategories": Kolekcja typów wyliczeniowych.

Ponadto Właściwość `Press` tego wystąpienia `Book` ma następujące właściwości dynamiczne:

- "Blog": typ pierwotny
- "Address": typ złożony

## <a name="query-the-metadata"></a>Zapytanie dotyczące metadanych

Aby uzyskać dokument metadanych OData, Wyślij żądanie GET do `~/$metadata`. Treść odpowiedzi powinna wyglądać podobnie do tego:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

W dokumencie metadanych można zobaczyć, że:

- Dla typów `Book` i `Press` wartość atrybutu `OpenType` jest true. Typy `Customer` i `Address` nie mają tego atrybutu.
- Typ jednostki `Book` ma trzy zadeklarowane właściwości: ISBN, title i Press. Metadane OData nie zawierają właściwości `Book.Properties` z klasy CLR.
- Podobnie typ złożony `Press` ma tylko dwie zadeklarowane właściwości: Name i Category. Metadane nie zawierają właściwości `Press.DynamicProperties` z klasy CLR.

## <a name="query-an-entity"></a>Zapytanie o jednostkę

Aby uzyskać książkę z ISBN równą "978-0-7356-7942-9", Wyślij żądanie GET do `~/Books('978-0-7356-7942-9')`. Treść odpowiedzi powinna wyglądać podobnie do poniższego. (Z wcięciem, aby zwiększyć czytelność).

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Zauważ, że właściwości dynamiczne są zawarte w tekście z zadeklarowanymi właściwościami.

## <a name="post-an-entity"></a>Opublikuj jednostkę

Aby dodać jednostkę książki, Wyślij żądanie POST do `~/Books`. Klient może ustawić właściwości dynamiczne w ładunku żądania.

Oto przykładowe żądanie. Zanotuj właściwości "price" i "Opublikowano".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

W przypadku ustawienia punktu przerwania w metodzie kontrolera można zobaczyć, że interfejs API sieci Web dodał te właściwości do słownika `Properties`.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Dodatkowe materiały

[Przykład typu Open Type protokołu OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)

---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Dziedziczenie typu złożonego w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET | Microsoft Docs
author: microsoft
description: Zgodnie ze specyfikacją OData v4, typ złożony może dziedziczyć z innego typu złożonego. (Typ złożony jest typem strukturalnym bez klucza). Internetowy interfejs API...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556309"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Dziedziczenie typu złożonego w protokole OData v4 przy użyciu interfejsu API sieci Web ASP.NET

przez [firmę Microsoft](https://github.com/microsoft)

> Zgodnie ze [specyfikacją](http://www.odata.org/documentation/odata-version-4-0/)OData v4, typ złożony może dziedziczyć z innego typu złożonego. (Typ *złożony* jest typem strukturalnym bez klucza). Internetowy interfejs API OData 5,3 obsługuje dziedziczenie typu złożonego.
> 
> W tym temacie przedstawiono sposób tworzenia modelu Entity Data Model (EDM) przy użyciu złożonych typów dziedziczenia. Aby uzyskać pełny kod źródłowy, zobacz [przykładowy dziedziczenie typu złożonego OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Internetowy interfejs API OData 5,3
> - OData 4

## <a name="model-hierarchy"></a>Hierarchia modelu

Aby zilustrować dziedziczenie typu złożonego, użyjemy następującej hierarchii klas.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` jest abstrakcyjnym typem złożonym. `Rectangle`, `Triangle`i `Circle` są typami złożonymi pochodnymi `Shape`, a `RoundRectangle` pochodzą z `Rectangle`. `Window` jest typem jednostki i zawiera wystąpienie `Shape`.

Oto klasy CLR, które definiują te typy.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Kompiluj model modelu EDM

Aby utworzyć modelu EDM, można użyć **ODataConventionModelBuilder**, który wnioskuje relacje dziedziczenia z typów CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Możesz również jawnie skompilować modelu EDM przy użyciu **ODataModelBuilder**. Zajmuje to więcej kodu, ale daje większą kontrolę nad modelu EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Te dwa przykłady tworzą ten sam schemat modelu EDM.

## <a name="metadata-document"></a>Dokument metadanych

Oto dokument metadanych OData przedstawiający dziedziczenie typu złożonego.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

W dokumencie metadanych można zobaczyć, że:

- Typ złożony `Shape` jest abstrakcyjny.
- `Rectangle`, `Triangle`i `Circle` typ złożony mają `Shape`typ podstawowy.
- Typ `RoundRectangle` ma `Rectangle`typu podstawowego.

## <a name="casting-complex-types"></a>Rzutowanie typów złożonych

Rzutowanie na typy złożone jest teraz obsługiwane. Na przykład następujące zapytanie rzutuje `Shape` na `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Oto ładunek odpowiedzi:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]

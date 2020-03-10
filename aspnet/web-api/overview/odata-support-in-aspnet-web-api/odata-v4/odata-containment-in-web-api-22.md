---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Zawieranie w protokole OData v4 przy użyciu interfejsu Web API 2,2 | Microsoft Docs
author: rick-anderson
description: Tradycyjnie można uzyskać dostęp do jednostki tylko wtedy, gdy została ona Zahermetyzowana wewnątrz zestawu jednostek. Jednak usługa OData v4 udostępnia dwie dodatkowe opcje, pojedyncze i con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525124"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a>Zawieranie w protokole OData v4 przy użyciu interfejsu Web API 2,2

według Jinfu Tan

> Tradycyjnie można uzyskać dostęp do jednostki tylko wtedy, gdy została ona Zahermetyzowana wewnątrz zestawu jednostek. Jednak usługa OData v4 oferuje dwie dodatkowe opcje, pojedyncze i zawierające, z których każdy obsługuje WebAPI 2,2.

W tym temacie pokazano, jak zdefiniować zawieranie w punkcie końcowym OData w WebApi 2,2. Aby uzyskać więcej informacji na temat zawierania, zobacz artykuł [zawieranie jest dostarczane z użyciem protokołu OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Aby utworzyć punkt końcowy usługi OData v4 w interfejsie API sieci Web, zobacz [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).

Najpierw utworzymy model domeny zawierania w usłudze OData przy użyciu tego modelu danych:

![Model danych](odata-containment-in-web-api-22/_static/image1.png)

Konto zawiera wiele PaymentInstruments (PI), ale nie definiujemy zestawu jednostek dla PI. Zamiast tego PIs można uzyskać dostęp tylko za pomocą konta.

Rozwiązanie używane w tym temacie można pobrać z [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>Definiowanie modelu danych

1. Zdefiniuj typy CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    Atrybut `Contained` jest używany dla właściwości nawigacji zawierania.
2. Generuj model modelu EDM na podstawie typów CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` będzie obsługiwał Kompilowanie modelu EDM, jeśli atrybut `Contained` zostanie dodany do odpowiedniej właściwości nawigacji. Jeśli właściwość jest typem kolekcji, zostanie również utworzona funkcja `GetCount(string NameContains)`.

    Wygenerowane metadane będą wyglądać następująco:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    Atrybut `ContainsTarget` wskazuje, że właściwość nawigacji jest zawiera.

## <a name="define-the-containing-entity-set-controller"></a>Zdefiniuj kontroler zawierający zestaw jednostek

Zawarte jednostki nie mają własnego kontrolera; Akcja jest zdefiniowana w kontrolerze zawierającym zestaw jednostek. W tym przykładzie istnieje AccountsController, ale nie PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Jeśli ścieżka OData ma 4 lub więcej segmentów, tylko Routing atrybutów działa tak, jak `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` na powyższym kontrolerze. W przeciwnym razie zarówno atrybut, jak i konwencjonalny Routing działa: na przykład, `GetPayInPIs(int key)` dopasowuje `GET ~/Accounts(1)/PayinPIs`.

*Dzięki Leo hu w odniesieniu do oryginalnej zawartości tego artykułu.*

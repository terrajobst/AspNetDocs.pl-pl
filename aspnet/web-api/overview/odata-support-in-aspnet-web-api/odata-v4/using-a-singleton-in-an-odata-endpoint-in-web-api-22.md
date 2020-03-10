---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Tworzenie pojedynczych w protokole OData v4 przy użyciu interfejsu Web API 2,2 | Microsoft Docs
author: rick-anderson
description: W tym temacie przedstawiono sposób definiowania pojedynczego elementu w punkcie końcowym OData w interfejsie Web API 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622088"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Tworzenie pojedynczego elementu w protokole OData v4 przy użyciu interfejsu Web API 2,2

Autor Zoe Luo

> Tradycyjnie można uzyskać dostęp do jednostki tylko wtedy, gdy została ona Zahermetyzowana wewnątrz zestawu jednostek. Jednak usługa OData v4 oferuje dwie dodatkowe opcje, pojedyncze i zawierające, z których każdy obsługuje WebAPI 2,2.

W tym artykule przedstawiono sposób definiowania pojedynczego elementu w punkcie końcowym OData w interfejsie Web API 2,2. Aby uzyskać informacje na temat tego, co to jest pojedynczy i jak można korzystać z niego, zobacz [Używanie pojedynczych do definiowania podmiotu specjalnego](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Aby utworzyć punkt końcowy usługi OData v4 w interfejsie API sieci Web, zobacz [Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md). 

Utworzymy singleton w projekcie interfejsu API sieci Web przy użyciu następującego modelu danych:

![model danych](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Pojedyncze nazwane `Umbrella` zostanie zdefiniowane na podstawie typu `Company`, a zestaw jednostek o nazwie `Employees` zostanie zdefiniowany w oparciu o typ `Employee`.

Rozwiązanie używane w tym samouczku można pobrać z [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Zdefiniowanie modelu danych

1. Zdefiniuj typy CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generuj model modelu EDM na podstawie typów CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    W tym miejscu `builder.Singleton<Company>("Umbrella")` nakazuje konstruktorowi modelu tworzenie pojedynczych nazwanych `Umbrella` w modelu EDM.

    Wygenerowane metadane będą wyglądać następująco:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Z metadanych możemy zobaczyć, że właściwość nawigacji `Company` w zestawie jednostek `Employees` jest powiązana z `Umbrella`pojedynczym. Powiązanie jest wykonywane automatycznie przez `ODataConventionModelBuilder`, ponieważ tylko `Umbrella` ma typ `Company`. Jeśli w modelu występuje jakakolwiek niejednoznaczność, można użyć `HasSingletonBinding`, aby jawnie powiązać właściwość nawigacji z pojedynczą; `HasSingletonBinding` ma ten sam efekt, co użycie atrybutu `Singleton` w definicji typu CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Zdefiniuj kontroler singleton

Podobnie jak kontroler obiektów EntitySet, kontroler singleton dziedziczy po `ODataController`, a nazwa kontrolera singleton powinna być `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

W celu obsługi różnych rodzajów żądań akcje muszą być wstępnie zdefiniowane w kontrolerze. **Routing atrybutu** jest domyślnie włączony w WebAPI 2,2. Na przykład, aby zdefiniować akcję do obsługi zapytań `Revenue` z `Company` przy użyciu routingu atrybutów, należy wykonać następujące czynności:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Jeśli nie chcesz definiować atrybutów dla każdej akcji, po prostu Zdefiniuj swoje działania zgodnie z [konwencjami routingu OData](../odata-routing-conventions.md). Ponieważ klucz nie jest wymagany do wykonywania zapytań dotyczących pojedynczych, akcje zdefiniowane w kontrolerze pojedynczym różnią się nieco od akcji zdefiniowanych w kontrolerze obiektu EntitySet.

W przypadku odwołania, sygnatury metod dla każdej definicji akcji w kontrolerze singleton są wymienione poniżej.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Zasadniczo należy to zrobić po stronie usługi. [Przykładowy projekt](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) zawiera cały kod rozwiązania i klienta OData, który pokazuje, jak używać pojedynczej. Klient został utworzony przez wykonanie kroków opisanych w temacie [Tworzenie aplikacji klienckiej OData v4](create-an-odata-v4-client-app.md).

. 

*Dzięki Leo hu w odniesieniu do oryginalnej zawartości tego artykułu.*

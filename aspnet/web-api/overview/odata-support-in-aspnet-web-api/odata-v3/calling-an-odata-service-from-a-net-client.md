---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Wywoływanie usługi OData z klienta platformy .NET (C#) | Microsoft Docs
author: MikeWasson
description: W tym samouczku pokazano, jak wywołać usługę OData z C# aplikacji klienckiej. Wersje oprogramowania używane w samouczku Visual Studio 2013 (działa z Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614682"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a>Wywoływanie usługi protokołu OData z klienta .NET (C#)

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> W tym samouczku pokazano, jak wywołać usługę OData z C# aplikacji klienckiej.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (współdziała z programem Visual Studio 2012)
> - [Biblioteka klienta usług danych WCF](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (Przykładowa usługa OData jest tworzona przy użyciu interfejsu Web API 2, ale aplikacja kliencka nie jest zależna od interfejsu API sieci Web).

W tym samouczku omówiono tworzenie aplikacji klienckiej, która wywołuje usługę OData. Usługa OData udostępnia następujące jednostki:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

W poniższych artykułach opisano sposób implementacji usługi OData w interfejsie Web API. (Jednak nie musisz czytać ich, aby poznać ten samouczek).

- [Tworzenie punktu końcowego OData w interfejsie Web API 2](creating-an-odata-endpoint.md)
- [Relacje jednostek OData w interfejsie Web API 2](working-with-entity-relations.md)
- [Akcje protokołu OData w interfejsie Web API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generuj serwer proxy usługi

Pierwszym krokiem jest wygenerowanie serwera proxy usługi. Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData. Serwer proxy tłumaczy wywołania metod na żądania HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Zacznij od otwarcia projektu usługi OData w programie Visual Studio. Naciśnij kombinację klawiszy CTRL + F5, aby uruchomić usługę lokalnie w IIS Express. Zanotuj adres lokalny, włącznie z numerem portu przypisywanym przez program Visual Studio. Ten adres będzie potrzebny podczas tworzenia serwera proxy.

Następnie otwórz inne wystąpienie programu Visual Studio i Utwórz projekt aplikacji konsolowej. Aplikacja konsolowa będzie naszą aplikacją kliencką OData. (Można również dodać projekt do tego samego rozwiązania, co usługa).

> [!NOTE]
> Pozostałe kroki odwołują się do projektu konsoli.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy pozycję **odwołania** i wybierz pozycję **Dodaj odwołanie do usługi**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

W oknie dialogowym **Dodaj odwołanie do usługi** wpisz adres usługi OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

gdzie *port* jest numerem portu.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

W **obszarze Przestrzeń nazw**wpisz "ProductService". Ta opcja definiuje przestrzeń nazw klasy proxy.

Kliknij pozycję **Przejdź**. Program Visual Studio odczytuje dokument metadanych OData w celu odnalezienia jednostek w usłudze.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Kliknij przycisk **OK** , aby dodać klasę proxy do projektu.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Utwórz wystąpienie klasy serwera proxy usługi

Wewnątrz metody `Main` Utwórz nowe wystąpienie klasy proxy w następujący sposób:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Ponownie Użyj rzeczywistego numeru portu, na którym działa usługa. Po wdrożeniu usługi będziesz używać identyfikatora URI usługi Live. Nie musisz aktualizować serwera proxy.

Poniższy kod dodaje procedurę obsługi zdarzeń, która drukuje identyfikatory URI żądania do okna konsoli. Ten krok nie jest wymagany, ale jest interesujący do wyświetlania identyfikatorów URI dla każdego zapytania.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Wysyłanie zapytań do usługi

Poniższy kod pobiera listę produktów z usługi OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Zwróć uwagę, że nie musisz pisać żadnego kodu w celu wysłania żądania HTTP lub przeanalizowania odpowiedzi. Klasa proxy robi to automatycznie podczas wyliczania kolekcji `Container.Products` w pętli **foreach** .

Po uruchomieniu aplikacji dane wyjściowe powinny wyglądać następująco:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Aby uzyskać jednostkę według identyfikatora, należy użyć klauzuli `where`.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

W pozostałej części tego tematu nie będzie wyświetlana cała funkcja `Main`, tylko kod wymagany do wywołania usługi.

## <a name="apply-query-options"></a>Zastosuj opcje zapytania

Funkcja OData definiuje [Opcje zapytania](../supporting-odata-query-options.md) , których można użyć do filtrowania, sortowania, stronicowania danych i tak dalej. W przypadku serwera proxy usługi można zastosować te opcje przy użyciu różnych wyrażeń LINQ.

W tej sekcji pokażę krótkie przykłady. Aby uzyskać więcej informacji, zobacz temat [uwagi dotyczące LINQ (usługi danych programu WCF)](https://msdn.microsoft.com/library/ee622463.aspx) w witrynie MSDN.

### <a name="filtering-filter"></a>Filtrowanie ($filter)

Aby przefiltrować, użyj klauzuli `where`. Poniższe przykładowe filtry według kategorii produktów.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Ten kod odnosi się do poniższego zapytania OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Zwróć uwagę, że serwer proxy konwertuje klauzulę `where` na wyrażenie `$filter` OData.

### <a name="sorting-orderby"></a>Sortowanie ($orderby)

Aby posortować, użyj klauzuli `orderby`. Poniższy przykład sortuje według ceny, od najwyższego do najniższego.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Oto odpowiednie żądanie OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Stronicowanie po stronie klienta ($skip i $top)

W przypadku dużych zestawów jednostek klient może chcieć ograniczyć liczbę wyników. Na przykład klient może wyświetlić 10 wpisów jednocześnie. Jest to nazywane *stronicowaniem po stronie klienta*. (Istnieje również [stronicowanie po stronie serwera](../supporting-odata-query-options.md#server-paging), gdzie serwer ogranicza liczbę wyników). Aby wykonać stronicowanie po stronie klienta, użyj metody LINQ **Skip** i **Take** . Poniższy przykład pomija pierwsze 40 wyników i pobiera następne 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Oto odpowiednie żądanie OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Wybierz ($select) i rozwiń ($expand)

Aby dołączyć powiązane jednostki, użyj metody `DataServiceQuery<t>.Expand`. Na przykład, aby uwzględnić `Supplier` dla każdej `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Oto odpowiednie żądanie OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Aby zmienić kształt odpowiedzi, użyj klauzuli LINQ **SELECT** . Poniższy przykład pobiera tylko nazwę każdego produktu bez innych właściwości.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Oto odpowiednie żądanie OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Klauzula SELECT może obejmować powiązane jednostki. W takim przypadku nie wywołuj **rozwinięcia**; serwer proxy automatycznie uwzględnia rozszerzenie w tym przypadku. Poniższy przykład pobiera nazwę i dostawcę każdego produktu.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Oto odpowiednie żądanie OData. Należy zauważyć, że zawiera on opcję **$expand** .

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Aby uzyskać więcej informacji na temat $select i $expand, zobacz [używanie $SELECT, $expand i $Value w interfejsie Web API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Dodaj nową jednostkę

Aby dodać nową jednostkę do zestawu jednostek, wywołaj `AddToEntitySet`, gdzie *EntitySet* jest nazwą zestawu jednostek. Na przykład `AddToProducts` dodaje nowy `Product` do zestawu jednostek `Products`. Podczas generowania serwera proxy Usługi danych programu WCF automatycznie tworzy te metody **AddTo** z jednoznacznie określonymi typami.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Aby dodać łącze między dwiema jednostkami, użyj metod **AddLink** i **SetLink** . Poniższy kod dodaje nowego dostawcę i nowy produkt, a następnie tworzy linki między nimi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Użyj **AddLink** , gdy właściwość nawigacji jest kolekcją. W tym przykładzie dodajemy produkt do kolekcji `Products` na dostawcy.

Użyj **SetLink** , gdy właściwość nawigacji jest pojedynczą jednostką. W tym przykładzie ustawiamy Właściwość `Supplier` produktu.

## <a name="update--patch"></a>Aktualizacja/poprawka

Aby zaktualizować jednostkę, wywołaj metodę **UpdateObject** .

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Aktualizacja jest wykonywana po wywołaniu **metody SaveChanges**. Domyślnie WCF wysyła żądanie scalania HTTP. Opcja **PatchOnUpdate** informuje program WCF, aby zamiast tego wysłać poprawkę http.

> [!NOTE]
> Dlaczego warto zastosować poprawki i scalić? Oryginalna specyfikacja protokołu HTTP 1,1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nie definiuje żadnej metody http z semantyką "częściowej aktualizacji". W celu obsługi aktualizacji częściowych Specyfikacja OData zdefiniowana przez metodę MERGE. W 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) zdefiniowano metodę patch dla aktualizacji częściowych. Część historii w tym [wpisie w blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) znajdziesz w blogu usługi danych programu WCF. Obecnie poprawka jest preferowana przez SCALAnie. Kontroler OData utworzony przez szkielet interfejsu API sieci Web obsługuje obie metody.

Jeśli chcesz zastąpić całą jednostkę (PUT semantyki), określ opcję **ReplaceOnUpdate** . Powoduje to, że usługa WCF wysyła żądanie HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Usuwanie jednostki

Aby usunąć jednostkę, wywołaj metodę **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Wywołaj akcję OData

W protokole OData [Akcje](odata-actions.md) są sposobem dodawania zachowań po stronie serwera, które nie są łatwo zdefiniowane jako operacje CRUD na jednostkach.

Mimo że dokument metadanych OData zawiera opis akcji, Klasa proxy nie tworzy żadnych metod z jednoznacznie określonymi typami. Nadal można wywołać akcję OData przy użyciu metody **wykonywania** generycznego. Należy jednak znać typy danych parametrów i wartość zwracaną.

Na przykład akcja `RateProduct` przyjmuje parametr o nazwie "Rating" typu `Int32` i zwraca `double`. Poniższy kod przedstawia sposób wywołania tej akcji.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Aby uzyskać więcej informacji, zobacz[wywoływanie operacji usługi i akcji](https://msdn.microsoft.com/library/hh230677.aspx).

Jedną z opcji jest zwiększenie klasy **kontenera** w celu zapewnienia metody silnie wpisanej, która wywołuje akcję:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]

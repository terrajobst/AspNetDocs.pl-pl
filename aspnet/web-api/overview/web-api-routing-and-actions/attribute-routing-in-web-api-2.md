---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing atrybutów w programie ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554986"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Routing atrybutów w interfejsie Web API ASP.NET 2

według [Jan Wasson](https://github.com/MikeWasson)

*Routing* to sposób, w jaki interfejs API sieci Web dopasowuje identyfikator URI do akcji. Interfejs Web API 2 obsługuje nowy typ routingu nazywany *routingiem atrybutu*. Jak nazwa oznacza, funkcja routingu atrybutów używa atrybutów do definiowania tras. Routing atrybutu daje większą kontrolę nad identyfikatorami URI w interfejsie API sieci Web. Można na przykład łatwo tworzyć identyfikatory URI opisujące hierarchie zasobów.

Wcześniejszy styl routingu o nazwie routing oparty na Konwencji jest nadal w pełni obsługiwany. W rzeczywistości można połączyć obie techniki w tym samym projekcie.

W tym temacie przedstawiono sposób włączania routingu atrybutów i opisano różne opcje routingu atrybutów. Aby zapoznać się z kompleksowym samouczkiem korzystającym z routingu atrybutów, zobacz [Tworzenie interfejsu API REST z routingiem atrybutów w interfejsie Web API 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Wymagania wstępne

[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise Edition

Alternatywnie można zainstalować wymagane pakiety przy użyciu Menedżera pakietów NuGet. W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. Wprowadź następujące polecenie w oknie Konsola Menedżera pakietów:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Dlaczego atrybut routingu?

Pierwsza wersja interfejsu API sieci Web używa routingu *opartego na Konwencji* . W tym typie routingu należy zdefiniować jeden lub więcej szablonów tras, które są zasadniczo sparametryzowanemi ciągami. Gdy struktura odbiera żądanie, jest zgodna z identyfikatorem URI względem szablonu trasy. (Aby uzyskać więcej informacji na temat routingu opartego na Konwencji, zobacz [Routing in Web API ASP.NET](routing-in-aspnet-web-api.md).

Jedną z zalet routingu opartego na Konwencji jest to, że szablony są zdefiniowane w jednym miejscu, a reguły routingu są stosowane spójnie na wszystkich kontrolerach. Niestety, routing oparty na Konwencji utrudnia obsługę niektórych wzorców identyfikatorów URI, które są wspólne w interfejsach API RESTful. Na przykład zasoby często zawierają zasoby podrzędne: klienci mają zamówienia, filmy mają aktorów, książki mają autorów itd. W naturalny sposób można tworzyć identyfikatory URI odzwierciedlające te relacje:

`/customers/1/orders`

Ten typ identyfikatora URI jest trudny do utworzenia przy użyciu routingu opartego na Konwencji. Chociaż można to zrobić, wyniki nie będą skalowane prawidłowo, jeśli istnieje wiele kontrolerów lub typów zasobów.

Dzięki routingu atrybutów można zdefiniować trasę dla tego identyfikatora URI. Wystarczy dodać atrybut do akcji kontrolera:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Oto kilka innych wzorców, które ułatwiają Routing atrybutów.

**Obsługa wersji interfejsu API**

W tym przykładzie "/API/V1/Products" byłby kierowany do innego kontrolera niż "/API/v2/Products".

`/api/v1/products`
`/api/v2/products`

**Przeciążone segmenty URI**

W tym przykładzie "1" jest numerem zamówienia, ale "oczekujące" są mapowane do kolekcji.

`/orders/1`
`/orders/pending`

**Wiele typów parametrów**

W tym przykładzie "1" jest numerem zamówienia, ale "2013/06/16" określa datę.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Włączanie routingu atrybutów

Aby włączyć routing atrybutów, wywołaj **MapHttpAttributeRoutes** podczas konfiguracji. Ta metoda rozszerzenia jest zdefiniowana w klasie **System. Web. http. HttpConfigurationExtensions** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Routing atrybutów można łączyć z routingiem [opartym na Konwencji](routing-in-aspnet-web-api.md) . Aby zdefiniować trasy oparte na Konwencji, wywołaj metodę **MapHttpRoute** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Aby uzyskać więcej informacji na temat konfigurowania interfejsu API sieci Web, zobacz [konfigurowanie ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Uwaga: Migrowanie z interfejsu API sieci Web 1

W systemach wcześniejszych niż Web API 2 szablon projektu interfejsu API sieci Web został wygenerowany w następujący sposób:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Jeśli jest włączona funkcja routingu atrybutu, ten kod zgłosi wyjątek. Jeśli uaktualniasz istniejący projekt internetowego interfejsu API w celu używania routingu atrybutów, pamiętaj o zaktualizowaniu tego kodu konfiguracji do poniższego:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Aby uzyskać więcej informacji, zobacz [Konfigurowanie internetowego interfejsu API z hostingiem ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Dodawanie atrybutów trasy

Oto przykład trasy zdefiniowanej przy użyciu atrybutu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Ciąg &quot;Customers/{customerId}/Orders&quot; jest szablonem identyfikatora URI dla trasy. Interfejs API sieci Web próbuje dopasować identyfikator URI żądania do szablonu. W tym przykładzie "klienci" i "zamówienia" są segmentami literału, a "{customerId}" jest parametrem zmiennej. Następujące identyfikatory URI byłyby zgodne z tym szablonem:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Można ograniczyć Dopasowywanie przy użyciu [ograniczeń](#constraints), opisanych w dalszej części tego tematu.

Zwróć uwagę, że parametr &quot;{customerId}&quot; w szablonie trasy jest zgodny z nazwą parametru *customerId* w metodzie. Gdy interfejs API sieci Web wywołuje akcję kontrolera, próbuje powiązać parametry trasy. Na przykład jeśli identyfikator URI jest `http://example.com/customers/1/orders`, interfejs API sieci Web próbuje powiązać wartość "1" z parametrem *IDKlienta* w akcji.

Szablon identyfikatora URI może mieć kilka parametrów:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Wszystkie metody kontrolera, które nie mają atrybutu trasy, używają routingu opartego na Konwencji. W ten sposób można połączyć oba typy routingu w tym samym projekcie.

## <a name="http-methods"></a>Metody HTTP

Interfejs API sieci Web wybiera także akcje oparte na metodzie HTTP żądania (GET, POST itp.). Domyślnie interfejs API sieci Web szuka dopasowania bez uwzględniania wielkości liter na początku nazwy metody kontrolera. Na przykład Metoda kontrolera o nazwie `PutCustomers` dopasowuje żądanie HTTP PUT.

Tę Konwencję można zastąpić, dekorowania nazwy metodę z dowolnym z następujących atrybutów:

- **[HttpDelete]**
- **Narzędzia HttpGet**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **HTTPPOST**
- **[HttpPut]**

Poniższy przykład mapuje metodę "xmlbook" na żądanie HTTP POST.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Dla wszystkich innych metod HTTP, w tym niestandardowymi metodami, należy użyć atrybutu **AcceptVerbs** , który pobiera listę metod http.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Prefiksy tras

Często trasy na kontrolerze są uruchamiane z tym samym prefiksem. Na przykład:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Można ustawić wspólny prefiks dla całego kontrolera przy użyciu atrybutu **[RoutePrefix]** :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Użyj tyldy (~) w atrybucie metody, aby zastąpić prefiks trasy:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Prefiks trasy może zawierać parametry:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Ograniczenia trasy

Ograniczenia trasy umożliwiają ograniczenie sposobu dopasowywania parametrów w szablonie trasy. Ogólna składnia jest &quot;{Parameter: Constraint}&quot;. Na przykład:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

W tym miejscu Pierwsza trasa zostanie wybrana tylko wtedy, gdy identyfikator &quot;&quot; segment identyfikatora URI jest liczbą całkowitą. W przeciwnym razie zostanie wybrana druga trasa.

Poniższa tabela zawiera listę ograniczeń, które są obsługiwane.

| Typu | Opis | Przykład |
| --- | --- | --- |
| alfa | Dopasowuje wielkie lub małe litery alfabetu łacińskiego (a-z, A-Z) | {x:alpha} |
| bool | Dopasowuje wartość logiczną. | {x:bool} |
| datetime | Dopasowuje wartość **daty i godziny** . | {x:datetime} |
| decimal | Dopasowuje wartość dziesiętną. | {x:decimal} |
| double | Dopasowuje 64-bitową wartość zmiennoprzecinkową. | {x:double} |
| float | Dopasowuje 32-bitową wartość zmiennoprzecinkową. | {x:float} |
| guid | Dopasowuje wartość identyfikatora GUID. | {x:guid} |
| int | Dopasowuje 32-bitową wartość całkowitą. | {x:int} |
| {1&gt;length&lt;1} | Dopasowuje ciąg o określonej długości lub w określonym zakresie długości. | {x:length (6)} {x:length (1, 20)} |
| long | Dopasowuje 64-bitową wartość całkowitą. | {x:long} |
| maks. | Dopasowuje liczbę całkowitą z maksymalną wartością. | {x:max(10)} |
| MaxLength | Dopasowuje ciąg z maksymalną długością. | {x:maxlength(10)} |
| min. | Dopasowuje liczbę całkowitą z wartością minimalną. | {x:min(10)} |
| minLength | Dopasowuje ciąg o długości minimalnej. | {x:minlength(10)} |
| range | Dopasowuje liczbę całkowitą w zakresie wartości. | {x:Range (10, 50)} |
| regex | Dopasowuje wyrażenie regularne. | {x:Regex (^ \d{3}-\d{3}-\d{4}$)} |

Należy zauważyć, że niektóre ograniczenia, takie jak &quot;minimum&quot;, przyjmują argumenty w nawiasach. Do parametru można zastosować wiele ograniczeń rozdzielonych dwukropkiem.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Niestandardowe ograniczenia trasy

Można utworzyć niestandardowe ograniczenia trasy, implementując Interfejs **IHttpRouteConstraint** . Na przykład następujące ograniczenie ogranicza parametr do wartości innej niż zero Integer.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Poniższy kod przedstawia sposób zarejestrowania ograniczenia:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Teraz można zastosować ograniczenie w ramach tras:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Możesz również zastąpić całą klasę **DefaultInlineConstraintResolver** przez implementację interfejsu **IInlineConstraintResolver** . Wykonanie tej operacji spowoduje zastąpienie wszystkich wbudowanych ograniczeń, chyba że implementacja **IInlineConstraintResolver** w ich ramach nie dodaje.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Opcjonalne parametry URI i wartości domyślne

Można określić parametr identyfikatora URI jako opcjonalny, dodając znak zapytania do parametru trasy. Jeśli parametr trasy jest opcjonalny, należy zdefiniować wartość domyślną dla parametru metody.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

W tym przykładzie `/api/books/locale/1033` i `/api/books/locale` zwracać tego samego zasobu.

Alternatywnie możesz określić wartość domyślną w szablonie trasy w następujący sposób:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Jest to prawie tak samo jak w poprzednim przykładzie, ale podczas stosowania wartości domyślnej występuje niewielka różnica zachowania.

- W pierwszym przykładzie ("{LCID: int?}") wartość domyślna 1033 jest przypisywana bezpośrednio do parametru metody, dlatego parametr będzie miał dokładną wartość.
- W drugim przykładzie ("{LCID: int = 1033}") wartość domyślna "1033" przechodzi przez proces powiązania modelu. Model segregatora domyślnego spowoduje przekonwertowanie "1033" na wartość liczbową 1033. Można jednak podłączyć spinacz modelu niestandardowego, który może coś się różnić.

(W większości przypadków, jeśli nie masz niestandardowych powiązań modelu w potoku, te dwa formularze będą równoważne).

<a id="route-names"></a>
## <a name="route-names"></a>Nazwy tras

W interfejsie Web API Każda trasa ma nazwę. Nazwy tras są przydatne do generowania linków, dzięki czemu można dołączyć łącze w odpowiedzi HTTP.

Aby określić nazwę trasy, ustaw właściwość **Nazwa** atrybutu. Poniższy przykład pokazuje, jak ustawić nazwę trasy, a także użyć nazwy trasy podczas generowania linku.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Kolejność tras

Gdy struktura próbuje dopasować identyfikator URI przy użyciu trasy, szacuje trasy w określonej kolejności. Aby określić kolejność, ustaw właściwość **Order** dla atrybutu Route. Niższe wartości są oceniane jako pierwsze. Domyślna wartość zamówienia to zero.

Poniżej przedstawiono sposób określania sumy porządkowania:

1. Porównaj Właściwość **Order** atrybutu Route.
2. Sprawdź każdy segment identyfikatora URI w szablonie trasy. Dla każdego segmentu porządkuj w następujący sposób:

    1. Segmenty literału.
    2. Parametry trasy z ograniczeniami.
    3. Parametry trasy bez ograniczeń.
    4. Wieloznaczne segmenty parametrów z ograniczeniami.
    5. Wieloznaczne segmenty parametrów bez ograniczeń.
3. W przypadku równego powiązania trasy są uporządkowane według porównania ciągów porządkowych bez uwzględniania wielkości liter ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) szablonu trasy.

Oto przykład. Załóżmy, że definiujesz następujący kontroler:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Te trasy są uporządkowane w następujący sposób.

1. zamówienia/szczegóły
2. zamówienia/{ID}
3. zamówienia/{CustomerName}
4. Orders/{\*Date}
5. zamówienia/oczekujące

Zwróć uwagę, że "Details" jest segmentem literału i pojawia się przed "{ID}", ale "Pending" pojawia się jako Last, ponieważ właściwość **Order** ma wartość 1. (W tym przykładzie zakłada się, że nie ma żadnych klientów o nazwie "Details" lub "Pending". Ogólnie rzecz biorąc, spróbuj uniknąć niejednoznacznych tras. W tym przykładzie lepszym szablonem trasy dla `GetByCustomer` jest "Customers/{CustomerName}")

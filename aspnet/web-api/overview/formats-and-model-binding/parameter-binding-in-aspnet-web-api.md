---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Powiązanie parametrów w ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Opisuje sposób, w jaki interfejs API sieci Web wiąże parametry i jak dostosować proces powiązania w ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5386532ab581e023d93d16a5d4107e07f40b986f
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985827"
---
# <a name="parameter-binding-in-aspnet-web-api"></a>Powiązanie parametrów w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

[!INCLUDE[](~/includes/coreWebAPI.md)]

W tym artykule opisano, jak interfejs API sieci Web wiąże parametry i jak można dostosować proces powiązania. Gdy interfejs API sieci Web wywołuje metodę na kontrolerze, musi ustawić wartości parametrów, proces o nazwie *Binding*.

Domyślnie interfejs API sieci Web używa następujących reguł do wiązania parametrów:

- Jeśli parametr jest typu prostego, interfejs API sieci Web próbuje uzyskać wartość z identyfikatora URI. Typy proste obejmują [typy pierwotne](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) .NET (**int**, **bool**, **Double**itd.), a także wartości **TimeSpan**, **DateTime**, **GUID**, **Decimal**i **String** *oraz dowolnego typu* z typem konwerter, który można przekonwertować z ciągu. (Więcej informacji na temat konwerterów typów w dalszej części).
- W przypadku typów złożonych interfejs API sieci Web próbuje odczytać wartość z treści wiadomości przy użyciu programu [formatującego typu nośnika](media-formatters.md).

Oto przykład typowej metody kontrolera API sieci Web:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Parametr *ID* jest &quot;typu prostego&quot; , więc interfejs API sieci Web próbuje uzyskać wartość z identyfikatora URI żądania. Parametr *Item* jest typem złożonym, więc interfejs API sieci Web używa programu formatującego typu nośnika do odczytywania wartości z treści żądania.

Aby uzyskać wartość z identyfikatora URI, interfejs API sieci Web przegląda dane trasy i ciąg zapytania identyfikatora URI. Dane trasy są wypełniane, gdy system routingu analizuje identyfikator URI i dopasowuje go do trasy. Aby uzyskać więcej informacji, zobacz [Routing i wybór akcji](../web-api-routing-and-actions/routing-and-action-selection.md).

W pozostałej części artykułu pokażę, jak można dostosować proces powiązania modelu. W przypadku typów złożonych należy jednak wziąć pod uwagę używanie programu formatującego typu nośnika, gdy jest to możliwe. Kluczową zasadą protokołu HTTP jest to, że zasoby są wysyłane w treści wiadomości, przy użyciu negocjacji zawartości, aby określić reprezentację zasobu. Elementy formatujące typy multimedialne zostały zaprojektowane do tego celu dokładnie.

## <a name="using-fromuri"></a>Korzystanie z [FromUri]

Aby wymusić odczytywanie przez internetowy interfejs API typu złożonego z identyfikatora URI, Dodaj atrybut **[FromUri]** do parametru. W poniższym przykładzie zdefiniowano `GeoPoint` typ wraz z metodą kontrolera, która `GeoPoint` pobiera z identyfikatora URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Klient może umieścić wartości szerokości i długości geograficznej w ciągu zapytania oraz w interfejsie API sieci Web, które `GeoPoint`będą używane do konstruowania. Na przykład:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Korzystanie z [FromBody]

Aby wymusić odczytanie typu prostego z treści żądania przez internetowy interfejs API, Dodaj atrybut **[FromBody]** do parametru:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

W tym przykładzie interfejs API sieci Web użyje programu formatującego typ nośnika, aby odczytać wartość *nazwy* z treści żądania. Oto przykładowe żądanie klienta.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Gdy parametr ma [FromBody], interfejs API sieci Web używa nagłówka Content-Type do wybierania elementu formatującego. W tym przykładzie typ zawartości to &quot;Application/JSON&quot; , a treść żądania jest nieprzetworzonym ciągiem JSON (nie obiektem JSON).

Co najwyżej jeden parametr może zostać odczytany z treści komunikatu. W związku z tym nie będzie to zadziałało:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Przyczyną tej reguły jest fakt, że treść żądania może być przechowywana w niebuforowanym strumieniu, który można odczytać tylko raz.

## <a name="type-converters"></a>Konwertery typów

Można sprawić, aby interfejs API sieci Web traktował klasę jako typ prosty (dzięki czemu interfejs API sieci Web spróbuje powiązać ją z identyfikatorem URI), tworząc obiekt **TypeConverter** i dostarczając konwersję ciągów.

Poniższy kod przedstawia `GeoPoint` klasę, która reprezentuje punkt geograficzny, oraz obiekt **TypeConverter** , który konwertuje ciągi na `GeoPoint` wystąpienia. Klasa ma atrybut **[TypeConverter]** do określenia konwertera typów. `GeoPoint` (Ten przykład został inspirowany wpisem na blogu Jan, [jak powiązać z obiektami niestandardowymi w sygnaturach akcji w MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)).

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Teraz interfejs API sieci Web `GeoPoint` będzie traktowany jako typ prosty, co oznacza, że `GeoPoint` podejmie próbę powiązania parametrów z identyfikatora URI. Nie trzeba dołączać **[FromUri]** do parametru.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Klient może wywołać metodę z identyfikatorem URI podobnym do tego:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Powiązania modelu

Bardziej elastyczną opcją niż konwerter typu jest utworzenie spinacza modelu niestandardowego. Dzięki modelowi spinacza masz dostęp do elementów, takich jak żądanie HTTP, Opis akcji i wartości pierwotnych z danych trasy.

Aby utworzyć spinacz modelu, zaimplementuj Interfejs **IModelBinder** . Ten interfejs definiuje pojedynczą metodę, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Oto spinacz modelu dla `GeoPoint` obiektów.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Spinacz modelu pobiera pierwotne wartości wejściowe od *dostawcy wartości*. Ten projekt oddziela dwie odrębne funkcje:

- Dostawca wartości pobiera żądanie HTTP i wypełnia słownik par klucz-wartość.
- Model spinacza używa tego słownika do wypełnienia modelu.

Dostawca wartości domyślnej w interfejsie Web API pobiera wartości z danych trasy i ciągu zapytania. Na przykład jeśli identyfikator URI to `http://localhost/api/values/1?location=48,-122`, dostawca wartości tworzy następujące pary klucz-wartość:

- id = &quot;1&quot;
- Lokalizacja = &quot;48 122&quot;

(Przy założeniu, że domyślnym szablonem trasy jest &quot;interfejs API/{Controller}/{ID&quot;}).

Nazwa parametru do powiązania jest przechowywana we właściwości **ModelBindingContext. ModelName** . Spinacz modelu szuka klucza z tą wartością w słowniku. Jeśli wartość istnieje i można ją przekonwertować na obiekt `GeoPoint`, spinacz modelu przypisuje wartość powiązaną do właściwości **ModelBindingContext. model** .

Należy zauważyć, że spinacz modelu nie jest ograniczony do konwersji typu prostego. W tym przykładzie spinacz modelu najpierw szuka tabeli znanych lokalizacji, a jeśli to się nie powiedzie, używa konwersji typu.

**Ustawianie spinacza modelu**

Istnieje kilka sposobów na ustawienie spinacza modelu. Najpierw można dodać atrybut **[ModelBinder]** do parametru.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Można również dodać atrybut **[ModelBinder]** do typu. Interfejs API sieci Web będzie używać określonego spinacza modelu dla wszystkich parametrów tego typu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Na koniec można dodać dostawcę modelu spinacza do **HttpConfiguration**. Dostawca modelu segregatorów jest po prostu klasą fabryki, która tworzy spinacz modelu. Dostawcę można utworzyć, wywodząc się z klasy [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) . Jeśli jednak spinacz modelu obsługuje pojedynczy typ, łatwiej jest użyć wbudowanej **SimpleModelBinderProvider**, która jest przeznaczona do tego celu. Poniższy kod pokazuje, jak to zrobić.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

W przypadku dostawcy powiązań modelu nadal trzeba dodać atrybut **[ModelBinder]** do parametru, aby poinformować internetowy interfejs API, że powinien on używać spinacza modelu, a nie do programu formatującego typu nośnika. Ale teraz nie musisz określać typu spinacza modelu w atrybucie:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Dostawcy wartości

Określono, że spinacz modelu pobiera wartości z dostawcy wartości. Aby napisać niestandardowego dostawcę wartości, zaimplementuj Interfejs **IValueProvider** . Oto przykład, który pobiera wartości z plików cookie w żądaniu:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Należy również utworzyć fabrykę dostawców wartości, wyprowadzając ją z klasy **ValueProviderFactory** .

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Dodaj fabrykę dostawcy wartości do **HttpConfiguration** w następujący sposób.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Interfejs API sieci Web składa wszystkie dostawców wartości, więc gdy spinacz modelu wywołuje **ValueProvider. GetValue**, spinacz modelu otrzymuje wartość od pierwszego dostawcy wartości, który może go utworzyć.

Alternatywnie można ustawić fabrykę dostawcy wartości na poziomie parametru przy użyciu atrybutu **ValueProvider** w następujący sposób:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Oznacza to, że interfejs API sieci Web używa powiązania modelu z określoną fabryką dostawcy wartości, a nie do korzystania z innych zarejestrowanych dostawców wartości.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Powiązania modelu są konkretnym wystąpieniem bardziej ogólnego mechanizmu. Jeśli szukasz atrybutu **[ModelBinder]** , zobaczysz, że pochodzi on z klasy abstrakcyjnej **ParameterBindingAttribute** . Ta klasa definiuje pojedynczą metodę, **GetBinding**, która zwraca obiekt **HttpParameterBinding** :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

**HttpParameterBinding** jest odpowiedzialny za powiązanie parametru z wartością. W przypadku **[ModelBinder]** atrybut zwraca implementację **HttpParameterBinding** , która używa **IModelBinder** do przeprowadzenia rzeczywistego powiązania. Możesz również zaimplementować własne **HttpParameterBinding**.

Załóżmy na przykład, że chcesz uzyskać elementy ETag z `if-match` i `if-none-match` Headers w żądaniu. Zaczniemy od zdefiniowania klasy do reprezentowania elementów ETag.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Zdefiniujemy również Wyliczenie, aby wskazać, czy ma zostać pobrany element ETag z `if-match` nagłówka, `if-none-match` czy z nagłówka.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Poniżej znajduje się **HttpParameterBinding** , który pobiera element ETag z żądanego nagłówka i wiąże go z parametrem typu ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

Metoda **ExecuteBindingAsync** wykonuje powiązanie. W ramach tej metody Dodaj powiązaną wartość parametru do słownika **ActionArgument** w **HttpActionContext**.

> [!NOTE]
> Jeśli metoda **ExecuteBindingAsync** odczytuje treść komunikatu żądania, Przesłoń Właściwość **WillReadBody** , aby zwracała wartość true. Treść żądania może być niebufornym strumieniem, który można tylko raz odczytać, dzięki czemu interfejs API sieci Web wymusza zasadę, którą może odczytać treść komunikatu z najwyżej jednego powiązania.

Aby zastosować niestandardowe **HttpParameterBinding**, można zdefiniować atrybut pochodzący z **ParameterBindingAttribute**. Dla `ETagParameterBinding`programu zdefiniujemy dwa atrybuty, jeden dla `if-match` nagłówków i jeden dla `if-none-match` nagłówków. Oba pochodzą z abstrakcyjnej klasy bazowej.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Oto Metoda kontrolera, która używa `[IfNoneMatch]` atrybutu.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Poza **ParameterBindingAttribute**istnieje inny element Hook służący do dodawania niestandardowego **HttpParameterBinding**. W obiekcie **HttpConfiguration** Właściwość **ParameterBindingRules** jest kolekcją funkcji anonimowych typu (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Można na przykład dodać regułę, która jest stosowana `ETagParameterBinding` przez dowolny parametr ETag metody get z: `if-none-match`

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Funkcja powinna zwracać `null` w przypadku parametrów, w których powiązanie nie ma zastosowania.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Cały proces wiązania parametrów jest kontrolowany przez podłączane usługi **IActionValueBinder**. Domyślna implementacja **IActionValueBinder** wykonuje następujące czynności:

1. Poszukaj **ParameterBindingAttribute** na parametrze. Obejmuje to **[FromBody]** , **[FromUri]** i **[ModelBinder]** lub atrybuty niestandardowe.
2. W przeciwnym razie poszukaj w **HttpConfiguration. ParameterBindingRules** funkcji, która zwraca wartość inną niż null **HttpParameterBinding**.
3. W przeciwnym razie użyj reguł domyślnych, które zostały opisane wcześniej. 

    - Jeśli typ parametru to "Simple" lub ma konwerter typu, powiąż z identyfikatorem URI. Jest to równoznaczne z umieszczeniem atrybutu **[FromUri]** na parametrze.
    - W przeciwnym razie spróbuj odczytać parametr z treści komunikatu. Jest to odpowiednik umieszczania **[FromBody]** na parametrze.

Jeśli chcesz, możesz zastąpić całą usługę **IActionValueBinder** z implementacją niestandardową.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Przykład powiązania parametru niestandardowego](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Jan zapisał dobrą serię wpisów w blogu na temat powiązania parametrów interfejsu API sieci Web:

- [Jak interfejs API sieci Web wykonuje powiązanie parametrów](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Powiązanie parametrów stylu MVC dla internetowego interfejsu API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Jak utworzyć powiązanie z obiektami niestandardowymi w sygnaturach akcji w interfejsie API MVC/Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Jak utworzyć niestandardowego dostawcę wartości w interfejsie API sieci Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Powiązanie parametrów internetowego interfejsu API pod okapem](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)

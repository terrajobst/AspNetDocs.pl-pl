---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing i wybór akcji w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554888"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a>Routing i wybór akcji w interfejsie API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

W tym artykule opisano sposób, w jaki interfejs API sieci Web ASP.NET kieruje żądanie HTTP do określonej akcji na kontrolerze.

> [!NOTE]
> Ogólne omówienie routingu można znaleźć [w temacie Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).

Ten artykuł zawiera szczegółowe informacje o procesie routingu. Jeśli tworzysz projekt interfejsu API sieci Web i okaże się, że niektóre żądania nie są kierowane w oczekiwany sposób, miejmy nadzieję tego artykułu.

Routing ma trzy główne etapy:

1. Dopasowanie identyfikatora URI do szablonu trasy.
2. Wybieranie kontrolera.
3. Wybieranie akcji.

Niektóre części procesu można zastąpić własnymi zachowaniami niestandardowymi. W tym artykule opisano zachowanie domyślne. Na końcu zanotujemy miejsca, w których można dostosować zachowanie.

## <a name="route-templates"></a>Szablony tras

Szablon trasy wygląda podobnie do ścieżki identyfikatora URI, ale może mieć wartości symboli zastępczych, wskazujący na nawiasy klamrowe:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Podczas tworzenia trasy można podać wartości domyślne dla niektórych lub wszystkich symboli zastępczych:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Można również określić ograniczenia, które ograniczają sposób, w jaki segment identyfikatora URI może być zgodny z symbolem zastępczym:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Struktura próbuje dopasować segmenty w ścieżce URI do szablonu. Literały w szablonie muszą dokładnie pasować. Symbol zastępczy pasuje do dowolnej wartości, o ile nie określono ograniczeń. Struktura nie jest zgodna z innymi częściami identyfikatora URI, takimi jak nazwa hosta lub parametry zapytania. Struktura wybiera pierwszą trasę w tabeli tras, która jest zgodna z identyfikatorem URI.

Istnieją dwa specjalne symbole zastępcze: "{Controller}" i "{Action}".

- "{Controller}" zawiera nazwę kontrolera.
- "{Action}" zawiera nazwę akcji. W przypadku interfejsu API sieci Web zwykła Konwencja to pominięcie "{Action}".

### <a name="defaults"></a>Wartość domyślna

W przypadku podania wartości domyślnych trasa będzie zgodna z identyfikatorem URI, w którym brakuje tych segmentów. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Identyfikatory URI `http://localhost/api/products/all` i `http://localhost/api/products` są zgodne z poprzednią trasą. W ostatnim identyfikatorze URI w brakującym segmencie `{category}` jest przypisywana wartość domyślna `all`.

### <a name="route-dictionary"></a>Słownik tras

Jeśli struktura znajdzie dopasowanie dla identyfikatora URI, tworzy słownik zawierający wartość dla każdego symbolu zastępczego. Klucze są nazwami symboli zastępczych, bez uwzględnienia nawiasów klamrowych. Wartości są pobierane ze ścieżki identyfikatora URI lub z wartości domyślnych. Słownik jest przechowywany w obiekcie **IHttpRouteData** .

W ramach tej fazy dopasowania trasy specjalne symbole zastępcze "{Controller}" i "{Action}" są traktowane podobnie jak inne symbole zastępcze. Są one po prostu przechowywane w słowniku z innymi wartościami.

Wartością domyślną może być wartość specjalna **RouteParameter. Optional**. Jeśli symbol zastępczy zostanie przypisany do tej wartości, wartość nie zostanie dodana do słownika tras. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

W przypadku ścieżki URI "interfejsy API/produkty" słownik tras będzie zawierać następujące polecenie:

- Kontroler: "produkty"
- Kategoria: "wszystkie"

Jednak dla "API/produkty/zabawki/123" słownik tras będzie zawierał następujące polecenie:

- Kontroler: "produkty"
- Kategoria: "zabawki"
- ID: "123"

Wartości domyślne mogą również zawierać wartość, która nie występuje w żadnym miejscu szablonu trasy. Jeśli trasa jest zgodna, ta wartość jest przechowywana w słowniku. Na przykład:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Jeśli ścieżka identyfikatora URI to "API/root/8", słownik będzie zawierać dwie wartości:

- Kontroler: "klienci"
- Identyfikator: "8"

## <a name="selecting-a-controller"></a>Wybieranie kontrolera

Wybór kontrolera jest obsługiwany przez metodę **IHttpControllerSelector. SelectController** . Ta metoda przyjmuje wystąpienie **HttpRequestMessage** i zwraca **HttpControllerDescriptor**. Domyślna implementacja jest dostarczana przez klasę **DefaultHttpControllerSelector** . Ta klasa używa prostego algorytmu:

1. Poszukaj w słowniku tras klucza "Controller".
2. Wypełnij wartość tego klucza i dołącz ciąg "Controller", aby uzyskać nazwę typu kontrolera.
3. Poszukaj kontrolera internetowego interfejsu API o tej nazwie typu.

Na przykład jeśli słownik tras zawiera parę klucz-wartość "Controller" = "Products", typ kontrolera to "ProductsController". Jeśli nie ma pasującego typu lub wiele dopasowań, struktura zwraca błąd do klienta.

W kroku 3 **DefaultHttpControllerSelector** używa interfejsu **IHttpControllerTypeResolver** , aby uzyskać listę typów kontrolerów interfejsu API sieci Web. Domyślna implementacja **IHttpControllerTypeResolver** zwraca wszystkie klasy publiczne, które implementują **IHttpController**, (b) nie są abstrakcyjne i (c) mają nazwę kończącą się na "Controller".

## <a name="action-selection"></a>Wybór akcji

Po wybraniu kontrolera, struktura wybiera akcję przez wywołanie metody **IHttpActionSelector. SelectAction** . Ta metoda pobiera **HttpControllerContext** i zwraca **HttpActionDescriptor**.

Domyślna implementacja jest dostarczana przez klasę **ApiControllerActionSelector** . Aby wybrać akcję, będzie ona wyglądać następująco:

- Metoda HTTP żądania.
- Symbol zastępczy "{Action}" w szablonie trasy, jeśli jest obecny.
- Parametry akcji na kontrolerze.

Przed przystąpieniem do algorytmu wyboru musimy zrozumieć pewne kwestie dotyczące akcji kontrolera.

**Które metody kontrolera są uznawane za "akcje"?** Po wybraniu akcji struktura sprawdza tylko publiczne metody wystąpienia na kontrolerze. Oprócz tego wyklucza metody ["name Special"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (konstruktory, zdarzenia, przeciążenia operatora itd.) i metody dziedziczone z klasy **ApiController** .

**Metody HTTP.** Struktura wybiera tylko te akcje, które pasują do metody HTTP żądania, określone w następujący sposób:

1. Można określić metodę HTTP z atrybutem: **AcceptVerbs**, **HttpDelete**, **Narzędzia HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HTTPPOST**lub **HttpPut**.
2. W przeciwnym razie, jeśli nazwa metody kontrolera rozpoczyna się od "Get", "post", "Put", "Delete", "Start", "Options" lub "patch", wówczas akcja obsługuje tę metodę HTTP.
3. Jeśli żaden z powyższych metod obsługuje wpis POST.

**Powiązania parametrów.** Powiązanie parametru to sposób, w jaki interfejs API sieci Web tworzy wartość dla parametru. Oto domyślna reguła dla powiązania parametrów:

- Typy proste są pobierane z identyfikatora URI.
- Typy złożone są pobierane z treści żądania.

Typy proste obejmują wszystkie [.NET Framework typy pierwotne](https://msdn.microsoft.com/library/system.type.isprimitive), a także **DateTime**, **Decimal**, **GUID**, **String**i **TimeSpan**. Dla każdej akcji, co najwyżej jeden parametr może odczytać treść żądania.

> [!NOTE]
> Istnieje możliwość zastąpienia domyślnych reguł powiązań. Zobacz [powiązanie parametrów WebAPI pod okapem](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).

W tym tle jest to algorytm wyboru akcji.

1. Utwórz listę wszystkich akcji na kontrolerze, które pasują do metody żądania HTTP.
2. Jeśli słownik trasy ma wpis "Action", Usuń akcje, których nazwa nie jest zgodna z tą wartością.
3. Spróbuj dopasować parametry akcji do identyfikatora URI w następujący sposób: 

    1. Dla każdej akcji należy uzyskać listę parametrów, które są typu prostego, gdzie powiązanie pobiera parametr z identyfikatora URI. Wyklucz parametry opcjonalne.
    2. Na tej liście spróbuj znaleźć dopasowanie dla każdej nazwy parametru w słowniku trasy lub w ciągu zapytania identyfikatora URI. W dopasowaniach jest rozróżniana wielkość liter i nie zależą od kolejności parametrów.
    3. Wybierz akcję, w której każdy parametr na liście ma dopasowanie w identyfikatorze URI.
    4. Jeśli więcej niż jedna akcja spełnia te kryteria, wybierz ją z największą liczbą pasujących parametrów.
4. Ignoruj akcje z atrybutem **[Unactions]** .

#3 kroków jest prawdopodobnie najbardziej mylące. Podstawowym pomysłem jest to, że parametr może pobrać jego wartość z identyfikatora URI, z treści żądania lub z niestandardowego powiązania. W przypadku parametrów, które pochodzą z identyfikatora URI, chcemy upewnić się, że identyfikator URI rzeczywiście zawiera wartość dla tego parametru, w ścieżce (za pośrednictwem słownika tras) lub w ciągu zapytania.

Rozważmy na przykład następujące czynności:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Parametr *ID* jest powiązany z identyfikatorem URI. W związku z tym ta akcja może być zgodna tylko z identyfikatorem URI zawierającym wartość "ID" w słowniku trasy lub w ciągu zapytania.

Parametry opcjonalne są wyjątkiem, ponieważ są opcjonalne. W przypadku parametru opcjonalnego jest to prawidłowe, Jeśli powiązanie nie może pobrać wartości z identyfikatora URI.

Typy złożone są wyjątkiem z innego powodu. Typ złożony można powiązać tylko z identyfikatorem URI za pomocą niestandardowego powiązania. Jednak w takim przypadku struktura nie może się z wyprzedzeniem wiedzieć, czy parametr zostałby powiązany z określonym identyfikatorem URI. Aby się dowiedzieć, należy wywołać powiązanie. Celem algorytmu wyboru jest wybranie akcji z opisu statycznego przed wywołaniem jakichkolwiek powiązań. W związku z tym typy złożone są wykluczone z zgodnego algorytmu.

Po wybraniu akcji wszystkie powiązania parametrów są wywoływane.

Podsumowanie:

- Akcja musi być zgodna z metodą HTTP żądania.
- Nazwa akcji musi być zgodna z wpisem "Action" w słowniku trasy, jeśli jest obecny.
- Dla każdego parametru akcji, jeśli parametr jest pobierany z identyfikatora URI, nazwa parametru musi być znaleziona w słowniku trasy lub w ciągu zapytania identyfikatora URI. (Parametry opcjonalne i parametry o typach złożonych są wykluczone).
- Spróbuj dopasować największą liczbę parametrów. Najlepszym dopasowaniem może być Metoda bez parametrów.

## <a name="extended-example"></a>Przykład rozszerzony

Rozsyłan

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Kontroler:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Żądanie HTTP:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Dopasowanie trasy

Identyfikator URI pasuje do trasy o nazwie "DefaultApi". Słownik tras zawiera następujące wpisy:

- Kontroler: "produkty"
- Identyfikator: "1"

Słownik tras nie zawiera parametrów ciągu zapytania, "Version" i "Details", ale te informacje nadal będą brane pod uwagę podczas wyboru akcji.

### <a name="controller-selection"></a>Wybór kontrolera

W przypadku wpisu "Controller" w słowniku trasy typ kontrolera jest `ProductsController`.

### <a name="action-selection"></a>Wybór akcji

Żądanie HTTP jest żądaniem GET. Akcje kontrolera obsługiwane przez program GET to `GetAll`, `GetById`i `FindProductsByName`. Słownik tras nie zawiera wpisu "Action", więc nie musi on pasować do nazwy akcji.

Następnie spróbujemy dopasować nazwy parametrów dla akcji, szukając tylko akcji GET.

| Akcja | Parametry do dopasowania |
| --- | --- |
| `GetAll` | brak |
| `GetById` | # |
| `FindProductsByName` | Nazwij |

Należy zauważyć, że parametr *version* `GetById` nie jest brany pod uwagę, ponieważ jest to parametr opcjonalny.

Metoda `GetAll` pasuje do siebie. Metoda `GetById` jest również zgodna, ponieważ słownik tras zawiera wartość "ID". Metoda `FindProductsByName` nie jest zgodna.

Metoda `GetById` WINS, ponieważ pasuje do jednego parametru, a nie parametrów dla `GetAll`. Metoda jest wywoływana z następującymi wartościami parametrów:

- *Identyfikator* = 1
- *wersja* = 1,5

Zwróć uwagę, że mimo że *wersja* nie została użyta w algorytmie wyboru, wartość parametru pochodzi z ciągu zapytania identyfikatora URI.

## <a name="extension-points"></a>Punkty rozszerzenia

Interfejs API sieci Web udostępnia punkty rozszerzenia dla niektórych części procesu routingu.

| Interfejs | Opis |
| --- | --- |
| **IHttpControllerSelector** | Wybiera kontroler. |
| **IHttpControllerTypeResolver** | Pobiera listę typów kontrolerów. **DefaultHttpControllerSelector** wybiera typ kontrolera z tej listy. |
| **IAssembliesResolver** | Pobiera listę zestawów projektu. Interfejs **IHttpControllerTypeResolver** używa tej listy do znajdowania typów kontrolerów. |
| **IHttpControllerActivator** | Tworzy nowe wystąpienia kontrolera. |
| **IHttpActionSelector** | Wybiera akcję. |
| **IHttpActionInvoker** | Wywołuje akcję. |

Aby zapewnić własną implementację dla dowolnego z tych interfejsów, Użyj kolekcji **Services** w obiekcie **HttpConfiguration** :

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]

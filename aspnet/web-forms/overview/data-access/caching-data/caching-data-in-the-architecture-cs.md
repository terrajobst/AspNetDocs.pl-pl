---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Buforowanie danych w architekturze (C#) | Microsoft Docs
author: rick-anderson
description: W poprzednim samouczku dowiesz się, jak zastosować buforowanie w warstwie prezentacji. W tym samouczku dowiesz się, jak korzystać z naszych warstwowych architectu...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 192cadb8e2f862ac2a97a36b375e247b281ece93
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600832"
---
# <a name="caching-data-in-the-architecture-c"></a>Buforowanie danych w architekturze (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) lub [Pobierz plik PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> W poprzednim samouczku dowiesz się, jak zastosować buforowanie w warstwie prezentacji. W tym samouczku dowiesz się, jak wykorzystać architekturę warstwową do buforowania danych w warstwie logiki biznesowej. Możemy to zrobić, rozszerzając architekturę w celu uwzględnienia warstwy pamięci podręcznej.

## <a name="introduction"></a>Wprowadzenie

Jak zostało to opisane w poprzednim samouczku, buforowanie danych ObjectDataSource s jest tak proste jak ustawienie kilku właściwości. Niestety, element ObjectDataSource stosuje buforowanie w warstwie prezentacji, co ściśle Couples Zasady buforowania przy użyciu strony ASP.NET. Jedną z przyczyn utworzenia architektury warstwowej jest umożliwienie przerwania takich sprzężeń. Warstwa logiki biznesowej na przykład oddziela logikę biznesową od stron ASP.NET, podczas gdy warstwa dostępu do danych oddziela szczegóły dostępu do danych. Takie oddzielenie informacji o logice biznesowej i dostępie do danych jest preferowane w części, ponieważ sprawia, że system jest bardziej czytelny i bardziej elastyczny. Pozwala ona również na znajomość domeny i dział robocizny pracujących nad warstwą prezentacji. nie należy znać informacji o szczegółach dotyczących bazy danych w celu wykonania zadania. Oddzielenie zasad buforowania od warstwy prezentacji oferuje podobne korzyści.

W tym samouczku zostanie wdrożona nasza architektura obejmująca *warstwę pamięci podręcznej* (lub CL dla krótkich), która korzysta z naszych zasad buforowania. Warstwa buforowania będzie zawierać klasę `ProductsCL`, która zapewnia dostęp do informacji o produkcie przy użyciu metod takich jak `GetProducts()`, `GetProductsByCategoryID(categoryID)`i tak dalej, że po wywołaniu zostanie najpierw podjęta próba pobrania danych z pamięci podręcznej. Jeśli pamięć podręczna jest pusta, te metody spowodują wywołanie odpowiedniej metody `ProductsBLL` w LOGIKI biznesowej, która spowodowałaby pobieranie danych z DAL. Metody `ProductsCL` buforują dane pobierane z LOGIKI biznesowej przed ich zwróceniem.

Jak pokazano na rysunku 1, te CL znajdują się między warstwami prezentacji i logiki biznesowej.

![Warstwa buforowania (CL) jest kolejną warstwą w naszej architekturze](caching-data-in-the-architecture-cs/_static/image1.png)

**Rysunek 1**. warstwa buforowania (CL) jest kolejną warstwą w naszej architekturze

## <a name="step-1-creating-the-caching-layer-classes"></a>Krok 1. tworzenie klas warstwy buforowania

W tym samouczku utworzymy bardzo proste CL przy użyciu pojedynczej klasy `ProductsCL`, która ma tylko kilku metod. Utworzenie kompletnej warstwy buforowania dla całej aplikacji wymagało utworzenia klas `CategoriesCL`, `EmployeesCL`i `SuppliersCL` oraz zapewnienia metody w tych klasach warstwy buforowania dla każdej metody dostępu do danych lub modyfikacji w LOGIKI biznesowej. Podobnie jak w przypadku LOGIKI biznesowej i DAL, warstwa buforowania powinna zostać zaimplementowana jako oddzielny projekt biblioteki klas. jednak zostanie ona wdrożona jako Klasa w folderze `App_Code`.

Aby dokładniej oddzielić klasy CL z klas DAL i LOGIKI biznesowej, pozwól s utworzyć nowy podfolder w folderze `App_Code`. Kliknij prawym przyciskiem myszy folder `App_Code` w Eksplorator rozwiązań, wybierz pozycję Nowy folder i nazwij nowy folder `CL`. Po utworzeniu tego folderu Dodaj do niego nową klasę o nazwie `ProductsCL.cs`.

![Dodaj nowy folder o nazwie CL i klasę o nazwie ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Rysunek 2**. Dodawanie nowego folderu o nazwie `CL` i klasy o nazwie `ProductsCL.cs`

Klasa `ProductsCL` powinna zawierać ten sam zestaw metod dostępu do danych i modyfikacji, jak znaleziono w odpowiedniej klasie warstwy logiki biznesowej (`ProductsBLL`). Zamiast tworzyć wszystkie te metody, pozwól, aby po prostu kompilować kilka tutaj, aby uzyskać wrażenie dla wzorców używanych przez CL. W szczególności dodamy metody `GetProducts()` i `GetProductsByCategoryID(categoryID)` w kroku 3 i Przeciążenie `UpdateProduct` w kroku 4. Pozostałe metody `ProductsCL` i `CategoriesCL`, `EmployeesCL`i klasy `SuppliersCL` można dodać na wypoczynek.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Krok 2: odczytywanie i zapisywanie w pamięci podręcznej danych

Funkcja buforowania elementu ObjectDataSource w poprzednim samouczku wewnętrznie używa pamięci podręcznej danych ASP.NET do przechowywania danych pobranych z LOGIKI biznesowej. Dostęp do pamięci podręcznej danych można również programistycznie z klas ASP.NETych za pomocą kodu stron lub z klas w architekturze aplikacji sieci Web. Aby odczytywać i zapisywać dane w pamięci podręcznej danych z klasy ASP.NET strony powiązanej z kodem, użyj następującego wzorca:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[Metoda`Insert`](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) [`Cache` klasy](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s ma wiele przeciążeń. `Cache["key"] = value` i `Cache.Insert(key, value)` są synonimami i oba dodają element do pamięci podręcznej przy użyciu określonego klucza bez zdefiniowanego wygaśnięcia. Zazwyczaj chcemy określić wygaśnięcie przy dodawaniu elementu do pamięci podręcznej, w zależności od czasu, lub w obu przypadkach. Użyj jednego z innych przeciążeń metody `Insert`, aby zapewnić informacje o wygasaniu zależności lub czasu.

Metody warstwy buforowania muszą najpierw sprawdzić, czy żądane dane znajdują się w pamięci podręcznej, a jeśli tak, należy zwrócić ją z tego miejsca. Jeśli żądane dane nie znajduje się w pamięci podręcznej, należy wywołać odpowiednią metodę LOGIKI biznesowej. Wartość zwracana powinna być buforowana i zwracana, jak pokazano na poniższym diagramie sekwencji.

![Metody buforowania warstwy s zwracają dane z pamięci podręcznej, jeśli są dostępne](caching-data-in-the-architecture-cs/_static/image3.png)

**Rysunek 3**. buforowanie metod warstwy s zwraca dane z pamięci podręcznej, jeśli są dostępne

Sekwencja przedstawiony na rysunku 3 jest realizowana w klasach CL przy użyciu następującego wzorca:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

W tym miejscu *wpisz* typ danych przechowywanych w pamięci podręcznej `Northwind.ProductsDataTable`, na przykład, gdy *klucz* jest kluczem, który jednoznacznie identyfikuje element pamięci podręcznej. Jeśli element o określonym *kluczu* nie znajduje się w pamięci podręcznej, *wystąpienie* zostanie `null` i dane zostaną pobrane z odpowiedniej metody logiki biznesowej i dodane do pamięci podręcznej. Po osiągnięciu czasu `return instance` *wystąpienia* zawiera odwołanie do danych z pamięci podręcznej lub pobrane z logiki biznesowej.

Należy pamiętać, aby użyć powyższego wzorca podczas uzyskiwania dostępu do danych z pamięci podręcznej. Poniższy wzorzec, który na pierwszy rzut oka, wygląda jako odpowiednik, zawiera delikatne różnice, które wprowadzają sytuację wyścigu. Sytuacje wyścigu są trudne do debugowania, ponieważ ujawniają się one sporadycznie i trudno je odtworzyć.

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

Różnica w tej sekundzie jest nieprawidłowym fragmentem kodu, który zamiast przechowywania odwołania do pamięci podręcznej w zmiennej lokalnej jest dostęp do pamięci podręcznej danych bezpośrednio w instrukcji warunkowej *i* w `return`. Załóżmy, że po osiągnięciu tego kodu `Cache["key"]` nie`null`, ale przed osiągnięciem instrukcji `return`, system wyklucza *klucz* z pamięci podręcznej. W tym rzadkim przypadku kod zwróci wartość `null`, a nie obiekt o oczekiwanym typie.

> [!NOTE]
> Pamięć podręczna danych jest bezpieczna wątkowo, dlatego nie trzeba synchronizować dostępu do wątków w przypadku prostych odczytów lub zapisów. Jeśli jednak trzeba wykonać wiele operacji na danych w pamięci podręcznej, które muszą być niepodzielne, użytkownik jest odpowiedzialny za wdrożenie blokady lub innego mechanizmu w celu zapewnienia bezpieczeństwa wątków. Aby uzyskać więcej informacji [, zobacz Synchronizowanie dostępu do pamięci Podręcznej ASP.NET](http://www.ddj.com/184406369) .

Element może być programowo wykluczony z pamięci podręcznej danych przy użyciu [metody`Remove`](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) , takiej jak:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Krok 3. Zwracanie informacji o produkcie z klasy`ProductsCL`

W tym samouczku postanowimy zaimplementować dwie metody zwracania informacji o produkcie z klasy `ProductsCL`: `GetProducts()` i `GetProductsByCategoryID(categoryID)`. Podobnie jak w przypadku klasy `ProductsBL` w warstwie logiki biznesowej, Metoda `GetProducts()` w CL zwraca informacje o wszystkich produktach jako obiekt `Northwind.ProductsDataTable`, podczas gdy `GetProductsByCategoryID(categoryID)` zwraca wszystkie produkty z określonej kategorii.

Poniższy kod przedstawia część metod w klasie `ProductsCL`:

[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

Najpierw należy zwrócić uwagę na `DataObject` i `DataObjectMethodAttribute` atrybuty zastosowane do klasy i metod. Te atrybuty dostarczają informacje do kreatora ObjectDataSource s wskazujące klasy i metody, które powinny być wyświetlane w krokach kreatora s. Ze względu na to, że metody i klasy CL są dostępne z elementu ObjectDataSource w warstwie prezentacji, te atrybuty zostały dodane w celu zwiększenia środowiska czasu projektowania. Zapoznaj się z powrotem z samouczkiem [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-cs.md) , aby uzyskać dokładniejszy opis tych atrybutów i ich efektów.

W metodach `GetProducts()` i `GetProductsByCategoryID(categoryID)` dane zwrócone przez metodę `GetCacheItem(key)` są przypisywane do zmiennej lokalnej. Metoda `GetCacheItem(key)`, którą sprawdzimy wkrótce, zwraca określony element z pamięci podręcznej na podstawie określonego *klucza*. Jeśli nie zostaną znalezione takie dane w pamięci podręcznej, zostaną pobrane z odpowiedniej metody klasy `ProductsBLL`, a następnie dodane do pamięci podręcznej przy użyciu metody `AddCacheItem(key, value)`.

Interfejs `GetCacheItem(key)` i `AddCacheItem(key, value)` metody z pamięcią podręczną danych, odczytywanie i zapisywanie odpowiednio wartości. Metoda `GetCacheItem(key)` jest prostszym z dwóch. Po prostu zwraca wartość z klasy cache przy użyciu *klucza*przekazywania:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` nie używa wartości *klucza* jako dostarczonej, ale zamiast tego wywołuje metodę `GetCacheKey(key)`, która zwraca *klucz* poprzedzony ProductsCache-. `MasterCacheKeyArray`, który zawiera ciąg ProductsCache, jest również używany przez metodę `AddCacheItem(key, value)`, ponieważ zobaczymy chwilę.

W klasie z kodem ASP.NET Page s można uzyskać dostęp do pamięci podręcznej danych przy użyciu [właściwości`Cache`](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)klasy `Page` i umożliwia składnię taką jak `Cache["key"] = value`, zgodnie z opisem w kroku 2. Z klasy w ramach architektury można uzyskać dostęp do pamięci podręcznej danych przy użyciu `HttpRuntime.Cache` lub `HttpContext.Current.Cache`. Wpis w blogu [Piotr Johnsonem](https://weblogs.asp.net/pjohnson/default.aspx) [HttpRuntime. cache zamiast HttpContext. Current. cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) uwagi o nieznacznej wydajności w przypadku używania `HttpRuntime` zamiast `HttpContext.Current`; w związku z tym `ProductsCL` używa `HttpRuntime`.

> [!NOTE]
> Jeśli architektura jest implementowana przy użyciu projektów biblioteki klas, należy dodać odwołanie do zestawu `System.Web`, aby użyć klas [httpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) i [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) .

Jeśli element nie zostanie znaleziony w pamięci podręcznej, metody `ProductsCL` klasy s pobierają dane z LOGIKI biznesowej i dodają je do pamięci podręcznej przy użyciu metody `AddCacheItem(key, value)`. Aby dodać *wartość* do pamięci podręcznej, można użyć poniższego kodu, który korzysta z 60 sekund po upływie limitu czasu:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` określa czas wygaśnięcia (w sekundach) 60 sekund, podczas gdy [`System.Web.Caching.Cache.NoSlidingExpiration`](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) wskazuje, że nie wystąpiło wygaśnięcie. Chociaż to `Insert` Przeciążenie metody ma parametry wejściowe dla wygaśnięcia bezwzględnego i przesuwania, można podać tylko jeden z nich. W przypadku próby określenia czasu bezwzględnego i przedziału czasu Metoda `Insert` zgłosi wyjątek `ArgumentException`.

> [!NOTE]
> Ta implementacja metody `AddCacheItem(key, value)` aktualnie ma pewne nieprawidłowości. Będziemy nam rozwiązać te problemy i przezwyciężyć je w kroku 4.

## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Krok 4. unieważnienie pamięci podręcznej, gdy dane są modyfikowane za pomocą architektury

Wraz z metodami pobierania danych warstwa buforowania musi dostarczać te same metody jak LOGIKI biznesowej do wstawiania, aktualizowania i usuwania danych. Metody modyfikacji danych CL s nie modyfikują danych w pamięci podręcznej, ale raczej wywołują odpowiednią metodę modyfikacji danych LOGIKI biznesowej, a następnie unieważnić pamięć podręczną. Jak zostało to opisane w poprzednim samouczku, jest to takie samo zachowanie, które element ObjectDataSource ma zastosowanie po włączeniu jego funkcji pamięci podręcznej, a jego metody `Insert`, `Update`lub `Delete` są wywoływane.

Następujące `UpdateProduct` Przeciążenie ilustruje sposób implementacji metod modyfikacji danych w CL:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

Odpowiednia metoda warstwy logiki biznesowej modyfikacji danych jest wywoływana, ale przed zwróceniem odpowiedzi musimy unieważnić pamięć podręczną. Niestety, unieważnienie pamięci podręcznej nie jest proste, ponieważ Klasa `ProductsCL` s `GetProducts()` i `GetProductsByCategoryID(categoryID)` metody dodają elementy do pamięci podręcznej z różnymi kluczami, a metoda `GetProductsByCategoryID(categoryID)` dodaje inny element pamięci podręcznej dla każdego unikatowego *IDkategorii*.

Podczas unieważniania pamięci podręcznej musimy usunąć *wszystkie* elementy, które mogły zostać dodane przez klasę `ProductsCL`. Można to osiągnąć, kojarząc *zależność pamięci podręcznej* z każdym elementem dodanym do pamięci podręcznej w metodzie `AddCacheItem(key, value)`. Ogólnie rzecz biorąc, zależność pamięci podręcznej może być kolejną pozycją w pamięci podręcznej, plikiem w systemie plików lub danymi z bazy danych Microsoft SQL Server. Gdy zależność ulegnie zmianie lub zostanie usunięta z pamięci podręcznej, elementy pamięci podręcznej, z którą są skojarzone, są automatycznie usuwane z pamięci podręcznej. W tym samouczku chcemy utworzyć dodatkowy element w pamięci podręcznej, który służy jako zależność pamięci podręcznej dla wszystkich elementów dodanych za pomocą klasy `ProductsCL`. W ten sposób wszystkie te elementy można usunąć z pamięci podręcznej, po prostu usuwając zależność pamięci podręcznej.

Pozwól s zaktualizować metodę `AddCacheItem(key, value)` tak, aby każdy element dodany do pamięci podręcznej za pomocą tej metody został skojarzony z pojedynczą zależnością pamięci podręcznej:

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` jest tablicą ciągów, która zawiera pojedynczą wartość ProductsCache. Najpierw element pamięci podręcznej jest dodawany do pamięci podręcznej i przypisywany jest bieżącą datę i godzinę. Jeśli element pamięci podręcznej już istnieje, zostanie zaktualizowany. Następnie zostanie utworzona zależność pamięci podręcznej. Konstruktor [`CacheDependency` klasy](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s ma wiele przeciążeń, ale jest on używany w tym miejscu, który oczekuje dwóch `string` danych wejściowych tablicy. Pierwszy z nich określa zestaw plików, które mają być używane jako zależności. Ponieważ nie chcemy używać żadnych zależności opartych na plikach, wartość `null` jest używana jako pierwszy parametr wejściowy. Drugi parametr wejściowy określa zestaw kluczy pamięci podręcznej do użycia jako zależności. Tutaj określamy nasze pojedyncze zależności, `MasterCacheKeyArray`. `CacheDependency` jest następnie przenoszona do metody `Insert`.

W przypadku tej modyfikacji `AddCacheItem(key, value)`nieprawidłowej pamięci podręcznej jest tak proste jak usuwanie zależności.

[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Krok 5. wywoływanie warstwy buforowania z warstwy prezentacji

Klasy i metody buforowania mogą służyć do pracy z danymi przy użyciu technik, które nie są sprawdzane w tych samouczkach. Aby zilustrować pracę z danymi buforowanymi, Zapisz zmiany w klasie `ProductsCL`, a następnie otwórz stronę `FromTheArchitecture.aspx` w folderze `Caching` i Dodaj widok GridView. W tagu inteligentnym GridView Utwórz nowy element ObjectDataSource. W pierwszym kroku kreatora należy zobaczyć klasę `ProductsCL` jako jedną z opcji z listy rozwijanej.

[![Klasa ProductsCL jest uwzględniona na liście rozwijanej obiekt biznesowy](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Rysunek 4**. Klasa `ProductsCL` jest uwzględniona na liście rozwijanej obiekt biznesowy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-in-the-architecture-cs/_static/image6.png))

Po wybraniu `ProductsCL`kliknij przycisk Dalej. Lista rozwijana na karcie Wybierz zawiera dwa elementy — `GetProducts()` i `GetProductsByCategoryID(categoryID)`, a na karcie aktualizacja znajduje się jedyny `UpdateProduct` przeciążenia. Wybierz metodę `GetProducts()` z karty wybierz i metodę `UpdateProducts` z karty aktualizacja, a następnie kliknij przycisk Zakończ.

[![metody klasy s ProductsCL są wyświetlane na listach rozwijanych](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Rysunek 5**. metody klasy `ProductsCL` s są wymienione na liście rozwijanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](caching-data-in-the-architecture-cs/_static/image9.png))

Po zakończeniu działania kreatora program Visual Studio ustawi właściwość ObjectDataSource s `OldValuesParameterFormatString` na `original_{0}` i doda odpowiednie pola do widoku GridView. Zmień właściwość `OldValuesParameterFormatString` na wartość domyślną, `{0}`i skonfiguruj widok GridView, aby obsługiwał stronicowanie, sortowanie i edytowanie. Ponieważ Przeciążenie `UploadProducts` używane przez CL akceptuje tylko edytowane nazwy i ceny produktu, należy ograniczyć widok GridView, aby można było edytować tylko te pola.

W poprzednim samouczku zdefiniowano widok GridView, aby uwzględnić pola `ProductName`, `CategoryName`i `UnitPrice`. Możesz przeprowadzić replikację tego formatowania i struktury, w takim przypadku znaczniki deklaratywne GridView i ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

W tym momencie mamy stronę korzystającą z warstwy buforowania. Aby wyświetlić pamięć podręczną w działaniu, ustaw punkty przerwania w metodach `GetProducts()` `ProductsCL` klasy s i `UpdateProduct`. Odwiedź stronę w przeglądarce i przechodzenie przez kod podczas sortowania i stronicowania, aby wyświetlić dane pobrane z pamięci podręcznej. Następnie zaktualizuj rekord i zwróć uwagę, że pamięć podręczna jest unieważniona i w związku z tym jest pobierana z LOGIKI biznesowej, gdy dane są ponownie powiązane z elementem GridView.

> [!NOTE]
> Warstwa buforowania podana w tym artykule nie została ukończona. Zawiera tylko jedną klasę, `ProductsCL`, która tylko sportuje kilku metod. Ponadto tylko jedna strona ASP.NET używa CL (`~/Caching/FromTheArchitecture.aspx`) wszystkie inne nadal odwoływać się bezpośrednio do LOGIKI biznesowej. Jeśli planujesz używanie CL w aplikacji, wszystkie wywołania z warstwy prezentacji powinny przechodzić do CL, co wymagałoby, aby klasy i metody CL zostały objęte tymi klasami i metodami w LOGIKI biznesowej aktualnie używanym przez warstwę prezentacji.

## <a name="summary"></a>Podsumowanie

Podczas gdy buforowanie może być stosowane w warstwie prezentacji przy użyciu ASP.NET 2,0 s kontrolki SqlDataSource i elementów ObjectDataSource, idealne obowiązki buforowania byłyby delegowane do oddzielnej warstwy w architekturze. W tym samouczku utworzyliśmy warstwę pamięci podręcznej, która znajduje się między warstwą prezentacji a warstwą logiki biznesowej. Warstwa buforowania musi dostarczać ten sam zestaw klas i metod, które istnieją w LOGIKI biznesowej i są wywoływane z warstwy prezentacji.

Przykłady w tej warstwie buforowania zostały omówione w tym i poprzednie samouczki zamieszczono z *wyprzedzeniem*. W przypadku ponownego ładowania dane są ładowane do pamięci podręcznej tylko wtedy, gdy jest wykonywane żądanie dotyczące danych i brakuje w niej danych. Dane można także *aktywnie ładować* do pamięci podręcznej, a technika ładująca dane do pamięci podręcznej, zanim będzie ona faktycznie wymagana. W następnym samouczku zobaczymy przykład aktywnego ładowania, gdy zobaczymy, jak przechowywać wartości statyczne w pamięci podręcznej podczas uruchamiania aplikacji.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Teresa Murph. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-with-the-objectdatasource-cs.md)
> [dalej](caching-data-at-application-startup-cs.md)

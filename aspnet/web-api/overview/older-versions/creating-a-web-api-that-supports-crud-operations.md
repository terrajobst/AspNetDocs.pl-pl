---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Włączanie operacji CRUD w ASP.NET Web API 1-ASP.NET 4. x
author: MikeWasson
description: W samouczku pokazano, jak obsługiwać operacje CRUD w usłudze HTTP przy użyciu internetowego interfejsu API ASP.NET dla ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600334"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Włączanie operacji CRUD w interfejsie Web API ASP.NET 1

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> W tym samouczku pokazano, jak obsługiwać operacje CRUD w usłudze HTTP przy użyciu internetowego interfejsu API ASP.NET dla ASP.NET 4. x.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Visual Studio 2012
> - Interfejs API sieci Web 1 (działa również z interfejsem API sieci Web 2)

CRUD &quot;tworzenie, odczytywanie, aktualizowanie i usuwanie&quot; które są cztery podstawowe operacje bazy danych. Wiele usług HTTP modeluje również operacje CRUD za pośrednictwem interfejsów API REST lub REST.

W tym samouczku utworzysz prosty internetowy interfejs API do zarządzania listą produktów. Każdy produkt będzie zawierać nazwę, cenę i kategorię (na przykład &quot;zabawki&quot; lub &quot;&quot;sprzętu) oraz identyfikator produktu.

Interfejs API produktów będzie uwidaczniał następujące metody.

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobierz listę wszystkich produktów | Pobierz | /api/products |
| Pobierz produkt według identyfikatora | Pobierz | *Identyfikator* /API/Products/ |
| Pobierz produkt według kategorii | Pobierz | /API/Products? Category =*Kategoria* |
| Utwórz nowy produkt | POUBOJOWEGO | /api/products |
| Aktualizowanie produktu | Ubrani | *Identyfikator* /API/Products/ |
| Usuwanie produktu | DELETE | *Identyfikator* /API/Products/ |

Zwróć uwagę, że niektóre identyfikatory URI zawierają identyfikator produktu w ścieżce. Na przykład aby uzyskać produkt o IDENTYFIKATORze 28, klient wysyła żądanie GET dla `http://hostname/api/products/28`.

### <a name="resources"></a>Resources

Interfejs API produktów definiuje identyfikatory URI dla dwóch typów zasobów:

| Zasób | {1&gt;URI&lt;1} |
| --- | --- |
| Lista wszystkich produktów. | /api/products |
| Pojedynczy produkt. | *Identyfikator* /API/Products/ |

### <a name="methods"></a>Metody

Cztery główne metody HTTP (GET, PUT, POST i DELETE) można mapować na operacje CRUD w następujący sposób:

- Pobiera reprezentację zasobu pod określonym identyfikatorem URI. POBIERANIE nie powinno mieć żadnych efektów ubocznych na serwerze.
- UMIESZCZENIE aktualizacji zasobu o określonym identyfikatorze URI. Opcję PUT można także użyć do utworzenia nowego zasobu o określonym identyfikatorze URI, jeśli serwer zezwala klientom na określanie nowych identyfikatorów URI. W tym samouczku interfejs API nie obsługuje tworzenia przez funkcję PUT.
- Po utworzeniu nowego zasobu. Serwer przypisuje identyfikator URI nowego obiektu i zwraca ten identyfikator URI jako część komunikatu odpowiedzi.
- DELETE Usuwa zasób o określonym identyfikatorze URI.

Uwaga: Metoda PUT zastępuje całą jednostkę produktu. Oznacza to, że klient powinien wysłać kompletną reprezentację zaktualizowanego produktu. Jeśli chcesz obsługiwać aktualizacje częściowe, preferowana jest metoda PATCH. Ten samouczek nie implementuje poprawki.

## <a name="create-a-new-web-api-project"></a>Utwórz nowy projekt internetowego interfejsu API

Zacznij od uruchomienia programu Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** . Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**. Nazwij projekt &quot;ProductStore&quot; a następnie kliknij przycisk **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **Web API** i kliknij przycisk **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Dodawanie modelu

*Model* to obiekt, który reprezentuje dane w aplikacji. W interfejsie API sieci Web ASP.NET można używać silnie określonych obiektów CLR jako modeli i automatycznie serializować je do XML lub JSON dla klienta.

W przypadku interfejsu API ProductStore nasze dane składają się z produktów, więc utworzymy nową klasę o nazwie `Product`.

Jeśli Eksplorator rozwiązań nie jest jeszcze widoczna, kliknij menu **Widok** i wybierz pozycję **Eksplorator rozwiązań**. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder **modele** . Z menu kontekstowego wybierz pozycję **Dodaj**, a następnie wybierz pozycję **Klasa**. Nazwij klasę &quot;&quot;produktu.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Dodaj następujące właściwości do klasy `Product`.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Dodawanie repozytorium

Musimy przechowywać kolekcję produktów. Dobrym pomysłem jest oddzielenie kolekcji od naszej implementacji usługi. Dzięki temu można zmienić magazyn zapasowy bez ponownego zapisywania klasy usługi. Ten typ projektu nazywa się wzorcem *repozytorium* . Zacznij od zdefiniowania ogólnego interfejsu dla repozytorium.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder **modele** . Wybierz pozycję **Dodaj**, a następnie wybierz pozycję **nowy element**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń C# węzeł. W C#obszarze Wybierz pozycję **kod**. Na liście szablonów kodu wybierz pozycję **interfejs**. Nazwij interfejs &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Dodaj następującą implementację:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Teraz Dodaj kolejną klasę do folderu models o nazwie &quot;ProductRepository.&quot; Ta klasa zaimplementuje interfejs `IProductRepository`. Dodaj następującą implementację:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Repozytorium zachowuje listę w pamięci lokalnej. Jest to prawidłowe dla samouczka, ale w rzeczywistej aplikacji dane są przechowywane zewnętrznie, w bazie danych lub w magazynie w chmurze. Wzorzec repozytorium będzie łatwiej zmienić implementację później.

## <a name="adding-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

Jeśli pracujesz z ASP.NET MVC, masz już znane kontrolery. W interfejsie API sieci Web ASP.NET *kontroler* jest klasą, która obsługuje żądania HTTP od klienta. Kreator nowego projektu utworzył dwa kontrolery dla Ciebie podczas tworzenia projektu. Aby je wyświetlić, rozwiń folder controllers w Eksplorator rozwiązań.

- HomeController jest tradycyjnym kontrolerem ASP.NET MVC. Jest on odpowiedzialny za obsługę stron HTML dla witryny i nie jest bezpośrednio związany z naszym interfejsem API sieci Web.
- ValuesController jest przykładowym kontrolerem WebAPI.

Przejdź dalej i Usuń ValuesController, klikając prawym przyciskiem myszy plik w Eksplorator rozwiązań i wybierając pozycję **Usuń.** Teraz Dodaj nowy kontroler w następujący sposób:

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder controllers. Wybierz pozycję **Dodaj** , a następnie wybierz pozycję **kontroler**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

W kreatorze **dodawania kontrolera** nazwij kontroler &quot;ProductsController&quot;. Z listy rozwijanej **szablon** wybierz pozycję **pusty kontroler interfejsu API**. Następnie kliknij przycisk **Dodaj**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Nie jest konieczne umieszczenie kontrolerów w folderze o nazwie controllers. Nazwa folderu nie jest ważna. jest to po prostu wygodny sposób organizowania plików źródłowych.

Kreator **dodawania kontrolera** utworzy plik o nazwie ProductsController.cs w folderze controllers. Jeśli ten plik nie jest jeszcze otwarty, kliknij go dwukrotnie, aby go otworzyć. Dodaj następującą instrukcję **using** :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Dodaj pole, które zawiera wystąpienie **IProductRepository** .

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Wywoływanie `new ProductRepository()` w kontrolerze nie jest najlepszym projektem, ponieważ łączy kontroler z określoną implementacją `IProductRepository`. Aby uzyskać lepsze podejście, zobacz [Korzystanie z programu rozpoznawania zależności interfejsu API sieci Web](../advanced/dependency-injection.md).

## <a name="getting-a-resource"></a>Pobieranie zasobu

Interfejs API ProductStore będzie uwidaczniał kilka &quot;operacji odczytu&quot; jako metody HTTP GET. Każda akcja będzie odpowiadać metodzie w klasie `ProductsController`.

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobierz listę wszystkich produktów | Pobierz | /api/products |
| Pobierz produkt według identyfikatora | Pobierz | *Identyfikator* /API/Products/ |
| Pobierz produkt według kategorii | Pobierz | /API/Products? Category =*Kategoria* |

Aby uzyskać listę wszystkich produktów, należy dodać tę metodę do klasy `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Nazwa metody rozpoczyna się od &quot;Get&quot;, tak więc według Konwencji mapuje się na żądania GET. Ponadto, ponieważ metoda nie ma parametrów, mapuje na identyfikator URI, który nie zawiera *&quot;identyfikatora&quot;* segmentu w ścieżce.

Aby uzyskać produkt według identyfikatora, należy dodać tę metodę do klasy `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Ta nazwa metody rozpoczyna się również od &quot;Get&quot;, ale metoda ma parametr o nazwie *ID*. Ten parametr jest mapowany do &quot;identyfikator&quot; segmentu ścieżki URI. Struktura internetowego interfejsu API ASP.NET automatycznie konwertuje identyfikator do poprawnego typu danych (**int**) dla parametru.

Metoda getproduct zwraca wyjątek typu **HttpResponseException** , jeśli *Identyfikator* jest nieprawidłowy. Ten wyjątek zostanie przetłumaczony przez platformę na błąd 404 (nie można odnaleźć).

Na koniec Dodaj metodę, aby znaleźć produkty według kategorii:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Jeśli identyfikator URI żądania zawiera ciąg zapytania, interfejs API sieci Web próbuje dopasować parametry zapytania do parametrów w metodzie kontrolera. W związku z tym identyfikator URI formularza "API/productss? Category =*Category*" zostanie zmapowany na tę metodę.

## <a name="creating-a-resource"></a>Tworzenie zasobu

Następnie dodamy metodę do klasy `ProductsController`, aby utworzyć nowy produkt. Oto prosta implementacja metody:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Zwróć uwagę na dwie rzeczy dotyczące tej metody:

- Nazwa metody rozpoczyna się od &quot;post...&quot;. Aby utworzyć nowy produkt, klient wysyła żądanie HTTP POST.
- Metoda przyjmuje parametr typu produkt. W interfejsie API sieci Web parametry o typach złożonych są deserializowane od treści żądania. W związku z tym oczekujemy, że klient wyśle serializowaną reprezentację obiektu produktu w formacie XML lub JSON.

Ta implementacja będzie działała, ale nie została ukończona. Najlepiej, aby odpowiedź HTTP obejmowała następujące elementy:

- **Kod odpowiedzi:** Domyślnie Struktura interfejsu API sieci Web ustawia kod stanu odpowiedzi na 200 (OK). Jednak zgodnie z protokołem HTTP/1.1, gdy żądanie POST powoduje utworzenie zasobu, serwer powinien odpowiedzieć ze stanem 201 (utworzony).
- **Lokalizacja:** Gdy serwer tworzy zasób, powinien zawierać identyfikator URI nowego zasobu w nagłówku lokalizacji odpowiedzi.

Interfejs API sieci Web ASP.NET ułatwia manipulowanie komunikatem odpowiedzi HTTP. Ulepszona implementacja:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Zwróć uwagę, że zwracany typ metody to teraz **HttpResponseMessage**. Zwracając **HttpResponseMessage** zamiast produktu, możemy kontrolować szczegóły komunikatu odpowiedzi HTTP, w tym kod stanu i nagłówek lokalizacji.

Metoda **onresponse** tworzy **HttpResponseMessage** i automatycznie zapisuje serializowaną reprezentację obiektu produktu w treści komunikatu odpowiedzi.

> [!NOTE]
> Ten przykład nie sprawdza poprawności `Product`. Aby uzyskać informacje na temat weryfikacji modelu, zobacz [Walidacja modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

## <a name="updating-a-resource"></a>Aktualizowanie zasobu

Aktualizacja produktu z funkcją PUT jest prosta:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Nazwa metody rozpoczyna się od &quot;Put...&quot;, więc interfejs API sieci Web dopasowuje go do UMIESZCZAnia żądań. Metoda przyjmuje dwa parametry, identyfikator produktu i zaktualizowany produkt. Parametr *ID* jest pobierany ze ścieżki identyfikatora URI, a parametr *produktu* jest deserializowany od treści żądania. Domyślnie Struktura interfejsu API sieci Web ASP.NET przyjmuje proste typy parametrów z tras i typów złożonych z treści żądania.

## <a name="deleting-a-resource"></a>Usuwanie zasobu

Aby usunąć zasób, zdefiniuj "Usuwanie..." Method.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Jeśli żądanie usunięcia powiedzie się, może zwrócić status 200 (OK) o treści jednostki opisującej stan; stan 202 (zaakceptowany), Jeśli usunięcie nadal oczekuje; lub status 204 (brak zawartości) bez treści jednostki. W tym przypadku Metoda `DeleteProduct` ma `void` typ zwracany, więc interfejs API sieci Web ASP.NET automatycznie przetłumaczy ten element na kod stanu 204 (brak zawartości).

---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Tworzenie punktu końcowego OData V3 przy użyciu interfejsu Web API 2 | Microsoft Docs
author: MikeWasson
description: Protokół Open Data Protocol (OData) to protokół dostępu do danych dla sieci Web. Usługa OData zapewnia jednolity sposób tworzenia struktur danych, wykonywania zapytań dotyczących danych i manipulowania danymi...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600432"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Tworzenie punktu końcowego OData V3 przy użyciu interfejsu Web API 2

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Protokół Open Data Protocol](http://www.odata.org/) (OData) to protokół dostępu do danych dla sieci Web. Usługa OData zapewnia jednolity sposób tworzenia struktur danych, wykonywania zapytań dotyczących danych i manipulowania danymi za pomocą operacji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie). Usługa OData obsługuje zarówno formaty AtomPub (XML), jak i JSON. Usługa OData definiuje również sposób ujawniania metadanych dotyczących danych. Klienci mogą używać metadanych do wykrywania informacji o typie i relacji dla zestawu danych.
>
> Interfejs API sieci Web ASP.NET ułatwia tworzenie punktu końcowego OData dla zestawu danych. Można kontrolować dokładne operacje OData obsługiwane przez punkt końcowy. Można hostować wiele punktów końcowych OData, obok punktów końcowych innych niż OData. Masz pełną kontrolę nad modelem danych, logiką biznesową i warstwą danych.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Web API 2
> - Usługa OData w wersji 3
> - Entity Framework 6
> - [Serwer proxy debugowania sieci Web programu Fiddler (opcjonalnie)](http://www.fiddler2.com)
>
> Obsługa protokołu OData interfejsu API sieci Web została dodana w [ASP.NET and Web Tools 2012,2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Jednak w tym samouczku użyto szkieletu, który został dodany w Visual Studio 2013.

W tym samouczku utworzysz prosty punkt końcowy OData, który mogą wykonywać zapytania na klientach. Zostanie również utworzony C# klient dla punktu końcowego. Po ukończeniu tego samouczka następny zestaw samouczków pokazuje, jak dodać więcej funkcji, w tym relacje między jednostkami, akcje i $expand/$select.

- [Tworzenie projektu programu Visual Studio](#create-project)
- [Dodaj model jednostki](#add-model)
- [Dodawanie kontrolera OData](#add-controller)
- [Dodawanie modelu EDM i trasy](#edm)
- [Wypełnianie bazy danych (opcjonalnie)](#seed-db)
- [Eksplorowanie punktu końcowego OData](#explore)
- [Formaty serializacji OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

W tym samouczku utworzysz punkt końcowy OData, który obsługuje podstawowe operacje CRUD. Punkt końcowy uwidacznia pojedynczy zasób, listę produktów. W kolejnych samouczkach zostanie dodana większa funkcja.

Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie startowej. Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł Wizualizacja C# . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Wybierz szablon **aplikacji sieci Web ASP.NET** .

![](creating-an-odata-endpoint/_static/image1.png)

W oknie dialogowym **Nowy projekt ASP.NET** wybierz **pusty** szablon. W obszarze &quot;Dodaj foldery i podstawowe odwołania dla...&quot;, sprawdź **internetowy interfejs API**. Kliknij przycisk **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Dodaj model jednostki

*Model* to obiekt, który reprezentuje dane w aplikacji. W tym samouczku potrzebny jest model, który reprezentuje produkt. Model odpowiada typowi obiektu OData.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder modele. Z menu kontekstowego wybierz pozycję **Dodaj** , a następnie wybierz pozycję **Klasa**.

![](creating-an-odata-endpoint/_static/image3.png)

W oknie dialogowym **Dodaj nowy** element nazwij klasę &quot;&quot;produktu.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Według Konwencji klasy modelu są umieszczane w folderze models. Nie musisz stosować tej Konwencji we własnych projektach, ale użyjemy jej w tym samouczku.

W pliku Product.cs Dodaj następującą definicję klasy:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Właściwość ID będzie kluczem jednostki. Klienci mogą badać produkty według identyfikatora. To pole będzie również kluczem podstawowym w bazie danych zaplecza.

Kompiluj projekt teraz. W następnym kroku użyjemy szkieletu programu Visual Studio, który używa odbicia, aby znaleźć typ produktu.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Dodawanie kontrolera OData

*Kontroler* jest klasą, która obsługuje żądania HTTP. Należy zdefiniować osobny kontroler dla każdego zestawu jednostek w usłudze OData. W tym samouczku utworzymy jeden kontroler.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers. Wybierz pozycję **Dodaj** , a następnie wybierz pozycję **kontroler**.

![](creating-an-odata-endpoint/_static/image5.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz kontroler &quot;Web API 2 OData z akcjami, używając Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

W oknie dialogowym **Dodawanie kontrolera** Nazwij kontroler "ProductsController". Zaznacz pole wyboru &quot;Użyj akcji kontrolera asynchronicznego&quot;. Z listy rozwijanej **model** wybierz klasę Product (produkt).

![](creating-an-odata-endpoint/_static/image7.png)

Kliknij przycisk **nowy kontekst danych..** . Pozostaw domyślną nazwę dla typu kontekstu danych, a następnie kliknij przycisk **Dodaj**.

![](creating-an-odata-endpoint/_static/image8.png)

Kliknij przycisk Dodaj w oknie dialogowym Dodaj kontroler, aby dodać kontroler.

![](creating-an-odata-endpoint/_static/image9.png)

Uwaga: Jeśli pojawi się komunikat o błędzie z informacją &quot;Wystąpił błąd podczas pobierania typu...&quot;, upewnij się, że projekt programu Visual Studio został skompilowany po dodaniu klasy Product. Szkielet używa odbicia, aby znaleźć klasę.

![](creating-an-odata-endpoint/_static/image10.png)

Tworzenie szkieletu powoduje dodanie dwóch plików kodu do projektu:

- Products.cs definiuje kontroler internetowego interfejsu API, który implementuje punkt końcowy OData.
- ProductServiceContext.cs udostępnia metody do wykonywania zapytań względem źródłowej bazy danych przy użyciu Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Dodawanie modelu EDM i trasy

W Eksplorator rozwiązań rozwiń węzeł aplikacja\_folder Start i Otwórz plik o nazwie WebApiConfig.cs. Ta klasa zawiera kod konfiguracyjny dla internetowego interfejsu API. Zastąp ten kod następującym:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Ten kod wykonuje dwie czynności:

- Tworzy Entity Data Model (EDM) dla punktu końcowego OData.
- Dodaje trasę dla punktu końcowego.

EDM to abstrakcyjny model danych. Modelu EDM służy do tworzenia dokumentu metadanych i definiowania identyfikatorów URI dla usługi. **ODataConventionModelBuilder** tworzy obiekt EDM przy użyciu zestawu domyślnych konwencji nazewnictwa modelu EDM. To podejście wymaga najmniejszego kodu. Jeśli chcesz mieć większą kontrolę nad modelu EDM, możesz użyć klasy **ODataModelBuilder** do utworzenia modelu EDM przez dodanie właściwości, kluczy i właściwości nawigacji jawnie.

Metoda **EntitySet** dodaje jednostkę ustawioną do modelu EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Ciąg "Products" definiuje nazwę zestawu jednostek. Nazwa kontrolera musi być zgodna z nazwą zestawu jednostek. W tym samouczku zestaw jednostek ma nazwę "produkty", a kontroler ma nazwę `ProductsController`. Jeśli nazwa zestawu jednostek to "ProductSet", należy nazwać kontroler `ProductSetController`. Należy pamiętać, że punkt końcowy może mieć wiele zestawów jednostek. Wywołaj element **EntitySet&lt;t&gt;** dla każdego zestawu jednostek, a następnie zdefiniuj odpowiedni kontroler.

Metoda **MapODataRoute** dodaje trasę do punktu końcowego OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Pierwszy parametr jest przyjazną nazwą dla trasy. Klienci usługi nie widzą tej nazwy. Drugi parametr jest prefiksem identyfikatora URI dla punktu końcowego. Mając ten kod, identyfikator URI dla zestawu jednostek Products to http://<em>hostname</em>/OData/Products. Aplikacja może mieć więcej niż jeden punkt końcowy OData. Dla każdego punktu końcowego Wywołaj <strong>MapODataRoute</strong> i podaj unikatową nazwę trasy i unikatowy prefiks identyfikatora URI.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Wypełnianie bazy danych (opcjonalnie)

W tym kroku będziesz używać Entity Framework do wypełniania bazy danych niektórymi danymi testowymi. Ten krok jest opcjonalny, ale pozwala na natychmiastowe przetestowanie punktu końcowego OData.

W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Spowoduje to dodanie folderu o nazwie migrations i pliku kodu o nazwie Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Otwórz ten plik i Dodaj następujący kod do metody `Configuration.Seed`.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

W oknie Konsola Menedżera pakietów wprowadź następujące polecenia:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Te polecenia generują kod, który tworzy bazę danych, a następnie wykonuje ten kod.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Eksplorowanie punktu końcowego OData

W tej sekcji użyjemy [serwera proxy debugowania sieci Web programu Fiddler](http://www.fiddler2.com) do wysyłania żądań do punktu końcowego i badania komunikatów odpowiedzi. Pomoże to zrozumieć możliwości punktu końcowego OData.

W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie. Domyślnie program Visual Studio otwiera przeglądarkę w celu `http://localhost:*port*`, gdzie *port* jest numerem portu skonfigurowanym w ustawieniach projektu.

Numer portu można zmienić w ustawieniach projektu. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Właściwości**. W oknie właściwości wybierz pozycję **Sieć Web**. Wprowadź numer portu w polu **adres URL projektu**.

### <a name="service-document"></a>Dokument usługi

*Dokument usługi* zawiera listę zestawów jednostek dla punktu końcowego OData. Aby uzyskać dokument usługi, Wyślij żądanie GET do głównego identyfikatora URI usługi.

Korzystając z programu Fiddler, wprowadź następujący identyfikator URI na karcie **układacz** : `http://localhost:port/odata/`, gdzie *port* jest numerem portu.

![](creating-an-odata-endpoint/_static/image13.png)

Kliknij przycisk **Execute (wykonaj** ). Programu Fiddler wysyła do aplikacji żądanie HTTP GET. Odpowiedź powinna zostać wyświetlona na liście sesje sieci Web. Jeśli wszystko działa, kod stanu będzie 200.

![](creating-an-odata-endpoint/_static/image14.png)

Kliknij dwukrotnie odpowiedź na liście sesji sieci Web, aby wyświetlić szczegóły komunikatu odpowiedzi na karcie inspektorzy.

![](creating-an-odata-endpoint/_static/image15.png)

Nieprzetworzony komunikat odpowiedzi HTTP powinien wyglądać podobnie do poniższego:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Domyślnie interfejs API sieci Web zwraca dokument usługi w formacie AtomPub. Aby poprosić o dane JSON, Dodaj następujący nagłówek do żądania HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Teraz odpowiedź HTTP zawiera ładunek JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Dokument metadanych usługi

W *dokumencie metadanych usługi* opisano model danych usługi przy użyciu języka XML o nazwie język definicji schematu koncepcyjnego (CSDL). Dokument metadanych przedstawia strukturę danych w usłudze i może służyć do generowania kodu klienta.

Aby uzyskać dokument metadanych, Wyślij żądanie GET do `http://localhost:port/odata/$metadata`. Oto metadane dla punktu końcowego wyświetlanego w tym samouczku.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Zestaw jednostek

Aby uzyskać zestaw jednostek Products, Wyślij żądanie GET do `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Jednostka

Aby uzyskać indywidualny produkt, Wyślij żądanie GET do `http://localhost:port/odata/Products(1)`, gdzie "1" jest IDENTYFIKATORem produktu.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formaty serializacji OData

Usługa OData obsługuje kilka formatów serializacji:

- Atom pub (XML)
- Notacja JSON "Light" (wprowadzona w protokole OData V3)
- JSON "verbose" (OData v2)

Domyślnie interfejs API sieci Web używa formatu AtomPubJSON "Light".

Aby uzyskać format AtomPub, należy ustawić nagłówek Accept na wartość "Application/Atom + XML". Oto przykład treści odpowiedzi:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Można zobaczyć jedną z oczywistych niekorzystnych niedogodności w formacie Atom: jest to znacznie bardziej szczegółowe niż oświetlenie JSON. Jeśli jednak klient ma świadomość AtomPub, klient może preferować ten format w formacie JSON.

Oto wersja uproszczona JSON tej samej jednostki:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Format oświetlenia JSON został wprowadzony w wersji 3 protokołu OData. W celu zapewnienia zgodności z poprzednimi wersjami klient może zażądać starszego formatu JSON "verbose". Aby zażądać pełnego kodu JSON, ustaw nagłówek Accept na `application/json;odata=verbose`. Oto pełna wersja:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Ten format umożliwia przekazanie większej liczby metadanych w treści odpowiedzi, co może zwiększyć znaczne obciążenie całej sesji. Ponadto dodaje poziom pośredni przez Zawijanie obiektu we właściwości o nazwie "d".

## <a name="next-steps"></a>Następne kroki

- [Dodawanie relacji jednostek](working-with-entity-relations.md)
- [Dodawanie akcji OData](odata-actions.md)
- [Wywoływanie usługi OData z klienta platformy .NET](calling-an-odata-service-from-a-net-client.md)

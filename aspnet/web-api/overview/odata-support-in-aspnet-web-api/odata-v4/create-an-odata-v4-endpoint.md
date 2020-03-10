---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Tworzenie punktu końcowego OData v4 przy użyciu ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: Protokół Open Data Protocol (OData) to protokół dostępu do danych dla sieci Web. Usługa OData zapewnia jednolity sposób wykonywania zapytań i manipulowania zestawami danych za pomocą operacji CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598736"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Tworzenie punktu końcowego OData v4 przy użyciu interfejsu API sieci Web ASP.NET 

> Protokół Open Data Protocol (OData) to protokół dostępu do danych dla sieci Web. Usługa OData zapewnia jednolity sposób wykonywania zapytań i manipulowania zestawami danych za pomocą operacji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).
>
> Interfejs API sieci Web ASP.NET obsługuje zarówno v3, jak i v4 protokołu. Można nawet korzystać z punktu końcowego v4, który działa obok siebie z punktem końcowym v3.
>
> W tym samouczku pokazano, jak utworzyć punkt końcowy protokołu OData w wersji 4, który obsługuje operacje CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
> - Internetowy interfejs API 5,2
> - OData 4
> - Visual Studio 2017 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - .NET 4.7.2
>
> ## <a name="tutorial-versions"></a>Wersje samouczka
>
> W przypadku usługi OData w wersji 3 zobacz [Tworzenie punktu końcowego OData V3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

W programie Visual Studio w menu **plik** wybierz pozycję **Nowy** **projekt**&gt;.

Rozwiń węzeł **zainstalowane** &gt; **Visual C#**  &gt; **Web**i wybierz szablon **aplikacja sieci Web ASP.NET (.NET Framework)** . Nazwij projekt &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Kliknij przycisk **OK**.

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Wybierz **pusty** szablon. W obszarze **Dodaj foldery i podstawowe odwołania dla:** wybierz pozycję **interfejs API sieci Web**. Kliknij przycisk **OK**.

## <a name="install-the-odata-packages"></a>Instalowanie pakietów OData

W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** &gt; **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

To polecenie powoduje zainstalowanie najnowszych pakietów NuGet OData.

## <a name="add-a-model-class"></a>Dodawanie klasy modelu

*Model* to obiekt, który reprezentuje jednostkę danych w aplikacji.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele. Z menu kontekstowego wybierz pozycję **Dodaj** **klasę**&gt;.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Według Konwencji klasy modelu są umieszczane w folderze modele, ale nie trzeba stosować tej Konwencji we własnych projektach.

Nadaj klasie nazwę `Product`. W pliku Product.cs Zastąp kod standardowy następującymi:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

Właściwość `Id` jest kluczem jednostki. Klienci mogą wysyłać zapytania o jednostki według klucza. Na przykład aby uzyskać produkt o IDENTYFIKATORze 5, identyfikator URI jest `/Products(5)`. Właściwość `Id` będzie również kluczem podstawowym w bazie danych zaplecza.

## <a name="enable-entity-framework"></a>Włącz Entity Framework

W tym samouczku użyjemy Code First Entity Framework (EF), aby utworzyć bazę danych zaplecza.

> [!NOTE]
> Protokół OData interfejsu API sieci Web nie wymaga EF. Użyj dowolnej warstwy dostępu do danych, która może przetłumaczyć jednostki bazy danych na modele.

Najpierw zainstaluj pakiet NuGet dla programu EF. W menu **Narzędzia** wybierz kolejno pozycje **menedżer pakietów NuGet** &gt; **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Otwórz plik Web. config i Dodaj następującą sekcję w elemencie **Configuration** , po elemencie **configSections** .

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

To ustawienie powoduje dodanie parametrów połączenia dla bazy danych LocalDB. Ta baza danych będzie używana podczas lokalnego uruchamiania aplikacji.

Następnie Dodaj klasę o nazwie `ProductsContext` do folderu models:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

W konstruktorze `"name=ProductsContext"` podaje nazwę ciągu połączenia.

## <a name="configure-the-odata-endpoint"></a>Konfigurowanie punktu końcowego OData

Otwórz aplikację pliku\_Start/WebApiConfig. cs. Dodaj następujące instrukcje **using** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Następnie Dodaj następujący kod do metody **register** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Ten kod wykonuje dwie czynności:

- Tworzy Entity Data Model (EDM).
- Dodaje trasę.

EDM to abstrakcyjny model danych. Modelu EDM służy do tworzenia dokumentu metadanych usługi. Klasa **ODataConventionModelBuilder** tworzy obiekt EDM przy użyciu domyślnych konwencji nazewnictwa. To podejście wymaga najmniejszego kodu. Jeśli chcesz mieć większą kontrolę nad modelu EDM, możesz użyć klasy **ODataModelBuilder** do utworzenia modelu EDM przez dodanie właściwości, kluczy i właściwości nawigacji jawnie.

*Trasa* informuje interfejs API sieci Web, jak kierować żądania HTTP do punktu końcowego. Aby utworzyć trasę protokołu OData w wersji 4, wywołaj metodę rozszerzenia **MapODataServiceRoute** .

Jeśli aplikacja ma wiele punktów końcowych OData, utwórz oddzielną trasę dla każdej z nich. Nadaj każdej trasie unikatową nazwę i prefiks trasy.

## <a name="add-the-odata-controller"></a>Dodawanie kontrolera OData

*Kontroler* jest klasą, która obsługuje żądania HTTP. Należy utworzyć oddzielny kontroler dla każdego zestawu jednostek w usłudze OData. W tym samouczku utworzysz jeden kontroler dla jednostki `Product`.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz polecenie **Dodaj** **klasę**&gt;. Nadaj klasie nazwę `ProductsController`.

> [!NOTE]
> Wersja tego samouczka dla protokołu OData V3 używa tworzenia szkieletu **kontrolera** . Obecnie nie ma szkieletu dla protokołu OData v4.

Zastąp kod standardowy w ProductsController.cs poniższymi.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Kontroler używa klasy `ProductsContext`, aby uzyskać dostęp do bazy danych przy użyciu EF. Należy zauważyć, że kontroler zastępuje metodę **Dispose** , aby usunąć **ProductsContext**.

Jest to punkt początkowy dla kontrolera. Następnie dodamy metody dla wszystkich operacji CRUD.

## <a name="query-the-entity-set"></a>Zapytanie dotyczące zestawu jednostek

Dodaj następujące metody do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Bezparametrowa wersja metody `Get` zwraca całą kolekcję produktów. Metoda `Get` z parametrem *klucza* wyszukuje produkt według jego klucza (w tym przypadku właściwości `Id`).

Atrybut **[EnableQuery]** umożliwia klientom modyfikowanie zapytania przy użyciu opcji zapytania, takich jak $filter, $sort i $Page. Aby uzyskać więcej informacji, zobacz [Obsługa opcji zapytania OData](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Dodawanie jednostki do zestawu jednostek

Aby umożliwić klientom Dodawanie nowego produktu do bazy danych programu, Dodaj następującą metodę do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Aktualizuj jednostkę

Usługa OData obsługuje dwie różne semantyki do aktualizowania jednostki, poprawki i UMIESZCZAnia.

- Poprawka wykonuje aktualizację częściową. Klient określa tylko właściwości do zaktualizowania.
- Parametr PUT zastępuje całą jednostkę.

Wadą jest to, że klient musi wysyłać wartości dla wszystkich właściwości w jednostce, w tym wartości, które nie są zmieniane. [Specyfikacja OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) wskazuje, że poprawka jest preferowana.

W każdym przypadku jest to kod dla obu metod PATCH i PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

W przypadku poprawki kontroler używa typu **Delta&lt;t&gt;** do śledzenia zmian.

## <a name="delete-an-entity"></a>Usuwanie jednostki

Aby umożliwić klientom usuwanie produktu z bazy danych programu, Dodaj następującą metodę do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]

---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Wprowadzenie do usługi ASP.NET Web API 2 (C#) — ASP.NET 4. x
author: MikeWasson
description: Samouczek z kodem. Użyj interfejsu API sieci Web ASP.NET, aby utworzyć internetowy interfejs API, który zwraca listę produktów.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084055"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a>Wprowadzenie do usługi ASP.NET Web API 2 (C#)

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

W tym samouczku użyjesz internetowego interfejsu API platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów.

Protokół HTTP nie jest używany tylko do obsługi stron internetowych. Protokół HTTP to również zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest prosty, elastyczny i uniwersalny. Prawie każda platforma obsługuje bibliotekę HTTP, więc usługi HTTP mogą być stosowane dla szerokiej gamy klientów takich jak przeglądarki, urządzenia przenośne czy tradycyjne aplikacje komputerowe.

Internetowy interfejs API platformy ASP.NET to architektura służąca do tworzenia internetowych interfejsów API w programie .NET Framework. 

## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- Internetowy interfejs API 2

Aby uzyskać nowszą wersję tego samouczka [, zobacz Tworzenie internetowego interfejsu API za pomocą ASP.NET Core i programu Visual Studio dla systemu Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) .

## <a name="create-a-web-api-project"></a>Tworzenie projektu interfejsu API sieci Web

W tym samouczku użyjesz internetowego interfejsu API platformy ASP.NET do tworzenia internetowego interfejsu API, który zwraca listę produktów. Strona sieci Web frontonu używa technologii jQuery do wyświetlania wyników.

![](tutorial-your-first-web-api/_static/image1.png)

Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** . Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web ASP.NET**. Nadaj projektowi nazwę "ProductsApp" i kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

W oknie dialogowym **Nowy projekt ASP.NET** wybierz **pusty** szablon. W obszarze &quot;Dodaj foldery i podstawowe odwołania dla&quot;Sprawdź, czy jest **internetowy interfejs API**. Kliknij przycisk **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Projekt interfejsu API sieci Web można również utworzyć przy użyciu szablonu&quot; &quot;Web API. Szablon internetowego interfejsu API używa platformy ASP.NET MVC do udostępniania stron pomocy do interfejsu API. Używam pustego szablonu na potrzeby tego samouczka, ponieważ chcę pokazać internetowy interfejs API bez platformy MVC. Ogólnie rzecz biorąc, nie trzeba znać platformy ASP.NET MVC, aby korzystać z internetowego interfejsu API.

## <a name="adding-a-model"></a>Dodawanie modelu

*Model* to obiekt, który reprezentuje dane w aplikacji. Internetowy interfejs API platformy ASP.NET może automatycznie zserializować model do formatu JSON, XML lub innego, a następnie wpisać te dane serializowane w treści komunikatu odpowiedzi HTTP. Jeśli klient może odczytać format serializacji, może również wykonywać deserializację obiektu. Większość klientów może analizować albo format XML, albo JSON. Ponadto klient może wskazać pożądany format, ustawiając nagłówek Accept w komunikacie żądania HTTP.

Zacznijmy od utworzenia prostego modelu reprezentującego produkt.

Jeśli Eksplorator rozwiązań nie jest jeszcze widoczna, kliknij menu **Widok** i wybierz pozycję **Eksplorator rozwiązań**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele. Z menu kontekstowego wybierz pozycję **Dodaj** , a następnie wybierz pozycję **Klasa**.

![](tutorial-your-first-web-api/_static/image4.png)

Nazwij klasę &quot;&quot;produktu. Dodaj następujące właściwości do klasy `Product`.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Dodawanie kontrolera

W interfejsie API sieci Web *kontroler* jest obiektem, który obsługuje żądania HTTP. Dodamy kontroler, który może zwrócić listę produktów lub pojedynczy produkt określony przez identyfikator.

> [!NOTE]
> Jeśli używasz platformy ASP.NET MVC, znasz już kontrolery. Kontrolery internetowego interfejsu API są podobne do kontrolerów MVC, ale dziedziczą klasę **ApiController** zamiast klasy **Controller** .

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder controllers. Wybierz pozycję **Dodaj** , a następnie wybierz pozycję **kontroler**.

![](tutorial-your-first-web-api/_static/image5.png)

W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **kontroler interfejsu API sieci Web — puste**. Kliknij pozycję **Add** (Dodaj).

![](tutorial-your-first-web-api/_static/image6.png)

W oknie dialogowym **Dodawanie kontrolera** nazwij kontroler &quot;ProductsController&quot;. Kliknij pozycję **Add** (Dodaj).

![](tutorial-your-first-web-api/_static/image7.png)

Funkcja tworzenia szkieletów utworzy plik o nazwie ProductsController.cs w folderze kontrolerów.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Kontroler nie musi być umieszczany w folderze kontrolerów (Controllers). Nazwy folderów ułatwiają tylko organizowanie plików źródłowych.

Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie, aby go otworzyć. Zastąp kod w tym pliku zgodnie z poniższym przykładem:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

W tym przykładzie w celu uproszczenia produkty są przechowywane w stałej tablicy wewnątrz klasy kontrolera. Oczywiście w rzeczywistej aplikacji przesyłane jest zapytanie do bazy danych lub innego zewnętrznego źródła danych.

Kontroler definiuje dwie metody, które zwracają produkty:

- Metoda `GetAllProducts` zwraca całą listę produktów jako typ **&gt;produktu IEnumerable&lt;** .
- Metoda `GetProduct` wyszukuje pojedynczy produkt według jego identyfikatora.

Gotowe. Masz działający internetowy interfejs API. Każda metoda na kontrolerze odnosi się do co najmniej jednego identyfikatora URI:

| Controller — Metoda | Identyfikator URI |
| --- | --- |
| GetAllProducts | /api/products |
| Getproduct | *Identyfikator* /API/Products/ |

Dla metody `GetProduct` *Identyfikator* identyfikatora URI jest symbolem zastępczym. Na przykład aby uzyskać produkt o IDENTYFIKATORze 5, identyfikator URI jest `api/products/5`.

Aby uzyskać więcej informacji o sposobie, w jaki interfejs API sieci Web kieruje żądania HTTP do metod kontrolera, zobacz [Routing in Web api ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Wywoływanie interfejsu API sieci Web przy użyciu języków JavaScript i jQuery

W tej sekcji dodamy stronę HTML używającą technologii AJAX do wywołania internetowego interfejsu API. Użyjemy technologii jQuery do wykonania wywołań AJAX oraz do zaktualizowania wyników na stronie.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj**, a następnie wybierz pozycję **nowy element**.

![](tutorial-your-first-web-api/_static/image9.png)

W oknie dialogowym **Dodaj nowy element** wybierz węzeł **sieci Web** w obszarze **Wizualizacja C#** , a następnie wybierz element **strony HTML** . Nazwij stronę &quot;index. html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Zamień zawartość pliku na następującą:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Istnieje kilka sposobów uzyskania biblioteki jQuery. W tym przykładzie użyto usługi [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Można go również pobrać z [http://jquery.com/](http://jquery.com/), a szablon projektu "Web API" ASP.NET "również obejmuje jQuery.

### <a name="getting-a-list-of-products"></a>Pobieranie listy produktów

Aby uzyskać listę produktów, Wyślij żądanie HTTP GET w celu &quot;&quot;/API/Products.

Funkcja jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) wysyła żądanie AJAX. Odpowiedź zawiera tablicę obiektów JSON. Funkcja `done` określa wywołanie zwrotne, które jest wywoływane, jeśli żądanie zakończy się pomyślnie. W wywołaniu zwrotnym aktualizujemy model DOM informacjami o produkcie.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Pobieranie produktu według identyfikatora

Aby uzyskać produkt według identyfikatora, Wyślij żądanie HTTP GET do &quot;*Identyfikator* /API/Products/&quot;, gdzie *ID* jest identyfikatorem produktu.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Nadal dzwonimy do `getJSON`, aby wysłać żądanie AJAX, ale ten czas jest umieszczany w IDENTYFIKATORze URI żądania. Odpowiedź na to żądanie jest reprezentacją JSON jednego produktu.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Naciśnij klawisz F5, aby rozpocząć debugowanie aplikacji. Strona internetowa powinna wyglądać następująco:

![](tutorial-your-first-web-api/_static/image11.png)

Aby uzyskać produkt według identyfikatora, wprowadź identyfikator i kliknij przycisk Wyszukaj:

![](tutorial-your-first-web-api/_static/image12.png)

W przypadku wprowadzenia nieprawidłowego identyfikatora serwer zwróci błąd HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Używanie klawisza F12 do wyświetlania żądania i odpowiedzi HTTP

Podczas pracy z usługą HTTP może być bardzo przydatne, aby wyświetlić żądania HTTP i komunikaty odpowiedzi. Można to zrobić w programie Internet Explorer 9 przy użyciu narzędzi deweloperskich uruchamianych za pomocą klawisza F12. W programie Internet Explorer 9 Naciśnij klawisz **F12** , aby otworzyć narzędzia. Kliknij kartę **Sieć** , a następnie naciśnij pozycję **Rozpocznij przechwytywanie**. Teraz wróć do strony sieci Web, a następnie naciśnij klawisz **F5** , aby ponownie załadować stronę sieci Web. Program Internet Explorer będzie przechwytywać ruch HTTP między przeglądarką a serwerem internetowym. Widok podsumowania pokazuje cały ruch sieciowy dla strony:

![](tutorial-your-first-web-api/_static/image14.png)

Zlokalizuj wpis dla względnego identyfikatora URI "api/products//". Wybierz ten wpis, a następnie kliknij pozycję **Przejdź do widoku szczegółowego**. Widok szczegółowy zawiera karty, na których są wyświetlane nagłówki i zawartość żądań oraz odpowiedzi. Na przykład po kliknięciu karty **nagłówki żądań** można zobaczyć, że klient zażądał &quot;Application/JSON&quot; w nagłówku Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Po kliknięciu karty Treść odpowiedzi możesz zobaczyć, jak lista produktów została zserializowana do formatu JSON. Inne przeglądarki mają podobną funkcję. Innym przydatnym narzędziem jest [programu Fiddler](http://www.fiddler2.com/fiddler2/), serwer proxy debugowania sieci Web. Służy on do wyświetlania ruchu HTTP, a także do tworzenia żądań HTTP, co daje pełną kontrolę nad nagłówkami HTTP w żądaniu.

## <a name="see-this-app-running-on-azure"></a>Wyświetlanie aplikacji działającej na platformie Azure

Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo? Pełną wersję aplikacji możesz wdrożyć na koncie platformy Azure — wystarczy kliknąć poniższy przycisk.

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure. Jeśli nie masz jeszcze konta, możesz:

- [Bezpłatnie Otwórz konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — Uzyskaj kredyty, których możesz użyć do wypróbowania płatnych usług platformy Azure, a nawet po ich użyciu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywuj korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) — subskrypcja MSDN daje środki na korzystanie z płatnych usług platformy Azure.

## <a name="next-steps"></a>Następne kroki

- Aby zapoznać się z bardziej kompletnym przykładem usługi HTTP, która obsługuje akcje POST, PUT i DELETE oraz zapisywać dane w bazie danych, zobacz [Używanie interfejsu Web API 2 z Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Aby uzyskać więcej informacji na temat tworzenia płynnych i odpowiadających aplikacji sieci Web na podstawie usługi HTTP, zobacz [ASP.NET aplikacji jednostronicowej](../../../single-page-application/index.md).
- Aby uzyskać informacje na temat sposobu wdrażania projektu sieci Web programu Visual Studio w celu Azure App Service, zobacz [Tworzenie aplikacji sieci web ASP.NET w Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

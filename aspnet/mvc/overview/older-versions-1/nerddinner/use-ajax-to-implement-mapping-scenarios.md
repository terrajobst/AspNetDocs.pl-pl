---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Implementowanie scenariuszy mapowania przy użyciu technologii AJAX | Microsoft Docs
author: microsoft
description: Krok 11 pokazuje, jak zintegrować obsługę mapowania AJAX z naszą aplikacją NerdDinner, umożliwiając użytkownikom, którzy tworzą, edytują lub wyświetlają obiady, aby zobaczyć l...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 7fc90f978b9f9eca511feca70a3c0d02ec69b940
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580116"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Korzystanie z technologii AJAX w celu implementacji scenariuszy mapowania

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Ten krok 11 zawiera bezpłatny [Samouczek dotyczący aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera informacje na temat tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 11 pokazuje, jak zintegrować obsługę mapowania AJAX z naszą aplikacją NerdDinner, umożliwiając użytkownikom, którzy tworzą, edytują lub wyświetlają obiady, aby zobaczyć swoją uroczystą lokalizację.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner krok 11: Integrowanie mapy AJAX

Teraz nasza aplikacja jest nieco bardziej atrakcyjna, dzięki integracji z obsługą mapowania AJAX. Spowoduje to umożliwienie użytkownikom, którzy tworzą, edytują lub wyświetlają obiady, aby zobaczyć swoją lokalizację w formie graficznej.

### <a name="creating-a-map-partial-view"></a>Tworzenie widoku częściowego mapy

Będziemy używać funkcji mapowania w kilku miejscach w naszej aplikacji. Aby zachować nasz kod, będziemy hermetyzować typowe funkcje mapy w ramach jednego szablonu częściowego, który można wykorzystać w wielu akcjach i widokach kontrolera. Zmienimy nazwę tego widoku częściowego "map. ascx" i utworzysz go w katalogu \Views\Dinners.

Możemy utworzyć mapę. ascx częściowo, klikając prawym przyciskiem myszy katalog \Views\Dinners i wybierając polecenie menu Dodaj-&gt;widok. Zmienimy nazwę widoku "map. ascx", Sprawdzamy go jako widok częściowy i wskazują, że będziemy przekazywać do niego silnie wpisaną klasę modelu "obiad":

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Po kliknięciu przycisku "Dodaj" zostanie utworzony nasz częściowy szablon. Następnie zaktualizujemy plik map. ascx do następującej zawartości:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Pierwszy &lt;skryptu&gt; odwołanie do biblioteki mapowania Microsoft Virtual Earth 6,2. Drugi &lt;skrypt&gt; odwołania do pliku map. js, który wkrótce utworzymy, który będzie hermetyzował wspólną logikę mapowania języka JavaScript. &lt;DIV ID = "theMap"&gt; element jest kontenerem HTML używanym przez program Virtual Earth do hostowania mapy.

Następnie mamy osadzony blok &lt;skryptu&gt; zawierający dwie funkcje języka JavaScript specyficzne dla tego widoku. Pierwsza funkcja korzysta z technologii jQuery do utworzenia szkieletu funkcji, która jest wykonywana, gdy strona jest gotowa do uruchomienia skryptu po stronie klienta. Wywołuje funkcję pomocnika LoadMap (), którą zdefiniujemy w naszym pliku skryptu mapy. js w celu załadowania kontrolki mapy platformy Virtual Earth. Druga funkcja to procedura obsługi zdarzeń wywołania zwrotnego, która dodaje kod PIN do mapy, która identyfikuje lokalizację.

Zwróć uwagę na to, jak korzystamy z bloku &lt;% =%&gt; w bloku skryptu po stronie klienta, aby osadzić szerokość i długość obszaru obiadu, który chcemy zmapować na kod JavaScript. Jest to przydatna technika do wyprowadzania wartości dynamicznych, które mogą być używane przez skrypt po stronie klienta (bez konieczności oddzielenia wywołania AJAX z powrotem do serwera w celu pobrania wartości, co sprawia, że jest to szybsze). Bloki &lt;% =%&gt; będą wykonywane, gdy widok jest renderowany na serwerze, a więc dane wyjściowe HTML zakończą się po prostu z osadzonymi wartościami JavaScript (na przykład: var Latitude = 47,64312;).

### <a name="creating-a-mapjs-utility-library"></a>Tworzenie biblioteki narzędzi map. js

Utwórzmy teraz plik map. js, którego można użyć do hermetyzacji funkcji języka JavaScript dla mapy (i implementacji powyższych metod LoadMap i LoadPin). Możemy to zrobić, klikając prawym przyciskiem myszy katalog \Scripts w naszym projekcie, a następnie wybierając polecenie menu "Dodaj&gt;nowy element", wybierz element JScript i nadaj mu nazwę "map. js".

Poniżej znajduje się kod JavaScript, który dodamy do pliku map. js, który będzie współdziałać z platformą Virtual Earth w celu wyświetlenia naszej mapy i dodania do niej numerów PIN dla naszych obiadów:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrowanie mapy za pomocą formularzy Utwórz i edytuj

Teraz integrujemy obsługę mapy z naszymi istniejącymi scenariuszami tworzenia i edytowania. Dobrą nowością jest to, że jest to łatwe w użyciu i nie wymagamy od nas zmiany kodu kontrolera. Ponieważ nasze widoki tworzenia i edytowania współdzielą wspólny widok częściowy "DinnerForm" w celu zaimplementowania interfejsu użytkownika w formie obiadu, możemy dodać mapę w jednym miejscu i mieć nasze scenariusze tworzenia i edytowania.

Wszystko, czego potrzebujemy, aby otworzyć widok częściowy \Views\Dinners\DinnerForm.ascx i zaktualizować go w celu uwzględnienia naszej nowej mapy częściowej. Poniżej znajdują się informacje o zaktualizowanym DinnerForm po dodaniu mapy (Uwaga: elementy formularza HTML są pomijane w poniższym fragmencie kodu dla zwięzłości):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Część DinnerForm powyżej pobiera obiekt typu "DinnerFormViewModel" jako typ modelu (ponieważ wymaga zarówno obiektu obiadu, jak i SelectList do zapełnienia DropDownList krajów). Nasza Mapa częściowa wystarczy do obiektu typu "obiad" jako typu modelu i dlatego, gdy renderuje częściową mapę, przekazujemy tylko podrzędną podwłaściwości DinnerFormViewModel do niej:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Funkcja języka JavaScript dodana do częściowej używa jQuery do dołączania zdarzenia "rozmycie" do pola tekstowego HTML "Address". Prawdopodobnie wysłuchuje zdarzenia "Focus", które są wyzwalane, gdy użytkownik kliknie lub naciśnie kartę w polu tekstowym. Przeciwieństwem jest zdarzenie "rozmycie", które jest wyzwalane, gdy użytkownik opuszcza pole tekstowe. Powyższa procedura obsługi zdarzeń czyści wartości pola tekstowego szerokość i Długość geograficzna, gdy tak się stanie, a następnie wykreśla nową lokalizację adresu na naszej mapie. Procedura obsługi zdarzeń wywołania zwrotnego, która została zdefiniowana w pliku map. js, następnie aktualizuje pola tekstowe długości i szerokości geograficznej w naszym formularzu przy użyciu wartości zwracanych przez program Virtual Earth na podstawie adresu, który podałeś.

Teraz, po ponownym uruchomieniu aplikacji i kliknięciu karty "obiad z hostem" zobaczysz domyślną mapę wyświetlaną wraz z naszymi standardowymi elementami formularza obiadu:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Gdy wpiszesz adres, a następnie tabulator zostanie przesunięty w górę, mapa zostanie dynamicznie zaktualizowana w celu wyświetlenia lokalizacji, a nasza procedura obsługi zdarzeń wypełni pola tekstowe szerokości/długości geograficznej wartościami lokalizacji:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Jeśli zapiszemy nowy obiad i otworzysz go ponownie do edycji, zobaczymy, że lokalizacja mapy zostanie wyświetlona po załadowaniu strony:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Za każdym razem, gdy pole adres zostanie zmienione, zostanie zaktualizowana Mapa i współrzędne szerokości geograficznej.

Teraz, gdy mapa Wyświetla lokalizację obiadu, można także zmienić pola formularza Latitude i długości geograficznej na widoczne jako elementy ukryte (ponieważ mapa jest automatycznie aktualizowana przy każdym wprowadzeniu adresu). W tym celu będziemy przełączać się za pomocą pomocnika HTML HTML. TextBox () w celu użycia metody pomocnika html. Hidden ():

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Teraz nasze formularze są nieco bardziej przyjazne dla użytkownika i nie należy wyświetlać surowej szerokości/długości geograficznej (przy czym są przechowywane z każdym obiadem w bazie danych):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrowanie mapy z widokiem szczegółów

Po zintegrowaniu mapy z naszymi scenariuszami tworzenia i edytowania można także zintegrować ją z naszym scenariuszem szczegółowym. Wszystko, czego potrzebujemy do wywołania &lt;% html. RenderPartial ("map"); %&gt; w widoku szczegółów.

Poniżej przedstawiono sposób, w jaki kod źródłowy do pełnego widoku szczegółów (z integracją mapy) wygląda następująco:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

A teraz, gdy użytkownik nawiguje do adresu URL/Dinners/Details/[ID], zobaczymy szczegółowe informacje o kolacji, lokalizacji obiadu na mapie (wykonane z użyciem wypychanego numeru PIN, gdy wskaźnik myszy zostanie wyświetlony w polu Tytuł obiadu i jego adres), a następnie masz link AJAX do RSVP:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementowanie wyszukiwania lokalizacji w naszej bazie danych i repozytorium

Aby zakończyć naszą implementację technologii AJAX, dodajmy mapę do strony głównej aplikacji, która umożliwia użytkownikom graficzne wyszukiwanie obiadów w sąsiedztwie.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Zaczniemy od zaimplementowania obsługi bazy danych i warstwy repozytorium danych w celu wydajnego przeprowadzenia wyszukiwania za pomocą usługi RADIUS na podstawie lokalizacji. Możemy użyć nowych [funkcji geoprzestrzennych programu sql 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) do wdrożenia tego rozwiązania lub alternatywnie użyć podejścia do funkcji SQL, które Obserwuj Dryden omówione w artykule: [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) i robname Blogged dotyczące używania z usługą with LINQ to SQL tutaj: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Do zaimplementowania tej techniki zostanie otwarty "Eksplorator serwera" w programie Visual Studio, wybierz bazę danych NerdDinner, a następnie kliknij prawym przyciskiem myszy węzeł podrzędne "Functions" w obszarze IT i wybierz polecenie, aby utworzyć nową "funkcję zwracającą wartość skalarną":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Następnie wkleimy następujące funkcje DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Następnie utworzymy nową funkcję zwracającą tabelę w SQL Server, że wywołamy "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Ta funkcja tabeli "NearestDinners" korzysta z funkcji pomocnika DistanceBetween w celu zwrócenia wszystkich obiadów w ciągu 100 kilometrów długości geograficznej i dostosowanej przez nas wartości:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Aby wywołać tę funkcję, najpierw otwieramy projektanta LINQ to SQL, klikając dwukrotnie plik NerdDinner. dbml w naszym katalogu \Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Następnie przeciągniemy funkcje NearestDinners i DistanceBetween do projektanta LINQ to SQL, co spowoduje, że zostaną dodane jako metody klasy LINQ to SQL NerdDinnerDataContext:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Następnie możemy uwidocznić metodę zapytania "FindByLocation" w klasie DinnerRepository, która korzysta z funkcji NearestDinner w celu zwrócenia nadchodzących obiadów, które znajdują się w odległości 100 kilometrów w określonej lokalizacji:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementacja metody akcji wyszukiwania AJAX opartej na notacji JSON

Teraz implementujmy metodę akcji kontrolera, która wykorzystuje nową metodę repozytorium FindByLocation () w celu zwrócenia listy danych obiadu, których można użyć do wypełnienia mapy. Ta metoda akcji zwraca z powrotem dane obiadu w formacie JSON (JavaScript Object Notation), dzięki czemu będzie można łatwo manipulować przy użyciu języka JavaScript na kliencie.

Aby zaimplementować ten element, utworzymy nową klasę "SearchController", klikając prawym przyciskiem myszy katalog \Controllers i wybierając polecenie menu Dodaj kontroler&gt;. Następnie zaimplementujemy metodę akcji "SearchByLocation" w nowej klasie SearchController podobnej do następującej:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Metoda akcji SearchByLocation SearchController wewnętrznie wywołuje metodę FindByLocation na DinnerRepository, aby uzyskać listę pobliskich obiadów. Zamiast zwracać obiekty obiadu bezpośrednio do klienta, ale zamiast tego zwraca obiekty JsonDinner. Klasa JsonDinner uwidacznia podzestaw właściwości obiadu (na przykład: ze względów bezpieczeństwa nie ujawniają nazwiska osób, które mają zaproszenie na obiad). Zawiera również właściwość RSVPCount, która nie istnieje na obiadu — i która jest obliczana dynamicznie przez liczenie obiektów RSVP skojarzonych z określonym obiadem.

Następnie korzystamy z metody pomocnika JSON () w klasie podstawowej kontrolera, aby zwrócić sekwencję obiadów przy użyciu formatu sieci opartego na notacji JSON. JSON to standardowy format tekstowy służący do reprezentowania prostych struktur danych. Poniżej znajduje się przykładowa lista dwóch obiektów JsonDinner w formacie JSON, które wyglądają jak w przypadku powrotu z naszej metody działania:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Wywoływanie metody AJAX opartej na notacji JSON przy użyciu jQuery

Teraz można przystąpić do aktualizowania strony głównej aplikacji NerdDinner, aby używać metody akcji SearchByLocation SearchController. Aby to zrobić, otworzymy szablon widoku/Views/Home/Index.aspx i zaktualizujesz go do postaci TextBox, przycisku wyszukiwania, naszej mapy i &lt;DIV&gt; elementu o nazwie dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Następnie możemy dodać dwie funkcje JavaScript do strony:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Pierwsza funkcja JavaScript ładuje mapę podczas pierwszego ładowania strony. Druga funkcja JavaScript dzieli na przycisk programu obsługi zdarzeń w języku JavaScript. Po naciśnięciu przycisku wywołuje funkcję języka JavaScript FindDinnersGivenLocation (), którą dodamy do naszego pliku map. js:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Ta mapowanie wywołań funkcji FindDinnersGivenLocation (). Znajdź () w kontrolce Virtual Earth, aby wyśrodkować ją w podanej lokalizacji. Gdy usługa mapy usługi Virtual Earth zwróci wartość, mapa. Metoda Find () wywołuje metodę wywołania zwrotnego callbackUpdateMapDinners, która została przez nas przeniesiona jako argument końcowy.

Metoda callbackUpdateMapDinners () to miejsce, w którym wykonywane są rzeczywiste prace. Używa metody pomocnika $. post () platformy jQuery do wykonania wywołania AJAX do metody akcji SearchByLocation () SearchController (), przekazując ją do szerokości i długości geograficznej nowo wyśrodkowanej mapy. Definiuje wbudowaną funkcję, która będzie wywoływana po zakończeniu metody pomocnika $. post (), a wyniki obiadu w formacie JSON zwrócone z metody akcji SearchByLocation () zostaną przesłane przy użyciu zmiennej o nazwie "obiady". Następnie wykonuje instrukcję foreach dla każdego zwróconego obiadu i używa szerokości i długości geograficznej obiadu oraz innych właściwości, aby dodać nowy numer PIN na mapie. Dodaje także wpis z obiadu do listy HTML obiadów z prawej strony mapy. Następnie tworzy okablowanie, aby wyświetlić szczegółowe informacje o kolacji dla wszystkich pinezki i listę HTML, dzięki czemu szczegóły dotyczące obiadu są wyświetlane po umieszczeniu na nim wskaźnika myszy:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

A teraz, gdy uruchomimy aplikację i odwiedź stronę główną, zostanie wyświetlona mapa. Gdy wprowadzimy nazwę miasta, mapa będzie wyświetlać nadchodzące obiady w najbliższym czasie:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Umieszczenie wskaźnika na obiadu spowoduje wyświetlenie szczegółowych informacji o nim.

Kliknięcie tytułu obiadu w dymku lub po prawej stronie na liście HTML spowoduje przejście do obiadu, a następnie opcjonalnego protokołu RSVP dla:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Następny krok

Teraz wdrożono wszystkie funkcje aplikacji aplikacji NerdDinner. Teraz przyjrzyjmy się sposobom, w jaki możemy włączyć zautomatyzowane testowanie jednostkowe.

> [!div class="step-by-step"]
> [Poprzednie](use-ajax-to-deliver-dynamic-updates.md)
> [dalej](enable-automated-unit-testing.md)

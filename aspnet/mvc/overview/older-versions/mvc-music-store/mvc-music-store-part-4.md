---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Część 4: modele i dostęp do danych | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 4 obejmuje modele i dostęp do danych.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559676"
---
# <a name="part-4-models-and-data-access"></a>Część 4. Modele i dostęp do danych

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 4 obejmuje modele i dostęp do danych.

Do tej pory właśnie przekazano "fikcyjne dane" z naszych kontrolerów do naszego szablonu widoku. Teraz jesteśmy gotowi do podłączania rzeczywistej bazy danych. W tym samouczku opisano sposób korzystania z wersji SQL Server Compact (często nazywanej standardem SQL CE) jako aparatu bazy danych. SQL CE to bezpłatna, osadzona baza danych oparta na plikach, która nie wymaga żadnej instalacji ani konfiguracji, co sprawia, że jest ona naprawdę wygodna do lokalnego tworzenia.

## <a name="database-access-with-entity-framework-code-first"></a>Dostęp do bazy danych z kodem Entity Framework — pierwszy

Użyjemy obsługi Entity Framework (EF), która jest dołączona do projektów ASP.NET MVC 3 do wykonywania zapytań i aktualizacji bazy danych. Dr to elastyczny interfejs API danych relacyjnego mapowania obiektów (ORM), który umożliwia deweloperom wykonywanie zapytań i aktualizowanie danych przechowywanych w bazie danych w sposób zorientowany obiektowo.

Entity Framework w wersji 4 obsługuje model programistyczny o nazwie Code-First. Kod — w pierwszej kolejności można utworzyć obiekt modelu, pisząc proste klasy (znane również jako POCO z "zwykłych-starych" obiektów CLR) i można nawet utworzyć bazę danych na bieżąco z klas.

### <a name="changes-to-our-model-classes"></a>Zmiany naszych klas modelu

W ramach tego samouczka będziemy korzystać z funkcji tworzenia bazy danych w Entity Framework. Przed wykonaniem tej czynności należy wprowadzić kilka drobnych zmian w naszych klasach modelu w celu dodania ich do innych elementów, które będą używane później.

#### <a name="adding-the-artist-model-classes"></a>Dodawanie klas modelu wykonawcy

Nasze albumy zostaną skojarzone z artyści, dlatego dodamy prostą klasę modelu do opisania wykonawcy. Dodaj nową klasę do folderu models o nazwie Artist.cs przy użyciu kodu pokazanego poniżej.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Aktualizowanie naszych klas modelu

Zaktualizuj klasę albumu, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Następnie wprowadź następujące aktualizacje klasy gatunek.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a>Dodawanie folderu danych\_aplikacji

Dodamy\_katalogu danych aplikacji do naszego projektu, aby przechowywać pliki bazy danych SQL Server Express. \_danych aplikacji jest specjalnym katalogiem w ASP.NET, który ma już odpowiednie uprawnienia dostępu do bazy danych. W menu Projekt wybierz pozycję Dodaj folder ASP.NET, a następnie kliknij pozycję Aplikacja\_dane.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Tworzenie parametrów połączenia w pliku Web. config

Dodamy kilka wierszy do pliku konfiguracji witryny sieci Web, aby Entity Framework wie, jak nawiązać połączenie z bazą danych. Kliknij dwukrotnie plik Web. config znajdujący się w katalogu głównym projektu.

![](mvc-music-store-part-4/_static/image2.png)

Przewiń w dół tego pliku i Dodaj sekcję &lt;connectionStrings&gt; bezpośrednio nad ostatnim wierszem, jak pokazano poniżej.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Dodawanie klasy kontekstu

Kliknij prawym przyciskiem myszy folder modele i Dodaj nową klasę o nazwie MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Ta klasa będzie reprezentować kontekst bazy danych Entity Framework i będzie obsługiwać nasze operacje tworzenia, odczytywania, aktualizowania i usuwania. Kod dla tej klasy pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

To nie istnieje inna konfiguracja, interfejsy specjalne itp. Rozszerzając klasę bazową DbContext, Nasza Klasa MusicStoreEntities może obsługiwać nasze operacje bazy danych. Teraz, gdy mamy już podłączane, dodajmy kilka więcej właściwości do naszych klas modelu, aby korzystać z niektórych dodatkowych informacji w naszej bazie danych.

### <a name="adding-our-store-catalog-data"></a>Dodawanie naszych danych katalogu sklepu

Będziemy korzystać z funkcji w Entity Framework, która dodaje dane "inicjatora" do nowo utworzonej bazy danych. Spowoduje to wstępne wypełnienie naszego katalogu sklepu listą gatunków, artystów i albumów. Pobieranie pliku MvcMusicStore-Assets. zip — który zawiera pliki projektu witryny używane wcześniej w tym samouczku — zawiera plik klasy z danymi tego inicjatora znajdującą się w folderze o nazwie Code.

W folderze Code/models Znajdź plik SampleData.cs i upuść go do folderu models w naszym projekcie, jak pokazano poniżej.

![](mvc-music-store-part-4/_static/image4.png)

Teraz musimy dodać jeden wiersz kodu, aby poinformować Entity Framework o tej klasie SampleData. Kliknij dwukrotnie plik Global. asax w folderze głównym projektu, aby go otworzyć, a następnie Dodaj następujący wiersz do początku metody\_uruchamiania aplikacji.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

W tym momencie wykonamy czynności niezbędne do skonfigurowania Entity Framework dla naszego projektu.

## <a name="querying-the-database"></a>wykonywanie zapytania w bazie danych

Teraz zaktualizujmy nasze StoreController, tak aby zamiast korzystania z "fikcyjnych danych" zamiast tego wywoływać do naszej bazy danych zapytania dotyczące wszystkich informacji. Zaczniemy od zadeklarowania pola na **StoreController** , aby pomieścić wystąpienie klasy MusicStoreEntities o nazwie storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Aktualizowanie indeksu magazynu w celu wysyłania zapytań do bazy danych

Klasa MusicStoreEntities jest obsługiwana przez Entity Framework i uwidacznia Właściwość kolekcji dla każdej tabeli w bazie danych. Zaktualizujmy akcję indeksu StoreController w celu pobrania wszystkich gatunków w naszej bazie danych. Wcześniej było to spowodowane przez stałe kodowanie danych ciągu. Teraz możemy używać kolekcji generes kontekstu Entity Framework:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

W naszym szablonie widoku nie trzeba zmieniać żadnych zmian, ponieważ nadal zwracamy ten sam StoreIndexViewModel, który zwróciłeś wcześniej — teraz zwracamy dane na żywo z naszej bazy danych.

Po ponownym uruchomieniu projektu i odwiedzeniu adresu URL "/Store" zostanie teraz wyświetlona lista wszystkich gatunków w bazie danych:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Aktualizowanie przeglądania i szczegółów sklepu w celu korzystania z danych na żywo

Za pomocą metody akcji/Store/Browse? gatunek = *[część-gatunek]* szukamy gatunku według nazwy. Oczekiwany jest tylko jeden wynik, ponieważ nie będziemy musieli mieć dwóch wpisów dla tej samej nazwy gatunku i dlatego możemy użyć. Pojedyncze () rozszerzenie w LINQ do zapytania dla odpowiedniego obiektu gatunku, takiego jak ten (nie należy jeszcze wpisywać):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Pojedyncza Metoda przyjmuje wyrażenie lambda jako parametr, który określa, że chcemy, aby obiekt o pojedynczym gatunku był zgodny z określoną wartością. W powyższym przypadku ładujemy pojedynczy obiekt typu gatunek z wartością nazwy zgodną z disco.

Będziemy korzystać z funkcji Entity Framework, która umożliwia wskazanie innych powiązanych jednostek, które chcemy załadować, a także po pobraniu obiektu gatunku. Ta funkcja jest nazywana kształtem wyników zapytania i umożliwia nam skrócenie liczby potrzeb dostępu do bazy danych w celu pobrania wszystkich potrzebnych informacji. Chcemy wstępnie pobrać albumy dla gatunku, który pobieramy, dlatego będziemy aktualizować zapytanie w celu uwzględnienia z gatunku. Dołącz ("albumy"), aby wskazać, że chcemy również mieć powiązane albumy. Jest to bardziej wydajne, ponieważ spowoduje to pobranie zarówno naszego gatunku, jak i danych z albumu w ramach pojedynczego żądania bazy danych.

Dzięki wyjaśnieniu z metody, Oto jak wygląda zaktualizowana akcja przeglądania kontrolera:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Teraz można zaktualizować Widok przeglądania sklepu, aby wyświetlić albumy, które są dostępne w każdym gatunku. Otwórz szablon widoku (znaleziony w/Views/Store/Browse.cshtml) i Dodaj listę punktowaną z listy Albumy, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Uruchamiając aplikację i przechodząc do/Store/Browse? gatunek = Jazz pokazuje, że nasze wyniki są teraz ściągane z bazy danych, wyświetlając wszystkie albumy w naszym wybranym gatunku.

![](mvc-music-store-part-4/_static/image2.jpg)

Wprowadzimy tę samą zmianę w adresie URL/Store/Details/[ID] i zamienimy nasze fikcyjne dane z zapytaniem bazy danych, które ładuje album, którego identyfikator pasuje do wartości parametru.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Uruchomienie naszej aplikacji i przechodzenie do/Store/Details/1 pokazuje, że nasze wyniki są teraz ściągane z bazy danych.

![](mvc-music-store-part-4/_static/image5.png)

Teraz, gdy strona szczegółów sklepu została skonfigurowana do wyświetlania albumu przez identyfikator albumu, zaktualizujmy widok **przeglądania** , aby połączyć się z widokiem szczegółów. Będziemy używać języka HTML. ActionLink dokładnie tak samo jak w przypadku linków z indeksu magazynu, aby przechować wyszukiwanie na końcu poprzedniej sekcji. Pełne Źródło dla widoku przeglądania jest wyświetlane poniżej.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Teraz można przeglądać stronę ze swoim sklepem na stronie z gatunku, która zawiera listę dostępnych albumów, a następnie klikając album, aby wyświetlić szczegółowe informacje o tym albumie.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-3.md)
> [dalej](mvc-music-store-part-5.md)

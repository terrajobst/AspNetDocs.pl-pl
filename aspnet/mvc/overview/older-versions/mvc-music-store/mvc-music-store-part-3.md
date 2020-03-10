---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Część 3: widoki i modele widoków | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 3 obejmuje widoki i modele widoków.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559809"
---
# <a name="part-3-views-and-viewmodels"></a>Część 3. Widoki i modele widoków

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 3 obejmuje widoki i modele widoków.

Do tej pory właśnie zwracamy ciągi z akcji kontrolera. Jest to świetny sposób, aby poznać sposób działania kontrolerów, ale nie jest to, w jaki sposób chcesz utworzyć rzeczywistą aplikację sieci Web. Chcemy ulepszyć generowanie kodu HTML z powrotem do przeglądarek odwiedzających naszą witrynę — jeden tam, gdzie możemy używać plików szablonów, aby łatwiej dostosować zawartość HTML. To dokładnie, które widoki robią.

## <a name="adding-a-view-template"></a>Dodawanie szablonu widoku

Aby użyć szablonu widoku, zmienimy metodę indeksu HomeController, aby zwracała ActionResult, i będzie mieć widok zwracany (), podobny do poniższego:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Powyższa zmiana wskazuje, że zamiast zwrócić ciąg, chcemy użyć "widoku" do wygenerowania wyniku z powrotem.

Teraz dodamy odpowiedni szablon widoku do projektu. Aby to zrobić, ustaw kursor tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję "Dodaj widok". Spowoduje to wyświetlenie okna dialogowego Dodawanie widoku:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

Okno dialogowe "Dodawanie widoku" pozwala nam szybko i łatwo generować pliki szablonów widoków. Domyślnie okno dialogowe "Dodaj widok" wstępnie wypełnia nazwę szablonu widoku do utworzenia, aby była zgodna z metodą akcji, która będzie go używać. Ze względu na to, że użyto menu kontekstowego "Dodaj widok" w metodzie akcji index () naszego HomeController, w oknie dialogowym "Dodaj widok" powyżej znajduje się wartość "index" (nazwa widoku jest wstępnie wypełniona). Nie trzeba zmieniać żadnych opcji w tym oknie dialogowym, dlatego kliknij przycisk Dodaj.

Po kliknięciu przycisku Dodaj, Visual Web Developer utworzy nowy szablon widoku index. cshtml dla nas w katalogu \Views\Home, tworząc folder, jeśli jeszcze nie istnieje.

![](mvc-music-store-part-3/_static/image2.png)

Nazwa i lokalizacja pliku "index. cshtml" są ważne i są zgodne z domyślnymi konwencjami nazewnictwa MVC ASP.NET. Nazwa katalogu, \Views\Home, pasuje do kontrolera o nazwie HomeController. Nazwa szablonu widoku, indeks, dopasowuje metodę akcji kontrolera, która będzie wyświetlać widok.

ASP.NET MVC umożliwia nam uniknięcie jawnego określenia nazwy lub lokalizacji szablonu widoku, gdy używamy tej konwencji nazewnictwa do zwrócenia widoku. Domyślnie renderuje szablon widoku \Views\Home\Index.cshtml, gdy piszesz kod podobny do poniższego w naszym HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer utworzył i otworzył szablon widoku "index. cshtml" po kliknięciu przycisku "Dodaj" w oknie dialogowym "Dodawanie widoku". Zawartość index. cshtml pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Ten widok używa składnia Razor, który jest bardziej zwięzły niż aparat widoku formularzy sieci Web używany w ASP.NET Web Forms i poprzednich wersjach ASP.NET MVC. Aparat widoku formularzy sieci Web jest nadal dostępny w ASP.NET MVC 3, ale wielu deweloperów stwierdzi, że aparat widoku Razor dopasowuje ASP.NET rozwój MVC.

Pierwsze trzy wiersze ustawiają tytuł strony przy użyciu ViewBag. title. Wkrótce sprawdzimy, jak to działa bardziej szczegółowo, ale najpierw zaktualizujemy tekst nagłówka tekstu i wyświetli stronę. Zaktualizuj tag &lt;H2&gt;, aby powiedzieć "to jest Strona główna", jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Uruchomienie aplikacji pokazuje, że nowy tekst jest widoczny na stronie głównej.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Korzystanie z układu dla wspólnych elementów lokacji

Większość witryn sieci Web ma zawartość, która jest udostępniana między wieloma stronami: nawigowanie, stopki, obrazy logo, odwołania arkusza stylów itp. Aparat widoku Razor ułatwia zarządzanie przy użyciu strony o nazwie \_Layout. cshtml, która została automatycznie utworzona dla nas w folderze/Views/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Kliknij dwukrotnie ten folder, aby wyświetlić zawartość, która jest pokazana poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Zawartość z poszczególnych widoków zostanie wyświetlona przy użyciu polecenia @RenderBody(), a każda Wspólna zawartość, która ma być wyświetlana poza programem, można dodać do znacznika \_Layout. cshtml. Chcemy, aby nasz sklep MVC Music miał wspólny nagłówek z linkami do naszej strony głównej i obszaru przechowywania na wszystkich stronach w witrynie, dlatego dodamy go do szablonu bezpośrednio powyżej tej instrukcji @RenderBody().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Aktualizowanie arkusza stylów

Szablon pustego projektu zawiera bardzo uproszczony plik CSS, który zawiera tylko style używane do wyświetlania komunikatów sprawdzania poprawności. Nasz Projektant udostępnił kilka dodatkowych arkuszy CSS i obrazów, aby zdefiniować wygląd i działanie naszej witryny, dlatego dodamy je teraz.

Zaktualizowany plik i obrazy CSS są zawarte w katalogu zawartości MvcMusicStore-Assets. zip, który jest dostępny w [sklepie MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store). Będziemy wybierać obydwie te elementy w Eksploratorze Windows i upuszczać je do folderu zawartości naszego rozwiązania w programie Visual Web Developer, jak pokazano poniżej:

![](mvc-music-store-part-3/_static/image5.png)

Zostanie wyświetlony monit o potwierdzenie, czy chcesz zastąpić istniejący plik. css. kliknij przycisk Tak.

![](mvc-music-store-part-3/_static/image6.png)

Folder zawartości aplikacji będzie teraz wyświetlany w następujący sposób:

![](mvc-music-store-part-3/_static/image7.png)

Teraz Uruchommy aplikację i zobacz, w jaki sposób zmiany wyglądają na stronie głównej.

![](mvc-music-store-part-3/_static/image8.png)

- Zapoznaj się z informacjami o zmianach: odnaleziono metodę akcji indeksu HomeController i Wyświetlono szablon \Views\Home\Index.cshtmlView, mimo że nasz kod nosi nazwę "return View ()", ponieważ nasz szablon widoku przestrzega standardowej konwencji nazewnictwa.
- Na stronie głównej jest wyświetlany prosty komunikat powitalny, który jest zdefiniowany w szablonie widoku \Views\Home\Index.cshtml.
- Strona główna używa szablonu \_Layout. cshtml, dlatego Komunikat powitalny jest zawarty w układzie HTML witryny standardowej.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Używanie modelu do przekazywania informacji do naszego widoku

Szablon widoku, który po prostu wyświetla kod HTML stałe, nie może utworzyć bardzo interesującej witryny sieci Web. Aby utworzyć dynamiczną witrynę sieci Web, należy przekazać informacje z naszych akcji kontrolera do naszych szablonów widoków.

W wzorcu Model-View-Controller pojęcie model odnosi się do obiektów, które reprezentują dane w aplikacji. Często obiekty modelu są zgodne z tabelami w bazie danych, ale nie muszą.

Metody akcji kontrolera, które zwracają ActionResult, mogą przekazać obiekt modelu do widoku. Dzięki temu kontroler może oczyścić wszystkie informacje potrzebne do wygenerowania odpowiedzi, a następnie przekazać te informacje do szablonu widoku, który zostanie użyty do wygenerowania odpowiedniej odpowiedzi HTML. Jest to najłatwiej zrozumieć, wyświetlając go w działaniu, więc zacznijmy.

Najpierw utworzysz klasy modelu, aby reprezentować gatunek i albumy w naszym sklepie. Zacznijmy od utworzenia klasy gatunku. Kliknij prawym przyciskiem myszy folder "models" w projekcie, wybierz opcję "Dodaj klasę" i Nazwij plik "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Następnie Dodaj publiczną właściwość nazwy ciągu do klasy, która została utworzona:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Uwaga: na wypadek zastanawiasz się, że w notacji {get; set;} jest C#używana funkcja właściwości, która została zaimplementowana. Daje to nam zalety właściwości bez konieczności deklarowania pola zapasowego.*

Następnie wykonaj te same kroki, aby utworzyć klasę albumu (o nazwie Album.cs), która ma tytuł i Właściwość gatunku:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Teraz możemy zmodyfikować StoreController tak, aby korzystał z widoków, które wyświetlają dynamiczne informacje z naszego modelu. Jeśli — dla celów demonstracyjnych już teraz — nazywamy Twoje albumy na podstawie identyfikatora żądania, możemy wyświetlić te informacje jak w poniższym widoku.

![](mvc-music-store-part-3/_static/image10.png)

Zaczniemy od zmiany akcji szczegóły sklepu, aby wyświetlić informacje o pojedynczym albumie. Dodaj instrukcję "Using" na początku klasy **StoreControllers** , aby uwzględnić przestrzeń nazw MvcMusicStore. Models, więc nie trzeba wpisywać MvcMusicStore. models. album za każdym razem, gdy chcemy użyć klasy albumu. Sekcja "usings" tej klasy powinna teraz wyglądać następująco.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Następnie zaktualizujemy akcję kontrolera szczegółów w taki sposób, aby zwracała ActionResult zamiast ciągu, podobnie jak w przypadku metody indeksu HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Teraz możemy zmodyfikować logikę, aby zwracała obiekt albumu do widoku. W dalszej części tego samouczka będziemy pobierać dane z bazy danych, ale w celu rozpoczęcia pracy będziemy używać "fikcyjnych danych".

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Uwaga: Jeśli nie masz doświadczenia w programie C#, możesz założyć, że użycie funkcji VAR oznacza, że nasza zmienna albumu jest opóźniona. To nie jest poprawne — C# kompilator używa wnioskowania typu na podstawie tego, co przypiszemy do zmiennej, aby określić, że album jest typu albumu i kompiluje lokalną zmienną albumu jako typ albumu, dzięki czemu będziemy mogli sprawdzać czas kompilacji i obsługiwać Edytor kodu programu Visual Studio.*

Teraz utworzysz szablon widoku, który używa naszego albumu do wygenerowania odpowiedzi HTML. Przed wykonaniem tej czynności musimy skompilować projekt, aby okno dialogowe Dodawanie widoku wie o naszej nowo utworzonej klasie albumu. Można skompilować projekt, wybierając element menu Debuguj ⇨ kompilację MvcMusicStore (Aby uzyskać dodatkowe środki, można użyć skrótu Ctrl-Shift-B do skompilowania projektu).

![](mvc-music-store-part-3/_static/image11.png)

Teraz, po skonfigurowaniu naszych klas pomocniczych, jesteśmy gotowi do skompilowania naszego szablonu widoku. Kliknij prawym przyciskiem myszy w ramach metody szczegóły i wybierz pozycję "Dodaj widok..." z menu kontekstowego.

![](mvc-music-store-part-3/_static/image12.png)

Utworzymy nowy szablon widoku, taki jak wcześniej z HomeController. Ponieważ tworzymy ją z StoreController, zostanie ona domyślnie wygenerowana w pliku \Views\Store\Index.cshtml.

W przeciwieństwie do wcześniej sprawdzimy pole wyboru "Utwórz silnie wpisaną" widok. Następnie wybieramy naszą klasę "album" w menu rozwijanym "Wyświetl dane" klasy "downlist". Spowoduje to wyświetlenie okna dialogowego "Dodawanie widoku" w celu utworzenia szablonu widoku, który oczekuje, że obiekt albumu zostanie przesłany do niego do użycia.

![](mvc-music-store-part-3/_static/image13.png)

Po kliknięciu przycisku "Dodaj" zostanie utworzony szablon widoku \Views\Store\Details.cshtml zawierający poniższy kod.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Zwróć uwagę na pierwszy wiersz, który wskazuje, że ten widok jest silnie wpisana do naszej klasy albumu. Aparat widoku Razor rozumie, że został przekazano obiekt albumu, dzięki czemu możemy łatwo uzyskać dostęp do właściwości modelu i nawet korzystać z funkcji IntelliSense w edytorze Visual Web Developer.

Zaktualizuj tag &lt;H2&gt; tak, aby wyświetlał Właściwość tytuł albumu, modyfikując ten wiersz tak, aby pojawił się w następujący sposób.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Zauważ, że funkcja IntelliSense jest wyzwalana po wprowadzeniu kropki po słowie kluczowym @Model, pokazując właściwości i metody obsługiwane przez klasę albumu.

Teraz ponownie uruchom nasz projekt i przejdź do adresu URL/Store/Details/5. Zobaczymy szczegółowe informacje o albumie, jak pokazano poniżej.

![](mvc-music-store-part-3/_static/image14.png)

Teraz wprowadzimy podobną aktualizację metody operacji przeglądania sklepu. Zaktualizuj metodę, tak aby zwracała ActionResult, i zmodyfikuj logikę metody, aby tworzyła nowy obiekt gatunku i zwracał go do widoku.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Kliknij prawym przyciskiem myszy w metodzie Przeglądaj i wybierz pozycję "Dodaj widok..." z menu kontekstowego Dodaj widok, który jest silnie określony, Dodaj silnie wpisanej klasy gatunku.

![](mvc-music-store-part-3/_static/image15.png)

Zaktualizuj element &lt;H2&gt; w kodzie widoku (w/Views/Store/Browse.cshtml), aby wyświetlić informacje o gatunku.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Teraz ponownie uruchom nasz projekt i przejdź do/Store/Browse? Gatunek = adres URL Disco. Zobaczysz stronę przeglądania, która wygląda jak poniżej.

![](mvc-music-store-part-3/_static/image16.png)

Na koniec Przyjrzyjmy nieco bardziej skomplikowaną aktualizację do metody akcji **indeksu magazynu** i widoku, aby wyświetlić listę wszystkich gatunków w naszym sklepie. Zajmiemy się tym, korzystając z listy gatunków jako obiektu modelu, a nie tylko jednego gatunku.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Kliknij prawym przyciskiem myszy metodę akcji indeks magazynu i wybierz opcję Dodaj widok jako poprzednio, wybierz gatunek jako klasę modelu i naciśnij przycisk Dodaj.

![](mvc-music-store-part-3/_static/image17.png)

Najpierw zmienimy deklarację @model, aby wskazać, że widok będzie oczekiwać kilku obiektów gatunku, a nie tylko jednego. Zmień pierwszy wiersz/Store/Index.cshtml w następujący sposób:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Oznacza to, że aparat widoku Razor będzie pracował z obiektem modelu, który może zawierać kilka obiektów gatunku. Używamy gatunku IEnumerable&lt;&gt;, a nie listy&lt;&gt; gatunku, ponieważ jest to bardziej ogólny, dzięki czemu możemy zmienić nasz Typ modelu później na dowolny typ obiektu, który obsługuje interfejs IEnumerable.

Następnie będziemy przepętlać przez obiekty gatunku w modelu, jak pokazano w poniższym kodzie widoku poniżej.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Zwróć uwagę, że firma Microsoft zapewnia pełną obsługę technologii IntelliSense, ponieważ wprowadzamy ten kod, więc w przypadku wpisania "@Model". zobaczymy wszystkie metody i właściwości obsługiwane przez interfejs IEnumerable typu gatunek.

![](mvc-music-store-part-3/_static/image18.png)

W naszej pętli "foreach" Visual Web Developer wie, że każdy element jest typu gatunek, więc dla każdego typu gatunku widzimy funkcję IntelliSense.

![](mvc-music-store-part-3/_static/image19.png)

Następnie funkcja tworzenia szkieletów bada obiekt gatunku i stwierdził, że każdy z nich będzie miał Właściwość Name, dlatego pętle i zapisuje je. Generuje również linki Edytuj, szczegóły i Usuń do każdego pojedynczego elementu. Będziemy korzystać z tej usługi później w naszym Menedżerze sklepu, ale teraz będziemy mieć prostą listę.

Gdy uruchomimy aplikację i przejdziesz do/Store, zobaczymy, że jest wyświetlana zarówno liczba, jak i Lista gatunków.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Dodawanie linków między stronami

Nasz adres URL/Store, który zawiera listę gatunku, w postaci zwykłego tekstu. Zmieńmy to w taki sposób, aby zamiast zwykłego tekstu zamiast tego był miał połączenie z odpowiednim adresem URL/Store/Browse, dzięki czemu kliknięcie gatunku muzycznego, takiego jak "Disco", spowoduje przejście do adresu URL/Store/Browse? gatunek = Disco. Możemy zaktualizować nasz szablon widoku \Views\Store\Index.cshtml, aby wyprowadził te linki przy użyciu kodu, takiego jak poniżej **(nie należy wpisywać tego polecenia w trakcie działania)** :

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

To działa, ale może prowadzić do późniejszego problemu, ponieważ opiera się on na ciągu stałe. Na przykład jeśli chcemy zmienić nazwę kontrolera, musimy przeszukać nasz kod szukający linków, które wymagają aktualizacji.

Alternatywną metodą, której można użyć, jest skorzystanie z metody pomocnika HTML. ASP.NET MVC zawiera metody pomocników HTML, które są dostępne w naszym kodzie szablonu widoku do wykonywania różnych typowych zadań w podobny sposób. Metoda pomocnika html. ActionLink () jest szczególnie przydatna i ułatwia tworzenie kodu HTML &lt;&gt; linków i bierze pod uwagę irytujące szczegóły, takie jak upewnienie się, że ścieżki URL są poprawnie zakodowane w adresie URL.

Plik HTML. ActionLink () ma kilka różnych przeciążeń, aby można było określić tyle informacji, ile potrzebujesz w przypadku linków. W najprostszym przypadku należy podać tylko tekst łącza i metodę akcji, aby przejść do po kliknięciu hiperlinku na kliencie. Można na przykład połączyć się z metodą "/Store/" index () na stronie szczegółów sklepu z tekstem linku "przejdź do indeksu magazynu" przy użyciu następującego wywołania:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Uwaga: w tym przypadku nie musimy określić nazwy kontrolera, ponieważ po prostu łączysz się z inną akcją w ramach tego samego kontrolera, który renderuje bieżący widok.*

Nasze linki do strony przeglądania muszą przekazać parametr, mimo że będziemy używać innego przeciążenia metody html. ActionLink, która przyjmuje trzy parametry:

- 1. Tekst łącza, który będzie wyświetlał nazwę gatunku
- 2. Nazwa akcji kontrolera (Przeglądaj)
- 3. Wartości parametrów trasy, określające zarówno nazwę (gatunek), jak i wartość (nazwę gatunku)

W tym celu należy napisać wszystkie te linki do widoku indeksu magazynu:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Teraz po ponownym uruchomieniu projektu i otrzymaniu dostępu do adresu URL/Store/zostanie wyświetlona lista gatunku. Każdy gatunek jest hiperłączem — po jego kliknięciu zajmiemy nas/Store/Browse? gatunek = *[Gatunek]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

KOD HTML dla listy gatunek wygląda następująco:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-2.md)
> [dalej](mvc-music-store-part-4.md)

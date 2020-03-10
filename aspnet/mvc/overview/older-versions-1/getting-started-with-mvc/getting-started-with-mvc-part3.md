---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Dodawanie widoku | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 462b1210c45da67058899193afcea973f3daf122
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581558"
---
# <a name="adding-a-view"></a>Dodawanie widoku

przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

W tej sekcji zamierzamy zajrzeć się, w jaki sposób możemy użyć naszego klasy HelloWorldController, aby wyczyścić generowanie odpowiedzi HTML z powrotem do klienta.

Zacznijmy od użycia szablonu widoku z naszymi metodami indeksu. Nasza metoda jest nazywana indeksem i znajduje się w HelloWorldController. Obecnie nasza metoda index () zwraca ciąg z komunikatem, który jest stałe w ramach klasy Controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Zmieńmy teraz metodę index, aby zamiast tego wyglądać następująco:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Teraz dodamy szablon widoku do naszego projektu, którego możemy użyć dla naszego metody index (). Aby to zrobić, kliknij prawym przyciskiem myszy myszą w środku metody indeksu i kliknij polecenie Dodaj widok...

![image](getting-started-with-mvc-part3/_static/image1.png)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie widoku", które zapewnia nam kilka opcji tworzenia szablonu widoku, który może być używany przez naszą metodę index. Na razie nie zmieniaj niczego i po prostu kliknij przycisk Dodaj.

[Okno dialogowe dodawania widoku ![](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Po kliknięciu przycisku Dodaj nowy folder i nowy plik pojawi się w folderze rozwiązania, jak pokazano tutaj. Teraz mam folder HelloWorld w obszarze widoki i plik index. aspx znajdujący się w tym folderze.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Nowy plik indeksu jest również otwarty i gotowy do edycji. Dodaj tekst poniżej pierwszej &lt;H2&gt;index&lt;/H2&gt; na przykład "Hello world".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Uruchom aplikację i ponownie odwiedź [`http://localhost:xx/HelloWorld`](http://localhostxx) w przeglądarce. Metoda index w naszym kontrolerze w tym przykładzie nie wykonała żadnej pracy, ale wywołała "zwracany widok ()", który wskazywał, że chciałem użyć pliku szablonu widoku w celu renderowania odpowiedzi z powrotem do klienta. Ponieważ nie określono jawnie nazwy pliku szablonu widoku, ASP.NET MVC domyślnie przy użyciu pliku widoku index. aspx znajdującego się w folderze \Views\HelloWorld. Teraz zobaczymy ciąg, który został zakodowany w naszym widoku.

[Indeks ![— Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Wyglądają dość dobre. Należy jednak zauważyć, że tytuł przeglądarki mówi "index", a duży tytuł na stronie ma wartość "moja aplikacja MVC". Zmieńmy te.

### <a name="changing-views-and-master-pages"></a>Zmiana widoków i stron wzorcowych

Najpierw Zmieńmy tekst "moja aplikacja MVC". Ten tekst jest współużytkowany i pojawia się na każdej stronie. Faktycznie występuje tylko w jednym miejscu w naszym kodzie, nawet jeśli znajduje się na każdej stronie w naszej aplikacji. Przejdź do folderu/Views/Shared w Eksplorator rozwiązań i Otwórz plik site. Master. Ten plik jest nazywany stroną wzorcową i jest udostępnianą "powłoką", którą używają wszystkie inne strony.

Zauważ, że w tym pliku jest wyświetlany tekst "kontrolka mainContent".

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Ten symbol zastępczy to miejsce, w którym wszystkie utworzone strony zostaną wyświetlone na stronie wzorcowej. Spróbuj zmienić tytuł, a następnie uruchom aplikację i odwiedź kilka stron. Zobaczysz, że jedna zmiana zostanie wyświetlona na wielu stronach.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Teraz każda strona będzie miała nagłówek podstawowy — czyli H1-of "moja aplikacja filmów MVC". Obsługuje on biały tekst na górze, który jest udostępniany na wszystkich stronach.

Oto witryna. Master w całości z naszym zmienionym tytułem:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Teraz Zmieńmy tytuł strony indeksu.

Open /HelloWorld/Index.aspx. Istnieją dwa miejsca do zmiany. Po pierwsze tytuł, który pojawia się w tytule przeglądarki, a następnie pomocniczy nagłówek — to również H2. Zrobię je nieco inaczej, aby zobaczyć, który bit kodu zmienia się w ramach aplikacji.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Uruchom aplikację i odwiedź witrynę/Movies. Zwróć uwagę, że tytuł przeglądarki, nagłówek podstawowy i nagłówki pomocnicze uległy zmianie. W aplikacji można łatwo wprowadzać duże zmiany, korzystając z niewielkich zmian w Twoim widoku.

[Lista filmów ![— Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nasz mały bit "Data" (w tym przypadku "Hello world!" komunikat) został sztywno zakodowany. Mamy (przeglądający), a mamy C (kontrolery), ale nie ma jeszcze M (model). Wkrótce przeprowadzimy Cię przez proces tworzenia bazy danych i pobierania z niej danych modelu.

## <a name="passing-a-viewmodel"></a>Przekazywanie elementu ViewModel

Zanim przejdziemy do bazy danych i porozmawiamy z modelami, najpierw porozmawiamy o "modele widoków". Są to obiekty, które przedstawiają elementy wymagane przez szablon widoku do renderowania odpowiedzi HTML z powrotem do klienta. Są one zazwyczaj tworzone i przesyłane przez klasę kontrolera do szablonu widoku i powinny zawierać tylko te dane, które są wymagane przez szablon widoku — i nie tylko.

Wcześniej przy użyciu naszego przykładu HelloWorld, nasza metoda akcji powitalnej () przyjęła nazwę i parametr numTimes i wyprowadza go do przeglądarki. Zamiast korzystać z kontrolera w dalszym ciągu renderować tę odpowiedź bezpośrednio, zamiast tego należy przystąpić do przechowywania tych danych, a następnie przekazać je do szablonu widoku w celu renderowania odpowiedzi HTML z użyciem. W ten sposób kontroler jest objęty jednym elementem i szablonem widoku, który umożliwia nam zachowanie czystego "separacji problemów" w naszej aplikacji.

Wróć do pliku HelloWorldController.cs i Dodaj nową klasę "WelcomeViewModel" i Zmień metodę Welcome na kontrolerze. Oto kompletny HelloWorldController.cs z nową klasą w tym samym pliku.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Mimo że znajduje się on w wielu wierszach, nasze metody powitalnej są naprawdę tylko dwoma instrukcjami w kodzie. Pierwsza instrukcja pakuje nasze dwa parametry do obiektu ViewModel, a drugi przekazuje obiekt wyniku do widoku.

Teraz potrzebujemy szablonu widoku powitalnego. Kliknij prawym przyciskiem myszy metodę Welcome i wybierz polecenie Dodaj widok. Tym razem sprawdzimy "Tworzenie widoku o jednoznacznie określonym typie" i wybieramy nasze klasy WelcomeViewModel z listy rozwijanej. Ten nowy widok będzie wiedzieć tylko o WelcomeViewModels i nie ma żadnych innych typów obiektów.

> *Uwaga: po dodaniu WelcomeViewModel do wyświetlania na liście rozwijanej należy wykonać kompilację.*

Oto, co powinien wyglądać okno dialogowe Dodaj widok. Kliknij przycisk Dodaj. ![Dodaj widok w kółku](getting-started-with-mvc-part3/_static/image10.png)

Dodaj ten kod do &lt;H2&gt; w nowym elemencie Welcome. aspx. Utworzymy pętlę i będziemy mówić do powitania tyle razy, ile powinien!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Zwróć uwagę na to, że podczas wpisywania otrzymujesz ten widok dotyczący WelcomeViewModel (z ślubem, pamiętaj?), aby uzyskać pomoc IntelliSense za każdym razem, gdy odwołujemy się do naszego obiektu modelu, jak pokazano na poniższym zrzucie ekranu:

[![kod źródłowy NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Uruchom aplikację i ponownie odwiedź `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`. Teraz, gdy pobieramy dane z adresu URL, zostanie on automatycznie przekazana do naszego kontrolera, nasz kontroler przypakuje dane do ViewModel i przekaże ten obiekt do naszego widoku. Widok niż wyświetla dane w formacie HTML dla użytkownika.

[![— Zapraszamy — Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Jest to również rodzaj "M" dla modelu, ale nie jako rodzaj bazy danych. Zapoznaj się z informacjami i Utwórz bazę danych dla filmów.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part2.md)
> [dalej](getting-started-with-mvc-part4.md)

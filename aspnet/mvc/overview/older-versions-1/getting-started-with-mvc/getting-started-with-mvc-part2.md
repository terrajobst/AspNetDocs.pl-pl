---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Dodawanie kontrolera | Microsoft Docs
author: shanselman
description: Zaktualizowana wersja, jeśli ten samouczek jest dostępny w tym miejscu przy użyciu Visual Studio 2013. W nowym samouczku jest używany ASP.NET MVC 5, który zapewnia wiele ulepszeń w porównaniu do t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: e2a298584473f57c2b14edf507f0f6886d906ea3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543982"
---
# <a name="adding-a-controller"></a>Dodawanie kontrolera

przez [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Zaktualizowana wersja, jeśli ten samouczek jest dostępny w [tym miejscu](../../getting-started/introduction/getting-started.md) przy użyciu [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). W nowym samouczku jest używany ASP.NET MVC 5, który zapewnia wiele ulepszeń w tym samouczku.
>
>
> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

MVC to model, widok, kontroler. MVC to wzorzec służący do tworzenia aplikacji w taki sposób, aby każda część była obowiązkiem innym niż inne.

- Model: dane aplikacji
- Widoki: pliki szablonów używane przez aplikację do dynamicznego generowania odpowiedzi HTML.
- Kontrolery: klasy obsługujące przychodzące żądania adresów URL do aplikacji, pobieranie danych modelu, a następnie określanie szablonów wyświetlania, które renderują odpowiedzi z powrotem do klienta

Będziemy omawiać wszystkie te koncepcje w tym samouczku i pokazują, jak używać ich do kompilowania aplikacji.

Utwórz nowy kontroler, klikając prawym przyciskiem myszy folder controllers w Eksploratorze rozwiązań i wybierając polecenie Dodaj kontroler.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nazwij nowy kontroler "HelloWorldController" i kliknij przycisk Dodaj.

[Okno dialogowe Dodawanie kontrolera ![](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Zwróć uwagę na Eksplorator rozwiązań po prawej stronie, że został utworzony nowy plik o nazwie HelloWorldController.cs i ten plik jest teraz otwarty w środowisku **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Utwórz dwie nowe metody, które wyglądają podobnie jak w przypadku nowej klasy publicznej HelloWorldController. Na przykład będziemy zwracać ciąg HTML bezpośrednio z naszego kontrolera.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Twój kontroler ma nazwę HelloWorldController, a nowa metoda jest nazywana indeksem. Uruchom aplikację ponownie, tak jak wcześniej (kliknij przycisk Odtwórz lub naciśnij klawisz F5, aby to zrobić). Po rozpoczęciu pracy przeglądarki zmień ścieżkę na pasku adresu, aby `http://localhost:xx/HelloWorld`, gdzie XX jest dowolną liczbą wybraną przez komputer. Teraz Twoja przeglądarka powinna wyglądać jak na poniższym zrzucie ekranu. W naszej metodzie zwracamy ciąg przesłany do metody o nazwie "Content". Firma Microsoft informuje o tym, że system zwróci jakiś kod HTML.

ASP.NET MVC wywołuje różne klasy kontrolera (i różne metody akcji w nich) w zależności od przychodzącego adresu URL. Domyślna logika mapowania używana przez ASP.NET MVC używa formatu, takiego jak ten, aby kontrolować kod, który jest uruchamiany:

/[Controller]/[ActionName]/[parametry]

Pierwsza część adresu URL określa klasę kontrolera do wykonania. Dlatego/HelloWorld mapuje do klasy HelloWorldController. Druga część adresu URL określa metodę akcji dla klasy, która ma zostać wykonana. Dlatego/HelloWorld/Index mógłby spowodować wykonanie metody index () klasy HelloWorldController. Zwróć uwagę, że mamy tylko odwiedzić/HelloWorld powyżej, a indeks metody został implikowany. Wynika to z faktu, że metoda o nazwie "index" jest metodą domyślną, która zostanie wywołana na kontrolerze, jeśli nie została określona jawnie.

[![to jest moja Akcja domyślna](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Teraz przyjrzyjmy się `http://localhost:xx/HelloWorld/Welcome.` teraz nasze metody powitalnej zostały wykonane i zwrócone przez nią ciąg HTML.

Ponownie,/[Controller]/[ActionName]/[Parameters], więc Controller to HelloWorld, a Welcome to metoda w tym przypadku. Jeszcze nie wykonano żadnych parametrów.

[![to jest metoda akcji powitalnej](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Zmodyfikujmy przykład w nieco inny sposób, aby można było przekazać niektóre informacje z adresu URL do naszego kontrolera, na przykład takie:/HelloWorld/Welcome? Name = Scott&amp;numtimes = 4. Zmień metodę powitalną, aby uwzględnić dwa parametry i zaktualizować ją jak poniżej. Należy zauważyć, że użyto C# opcjonalnej funkcji parametru, aby wskazać, że parametr numTimes ma domyślnie wartość 1, jeśli nie została przeniesiona.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Uruchom aplikację i odwiedź `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` zmienić wartość Nazwa i numtimes. System automatycznie zmapowan nazwane parametry z ciągu zapytania na pasku adresu do parametrów w metodzie.

W obu tych przykładach kontroler wykonywał całą służbę i zwraca kod HTML bezpośrednio. Zwykle nie chcemy, aby nasze kontrolery zwracają kod HTML bezpośrednio — ponieważ nie są one bardzo uciążliwe w kodzie. Zamiast tego zwykle użyjesz osobnego pliku szablonu widoku, aby ułatwić generowanie odpowiedzi HTML. Przyjrzyjmy się, jak to zrobić. Zamknij przeglądarkę i wróć do środowiska IDE.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part1.md)
> [dalej](getting-started-with-mvc-part3.md)

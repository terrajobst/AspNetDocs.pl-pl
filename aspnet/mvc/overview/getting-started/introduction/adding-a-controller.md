---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Dodawanie kontrolera | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 194a8a7398e163f0c37164a8724f98b16444984b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615809"
---
# <a name="adding-a-controller"></a>Dodawanie kontrolera

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

MVC oznacza *Model-View-Controller*. MVC to wzorzec służący do tworzenia aplikacji, które są dobrze zaprojektowane, weryfikowalne i łatwe w obsłudze. Aplikacje oparte na MVC zawierają:

- **M** Odels: klasy reprezentujące dane aplikacji i używające logiki walidacji do wymuszania reguł dla tych danych.
- **V** iews: pliki szablonów używane przez aplikację do dynamicznego generowania odpowiedzi html.
- **C** Ontrollers: klasy obsługujące żądania przeglądarki przychodzącej, pobieranie danych modelu, a następnie określanie szablonów wyświetlania, które zwracają odpowiedź do przeglądarki.

Będziemy omawiać wszystkie te koncepcje w tej serii samouczków i pokazują, jak używać ich do kompilowania aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , a następnie kliknij polecenie **Dodaj**, a następnie pozycję **kontroler**.

![](adding-a-controller/_static/image1.png)

W oknie dialogowym **Dodawanie szkieletu** kliknij pozycję **kontroler MVC 5 — pusty**, a następnie kliknij przycisk **Dodaj**.

![](adding-a-controller/_static/image2.png)  

Nazwij nowy kontroler "HelloWorldController" i kliknij przycisk **Dodaj**.

![Dodaj kontroler](adding-a-controller/_static/image3.png)

Zwróć uwagę, że w **Eksplorator rozwiązań** został utworzony nowy plik o nazwie *HelloWorldController.cs* i nowy folder *Views\HelloWorld*. Kontroler jest otwarty w IDE.

![](adding-a-controller/_static/image4.png)

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontrolera zwrócą ciąg HTML jako przykład. Kontroler ma nazwę `HelloWorldController` a pierwsza metoda ma nazwę `Index`. Wywołajmy ją z przeglądarki. Uruchom aplikację (naciśnij klawisz F5 lub CTRL + F5). W przeglądarce Dołącz &quot;HelloWorld&quot; do ścieżki na pasku adresu. (Na przykład na poniższej ilustracji jest `http://localhost:1234/HelloWorld.`) Strona w przeglądarce będzie wyglądać podobnie do poniższego zrzutu ekranu. W powyższej metodzie kod zwrócił ciąg bezpośrednio. Otrzymasz informację o tym, że system po prostu zwróci kod HTML.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC wywołuje różne klasy kontrolera (i różne metody akcji w nich) w zależności od przychodzącego adresu URL. Domyślna logika routingu adresów URL używana przez ASP.NET MVC używa formatu, takiego jak to, aby określić kod, który ma zostać wywołany:

`/[Controller]/[ActionName]/[Parameters]`

Format routingu w aplikacji jest ustawiany *\_Start/RouteConfig. cs* .

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Gdy uruchamiasz aplikację i nie podasz żadnych segmentów adresów URL, domyślnym kontrolerem "Home" i akcją "index" określono w sekcji wartości domyślne w powyższym kodzie.

Pierwsza część adresu URL określa klasę kontrolera do wykonania. Tak więc */HelloWorld* mapuje do klasy `HelloWorldController`. Druga część adresu URL określa metodę akcji dla klasy, która ma zostać wykonana. Dlatego */HelloWorld/index* może spowodować wykonanie metody `Index` klasy `HelloWorldController`. Zwróć uwagę, że tylko chcemy przeglądać do */HelloWorld* , a metoda `Index` była używana domyślnie. Wynika to z faktu, że metoda o nazwie `Index` jest metodą domyślną, która zostanie wywołana na kontrolerze, jeśli nie została określona jawnie. Trzecia część segmentu adresu URL (`Parameters`) jest dla danych trasy. Będziemy widzieć dane trasy w dalszej części tego samouczka.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. Metoda `Welcome` jest uruchamiana i zwraca ciąg &quot;jest to metoda akcji powitalnej...&quot;. Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontroler jest `HelloWorld` i `Welcome` jest metodą akcji. Nie użyto jeszcze `[Parameters]` części adresu URL.

![](adding-a-controller/_static/image6.png)

Zmodyfikujmy przykład nieco tak, aby można było przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Zmień metodę `Welcome` tak, aby obejmowała dwa parametry, jak pokazano poniżej. Należy zauważyć, że kod używa C# funkcji opcjonalnej parametru, aby wskazać, że parametr `numTimes` powinien domyślnie mieć wartość 1, jeśli dla tego parametru nie zostanie przeniesiona żadnej wartości.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Uwaga dotycząca zabezpieczeń: powyższy kod używa [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) do ochrony aplikacji przed złośliwymi danymi wejściowymi (tj. JavaScript). Aby uzyskać więcej informacji [, zobacz jak: Ochrona przed programami wykorzystującymi luki w zabezpieczeniach w aplikacji sieci Web przez zastosowanie kodowania HTML do ciągów znaków](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).

 Uruchom aplikację i przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Możesz wypróbować różne wartości `name` i `numtimes` w adresie URL. [System powiązań modelu MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwane parametry z ciągu zapytania na pasku adresu na parametry w metodzie.

![](adding-a-controller/_static/image7.png)

W powyższym przykładzie segment adresu URL (`Parameters`) nie jest używany, parametry `name` i `numTimes` są przesyłane jako [ciągi zapytań](http://en.wikipedia.org/wiki/Query_string). Znak ? (znak zapytania) w powyższym adresie URL jest separatorem, a ciągi zapytania są zgodne. Znak &amp; oddziela ciągi zapytań.

Zastąp metodę powitalną następującym kodem:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Uruchom aplikację i wprowadź następujący adres URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Tym razem trzeci segment adresu URL pasuje do parametru Route `ID.` Metoda akcji `Welcome` zawiera parametr (`ID`), który pasował do specyfikacji adresu URL w metodzie `RegisterRoutes`.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

W aplikacjach ASP.NET MVC bardziej typowe jest przekazywanie parametrów jako danych tras (podobnie jak w przypadku powyższego identyfikatora) niż przekazywanie ich jako ciągów zapytań. Można również dodać trasę, aby przekazywać `name` i `numtimes` w parametrach jako dane trasy w adresie URL. W pliku *\_aplikacji Start\RouteConfig.cs* Dodaj trasę "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Uruchom aplikację i przejdź do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

W przypadku wielu aplikacji MVC domyślna trasa działa prawidłowo. Więcej informacji można znaleźć w dalszej części tego samouczka, aby przekazać dane przy użyciu spinacza modelu i nie trzeba będzie modyfikować trasy domyślnej dla tego programu.

W tych przykładach kontroler wykonywał &quot;VC&quot; część MVC — to znaczy, że widok i działanie kontrolera. Kontroler zwraca bezpośrednio kod HTML. Zwykle nie chcesz, aby kontrolery zwracające kod HTML bezpośrednio, ponieważ staną się bardzo uciążliwe w kodzie. Zamiast tego zwykle użyjesz osobnego pliku szablonu widoku, aby ułatwić generowanie odpowiedzi HTML. Przyjrzyjmy się dalej, jak to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](getting-started.md)
> [dalej](adding-a-view.md)

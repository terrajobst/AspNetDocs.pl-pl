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
ms.openlocfilehash: 6b38d757d37374b14979f8a079a46158ff64f9c3
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810765"
---
# <a name="adding-a-controller"></a>Dodawanie kontrolera

Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC oznacza *Model-View-Controller*. MVC to wzorzec służący do tworzenia aplikacji, które są dobrze zaprojektowane, weryfikowalne i łatwe w obsłudze. Aplikacje oparte na MVC zawierają:

- Odels **M** : Klasy reprezentujące dane aplikacji, które używają logiki walidacji do wymuszania reguł dla tych danych.
- Iews **V** : Pliki szablonów używane przez aplikację do dynamicznego generowania odpowiedzi HTML.
- Ontrollers języka **C** : Klasy obsługujące żądania przeglądarki przychodzącej, pobieranie danych modelu, a następnie określanie szablonów wyświetlania, które zwracają odpowiedź do przeglądarki.

Będziemy omawiać wszystkie te koncepcje w tej serii samouczków i pokazują, jak używać ich do kompilowania aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder controllers, a następnie kliknij polecenie **Dodaj**, a następnie pozycję **kontroler**.

![](adding-a-controller/_static/image1.png)

W oknie dialogowym **Dodawanie szkieletu** kliknij pozycję **kontroler MVC 5 — pusty**, a następnie kliknij przycisk **Dodaj**.

![](adding-a-controller/_static/image2.png)  

Nazwij nowy kontroler "HelloWorldController" i kliknij przycisk **Dodaj**.

![Dodaj kontroler](adding-a-controller/_static/image3.png)

Zwróć uwagę, że w **Eksplorator rozwiązań** został utworzony nowy plik o nazwie *HelloWorldController.cs* i nowy folder *Views\HelloWorld*. Kontroler jest otwarty w IDE.

![](adding-a-controller/_static/image4.png)

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontrolera zwrócą ciąg HTML jako przykład. Kontroler ma nazwę `HelloWorldController` , a pierwsza metoda ma nazwę `Index`. Wywołajmy ją z przeglądarki. Uruchom aplikację (naciśnij klawisz F5 lub CTRL + F5). W przeglądarce Dołącz &quot;HelloWorld&quot; do ścieżki na pasku adresu. (Na przykład na poniższej ilustracji `http://localhost:1234/HelloWorld.`) Strona przeglądarki będzie wyglądać podobnie do poniższego zrzutu ekranu. W powyższej metodzie kod zwrócił ciąg bezpośrednio. Otrzymasz informację o tym, że system po prostu zwróci kod HTML.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC wywołuje różne klasy kontrolera (i różne metody akcji w nich) w zależności od przychodzącego adresu URL. Domyślna logika routingu adresów URL używana przez ASP.NET MVC używa formatu, takiego jak to, aby określić kod, który ma zostać wywołany:

`/[Controller]/[ActionName]/[Parameters]`

Format routingu można ustawić w pliku *Start/RouteConfig\_. cs aplikacji* .

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Gdy uruchamiasz aplikację i nie podasz żadnych segmentów adresów URL, domyślnym kontrolerem "Home" i akcją "index" określono w sekcji wartości domyślne w powyższym kodzie.

Pierwsza część adresu URL określa klasę kontrolera do wykonania. Tak więc */HelloWorld* mapuje `HelloWorldController` do klasy. Druga część adresu URL określa metodę akcji dla klasy, która ma zostać wykonana. Dlatego */HelloWorld/index* mogłoby spowodować `Index` wykonanie metody `HelloWorldController` klasy. Zwróć uwagę, że chcemy tylko przeglądać do */HelloWorld* , `Index` a metoda została użyta domyślnie. Wynika to z faktu, `Index` że metoda o nazwie jest metodą domyślną, która zostanie wywołana na kontrolerze, jeśli nie została określona jawnie. Trzecia część segmentu URL ( `Parameters`) jest dla danych trasy. Będziemy widzieć dane trasy w dalszej części tego samouczka.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. Metoda jest uruchamiana i zwraca ciąg &quot;, który jest metodą akcji powitalnej... `Welcome` &quot;. Domyślne mapowanie MVC to `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontroler jest `HelloWorld` i `Welcome` jest metodą akcji. Nie użyto `[Parameters]` jeszcze części adresu URL.

![](adding-a-controller/_static/image6.png)

Zmodyfikujmy przykład nieco tak, aby można było przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). `Welcome` Zmień metodę, tak aby obejmowała dwa parametry, jak pokazano poniżej. Należy zauważyć, że kod używa C# funkcji opcjonalnej parametru, aby wskazać, `numTimes` że parametr powinien domyślnie mieć wartość 1, jeśli dla tego parametru nie zostanie przeniesiona żadnej wartości.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Uwaga dotycząca zabezpieczeń: Powyższy kod używa [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) do ochrony aplikacji przed złośliwymi danymi wejściowymi (tj. JavaScript). Aby uzyskać więcej informacji, zobacz [jak: Ochrona przed programami wykorzystującymi luki w zabezpieczeniach w aplikacji sieci Web przez zastosowanie](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)kodowania HTML do ciągów znaków.

 Uruchom aplikację i przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Możesz wypróbować różne wartości `name` dla `numtimes` i w adresie URL. [System powiązań modelu MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwane parametry z ciągu zapytania na pasku adresu na parametry w metodzie.

![](adding-a-controller/_static/image7.png)

W powyższym przykładzie segment adresu URL ( `Parameters`) nie jest używany `name` , a parametry `numTimes` i są przesyłane jako [ciągi zapytań](http://en.wikipedia.org/wiki/Query_string). Polu? (znak zapytania) w powyższym adresie URL jest separatorem, a ciągi zapytania są zgodne. &amp; Znak oddziela ciągi zapytań.

Zastąp metodę powitalną następującym kodem:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Uruchom aplikację i wprowadź następujący adres URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Tym razem trzeci segment adresu `ID.` URL pasuje do parametru `Welcome` Route Metoda Action zawiera parametr (`ID` `RegisterRoutes` ) pasujący do specyfikacji adresu URL w metodzie.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

W aplikacjach ASP.NET MVC bardziej typowe jest przekazywanie parametrów jako danych tras (podobnie jak w przypadku powyższego identyfikatora) niż przekazywanie ich jako ciągów zapytań. Można również dodać trasę do przekazywania zarówno `name` parametrów, jak i `numtimes` w, jako dane trasy w adresie URL. W pliku *Start\RouteConfig.cs\_aplikacji* Dodaj trasę "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Uruchom aplikację i przejdź do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

W przypadku wielu aplikacji MVC domyślna trasa działa prawidłowo. Więcej informacji można znaleźć w dalszej części tego samouczka, aby przekazać dane przy użyciu spinacza modelu i nie trzeba będzie modyfikować trasy domyślnej dla tego programu.

W tych przykładach kontroler &quot;wykonywał składnik VC&quot; składnika MVC — to znaczy, że widok i działanie kontrolera. Kontroler zwraca bezpośrednio kod HTML. Zwykle nie chcesz, aby kontrolery zwracające kod HTML bezpośrednio, ponieważ staną się bardzo uciążliwe w kodzie. Zamiast tego zwykle użyjesz osobnego pliku szablonu widoku, aby ułatwić generowanie odpowiedzi HTML. Przyjrzyjmy się dalej, jak to zrobić.

> [!div class="step-by-step"]
> [Poprzedni](getting-started.md)Następny
> [](adding-a-view.md)

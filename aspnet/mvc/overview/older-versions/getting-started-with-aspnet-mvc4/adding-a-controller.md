---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Dodawanie kontrolera | Microsoft Docs
author: Rick-Anderson
description: 'Uwaga: zaktualizowana wersja tego samouczka jest dostępna w tym miejscu, w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f528c56435976c7f31fce453c834ef9eaebe6244
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456105"
---
# <a name="adding-a-controller"></a>Dodawanie kontrolera

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.

MVC oznacza *Model-View-Controller*. MVC to wzorzec służący do tworzenia aplikacji, które są dobrze zaprojektowane, weryfikowalne i łatwe w obsłudze. Aplikacje oparte na MVC zawierają:

- **M** Odels: klasy reprezentujące dane aplikacji i używające logiki walidacji do wymuszania reguł dla tych danych.
- **V** iews: pliki szablonów używane przez aplikację do dynamicznego generowania odpowiedzi html.
- **C** Ontrollers: klasy obsługujące żądania przeglądarki przychodzącej, pobieranie danych modelu, a następnie określanie szablonów wyświetlania, które zwracają odpowiedź do przeglądarki.

Będziemy omawiać wszystkie te koncepcje w tej serii samouczków i pokazują, jak używać ich do kompilowania aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , a następnie wybierz polecenie **Dodaj kontroler**.

![](adding-a-controller/_static/image1.png)

Nazwij nowy kontroler &quot;HelloWorldController&quot;. Pozostaw szablon domyślny jako **pusty kontroler MVC** , a następnie kliknij przycisk **Dodaj**.

![Dodaj kontroler](adding-a-controller/_static/image2.png)

Zwróć uwagę, że w **Eksplorator rozwiązań** został utworzony nowy plik o nazwie *HelloWorldController.cs*. Plik jest otwarty w środowisku IDE.

![](adding-a-controller/_static/image3.png)

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontrolera zwrócą ciąg HTML jako przykład. Kontroler ma nazwę `HelloWorldController`, a pierwsza z powyższych metod nosi nazwę `Index`. Wywołajmy ją z przeglądarki. Uruchom aplikację (naciśnij klawisz F5 lub CTRL + F5). W przeglądarce Dołącz &quot;HelloWorld&quot; do ścieżki na pasku adresu. (Na przykład na poniższej ilustracji jest `http://localhost:1234/HelloWorld.`) Strona w przeglądarce będzie wyglądać podobnie do poniższego zrzutu ekranu. W powyższej metodzie kod zwrócił ciąg bezpośrednio. Otrzymasz informację o tym, że system po prostu zwróci kod HTML.

![](adding-a-controller/_static/image4.png)

ASP.NET MVC wywołuje różne klasy kontrolera (i różne metody akcji w nich) w zależności od przychodzącego adresu URL. Domyślna logika routingu adresów URL używana przez ASP.NET MVC używa formatu, takiego jak to, aby określić kod, który ma zostać wywołany:

`/[Controller]/[ActionName]/[Parameters]`

Pierwsza część adresu URL określa klasę kontrolera do wykonania. Tak więc */HelloWorld* mapuje do klasy `HelloWorldController`. Druga część adresu URL określa metodę akcji dla klasy, która ma zostać wykonana. Dlatego */HelloWorld/index* może spowodować wykonanie metody `Index` klasy `HelloWorldController`. Zwróć uwagę, że tylko chcemy przeglądać do */HelloWorld* , a metoda `Index` była używana domyślnie. Wynika to z faktu, że metoda o nazwie `Index` jest metodą domyślną, która zostanie wywołana na kontrolerze, jeśli nie została określona jawnie.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. Metoda `Welcome` jest uruchamiana i zwraca ciąg &quot;jest to metoda akcji powitalnej...&quot;. Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontroler jest `HelloWorld` i `Welcome` jest metodą akcji. Nie użyto jeszcze `[Parameters]` części adresu URL.

![](adding-a-controller/_static/image5.png)

Zmodyfikujmy przykład nieco tak, aby można było przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Zmień metodę `Welcome` tak, aby obejmowała dwa parametry, jak pokazano poniżej. Należy zauważyć, że kod używa C# funkcji opcjonalnej parametru, aby wskazać, że parametr `numTimes` powinien domyślnie mieć wartość 1, jeśli dla tego parametru nie zostanie przeniesiona żadnej wartości.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uruchom aplikację i przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Możesz wypróbować różne wartości `name` i `numtimes` w adresie URL. [System powiązań modelu MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwane parametry z ciągu zapytania na pasku adresu na parametry w metodzie.

![](adding-a-controller/_static/image6.png)

W obu tych przykładach kontroler wykonywał &quot;VC&quot; część MVC — to znaczy, że widok i działanie kontrolera. Kontroler zwraca bezpośrednio kod HTML. Zwykle nie chcesz, aby kontrolery zwracające kod HTML bezpośrednio, ponieważ staną się bardzo uciążliwe w kodzie. Zamiast tego zwykle użyjesz osobnego pliku szablonu widoku, aby ułatwić generowanie odpowiedzi HTML. Przyjrzyjmy się dalej, jak to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-4.md)
> [dalej](adding-a-view.md)

---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Dodawanie kontrolera (C#) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 959116ff773f4ef466cda6b172e8321590b50e5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540944"
---
# <a name="adding-a-controller-c"></a>Dodawanie kontrolera (C#)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.
> 
> 
> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu. [Pobierz wersję C# programu](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przejdź do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.

MVC oznacza *Model-View-Controller*. MVC to wzorzec służący do tworzenia aplikacji, które są dobrze zaprojektowane i łatwe w obsłudze. Aplikacje oparte na MVC zawierają:

- Kontrolery: klasy obsługujące żądania przychodzące do aplikacji, pobieranie danych modelu, a następnie określanie szablonów wyświetlania, które zwracają odpowiedź do klienta.
- Modele: klasy reprezentujące dane aplikacji, które używają logiki walidacji do wymuszania reguł dla tych danych.
- Widoki: pliki szablonów używane przez aplikację do dynamicznego generowania odpowiedzi HTML.

Będziemy omawiać wszystkie te koncepcje w tej serii samouczków i pokazują, jak używać ich do kompilowania aplikacji.

Zacznijmy od utworzenia klasy kontrolera. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , a następnie wybierz polecenie **Dodaj kontroler**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nazwij nowy kontroler "HelloWorldController". Pozostaw szablon domyślny jako **pusty kontroler** , a następnie kliknij przycisk **Dodaj**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Zwróć uwagę, że w **Eksplorator rozwiązań** został utworzony nowy plik o nazwie *HelloWorldController.cs*. Plik jest otwarty w środowisku IDE.

![](adding-a-controller/_static/image5.png)

W bloku `public class HelloWorldController` Utwórz dwie metody, które wyglądają jak w poniższym kodzie. Kontroler zwróci jako przykład ciąg HTML.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Twój kontroler ma nazwę `HelloWorldController`, a pierwsza z powyższych metod nosi nazwę `Index`. Wywołajmy ją z przeglądarki. Uruchom aplikację (naciśnij klawisz F5 lub CTRL + F5). W przeglądarce Dołącz "HelloWorld" do ścieżki na pasku adresu. (Na przykład na poniższej ilustracji jest `http://localhost:43246/HelloWorld.`) Strona w przeglądarce będzie wyglądać podobnie do poniższego zrzutu ekranu. W powyższej metodzie kod zwrócił ciąg bezpośrednio. Otrzymasz informację o tym, że system po prostu zwróci kod HTML.

![](adding-a-controller/_static/image6.png)

ASP.NET MVC wywołuje różne klasy kontrolera (i różne metody akcji w nich) w zależności od przychodzącego adresu URL. Domyślna logika mapowania używana przez ASP.NET MVC używa formatu, takiego jak to, aby określić kod, który ma zostać wywołany:

`/[Controller]/[ActionName]/[Parameters]`

Pierwsza część adresu URL określa klasę kontrolera do wykonania. Tak więc */HelloWorld* mapuje do klasy `HelloWorldController`. Druga część adresu URL określa metodę akcji dla klasy, która ma zostać wykonana. Dlatego */HelloWorld/index* może spowodować wykonanie metody `Index` klasy `HelloWorldController`. Zwróć uwagę, że tylko chcemy przeglądać do */HelloWorld* , a metoda `Index` była używana domyślnie. Wynika to z faktu, że metoda o nazwie `Index` jest metodą domyślną, która zostanie wywołana na kontrolerze, jeśli nie została określona jawnie.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. Metoda `Welcome` jest uruchamiana i zwraca ciąg "to jest metoda akcji powitalnej...". Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontroler jest `HelloWorld` i `Welcome` jest metodą akcji. Nie użyto jeszcze `[Parameters]` części adresu URL.

![](adding-a-controller/_static/image7.png)

Zmodyfikujmy przykład nieco tak, aby można było przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Zmień metodę `Welcome` tak, aby obejmowała dwa parametry, jak pokazano poniżej. Należy zauważyć, że kod używa C# funkcji opcjonalnej parametru, aby wskazać, że parametr `numTimes` powinien domyślnie mieć wartość 1, jeśli dla tego parametru nie zostanie przeniesiona żadnej wartości.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Uruchom aplikację i przejdź do przykładowego adresu URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Możesz wypróbować różne wartości `name` i `numtimes` w adresie URL. System automatycznie mapuje nazwane parametry z ciągu zapytania na pasku adresu do parametrów w metodzie.

![](adding-a-controller/_static/image8.png)

W obu tych przykładach kontroler wykonywał część "VC" składnika MVC — to znaczy, że widok i działanie kontrolera. Kontroler zwraca bezpośrednio kod HTML. Zwykle nie chcesz, aby kontrolery zwracające kod HTML bezpośrednio, ponieważ staną się bardzo uciążliwe w kodzie. Zamiast tego zwykle użyjesz osobnego pliku szablonu widoku, aby ułatwić generowanie odpowiedzi HTML. Przyjrzyjmy się dalej, jak to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-3.md)
> [dalej](adding-a-view.md)

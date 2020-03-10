---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Dodawanie kontrolera (VB) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e77f62a9796211b0e59a99c71bc532659b7cb92
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540517"
---
# <a name="adding-a-controller-vb"></a>Dodawanie kontrolera (VB)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępny do tego tematu. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przejdź do [ C# wersji](../cs/adding-a-controller.md) tego samouczka.

MVC oznacza *Model-View-Controller*. MVC to wzorzec służący do tworzenia aplikacji w taki sposób, aby każda część była odrębną odpowiedzialnością:

- Model: dane dla aplikacji.
- Widoki: pliki szablonów używane przez aplikację do dynamicznego generowania odpowiedzi HTML.
- Kontrolery: klasy obsługujące przychodzące żądania adresów URL do aplikacji, pobieranie danych modelu, a następnie określanie szablonów wyświetlania, które renderują odpowiedź do klienta.

Będziemy omawiać wszystkie te koncepcje w tym samouczku i pokazują, jak używać ich do kompilowania aplikacji.

Utwórz nowy kontroler, klikając prawym przyciskiem myszy folder *controllers* w **Eksplorator rozwiązań** a następnie wybierając pozycję **Dodaj kontroler**.

[![Addcontroller](adding-a-controller/_static/image2.png "Addcontroller")](adding-a-controller/_static/image1.png)

Nazwij nowy kontroler &quot;HelloWorldController&quot; a następnie kliknij przycisk **Dodaj**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Zwróć uwagę na **Eksplorator rozwiązań** po prawej stronie, że został utworzony nowy plik o nazwie *HelloWorldController.cs* i że plik jest otwarty w środowisku IDE.

W nowym bloku `public class HelloWorldController` Utwórz dwie nowe metody, które wyglądają jak w poniższym kodzie. Na przykład będziemy zwracać ciąg HTML bezpośrednio z kontrolera.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Twój kontroler ma nazwę `HelloWorldController` a nowa metoda ma nazwę `Index`. Uruchom aplikację (naciśnij klawisz F5 lub CTRL + F5). Po uruchomieniu przeglądarki Dołącz &quot;HelloWorld&quot; do ścieżki na pasku adresu. (Na komputerze jest `http://localhost:43246/HelloWorld`) Przeglądarka będzie wyglądać jak na poniższym zrzucie ekranu. W powyższej metodzie kod zwrócił ciąg bezpośrednio. Informujemy system, aby po prostu zwracał część HTML i

![](adding-a-controller/_static/image5.png)

ASP.NET MVC wywołuje różne klasy kontrolera (i różne metody akcji w nich) w zależności od przychodzącego adresu URL. Domyślna logika mapowania używana przez ASP.NET MVC używa formatu, takiego jak ten, aby kontrolować kod, który jest wywoływany:

`/[Controller]/[ActionName]/[Parameters]`

Pierwsza część adresu URL określa klasę kontrolera do wykonania. Tak więc */HelloWorld* mapuje do klasy `HelloWorldController`. Druga część adresu URL określa metodę akcji dla klasy, która ma zostać wykonana. Dlatego */HelloWorld/index* może spowodować wykonanie metody `Index` klasy `HelloWorldController`. Zwróć uwagę, że mamy tylko odwiedzić */HelloWorld* powyżej, a metoda `Index` była używana domyślnie. Wynika to z faktu, że metoda o nazwie `Index` jest metodą domyślną, która zostanie wywołana na kontrolerze, jeśli nie została określona jawnie.

Przejdź do `http://localhost:xxxx/HelloWorld/Welcome`. Metoda `Welcome` jest uruchamiana i zwraca ciąg &quot;jest to metoda akcji powitalnej...&quot;. Domyślne mapowanie MVC jest `/[Controller]/[ActionName]/[Parameters]`. Dla tego adresu URL kontroler jest `HelloWorld` i `Welcome` jest metodą. Nie użyto jeszcze `[Parameters]` części adresu URL.

![](adding-a-controller/_static/image6.png)

Zmodyfikujmy przykład nieco tak, aby można było przekazać niektóre informacje o parametrach z adresu URL do kontrolera (na przykład */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Zmień metodę `Welcome` tak, aby obejmowała dwa parametry, jak pokazano poniżej. Należy zauważyć, że funkcja opcjonalnego parametru języka VB została użyta w celu wskazania, że parametr `numTimes` powinien domyślnie mieć wartość 1, jeśli dla tego parametru nie zostanie przeniesiona żadnej wartości.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Uruchom aplikację i przejdź do `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Możesz wypróbować różne wartości `name` i `numtimes`. System automatycznie mapuje nazwane parametry z ciągu zapytania na pasku adresu na parametry w metodzie.

![](adding-a-controller/_static/image7.png)

W obu tych przykładach kontroler wykonywał składnik VC składnika MVC — jest to działanie widoku i kontrolera. Kontroler zwraca bezpośrednio kod HTML. Zwykle nie chcemy, aby kontrolery zwracające kod HTML bezpośrednio, ponieważ staną się bardzo uciążliwe w kodzie. Zamiast tego zwykle użyjesz osobnego pliku szablonu widoku, aby ułatwić generowanie odpowiedzi HTML. Przyjrzyjmy się, jak to zrobić.

> [!div class="step-by-step"]
> [Poprzednie](intro-to-aspnet-mvc-3.md)
> [dalej](adding-a-view.md)

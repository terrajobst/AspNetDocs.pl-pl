---
uid: visual-studio/overview/2013/using-browser-link
title: Korzystanie z linku przeglądarki w Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622907"
---
# <a name="using-browser-link-in-visual-studio-2013"></a>Korzystanie z linku przeglądarki w Visual Studio 2013

według [Jan Wasson](https://github.com/MikeWasson)

Link do przeglądarki to nowa funkcja w Visual Studio 2013, która tworzy kanał komunikacyjny między środowiskiem deweloperskim i jedną lub wieloma przeglądarkami sieci Web. Możesz użyć linku przeglądarki, aby odświeżyć aplikację sieci Web w kilku przeglądarkach jednocześnie, co jest przydatne w przypadku testowania między przeglądarkami.

- [Odświeżanie przeglądarki](#browser-refresh)
- [Wyświetlanie pulpitu nawigacyjnego linku przeglądarki](#dashboard)
- [Włączanie linku przeglądarki dla statycznych plików HTML](#static-html)
- [Wyłączanie linku przeglądarki](#disabling)
- [Jak to działa?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Odświeżanie przeglądarki

Po odświeżeniu przeglądarki możesz odświeżyć wiele przeglądarek, które są połączone z programem Visual Studio za pomocą linku przeglądarki.

Aby użyć funkcji odświeżania przeglądarki, należy najpierw utworzyć aplikację ASP.NET przy użyciu dowolnego szablonu projektu. Debuguj aplikację, naciskając klawisz F5 lub klikając ikonę strzałki na pasku narzędzi:

![](using-browser-link/_static/image1.png)

Możesz również użyć listy rozwijanej, aby wybrać określoną przeglądarkę do debugowania.

![](using-browser-link/_static/image2.png)

Aby debugować z wieloma przeglądarkami, wybierz pozycję **Przeglądaj za pomocą**. W oknie dialogowym **Przeglądaj przy użyciu** naciśnij i przytrzymaj klawisz CTRL, aby wybrać więcej niż jedną przeglądarkę. Kliknij przycisk **Przeglądaj** , aby debugować wybrane przeglądarki. Łącze przeglądarki działa również w przypadku uruchamiania przeglądarki spoza programu Visual Studio i przejścia do adresu URL aplikacji.

![](using-browser-link/_static/image3.png)

Formanty łączy przeglądarki znajdują się na liście rozwijanej z ikoną okrągłą strzałką. Ikona strzałki to przycisk **Odśwież** .

![](using-browser-link/_static/image4.png)

Aby sprawdzić, które przeglądarki są połączone, umieść kursor myszy nad przyciskiem **odświeżania** podczas debugowania. Połączone przeglądarki są wyświetlane w oknie etykietki narzędzi.

![](using-browser-link/_static/image5.png)

Aby odświeżyć połączone przeglądarki, kliknij przycisk **Odśwież** lub naciśnij klawisze CTRL + ALT + ENTER. Na przykład poniższy zrzut ekranu przedstawia projekt ASP.NET, który został utworzony przy użyciu szablonu projektu MVC 5. Możesz zobaczyć, że aplikacja działa w dwóch przeglądarkach u góry. W dolnej części projektu jest otwarty w programie Visual Studio.

![](using-browser-link/_static/image6.png)

W programie Visual Studio zmieniono nagłówek &lt;H1&gt; dla strony głównej:

![](using-browser-link/_static/image7.png)

Po kliknięciu przycisku **Odśwież** zmiany pojawiły się w obu oknach przeglądarki:

![](using-browser-link/_static/image8.png)

**Uwagi**

- Aby włączyć łącze przeglądarki, ustaw `debug=true` w elemencie [&lt;kompilacja&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) w pliku Web. config projektu.
- Aplikacja musi być uruchomiona na hoście lokalnym.
- Aplikacja musi być ukierunkowana na platformę .NET 4,0 lub nowszą.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Wyświetlanie pulpitu nawigacyjnego linku przeglądarki

Pulpit nawigacyjny linku przeglądarki pokazuje informacje o połączeniach z przeglądarką. Aby wyświetlić pulpit nawigacyjny, wybierz menu rozwijane łącze przeglądarki (mała strzałka obok przycisku **odświeżania** ). Następnie kliknij pozycję **pulpit nawigacyjny link do przeglądarki**.

![](using-browser-link/_static/image9.png)

Pulpit nawigacyjny wyświetla listę podłączonych przeglądarek i adres URL, do którego przejdzie Każda przeglądarka.

![](using-browser-link/_static/image10.png)

Sekcja **wymagania wstępne** zawiera wszystkie kroki niezbędne do włączenia linku przeglądarki dla tego projektu. Na przykład poniższy zrzut ekranu przedstawia projekt, w którym w pliku Web. config jest ustawiony parametr "debug" na wartość false.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Włączanie linku przeglądarki dla statycznych plików HTML

Aby włączyć łącze przeglądarki dla statycznych plików HTML, Dodaj następujące elementy do pliku Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Ze względu na wydajność Usuń to ustawienie przy publikowaniu projektu.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Wyłączanie linku przeglądarki

Link przeglądarki jest domyślnie włączony. Istnieje kilka sposobów jego wyłączenia:

- W menu rozwijanym łącze przeglądarki usuń zaznaczenie pola wyboru **Włącz łącze przeglądarki**. 

    ![](using-browser-link/_static/image12.png)
- W pliku Web. config Dodaj klucz o nazwie "vs: EnableBrowserLink" o wartości "false" w sekcji appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- W pliku Web. config Ustaw debugowanie na false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Jak to działa?

Link przeglądarki używa programu [sygnalizującego](../../../signalr/index.md) do utworzenia kanału komunikacyjnego między programem Visual Studio i przeglądarką. Gdy łącze przeglądarki jest włączone, program Visual Studio działa jako serwer sygnalizujący, z którym może się łączyć wielu klientów (przeglądarki). Link przeglądarki rejestruje także moduł HTTP z ASP.NET. Ten moduł wprowadza specjalne odwołania &lt;skryptu&gt; do każdego żądania strony z serwera. Odwołania do skryptów można zobaczyć, wybierając pozycję "Wyświetl Źródło" w przeglądarce.

![](using-browser-link/_static/image13.png)

Pliki źródłowe nie są modyfikowane. Moduł HTTP wprowadza odwołania do skryptów dynamicznie.

Ponieważ kod po stronie przeglądarki jest językiem JavaScript, działa on we wszystkich przeglądarkach, które [obsługuje program sygnalizujący](../../../signalr/overview/getting-started/supported-platforms.md), bez konieczności używania wtyczki przeglądarki.

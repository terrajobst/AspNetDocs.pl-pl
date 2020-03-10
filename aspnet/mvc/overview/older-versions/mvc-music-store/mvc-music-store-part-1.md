---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Część 1: przegląd i plik > Nowy projekt | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 1 obejmuje przegląd i plik > Nowy projekt.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559984"
---
# <a name="part-1-overview-and-file-new-project"></a>Część 1. Omówienie i Plik->Nowy projekt

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 1 obejmuje przegląd i plik&gt;nowy projekt.

## <a name="overview"></a>Omówienie

Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Web Developer for Web Development. Zaczniemy powoli, więc środowisko programistyczne dla początkujących użytkowników jest w stanie dobra.

Aplikacja, którą będziemy kompilować, to prosty magazyn utworów muzycznych. Istnieją trzy główne części aplikacji: zakupy, Wyewidencjonowywanie i administrowanie.

![](mvc-music-store-part-1/_static/image1.jpg)

Goście mogą przeglądać albumy według gatunku:

![](mvc-music-store-part-1/_static/image2.jpg)

Mogą oni wyświetlać jeden album i dodawać go do swojego koszyka:

![](mvc-music-store-part-1/_static/image3.jpg)

Mogą przeglądać ich koszyk, usuwając wszystkie elementy, które nie są już potrzebne:

![](mvc-music-store-part-1/_static/image4.jpg)

W wyniku wyewidencjonowania zostanie wyświetlony monit o zalogowanie lub zarejestrowanie konta użytkownika.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Po utworzeniu konta można wykonać zamówienie, wypełniając informacje o wysyłce i płatności. Aby zachować prostotę, korzystamy z wspaniałej promocji: wszystko jest bezpłatne, jeśli wprowadzisz kod promocyjny "bezpłatnie"!

![](mvc-music-store-part-1/_static/image5.jpg)

Po określeniu kolejności zobaczysz prosty ekran potwierdzenia:

![](mvc-music-store-part-1/_static/image6.jpg)

Oprócz stron przeznaczonych dla klientów zostanie również utworzona sekcja administrator, która zawiera listę albumów, z których administratorzy mogą tworzyć, edytować i usuwać albumy:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. plik —&gt; nowy projekt

### <a name="installing-the-software"></a>Instalowanie oprogramowania

Ten samouczek rozpoczyna się od utworzenia nowego projektu ASP.NET MVC 3 przy użyciu bezpłatnego programu Visual Web Developer 2010 Express (który jest bezpłatny), a następnie zwiększy możliwości, aby utworzyć kompletną, działającą aplikację. W ten sposób będziemy obejmować dostęp do bazy danych, scenariusze publikowania formularzy i sprawdzanie poprawności danych przy użyciu stron wzorcowych dla spójnego układu strony przy użyciu technologii AJAX do aktualizacji stron i weryfikacji, logowania użytkownika i innych.

Możesz wykonać instrukcje krok po kroku lub pobrać ukończoną aplikację ze [sklepu MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Do skompilowania aplikacji można użyć programu Visual Studio 2010 z dodatkiem SP1 lub Visual Web Developer 2010 Express SP1 (bezpłatna wersja programu Visual Studio 2010). Do hostowania bazy danych będziemy używać SQL Server Compact (również wolnych). Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej.

- [Wymagania wstępne programu Visual Studio Web Developer Express SP1]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4,0] — w tym obsługa środowiska uruchomieniowego i narzędzi

### <a name="creating-a-new-aspnet-mvc-3-project"></a>Tworzenie nowego projektu ASP.NET MVC 3

Zaczniemy od wybrania pozycji "nowy projekt" z menu plik w programie Visual Web Developer. Spowoduje to wyświetlenie okna dialogowego Nowy projekt.

![](mvc-music-store-part-1/_static/image5.png)

Po lewej stronie wybierzesz C# grupę szablonów sieci Web dla wizualizacji&gt;, a następnie wybierz szablon "aplikacja sieci Web ASP.NET MVC 3" w środkowej kolumnie. Nazwij projekt MvcMusicStore i naciśnij przycisk OK.

![](mvc-music-store-part-1/_static/image8.jpg)

Spowoduje to wyświetlenie pomocniczego okna dialogowego, które umożliwi nam wprowadzanie określonych ustawień MVC dla projektu. Wybierz następujące opcje:

Szablon projektu — wybierz opcję pusty

Wyświetl aparat — wybierz Razor

Użyj znacznika semantyki HTML5 — zaznaczone

Sprawdź, czy ustawienia są następujące, jak pokazano poniżej, a następnie naciśnij przycisk OK.

![](mvc-music-store-part-1/_static/image9.jpg)

Spowoduje to utworzenie naszego projektu. Zapoznaj się z folderami, które zostały dodane do naszej aplikacji w Eksplorator rozwiązań po prawej stronie.

![](mvc-music-store-part-1/_static/image10.jpg)

Pusty szablon MVC 3 nie jest całkowicie pusty — dodaje podstawową strukturę folderów:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC wykorzystuje podstawowe konwencje nazewnictwa dla nazw folderów:

| **Folder** | **Przeznaczenie** |
| --- | --- |
| **/Controllers** | Kontrolery odpowiadają na dane wejściowe z przeglądarki, decydują o tym, co należy zrobić z nim, i zwracają odpowiedź do użytkownika. |
| **/Views** | Widoki przechowują szablony interfejsu użytkownika |
| **/Models** | Modele przechowują dane i manipulowanie nimi |
| **/Content** | Ten folder zawiera nasze obrazy, CSS i wszelkie inne zawartość statyczną |
| **/Scripts** | Ten folder zawiera nasze pliki JavaScript |

Te foldery są uwzględniane nawet w pustej aplikacji ASP.NET MVC, ponieważ struktura ASP.NET MVC domyślnie używa podejścia "Konwencja od konfiguracji" i wprowadza pewne domyślne założenia na podstawie konwencji nazewnictwa folderów. Na przykład kontrolery szukają widoków w folderze widoki domyślnie, bez konieczności jawnego określania tego w kodzie. Naklejanie z konwencjami domyślnymi zmniejsza ilość kodu, który trzeba napisać, i ułatwia innym deweloperom zrozumienie Twojego projektu. Wyjaśnimy te konwencje bardziej jak w przypadku kompilowania aplikacji.

> [!div class="step-by-step"]
> [Dalej](mvc-music-store-part-2.md)

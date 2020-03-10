---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Część 2: kontrolery | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 2 obejmuje kontrolery.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559879"
---
# <a name="part-2-controllers"></a>Część 2. Kontrolery

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 2 obejmuje kontrolery.

W przypadku tradycyjnych platform sieci Web przychodzące adresy URL są zazwyczaj mapowane na pliki na dysku. Na przykład: żądanie dotyczące adresu URL, takiego jak "/Products.aspx" lub "/Products.php", może być przetwarzane przez plik "Products. aspx" lub "Products. php".

Sieci Web platformy MVC mapują adresy URL na kod serwera w nieco inny sposób. Zamiast mapować przychodzące adresy URL na pliki, zamiast tego mapują adresy URL na metody klasy. Klasy te są nazywane "kontrolerami" i są odpowiedzialne za przetwarzanie przychodzących żądań HTTP, obsługę danych wejściowych użytkownika, pobieranie i zapisywanie danych oraz określanie odpowiedzi na odesłanie do klienta (wyświetlanie kodu HTML, pobieranie pliku, przekierowanie do innego Adres URL itp.).

## <a name="adding-a-homecontroller"></a>Dodawanie elementu HomeController

Zaczniemy nasze aplikacje ze sklepu MVC Music, dodając klasę kontrolera, która będzie obsługiwać adresy URL na stronie głównej naszej witryny. Będziemy przestrzegać domyślnych konwencji nazewnictwa ASP.NET MVC i wywoływać IT HomeController.

Kliknij prawym przyciskiem myszy folder "controllers" w ramach Eksplorator rozwiązań i wybierz pozycję "Dodaj", a następnie "kontroler..." dotyczące

![](mvc-music-store-part-2/_static/image1.jpg)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie kontrolera". Nazwij kontroler "HomeController" i naciśnij przycisk Dodaj.

![](mvc-music-store-part-2/_static/image1.png)

Spowoduje to utworzenie nowego pliku HomeController.cs z następującym kodem:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Aby zacząć jak najszybciej, Zamień metodę index na prostą metodę, która po prostu zwraca ciąg. Wprowadzimy dwie zmiany:

- Zmień metodę, aby zwracała ciąg zamiast elementu ActionResult
- Zmień instrukcję return, aby zwracała wartość "Hello z domu"

Metoda powinna teraz wyglądać następująco:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz Uruchommy lokację. Możemy uruchomić nasz serwer sieci Web i wypróbować lokację przy użyciu dowolnego z następujących elementów:

- Wybierz element menu Debuguj debugowanie ⇨
- Kliknij przycisk Zielona strzałka na pasku narzędzi ![](mvc-music-store-part-2/_static/image2.jpg)
- Użyj skrótu klawiaturowego F5.

Użycie któregokolwiek z powyższych kroków spowoduje skompilowanie projektu, a następnie uruchomienie serwera deweloperskiego ASP.NET, który jest wbudowanym Visual Web Developer. W dolnym rogu ekranu zostanie wyświetlone powiadomienie informujące o tym, że serwer ASP.NET Development został uruchomiony i zostanie wyświetlony numer portu, w którym jest uruchomiony.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer następnie automatycznie otworzy okno przeglądarki, którego adres URL wskazuje nasz serwer sieci Web. Pozwoli to na szybkie wypróbowanie naszej aplikacji sieci Web:

![](mvc-music-store-part-2/_static/image3.png)

Tak stało się bardzo szybkie — utworzyliśmy nową witrynę sieci Web, dodaliśmy funkcję z trzema wierszami, a w przeglądarce mamy tekst. Nie nauka rakiet, ale jest rozpoczęciem.

*Uwaga: Visual Web Developer zawiera serwer ASP.NET Development, który będzie uruchamiał witrynę internetową przy użyciu losowego, bezpłatnego numeru "Port". Na poniższym zrzucie ekranu lokacja jest uruchomiona w `http://localhost:26641/`, więc korzysta z portu 26641. Numer portu będzie różny. Gdy będziemy komunikować się z adresem URL podobnym do/Store/Browse w tym samouczku, będzie on przekroczyć numer portu. Przy założeniu, że numer portu 26641, przeglądanie do/Store/Browse będzie oznaczało przechodzenie do `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Dodawanie elementu StoreController

Dodaliśmy prostą HomeController, która implementuje stronę główną naszej witryny. Dodajmy teraz kolejny kontroler, którego będziemy używać do implementowania funkcji przeglądania naszego sklepu z muzyką. Nasz kontroler sklepu będzie obsługiwał trzy scenariusze:

- Strona z listą gatunków utworów muzycznych w naszym sklepie z muzyką
- Strona przeglądania zawierająca listę wszystkich albumów muzycznych w konkretnym gatunku
- Strona szczegółów, która zawiera informacje o konkretnym albumie muzycznym

Zaczniemy od dodania nowej klasy StoreController.. Jeśli jeszcze tego nie zrobiono, Zatrzymaj uruchamianie aplikacji przez zamknięcie przeglądarki lub wybranie elementu menu Debuguj ⇨ zatrzymanie debugowania.

Teraz Dodaj nowy StoreController. Podobnie jak w przypadku HomeController, wykonajmy tę czynność, klikając prawym przyciskiem myszy folder "controllers" w Eksplorator rozwiązań i wybierając pozycję menu kontrolera Dodaj kontroler&gt;

![](mvc-music-store-part-2/_static/image4.png)

Nasz nowy StoreController ma już metodę "index". Użyjemy tej metody "index", aby zaimplementować naszą stronę aukcji, która zawiera listę wszystkich gatunków w naszym sklepie z muzyką. Będziemy również dodawać dwie dodatkowe metody, aby zaimplementować te dwa inne scenariusze, które chcemy obsługiwać nasze StoreController: Przeglądaj i szczegóły.

Te metody (index, Browse i Details) w naszym kontrolerze nazywają się "akcjami kontrolera", jak już widzisz metodę akcji HomeController. index (), ich zadaniem jest odpowiadanie na żądania adresów URL i (ogólnie mówiąc) Ustalanie zawartości powinien zostać wysłany z powrotem do przeglądarki lub użytkownika, który wywołał adres URL.

Zaczniemy nasze implementacje StoreController, zmieniając metodę theIndex () w celu zwrócenia ciągu "Hello from Store. index ()" i dodamy podobne metody dla przeglądania () i szczegółów ():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Uruchom projekt ponownie i przejdź do następujących adresów URL:

- /Store
- /Store/Browse
- /Store/Details

Dostęp do tych adresów URL spowoduje wywołanie metod akcji w naszym kontrolerze i zwrócenie odpowiedzi na ciąg:

![](mvc-music-store-part-2/_static/image5.png)

Jest to doskonałe rozwiązanie, ale są to tylko stałe ciągi. Zmieńmy je na dynamiczne, aby pobierali informacje z adresu URL i wyświetlały je w danych wyjściowych strony.

Najpierw zmienimy metodę przeglądania w celu pobrania wartości QueryString z adresu URL. Możemy to zrobić, dodając parametr "gatunek" do naszej metody działania. Gdy to zrobisz, ASP.NET MVC automatycznie przekaże wszystkie parametry kwerendy lub formularza post o nazwie "gatunek" do naszej metody działania, gdy zostanie wywołana.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Uwaga: używamy metody narzędzia HttpUtility. HtmlEncode do oczyszczenia danych wejściowych użytkownika. Uniemożliwia to użytkownikom wprowadzanie kodu JavaScript do naszego widoku przy użyciu linku, takiego jak/Store/Browse? Gatunek =&lt;&gt;Window. Location = "http://hackersite.com"&lt;/Script&gt;.*

Teraz przejdźmy do/Store/Browse? Gatunek = Disco

![](mvc-music-store-part-2/_static/image6.png)

Następnie zmień akcję szczegóły w celu odczytania i wyświetlenia parametru wejściowego o nazwie ID. W przeciwieństwie do poprzedniej metody, nie będziemy osadzać wartości identyfikatora jako parametru QueryString. Zamiast tego osadzimy go bezpośrednio w samym adresie URL. Na przykład:/Store/Details/5.

ASP.NET MVC pozwala łatwo to zrobić bez konieczności konfigurowania żadnych elementów. Domyślna konwencja routingu ASP.NET MVC to traktowanie segmentu adresu URL po nazwie metody akcji jako parametru o nazwie "ID". Jeśli metoda akcji ma parametr o nazwie ID, ASP.NET MVC automatycznie przekaże segment adresu URL jako parametr.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Uruchom aplikację i przejdź do/Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Podsumowaniemy do tej pory:

- Utworzyliśmy nowy projekt ASP.NET MVC w programie Visual Web Developer
- Omawiamy podstawową strukturę folderów aplikacji ASP.NET MVC
- Dowiesz się, jak uruchomić naszą witrynę sieci Web przy użyciu serwera ASP.NET Development
- Utworzyliśmy dwie klasy kontrolera: HomeController i StoreController
- Dodaliśmy metody akcji do naszych kontrolerów, które odpowiadają na żądania adresów URL i zwracają tekst do przeglądarki

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-1.md)
> [dalej](mvc-music-store-part-3.md)

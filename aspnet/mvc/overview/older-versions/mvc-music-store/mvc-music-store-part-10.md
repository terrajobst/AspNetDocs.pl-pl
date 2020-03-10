---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Część 10: ostateczne aktualizacje nawigacji i projektu witryny, wniosek | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 10 obejmuje ostateczne aktualizacje nawigacji i...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539369"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Część 10. Końcowe aktualizacje nawigacji i projektu witryny, podsumowanie

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 10 obejmuje ostateczne aktualizacje nawigacji i projektu witryny, wnioski.

Wszystkie najważniejsze funkcje naszej witryny zostały wykonane, ale nadal mamy kilka funkcji do dodania do nawigacji witryny, strony głównej i strony przeglądania sklepu.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Tworzenie widoku częściowego podsumowania koszyka zakupów

Chcemy uwidocznić liczbę elementów w koszyku użytkownika w całej lokacji.

![](mvc-music-store-part-10/_static/image1.png)

Można to łatwo zaimplementować, tworząc widok częściowy, który jest dodawany do naszej witryny. Master.

Jak pokazano wcześniej, kontroler ShoppingCart zawiera metodę akcji CartSummary, która zwraca widok częściowy:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Aby utworzyć widok częściowy CartSummary, kliknij prawym przyciskiem myszy folder widoki/ShoppingCart i wybierz polecenie Dodaj widok. Nazwij widok CartSummary i zaznacz pole wyboru "Utwórz widok częściowy", jak pokazano poniżej.

![](mvc-music-store-part-10/_static/image2.png)

Widok częściowy CartSummary jest naprawdę prosty — jest to tylko link do widoku indeksu ShoppingCart, który pokazuje liczbę elementów w koszyku. Pełny kod dla CartSummary. cshtml jest następujący:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

W każdej stronie w witrynie można uwzględnić widok częściowy, w tym wzorzec witryny, przy użyciu metody html. RenderAction. RenderAction wymaga, aby określić nazwę akcji ("CartSummary") i nazwę kontrolera ("ShoppingCart") w następujący sposób.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Przed dodaniem tego do układu lokacji utworzymy również menu gatunek, dzięki czemu będziemy mogli jednocześnie wprowadzić wszystkie nasze witryny. Master.

## <a name="creating-the-genre-menu-partial-view"></a>Tworzenie widoku częściowego menu gatunku

Możemy ułatwić naszym użytkownikom nawigowanie w sklepie przez dodanie menu gatunek, które zawiera listę wszystkich gatunków dostępnych w naszym sklepie.

![](mvc-music-store-part-10/_static/image3.png)

Należy wykonać te same kroki, aby utworzyć widok częściowy GenreMenu, a następnie dodać go zarówno do wzorca lokacji. Najpierw Dodaj następującą akcję kontrolera GenreMenu do StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Ta akcja zwraca listę gatunków, które będą wyświetlane przez widok częściowy, który zostanie utworzony dalej.

*Uwaga: dodaliśmy atrybut [ChildActionOnly] do tej akcji kontrolera, co oznacza, że ta akcja ma być używana tylko z widoku częściowego. Ten atrybut uniemożliwi wykonanie akcji kontrolera przez przechodzenie do/Store/GenreMenu. Nie jest to wymagane w przypadku widoków częściowych, ale jest to dobre rozwiązanie, ponieważ chcemy upewnić się, że nasze działania kontrolera są używane zgodnie z oczekiwaniami. Zwracamy również PartialView, a nie widok, dzięki czemu aparat widoku wie, że nie powinien korzystać z układu dla tego widoku, ponieważ jest uwzględniany w innych widokach.*

Kliknij prawym przyciskiem myszy akcję kontrolera GenreMenu i Utwórz widok częściowy o nazwie GenreMenu, który jednoznacznie wpisano przy użyciu klasy danych widoku gatunku, jak pokazano poniżej.

![](mvc-music-store-part-10/_static/image4.png)

Zaktualizuj kod widoku częściowego widoku GenreMenu, aby wyświetlić elementy przy użyciu listy nieuporządkowanej w następujący sposób.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aktualizowanie układu witryny w celu wyświetlenia naszych częściowych widoków

Możemy dodać nasze częściowe widoki do układu lokacji (/Views/Shared/\_Layout. cshtml), wywołując plik HTML. RenderAction (). Dodamy je zarówno w, jak i kilka dodatkowych znaczników, aby je wyświetlić, jak pokazano poniżej:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Teraz po uruchomieniu aplikacji zobaczymy gatunek w lewym obszarze nawigacji i podsumowanie koszyka u góry.

## <a name="update-to-the-store-browse-page"></a>Aktualizacja na stronie przeglądania sklepu

Strona przeglądania sklepu jest funkcjonalna, ale nie jest bardzo dobra. Możemy zaktualizować stronę, aby wyświetlić albumy w lepszym układzie przez zaktualizowanie kodu widoku (znalezionego w/Views/Store/Browse.cshtml) w następujący sposób:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Tutaj korzystamy z adresu URL. akcja, a nie HTML. ActionLink, aby można było zastosować formatowanie specjalne do linku zawierającego kompozycję albumu.

*Uwaga: na potrzeby tych albumów jest wyświetlana ogólna okładka albumu. Te informacje są przechowywane w bazie danych programu i można je edytować za pomocą Menedżera sklepu. Witamy w dodawaniu własnej kompozycji.*

Teraz po przejściu do gatunku zobaczysz albumy wyświetlane w siatce z kompozycją albumu.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualizowanie strony głównej w celu wyświetlenia najważniejszych albumów do sprzedaży

Chcemy skorzystać z najpopularniejszych albumów sprzedaży na stronie głównej, aby zwiększyć sprzedaż. Wprowadzimy pewne aktualizacje naszych HomeController, aby obsłużyć to i dodać również do kilku dodatkowych grafik.

Najpierw dodamy do naszej klasy albumu właściwość nawigacji, dzięki czemu EntityFramework wie, że są one skojarzone. Ostatnie kilka wierszy naszej klasy **albumu** powinno teraz wyglądać następująco:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Uwaga: wymaga to dodania instrukcji using w celu przyłączenia przestrzeni nazw System. Collections. Generic.*

Najpierw dodamy pole storeDB oraz instrukcje MvcMusicStore. models using, jak w przypadku innych kontrolerów. Następnie dodamy następującą metodę do HomeController, która wysyła zapytanie do naszej bazy danych w celu znalezienia najważniejszych albumów sprzedaży zgodnie z OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Jest to Metoda prywatna, ponieważ nie chcemy, aby była ona dostępna jako akcja kontrolera. Firma Microsoft umieszcza ją w HomeController dla uproszczenia, ale zachęcamy do przenoszenia logiki biznesowej do oddzielnych klas usługi.

W tym miejscu możemy zaktualizować akcję kontrolera indeksu, aby wykonać zapytanie o 5 najważniejszych albumów do sprzedaży i zwrócić je do widoku.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Pełny kod dla zaktualizowanego HomeController jest przedstawiony poniżej.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Na koniec należy zaktualizować nasz widok indeksu macierzystego, aby można było wyświetlić listę albumów, aktualizując Typ modelu i dodając listę albumów na dole. Będziemy mogli również dodać do strony nagłówek i sekcję promocyjną.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Teraz po uruchomieniu aplikacji zobaczymy naszą zaktualizowaną stronę główną z najpopularniejszymi albumami do sprzedaży i naszym komunikatem promocyjnym.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Podsumowanie

Wiemy, że ASP.NET MVC ułatwia tworzenie zaawansowanej witryny sieci Web z dostępem do baz danych, członkostwem, AJAX itp. szybko. Miejmy nadzieję ten samouczek zawiera narzędzia potrzebne do rozpoczęcia tworzenia własnych aplikacji ASP.NET MVC.

> [!div class="step-by-step"]
> [Wstecz](mvc-music-store-part-9.md)

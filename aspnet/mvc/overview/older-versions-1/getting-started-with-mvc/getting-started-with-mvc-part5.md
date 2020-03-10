---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Uzyskiwanie dostępu do danych modelu z kontrolera | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543751"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Uzyskiwanie dostępu do danych modelu za pomocą kontrolera

przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

W tej sekcji zamierzamy utworzyć nową klasę MoviesController i napisać kod, który pobiera nasze dane filmu i wyświetla go z powrotem do przeglądarki przy użyciu szablonu widoku.

Kliknij prawym przyciskiem myszy folder controllers i Utwórz nowy MoviesController.

[![dodać kontrolera](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Spowoduje to utworzenie nowego pliku "MoviesController.cs" pod naszym folderem \Controllers w naszym projekcie. Zaktualizujmy MovieController, aby pobrać listę filmów z ostatnio wypełnionej bazy danych.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Wykonujemy zapytanie LINQ, dzięki czemu pobieramy tylko filmy wydane po dniu 1984. Potrzebujemy szablonu widoku, aby renderować tę listę filmów z powrotem, więc kliknij prawym przyciskiem myszy metodę i wybierz polecenie Dodaj widok, aby je utworzyć.

W oknie dialogowym Dodawanie widoku zostanie wyświetlona lista&lt;filmów. models. Movie&gt; do naszego szablonu widoku. W przeciwieństwie do poprzednich przypadków, w których użyto okna dialogowego Dodawanie widoku i wybrano opcję utworzenia szablonu "pusty", ten czas wskazuje, że program Visual Studio ma automatycznie "szkielet" jako szablon widoku dla nas z niestandardową zawartością. W tym celu należy wybrać element "list" w menu rozwijanym "Wyświetl zawartość".

Należy pamiętać, że w przypadku utworzenia nowej klasy należy skompilować aplikację do wyświetlania w oknie dialogowym Dodawanie widoku.

![Dodaj widok](getting-started-with-mvc-part5/_static/image3.png)

Kliknij przycisk Dodaj, a system automatycznie wygeneruje kod dla tego widoku, który wyświetla naszą listę filmów. Jest to dobry moment, aby zmienić nagłówek &lt;H2&gt; na taki, jak "Moja lista filmów", tak jak wcześniej, za pomocą widoku Hello world.

[![filmów — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Uruchom aplikację i odwiedź stronę/Movies na pasku adresu. Teraz pobrano dane z bazy danych przy użyciu podstawowego zapytania wewnątrz kontrolera i zwróciło dane do widoku, który wie o filmach. Ten widok umożliwia przechodzenie przez listę filmów i utworzenie tabeli danych dla nas.

[Lista filmów ![— Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Nie będziemy wdrażać funkcji Edytuj, szczegóły i Usuń w tej aplikacji — dlatego nie są potrzebne domyślne linki utworzone dla nas przez szablon szkieletu. Otwórz plik/Movies/Index.aspx i usuń go.

Oto kod źródłowy, dla którego nasz zaktualizowany szablon widoku powinien wyglądać jak po wprowadzeniu tych zmian:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Tworzy linki, które nie są potrzebne, więc usuniemy je z tego przykładu. Nasz nowy link zostanie zachowany w tym przypadku. Oto, jak nasza aplikacja zostanie usunięta z tą kolumną.

[Lista filmów ![— Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Mamy już prostą listę naszych danych filmowych. Jeśli jednak klikniemy link "Utwórz nowy", zostanie wyświetlony komunikat o błędzie, ponieważ nie jest on podłączany. Zaimplementujmy metodę tworzenia akcji i umożliwisz użytkownikowi wprowadzanie nowych filmów w naszej bazie danych.

> [!div class="step-by-step"]
> [Poprzednie](getting-started-with-mvc-part4.md)
> [dalej](getting-started-with-mvc-part6.md)

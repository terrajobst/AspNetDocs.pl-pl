---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Dodawanie kolumny do modelu | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543583"
---
# <a name="adding-a-column-to-the-model"></a>Dodawanie kolumny do modelu

przez [Scott Hanselman](https://github.com/shanselman)

> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

W tej sekcji omówiono, jak możemy wprowadzić zmiany w schemacie bazy danych i obsłużyć zmiany w naszej aplikacji.

Dodajmy kolumnę "Rating" do tabeli filmów. Wróć do środowiska IDE i kliknij Eksplorator bazy danych. Kliknij prawym przyciskiem myszy tabelę filmów i wybierz polecenie Otwórz definicję tabeli.

Dodaj kolumnę "Rating", jak pokazano poniżej. Ponieważ nie mamy teraz żadnych ocen, kolumna może zezwalać na wartości null. Kliknij polecenie Zapisz.

[![edytowania filmów](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Następnie wróć do Eksplorator rozwiązań i Otwórz plik filmy. edmx (znajdujący się w folderze \Models). Kliknij prawym przyciskiem myszy powierzchnię projektu (biały obszar) i wybierz pozycję Aktualizuj model z bazy danych.

[![filmów — Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Spowoduje to uruchomienie "kreatora aktualizacji". Kliknij znajdującą się na niej kartę Odśwież i kliknij przycisk Zakończ. Nasze klasy modelu filmu zostaną następnie zaktualizowane przy użyciu nowej kolumny.

![Kreator aktualizacji (2)](getting-started-with-mvc-part8/_static/image5.png)

Po kliknięciu przycisku Zakończ zobaczysz, że nowa kolumna ocen została dodana do jednostki filmu w naszym modelu.

[![jednostka filmu](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Dodaliśmy kolumnę w modelu bazy danych, ale widoki nie są dla niego znane.

## <a name="update-views-with-model-changes"></a>Aktualizowanie widoków ze zmianami modelu

Istnieje kilka sposobów aktualizowania naszych szablonów widoku w celu odzwierciedlenia nowej kolumny klasyfikacji. Ponieważ Utworzyliśmy te widoki przez wygenerowanie ich za pomocą okna dialogowego Dodaj widok, możemy je usunąć i ponownie utworzyć. Jednak zazwyczaj ludzie już wprowadziły modyfikacje w swoich szablonach widoków od początkowej generacji szkieletowej i chcą ręcznie dodawać lub usuwać pola, podobnie jak w przypadku pola ID dla tworzenia.

Otwórz szablon \Views\Movies\Index.aspx i Dodaj &lt;ową ocenę&gt;&lt;/th&gt; do nagłówka tabeli filmów. Dodano mnie po gatunku. Następnie w tej samej pozycji kolumny, ale mniej więcej, Dodaj wiersz do danych wyjściowych naszej nowej klasyfikacji.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Nasz końcowy szablon index. aspx będzie wyglądać następująco:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Następnie Otwórzmy szablon \Views\Movies\Create.aspx i dodasz etykietę i pole tekstowe nowej właściwości rankingu:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Nasz końcowy szablon Create. aspx będzie wyglądać następująco i zmienimy tytuł przeglądarki i pomocniczą &lt;H2&gt; tytułu w taki sam sposób jak "Utwórz film" w tym miejscu.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Uruchom aplikację, a teraz masz nowe pole w bazie danych, które zostało dodane do strony Tworzenie. Dodaj nowy film — tym razem z klasyfikacją, a następnie kliknij przycisk Utwórz.

[![utworzyć filmu — Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Po kliknięciu przycisku Utwórz zostanie wysłana do strony indeks, na której jest wyświetlany nowy film z nową kolumną oceny w bazie danych.

[Lista filmów ![— Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

W tym podstawowym samouczku przedstawiono tworzenie kontrolerów, kojarzenie ich z widokami i przekazywaniem danych z zakodowanymi danymi. Następnie utworzyliśmy i zaprojektowano bazę danych oraz umieszczamy w niej dane. Pobrano dane z bazy danych i są one wyświetlane w tabeli HTML. Następnie dodaliśmy formularz Create, który umożliwia użytkownikowi dodawanie danych do bazy danych z poziomu aplikacji sieci Web. Dodaliśmy sprawdzanie poprawności, a następnie Walidacja używa języka JavaScript po stronie klienta. Na koniec baza danych została zmieniona w taki sposób, aby zawierała nową kolumnę danych, a następnie Zaktualizowano te dwie strony, aby utworzyć i wyświetlić te nowe dane.

Teraz zachęcamy do przechodzenia do naszego samouczka pośredniego "[sklepu z muzyką MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" oraz wielu filmów wideo i zasobów w [https://asp.net/mvc](https://asp.net/mvc) , aby dowiedzieć się więcej o ASP.NET MVC!

Owocnej pracy.

- Scott Hanselman — [http://hanselman.com](http://hanselman.com) i [@shanselman](http://twitter.com/shanselman) w serwisie Twitter.

> [!div class="step-by-step"]
> [Wstecz](getting-started-with-mvc-part7.md)

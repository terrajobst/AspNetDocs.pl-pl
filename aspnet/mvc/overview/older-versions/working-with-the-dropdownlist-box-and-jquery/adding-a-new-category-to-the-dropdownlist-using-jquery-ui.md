---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Dodawanie nowej kategorii do DropDownList przy użyciu interfejsu użytkownika jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 3207079ee468232e5f75b081421241c232936baf
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455727"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Dodawanie nowej kategorii do metody DropDownList przy użyciu zestawu jQuery UI

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

Tag `Select` HTML jest idealnym rozwiązaniem do prezentowania listy stałych danych kategorii, ale często trzeba dodać nową kategorię. Załóżmy, że chcemy dodać gatunek "Opera" do kategorii w naszej bazie danych? W tej sekcji użyjesz interfejsu użytkownika jQuery do dodania okna dialogowego, którego można użyć do dodania nowej kategorii. Na poniższej ilustracji przedstawiono sposób wyświetlania interfejsu użytkownika w przeglądarce.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Gdy użytkownik wybierze link **Dodaj nowy gatunek** , wyskakujące okno dialogowe wyświetli użytkownikowi komunikat o nowej nazwie gatunku (i opcjonalnie opis). Na poniższej ilustracji przedstawiono okno dialogowe **Dodawanie gatunku** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Po wprowadzeniu nowej nazwy gatunku, gdy przycisk **Zapisz** jest wypychany, następuje:

1. Wywołanie AJAX zapisuje dane do metody Create kontrolera gatunku, która zapisuje nowy gatunek do bazy danych i zwraca nowe informacje o gatunku (nazwę i identyfikator gatunku) jako kod JSON.
2. Język JavaScript dodaje nowe dane gatunku do listy wyboru.
3. Język JavaScript sprawia, że nowy gatunek wybranego elementu.

   Na poniższym obrazie program **Opera** został dodany do bazy danych i wybrany na liście rozwijanej **gatunek** . 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Otwórz plik *Views\StoreManager\Create.cshtml* i Zastąp znacznik gatunek następującym kodem:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre` widok częściowy zawiera całą logikę do podłączania kodu JavaScript i jQuery używanego do implementowania funkcji Dodaj nowy gatunek. Po zakończeniu wykonywania kodu będzie on prosty do wykonania tego samego przy użyciu interfejsu użytkownika wykonawcy.

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder *Views\StoreManager* , a następnie wybierz polecenie **Dodaj**, a następnie **Wyświetl**. W polu wprowadzanie **nazwy widoku** wprowadź `_ChooseGenre` a następnie wybierz pozycję **Dodaj**. Zastąp znaczniki w pliku *Views\StoreManager\\_ChooseGenre. cshtml* :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Pierwszy wiersz deklaruje, że przekazujemy `Album` jako nasz model, dokładnie tę samą instrukcję modelową, która znajduje się w widoku Create (Tworzenie). Kilka następnych wierszy jest znacznikiem pomocnika **etykiet** . Następnym wierszem jest wywołanie pomocnika **DropDownList** , dokładnie takie samo jak w przypadku oryginalnego widoku tworzenia. Następny wiersz dodaje łącze o nazwie `Add New Genre`i style, takie jak przycisk. Wiersz zawierający `ValidationMessageFor` jest kopiowany bezpośrednio z widoku tworzenie. Następujące wiersze:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

tworzy ukryty blok DIV o IDENTYFIKATORze `genreDialog`. Będziemy używać platformy jQuery do przełączania okna dialogowego **Dodawanie gatunku** z identyfikatorem `genreDialog` w tym elemencie DIV. Ostatnie dwa Tagi skryptu zawierają linki do plików języka JavaScript, które zostaną użyte do zaimplementowania funkcji Dodaj nowy gatunek. Plik */scripts/chooseGenre.js* jest dostępny dla Ciebie w projekcie. przeanalizuje go później w samouczku.

Uruchom aplikację i kliknij przycisk **Dodaj nowy gatunek** . W oknie dialogowym **Dodaj gatunek** wprowadź wartość **Opera** w polu **Nazwa** wejściowa.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Kliknij przycisk **Zapisz**. Wywołanie AJAX tworzy kategorię Opera, a następnie wypełnia listę rozwijaną za pomocą programu Opera i ustawia program Opera jako wybrany gatunek.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Wprowadź wykonawcę, tytuł i cenę, a następnie wybierz przycisk **Utwórz** . Jeśli wprowadzisz cenę niższą niż $8,99, nowy album pojawi się u góry widoku indeksu. Sprawdź, czy nowy wpis albumu został zapisany w bazie danych.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Spróbuj utworzyć nowy gatunek z tylko jedną literą. Poniższy kod w pliku *Models\Genre.cs* ustawia minimalną i maksymalną długość nazwy gatunku.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Raporty weryfikacji po stronie klienta należy wprowadzić ciąg z zakresu od 2 do 20 znaków.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Sprawdzanie, w jaki sposób nowy gatunek jest dodawany do bazy danych i listy wyboru.

Otwórz plik *Scripts\chooseGenre.js* i zapoznaj się z kodem.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Drugi wiersz używa identyfikatora `genreDialog` do tworzenia okna dialogowego znacznika DIV w pliku *Views\StoreManager\\_ChooseGenre. cshtml* . Większość nazwanych parametrów nie ma wyjaśnień. Parametr `autoOpen` ma wartość FAŁSZ, a wybranie przycisku **Utwórz gatunek** spowoduje otwarcie okna dialogowego w sposób jawny (jest to opisane w drugim dniu). Okno dialogowe ma dwa przyciski, **Zapisz** i **Anuluj**. Przycisk **Anuluj** zamyka okno dialogowe. Poniższy kod przedstawia funkcję **Save** Button.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Wybrano `var createGenreForm` z identyfikatora `createGenreForm`. Identyfikator `createGenreForm` został ustawiony w następującym kodzie znalezionym w pliku *Views\Genre\\_CreateGenre. cshtml* .

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

Przeciążenie pomocnika [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) użyte w pliku *Views\Genre\\_CreateGenre. cshtml* generuje HTML z atrybutem Action zawierającym adres URL służący do przesyłania formularza. Możesz to zobaczyć, wyświetlając stronę tworzenie albumu w przeglądarce i wybierając pozycję Pokaż źródło w przeglądarce. Poniższy znacznik pokazuje wygenerowany kod HTML zawierający tag formularza.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

Linia `$.post` jQuery wykonuje wywołanie AJAX do atrybutu Action (`/StoreManager/Create`) i przekazuje dane z okna dialogowego **Tworzenie gatunku** . Dane składają się z nazwy nowego gatunku i opcjonalnego opisu. Jeśli wywołanie AJAX zakończy się pomyślnie, Nowa nazwa i wartość gatunku są dodawane do Select Markup, a nowy gatunek jest ustawiony na wybraną wartość. Ponieważ jest to dynamicznie generowana adiustacja, nie można wyświetlić nowej opcji Select, wyświetlając źródło w przeglądarce. Nowy kod HTML można zobaczyć przy użyciu narzędzi deweloperskich programu IE 9 F12. Aby wyświetlić nową opcję wyboru, w programie Internet Explorer 9 Naciśnij klawisz F12, aby uruchomić narzędzia deweloperskie F12. Przejdź do strony Tworzenie i Dodaj nowy gatunek, aby nowy gatunek został wybrany na liście wyboru gatunku. W narzędziach deweloperskich F12:

1. Wybierz kartę HTML.
2. Naciśnij ikonę odświeżania.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. W polu wyszukiwania wprowadź GenreID.
4. Przy użyciu następnej ikony   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Przejdź do następującego tagu SELECT:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Rozwiń ostatnią wartość opcji.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Poniższy kod w pliku *Scripts\chooseGenre.js* pokazuje, jak przycisk **Dodaj nowy gatunek** jest połączony z zdarzeniem kliknięcia i jak zostanie utworzone okno dialogowe **Dodaj nowy gatunek** .

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

Pierwszy wiersz tworzy funkcję kliknięcia dołączoną do przycisku **Dodaj nowy gatunek** . Następujące znaczniki z pliku Views\StoreManager\\_ChooseGenre. cshtml pokazują, jak zostanie utworzony przycisk **Dodaj nowy gatunek** :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Metoda Load tworzy i otwiera okno dialogowe Dodawanie gatunku i wywołuje metodę jQuery `parse`, dzięki czemu sprawdzanie poprawności klienta odbywa się na danych wprowadzonych w oknie dialogowym.

W tej sekcji przedstawiono sposób tworzenia okna dialogowego, które może służyć do dodawania nowych danych kategorii do listy wyboru. Za pomocą tej samej procedury można utworzyć interfejs użytkownika w celu dodania nowego wykonawcy do listy wyboru wykonawcy. W tym samouczku przedstawiono przegląd pracy z pomocnikiem HTML ASP.NET MVC **DropDownList**. Aby uzyskać dodatkowe informacje na temat pracy z **DropDownList**, zobacz sekcję dotyczącą dodawania odwołań poniżej. Poinformuj nas o tym, czy ten samouczek był pomocny.

Rick. Anderson [at] Microsoft. com

### <a name="additional-references"></a>Dodatkowe informacje

- [ASP.NET MVC — kaskadowe listy rozwijane samouczków](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) według [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Wybrane](https://harvesthq.github.com/chosen/) Wtyczka języka JavaScript, która obsługuje wiele zaznaczeń i filtrowanie.

### <a name="contributors"></a>Współautorzy

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Recenzenci

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Jan Pope
- Dykstra Tomasz

> [!div class="step-by-step"]
> [Wstecz](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)

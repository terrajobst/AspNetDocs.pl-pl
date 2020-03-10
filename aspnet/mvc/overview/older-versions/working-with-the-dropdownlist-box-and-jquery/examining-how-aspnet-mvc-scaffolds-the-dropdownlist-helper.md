---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Badanie sposobu, w jaki ASP.NET MVC szkieletuje pomocnika DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614906"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Badanie sposobu tworzenia szkieletu pomocnika DropDownList przez wzorzec ASP.NET MVC

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *controllers* , a następnie wybierz polecenie **Dodaj kontroler**. Nadaj nazwę kontrolerowi **StoreManagerController**. Ustaw opcje okna dialogowego **Dodaj kontroler** , jak pokazano na poniższej ilustracji.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Edytuj widok *StoreManager\Index.cshtml* i Usuń `AlbumArtUrl`. Usunięcie `AlbumArtUrl` sprawia, że prezentacja będzie bardziej czytelna. Poniżej przedstawiono kompletny kod.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Otwórz plik *Controllers\StoreManagerController.cs* i znajdź metodę `Index`. Dodaj klauzulę `OrderBy`, aby albumy były sortowane według cen. Pełny kod jest przedstawiony poniżej.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Sortowanie według cen ułatwia testowanie zmian w bazie danych. Podczas testowania metod edycji i tworzenia można użyć niskich cen, aby zapisane dane były wyświetlane jako pierwsze.

Otwórz plik *StoreManager\Edit.cshtml* . Dodaj następujący wiersz tuż po tagu legendy.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

Poniższy kod przedstawia kontekst tej zmiany:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` jest wymagana do wprowadzania zmian w rekordzie albumu.

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację. Wybierz łącze **administratora** , a następnie wybierz link **Utwórz nowy** , aby utworzyć nowy album. Sprawdź, czy Zapisano informacje o albumie. Edytuj album i sprawdź, czy wprowadzone zmiany są utrwalane.

### <a name="the-album-schema"></a>Schemat albumu

Kontroler `StoreManager` utworzony przez mechanizm szkieletu MVC umożliwia CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie) dostępu do albumów w bazie danych magazynu utworów muzycznych. Poniżej przedstawiono schemat informacji o albumie:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

W tabeli `Albums` nie jest przechowywany gatunek i opis albumu, który przechowuje klucz obcy w tabeli `Genres`. Tabela `Genres` zawiera nazwę i Opis gatunku. Podobnie tabela `Albums` nie zawiera nazwy wykonawców albumów, ale klucz obcy do tabeli `Artists`. Tabela `Artists` zawiera nazwę wykonawcy. W przypadku badania danych w tabeli `Albums` można zobaczyć, że każdy wiersz zawiera klucz obcy do tabeli `Genres` i klucz obcy do tabeli `Artists`. Na poniższej ilustracji przedstawiono niektóre dane tabeli z tabeli `Albums`.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>Tag SELECT HTML

Element `<select>` HTML (utworzony przez pomocnika HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) ) służy do wyświetlania kompletnej listy wartości (takich jak lista gatunków). W przypadku formularzy edycji, gdy bieżąca wartość jest znana, na liście wyboru można wyświetlić bieżącą wartość. Te wartości zostały wcześniej wykorzystane, gdy ustawimy wybraną wartość na **komedia**. Lista wyboru jest idealnym rozwiązaniem do wyświetlania danych kategorii lub kluczy obcych. Element `<select>` dla gatunku klucz obcy zawiera listę możliwych nazw gatunku, ale podczas zapisywania formularza Właściwość gatunek jest aktualizowana o wartości klucza obcego gatunku, a nie nazwy wyświetlanego gatunku. Na poniższym obrazie wybrany gatunek to **Disco** , a wykonawca to **Donną lato**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Badanie kodu szkieletowego ASP.NET MVC

Otwórz plik *Controllers\StoreManagerController.cs* i znajdź metodę `HTTP GET Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

Metoda `Create` dodaje dwa obiekty [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) do `ViewBag`, jeden do zawierają informacje o gatunku i jeden do zawierają informacje o wykonawcy. Użycie przeciążenia konstruktora [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) powyżej przyjmuje trzy argumenty:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *elementy*: element [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) zawierający elementy z listy. W powyższym przykładzie lista gatunków zwracana przez `db.Genres`.
2. *dataValueField*: Nazwa właściwości na liście **IEnumerable** , która zawiera wartość klucza. W powyższym przykładzie `GenreId` i `ArtistId`.
3. *dataTextField*: Nazwa właściwości na liście **IEnumerable** , która zawiera informacje do wyświetlenia. W tabeli artyści i gatunek jest używane pole `name`.

Otwórz plik *Views\StoreManager\Create.cshtml* i przejrzyj znacznik pomocnika `Html.DropDownList` dla pola gatunek.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

Pierwszy wiersz pokazuje, że widok tworzenia ma `Album` model. W podanej powyżej metodzie `Create` żaden model nie został przesłany, więc widok pobiera model `Album` o **wartości null** . W tym momencie tworzymy nowy album, dlatego nie mamy żadnych danych `Album`.

Przeciążanie [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) pokazane powyżej Pobiera nazwę pola do powiązania z modelem. Używa ona również tej nazwy do wyszukania obiektu **ViewBag** zawierającego obiekt [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) . Przy użyciu tego przeciążenia należy nazwać obiekt **ViewBag SelectList** `GenreId`. Drugi parametr (`String.Empty`) jest tekstem, który ma być wyświetlany, gdy nie wybrano żadnego elementu. Jest to dokładnie to, co chcemy zrobić podczas tworzenia nowego albumu. Jeśli usunięto drugi parametr i użyto następującego kodu:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

Lista wyboru będzie domyślnie do pierwszego elementu lub skały w naszym przykładzie.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Badanie metody `HTTP POST Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

To Przeciążenie metody `Create` przyjmuje obiekt `album`, utworzony przez system powiązania modelu MVC ASP.NET z wartości formularza ogłoszone. Gdy przesyłasz nowy album, jeśli stan modelu jest prawidłowy i nie ma żadnych błędów bazy danych, nowy album zostanie dodany do bazy danych. Na poniższej ilustracji przedstawiono tworzenie nowego albumu.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Można użyć [Narzędzia programu Fiddler](http://www.fiddler2.com/fiddler2/) do sprawdzenia wartości ogłoszonych formularzy, które są używane przez powiązanie modelu MVC ASP.NET do tworzenia obiektu albumu.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refaktoryzacja tworzenia SelectList ViewBag

Metody `Edit` i `HTTP POST Create` mają identyczny kod w celu skonfigurowania **SelectList** w **ViewBag**. W duchu [suchego](http://en.wikipedia.org/wiki/Don't_repeat_yourself)kod będzie refaktoryzacji. Użyjemy tego kodu refaktoryzacji później.

Utwórz nową metodę, aby dodać gatunek i **SelectList** wykonawcy do **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Zastąp dwa wiersze ustawiające `ViewBag` w każdej `Create` i `Edit` metody z wywołaniem metody `SetGenreArtistViewBag`. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Utwórz nowy album i edytuj album, aby sprawdzić, czy zmiany zostały wykonane.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Jawne przekazanie SelectList do DropDownList

Widoki tworzenie i edytowanie utworzone przez szkielet ASP.NET MVC używają następujących przeciążeń **DropDownList** :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

Poniżej pokazano `DropDownList` znaczników dla widoku tworzenia.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Ponieważ właściwość `ViewBag` dla `SelectList` ma nazwę `GenreId`, pomocnik **DropDownList** będzie używać `GenreId`**SelectList** w **ViewBag**. W poniższym przeciążeniu **DropDownList** `SelectList` jest jawnie przekazywać.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Otwórz plik *Views\StoreManager\Edit.cshtml* i Zmień wywołanie **DropDownList** , aby jawnie przekazać **SelectList**, używając powyżej przeciążenia. Zrób to dla kategorii gatunek. Ukończony kod jest przedstawiony poniżej:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Uruchom aplikację i kliknij link **administratora** , a następnie przejdź do albumu Jazz i wybierz łącze **Edytuj** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Zamiast wyświetlania Jazz jako aktualnie wybranego gatunku, zostanie wyświetlona skała. Gdy argument ciągu (właściwość do powiązania) i obiekt **SelectList** mają taką samą nazwę, wybrana wartość nie jest używana. Gdy nie zostanie podana wybrana wartość, przeglądarki domyślnie są pierwszym elementem w **SelectList**(który jest w powyższym przykładzie **skalą** ). Jest to znane ograniczenie pomocnika **DropDownList** .

Otwórz plik *Controllers\StoreManagerController.cs* i Zmień nazwy obiektów **SelectList** na `Genres` i `Artists`. Ukończony kod jest przedstawiony poniżej:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Nazwy gatunków i artystów są lepszymi nazwami dla kategorii, ponieważ zawierają więcej niż tylko identyfikator każdej kategorii. Refaktoryzacja była wcześniej płacona. Zamiast zmieniać **ViewBag** w czterech metodach, zmiany zostały odizolowane od metody `SetGenreArtistViewBag`.

Zmień wywołanie **DropDownList** w widokach tworzenie i edytowanie, aby użyć nowych nazw **SelectList** . Poniżej przedstawiono nowe znaczniki widoku edycji:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

Widok tworzenia wymaga pustego ciągu, aby zapobiec wyświetlaniu pierwszego elementu w SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Utwórz nowy album i edytuj album, aby sprawdzić, czy zmiany zostały wykonane. Przetestuj edycję kodu, wybierając album o gatunku innym niż Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Używanie modelu widoku z pomocnikiem DropDownList

Utwórz nową klasę w folderze modele widoków o nazwie `AlbumSelectListViewModel`. Zastąp kod w klasie `AlbumSelectListViewModel` następującym:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

Konstruktor `AlbumSelectListViewModel` pobiera album, listę artystów i gatunek oraz tworzy obiekt zawierający album i `SelectList` dla gatunku i artystów.

Skompiluj projekt, aby `AlbumSelectListViewModel` był dostępny podczas tworzenia widoku w następnym kroku.

Dodaj do `StoreManagerController`metodę `EditVM`. Poniżej przedstawiono kompletny kod.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Kliknij prawym przyciskiem myszy `AlbumSelectListViewModel`, wybierz opcję **Rozwiąż**, a następnie **za pomocą MvcMusicStore. modele widoków;** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Alternatywnie można dodać następującą instrukcję using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Kliknij prawym przyciskiem myszy `EditVM` i wybierz polecenie **Dodaj widok**. Użyj opcji przedstawionych poniżej.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Wybierz pozycję **Dodaj**, a następnie zastąp zawartość pliku *Views\StoreManager\EditVM.cshtml* następującym:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

Znaczniki `EditVM` są bardzo podobne do oryginalnego znacznika `Edit` z następującymi wyjątkami.

- Właściwości modelu w widoku `Edit` są `model.property`(na przykład `model.Title`). Właściwości modelu w widoku `EditVm` są `model.Album.property`(na przykład `model.Album.Title`). Dzieje się tak, ponieważ widok `EditVM` jest przekazywać kontener dla `Album`, a nie `Album` jak w widoku `Edit`.
- Drugi parametr **DropDownList** pochodzi z modelu widoku, a nie **ViewBag**.
- Pomocnik **BeginForm** w `EditVM` widoku jawnie zapisuje z powrotem do `Edit` metody akcji. Przez zaksięgowanie z powrotem do akcji `Edit` nie musimy pisać akcji `HTTP POST EditVM` i mogą ponownie wykorzystać akcję `HTTP POST` `Edit`.

Uruchom aplikację i edytuj album. Zmień adres URL, aby użyć `EditVM`. Zmień pole i naciśnij przycisk **Zapisz** , aby sprawdzić, czy kod działa.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Której metody należy użyć?

Wszystkie trzy wymienione metody są akceptowalne. Wielu deweloperów preferuje jawne przekazywanie `SelectList` do `DropDownList` przy użyciu `ViewBag`. To podejście ma dodatkową zaletę, która zapewnia elastyczność używania bardziej odpowiedniej nazwy dla kolekcji. Jedynym zastrzeżeń nie jest nazwa obiektu `ViewBag SelectList` o tej samej nazwie co właściwość modelu.

Niektórzy deweloperzy preferują podejście ViewModel. Inne zapoznają się z bardziej szczegółowym znacznikiem i wygenerowanym kodem HTML ViewModel.

W tej sekcji poznasz trzy podejścia do używania **DropDownList** z danymi kategorii. W następnej sekcji pokażemy, jak dodać nową kategorię.

> [!div class="step-by-step"]
> [Poprzednie](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [dalej](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)

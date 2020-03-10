---
uid: web-pages/overview/data/working-with-files
title: Praca z plikami w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale wyjaśniono, jak odczytywać, zapisywać, dołączać, usuwać i ładować pliki.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 684c47a8a8480dc040e5144144577c94c35d39e5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642367"
---
# <a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Praca z plikami w witrynie ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób odczytywania, zapisywania, dołączania, usuwania i przekazywania plików w witrynie ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Jeśli chcesz przekazać obrazy i manipulować nimi (na przykład przerzucić lub zmienić ich rozmiar), zobacz [Praca z obrazami w witrynie ASP.NET Web Pages](/aspnet/web-pages/overview/ui-layouts-and-themes/9-working-with-images).
> 
> 
> **Dowiesz się:** 
> 
> - Jak utworzyć plik tekstowy i zapisać w nim dane.
> - Jak dołączyć dane do istniejącego pliku.
> - Jak odczytać plik i wyświetlić go.
> - Jak usunąć pliki z witryny sieci Web.
> - Jak umożliwić użytkownikom przekazywanie jednego pliku lub wielu plików.
> 
> Są to funkcje programowania ASP.NET wprowadzone w artykule:
> 
> - Obiekt `File`, który zapewnia sposób zarządzania plikami.
> - Pomocnik `FileUpload`.
> - Obiekt `Path`, który dostarcza metody, które umożliwiają manipulowanie ścieżkami i nazwami plików.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - WebMatrix 2
>   
> 
> Ten samouczek współpracuje również z programem WebMatrix 3.

<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Tworzenie pliku tekstowego i zapisywanie w nim danych

Oprócz korzystania z bazy danych w witrynie sieci Web, możesz współpracować z plikami. Można na przykład używać plików tekstowych jako prostego sposobu przechowywania danych dla witryny. (Plik tekstowy, który jest używany do przechowywania danych, jest czasami nazywany *plikiem prostym*). Pliki tekstowe mogą znajdować się w różnych formatach, takich jak *txt*, *XML*lub *CSV* (wartości rozdzielane przecinkami).

Jeśli chcesz przechowywać dane w pliku tekstowym, możesz użyć metody `File.WriteAllText`, aby określić plik do utworzenia oraz dane, które mają być w nim zapisane. W tej procedurze utworzysz stronę zawierającą prosty formularz z trzema `input` elementami (imię, nazwisko i adres e-mail) i przycisk **Prześlij** . Po przesłaniu formularza użytkownik będzie przechowywał dane wejściowe użytkownika w pliku tekstowym.

1. Utwórz nowy folder o nazwie *App\_Data*, jeśli jeszcze nie istnieje.
2. W katalogu głównym witryny sieci Web Utwórz nowy plik o nazwie *UserData. cshtml*.
3. Zastąp istniejącą zawartość następującym: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Znacznik HTML tworzy formularz z trzema polami tekstowymi. W kodzie można użyć właściwości `IsPost`, aby określić, czy strona została przesłana przed rozpoczęciem przetwarzania.

    Pierwsze zadanie polega na uzyskaniu danych wejściowych użytkownika i przypisaniu go do zmiennych. Następnie kod łączy wartości oddzielnych zmiennych w jeden ciąg rozdzielany przecinkami, który jest następnie przechowywany w innej zmiennej. Należy zauważyć, że separator przecinka jest ciągiem zawartym w cudzysłowie (","), ponieważ jest to dosłownie osadzenie przecinka w ciągu, który tworzysz. Na końcu połączonych danych należy dodać `Environment.NewLine`. Spowoduje to dodanie podziału wiersza (znaku nowego wiersza). To, co tworzysz przy użyciu wszystkich takich złączek, jest ciągiem, który wygląda następująco:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Z niewidocznym podziałem wiersza na końcu).

    Następnie utworzysz zmienną (`dataFile`), która zawiera lokalizację i nazwę pliku, w którym będą przechowywane dane. Ustawienie lokalizacji wymaga szczególnej obsługi. W witrynach sieci Web jest to niewłaściwe rozwiązanie do odwoływania się w kodzie do ścieżek bezwzględnych, takich jak *C:\Folder\File.txt* dla plików na serwerze sieci Web. Jeśli witryna sieci Web jest przenoszona, ścieżka bezwzględna będzie niewłaściwa. Ponadto w przypadku witryny hostowanej (w przeciwieństwie do komputera) zazwyczaj nie wiadomo, co jest poprawna ścieżka podczas pisania kodu.

    Jednak czasami (podobnie jak w przypadku pisania pliku) potrzebna jest kompletna ścieżka. Rozwiązaniem jest użycie metody `MapPath` obiektu `Server`. Spowoduje to zwrócenie pełnej ścieżki do witryny sieci Web. Aby uzyskać ścieżkę do katalogu głównego witryny sieci Web, użytkownik może korzystać z operatora `~` (w celu represen wirtualnego katalogu głównego witryny) `MapPath`. (Można również przekazać do niego nazwę podfolderu, na przykład *~/App\_Data/* , aby uzyskać ścieżkę dla tego podfolderu). Następnie można połączyć dodatkowe informacje z dowolną metodą zwracaną w celu utworzenia pełnej ścieżki. W tym przykładzie należy dodać nazwę pliku. (Więcej informacji na temat sposobu pracy z ścieżkami plików i folderów można znaleźć w artykule [wprowadzenie do programowania ASP.NET stron sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)).

    Plik jest zapisywany w folderze *danych\_aplikacji* . Ten folder jest specjalnym folderem w ASP.NET, który jest używany do przechowywania plików danych, zgodnie z opisem w artykule [wprowadzenie do pracy z bazą danych w witrynach sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=195209).

    Metoda `WriteAllText` obiektu `File` zapisuje dane do pliku. Ta metoda przyjmuje dwa parametry: Nazwa (ścieżka) pliku do zapisu i rzeczywiste dane do zapisu. Należy zauważyć, że nazwa pierwszego parametru ma `@` znak jako prefiks. Oznacza to, że ASP.NET, że podajesz Verbatim literał ciągu i że znaki takie jak "/" nie powinny być interpretowane w specjalny sposób. (Aby uzyskać więcej informacji, zobacz [wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)).

    > [!NOTE]
    > Aby kod zapisywał pliki w folderze *danych\_aplikacji* , aplikacja musi mieć uprawnienia do odczytu i zapisu dla tego folderu. Na komputerze deweloperskim zwykle nie jest to problem. Jednak po opublikowaniu lokacji programu na serwerze sieci Web dostawcy hostingu może być konieczne jawne ustawienie tych uprawnień. Jeśli ten kod jest uruchamiany na serwerze dostawcy hostingu i pojawia się błąd, należy skontaktować się z dostawcą hostingu, aby dowiedzieć się, jak ustawić te uprawnienia.

- Uruchom stronę w przeglądarce. 

    ![](working-with-files/_static/image1.jpg)
- Wprowadź wartości w polach, a następnie kliknij przycisk **Prześlij**.
- Zamknij okno przeglądarki.
- Wróć do projektu i Odśwież widok.
- Otwórz plik *Data. txt* . Dane przesłane w formularzu są w pliku. 

    ![Image](working-with-files/_static/image2.jpg)
- Zamknij plik *Data. txt* .

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Dołączanie danych do istniejącego pliku

W poprzednim przykładzie użyto `WriteAllText`, aby utworzyć plik tekstowy, który ma tylko jedną część danych. Jeśli wywołasz metodę ponownie i przekażesz ją do tej samej nazwy pliku, istniejący plik zostanie całkowicie nadpisany. Jednak po utworzeniu pliku często należy dodać nowe dane na końcu pliku. Można to zrobić za pomocą metody `AppendAllText` obiektu `File`.

1. W witrynie sieci Web Utwórz kopię pliku *UserData. cshtml* i nadaj jej nazwę copy *UserDataMultiple. cshtml*.
2. Zastąp blok kodu przed otwierającym tagiem `<!DOCTYPE html>` i następującym blokiem kodu: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Ten kod zawiera jedną zmianę z poprzedniego przykładu. Zamiast używać `WriteAllText`, używa metody `the AppendAllText`. Metody są podobne, z tą różnicą, że `AppendAllText` dodaje dane na końcu pliku. Podobnie jak w przypadku `WriteAllText`, `AppendAllText` tworzy plik, jeśli jeszcze nie istnieje.
3. Uruchom stronę w przeglądarce.
4. Wprowadź wartości dla pól, a następnie kliknij przycisk **Prześlij**.
5. Dodaj więcej danych i ponownie prześlij formularz.
6. Wróć do projektu, kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij polecenie **Odśwież**.
7. Otwórz plik *Data. txt* . Teraz zawiera nowe dane, które zostały wprowadzone. 

    ![Image](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Odczytywanie i wyświetlanie danych z pliku

Nawet jeśli nie musisz pisać danych do pliku tekstowego, prawdopodobnie czasami trzeba będzie odczytywać dane z jednego z nich. Aby to zrobić, możesz ponownie użyć obiektu `File`. Można użyć obiektu `File`, aby odczytać każdy wiersz pojedynczo (rozdzielony podziałem wierszy) lub odczytać poszczególne elementy niezależnie od tego, jak są oddzielone.

Ta procedura pokazuje, jak odczytywać i wyświetlać dane, które zostały utworzone w poprzednim przykładzie.

1. W katalogu głównym witryny sieci Web Utwórz nowy plik o nazwie *DisplayData. cshtml*.
2. Zastąp istniejącą zawartość następującym: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Kod rozpocznie się, odczytując plik, który został utworzony w poprzednim przykładzie do zmiennej o nazwie `userData`, przy użyciu tego wywołania metody:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Kod, aby to zrobić, znajduje się w instrukcji `if`. Jeśli chcesz odczytać plik, dobrym pomysłem jest użycie metody `File.Exists`, aby określić, czy plik jest dostępny. Kod sprawdza również, czy plik jest pusty.

    Treść strony zawiera dwie `foreach` pętle, jeden zagnieżdżony w drugim. Pętla zewnętrzna `foreach` pobiera jeden wiersz jednocześnie z pliku danych. W takim przypadku linie są definiowane przez podziały wierszy w pliku &#8212; , który jest, każdy element danych znajduje się w osobnym wierszu. Pętla zewnętrzna tworzy nowy element (`<li>` element) wewnątrz listy uporządkowanej (`<ol>` elementu).

    Pętla wewnętrzna dzieli każdy wiersz danych na elementy (pola) przy użyciu przecinka jako ogranicznika. (Na podstawie poprzedniego przykładu oznacza to, że każdy wiersz zawiera trzy pola &#8212; imię, nazwisko i adres e-mail, z których każda jest oddzielona przecinkami). Pętla wewnętrzna tworzy również listę `<ul>` i wyświetla jeden element listy dla każdego pola w wierszu danych.

    Kod ilustruje sposób używania dwóch typów danych, tablicy i typu danych `char`. Tablica jest wymagana, ponieważ metoda `File.ReadAllLines` zwraca dane jako tablicę. Typ danych `char` jest wymagany, ponieważ metoda `Split` zwraca `array`, w którym każdy element jest typu `char`. (Aby uzyskać informacje na temat tablic, zobacz [wprowadzenie do programowania w sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects)).
3. Uruchom stronę w przeglądarce. Zostaną wyświetlone dane wprowadzone dla poprzednich przykładów. 

    ![Image](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Wyświetlanie danych z pliku rozdzielanego przecinkami programu Microsoft Excel**
> 
> Możesz użyć programu Microsoft Excel, aby zapisać dane zawarte w arkuszu kalkulacyjnym jako plik rozdzielany przecinkami (plik*CSV* ). Gdy to zrobisz, plik zostanie zapisany w postaci zwykłego tekstu, a nie w formacie programu Excel. Każdy wiersz w arkuszu kalkulacyjnym jest oddzielony podziałem wiersza w pliku tekstowym, a każdy element danych jest rozdzielony przecinkami. Możesz użyć kodu pokazanego w poprzednim przykładzie, aby odczytać plik rozdzielany przecinkami programu Excel, zmieniając nazwę pliku danych w kodzie.

<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Usuwanie plików

Aby usunąć pliki z witryny sieci Web, można użyć metody `File.Delete`. Ta procedura pokazuje, jak zezwolić użytkownikom na usuwanie obrazu (pliku*jpg* ) z folderu *obrazy* , jeśli zna nazwę pliku.

> [!NOTE] 
> 
> **Ważne** W produkcyjnej witrynie sieci Web zwykle ogranicza się, kto może wprowadzać zmiany w danych. Informacje o sposobie konfigurowania członkostwa i sposobach autoryzacji użytkowników do wykonywania zadań w witrynie można znaleźć w temacie [Dodawanie zabezpieczeń i członkostwa do witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).

1. W witrynie sieci Web utwórz podfolder o nazwie *obrazy*.
2. Skopiuj jeden lub więcej plików *. jpg* do folderu *obrazy* .
3. W folderze głównym witryny sieci Web Utwórz nowy plik o nazwie *operacja filedelete. cshtml*.
4. Zastąp istniejącą zawartość następującym: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Ta strona zawiera formularz, w którym użytkownicy mogą wprowadzić nazwę pliku obrazu. Nie wprowadzają one rozszerzenia nazwy pliku *. jpg* ; ograniczając nazwę pliku w ten sposób, uniemożliwiasz użytkownikom usuwanie dowolnych plików w witrynie.

    Kod odczytuje nazwę pliku wprowadzoną przez użytkownika, a następnie konstruuje kompletną ścieżkę. Aby można było utworzyć ścieżkę, kod używa bieżącej ścieżki witryny sieci Web (w sposób zwrócony przez metodę `Server.MapPath`), nazwy folderu *obrazów* , nazwy podanej przez użytkownika i ". jpg" jako ciągu literału.

    Aby usunąć plik, kod wywołuje metodę `File.Delete`, przekazując ją pełną ścieżkę, która właśnie została skonstruowana. Na końcu znacznika kod wyświetla komunikat potwierdzający, że plik został usunięty.
5. Uruchom stronę w przeglądarce. 

    ![Image](working-with-files/_static/image5.jpg)
6. Wprowadź nazwę pliku do usunięcia, a następnie kliknij przycisk **Prześlij**. Jeśli plik został usunięty, nazwa pliku zostanie wyświetlona w dolnej części strony.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Zezwalanie użytkownikom na przekazywanie pliku

Pomocnik `FileUpload` umożliwia użytkownikom przekazywanie plików do witryny sieci Web. W poniższej procedurze pokazano, jak umożliwić użytkownikom przekazywanie pojedynczego pliku.

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny internetowej zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie została ona wcześniej dodana.
2. W folderze *dane\_aplikacji* Utwórz nowy folder i nadaj mu nazwę *UploadedFiles*.
3. W katalogu głównym Utwórz nowy plik o nazwie *FileUpload. cshtml*.
4. Zastąp istniejącą zawartość na stronie następującymi: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Część treści strony używa pomocnika `FileUpload` do utworzenia pola przekazywania i przycisków, które są prawdopodobnie znane:

    ![Image](working-with-files/_static/image6.jpg)

    Właściwości ustawione dla pomocnika `FileUpload` określają, że chcesz, aby plik został przekazany do jednego pola, i że chcesz, aby przycisk przesyłania przeczytał **przekazywanie**. (W dalszej części artykułu dodasz więcej pól).

    Gdy użytkownik kliknie przycisk **Przekaż**, kod w górnej części strony pobiera plik i zapisuje go. Obiekt `Request`, który zwykle jest używany do pobierania wartości z pól formularza, ma również tablicę `Files`, która zawiera plik (lub pliki), które zostały przekazane. Możesz pobrać pojedyncze pliki z określonych pozycji w tablicy &#8212; na przykład, aby uzyskać pierwszy przekazany plik, uzyskasz `Request.Files[0]`, aby uzyskać drugi plik, uzyskasz `Request.Files[1]`i tak dalej. (Pamiętaj, że w programowaniu funkcja liczenia zwykle zaczyna się od zera).

    Po pobraniu przekazanego pliku należy umieścić go w zmiennej (tutaj `uploadedFile`), aby umożliwić manipulowanie nim. Aby określić nazwę przekazanego pliku, wystarczy uzyskać jego właściwość `FileName`. Jeśli jednak użytkownik przekaże plik, `FileName` zawiera oryginalną nazwę użytkownika, która obejmuje całą ścieżkę. Może wyglądać następująco:

    *C:\Users\Public\Sample.txt*

    Nie chcesz, aby wszystkie informacje o ścieżce były takie same jak ścieżki na komputerze użytkownika, a nie na serwerze. Wystarczy tylko rzeczywista nazwa pliku (*Sample. txt*). Można rozdzielić tylko plik ze ścieżki przy użyciu metody `Path.GetFileName`, tak jak to:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    Obiekt `Path` jest narzędziem, które ma wiele metod, takich jak to, których można użyć do rozdzielania ścieżek, łączenia ścieżek itd.

    Po otrzymaniu nazwy przekazanego pliku można utworzyć nową ścieżkę, w której ma zostać zapisany przekazany plik w witrynie sieci Web. W takim przypadku należy połączyć `Server.MapPath`, nazwy folderów (*App\_Data/UploadedFiles*) oraz nowo rozkrytą nazwę pliku w celu utworzenia nowej ścieżki. Następnie można wywołać metodę `SaveAs` przekazanego pliku, aby rzeczywiście zapisać plik.
5. Uruchom stronę w przeglądarce. 

    ![Image](working-with-files/_static/image7.jpg)
6. Kliknij przycisk **Przeglądaj** , a następnie wybierz plik do przekazania. 

    ![Image](working-with-files/_static/image8.jpg)

    Pole tekstowe obok przycisku **przeglądania** będzie zawierać ścieżkę i lokalizację pliku.

    ![Image](working-with-files/_static/image9.jpg)
7. Kliknij pozycję **Przekaż**.
8. W witrynie sieci Web kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij polecenie **Odśwież**.
9. Otwórz folder *UploadedFiles* . Przekazany plik znajduje się w folderze. 

    ![Image](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Zezwalanie użytkownikom na przekazywanie wielu plików

W poprzednim przykładzie można umożliwić użytkownikom przekazywanie jednego pliku. Można jednak użyć pomocnika `FileUpload`, aby przekazać więcej niż jeden plik jednocześnie. Jest to przydatne w przypadku scenariuszy, takich jak przekazywanie fotografii, gdzie przekazywanie jednego pliku w danym momencie jest żmudnym. (Informacje o przekazywaniu zdjęć w [pracy z obrazami można znaleźć w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897)). W tym przykładzie pokazano, jak umożliwić użytkownikom przekazywanie dwóch naraz, chociaż możesz użyć tej samej techniki, aby przekazywać więcej niż te informacje.

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).
2. Utwórz nową stronę o nazwie *FileUploadMultiple. cshtml*.
3. Zastąp istniejącą zawartość na stronie następującymi:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    W tym przykładzie pomocnik `FileUpload` w treści strony jest skonfigurowany tak, aby umożliwić użytkownikom przekazywanie domyślnie dwóch plików. Ponieważ `allowMoreFilesToBeAdded` jest ustawiona na `true`, pomocnik renderuje link, który umożliwia użytkownikowi dodawanie kolejnych pól przekazywania:

    ![Image](working-with-files/_static/image11.jpg)

    Aby przetworzyć pliki, które przekazuje użytkownik, kod używa tej samej podstawowej techniki, która została użyta w poprzednim przykładzie &#8212; Pobierz plik z `Request.Files` a następnie zapisz go. (W tym różne czynności, które należy wykonać, aby uzyskać właściwą nazwę i ścieżkę pliku). Innowacje w tym czasie mogą przekazywać wiele plików, a użytkownik nie wie wielu. Aby dowiedzieć się, możesz uzyskać `Request.Files.Count`.

    Dzięki tej liczbie można przechodzić między `Request.Files`, pobierać każdy plik z kolei i zapisywać go. Gdy chcesz zapętlać znaną liczbę razy za pomocą kolekcji, możesz użyć pętli `for`, tak jak to:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Zmienna `i` jest tylko tymczasowym licznikiem, który przejdzie od zera do dowolnego ustawionego górnego limitu. W takim przypadku górny limit to liczba plików. Ale ponieważ licznik zaczyna się od zera, jak zwykle jest to typowy dla obliczeń scenariuszy w ASP.NET, górny limit jest w rzeczywistości mniejszy niż liczba plików. (Jeśli przekazano trzy pliki, liczba jest równa zero do 2).

    Zmienna `uploadedCount` sumuje wszystkie pliki, które zostały pomyślnie przekazane i zapisane. Ten kod może nie być w stanie przekazywać oczekiwanego pliku.
4. Uruchom stronę w przeglądarce. Przeglądarka wyświetli stronę i jej dwa pola przekazywania.
5. Wybierz dwa pliki do przekazania.
6. Kliknij przycisk **Dodaj inny plik**. Na stronie zostanie wyświetlone nowe pole przekazywania. 

    ![Image](working-with-files/_static/image12.jpg)
7. Kliknij pozycję **Przekaż**.
8. W witrynie sieci Web kliknij prawym przyciskiem myszy folder projektu, a następnie kliknij polecenie **Odśwież**.
9. Otwórz folder *UploadedFiles* , aby zobaczyć pomyślnie przekazane pliki.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Praca z obrazami w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897)

[Eksportowanie do pliku CSV](https://msdn.microsoft.com/library/ms155919.aspx)

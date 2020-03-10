---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Praca z obrazami w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale pokazano, jak dodawać i wyświetlać obrazy (zmieniać rozmiar, przerzucać i dodawać znaki wodne) w witrynie sieci Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 53514b3c314fc182a43c82974ffcfa8158a636a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631860"
---
# <a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Praca z obrazami w witrynie ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób dodawania, wyświetlania i manipulowania obrazami (zmiany rozmiaru, przerzucania i dodawania znaków wodnych) w witrynie internetowej ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Sposób dynamicznego dodawania obrazu do strony.
> - Jak umożliwić użytkownikom przekazywanie obrazu.
> - Jak zmienić rozmiar obrazu.
> - Jak przerzucić lub obrócić obraz.
> - Jak dodać znak wodny do obrazu.
> - Jak używać obrazu jako znaku wodnego.
> 
> Są to funkcje programowania ASP.NET wprowadzone w artykule:
> 
> - Pomocnik `WebImage`.
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

<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Dynamiczne Dodawanie obrazu do strony sieci Web

Podczas tworzenia witryny sieci Web można dodawać obrazy do witryny sieci Web oraz do poszczególnych stron. Możesz również umożliwić użytkownikom przekazywanie obrazów, co może być przydatne w przypadku zadań, takich jak dodanie zdjęcia do profilu.

Jeśli obraz jest już dostępny w witrynie i chcesz, aby był on wyświetlany na stronie, użyj elementu HTML `<img>` w następujący sposób:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Czasami trzeba mieć możliwość dynamicznego &#8212; wyświetlania obrazów, co oznacza, że nie wiesz, jaki obraz ma być wyświetlany, dopóki strona nie zostanie uruchomiona.

Procedura opisana w tej sekcji pokazuje, jak wyświetlić obraz na bieżąco, gdzie użytkownicy określają nazwę pliku obrazu na podstawie listy nazw obrazów. Wybierają nazwę obrazu z listy rozwijanej, a po przesłaniu strony zostanie wyświetlony wybrany obraz.

![Image](9-working-with-images/_static/image1.jpg "ch9images-1. jpg")

1. W programie WebMatrix Utwórz nową witrynę sieci Web.
2. Dodaj nową stronę o nazwie *DynamicImage. cshtml*.
3. W folderze głównym witryny sieci Web Dodaj nowy folder i nadaj mu nazwę *obrazy*.
4. Dodaj cztery obrazy do folderu *obrazy* , który właśnie został utworzony. (Wszystkie obrazy, które są przydatne, będą się znajdować na stronie). Zmień nazwy obrazów *Photo1. jpg*, *Photo2. jpg*, *Photo3. jpg*i *Photo4. jpg*. (Nie będziesz używać *Photo4. jpg* w tej procedurze, ale będziesz ich używać w dalszej części artykułu).
5. Upewnij się, że cztery obrazy nie są oznaczone jako tylko do odczytu.
6. Zastąp istniejącą zawartość na stronie następującymi:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Treść strony ma listę rozwijaną (`<select>` element) o nazwie `photoChoice`. Lista zawiera trzy opcje, a atrybut `value` każdej opcji listy ma nazwę jednego z obrazów umieszczonych w folderze *obrazy* . Zasadniczo lista umożliwia użytkownikowi wybranie przyjaznej nazwy, takiej jak &quot;Photo 1&quot;, a następnie przekazanie nazwy pliku *jpg* podczas przesyłania strony.

    W kodzie można uzyskać wybór użytkownika (innymi słowy, nazwę pliku obrazu) z listy, odczytując `Request["photoChoice"]`. Najpierw widzisz, czy w ogóle istnieje zaznaczenie. Jeśli istnieje, należy skonstruować ścieżkę do obrazu, który składa się z nazwy folderu dla obrazów i nazwy pliku obrazu użytkownika. (Jeśli próbowano utworzyć ścieżkę, ale nie było nic w `Request["photoChoice"]`, wystąpi błąd). Skutkuje to ścieżką względną w następujący sposób:

    *obrazy/Photo1. jpg*

    Ścieżka jest przechowywana w zmiennej o nazwie `imagePath`, która będzie potrzebna później na stronie.

    W treści znajduje się również element `<img>`, który służy do wyświetlania obrazu, który został wybrany przez użytkownika. Atrybut `src` nie jest ustawiony na nazwę pliku lub adres URL, na przykład w celu wyświetlenia statycznego elementu. Zamiast tego jest ustawiony na `@imagePath`, co oznacza, że pobiera jego wartość z ścieżki ustawionej w kodzie.

    Przy pierwszym uruchomieniu strony nie ma obrazu do wyświetlenia, ponieważ użytkownik nie zaznaczył niczego. Zwykle oznacza to, że atrybut `src` będzie pusty, a obraz będzie wyświetlany jako czerwona &quot;x&quot; (lub niezależnie od tego, czy przeglądarka jest renderowana, gdy nie można znaleźć obrazu). Aby tego uniknąć, należy umieścić element `<img>` w bloku `if`, który sprawdza, czy zmienna `imagePath` ma coś w nim. Jeśli użytkownik wykonał zaznaczenie, `imagePath` zawiera ścieżkę. Jeśli użytkownik nie wybrał obrazu lub gdy jest wyświetlany po raz pierwszy, element `<img>` nie jest jeszcze renderowany.
7. Zapisz plik i Uruchom stronę w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem).
8. Wybierz obraz z listy rozwijanej, a następnie kliknij pozycję **przykład obrazu**. Upewnij się, że widzisz różne obrazy dla różnych opcji.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Przekazywanie obrazu

W poprzednim przykładzie pokazano sposób dynamicznego wyświetlania obrazu, ale działał on tylko z obrazami, które już znajdowały się w witrynie sieci Web. Ta procedura pokazuje, jak umożliwić użytkownikom przekazywanie obrazu, który jest następnie wyświetlany na stronie. W ASP.NET można manipulować obrazami na bieżąco przy użyciu pomocnika `WebImage`, który ma metody umożliwiające tworzenie, manipulowanie i zapisywanie obrazów. Pomocnik `WebImage` obsługuje wszystkie popularne typy plików obrazów sieci Web, w tym *jpg*, *. png*i *. bmp*. W tym artykule zostaną użyte obrazy *jpg* , ale można użyć dowolnego typu obrazu.

![Image](9-working-with-images/_static/image2.jpg "ch9images-2. jpg")

1. Dodaj nową stronę i nadaj jej nazwę *UploadImage. cshtml*.
2. Zastąp istniejącą zawartość na stronie następującymi: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Treść tekstu ma `<input type="file">` element, który umożliwia użytkownikom wybranie pliku do przekazania. Po kliknięciu przycisku **Prześlij**, pobrany plik zostanie przesłany wraz z formularzem.

    Aby uzyskać przekazany obraz, użyj pomocnika `WebImage`, który ma wszystkie przydatne metody pracy z obrazami. W celu uzyskania przekazanego obrazu (jeśli istnieje) i zapisania go w zmiennej o nazwie `photo`można użyć `WebImage.GetImageFromRequest`.

    Duża część pracy w tym przykładzie obejmuje pobieranie i Ustawianie nazw plików i ścieżek. Problem polega na tym, że chcesz uzyskać nazwę (i tylko nazwę) obrazu przekazanego przez użytkownika, a następnie utworzyć nową ścieżkę do lokalizacji, w której ma zostać zapisany obraz. Ponieważ użytkownicy mogą potencjalnie przekazać wiele obrazów o tej samej nazwie, należy użyć dodatkowego kodu do utworzenia unikatowych nazw i upewnić się, że użytkownicy nie zastępują istniejących obrazów.

    Jeśli obraz rzeczywiście został przekazany (test `if (photo != null)`), nazwa obrazu jest pobierana z właściwości `FileName` obrazu. Gdy użytkownik przekaże obraz, `FileName` zawiera oryginalną nazwę użytkownika, która zawiera ścieżkę z komputera użytkownika. Może wyglądać następująco:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Nie chcesz, aby wszystkie informacje o ścieżce były &#8212; aktualne, mimo że chcesz tylko rzeczywistą nazwę pliku (*SamplePhoto1. jpg*). Można rozdzielić tylko plik ze ścieżki przy użyciu metody `Path.GetFileName`, tak jak to:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Następnie można utworzyć nową unikatową nazwę pliku, dodając identyfikator GUID do oryginalnej nazwy. (Aby uzyskać więcej informacji o identyfikatorach GUID, zobacz [Informacje o identyfikatorach GUID](#SB_AboutGUIDs) w dalszej części tego artykułu). Następnie utworzysz kompletną ścieżkę, której można użyć do zapisania obrazu. Ścieżka zapisu składa się z nowej nazwy pliku, folderu (obrazów) i bieżącej lokalizacji witryny sieci Web.

    > [!NOTE]
    > Aby kod zapisywał pliki w folderze *obrazy* , aplikacja musi mieć uprawnienia do odczytu i zapisu dla tego folderu. Na komputerze deweloperskim zwykle nie jest to problem. Jednak po opublikowaniu lokacji programu na serwerze sieci Web dostawcy hostingu może być konieczne jawne ustawienie tych uprawnień. Jeśli ten kod jest uruchamiany na serwerze dostawcy hostingu i pojawia się błąd, należy skontaktować się z dostawcą hostingu, aby dowiedzieć się, jak ustawić te uprawnienia.

    Na koniec przekazanie ścieżki zapisu do metody `Save` pomocnika `WebImage`. Spowoduje to zapisanie przekazanego obrazu pod nową nazwą. Metoda Save wygląda następująco: `photo.Save(@"~\" + imagePath)`. Pełna ścieżka jest dołączana do `@"~\"`, która jest bieżącą lokalizacją witryny sieci Web. (Aby uzyskać informacje na temat operatora `~`, zobacz [wprowadzenie do programowania w sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths)).

    Jak w poprzednim przykładzie, treść strony zawiera element `<img>`, aby wyświetlić obraz. Jeśli ustawiono `imagePath`, element `<img>` jest renderowany, a jego atrybut `src` jest ustawiony na wartość `imagePath`.
3. Uruchom stronę w przeglądarce.
4. Przekaż obraz i upewnij się, że jest on wyświetlany na stronie.
5. W swojej lokacji Otwórz folder *images* . Zobaczysz, że dodano nowy plik, którego nazwa pliku wygląda następująco: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_moje zdjęcie. png*

    Jest to obraz, który został przekazany z identyfikatorem GUID poprzedzonym prefiksem nazwy. (Własny plik ma inny identyfikator GUID i prawdopodobnie nazywa się coś innego niż *. png*).

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Identyfikatory GUID — informacje
> 
> Identyfikator GUID (Globally Unique ID) jest identyfikatorem, który zwykle jest renderowany w formacie podobnym do: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Liczby i litery (od A-F) różnią się w zależności od identyfikatora GUID, ale wszystkie są zgodne z wzorcem używania grup z 8-4-4-4-12 znaków. (Technicznie identyfikator GUID jest liczbą 16-bajtową/128-bitową). Jeśli potrzebujesz identyfikatora GUID, możesz wywołać wyspecjalizowany kod, który generuje identyfikator GUID. Pomysł związany z identyfikatorami GUID polega na tym, że pomiędzy olbrzymim rozmiarem liczby (3,4 x 10<sup>38</sup>) a algorytmem generowania go, wynikowy numer jest praktycznie gwarantowany jako jeden z rodzajów. Dlatego identyfikatory GUID są dobrym sposobem na generowanie nazw dla elementów, gdy należy zagwarantujeć, że nie będziesz używać tej samej nazwy dwa razy. Minusem, oczywiście, jest to, że identyfikatory GUID nie są szczególnie przyjazne dla użytkownika, więc mają być używane, gdy nazwa jest używana tylko w kodzie.

<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Zmiana rozmiaru obrazu

Jeśli witryna sieci Web akceptuje obrazy od użytkownika, możesz chcieć zmienić rozmiar obrazów przed ich wyświetleniem lub zapisaniem. Można ponownie użyć pomocnika `WebImage` dla tego elementu.

Ta procedura pokazuje, jak zmienić rozmiar przekazanego obrazu w celu utworzenia miniatury, a następnie zapisania miniatury i oryginalnego obrazu w witrynie sieci Web. Wyświetlenie miniatury na stronie i użycie hiperłącza umożliwia przekierowanie użytkowników do pełnego obrazu.

![Image](9-working-with-images/_static/image3.jpg "ch9images-3. jpg")

1. Dodaj nową stronę o nazwie *Thumbnail. cshtml*.
2. W folderze *obrazy* utwórz podfolder o nazwie *kciuki*.
3. Zastąp istniejącą zawartość na stronie następującymi: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Ten kod jest podobny do kodu z poprzedniego przykładu. Różnica polega na tym, że ten kod zapisuje dwa razy, po utworzeniu miniatury obrazu. Najpierw uzyskasz przekazany obraz i zapiszesz go w folderze *images* . Następnie utwórz nową ścieżkę dla obrazu miniatury. Aby w rzeczywistości utworzyć miniaturę, należy wywołać metodę `Resize` pomocnika `WebImage`, aby utworzyć obraz 60 pikseli przez 60 pikseli. W przykładzie pokazano, jak zachować współczynnik proporcji i jak można zapobiec powiększaniu obrazu (w przypadku, gdy nowy rozmiar będzie w rzeczywistości większy). Obraz o zmienionym rozmiarze jest następnie zapisywany w podfolderze *kciuków* .

    Na końcu znacznika należy użyć tego samego `<img>` elementu z atrybutem Dynamic `src`, który był widoczny w poprzednich przykładach, aby warunkowo pokazać obraz. W takim przypadku wyświetlana jest miniatura. Należy również użyć elementu `<a>`, aby utworzyć hiperłącze do dużej wersji obrazu. Podobnie jak w przypadku atrybutu `src` elementu `<img>`, atrybut `href` elementu `<a>` jest ustawiany dynamicznie do każdego, co jest w `imagePath`. Aby upewnić się, że ścieżka może współpracować z adresem URL, należy przekazać `imagePath` do metody `Html.AttributeEncode`, która konwertuje zastrzeżone znaki w ścieżce do znaków, które są poprawne w adresie URL.
4. Uruchom stronę w przeglądarce.
5. Przekaż zdjęcie i sprawdź, czy miniatura jest pokazana.
6. Kliknij miniaturę, aby wyświetlić obraz w pełnym rozmiarze.
7. Zwróć uwagę na *obrazy* i *obrazy/kciuki*, aby dodać nowe pliki.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Obracanie i przerzucanie obrazu

Pomocnik `WebImage` umożliwia również przerzucanie i obracanie obrazów. Ta procedura pokazuje, jak pobrać obraz z serwera, przerzucić obraz o 180 stopni (w pionie), zapisać go, a następnie wyświetlić przerzucony obraz na stronie. W tym przykładzie właśnie korzystasz z pliku znajdującego się już na serwerze (*Photo2. jpg*). W rzeczywistej aplikacji prawdopodobnie zarzucasz obraz, którego nazwa otrzymujesz dynamicznie, podobnie jak w poprzednich przykładach.

![Image](9-working-with-images/_static/image4.jpg "ch9images-4. jpg")

1. Dodaj nową stronę o nazwie *FlipImage. cshtml*.
2. Zastąp istniejącą zawartość na stronie następującymi: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Kod używa pomocnika `WebImage` do pobrania obrazu z serwera. Ścieżkę do obrazu można utworzyć za pomocą tej samej techniki, która została użyta we wcześniejszych przykładach do zapisywania obrazów, i przekazać tę ścieżkę podczas tworzenia obrazu przy użyciu `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    W przypadku znalezienia obrazu można utworzyć nową ścieżkę i nazwę pliku, tak jak w poprzednich przykładach. Aby przerzucić obraz, należy wywołać metodę `FlipVertical`, a następnie ponownie zapisać obraz.

    Obraz zostanie ponownie wyświetlony na stronie przy użyciu `<img>` elementu z atrybutem `src` ustawionym na `imagePath`.
3. Uruchom stronę w przeglądarce. Obraz dla *Photo2. jpg* zostanie wyświetlony na osi poziomej.
4. Odśwież stronę lub zażądaj ponownie strony, aby zobaczyć, że obraz został przerzucony ponownie po prawej stronie.

Aby obrócić obraz, należy użyć tego samego kodu, z tą różnicą, że zamiast wywoływania `FlipVertical` lub `FlipHorizontal`, należy wywołać `RotateLeft` lub `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Dodawanie znaku wodnego do obrazu

Podczas dodawania obrazów do witryny sieci Web możesz chcieć dodać znak wodny do obrazu przed jego zapisaniem lub wyświetlić na stronie. Osoby często używają znaków wodnych, aby dodawać informacje o prawach autorskich do obrazu lub anonsować swoją nazwę biznesową.

![Image](9-working-with-images/_static/image5.jpg "ch9images-5. jpg")

1. Dodaj nową stronę o nazwie *znak wodny. cshtml*.
2. Zastąp istniejącą zawartość na stronie następującymi: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Ten kod jest podobny do kodu na stronie *FlipImage. cshtml* z wcześniejszego (chociaż ten czas używa pliku *Photo3. jpg* ). Aby dodać znak wodny, należy wywołać metodę `AddTextWatermark` pomocnika `WebImage` przed zapisaniem obrazu. W wywołaniu do `AddTextWatermark`przekazywać tekst &quot;"mój znak wodny&quot;, Ustaw kolor czcionki na żółty i ustaw dla rodziny czcionek czcionkę Arial. (Chociaż nie jest to tutaj widoczne, pomocnik `WebImage` umożliwia również określanie krycia, rodziny czcionek i rozmiaru czcionki oraz położenia tekstu znaku wodnego.) Po zapisaniu obrazu nie może być tylko do odczytu.

    Jak widać wcześniej, obraz jest wyświetlany na stronie przy użyciu elementu `<img>` z atrybutem src ustawionym na `@imagePath`.
3. Uruchom stronę w przeglądarce. Zwróć uwagę na tekst "mój znak wodny" w prawym dolnym rogu obrazu.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Używanie obrazu jako znaku wodnego

Zamiast używać tekstu dla znaku wodnego, można użyć innego obrazu. Osoby czasami używają obrazów takich jak logo firmy jako znak wodny lub używają obrazu znaku wodnego zamiast tekstu do informacji o prawach autorskich.

![Image](9-working-with-images/_static/image6.jpg "ch9images-6. jpg")

1. Dodaj nową stronę o nazwie *ImageWatermark. cshtml*.
2. Dodaj obraz do folderu *obrazy* , którego możesz użyć jako logo, a następnie zmień nazwę obrazu *MyCompanyLogo. jpg*. Obraz powinien być obrazem, który można zobaczyć jasno, gdy jest ustawiony na 80 pikseli szerokości i 20 pikseli.
3. Zastąp istniejącą zawartość na stronie następującymi: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Jest to kolejna odmiana kodu z wcześniejszych przykładów. W takim przypadku należy wywołać `AddImageWatermark`, aby dodać obraz znaku wodnego do obrazu docelowego (*Photo3. jpg*) przed zapisaniem obrazu. Gdy wywołasz `AddImageWatermark`, Szerokość jest ustawiana na 80 pikseli i wysokość do 20 pikseli. Obraz *MyCompanyLogo. jpg* jest wyrównany w poziomie i wyrównany w pionie w dolnej części obrazu docelowego. Nieprzezroczystość jest ustawiona na 100%, a uzupełnienie ma wartość 10 pikseli. Jeśli obraz znaku wodnego jest większy niż obraz docelowy, nic się nie dzieje. Jeśli obraz znaku wodnego jest większy niż obraz docelowy i ustawisz uzupełnienie dla znaku wodnego obrazu na zero, znak wodny zostanie zignorowany.

    Tak jak wcześniej, obraz zostanie wyświetlony przy użyciu elementu `<img>` i atrybutu `src` Dynamic.
4. Uruchom stronę w przeglądarce. Zauważ, że obraz znaku wodnego pojawia się u dołu obrazu głównego.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Praca z plikami w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202896)

[Wprowadzenie do programowania stron sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587)

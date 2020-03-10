---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Ten dodatek zawiera omówienie programowania za pomocą ASP.NET stron sieci Web w Visual Basic przy użyciu składnia Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526594"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor (Visual Basic)

Autor [FitzMacken](https://github.com/tfitzmac)

> Ten artykuł zawiera omówienie programowania za pomocą ASP.NET stron sieci Web przy użyciu składnia Razor i Visual Basic. ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznych stron sieci Web na serwerach sieci Web.
> 
> Dowiesz **się**:
> 
> - 8 najważniejszych porad programistycznych dotyczących rozpoczynania pracy z programowaniem ASP.NET stron sieci Web przy użyciu programu składnia Razor.
> - Podstawowe pojęcia związane z programowaniem, które będą potrzebne.
> - Co to jest kod serwera ASP.NET i składnia Razor.
>   
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

Większość przykładów używania stron sieci Web ASP.NET z użyciem C#składnia Razor. Jednak składnia Razor obsługuje również Visual Basic. Aby program ASP.NET stronę sieci Web w Visual Basic, należy utworzyć stronę sieci Web z rozszerzeniem nazwy pliku *. vbhtml* , a następnie dodać kod Visual Basic. Ten artykuł zawiera omówienie pracy z Visual Basic językiem i składnią w celu tworzenia stron sieci Web ASP.NET.

> [!NOTE]
> Domyślne szablony witryn sieci Web dla programu Microsoft WebMatrix (**piekarni**, **Photo Gallery**i **Starter site**itp.) są dostępne w C# systemach i Visual Basic. Szablony Visual Basic można zainstalować za pomocą programu jako pakietów NuGet. Szablony witryn sieci Web są instalowane w folderze głównym witryny w folderze o nazwie *Szablony firmy Microsoft*.

## <a name="the-top-8-programming-tips"></a>8 najważniejszych wskazówek dotyczących programowania

W tej sekcji przedstawiono kilka wskazówek, które należy znać podczas pisania kodu serwera ASP.NET przy użyciu składnia Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Dodaj kod do strony przy użyciu znaku @

Znak `@` uruchamia wyrażenia śródwierszowe, bloki pojedynczej instrukcji i bloki zawierające wiele instrukcji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Wynik wyświetlany w przeglądarce:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Kodowanie HTML**
> 
> Gdy wyświetlasz zawartość na stronie przy użyciu znaku `@`, jak w powyższych przykładach, ASP.NET HTML — koduje dane wyjściowe. Zastępuje to zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) kodami, które umożliwiają wyświetlanie znaków jako znaków na stronie sieci Web zamiast interpretowania ich jako tagów HTML lub jednostek. Bez kodowania HTML dane wyjściowe z kodu serwera mogą nie być wyświetlane prawidłowo i mogą uwidaczniać zagrożenie bezpieczeństwa.
> 
> Jeśli celem jest wyprowadzanie kodu HTML, który renderuje Tagi jako znaczniki (na przykład `<p></p>` akapitu lub `<em></em>` w celu wyróżnienia tekstu), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu w](#BM_CombiningTextMarkupAndCode) dalszej części tego artykułu.
> 
> Więcej informacji na temat kodowania HTML można znaleźć w artykule [Praca z formularzami HTML w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. bloki kodu zostały ujęte w kodzie... Kod końcowy

Blok kodu zawiera jedną lub więcej instrukcji kodu i jest ujęty w słowa kluczowe `Code` i `End Code`. Umieść otwierający `Code` słowo kluczowe bezpośrednio po znaku &#8212; `@` nie może być odstęp między nimi.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Wynik wyświetlany w przeglądarce:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. wewnątrz bloku można zakończyć każdą instrukcję Code z podziałem wiersza

W bloku kodu Visual Basic każda instrukcja zostaje zakończona podziałem wiersza. (W dalszej części artykułu zobaczysz sposób zawijania długiej instrukcji kodu do wielu wierszy w razie potrzeby).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Użyj zmiennych do przechowywania wartości

Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty itp. Tworzysz nową zmienną za pomocą słowa kluczowego `Dim`. Można wstawiać wartości zmiennych bezpośrednio na stronie przy użyciu `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Wynik wyświetlany w przeglądarce:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. w podwójnym cudzysłowie należy umieścić wartości ciągu literału

*Ciąg* jest sekwencją znaków, które są traktowane jako tekst. Aby określić ciąg, należy umieścić go w podwójnym cudzysłowie:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Aby osadzić znaki podwójnego cudzysłowu w ciągu wartości ciągu, Wstaw dwa znaki podwójnego cudzysłowu. Jeśli chcesz, aby znak podwójnego cudzysłowu pojawił się raz w danych wyjściowych na stronie, wprowadź go jako `""` w ciągu ujętym w cudzysłów, a jeśli chcesz, aby pojawił się dwukrotnie, wprowadź go jako `""""` w ciągu ujętym w cudzysłów.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Wynik wyświetlany w przeglądarce:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. kod Visual Basic nie uwzględnia wielkości liter

W języku Visual Basic nie jest rozróżniana wielkość liter. W każdym przypadku można napisać słowa kluczowe programowania (takie jak `Dim`, `If`i `True`) oraz nazwy zmiennych (takie jak `myString`lub `subTotal`).

Następujące wiersze kodu przypisują wartość do zmiennej `lastname` przy użyciu nazwy z małymi literami, a następnie wyprowadza wartość zmiennej na stronę przy użyciu nazwy z wielką literą.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Wynik wyświetlany w przeglądarce:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. większość kodowania obejmuje pracę z obiektami

Obiekt reprezentuje element, który można zaprogramować ze &#8212; stroną, polem tekstowym, plikiem, obrazem, żądaniem sieci Web, wiadomością e-mail, rekordem klienta (wierszem bazy danych) itd. Obiekty mają właściwości opisujące ich właściwości &#8212; obiekt pola tekstowego ma właściwość `Text`, obiekt żądania ma właściwość `Url`, wiadomość e-mail ma właściwość `From`, a obiekt klienta ma właściwość `FirstName`. Obiekty mają również metody, które są zleceniami &quot;,&quot; mogą być wykonywane. Przykłady obejmują metodę `Save` obiektu pliku, metodę `Rotate` obiektu obrazu oraz metodę `Send` obiektu poczty e-mail.

Często pracujesz z obiektem `Request`, który zawiera informacje, takie jak wartości pól formularza na stronie (pola tekstowe itd.), typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd. Ten przykład pokazuje, jak uzyskać dostęp do właściwości obiektu `Request` i jak wywołać metodę `MapPath` obiektu `Request`, która zapewnia ścieżkę bezwzględną strony na serwerze:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Wynik wyświetlany w przeglądarce:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. możesz napisać kod, który podejmuje decyzje

Kluczową cechą dynamicznych stron sieci Web jest określenie, jakie czynności należy wykonać na podstawie warunków. Najbardziej typowym sposobem wykonania tej czynności jest wykonanie instrukcji `If` (i opcjonalnej instrukcji `Else`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Instrukcja `If IsPost` to skrócony sposób pisania `If IsPost = True`. Wraz z `If` instrukcjami istnieją różne sposoby testowania warunków, powtarzania bloków kodu i tak dalej, które opisano w dalszej części tego artykułu.

Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **Prześlij**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **Metody GET i POST protokołu HTTP oraz Właściwość ispost**
> 
> Protokół używany przez strony sieci Web (HTTP) obsługuje bardzo ograniczoną liczbę metod (&quot;czasownik&quot;), które są używane do żądania do serwera. Dwa Najczęstsze są pobieranie, które są używane do odczytywania strony i wpisu, które są używane do przesyłania strony. Na ogół podczas pierwszego żądania strony użytkownik żąda strony przy użyciu polecenia GET. Jeśli użytkownik wypełni formularz, a następnie kliknie przycisk **Prześlij**, przeglądarka wysyła żądanie post do serwera.
> 
> W programowaniu sieci Web często warto wiedzieć, czy strona jest żądana jako GET, czy jako wpis, aby poznać sposób przetwarzania strony. Na stronach sieci Web ASP.NET można użyć właściwości `IsPost`, aby sprawdzić, czy żądanie jest GET lub POST. Jeśli żądanie jest WPISem, właściwość `IsPost` zwróci wartość true i można wykonać czynności, takie jak odczytywanie wartości pól tekstowych w formularzu. Wiele przykładów zobaczysz, jak przetwarzać stronę w różny sposób w zależności od wartości `IsPost`.

## <a name="a-simple-code-example"></a>Prosty przykład kodu

Ta procedura pokazuje, jak utworzyć stronę przedstawiającą podstawowe techniki programowania. W tym przykładzie utworzysz stronę umożliwiającą użytkownikom wprowadzanie dwóch liczb, a następnie ich dodanie i wyświetlenie wyniku.

1. W edytorze Utwórz nowy plik i nadaj mu nazwę *AddNumbers. vbhtml*.
2. Skopiuj poniższy kod i adiustację na stronie, zastępując wszystkie elementy znajdujące się już na stronie.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Oto kilka rzeczy, dla których warto zwrócić uwagę:

    - Znak `@` uruchamia pierwszy blok kodu na stronie i poprzedza zmienną `totalMessage` osadzoną w dolnej części.
    - Blok w górnej części strony jest ujęty w `Code...End Code`.
    - Zmienne `total`, `num1`, `num2`i `totalMessage` przechowują kilka liczb i ciąg.
    - Wartość ciągu literału przypisana do zmiennej `totalMessage` znajduje się w podwójnym cudzysłowie.
    - Ponieważ w kodzie Visual Basic nie jest rozróżniana wielkość liter, gdy zmienna `totalMessage` jest używana w dolnej części strony, jej nazwa musi być zgodna z pisownią deklaracji zmiennej w górnej części strony. Wielkość liter nie ma znaczenia.
    - Wyrażenie `num1.AsInt()` + `num2.AsInt()` pokazuje, jak korzystać z obiektów i metod. Metoda `AsInt` dla każdej zmiennej konwertuje ciąg wprowadzony przez użytkownika na liczbę całkowitą (liczbę całkowitą), którą można dodać.
    - Tag `<form>` zawiera atrybut `method="post"`. Oznacza to, że gdy użytkownik kliknie przycisk **Dodaj**, Strona zostanie wysłana na serwer przy użyciu metody POST protokołu HTTP. Gdy strona zostanie przesłana, kod `If IsPost` ma wartość true, a kod warunkowy zostanie uruchomiony, wyświetlając wynik dodawania liczb.
3. Zapisz stronę i uruchom ją w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Wprowadź dwie liczby całkowite, a następnie kliknij przycisk **Dodaj** .

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic język i składnia

Wcześniej przedstawiono podstawowy przykład tworzenia strony sieci Web ASP.NET oraz sposób dodawania kodu serwerowego do znacznika HTML. W tym artykule poznasz podstawy używania Visual Basic do pisania kodu serwera ASP.NET przy użyciu składnia Razor &#8212; , które są regułami języka programowania.

Jeśli masz doświadczenie z programowaniem (zwłaszcza jeśli użyto języka C, C++, C#, Visual Basic lub JavaScript), większość czytanych tutaj informacji będzie znana. Prawdopodobnie trzeba będzie zaznajomić się tylko z informacjami o sposobie dodawania kodu WebMatrix do znaczników w plikach *. vbhtml* .

### <a id="BM_CombiningTextMarkupAndCode"></a>Łączenie tekstu, znaczników i kodu w blokach kodu

W blokach kodu serwera często chcesz, aby tekst i adiustacje były wyprowadzane na stronie. Jeśli blok kodu serwera zawiera tekst, który nie jest kodem, a zamiast tego powinien być renderowany jako, ASP.NET musi mieć możliwość odróżnienia tego tekstu od kodu. Istnieje kilka sposobów, aby to zrobić.

- Ujmij tekst w element bloku HTML, taki jak `<p></p>` lub `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    Element HTML może zawierać tekst, dodatkowe elementy HTML i wyrażenia kodu serwera. Gdy ASP.NET widzi tag otwierającego HTML (na przykład `<p>`), renderuje wszystko element i jego zawartość jako plik przeglądarki (i rozwiązuje wyrażenie kodu serwera).

- Użyj operatora `@:` lub elementu `<text>`. `@:` wyprowadza pojedynczy wiersz zawartości zawierający zwykły tekst lub niedopasowane Tagi HTML; element `<text>` obejmuje wiele wierszy do danych wyjściowych. Te opcje są przydatne, gdy nie chcesz renderować elementu HTML jako części danych wyjściowych.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Poniższy przykład powtarza poprzedni przykład, ale używa pojedynczej pary tagów `<text>`, aby umieścić tekst do renderowania.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    W poniższym przykładzie Tagi `<text>` i `</text>` zawierają trzy wiersze, z których wszystkie mają niezwierany tekst i niedopasowane Tagi HTML (`<br />`), a także kod serwera i dopasowane Tagi HTML. Ponownie można także poprzedzać każdy wiersz pojedynczym operatorem `@:`; w dowolny sposób działa.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Podczas wyprowadzania tekstu, jak pokazano w tej &#8212; sekcji przy użyciu elementu HTML, operatora `@:` lub elementu &#8212; `<text>` ASP.NET nie jest kodowane w języku HTML. (Jak wspomniano wcześniej, ASP.NET koduje dane wyjściowe wyrażeń kodów serwera i bloków kodu serwera, które są poprzedzone `@`, z wyjątkiem przypadków specjalnych zanotowanych w tej sekcji).

### <a name="whitespace"></a>Odstępu

Dodatkowe spacje w instrukcji (i poza literałem ciągu) nie wpływają na instrukcję:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Przerywanie długich instrukcji w wielu wierszach

Można przerwać wykonywanie instrukcji Long Code w wielu wierszach przy użyciu znaku podkreślenia `_` (który w Visual Basic jest nazywany *znakiem kontynuacji*) po każdym wierszu kodu. Aby przerwać instrukcję do następnego wiersza, na końcu wiersza Dodaj spację, a następnie znak kontynuacji. Kontynuuj wykonywanie instrukcji w następnym wierszu. Można zawijać instrukcje na tyle wierszy, ile potrzebujesz, aby zwiększyć czytelność. Następujące instrukcje są takie same:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Nie można jednak otoczyć wiersza w środku literału ciągu. Poniższy przykład nie działa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Aby połączyć długi ciąg, który otacza wiele wierszy, takich jak powyższy kod, należy użyć *operatora łączenia* (`&`), który będzie widoczny w dalszej części tego artykułu.

### <a name="code-comments"></a>Komentarze do kodu

Komentarze umożliwiają wystawianie notatek dla siebie lub dla innych osób. Komentarze składnia Razor są poprzedzone `@*` i kończyć się `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

W blokach kodu można użyć komentarzy składnia Razor lub użyć zwykłego znaku Visual Basic komentarza, który jest pojedynczym cudzysłowem (`'`) poprzedzonym każdym wierszem.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Zmienne

Zmienna to nazwany obiekt, który służy do przechowywania danych. Można nazwać zmienne, ale nazwa musi zaczynać się od litery i nie może zawierać spacji ani znaków zarezerwowanych. W Visual Basic, jak pokazano wcześniej, przypadek liter w nazwie zmiennej nie ma znaczenia.

### <a name="variables-and-data-types"></a>Zmienne i typy danych

Zmienna może mieć określony typ danych, co wskazuje, jakiego rodzaju dane są przechowywane w zmiennej. Można mieć zmienne ciągów, które przechowują wartości ciągu (takie jak &quot;Hello World&quot;), zmienne typu Integer, które przechowują wartości całkowite (takie jak 3 lub 79) i zmienne dat, które przechowują wartości daty w różnych formatach (na przykład 4/12/2012 lub marzec 2009). Istnieje wiele innych typów danych, których można użyć.

Nie trzeba jednak określać typu dla zmiennej. W większości przypadków ASP.NET może ustalić typ na podstawie sposobu użycia danych w zmiennej. (Czasami musisz określić typ; zobaczysz przykłady, w których to prawda).

Aby zadeklarować zmienną bez określenia typu, użyj `Dim` i nazwy zmiennej (na przykład `Dim myVar`). Aby zadeklarować zmienną typu, użyj `Dim` plusa nazwy zmiennej, a następnie `As` a następnie nazwę typu (na przykład `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Poniższy przykład pokazuje niektóre wyrażenia wbudowane, które używają zmiennych na stronie sieci Web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Wynik wyświetlany w przeglądarce:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konwertowanie i testowanie typów danych

Chociaż ASP.NET zazwyczaj zwykle określa typ danych, czasami nie jest to możliwe. W związku z tym może być konieczne ASP.NET przez wykonanie jawnej konwersji. Nawet jeśli nie musisz konwertować typów, czasami warto przetestować, aby zobaczyć, jakiego typu dane mogą pracować.

Najczęstszym przypadkiem jest to, że trzeba przekonwertować ciąg na inny typ, na przykład na liczbę całkowitą lub datę. W poniższym przykładzie pokazano typowy przypadek, w którym należy przekonwertować ciąg na liczbę.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Zgodnie z regułą dane wprowadzane przez użytkownika są uznawane za ciągi. Nawet jeśli użytkownik otrzyma monit o wprowadzenie numeru, a nawet jeśli podano cyfrę, gdy dane wejściowe użytkownika są przesyłane i odczytywane w kodzie, dane są w formacie ciągu. W związku z tym należy przekonwertować ciąg na liczbę. W przykładzie, jeśli spróbujesz wykonać operacje arytmetyczne na wartościach bez ich konwersji, następujące wyniki błędu, ponieważ ASP.NET nie mogą dodać dwóch ciągów:

`Cannot implicitly convert type 'string' to 'int'.`

Aby przekonwertować wartości na liczby całkowite, należy wywołać metodę `AsInt`. Jeśli konwersja zakończy się pomyślnie, można dodać liczby.

W poniższej tabeli wymieniono niektóre typowe metody konwersji i testowania zmiennych.

:::row:::
    :::column:::
        <strong>Metoda</strong>
    :::column-end:::
    :::column:::
        <strong>Opis</strong>
    :::column-end:::
    :::column:::
        <strong>Przykład</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Konwertuje ciąg, który reprezentuje liczbę całkowitą (na przykład &quot;593&quot;) do liczby całkowitej.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Konwertuje ciąg, taki jak &quot;true&quot; lub &quot;false&quot; na typ Boolean.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Konwertuje ciąg, który ma wartość dziesiętną, taką jak &quot;1,3&quot; lub &quot;7,439&quot; do liczby zmiennoprzecinkowej.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Konwertuje ciąg, który ma wartość dziesiętną, taką jak &quot;1,3&quot; lub &quot;7,439&quot; do liczby dziesiętnej. (W ASP.NET liczba dziesiętna jest bardziej precyzyjna niż liczba zmiennoprzecinkowa).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Konwertuje ciąg, który reprezentuje wartość daty i godziny dla typu `DateTime` ASP.NET.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Konwertuje wszystkie inne typy danych na ciąg.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatory

Operator jest słowem kluczowym lub znakiem, który informuje ASP.NET jakiego rodzaju polecenia wykonać w wyrażeniu. Visual Basic obsługuje wiele operatorów, ale wystarczy tylko rozpoznać kilka, aby rozpocząć opracowywanie stron sieci Web ASP.NET. Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.

:::row:::
    :::column:::
        <strong>Zakład</strong>
    :::column-end:::
    :::column:::
        <strong>Opis</strong>
    :::column-end:::
    :::column:::
        <strong>Przykłady</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Operatory matematyczne używane w wyrażeniach liczbowych.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Przypisanie i równość. W zależności od kontekstu przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie lub sprawdza wartości pod kątem równości.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Nierówności. Zwraca `True`, jeśli wartości nie są równe.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Mniejsze niż, większe niż, mniejsze niż lub równe i większe niż lub równe.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Łączenie, które jest używane do dołączania ciągów.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Operatory zwiększania i zmniejszania, które dodają i odejmujeją 1 (odpowiednio) od zmiennej.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Kropka. Używane do rozróżniania obiektów i ich właściwości i metod.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Nawiasów. Służy do grupowania wyrażeń, przekazywania parametrów do metod i uzyskiwania dostępu do elementów członkowskich tablic i kolekcji.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Niemożliwe. Odwraca wartość true na false i odwrotnie. Zwykle używany jako skrócony sposób testowania dla `False` (to oznacza, że nie `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        Logiczne i i lub, które są używane do łączenia warunków.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a>Praca z ścieżkami plików i folderów w kodzie

Często pracujesz ze ścieżkami plików i folderów w kodzie. Oto przykład struktury folderów fizycznych dla witryny sieci Web, która może pojawić się na komputerze deweloperskim:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Poniżej przedstawiono niektóre istotne informacje dotyczące adresów URL i ścieżek:

- Adres URL rozpoczyna się od nazwy domeny (`http://www.example.com`) lub nazwy serwera (`http://localhost`, `http://mycomputer`).
- Adres URL odpowiada ścieżce fizycznej na komputerze-hoście. Na przykład `http://myserver` mogą odpowiadać *C:\websites\mywebsite* folderu na serwerze.
- Ścieżka wirtualna jest skrótem do reprezentowania ścieżek w kodzie bez konieczności określania pełnej ścieżki. Zawiera część adresu URL, która następuje po nazwie domeny lub serwera. W przypadku używania ścieżek wirtualnych można przenieść kod do innej domeny lub serwera bez konieczności aktualizacji ścieżek.

Oto przykład, który pomoże zrozumieć różnice:

| Pełny adres URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nazwa serwera | *mycompanyserver* |
| Ścieżka wirtualna | */humanresources/CompanyPolicy.htm* |
| Ścieżka fizyczna | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Wirtualny katalog główny jest/, podobnie jak katalog główny dysku C:. (Ścieżki folderów wirtualnych zawsze używają ukośników). Ścieżka wirtualna folderu nie musi mieć takiej samej nazwy jak folder fizyczny; może to być alias. (Na serwerach produkcyjnych ścieżka wirtualna rzadko jest zgodna z dokładną ścieżką fizyczną).

Podczas pracy z plikami i folderami w kodzie czasami trzeba odwoływać się do ścieżki fizycznej i czasami ścieżki wirtualnej, w zależności od obiektów, z którymi pracujesz. ASP.NET udostępnia następujące narzędzia do pracy z ścieżkami plików i folderów w kodzie: Metoda `Server.MapPath` i operator `~` i Metoda `Href`.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Konwertowanie wirtualnej na ścieżki fizyczne: Metoda Server. MapPath

Metoda `Server.MapPath` Konwertuje ścieżkę wirtualną (na przykład */default.cshtml*) na bezwzględną ścieżkę fizyczną (na przykład *C:\WebSites\MyWebSiteFolder\default.cshtml*). Tej metody należy używać w dowolnym momencie, gdy potrzebna jest kompletna ścieżka fizyczna. Typowym przykładem jest odczytywanie lub zapisywanie pliku tekstowego lub pliku obrazu na serwerze sieci Web.

Zwykle nie jest znana absolutna ścieżka fizyczna witryny na serwerze lokacji hostingu, dlatego ta metoda może przekonwertować ścieżkę, którą znasz — ścieżkę wirtualną — do odpowiedniej ścieżki na serwerze. Ścieżka wirtualna do pliku lub folderu zostanie przekazana do metody i zwraca ścieżkę fizyczną:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odwoływanie się do wirtualnego katalogu głównego: Metoda ~ operatora i href

W pliku *. cshtml* lub *. vbhtml* można odwoływać się do wirtualnej ścieżki katalogu głównego przy użyciu operatora `~`. Jest to bardzo przydatne, ponieważ można przenieść strony w obrębie witryny, a wszystkie linki, które zawierają do innych stron, nie zostaną zerwane. Jest on również przydatny w przypadku przenoszenia witryny sieci Web do innej lokalizacji. Oto kilka przykładów:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Jeśli witryna sieci Web jest `http://myserver/myapp`, Oto, jak ASP.NET będzie traktować te ścieżki podczas uruchamiania strony:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet`: `http://myserver/myapp/styles/Stylesheet.css`

(Te ścieżki nie będą widoczne w rzeczywistości jako wartości zmiennej, ale ASP.NET będą traktować ścieżki tak, jakby były.)

Operatora `~` można użyć zarówno w kodzie serwera (jak powyżej), jak to:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

W znacznikach można użyć operatora `~`, aby tworzyć ścieżki do zasobów, takich jak pliki obrazów, inne strony sieci Web i pliki CSS. Po uruchomieniu strony ASP.NET przegląda stronę (kod i znacznik) i rozwiązuje wszystkie `~` odwołania do odpowiedniej ścieżki.

## <a name="conditional-logic-and-loops"></a>Logika warunkowa i pętle

Kod serwera ASP.NET umożliwia wykonywanie zadań w oparciu o warunki i pisanie kodu, który powtarza instrukcje, która pociąga za siebie określoną liczbę razy, kod, który uruchamia pętlę.

### <a name="testing-conditions"></a>Warunki testowania

Aby przetestować prosty warunek, użyj instrukcji `If...Then`, która zwraca `True` lub `False` w oparciu o określony test:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Słowo kluczowe `If` uruchamia blok. Rzeczywisty test (warunek) następuje po słowie kluczowym `If` i zwraca wartość true lub false. Instrukcja `If` zostanie zakończona z `Then`. Instrukcje, które będą uruchamiane, jeśli test ma wartość true, są ujęte w `If` i `End If`. Instrukcja `If` może zawierać blok `Else`, który określa instrukcje do uruchomienia, jeśli warunek ma wartość false:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Jeśli instrukcja `If` uruchamia blok kodu, nie trzeba używać zwykłych instrukcji `Code...End Code` w celu uwzględnienia bloków. Możesz po prostu dodać `@` do bloku i będzie on działać. Takie podejście współdziała z `If`, a także innymi Visual Basic programistycznymi słowami kluczowymi, które są umieszczane w blokach kodu, takich jak `For`, `For Each`, `Do While`itd.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Można dodać wiele warunków przy użyciu jednego lub kilku bloków `ElseIf`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

W tym przykładzie, jeśli pierwszy warunek w bloku `If` nie ma wartości true, sprawdzana jest `ElseIf` warunek. Jeśli ten warunek jest spełniony, instrukcje w bloku `ElseIf` są wykonywane. Jeśli żaden z warunków nie zostanie spełniony, instrukcje w bloku `Else` są wykonywane. Można dodać dowolną liczbę bloków `ElseIf`, a następnie zamknąć z blokiem `Else`, podobnie jak w przypadku &quot;wszystko inne&quot; warunek.

Aby przetestować dużą liczbę warunków, użyj bloku `Select Case`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Wartość do przetestowania jest w nawiasach (w przykładzie zmienna Weekday). Każdy test używa instrukcji `Case`, która wyświetla wartość. Jeśli wartość instrukcji `Case` pasuje do wartości testowej, wykonywany jest kod w tym `Case` bloku.

Wynik ostatnich dwóch bloków warunkowych wyświetlanych w przeglądarce:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Kod zapętlenia

Często trzeba wielokrotnie uruchamiać te same instrukcje. Można to zrobić przez zapętlenie. Na przykład często uruchamiasz te same instrukcje dla każdego elementu w kolekcji danych. Jeśli wiesz dokładnie, ile razy chcesz wykonać pętlę, możesz użyć pętli `For`. Ten rodzaj pętli jest szczególnie przydatny do zliczania w górę lub w dół:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Pętla zaczyna się od słowa kluczowego `For`, a następnie trzech elementów:

- Bezpośrednio po instrukcji `For` deklaruje zmienną licznika (nie trzeba używać `Dim`), a następnie wskazuje zakres, jak w `i = 10 to 20`. Oznacza to, że zmienna `i` rozpocznie zliczanie w wysokości 10 i kontynuuje do momentu osiągnięcia 20 (włącznie).
- Między instrukcjami `For` i `Next` jest zawartość bloku. Może zawierać co najmniej jedną instrukcję Code, która jest wykonywana z każdą pętlą.
- Instrukcja `Next i` zatrzymuje pętlę. Zwiększa licznik i uruchamia następną iterację pętli.

Wiersz kodu między `For` i `Next` wierszy zawiera kod, który jest uruchamiany dla każdej iteracji pętli. Znacznik tworzy nowy akapit (`<p>` elementu) za każdym razem i dodaje linię do danych wyjściowych, wyświetlając wartość i (licznik). Po uruchomieniu tej strony przykład tworzy 11 wierszy wyświetlających dane wyjściowe, a tekst w każdym wierszu wskazujący numer elementu.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Jeśli pracujesz z kolekcją lub tablicą, często używasz pętli `For Each`. Kolekcja jest grupą podobnych obiektów, a pętla `For Each` umożliwia wykonywanie zadania na każdym elemencie w kolekcji. Pętla tego typu jest wygodna dla kolekcji, ponieważ w przeciwieństwie do pętli `For` nie trzeba zwiększać licznika ani ustawiać limitu. Zamiast tego kod pętli `For Each` po prostu przechodzi przez kolekcję do momentu zakończenia.

Ten przykład zwraca elementy z kolekcji `Request.ServerVariables` (która zawiera informacje o serwerze sieci Web). Używa pętli `For Each`, aby wyświetlić nazwę każdego elementu przez utworzenie nowego elementu `<li>` na liście punktowanej HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Po słowie kluczowym `For Each` następuje zmienna, która reprezentuje pojedynczy element w kolekcji (w przykładzie `myItem`), po którym następuje słowo kluczowe `In`, a następnie kolekcja, w której ma się znajdować pętla. W treści pętli `For Each` można uzyskać dostęp do bieżącego elementu przy użyciu zmiennej, która została zadeklarowana wcześniej.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Aby utworzyć więcej pętli ogólnego przeznaczenia, użyj instrukcji `Do While`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Ta pętla rozpoczyna się od słowa kluczowego `Do While`, po którym następuje warunek, po którym następuje blok, który ma zostać powtórzony. Pętle zwykle zwiększają się (Dodaj do) lub zmniejszają (odejmowanie od) zmiennej lub obiektu używanej do zliczania. W przykładzie operator `+=` dodaje 1 do wartości zmiennej za każdym razem, gdy pętla zostanie uruchomiona. (Aby zmniejszyć zmienną w pętli, która liczy w dół, użyj operatora zmniejszania `-=`).

## <a name="objects-and-collections"></a>Obiekty i kolekcje

Niemal wszystko w witrynie ASP.NET Web to obiekt, w tym sama strona sieci Web. W tej sekcji omówiono niektóre ważne obiekty, które często działają w kodzie.

### <a name="page-objects"></a>Obiekty strony

Najbardziej podstawowym obiektem w ASP.NET jest strona. Można uzyskać dostęp do właściwości obiektu strony bezpośrednio bez żadnego obiektu kwalifikującego. Poniższy kod pobiera ścieżkę pliku strony przy użyciu obiektu `Request` strony:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Właściwości obiektu `Page` można użyć, aby uzyskać wiele informacji, takich jak:

- `Request`. Jak już widzisz, jest to zbiór informacji dotyczących bieżącego żądania, w tym typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd.
- `Response`. Jest to zbiór informacji o odpowiedzi (stronie), które zostaną wysłane do przeglądarki po zakończeniu działania kodu serwera. Na przykład można użyć tej właściwości do zapisu informacji w odpowiedzi.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Obiekty kolekcji (tablice i słowniki)

Kolekcja jest grupą obiektów tego samego typu, na przykład z kolekcją `Customer` obiektów z bazy danych. ASP.NET zawiera wiele wbudowanych kolekcji, takich jak kolekcja `Request.Files`.

Często pracujesz z danymi w kolekcjach. Dwa popularne typy kolekcji to *Tablica* i *słownik*. Tablica jest przydatna, gdy chcesz przechowywać kolekcję podobnych elementów, ale nie chcesz tworzyć oddzielnej zmiennej do przechowywania poszczególnych elementów:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Z tablicami deklaruje się konkretny typ danych, taki jak `String`, `Integer`lub `DateTime`. Aby wskazać, że zmienna może zawierać tablicę, należy dodać nawiasy do nazwy zmiennej w deklaracji (takiej jak `Dim myVar() As String`). Dostęp do elementów w tablicy można uzyskać przy użyciu ich położenia (indeksu) lub instrukcji `For Each`. Indeksy tablicy są oparte na &#8212; zero oznaczające, że pierwszy element znajduje się na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Możesz określić liczbę elementów w tablicy, pobierając jej Właściwość `Length`. Aby uzyskać położenie określonego elementu w tablicy (to oznacza, aby przeszukać tablicę), użyj metody `Array.IndexOf`. Można również wykonywać operacje, takie jak odwracanie zawartości tablicy (Metoda `Array.Reverse`) lub sortowanie zawartości (Metoda `Array.Sort`).

Dane wyjściowe kodu tablicy ciągów wyświetlanego w przeglądarce:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Słownik jest kolekcją par klucz/wartość, w których podano klucz (lub nazwę), aby ustawić lub pobrać odpowiednią wartość:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Aby utworzyć słownik, użyj słowa kluczowego `New`, aby wskazać, że tworzysz nowy obiekt `Dictionary`. Można przypisać słownik do zmiennej za pomocą słowa kluczowego `Dim`. Możesz wskazać typy danych elementów w słowniku przy użyciu nawiasów (`( )`). Na końcu deklaracji należy dodać kolejną parę nawiasów, ponieważ jest to metoda, która tworzy nowy słownik.

Aby dodać elementy do słownika, można wywołać metodę `Add` zmiennej słownika (`myScores` w tym przypadku), a następnie określić klucz i wartość. Alternatywnie można użyć nawiasów, aby wskazać klucz i wykonać proste przypisanie, jak w poniższym przykładzie:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Wywoływanie metod z parametrami

Jak przedstawiono wcześniej w tym artykule, obiekty, z którymi program ma metody. Na przykład obiekt `Database` może mieć metodę `Database.Connect`. Wiele metod ma także jeden lub więcej parametrów. *Parametr* jest wartością, którą można przekazać do metody, aby umożliwić wykonywanie jej zadania. Na przykład Przyjrzyj się deklaracji dla metody `Request.MapPath`, która przyjmuje trzy parametry:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Ta metoda zwraca ścieżkę fizyczną na serwerze, który odpowiada określonej ścieżce wirtualnej. Trzy parametry dla metody to `virtualPath`, `baseVirtualDir`i `allowCrossAppMapping`. (Należy zauważyć, że w deklaracji parametry są wyświetlane z danymi o typach danych, które będą akceptowane). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.

Gdy używasz Visual Basic z składnia Razor, masz dwie opcje przekazywania parametrów do metody: *parametry pozycyjne* lub *nazwane parametry*. Aby wywołać metodę przy użyciu parametrów pozycyjnych, należy przekazać parametry w ścisłej kolejności, która jest określona w deklaracji metody. (Zazwyczaj znana jest ta kolejność, odczytując dokumentację dla metody). Należy postępować zgodnie z kolejnością, a w razie potrzeby nie można &#8212; pominąć żadnego z parametrów, należy przekazać pusty ciąg (`""`) lub null dla parametru pozycyjnego, dla którego nie ma wartości.

W poniższym przykładzie przyjęto założenie, że w witrynie sieci Web znajduje się folder o nazwie *skrypty* . Kod wywołuje metodę `Request.MapPath` i przekazuje wartości dla trzech parametrów w poprawnej kolejności. Następnie zostanie wyświetlona zamapowana ścieżka.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Jeśli istnieje wiele parametrów dla metody, można zachować przejrzystość kodu i bardziej czytelność przy użyciu nazwanych parametrów. Aby wywołać metodę przy użyciu nazwanych parametrów, należy określić nazwę parametru, po którym następuje `:=` a następnie podać wartość. Zaletą parametrów nazwanych jest możliwość dodawania ich w dowolnej kolejności. (Wadą jest to, że wywołanie metody nie jest jako kompaktowe).

Poniższy przykład wywołuje tę samą metodę, jak powyżej, ale używa nazwanych parametrów do dostarczenia wartości:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Jak widać, parametry są przesyłane w innej kolejności. Jeśli jednak zostanie uruchomiony poprzedni przykład i ten przykład zwróci tę samą wartość.

## <a name="handling-errors"></a>Obsługa błędów

### <a name="try-catch-statements"></a>Instrukcje try-catch

Często występują instrukcje w kodzie, które mogą zakończyć się niepowodzeniem z przyczyn poza formantem. Na przykład:

- Jeśli kod próbuje otworzyć, utworzyć, odczytać lub zapisać plik, mogą wystąpić wszystkie rodzaje błędów. Plik, który nie istnieje, może być zablokowany, kod może nie mieć uprawnień i tak dalej.
- Podobnie, jeśli kod próbuje zaktualizować rekordy w bazie danych, mogą wystąpić problemy z uprawnieniami, połączenie z bazą danych może być porzucone, dane do zapisania mogą być nieprawidłowe i tak dalej.

W warunkach programowania te sytuacje nazywają się *wyjątkami*. Jeśli Twój kod napotyka wyjątek, generuje (zgłasza) komunikat o błędzie, który jest, co jest najlepszym rozwiązaniem dla użytkowników.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

W sytuacjach, w których kod może napotkać wyjątki i aby uniknąć komunikatów o błędach tego typu, można użyć instrukcji `Try/Catch`. W instrukcji `Try` uruchamiasz sprawdzany kod. W co najmniej jednej instrukcji `Catch` można wyszukać konkretne błędy (określone typy wyjątków), które mogły wystąpić. Możesz dołączyć dowolną liczbę instrukcji `Catch`, ile potrzebujesz, aby wyszukać błędy, które są przewidywane.

> [!NOTE]
> Zalecamy uniknięcie użycia metody `Response.Redirect` w instrukcjach `Try/Catch`, ponieważ może to spowodować wyjątek na stronie.

Poniższy przykład pokazuje stronę, która tworzy plik tekstowy przy pierwszym żądaniu, a następnie wyświetla przycisk, który umożliwia użytkownikowi otwarcie pliku. Przykład celowo używa nieprawidłowej nazwy pliku, aby powodował wyjątek. Kod zawiera instrukcje `Catch` dla dwóch możliwych wyjątków: `FileNotFoundException`, które występuje, jeśli nazwa pliku jest zła, i `DirectoryNotFoundException`, który występuje, jeśli ASP.NET nie może odnaleźć folderu. (Możesz usunąć komentarz z instrukcji w przykładzie, aby zobaczyć, jak działa, gdy wszystko działa prawidłowo).

Jeśli kod nie obsłużył wyjątku, zobaczysz stronę błędu, jak poprzedni zrzut ekranu. Jednak sekcja `Try/Catch` pozwala zapobiec wyświetlaniu tych typów błędów przez użytkownika.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

### <a name="reference-documentation"></a>Dokumentacja referencyjna

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Język Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)

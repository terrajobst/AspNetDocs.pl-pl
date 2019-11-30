---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składniC#Razor () | Microsoft Docs
author: Rick-Anderson
description: Ten rozdział zawiera omówienie programowania za pomocą ASP.NET stron sieci Web przy użyciu składnia Razor. ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznego sieci Web PA...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2019
ms.locfileid: "74564883"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składniC#Razor ()

Autor [FitzMacken](https://github.com/tfitzmac)

> Ten artykuł zawiera omówienie programowania za pomocą ASP.NET stron sieci Web przy użyciu składnia Razor. ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznych stron sieci Web na serwerach sieci Web. Ten artykuł koncentruje się na C# korzystaniu z języka programowania.
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

## <a name="the-top-8-programming-tips"></a>8 najważniejszych wskazówek dotyczących programowania

W tej sekcji przedstawiono kilka wskazówek, które należy znać podczas pisania kodu serwera ASP.NET przy użyciu składnia Razor.

> [!NOTE]
> Składnia Razor jest oparta na języku C# programowania i jest to język, który jest najczęściej używany w przypadku stron sieci Web ASP.NET. Jednak składnia Razor obsługuje również język Visual Basic i wszystko, co widzisz, można również w Visual Basic. Aby uzyskać szczegółowe informacje, zobacz dodatek [Visual Basic języka i składni](https://go.microsoft.com/fwlink/?LinkId=202908).

Więcej informacji o większości tych technik programowania można znaleźć w dalszej części artykułu.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Dodaj kod do strony przy użyciu znaku @

Znak `@` uruchamia wyrażenia śródwierszowe, bloki pojedynczej instrukcji i bloki zawierające wiele instrukcji:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Te instrukcje wyglądają tak, jak w przypadku uruchamiania strony w przeglądarce:

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Kodowanie HTML**
> 
> Gdy wyświetlasz zawartość na stronie przy użyciu znaku `@`, jak w powyższych przykładach, ASP.NET HTML — koduje dane wyjściowe. Zastępuje to zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) kodami, które umożliwiają wyświetlanie znaków jako znaków na stronie sieci Web zamiast interpretowania ich jako tagów HTML lub jednostek. Bez kodowania HTML dane wyjściowe z kodu serwera mogą nie być wyświetlane prawidłowo i mogą uwidaczniać zagrożenie bezpieczeństwa.
> 
> Jeśli celem jest wyprowadzanie kodu HTML, który renderuje Tagi jako znaczniki (na przykład `<p></p>` akapitu lub `<em></em>` w celu wyróżnienia tekstu), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu w](#BM_CombiningTextMarkupAndCode) dalszej części tego artykułu.
> 
> Więcej informacji na temat kodowania HTML można znaleźć w artykule [Praca z formularzami](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Umieść bloki kodu w nawiasach klamrowych

*Blok kodu* zawiera jedną lub więcej instrukcji kodu i jest ujęty w nawiasy klamrowe.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Wynik wyświetlany w przeglądarce:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. wewnątrz bloku każda instrukcja Code kończy się średnikiem

W bloku kodu Każda pełna instrukcja Code musi kończyć się średnikiem. Wyrażenia śródwierszowe nie kończą się średnikiem.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Użyj zmiennych do przechowywania wartości

Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty itp. Tworzysz nową zmienną za pomocą słowa kluczowego `var`. Można wstawiać wartości zmiennych bezpośrednio na stronie przy użyciu `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Wynik wyświetlany w przeglądarce:

![Razor-IMG3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. w podwójnym cudzysłowie należy umieścić wartości ciągu literału

*Ciąg* jest sekwencją znaków, które są traktowane jako tekst. Aby określić ciąg, należy umieścić go w podwójnym cudzysłowie:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Jeśli ciąg, który ma być wyświetlany, zawiera znak ukośnika odwrotnego (`\`) lub podwójny cudzysłów (`"`), użyj *literału ciągu Verbatim* , który jest poprzedzony operatorem `@`. (W C#, znak \ ma specjalne znaczenie, chyba że używasz literału ciągu Verbatim).

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Aby osadzić podwójne cudzysłowy, użyj literału ciągu Verbatim i powtórz cudzysłowy:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Oto wynik użycia obu przykładów na stronie:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Należy zauważyć, że znak `@` jest używany zarówno do oznaczania literałów ciągu C# Verbatim w i do oznaczania kodu na stronach ASP.NET.

### <a name="6-code-is-case-sensitive"></a>6. kod uwzględnia wielkość liter

W C#programie słowa kluczowe (takie jak `var`, `true`i `if`) i nazwy zmiennych uwzględniają wielkość liter. Poniższe wiersze kodu tworzą dwie różne zmienne, `lastName` i `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

W przypadku deklarowania zmiennej jako `var lastName = "Smith";` i próby odwołującej się do tej zmiennej na stronie jako `@LastName`, otrzymasz wartość `"Jones"` zamiast `"Smith"`.

> [!NOTE]
> W Visual Basic słowa kluczowe i zmienne *nie* uwzględniają wielkości liter.

### <a name="7-much-of-your-coding-involves-objects"></a>7. większość kodowania obejmuje obiekty

*Obiekt* reprezentuje element, który można zaprogramować ze &#8212; stroną, polem tekstowym, plikiem, obrazem, żądaniem sieci Web, wiadomością e-mail, rekordem klienta (wierszem bazy danych) itd. Obiekty mają właściwości opisujące ich cechy, które można odczytać lub zmienić &#8212; obiekt pola tekstowego ma właściwość `Text` (między innymi), obiekt żądania ma właściwość `Url`, wiadomość e-mail ma właściwość `From`, a obiekt klienta ma właściwość `FirstName`. Obiekty mają również metody, które są zleceniami &quot;,&quot; mogą być wykonywane. Przykłady obejmują metodę `Save` obiektu pliku, metodę `Rotate` obiektu obrazu oraz metodę `Send` obiektu poczty e-mail.

Często pracujesz z obiektem `Request`, który zawiera informacje, takie jak wartości pól tekstowych (pól formularza) na stronie, typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd. Poniższy przykład pokazuje, jak uzyskać dostęp do właściwości obiektu `Request` i jak wywołać metodę `MapPath` obiektu `Request`, który zapewnia ścieżkę bezwzględną strony na serwerze:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Wynik wyświetlany w przeglądarce:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. możesz napisać kod, który podejmuje decyzje

Kluczową cechą dynamicznych stron sieci Web jest określenie, jakie czynności należy wykonać na podstawie warunków. Najbardziej typowym sposobem wykonania tej czynności jest wykonanie instrukcji `if` (i opcjonalnej instrukcji `else`).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Instrukcja `if(IsPost)` to skrócony sposób pisania `if(IsPost == true)`. Wraz z `if` instrukcjami istnieją różne sposoby testowania warunków, powtarzania bloków kodu i tak dalej, które opisano w dalszej części tego artykułu.

Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **Prześlij**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>Metody GET i POST protokołu HTTP oraz Właściwość ispost
> 
> Protokół używany przez strony sieci Web (HTTP) obsługuje bardzo ograniczoną liczbę metod (czasowników), które są używane do żądania do serwera. Dwa Najczęstsze są pobieranie, które są używane do odczytywania strony i wpisu, które są używane do przesyłania strony. Na ogół podczas pierwszego żądania strony użytkownik żąda strony przy użyciu polecenia GET. Jeśli użytkownik wypełni formularz, a następnie kliknie przycisk Prześlij, przeglądarka wyśle żądanie POST na serwer.
> 
> W programowaniu sieci Web często warto wiedzieć, czy strona jest żądana jako GET, czy jako wpis, aby poznać sposób przetwarzania strony. Na stronach sieci Web ASP.NET można użyć właściwości `IsPost`, aby sprawdzić, czy żądanie jest GET lub POST. Jeśli żądanie jest WPISem, właściwość `IsPost` zwróci wartość true i można wykonać czynności, takie jak odczytywanie wartości pól tekstowych w formularzu. Wiele przykładów zobaczysz, jak przetwarzać stronę w różny sposób w zależności od wartości `IsPost`.

## <a name="a-simple-code-example"></a>Prosty przykład kodu

Ta procedura pokazuje, jak utworzyć stronę przedstawiającą podstawowe techniki programowania. W tym przykładzie utworzysz stronę umożliwiającą użytkownikom wprowadzanie dwóch liczb, a następnie ich dodanie i wyświetlenie wyniku.

1. W edytorze Utwórz nowy plik i nadaj mu nazwę *AddNumbers. cshtml*.
2. Skopiuj poniższy kod i adiustację na stronie, zastępując wszystkie elementy znajdujące się już na stronie.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Oto kilka rzeczy, dla których warto zwrócić uwagę:

    - Znak `@` uruchamia pierwszy blok kodu na stronie i poprzedza zmienną `totalMessage`, która jest osadzona w dolnej części strony.
    - Blok w górnej części strony jest ujęty w nawiasy klamrowe.
    - W bloku w górnej części wszystkie wiersze kończą się średnikiem.
    - Zmienne `total`, `num1`, `num2`i `totalMessage` przechowują kilka liczb i ciąg.
    - Wartość ciągu literału przypisana do zmiennej `totalMessage` znajduje się w podwójnym cudzysłowie.
    - Ponieważ w kodzie jest rozróżniana wielkość liter, gdy zmienna `totalMessage` jest używana w dolnej części strony, jej nazwa musi być zgodna ze zmienną u góry.
    - Wyrażenie `num1.AsInt() + num2.AsInt()` pokazuje, jak korzystać z obiektów i metod. Metoda `AsInt` dla każdej zmiennej konwertuje ciąg wprowadzony przez użytkownika na liczbę (liczbę całkowitą), dzięki czemu można wykonywać na nim operacje arytmetyczne.
    - Tag `<form>` zawiera atrybut `method="post"`. Oznacza to, że gdy użytkownik kliknie przycisk **Dodaj**, Strona zostanie wysłana na serwer przy użyciu metody POST protokołu HTTP. Gdy strona zostanie przesłana, test `if(IsPost)` ma wartość true, a kod warunkowy zostanie uruchomiony, wyświetlając wynik dodawania liczb.
3. Zapisz stronę i uruchom ją w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Wprowadź dwie liczby całkowite, a następnie kliknij przycisk **Dodaj** . 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Podstawowe pojęcia związane z programowaniem

Ten artykuł zawiera omówienie programowania w sieci Web ASP.NET. Nie jest to wyczerpujące badanie, a jedynie krótki przewodnik po pojęciach związanych z programowaniem, których najczęściej używasz. Nawet w ten sposób obejmuje niemal wszystko, czego potrzebujesz, aby rozpocząć pracę ze stronami sieci Web ASP.NET.

Jednak pierwsze i małe tło techniczne.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Składnia Razor, kod serwera i ASP.NET

Składnia Razor jest prostą składnią programowania dla osadzania kodu opartego na serwerze na stronie sieci Web. Na stronie sieci Web, która używa składnia Razor, istnieją dwa rodzaje zawartości: zawartość klienta i kod serwera. Zawartość klienta to informacje, które są używane na stronach sieci Web: oznakowanie HTML (elementy), takie jak CSS, może być pewnym skryptem, takim jak JavaScript, i zwykłym tekstem.

Składnia Razor umożliwia dodanie kodu serwera do tej zawartości klienta. Jeśli na stronie znajduje się kod serwera, serwer uruchamia najpierw ten kod przed wysłaniem strony do przeglądarki. Uruchamiając na serwerze, kod może wykonywać zadania, które mogą być znacznie bardziej skomplikowane do korzystania z samej zawartości klienta, takich jak uzyskiwanie dostępu do baz danych opartych na serwerze. Co najważniejsze, kod serwera może dynamicznie tworzyć zawartość &#8212; klienta, która może generować znaczniki HTML lub inną zawartość na bieżąco, a następnie wysyłać ją do przeglądarki wraz z dowolnym STATYCZNYM kodem HTML, który może zawierać strona. W perspektywie przeglądarki zawartość klienta, która jest generowana przez kod serwera, nie jest inna niż jakakolwiek inna zawartość klienta. Jak już widzisz, kod serwera, który jest wymagany, jest bardzo prosty.

ASP.NET strony sieci Web zawierające składnia Razor mają specjalne rozszerzenie pliku ( *. cshtml* lub *. vbhtml*). Serwer rozpoznaje te rozszerzenia, uruchamia kod, który jest oznaczony za pomocą składnia Razor, a następnie wysyła stronę do przeglądarki.

### <a name="where-does-aspnet-fit-in"></a>Gdzie mieści się ASP.NET?

Składnia Razor jest oparta na technologii firmy Microsoft o nazwie ASP.NET, która z kolei opiera się na platformie Microsoft .NET. The.NET Framework to obszerna, kompleksowa platforma programistyczna firmy Microsoft do tworzenia praktycznie dowolnego typu aplikacji komputerowej. ASP.NET jest częścią .NET Framework, która jest przeznaczona specjalnie do tworzenia aplikacji sieci Web. Deweloperzy korzystali z ASP.NET, aby tworzyć wiele witryn sieci Web o największych i najwyższym ruchu na świecie. (W dowolnym momencie zobaczysz nazwę pliku Extension *. aspx* jako część adresu URL w witrynie, zobaczysz, że witryna została zapisywana przy użyciu ASP.NET).

Składnia Razor zapewnia wszystkie możliwości ASP.NET, ale przy użyciu uproszczonej składni, która jest łatwiejsza do uczenia się, jeśli jesteś początkującym użytkownikiem, który zwiększa produktywność, jeśli jesteś ekspertem. Mimo że ta składnia jest prosta do użycia, jej związek z rodziną ASP.NET i .NET Framework oznacza, że w miarę jak witryny sieci Web stają się bardziej zaawansowane, możesz uzyskać dostęp do większych platform.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Klasy i wystąpienia**
> 
> Kod serwera ASP.NET korzysta z obiektów, które są oparte na koncepcji klas. Klasa jest definicją lub szablonem obiektu. Na przykład aplikacja może zawierać klasę `Customer`, która definiuje właściwości i metody wymagane przez każdy obiekt klienta.
> 
> Gdy aplikacja musi pracować z rzeczywistymi informacjami o kliencie, tworzy wystąpienie (lub tworzy *wystąpienia*) obiektu klient. Każdy indywidualny klient jest osobnym wystąpieniem klasy `Customer`. Każde wystąpienie obsługuje te same właściwości i metody, ale wartości właściwości dla każdego wystąpienia są zwykle różne, ponieważ każdy obiekt klienta jest unikatowy. W jednym obiekcie klienta Właściwość `LastName` może mieć wartość "Smith"; w innym obiekcie klienta Właściwość `LastName` może mieć wartość "Nowak".
> 
> Podobnie każda indywidualna Strona sieci Web w swojej witrynie jest obiektem `Page`, który jest wystąpieniem klasy `Page`. Przycisk na stronie jest obiektem `Button`, który jest wystąpieniem klasy `Button` i tak dalej. Każde wystąpienie ma własne cechy, ale wszystkie są oparte na tym, co jest określone w definicji klasy obiektu.

## <a name="basic-syntax"></a>Podstawowa składnia

Wcześniej przedstawiono podstawowy przykład tworzenia strony sieci Web ASP.NET oraz sposób dodawania kodu serwerowego do znacznika HTML. Tutaj znajdziesz podstawowe informacje na temat pisania kodu serwera ASP.NET przy użyciu składnia Razor &#8212; , które są regułami języka programowania.

Jeśli masz doświadczenie z programowaniem (zwłaszcza jeśli użyto języka C, C++, C#, Visual Basic lub JavaScript), większość czytanych tutaj informacji będzie znana. Prawdopodobnie trzeba będzie zaznajomić się tylko z informacjami o sposobie dodawania kodu serwera do znaczników w plikach *. cshtml* .

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Łączenie tekstu, znaczników i kodu w blokach kodu

W blokach kodu serwera często chcesz na stronie wyprowadzać tekst lub znaczniki (lub obie). Jeśli blok kodu serwera zawiera tekst, który nie jest kodem, a zamiast tego powinien być renderowany jako, ASP.NET musi mieć możliwość odróżnienia tego tekstu od kodu. Istnieje kilka sposobów, aby to zrobić.

- Ujmij tekst w element HTML, taki jak `<p></p>` lub `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    Element HTML może zawierać tekst, dodatkowe elementy HTML i wyrażenia kodu serwera. Gdy ASP.NET widzi tag otwierającego HTML (na przykład `<p>`), renderuje wszystko, w tym element i jego zawartość, w postaci, w jakiej jest przeglądarką, rozpoznawania wyrażeń kodu serwera w miarę ich przeszukiwania.
- Użyj operatora `@:` lub elementu `<text>`. `@:` wyprowadza pojedynczy wiersz zawartości zawierający zwykły tekst lub niedopasowane Tagi HTML; element `<text>` obejmuje wiele wierszy do danych wyjściowych. Te opcje są przydatne, gdy nie chcesz renderować elementu HTML jako części danych wyjściowych.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Jeśli chcesz wypróbować wiele wierszy tekstu lub niedopasowane Tagi HTML, możesz poprzedzać każdy wiersz za pomocą `@:`lub umieścić wiersz w `<text>` elemencie. Podobnie jak w przypadku operatora `@:`, Tagi`<text>` są używane przez ASP.NET do identyfikowania zawartości tekstowej i nigdy nie są renderowane w danych wyjściowych strony.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Pierwszy przykład powtarza poprzedni przykład, ale używa pojedynczej pary tagów `<text>`, aby umieścić tekst do renderowania. W drugim przykładzie Tagi `<text>` i `</text>` zawierają trzy wiersze, z których wszystkie mają niezwierany tekst i niedopasowane Tagi HTML (`<br />`), a także kod serwera i dopasowane Tagi HTML. Ponownie można także poprzedzać każdy wiersz pojedynczym operatorem `@:`; w dowolny sposób działa.

    > [!NOTE]
    > Podczas wyprowadzania tekstu, jak pokazano w tej &#8212; sekcji przy użyciu elementu HTML, operatora `@:` lub elementu &#8212; `<text>` ASP.NET nie jest kodowane w języku HTML. (Jak wspomniano wcześniej, ASP.NET koduje dane wyjściowe wyrażeń kodów serwera i bloków kodu serwera, które są poprzedzone `@`, z wyjątkiem przypadków specjalnych zanotowanych w tej sekcji).

### <a name="whitespace"></a>Odstępu

Dodatkowe spacje w instrukcji (i poza literałem ciągu) nie wpływają na instrukcję:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Podział wiersza w instrukcji nie ma wpływu na instrukcję i można zawinąć instrukcje w celu zapewnienia czytelności. Następujące instrukcje są takie same:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Nie można jednak otoczyć wiersza w środku literału ciągu. Poniższy przykład nie działa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Aby połączyć długi ciąg, który otacza wiele wierszy, takich jak powyższy kod, dostępne są dwie opcje. Możesz użyć operatora łączenia (`+`), który będzie widoczny w dalszej części tego artykułu. Można również użyć znaku `@`, aby utworzyć Verbatim literał ciągu, jak zostało to opisane wcześniej w tym artykule. Literały ciągu Verbatim można przerwać w wierszach:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Komentarze kodu (i adiustacji)

Komentarze umożliwiają wystawianie notatek dla siebie lub dla innych osób. Umożliwiają one także wyłączenie (*komentowanie*) sekcji kodu lub znaczników, które nie powinny być uruchamiane, ale które mają być przechowywane na stronie przez czas.

Istnieje inna składnia komentarzy dla kodu Razor i znaczników HTML. Podobnie jak w przypadku wszystkich kodów Razor, komentarze Razor są przetwarzane (a następnie usuwane) na serwerze przed wysłaniem strony do przeglądarki. W związku z tym składnia komentarzy Razor pozwala umieścić komentarze w kodzie (lub nawet w znacznikach), które można zobaczyć podczas edytowania pliku, ale użytkownicy nie widzą nawet w źródle strony.

W przypadku komentarzy Razor ASP.NET należy uruchomić komentarz z `@*` i zakończyć z `*@`. Komentarz może znajdować się w jednym wierszu lub wielu wierszach:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Oto komentarz w bloku kodu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Poniżej znajduje się ten sam blok kodu z wierszem kodu oznaczonym jako komentarz, który nie zostanie uruchomiony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

W bloku kodu, jako alternatywa dla składni komentarzy Razor, można użyć składni komentarza używanego języka programowania, takiego jak C#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

W C#programie Komentarze jednowierszowe są poprzedzone `//` znakami, a Komentarze wielowierszowe zaczynają się od `/*` i kończyć `*/`. (Podobnie jak w przypadku komentarzy C# Razor, komentarze nie są renderowane do przeglądarki).

W przypadku znaczników, jak wiesz, możesz utworzyć komentarz HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Komentarze HTML zaczynają się od `<!--` znaków i kończą się `-->`. Możesz użyć komentarzy HTML do otaczania nietylko tekstu, ale również dowolnego znacznika HTML, który można zachować na stronie, ale nie powinien być renderowany. Ten komentarz HTML spowoduje ukrycie całej zawartości tagów i zawartego w nim tekstu:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

W przeciwieństwie do komentarzy Razor, komentarze HTML *są* renderowane na stronie, a użytkownik może je zobaczyć, wyświetlając Źródło strony.

Razor ma ograniczenia dotyczące zagnieżdżonych bloków C#. Aby uzyskać więcej informacji [, C# Zobacz nazwane zmienne i zagnieżdżone bloki generują przerwany kod](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Zmienne

Zmienna to nazwany obiekt, który służy do przechowywania danych. Można nazwać zmienne, ale nazwa musi zaczynać się od litery i nie może zawierać spacji ani znaków zarezerwowanych.

### <a name="variables-and-data-types"></a>Zmienne i typy danych

Zmienna może mieć określony typ danych, co wskazuje, jakiego rodzaju dane są przechowywane w zmiennej. Można mieć zmienne ciągów, które przechowują wartości ciągu (takie jak &quot;Hello World&quot;), zmienne typu Integer, które przechowują wartości całkowite (takie jak 3 lub 79) i zmienne dat, które przechowują wartości daty w różnych formatach (na przykład 4/12/2012 lub marzec 2009). Istnieje wiele innych typów danych, których można użyć.

Zwykle nie trzeba określać typu dla zmiennej. W większości przypadków ASP.NET może ustalić typ na podstawie sposobu użycia danych w zmiennej. (Czasami musisz określić typ; zobaczysz przykłady, w których to prawda).

Należy zadeklarować zmienną za pomocą słowa kluczowego `var` (jeśli nie chcesz określać typu) lub przy użyciu nazwy typu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

W poniższym przykładzie przedstawiono typowe zastosowania zmiennych na stronie sieci Web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Jeśli połączysz poprzednie przykłady na stronie, zobaczysz ten komunikat w przeglądarce:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konwertowanie i testowanie typów danych

Chociaż ASP.NET zazwyczaj zwykle określa typ danych, czasami nie jest to możliwe. W związku z tym może być konieczne ASP.NET przez wykonanie jawnej konwersji. Nawet jeśli nie musisz konwertować typów, czasami warto przetestować, aby zobaczyć, jakiego typu dane mogą pracować.

Najczęstszym przypadkiem jest to, że trzeba przekonwertować ciąg na inny typ, na przykład na liczbę całkowitą lub datę. W poniższym przykładzie pokazano typowy przypadek, w którym należy przekonwertować ciąg na liczbę.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Zgodnie z regułą dane wprowadzane przez użytkownika są uznawane za ciągi. Nawet w przypadku wyświetlenia monitu o wprowadzenie liczby, a nawet jeśli nie wprowadzono cyfry, gdy dane wejściowe użytkownika są przesyłane i odczytywane w kodzie, dane są w formacie ciągu. W związku z tym należy przekonwertować ciąg na liczbę. W przykładzie, jeśli spróbujesz wykonać operacje arytmetyczne na wartościach bez ich konwersji, następujące wyniki błędu, ponieważ ASP.NET nie mogą dodać dwóch ciągów:

*Nie można niejawnie przekonwertować typu "String" na "int".*

Aby przekonwertować wartości na liczby całkowite, należy wywołać metodę `AsInt`. Jeśli konwersja zakończy się pomyślnie, można dodać liczby.

W poniższej tabeli wymieniono niektóre typowe metody konwersji i testowania zmiennych.

:::row:::
    :::column:::
    <strong>Method</strong>
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
    Konwertuje ciąg reprezentujący liczbę całkowitą (na przykład "593") na liczbę całkowitą.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
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
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatory

Operator jest słowem kluczowym lub znakiem, który informuje ASP.NET jakiego rodzaju polecenia wykonać w wyrażeniu. C# Język (i składnia Razor na jego podstawie) obsługuje wiele operatorów, ale wystarczy tylko rozpoznać kilka, aby rozpocząć. Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.

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
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    Operatory matematyczne używane w wyrażeniach liczbowych.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    Przypisanie. Przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    Kryteri. Zwraca `true`, jeśli wartości są równe. (Zauważ rozróżnienie między operatorem `=` i operatorem `==`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    Nierówności. Zwraca `true`, jeśli wartości nie są równe.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    Mniejsze niż, większe niż, mniejsze niż lub równe, i większe niż lub równe.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    Łączenie, które jest używane do dołączania ciągów. ASP.NET wie różnicę między tym operatorem a operatorem dodawania na podstawie typu danych wyrażenia.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+=``-=`
    :::column-end:::
    :::column:::
    Operatory zwiększania i zmniejszania, które dodają i odejmujeją 1 (odpowiednio) od zmiennej.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    Nawiasów. Używane do grupowania wyrażeń i przekazywania parametrów do metod.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    Nawias. Służy do uzyskiwania dostępu do wartości w tablicach lub kolekcjach.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    Niemożliwe. Odwraca `true` wartość do `false` i na odwrót. Zwykle używany jako skrócony sposób testowania dla `false` (to oznacza, że nie `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&&``||`
    :::column-end:::
    :::column:::
    Logiczne i i lub, które są używane do łączenia warunków.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
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

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odwoływanie się do wirtualnego katalogu głównego: Metoda ~ operatora i href

W pliku *. cshtml* lub *. vbhtml* można odwoływać się do wirtualnej ścieżki katalogu głównego przy użyciu operatora `~`. Jest to bardzo przydatne, ponieważ można przenieść strony w obrębie witryny, a wszystkie linki, które zawierają do innych stron, nie zostaną zerwane. Jest on również przydatny w przypadku przenoszenia witryny sieci Web do innej lokalizacji. Oto kilka przykładów:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Jeśli witryna sieci Web jest `http://myserver/myapp`, Oto, jak ASP.NET będzie traktować te ścieżki podczas uruchamiania strony:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet`: `http://myserver/myapp/styles/Stylesheet.css`

(Te ścieżki nie będą widoczne w rzeczywistości jako wartości zmiennej, ale ASP.NET będą traktować ścieżki tak, jakby były.)

Operatora `~` można użyć zarówno w kodzie serwera (jak powyżej), jak to:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

W znacznikach można użyć operatora `~`, aby tworzyć ścieżki do zasobów, takich jak pliki obrazów, inne strony sieci Web i pliki CSS. Po uruchomieniu strony ASP.NET przegląda stronę (kod i znacznik) i rozwiązuje wszystkie `~` odwołania do odpowiedniej ścieżki.

## <a name="conditional-logic-and-loops"></a>Logika warunkowa i pętle

Kod serwera ASP.NET umożliwia wykonywanie zadań w oparciu o warunki i pisanie kodu, który powtarza instrukcje w konkretnym czasie (czyli kodzie, który uruchamia pętlę).

### <a name="testing-conditions"></a>Warunki testowania

Aby przetestować prosty warunek, użyj instrukcji `if`, która zwraca wartość PRAWDA lub FAŁSZ w oparciu o określony test:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Słowo kluczowe `if` uruchamia blok. Rzeczywisty test (warunek) jest w nawiasach i zwraca wartość PRAWDA lub FAŁSZ. Instrukcje, które są uruchamiane, jeśli test ma wartość true, są ujęte w nawiasy klamrowe. Instrukcja `if` może zawierać blok `else`, który określa instrukcje do uruchomienia, jeśli warunek ma wartość false:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Można dodać wiele warunków przy użyciu bloku `else if`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

W tym przykładzie, jeśli pierwszy warunek w bloku if nie ma wartości true, sprawdzana jest `else if` warunek. Jeśli ten warunek jest spełniony, instrukcje w bloku `else if` są wykonywane. Jeśli żaden z warunków nie zostanie spełniony, instrukcje w bloku `else` są wykonywane. Można dodać dowolną liczbę innych, jeśli bloki, a następnie zamknąć blok `else`, jak &quot;wszystko inne&quot; warunku.

Aby przetestować dużą liczbę warunków, użyj bloku `switch`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Wartość do przetestowania jest w nawiasach (w przykładzie zmienna `weekday`). Każdy test używa instrukcji `case` kończącej się dwukropkiem (:). Jeśli wartość instrukcji `case` pasuje do wartości testowej, kod w tym bloku Case jest wykonywany. Każdą instrukcję Case należy zamknąć za pomocą instrukcji `break`. (Jeśli zapomnisz uwzględnić przerwy w każdym bloku `case`, kod z następnej `case` instrukcji zostanie również uruchomiony). Blok `switch` często ma instrukcję `default` jako ostatni przypadek dla &quot;wszystkiego inne&quot; opcji, która jest uruchamiana, jeśli żaden z innych przypadków nie ma wartości true.

Wynik ostatnich dwóch bloków warunkowych wyświetlanych w przeglądarce:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Kod zapętlenia

Często trzeba wielokrotnie uruchamiać te same instrukcje. Można to zrobić przez zapętlenie. Na przykład często uruchamiasz te same instrukcje dla każdego elementu w kolekcji danych. Jeśli wiesz dokładnie, ile razy chcesz wykonać pętlę, możesz użyć pętli `for`. Ten rodzaj pętli jest szczególnie przydatny do zliczania w górę lub w dół:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Pętla zaczyna się od słowa kluczowego `for`, a następnie trzy instrukcje w nawiasach, każda zakończona średnikiem.

- Wewnątrz nawiasów Pierwsza instrukcja (`var i=10;`) tworzy licznik i inicjuje go do 10. Nie musisz nazwać licznika `i` &#8212; można użyć dowolnej zmiennej. Po uruchomieniu pętli `for` licznik jest automatycznie zwiększany.
- Druga instrukcja (`i < 21;`) ustawia warunek, dla którego chcesz obliczyć liczbę. W tym przypadku chcemy, aby przeszedł maksymalnie 20 (czyli kontynuować, gdy wartość licznika jest mniejsza niż 21).
- Trzecia instrukcja (`i++`) używa operatora przyrostu, który po prostu określa, że licznik powinien mieć 1 dodany do niego przy każdym uruchomieniu pętli.

Wewnątrz nawiasów klamrowych jest kod, który będzie uruchamiany dla każdej iteracji pętli. Znacznik powoduje utworzenie nowego akapitu (`<p>` elementu) za każdym razem i dodanie linii do danych wyjściowych, wyświetlając wartość `i` (licznik). Po uruchomieniu tej strony przykład tworzy 11 wierszy wyświetlających dane wyjściowe, a tekst w każdym wierszu wskazujący numer elementu.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Jeśli pracujesz z kolekcją lub tablicą, często używasz pętli `foreach`. Kolekcja jest grupą podobnych obiektów, a pętla `foreach` umożliwia wykonywanie zadania na każdym elemencie w kolekcji. Pętla tego typu jest wygodna dla kolekcji, ponieważ w przeciwieństwie do pętli `for` nie trzeba zwiększać licznika ani ustawiać limitu. Zamiast tego kod pętli `foreach` po prostu przechodzi przez kolekcję do momentu zakończenia.

Na przykład poniższy kod zwraca elementy w kolekcji `Request.ServerVariables`, która jest obiektem, który zawiera informacje o serwerze sieci Web. Używa pętli `foreac` h, aby wyświetlić nazwę każdego elementu przez utworzenie nowego elementu `<li>` na liście punktowanej HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Po słowie kluczowym `foreach` następuje nawiasy, w którym deklarujesz zmienną reprezentującą pojedynczy element w kolekcji (w przykładzie `var item`), po której następuje słowo kluczowe `in`, a następnie kolekcja, w której chcesz umieścić pętlę. W treści pętli `foreach` można uzyskać dostęp do bieżącego elementu przy użyciu zmiennej, która została zadeklarowana wcześniej.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Aby utworzyć więcej pętli ogólnego przeznaczenia, użyj instrukcji `while`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Pętla `while` rozpoczyna się od słowa kluczowego `while`, a następnie nawiasów, w których można określić czas, w którym pętla jest kontynuowana (w tym przypadku, tak długo, jak `countNum` jest mniejsza niż 50), a następnie blok, który ma zostać powtórzony. Pętle zwykle zwiększają się (Dodaj do) lub zmniejszają (odejmowanie od) zmiennej lub obiektu używanej do zliczania. W przykładzie operator `+=` dodaje 1 do `countNum` za każdym razem, gdy pętla zostanie uruchomiona. (Aby zmniejszyć zmienną w pętli, która liczy w dół, użyj operatora zmniejszania `-=`).

## <a name="objects-and-collections"></a>Obiekty i kolekcje

Niemal wszystko w witrynie ASP.NET Web to obiekt, w tym sama strona sieci Web. W tej sekcji omówiono niektóre ważne obiekty, które często działają w kodzie.

### <a name="page-objects"></a>Obiekty strony

Najbardziej podstawowym obiektem w ASP.NET jest strona. Można uzyskać dostęp do właściwości obiektu strony bezpośrednio bez żadnego obiektu kwalifikującego. Poniższy kod pobiera ścieżkę pliku strony przy użyciu obiektu `Request` strony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Aby wyczyścić, że odwołujesz się do właściwości i metod na bieżącym obiekcie strony, możesz opcjonalnie użyć słowa kluczowego `this`, aby reprezentować obiekt strony w kodzie. Oto poprzedni przykład kodu z `this` dodany do reprezentowania strony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Właściwości obiektu `Page` można użyć, aby uzyskać wiele informacji, takich jak:

- `Request`. Jak już widzisz, jest to zbiór informacji dotyczących bieżącego żądania, w tym typ przeglądarki, która złożyła żądanie, adres URL strony, tożsamość użytkownika itd.
- `Response`. Jest to zbiór informacji o odpowiedzi (stronie), które zostaną wysłane do przeglądarki po zakończeniu działania kodu serwera. Na przykład można użyć tej właściwości do zapisu informacji w odpowiedzi. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Obiekty kolekcji (tablice i słowniki)

*Kolekcja* jest grupą obiektów tego samego typu, na przykład z kolekcją `Customer` obiektów z bazy danych. ASP.NET zawiera wiele wbudowanych kolekcji, takich jak kolekcja `Request.Files`.

Często pracujesz z danymi w kolekcjach. Dwa popularne typy kolekcji to *Tablica* i *słownik*. Tablica jest przydatna, gdy chcesz przechowywać kolekcję podobnych elementów, ale nie chcesz tworzyć oddzielnej zmiennej do przechowywania poszczególnych elementów:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Z tablicami deklaruje się konkretny typ danych, taki jak `string`, `int`lub `DateTime`. Aby wskazać, że zmienna może zawierać tablicę, należy dodać nawiasy do deklaracji (na przykład `string[]` lub `int[]`). Dostęp do elementów w tablicy można uzyskać przy użyciu ich położenia (indeksu) lub instrukcji `foreach`. Indeksy tablicy są oparte na &#8212; zero oznaczające, że pierwszy element znajduje się na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Możesz określić liczbę elementów w tablicy, pobierając jej Właściwość `Length`. Aby uzyskać położenie określonego elementu w tablicy (aby przeszukać tablicę), użyj metody `Array.IndexOf`. Można również wykonywać operacje, takie jak odwracanie zawartości tablicy (Metoda `Array.Reverse`) lub sortowanie zawartości (Metoda `Array.Sort`).

Dane wyjściowe kodu tablicy ciągów wyświetlanego w przeglądarce:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Słownik jest kolekcją par klucz/wartość, w których podano klucz (lub nazwę), aby ustawić lub pobrać odpowiednią wartość:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Aby utworzyć słownik, użyj słowa kluczowego `new`, aby wskazać, że tworzysz nowy obiekt słownika. Można przypisać słownik do zmiennej za pomocą słowa kluczowego `var`. Możesz wskazać typy danych elementów w słowniku przy użyciu nawiasów ostrych (`< >`). Na końcu deklaracji należy dodać parę nawiasów, ponieważ jest to metoda, która tworzy nowy słownik.

Aby dodać elementy do słownika, można wywołać metodę `Add` zmiennej słownika (`myScores` w tym przypadku), a następnie określić klucz i wartość. Alternatywnie możesz użyć nawiasów kwadratowych, aby wskazać klucz i wykonać proste przypisanie, jak w poniższym przykładzie:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Aby uzyskać wartość z słownika, należy określić klucz w nawiasach:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Wywoływanie metod z parametrami

Podczas odczytywania wcześniej w tym artykule obiekty, z którymi program korzysta, mogą mieć metody. Na przykład obiekt `Database` może mieć metodę `Database.Connect`. Wiele metod ma także jeden lub więcej parametrów. *Parametr* jest wartością, którą można przekazać do metody, aby umożliwić wykonywanie jej zadania. Na przykład Przyjrzyj się deklaracji dla metody `Request.MapPath`, która przyjmuje trzy parametry:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Wiersz został opakowany, aby zwiększyć jego czytelność. Należy pamiętać, że podziały wierszy można umieścić niemal w dowolnym miejscu, z wyjątkiem wewnątrz ciągów, które są ujęte w cudzysłów.)

Ta metoda zwraca ścieżkę fizyczną na serwerze, który odpowiada określonej ścieżce wirtualnej. Trzy parametry dla metody to `virtualPath`, `baseVirtualDir`i `allowCrossAppMapping`. (Należy zauważyć, że w deklaracji parametry są wyświetlane z danymi o typach danych, które będą akceptowane). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.

Składnia Razor oferuje dwie opcje przekazywania parametrów do metody: *parametry pozycyjne* i *nazwane parametry*. Aby wywołać metodę przy użyciu parametrów pozycyjnych, należy przekazać parametry w ścisłej kolejności, która jest określona w deklaracji metody. (Zazwyczaj znana jest ta kolejność, odczytując dokumentację dla metody). Należy postępować zgodnie z kolejnością, a w razie potrzeby nie można &#8212; pominąć żadnego z parametrów, należy przekazać pusty ciąg (`""`) lub `null` dla parametru pozycyjnego, dla którego nie ma wartości.

W poniższym przykładzie przyjęto założenie, że w witrynie sieci Web znajduje się folder o nazwie *skrypty* . Kod wywołuje metodę `Request.MapPath` i przekazuje wartości dla trzech parametrów w poprawnej kolejności. Następnie zostanie wyświetlona zamapowana ścieżka.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Gdy metoda ma wiele parametrów, można zachować kod bardziej czytelny przy użyciu nazwanych parametrów. Aby wywołać metodę przy użyciu parametrów nazwanych, należy określić nazwę parametru, po którym następuje dwukropek (:), a następnie wartość. Zaletą parametrów nazwanych jest możliwość przekazywania ich w dowolnej kolejności. (Wadą jest to, że wywołanie metody nie jest jako kompaktowe).

Poniższy przykład wywołuje tę samą metodę, jak powyżej, ale używa nazwanych parametrów do dostarczenia wartości:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Jak widać, parametry są przesyłane w innej kolejności. Jeśli jednak zostanie uruchomiony poprzedni przykład i ten przykład zwróci tę samą wartość.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Obsługa błędów

### <a name="try-catch-statements"></a>Instrukcje try-catch

Często występują instrukcje w kodzie, które mogą zakończyć się niepowodzeniem z przyczyn poza formantem. Na przykład:

- Jeśli kod próbuje utworzyć plik lub uzyskać do niego dostęp, mogą wystąpić wszystkie rodzaje błędów. Plik, który nie istnieje, może być zablokowany, kod może nie mieć uprawnień i tak dalej.
- Podobnie, jeśli kod próbuje zaktualizować rekordy w bazie danych, mogą wystąpić problemy z uprawnieniami, połączenie z bazą danych może być porzucone, dane do zapisania mogą być nieprawidłowe i tak dalej.

W warunkach programowania te sytuacje nazywają się *wyjątkami*. Jeśli Twój kod napotyka wyjątek, generuje (zgłasza) komunikat o błędzie, który jest najprawdopodobniej przez użytkownika:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

W sytuacjach, w których kod może napotkać wyjątki i aby uniknąć komunikatów o błędach tego typu, można użyć instrukcji `try/catch`. W instrukcji `try` uruchamiasz sprawdzany kod. W co najmniej jednej instrukcji `catch` można wyszukać konkretne błędy (określone typy wyjątków), które mogły wystąpić. Możesz dołączyć dowolną liczbę instrukcji `catch`, ile potrzebujesz, aby wyszukać błędy, które są przewidywane.

> [!NOTE]
> Zalecamy uniknięcie użycia metody `Response.Redirect` w instrukcjach `try/catch`, ponieważ może to spowodować wyjątek na stronie.

Poniższy przykład pokazuje stronę, która tworzy plik tekstowy przy pierwszym żądaniu, a następnie wyświetla przycisk, który umożliwia użytkownikowi otwarcie pliku. Przykład celowo używa nieprawidłowej nazwy pliku, aby powodował wyjątek. Kod zawiera instrukcje `catch` dla dwóch możliwych wyjątków: `FileNotFoundException`, które występuje, jeśli nazwa pliku jest zła, i `DirectoryNotFoundException`, który występuje, jeśli ASP.NET nie może odnaleźć folderu. (Możesz usunąć komentarz z instrukcji w przykładzie, aby zobaczyć, jak działa, gdy wszystko działa prawidłowo).

Jeśli kod nie obsłużył wyjątku, zobaczysz stronę błędu, jak poprzedni zrzut ekranu. Jednak sekcja `try/catch` pozwala zapobiec wyświetlaniu tych typów błędów przez użytkownika.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

**Programowanie przy użyciu Visual Basic**

[Dodatek: Visual Basic język i składnia](https://go.microsoft.com/fwlink/?LinkId=202908)

**Dokumentacja referencyjna**

[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C#Językowe](https://msdn.microsoft.com/library/kx37x362.aspx)

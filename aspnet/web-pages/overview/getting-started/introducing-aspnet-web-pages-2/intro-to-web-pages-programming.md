---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Wprowadzenie do stron sieci Web ASP.NET — Podstawy programowania | Microsoft Docs
author: Rick-Anderson
description: 'Ten samouczek zawiera omówienie sposobu programowania programu na stronach sieci Web ASP.NET za pomocą składnia Razor. Dowiesz się: podstawowa składnia "Razor" używana dla żądania ściągnięcia...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 474de7671ac2931e5ba9ff635d77385403644521
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628479"
---
# <a name="introducing-aspnet-web-pages---programming-basics"></a>Wprowadzenie do stron sieci Web ASP.NET — Podstawy programowania

Autor [FitzMacken](https://github.com/tfitzmac)

> Ten samouczek zawiera omówienie sposobu programowania programu na stronach sieci Web ASP.NET za pomocą składnia Razor.
> 
> Zawartość:
> 
> - Podstawowa składnia "Razor", która jest używana do programowania na stronach sieci Web ASP.NET.
> - Podstawowe C#, czyli język programowania, którego będziesz używać.
> - Niektóre podstawowe koncepcje programowania dla stron sieci Web.
> - Jak zainstalować pakiety (składniki zawierające wstępnie skompilowany kod) do użycia z witryną programu.
> - Jak używać *pomocników* do wykonywania typowych zadań programistycznych.
>   
> 
> Omówione funkcje/technologie:
> 
> - Narzędzia NuGet i Menedżer pakietów.
> - Pomocnik `Gravatar`.

Ten samouczek jest przede wszystkim celem wprowadzenia do składni programowania, która będzie używana dla stron sieci Web ASP.NET. Dowiesz się więcej na temat *składnia Razor* i kodu, który jest C# pisany w języku programowania. Możliwość wypróbowania innowacyjnego tę składnię w poprzednim samouczku; w tym samouczku wyjaśnimy składnię więcej.

Firma Microsoft gwarantuje, że ten samouczek obejmuje większość programowania, które zobaczysz w jednym samouczku i *że jest to jedyny samouczek dotyczący* programowania. W pozostałych samouczkach tego zestawu utworzysz strony, które są interesujące.

Zapoznaj się również z *pomocą techniczną*. Pomocnik to składnik — spakowany fragment kodu, który można dodać do strony. Pomocnik wykonuje zadanie, które w przeciwnym razie może być żmudnym lub złożone.

## <a name="creating-a-page-to-play-with-razor"></a>Tworzenie strony do odtwarzania przy użyciu Razor

W tej sekcji poznasz bit z Razor, aby poznać podstawową składnię.

Uruchom program WebMatrix, jeśli nie jest jeszcze uruchomiony. Użyjesz witryny sieci Web utworzonej w poprzednim samouczku ([wprowadzenie ze stronami sieci Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Aby go otworzyć, kliknij pozycję **Moje witryny** i wybierz pozycję **WebPageMovies**:

![Ekran startowy WebMatrix z wyróżnionymi opcjami otwartej witryny i witrynami podświetlonymi](intro-to-web-pages-programming/_static/image1.png)

Wybierz obszar roboczy **pliki** .

Na wstążce kliknij przycisk **New (nowy** ), aby utworzyć stronę. Wybierz pozycję **cshtml** i nadaj nazwę nowej stronie *TestRazor. cshtml*.

Kliknij przycisk **OK**.

Skopiuj następujący plik do pliku, aby całkowicie zastąpić to, co już istnieje.

> [!NOTE]
> Podczas kopiowania kodu lub znaczników z przykładów na stronę, wcięcia i wyrównanie mogą nie być takie same jak w samouczku. Wcięcia i wyrównanie nie wpływają na sposób działania kodu, chociaż.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Badanie przykładowej strony

Większość wyświetlonych elementów to zwykły kod HTML. Jednak w górnej części tego bloku kodu jest:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Zwróć uwagę na następujące kwestie dotyczące tego bloku kodu:

- @ Znak informuje ASP.NET, że poniżej znajduje się kod Razor, a nie HTML. ASP.NET potraktuje wszystko po znaku @ jako kod, dopóki nie zostanie on ponownie uruchomiony w kodzie HTML. (W tym przypadku jest to &lt;! Element DOCTYPE&gt;.
- Nawiasy klamrowe ({i}) otaczają blok kodu Razor, jeśli kod ma więcej niż jeden wiersz. Nawiasy poinformują ASP.NET, gdzie kod dla tego bloku zaczyna się i kończą.
- Znaki//oznaczają komentarz — to znaczy część kodu, który nie zostanie wykonany.
- Każda instrukcja musi kończyć się średnikiem (;). (Nie komentarze, chociaż.)
- Możesz przechowywać wartości w *zmiennych*, które tworzysz (*deklaruj*) przy użyciu var słowa kluczowego. Podczas tworzenia zmiennej nadaj jej nazwę, która może zawierać litery, cyfry i znaki podkreślenia (\_). Nazwy zmiennych nie mogą rozpoczynać się od cyfry i nie mogą używać nazwy słowa kluczowego programowania (na przykład var).
- Ciągi znaków (takie jak "ASP.NET" i "Web Pages") należy ująć w cudzysłów. (Muszą to być podwójne cudzysłowy). Liczby nie są ujęte w cudzysłów.
- Odstępy poza znakami cudzysłowu nie mają znaczenia. Podziały wierszy nie mają znaczenia; wyjątek polega na tym, że nie można podzielić ciągu w cudzysłowie w wierszach. Wcięcia i wyrównanie nie mają znaczenia.

Nie jest to oczywiste w tym przykładzie, że cały kod uwzględnia wielkość liter. Oznacza to, że zmienna TheSum jest inną zmienną niż zmienne, które mogą mieć nazwę theSum lub TheSum. Podobnie, var jest słowem kluczowym, ale var nie jest.

### <a name="objects-and-properties-and-methods"></a>Obiekty i właściwości i metody

A następnie istnieje element DateTime. Now. W przypadku prostych warunków Data/godzina jest *obiektem*. Obiekt to element, który można programować z — stroną, polem tekstowym, plikiem, obrazem, żądaniem sieci Web, wiadomością e-mail, rekordem klienta itd. Obiekty mają jedną lub więcej *Właściwości* , które opisują ich cechy. Obiekt pola tekstowego ma właściwość Text (między innymi), obiekt żądania ma właściwość adresu URL (i inne), wiadomość e-mail ma właściwość od i do i tak dalej. Obiekty mają również *metody* , które są "zlecenia", które mogą wykonywać. Będziesz pracować z obiektami.

Jak widać na przykład, wartość DateTime jest obiektem, który umożliwia zaprogramowanie dat i godzin. Ma on właściwość o nazwie Now, która zwraca bieżącą datę i godzinę.

### <a name="using-code-to-render-markup-in-the-page"></a>Renderowanie znaczników na stronie przy użyciu kodu

W treści strony Zwróć uwagę na następujące kwestie:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Ponownie znak @ informuje ASP.NET, że jest to kod, a nie HTML. W znaczniku można dodać @, a po nim wyrażenie kodu, a ASP.NET będzie renderować wartość tego wyrażenia bezpośrednio w tym momencie. W przykładzie @a będzie renderowany niezależnie od wartości zmiennej o nazwie a, @product renderuje w zmiennej o nazwie Product i tak dalej.

Nie są ograniczone do zmiennych, chociaż. W tym miejscu w kilku wystąpieniach znak @ poprzedza wyrażenie:

- @ (a\*b) renderuje iloczyn dowolnego elementu to zmienne a i b. (Operator \* oznacza mnożenie).
- @ (technologia + "" + Product) renderuje wartości w zmiennej technologii i produkcie po ich łączeniu i dodawaniu odstępu między. Operator (+) służący do łączenia ciągów jest taki sam jak operator do dodawania liczb. ASP.NET może zazwyczaj stwierdzić, czy pracujesz z liczbami lub z ciągami i czy z operatorem +.
- @Request.Url renderuje Właściwość adresu URL obiektu żądania. Obiekt request zawiera informacje o bieżącym żądaniu z przeglądarki oraz o tym, że właściwość adresu URL zawiera adres URL tego bieżącego żądania.

Przykład został również zaprojektowany, aby pokazać, że można wykonywać zadania na różne sposoby. Możesz wykonywać obliczenia w bloku kodu u góry, umieścić wyniki w zmiennej, a następnie renderować zmienną w znaczniku. Lub można wykonywać obliczenia w prawidłowym wyrażeniu w znaczniku. Używane podejście zależy od tego, co robisz, i do pewnego stopnia zgodnie z własnymi preferencjami.

### <a name="seeing-the-code-in-action"></a>Wyświetlanie kodu w akcji

Kliknij prawym przyciskiem myszy nazwę pliku, a następnie wybierz polecenie **Uruchom w przeglądarce**. Zostanie wyświetlona strona w przeglądarce ze wszystkimi wartościami i wyrażeniami, które zostały rozwiązane na stronie.

![Strona "TestRazor" uruchomiona w przeglądarce](intro-to-web-pages-programming/_static/image2.png)

Przyjrzyj się źródłu w przeglądarce.

![Źródło strony "test Razor" w przeglądarce](intro-to-web-pages-programming/_static/image3.png)

Zgodnie z oczekiwaniami w poprzednim samouczku, żaden kod Razor nie znajduje się na stronie. Wszystkie wyświetlane są rzeczywiste wartości wyświetlania. Po uruchomieniu strony użytkownik wysyła żądanie do serwera sieci Web, który jest wbudowany w Program WebMatrix. Gdy żądanie zostanie odebrane, ASP.NET rozpoznaje wszystkie wartości i wyrażenia i renderuje ich wartości na stronie. Następnie wysyła stronę do przeglądarki.

> [!TIP] 
> 
> **Razor iC#**
> 
> Do tej pory mamy już, że pracujesz z składnia Razor. To prawda, ale nie jest to kompletna historia. Sam używany język programowania jest wywoływany *C#* . C#został utworzony przez firmę Microsoft na dekadę temu i stał się jednym z podstawowych języków programowania do tworzenia aplikacji systemu Windows. Wszystkie zasady, które wystąpiły, aby określić nazwę zmiennej i sposób tworzenia instrukcji i tak dalej, są faktycznie wszystkimi regułami C# języka.
> 
> Razor odwołuje się dokładniej do małego zestawu konwencji dotyczących sposobu osadzania tego kodu na stronie. Na przykład Konwencja używania @ do oznaczania kodu na stronie i używania @ {} do osadzenia bloku kodu jest aspektem Razor strony. Pomocniki są również uznawane za części Razor. Składnia Razor jest używany w większej liczbie miejsc niż tylko w ASP.NET stronach sieci Web. (Na przykład jest również używany w widokach ASP.NET MVC).
> 
> Wspominamy, że jeśli szukasz informacji o programowaniu ASP.NET strony sieci Web, znajdziesz wiele odwołań do Razor. Jednak wiele z tych odwołań nie ma zastosowania do wykonywanych operacji i dlatego może być mylące. W rzeczywistości wiele pytań związanych z programowaniem jest naprawdę w trakcie pracy z ASP.NET C# lub pracy z nim. Dlatego jeśli szukasz szczegółowych informacji na temat Razor, możesz nie znaleźć potrzebnych odpowiedzi.

## <a name="adding-some-conditional-logic"></a>Dodawanie logiki warunkowej

Jedną z doskonałych funkcji dotyczących używania kodu na stronie jest możliwość zmiany tego, co się dzieje na podstawie różnych warunków. W tej części samouczka znajdziesz różne sposoby zmiany elementów wyświetlanych na stronie.

Przykład będzie prosty i dość contrived, dzięki czemu możemy skoncentrować się na logice warunkowej. Ta strona zostanie utworzona:

- Pokaż inny tekst na stronie w zależności od tego, czy jest on wyświetlany po raz pierwszy, czy kliknięto przycisk w celu przesłania strony. Jest to pierwszy test warunkowy.
- Wyświetlaj komunikat tylko wtedy, gdy określona wartość jest przekazana w ciągu zapytania adresu URL (http://...? show = true). Jest to drugi test warunkowy.

W programie WebMatrix Utwórz stronę i nadaj jej nazwę *TestRazorPart2. cshtml*. Na wstążce kliknij pozycję **Nowy**, wybierz pozycję **cshtml**, Nazwij plik, a następnie kliknij przycisk **OK**.

Zamień zawartość tej strony na następujące elementy:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Blok kodu u góry inicjuje zmienną o nazwie Message z tekstem. W treści strony zawartość zmiennej komunikatu jest wyświetlana wewnątrz elementu &lt;p&gt;. Znacznik zawiera również element &lt;Input&gt;, aby utworzyć przycisk **Prześlij** .

Uruchom stronę, aby zobaczyć, jak działa teraz. Na razie jest to po prostu strona statyczna, nawet jeśli klikniesz przycisk **Prześlij** .

Wróć do WebMatrix. W bloku kodu, Dodaj następujący wyróżniony kod *po* wierszu, który inicjuje komunikat:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Blok if {}

Właśnie dodany element był warunkiem IF. W kodzie, warunek if ma strukturę taką jak:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Warunek do przetestowania jest w nawiasach. Musi być wartością lub wyrażeniem zwracającym wartość true lub false. Jeśli warunek ma wartość true, ASP.NET uruchamia instrukcję lub instrukcje, które znajdują się w nawiasach klamrowych. (Są *to częścią* logiki *if-then* ). Jeśli warunek ma wartość false, blok kodu jest pomijany.

Poniżej przedstawiono kilka przykładów warunków, które można testować w instrukcji if:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Można testować zmienne względem wartości lub względem wyrażeń przy użyciu *operatora logicznego* lub *operatora porównania*: równe (= =), większe niż (&gt;), mniejsze niż (&lt;), większe niż lub równe (&gt;=) i mniejsze lub równe (&lt;=). Operator! = nie jest równy — na przykład jeśli (a! = 0) oznacza, że *wartość nie jest równa 0*.

> [!NOTE]
> Upewnij się, że operator porównania dla operatora równości (= =) nie jest taki sam jak wartość =. Operator = służy tylko do przypisywania wartości (var a = 2). Jeśli te operatory są mieszane, wystąpi błąd lub uzyskasz pewne nietypowe wyniki.

Aby sprawdzić, czy coś ma wartość true, Pełna składnia to if (isdone = = true). Ale można również użyć skrótu if (isdone). Jeśli nie ma operatora porównania, ASP.NET zakłada, że test jest prawdziwy.

Polu! operator sam oznacza wartość logiczną NOT. Na przykład warunek if (! Ispost) oznacza, *że właściwość ispost nie ma wartości true*.

Możesz połączyć warunki przy użyciu operatora logicznego i (&amp;&amp;) lub operatora logicznego OR (| |). Na przykład ostatni warunek if w powyższych przykładach oznacza, *że FileProcessingIsDone nie ma wartości true, a displayMessage jest ustawiona na false*.

### <a name="the-else-block"></a>Blok else

Jedną z końcowych rzeczy o if Blocks: blok if może następować blok else. Blok else jest przydatny w przypadku, gdy warunek ma wartość false. Oto prosty przykład:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

W kolejnych samouczkach w tej serii znajdziesz kilka przykładów, w których jest przydatny blok else.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Testowanie, czy żądanie jest przesłane (post)

Istnieje więcej, ale Wróćmy do przykładu, który ma warunek if (ispost) {...}. Właściwość ispost jest w rzeczywistości właściwością bieżącej strony. Gdy strona jest żądana po raz pierwszy, funkcja ispost zwraca wartość false. Jeśli jednak klikniesz przycisk lub w inny sposób prześlesz stronę — to znaczy, opublikujemy ją — funkcja ispost zwraca wartość true. Dzięki temu można określić, czy zamierzasz przesłać formularz. (W odniesieniu do zleceń HTTP, jeśli żądanie jest operacją GET, funkcja ispost zwraca wartość false. Jeśli żądanie jest operacją POST, funkcja ispost zwraca wartość true.) W późniejszym samouczku będziesz korzystać z formularzy wejściowych, w których ten test jest szczególnie użyteczny.

Uruchom stronę. Ponieważ po raz pierwszy żądasz strony, zobaczysz "jest to pierwsze żądanie strony". Ten ciąg jest wartością, do której zainicjowano zmienną komunikatów. Istnieje test if (ispost), ale który zwraca wartość false, więc kod wewnątrz bloku if nie jest uruchamiany.

Kliknij przycisk **Prześlij** . Strona zostanie ponownie zażądana. Tak jak wcześniej zmienna komunikatu jest ustawiona na "jest to pierwszy raz...". Jednak ten czas test if (ispost) zwraca wartość true, więc kod wewnątrz bloku if zostanie uruchomiony. Kod zmienia wartość zmiennej Message na inną wartość, która jest renderowana w znaczniku.

Teraz Dodaj warunek if w znaczniku. Poniżej elementu &lt;p&gt; zawierającego przycisk **Prześlij** Dodaj następujące znaczniki:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Dodajesz kod wewnątrz znacznika, więc musisz zacząć od @. Następnie istnieje test if podobny do tego, który został dodany wcześniej w bloku kodu. W nawiasach klamrowych, mimo że dodawana jest zwykła zawartość HTML — co najmniej jest zwykłe do momentu, aż do @DateTime.Now. Jest to kolejny mały bit kodu Razor, dlatego należy dodać @ przed nim.

W tym miejscu możesz dodać warunki, jeśli w bloku kodu u góry i w znaczniku. W przypadku użycia warunku if w treści strony linie wewnątrz bloku mogą być znacznikiem lub kodem. W takim przypadku i tak samo jak w przypadku mieszania znaczników i kodu, musisz użyć @, aby ASP.NET, gdzie kod jest.

Uruchom stronę i kliknij pozycję **Prześlij**. Ten czas nie tylko widzisz inny komunikat podczas przesyłania ("teraz przesłano..."), ale zobaczysz nową wiadomość, która wyświetla datę i godzinę.

![Strona "test Razor 2" działająca w przeglądarce z sygnaturą czasową wyświetlaną po przesłaniu](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testowanie wartości ciągu zapytania

Jeszcze jeden test. Tym razem dodasz blok IF, który testuje wartość o nazwie show, która może być przekazana w ciągu zapytania. (W tym: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) Zmienisz stronę w taki sposób, aby komunikat był wyświetlany ("jest to pierwszy raz..." itp.), tylko wtedy, gdy wartość parametru show ma wartość true.

Na dole (ale wewnątrz) blok kodu w górnej części strony Dodaj następujące elementy:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Pełny blok kodu wygląda teraz podobnie jak w poniższym przykładzie. (Pamiętaj, że po skopiowaniu kodu na stronę, wcięcie może wyglądać inaczej. Ale nie ma to wpływu na sposób uruchamiania kodu.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Nowy kod w bloku inicjuje zmienną o nazwie showMessage na wartość false. Następnie wykonuje test IF, aby wyszukać wartość w ciągu zapytania. Pierwsze żądanie strony ma adres URL podobny do tego:

`http://localhost:43097/TestRazorPart2.cshtml`

Kod określa, czy adres URL zawiera zmienną o nazwie show w ciągu zapytania, taką jak ta wersja adresu URL:

`http://localhost:43097/TestRazorPart2.cshtml`? show = true

Test sam sprawdza Właściwość QueryString obiektu request. Jeśli ciąg zapytania zawiera element o nazwie show, a jeśli ten element jest ustawiony na wartość true, blok if działa i ustawia zmienną showMessage na wartość true.

W tym miejscu znajduje się lewę, którą można zobaczyć. Podobnie jak nazwa mówi, ciąg zapytania jest ciągiem. Jednakże można testować tylko dla wartości true i false, jeśli testowana wartość jest wartością logiczną (true/false). Aby można było przetestować wartość zmiennej show w ciągu zapytania, należy przekonwertować ją na wartość logiczną. To właśnie Metoda AsBool — Pobiera ciąg jako dane wejściowe i konwertuje go na wartość logiczną. Jasno, jeśli ciąg ma wartość "true", Metoda AsBool konwertuje tę wartość na true. Jeśli wartość ciągu jest coś innego, AsBool zwraca wartość false.

> [!TIP] 
> 
> **Typy danych i metody AS ()**
> 
> Mamy do tej pory, że podczas tworzenia zmiennej używane jest słowo kluczowe var. To nie jest cały wątek, chociaż. W celu manipulowania wartościami — w celu dodawania numerów lub łączenia ciągów, porównywania dat lub testowania dla prawdy/fałszu — C# musi współpracować z odpowiednią wewnętrzną reprezentacją wartości. C#*zwykle* może określać, co powinna być reprezentacja (czyli jakie *typy* danych są), na podstawie tego, co robią wartości. Teraz, ale nie jest to możliwe. Jeśli nie, musisz uzyskać pomoc, jawnie wskazując, jak C# powinny być reprezentowane dane. Metoda AsBool to — informuje C# , że wartość ciągu "true" lub "false" powinna być traktowana jako wartość logiczna. Podobne metody istnieją do reprezentowania ciągów jako innych typów, takich jak AsInt (Traktuj jako integer), AsDateTime (Traktuj jako data/godzina), AsFloat (Traktuj jako liczbę zmiennoprzecinkową) i tak dalej. Jeśli używasz tych metod AS (), jeśli C# nie może reprezentować wartości ciągu zgodnie z żądaniem, zobaczysz błąd.

W znaczniku strony Usuń lub Skomentuj ten element (w tym miejscu jest wyświetlany komentarz):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Z prawej strony, w której został usunięty lub oznaczony jako komentarz, Dodaj następujący tekst:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Test if wskazuje, że jeśli zmienna showMessage ma wartość true, renderuje element &lt;p&gt; z wartością zmiennej Message.

### <a name="summary-of-your-conditional-logic"></a>Podsumowanie logiki warunkowej

W przypadku, gdy nie masz całkowicie pewności, co zostało zrobione, poniżej przedstawiono podsumowanie.

- Zmienna komunikatu jest inicjowana do domyślnego ciągu ("jest to pierwszy raz...").
- Jeśli żądanie strony jest wynikiem przesłania (post), wartość komunikatu jest zmieniana na "teraz przesłane..."
- Zmienna showMessage jest inicjowana na wartość false.
- Jeśli ciąg zapytania zawiera? show = true, zmienna showMessage jest ustawiona na wartość true.
- W znaczniku, jeśli showMessage ma wartość true, renderowany jest element &lt;p&gt;, który pokazuje wartość komunikatu. (Jeśli showMessage ma wartość false, nic nie jest renderowane w tym momencie w znaczniku).
- W znaczniku, jeśli żądanie jest wpisem, jest renderowany &lt;p&gt; element, który wyświetla datę i godzinę.

Uruchom stronę. Nie ma komunikatu, ponieważ showMessage ma wartość false, więc w przypadku testu znaczników if (showMessage) zwraca wartość false.

Kliknij przycisk **Prześlij**. Zobaczysz datę i godzinę, ale nadal nie ma komunikatu.

W przeglądarce przejdź do pola adres URL i Dodaj następujący tekst na końcu adresu URL:? show = true, a następnie naciśnij klawisz ENTER.

![Strona "test Razor 2" w przeglądarce przedstawiająca ciąg zapytania](intro-to-web-pages-programming/_static/image5.png)

Zostanie wyświetlona ponownie strona. (Ze względu na to, że adres URL został zmieniony, jest to nowe żądanie, a nie przesłanie.) Kliknij przycisk **Prześlij** ponownie. Komunikat zostanie wyświetlony ponownie, zgodnie z datą i godziną.

![Strona "test Razor 2" po przesłaniu, gdy istnieje ciąg zapytania](intro-to-web-pages-programming/_static/image6.png)

W adresie URL Zmień wartość? show = true na? show = false i naciśnij klawisz ENTER. Prześlij ponownie stronę. Strona jest powracana do sposobu rozpoczęcia — Brak komunikatu.

Jak wspomniano wcześniej, logika tego przykładu jest nieco contrived. Jeśli jednak przejdziesz na wiele stron i zostanie on użyty co najmniej jeden z formularzy, które były widoczne w tym miejscu.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalowanie pomocnika (wyświetlanie obrazu skonfigurowali Gravatar)

Niektóre zadania, które często mają być wykonywane na stronach sieci Web, wymagają dużej ilości kodu lub wymagają dodatkowej wiedzy. Przykłady: Wyświetlanie wykresu dla danych; Umieszczanie na stronie przycisku "Like" w serwisie Facebook Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie lub zmienianie rozmiarów obrazów; Korzystanie z systemu PayPal dla witryny. Aby ułatwić Ci wykonywanie tego rodzaju rzeczy, ASP.NET strony sieci Web umożliwiają korzystanie z *pomocników*. Pomocnicy to składniki instalowane dla lokacji programu, które umożliwiają wykonywanie typowych zadań przy użyciu tylko kilku wierszy kodu Razor.

Strony sieci Web ASP.NET zawierają kilka pomocników wbudowanych w. Jednak wiele pomocników jest dostępnych w pakietach (dodatków) udostępnianych za pomocą Menedżera pakietów NuGet. Pakiet NuGet umożliwia wybranie pakietu do zainstalowania, a następnie uwzględnia wszystkie szczegóły instalacji.

W tej części samouczka zainstalujesz pomocnika, który umożliwia wyświetlenie obrazu skonfigurowali Gravatar ("globalnie rozpoznany awatar"). Poznasz dwie rzeczy. Jednym z nich jest znalezienie i zainstalowanie pomocnika. Dowiesz się również, jak pomocnik ułatwia wykonywanie czynności, które w przeciwnym razie trzeba wykonać przy użyciu dużej ilości kodu, którą trzeba napisać.

Możesz zarejestrować własne skonfigurowali Gravatar w witrynie sieci Web skonfigurowali Gravatar w [http://www.gravatar.com/](http://www.gravatar.com/), ale nie jest to konieczne do utworzenia konta skonfigurowali Gravatar w celu wykonania tej części samouczka.

W programie WebMatrix kliknij przycisk **NuGet** .

![Okno dialogowe Galeria NuGet w programie WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Spowoduje to uruchomienie Menedżera pakietów NuGet i wyświetlenie dostępnych pakietów. (Nie wszystkie pakiety są pomocnikami, niektóre dodają funkcje do samej macierzy WebMatrix, niektóre są dodatkowymi szablonami itd.) Może zostać wyświetlony komunikat o błędzie dotyczący niezgodności wersji. Aby zignorować ten komunikat o błędzie, kliknij przycisk **OK** i Kontynuuj pracę z tym samouczkiem.

![Okno dialogowe Galeria NuGet w programie WebMatrix](intro-to-web-pages-programming/_static/image8.png)

W polu wyszukiwania wprowadź ciąg "Pomocnicy asp.net". Pakiet NuGet pokazuje pakiety, które pasują do wyszukiwanych terminów.

![Galeria NuGet w programie WebMatrix pokazująca pakiety](intro-to-web-pages-programming/_static/image9.png)

Biblioteka pomocników sieci Web ASP.NET zawiera kod, który upraszcza wiele typowych zadań, takich jak korzystanie z obrazów skonfigurowali Gravatar. Wybierz pakiet **biblioteki ASP.NET Web pomocnicys** , a następnie kliknij przycisk **Instaluj** , aby uruchomić Instalatora. Wybierz opcję **tak** po wyświetleniu monitu, jeśli chcesz zainstalować pakiet, a następnie zaakceptuj warunki, aby zakończyć instalację.

Gotowe. Pakiet NuGet pobiera i instaluje wszystko, w tym wszelkie dodatkowe składniki, które mogą być wymagane (*zależności*).

Jeśli z jakiegoś powodu trzeba odinstalować pomocnika, proces jest bardzo podobny. Kliknij przycisk **NuGet** , kliknij kartę **zainstalowane** i wybierz pakiet, który chcesz odinstalować.

## <a name="using-a-helper-in-a-page"></a>Korzystanie z pomocnika na stronie

Teraz będziesz używać pomocnika, który właśnie został zainstalowany. Proces dodawania pomocnika do strony jest podobny w przypadku większości pomocników.

W programie WebMatrix Utwórz stronę i nadaj jej nazwę *GravatarTest. cshml*. (Tworzysz specjalną stronę do testowania pomocnika, ale możesz użyć pomocników na dowolnej stronie w witrynie).

Wewnątrz elementu &lt;Body&gt;, Dodaj element &lt;DIV&gt;. Wewnątrz elementu &lt;DIV&gt;, wpisz:

@Gravatar.

@ Znak jest tym samym znakiem, który był używany do oznaczania kodu Razor. **Skonfigurowali Gravatar** jest obiektem pomocnika, z którym pracujesz.

Gdy tylko wpiszesz kropkę (.), WebMatrix wyświetla listę *metod* (funkcji), które udostępnia pomocnik skonfigurowali Gravatar:

![Lista rozwijana IntelliSense pomocnika skonfigurowali Gravatar](intro-to-web-pages-programming/_static/image10.png)

Ta funkcja jest nazywana technologią *IntelliSense*. Ułatwia to kod, zapewniając wybór odpowiednich kontekstów. Technologia IntelliSense współpracuje z językiem HTML, CSS, kodem ASP.NET, JavaScript i innymi językami, które są obsługiwane w programie WebMatrix. Jest to inna funkcja, która ułatwia tworzenie stron sieci Web w programie WebMatrix.

Naciśnij pozycję G na klawiaturze, a zobaczysz, że technologia IntelliSense odnajdzie metodę GetHtml. Naciśnij klawisz Tab. Technologia IntelliSense wstawia wybraną metodę (GetHtml). Wpisz otwarty nawias i Zauważ, że nawias zamykający jest automatycznie dodawany. Wpisz swój adres e-mail w cudzysłowie między dwoma nawiasami. Jeśli masz konto skonfigurowali Gravatar, zostanie zwrócony obraz profilu. Jeśli nie masz konta skonfigurowali Gravatar, zwracany jest obraz domyślny. Gdy skończysz, wiersz będzie wyglądać następująco:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Teraz Wyświetl stronę w przeglądarce. Wyświetlany jest obraz lub obraz domyślny, w zależności od tego, czy masz konto skonfigurowali Gravatar.

![Skonfigurowali Gravatar](intro-to-web-pages-programming/_static/image11.png) ![obraz domyślny](intro-to-web-pages-programming/_static/image12.png)

Aby uzyskać informacje o tym, co robi pomocnik, Wyświetl źródło strony w przeglądarce. Wraz z kodem HTML na stronie zobaczysz element obrazu, który zawiera identyfikator. Jest to kod, który pomocnik jest renderowany na stronie w miejscu, w którym @Gravatar.GetHtml. Pomocnik przejął podane informacje i wygenerował kod, który bezpośrednio nastawia się na skonfigurowali Gravatar w celu przywrócenia poprawnego obrazu dla podanego konta.

Metoda GetHtml umożliwia również dostosowanie obrazu, dostarczając inne parametry. Poniższy kod pokazuje, jak zażądać obrazu ma szerokość i wysokość 40 pikseli, i używa określonego obrazu domyślnego o nazwie **wavatar** , jeśli określone konto nie istnieje.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Ten kod generuje coś takiego jak poniższy wynik (obraz domyślny będzie się znacznie różnić).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Przyszłe przejście

Aby zachować ten samouczek krótko, musiałeś skupić się tylko na kilku podstawach. Naturalnie, istnieje *wiele* więcej niż Razor i C#. Dowiesz się więcej na temat tych samouczków. Jeśli chcesz dowiedzieć się więcej o aspektach programistycznych Razor i C# teraz, możesz zapoznać się z dokładniejszym wprowadzeniem tutaj: [wprowadzenie do ASP.NET programowanie sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

W następnym samouczku przedstawiono pracę z bazą danych programu. W tym samouczku rozpocznie się Tworzenie przykładowej aplikacji, która umożliwia wyświetlanie listy ulubionych filmów.

## <a name="complete-listing-for-testrazor-page"></a>Ukończ listę dla strony TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Ukończ listę dla strony TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Ukończ listę dla strony GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Dodatkowe materiały

- [Wprowadzenie do programowania w ASP.NET sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Pomocnik usługi Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Poprzednie](getting-started.md)
> [dalej](displaying-data.md)

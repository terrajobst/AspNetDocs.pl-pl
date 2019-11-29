---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Zrozumienie możliwości debugowania ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Możliwość debugowania kodu to umiejętność, którą każdy deweloper powinien mieć w arsenał niezależnie od używanej technologii. Chociaż wielu deweloperów jest...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 08ced380f3551407d757524dbc84b5feeeb5482b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601487"
---
# <a name="understanding-aspnet-ajax-debugging-capabilities"></a>Objaśnienie możliwości debugowania kodu ASP.NET AJAX

przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> Możliwość debugowania kodu to umiejętność, którą każdy deweloper powinien mieć w arsenał niezależnie od używanej technologii. Chociaż wielu deweloperów jest przyzwyczajonych do korzystania z programu Visual Studio .NET lub Web Developer Express do debugowania aplikacji ASP.NET, które C# korzystają z VB.NET lub kodu, niektóre z nich nie wiedzą, że jest to również niezwykle przydatne do debugowania kodu po stronie klienta, takiego jak JavaScript. Ten sam typ technik służący do debugowania aplikacji .NET może być również stosowany do aplikacji obsługujących technologię AJAX i bardziej szczegółowych aplikacji ASP.NET AJAX.

## <a name="debugging-aspnet-ajax-applications"></a>Debugowanie aplikacji ASP.NET AJAX

Dan Wahlin

Możliwość debugowania kodu to umiejętność, którą każdy deweloper powinien mieć w arsenał niezależnie od używanej technologii. Nie mówiąc, że zrozumienie różnych dostępnych opcji debugowania może zaoszczędzić bardzo dużo czasu na projekcie i prawdopodobnie nawet kilka zarządzaniem mu towarzyszą. Chociaż wielu deweloperów jest przyzwyczajonych do korzystania z programu Visual Studio .NET lub Web Developer Express do debugowania aplikacji ASP.NET, które C# korzystają z VB.NET lub kodu, niektóre z nich nie wiedzą, że jest to również niezwykle przydatne do debugowania kodu po stronie klienta, takiego jak JavaScript. Ten sam typ technik służący do debugowania aplikacji .NET może być również stosowany do aplikacji obsługujących technologię AJAX i bardziej szczegółowych aplikacji ASP.NET AJAX.

W tym artykule dowiesz się, jak program Visual Studio 2008 i kilka innych narzędzi mogą służyć do debugowania aplikacji ASP.NET AJAX, aby szybko lokalizować usterki i inne problemy. W tej dyskusji zawarto informacje na temat włączania programu Internet Explorer 6 lub nowszego do debugowania przy użyciu programu Visual Studio 2008 i Eksploratora skryptów do przechodzenia przez kod oraz używania innych bezpłatnych narzędzi, takich jak pomocnik programowania w sieci Web. Dowiesz się również, jak debugować aplikacje ASP.NET AJAX w programie Firefox przy użyciu rozszerzenia o nazwie Firebug, które umożliwia przechodzenie przez kod JavaScript bezpośrednio w przeglądarce bez żadnych innych narzędzi. Na koniec nastąpi wprowadzenie do klas w bibliotece ASP.NET AJAX, które mogą pomóc w różnych zadaniach debugowania, takich jak śledzenie i instrukcje potwierdzeń kodu.

Przed podjęciem próby debugowania stron wyświetlanych w programie Internet Explorer należy wykonać kilka podstawowych kroków, aby umożliwić debugowanie. Zapoznaj się z pewnymi podstawowymi wymaganiami dotyczącymi instalacji, które należy wykonać, aby rozpocząć pracę.

## <a name="configuring-internet-explorer-for-debugging"></a>Konfigurowanie programu Internet Explorer do debugowania

Większość osób nie chce widzieć problemów dotyczących języka JavaScript występujących w witrynie sieci Web wyświetlanej w programie Internet Explorer. W rzeczywistości przeciętny użytkownik nie mógł nawet wiedzieć, co należy zrobić, jeśli wystąpił komunikat o błędzie. W związku z tym opcje debugowania są domyślnie wyłączone w przeglądarce. Jednak bardzo proste jest włączenie debugowania i umieszczenie go do użycia podczas tworzenia nowych aplikacji AJAX.

Aby włączyć funkcje debugowania, przejdź do pozycji Narzędzia Opcje internetowe w menu programu Internet Explorer i wybierz kartę Zaawansowane. Upewnij się, że w sekcji Przeglądanie nie są zaznaczone następujące elementy:

- Wyłącz debugowanie skryptu (Internet Explorer)
- Wyłącz debugowanie skryptu (inne)

Chociaż nie jest to wymagane, jeśli próbujesz debugować aplikację, prawdopodobnie wszystkie błędy JavaScript na stronie będą natychmiast widoczne i oczywiste. Wszystkie błędy można wymusić, aby wyświetlić okno komunikatu, zaznaczając pole wyboru "Wyświetl powiadomienie dotyczące każdego błędu skryptu". Chociaż jest to świetna opcja, którą można włączyć podczas tworzenia aplikacji, może ona szybko stać się irytujący, jeśli jesteś w stanie nieco innych witryn internetowych, ponieważ nie ma możliwości napotkania błędów języka JavaScript.

Rysunek 1 pokazuje, co powinno wyglądać okno dialogowe Zaawansowane programu Internet Explorer po poprawnym skonfigurowaniu debugowania.

[![konfigurowania programu Internet Explorer na potrzeby debugowania.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Rysunek 1**. Konfigurowanie programu Internet Explorer do debugowania.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))

Po włączeniu debugowania zobaczysz nowy element menu w menu Widok o nazwie debuger skryptów. Dostępne są dwie opcje, w tym otwieranie i przerywanie w następnej instrukcji. Po wybraniu tej usługi zostanie wyświetlony monit o debugowanie strony w programie Visual Studio 2008 (Zwróć uwagę, że program Visual Web Developer Express może być również używany do debugowania). Jeśli program Visual Studio .NET jest aktualnie uruchomiony, możesz wybrać użycie tego wystąpienia lub utworzyć nowe wystąpienie. Po wybraniu pozycji Przerwij przy następnej instrukcji zostanie wyświetlony monit o debugowanie strony podczas wykonywania kodu JavaScript. Jeśli kod JavaScript jest wykonywany w zdarzeniu OnLoad strony, można odświeżyć stronę, aby wyzwolić sesję debugowania. Jeśli kod JavaScript jest uruchamiany po kliknięciu przycisku, debuger zostanie uruchomiony natychmiast po kliknięciu przycisku.

> [!NOTE]
> Jeśli korzystasz z systemu Windows Vista z włączoną funkcją User Access Control (UAC) i masz program Visual Studio 2008 ustawiony do uruchamiania jako administrator, program Visual Studio nie będzie mógł dołączyć do procesu po wyświetleniu monitu o dołączenie. Aby obejść ten problem, najpierw uruchom program Visual Studio i Użyj tego wystąpienia do debugowania.

Mimo że w następnej sekcji pokazano, jak debugować stronę ASP.NET AJAX bezpośrednio z poziomu programu Visual Studio 2008 przy użyciu opcji debuger skryptów programu Internet Explorer jest przydatne, gdy strona jest już otwarta i chcesz dokładniej ją sprawdzić.

## <a name="debugging-with-visual-studio-2008"></a>Debugowanie za pomocą programu Visual Studio 2008

Program Visual Studio 2008 udostępnia funkcje debugowania, które deweloperzy na całym świecie wykorzystują codziennie do debugowania aplikacji .NET. Wbudowany debuger umożliwia przechodzenie przez kod, wyświetlanie danych obiektów, śledzenie określonych zmiennych, monitorowanie stosu wywołań i wiele innych. Oprócz debugowania VB.NET lub C# kodu debuger jest również przydatny do debugowania aplikacji ASP.NET AJAX i umożliwi przechodzenie między wierszami kodu JavaScript. Szczegóły, które są zgodne z technikami, które mogą służyć do debugowania plików skryptów po stronie klienta, a nie do przekazywania ogólnego procesu debugowania aplikacji przy użyciu programu Visual Studio 2008.

Proces debugowania strony w programie Visual Studio 2008 można uruchomić na kilka różnych sposobów. Najpierw można użyć opcji debugera skryptów programu Internet Explorer wymienionej w poprzedniej sekcji. Jest to dobre rozwiązanie, gdy strona jest już załadowana w przeglądarce i chcesz rozpocząć debugowanie. Alternatywnie możesz kliknąć prawym przyciskiem myszy stronę. aspx w Eksplorator rozwiązań i wybrać pozycję Ustaw jako stronę początkową z menu. Jeśli jesteś przyzwyczajony do debugowania stron ASP.NET, możesz to zrobić wcześniej. Po naciśnięciu klawisza F5 strona może być debugowana. Mimo że można ogólnie ustawić punkt przerwania w dowolnym miejscu w VB.NET lub C# kodzie, to nie zawsze dzieje się tak w przypadku języka JavaScript, jak widać dalej.

*Osadzony w porównaniu z zewnętrznymi skryptami*

Debuger programu Visual Studio 2008 traktuje kod JavaScript osadzony na stronie innej niż zewnętrzne pliki JavaScript. Za pomocą zewnętrznych plików skryptu można otworzyć plik i ustawić punkt przerwania w dowolnym wierszu. Punkty przerwania można ustawić, klikając obszar szary zasobnik z lewej strony okna edytora kodu. Gdy kod JavaScript jest osadzony bezpośrednio na stronie przy użyciu znacznika `<script>`, ustawienie punktu przerwania przez kliknięcie w obszarze zasobnik szary nie jest opcją. Próba ustawienia punktu przerwania w wierszu osadzonego skryptu spowoduje ostrzeżenie informujące o tym, że to nie jest prawidłowa Lokalizacja punktu przerwania.

Aby obejść ten problem, można przenieść kod do zewnętrznego pliku. js i odwołać się do niego przy użyciu atrybutu src tagu &lt;skryptu&gt;:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Co zrobić, jeśli przeniesienie kodu do pliku zewnętrznego nie jest opcją lub wymaga więcej pracy niż warto? Chociaż nie można ustawić punktu przerwania przy użyciu edytora, można dodać instrukcję debugera bezpośrednio do kodu, w którym chcesz rozpocząć debugowanie. Można również użyć klasy sys. Debug dostępnej w bibliotece ASP.NET AJAX, aby wymusić uruchomienie debugowania. Więcej informacji na temat klasy sys. Debug znajdziesz w dalszej części tego artykułu.

Przykład użycia słowa kluczowego `debugger` jest pokazywany na liście 1. Ten przykład wymusza przerwanie działania debugera bezpośrednio przed wywołaniem funkcji aktualizacji.

**Lista 1. Za pomocą słowa kluczowego Debugger, aby wymusić przerwanie debugera programu Visual Studio .NET.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Po osiągnięciu instrukcji debugera zostanie wyświetlony monit o debugowanie strony przy użyciu programu Visual Studio .NET i można zacząć przechodzenie przez kod. Podczas wykonywania tej czynności może wystąpić problem z uzyskaniem dostępu do ASP.NET plików skryptów biblioteki AJAX używanych na stronie, dlatego Przyjrzyjmy się użyciu programu Visual Studio. Eksplorator skryptów sieci.

## <a name="using-visual-studio-net-windows-to-debug"></a>Korzystanie z programu Visual Studio .NET Windows do debugowania

Po rozpoczęciu sesji debugowania i rozpoczęciu pracy przez kod przy użyciu domyślnego klawisza F11 może wystąpić okno dialogowe błędu wyświetlane na rysunku 2, chyba że wszystkie pliki skryptów używane na stronie są otwarte i dostępne do debugowania.

[okno dialogowe błędu ![wyświetlane, gdy dla debugowania nie jest dostępny żaden kod źródłowy.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Rysunek 2**. okno dialogowe błędu wyświetlane, gdy dla debugowania nie jest dostępny żaden kod źródłowy.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))

To okno dialogowe jest wyświetlane, ponieważ program Visual Studio .NET nie ma pewności, jak uzyskać kod źródłowy niektórych skryptów, do których odwołuje się Strona. Chociaż może to być dość frustrujące w pierwszej kolejności, istnieje prosta poprawka. Po rozpoczęciu sesji debugowania i osiągnięciu punktu przerwania przejdź do okna debugowanie skryptów systemu Windows w menu programu Visual Studio 2008 lub użyj klawiszy Ctrl + Alt + N.

> [!NOTE]
> Jeśli na liście nie jest widoczne menu Eksplorator skryptów, przejdź do pozycji **narzędzia** > **Dostosuj** > **polecenia** w menu programu Visual Studio .NET. Znajdź wpis **Debug** w sekcji kategorii i kliknij go, aby wyświetlić wszystkie dostępne pozycje menu. Na liście poleceń przewiń w dół do Eksploratora skryptów, a następnie przeciągnij go do menu Debuguj okna, wymienione wcześniej. Spowoduje to udostępnienie pozycji menu Eksploratora skryptów przy każdym uruchomieniu programu Visual Studio .NET.

Eksplorator skryptów może służyć do wyświetlania wszystkich skryptów używanych na stronie i otwierania ich w edytorze kodu. Po otwarciu Eksploratora skryptów kliknij dwukrotnie stronę. aspx, która jest aktualnie debugowana, aby otworzyć ją w oknie Edytor kodu. Wykonaj tę samą akcję dla wszystkich innych skryptów przedstawionych w Eksploratorze skryptów. Gdy wszystkie skrypty są otwarte w oknie kodu, możesz nacisnąć klawisz F11 (i użyć innych klawiszy skrótu debugowania), aby przejść przez swój kod. Rysunek 3 przedstawia przykład Eksploratora skryptów. Zawiera listę aktualnie debugowanego pliku (demonstracyjn. aspx), a także dwa skrypty niestandardowe i dwa skrypty wprowadzane dynamicznie do strony przez ScriptManager ASP.NET AJAX.

[![Eksplorator skryptów zapewnia łatwy dostęp do skryptów używanych na stronie.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Rysunek 3**. Eksplorator skryptów zapewnia łatwy dostęp do skryptów używanych na stronie.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))

Można również użyć kilku innych okien, aby dostarczyć przydatne informacje podczas przechodzenia przez kod na stronie. Można na przykład użyć okna zmienne lokalne, aby wyświetlić wartości różnych zmiennych, które są używane na stronie, bezpośrednie okno do szacowania określonych zmiennych lub warunków i wyświetlenia danych wyjściowych. Można również użyć okna danych wyjściowych, aby wyświetlić instrukcje śledzenia, które są zapisywane przy użyciu funkcji sys. Debug. Trace (która zostanie omówiona w dalszej części tego artykułu) lub funkcji Debug. writeln programu Internet Explorer.

Podczas przechodzenia przez kod przy użyciu debugera można przejść przez zmienne w kodzie, aby wyświetlić wartość, którą są przypisane. Jednak debuger skryptów czasami nie pokaże niczego podczas przesuwania wskaźnika myszy nad daną zmienną JavaScript. Aby wyświetlić wartość, zaznacz instrukcję lub zmienną, którą próbujesz wyświetlić w oknie edytora kodu, a następnie nad nią. Chociaż ta technika nie działa w każdej sytuacji, wiele razy będzie można zobaczyć wartość bez konieczności wyszukiwania w innym oknie debugowania, takim jak okno lokalne.

Samouczek wideo przedstawiający niektóre funkcje omówione w tym miejscu można wyświetlić w [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Debugowanie za pomocą pomocnika projektowania sieci Web

Mimo że program Visual Studio 2008 (i Visual Web Developer Express 2008) jest bardzo obsługujący narzędzia debugowania, dostępne są również dodatkowe opcje, które mogą być bardziej lekkie. Jednym z najnowszych narzędzi do zwolnienia jest pomocnik programowania w sieci Web. Nikhil Kothari firmy Microsoft (jeden z najważniejszych architektów AJAX firmy Microsoft) zapisał to doskonałe narzędzie, które może wykonywać wiele różnych zadań z prostego debugowania do wyświetlania komunikatów żądań i odpowiedzi HTTP. Pomocnika projektowania sieci Web można pobrać w [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Pomocnika programistycznego dla sieci Web może być używana bezpośrednio w programie Internet Explorer, co ułatwia korzystanie z programu. Jest ona uruchamiana, wybierając pozycję Narzędzia Web Development Helper z menu programu Internet Explorer. Spowoduje to otwarcie narzędzia w dolnej części przeglądarki, która jest świetna, ponieważ nie trzeba opuszczać przeglądarki do wykonywania kilku zadań, takich jak żądanie HTTP i rejestrowanie komunikatów w wiadomościach. Rysunek 4 przedstawia informacje o tym, co pomocnik programowania w sieci Web wygląda jak w działaniu.

[Pomocnik programowania w sieci Web ![](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Ilustracja 4**. pomocnik programowania w sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))

Pomocnik programowania w sieci Web nie jest narzędziem, które zostanie użyte do przechodzenia przez wiersz kodu w wierszu jak w przypadku programu Visual Studio 2008. Można go jednak użyć do wyświetlania danych wyjściowych śledzenia, łatwej do obliczenia zmiennych w skrypcie lub eksplorowania danych w obiekcie JSON. Jest to również bardzo przydatne do wyświetlania danych, które są przesyłane do i ze strony ASP.NET AJAX i serwera.

Gdy pomocnik programowania w sieci Web jest otwarty w programie Internet Explorer, debugowanie skryptów musi być włączone, wybierając pozycję skrypt Włącz debugowanie skryptów z menu pomocnika projektowania sieci Web, jak pokazano wcześniej na rysunku 4. Dzięki temu narzędzie może przechwycić błędy, które wystąpiły, gdy strona jest uruchamiana. Umożliwia również łatwy dostęp do komunikatów śledzenia, które są wyprowadzane na stronie. Aby wyświetlić informacje o śledzeniu lub wykonać polecenia skryptu w celu przetestowania różnych funkcji na stronie, wybierz pozycję skrypt Pokaż konsolę skryptu z menu pomocnika opracowywania aplikacji sieci Web. Zapewnia to dostęp do okna poleceń i prostego okna bezpośredniego.

*Wyświetlanie komunikatów śledzenia i danych obiektów JSON*

Okno bezpośrednie może służyć do wykonywania poleceń skryptu, a nawet ładowania lub zapisywania skryptów, które są używane do testowania różnych funkcji na stronie. W oknie polecenia są wyświetlane komunikaty śledzenia lub debugowania wypisywane przez przeglądaną stronę. Lista 2 pokazuje, jak napisać komunikat śledzenia przy użyciu funkcji Debug. writeln programu Internet Explorer.

**Lista 2. Zapisywanie komunikatu śledzenia po stronie klienta przy użyciu klasy Debug.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Jeśli właściwość LastName zawiera wartość Nowak, pomocnik programowania w sieci Web wyświetli komunikat "nazwisko osoby: Nowak" w oknie poleceń konsoli skryptu (przy założeniu, że debugowanie zostało włączone). Pomocnik programowania w sieci Web dodaje również obiekt debugService najwyższego poziomu do stron, których można użyć do zapisu informacji śledzenia lub wyświetlania zawartości obiektów JSON. Lista 3 przedstawia przykład użycia funkcji śledzenia klasy debugService.

**Lista 3. Za pomocą klasy debugService pomocnika projektowania sieci Web, aby napisać komunikat śledzenia.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Całkiem funkcja klasy debugService działa nawet wtedy, gdy debugowanie nie jest włączone w programie Internet Explorer, co ułatwia dostęp do danych śledzenia, gdy działa pomocnik programowania w sieci Web. Gdy narzędzie nie jest używane do debugowania strony, instrukcje śledzenia zostaną zignorowane, ponieważ wywołanie metody window. debugService zwróci wartość false.

Klasa debugService umożliwia również wyświetlanie danych obiektów JSON przy użyciu okna inspektora narzędzia Web Development. Lista 4 tworzy prosty obiekt JSON zawierający dane osobowe. Po utworzeniu obiektu zostanie wykonane wywołanie funkcji inspekcji klasy debugService, aby umożliwić wizualizację obiektu JSON.

**Lista 4. Używanie funkcji debugService. inspekcji do wyświetlania danych obiektu JSON.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Wywołanie funkcji GetPerson () na stronie lub w oknie bezpośrednim spowoduje wyświetlenie okna dialogowego Inspektor obiektów, jak pokazano na rysunku 5. Właściwości w obiekcie można zmienić dynamicznie, zaznaczając je, zmieniając wartość wyświetlaną w polu tekstowym wartość, a następnie klikając link Aktualizuj. Korzystanie z Inspektora obiektów ułatwia wyświetlanie danych obiektów JSON i eksperymentowanie z zastosowaniem różnych wartości do właściwości.

*Błędy debugowania*

Oprócz umożliwienia wyświetlania danych śledzenia i obiektów JSON, pomocnik programowania internetowego może również pomóc w debugowaniu błędów na stronie. Jeśli wystąpi błąd, zostanie wyświetlony monit o przejście do następnego wiersza kodu lub debugowanie skryptu (patrz rysunek 6). W oknie dialogowym błąd skryptu są wyświetlane wszystkie kompletne stosy wywołań, a także numery wierszy, dzięki czemu można łatwo identyfikować, gdzie występują problemy w skrypcie.

[![przy użyciu okna Inspektor obiektów do wyświetlania obiektu JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Rysunek 5**. Używanie okna Inspektor obiektów do wyświetlania obiektu JSON.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))

Wybranie opcji Debuguj umożliwia wykonywanie instrukcji skryptu bezpośrednio w oknie pomocnika projektowania aplikacji sieci Web w celu wyświetlenia wartości zmiennych, zapisania obiektów JSON i więcej. Jeśli ta sama akcja, która wyzwoliła błąd jest wykonywana ponownie, a na komputerze jest dostępny program Visual Studio 2008, zostanie wyświetlony monit o rozpoczęcie sesji debugowania, aby można było przejść przez wiersz kodu, zgodnie z opisem w poprzedniej sekcji.

[Okno dialogowe błędu skryptu pomocnika projektowania sieci Web ![](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Ilustracja 6**. okno dialogowe błędu skryptu pomocnika projektowania sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))

*Sprawdzanie komunikatów żądań i odpowiedzi*

Podczas debugowania ASP.NET strony AJAX często warto zobaczyć komunikaty żądań i odpowiedzi wysyłane między stroną a serwerem. Wyświetlanie zawartości w ramach komunikatów pozwala sprawdzić, czy są przesyłane odpowiednie dane, a także rozmiar komunikatów. Pomocnik programowania w sieci Web udostępnia doskonałą funkcję rejestratora komunikatów HTTP, która ułatwia wyświetlanie danych jako nieprzetworzonego tekstu lub w bardziej czytelnym formacie.

Aby wyświetlić żądania ASP.NET i odpowiedzi AJAX, należy włączyć rejestrator HTTP, wybierając pozycję HTTP Włącz rejestrowanie HTTP z menu pomocnika opracowywania aplikacji sieci Web. Po włączeniu wszystkie komunikaty wysyłane z bieżącej strony można wyświetlić w przeglądarce dzienników HTTP, do której można uzyskać dostęp, wybierając pozycję HTTP Pokaż dzienniki HTTP.

Chociaż wyświetlanie nieprzetworzonego tekstu wysyłanego w poszczególnych komunikatach żądania/odpowiedzi jest szczególnie przydatne (i opcji pomocnika projektowania sieci Web), często łatwiej jest wyświetlać dane komunikatów w bardziej graficznym formacie. Po włączeniu rejestrowania HTTP i zarejestrowaniu wiadomości można wyświetlić dane komunikatów, klikając dwukrotnie wiadomość w przeglądarce dzienników HTTP. To umożliwia wyświetlenie wszystkich nagłówków skojarzonych z wiadomością oraz rzeczywistej zawartości wiadomości. Rysunek 7 przedstawia przykład komunikatu żądania i komunikatu odpowiedzi wyświetlanego w oknie przeglądarki dzienników HTTP.

[Aby wyświetlić dane żądania i komunikatów odpowiedzi, ![przy użyciu przeglądarki dzienników HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Rysunek 7**. Wyświetlanie danych żądań i komunikatów odpowiedzi przy użyciu przeglądarki dzienników http.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))

Przeglądarka dzienników HTTP automatycznie analizuje obiekty JSON i wyświetla je przy użyciu widoku drzewa, dzięki czemu można szybko i łatwo wyświetlać dane właściwości obiektu. Gdy element UpdatePanel jest używany na stronie ASP.NET AJAX, przeglądarka dzieli każdą część komunikatu na poszczególne części, jak pokazano na rysunku 8. Jest to świetna funkcja, która znacznie ułatwia przeglądanie komunikatów i zrozumienie ich w porównaniu do wyświetlania nieprzetworzonych danych komunikatów.

[![komunikat odpowiedzi elementu UpdatePanel wyświetlany przy użyciu przeglądarki dzienników HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Rysunek 8**: komunikat odpowiedzi elementu UpdatePanel wyświetlany przy użyciu przeglądarki dzienników http.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))

Istnieje kilka innych narzędzi, których można użyć do wyświetlania komunikatów żądań i odpowiedzi oprócz pomocnika programowania w sieci Web. Kolejną dobrą opcją jest programu Fiddler, która jest dostępna bezpłatnie w [http://www.fiddlertool.com](http://www.fiddlertool.com). Chociaż programu Fiddler nie zostanie omówiona w tym miejscu, jest również dobrym rozwiązaniem, gdy konieczne jest dokładne sprawdzenie nagłówków i danych komunikatów.

## <a name="debugging-with-firefox-and-firebug"></a>Debugowanie za pomocą przeglądarki Firefox i Firebug

Mimo że program Internet Explorer jest nadal najczęściej używaną przeglądarką, inne przeglądarki, takie jak Firefox, staną się dość popularne i są używane więcej i więcej. W efekcie warto wyświetlać i debugować strony ASP.NET AJAX w programie Firefox oraz Internet Explorer, aby zapewnić prawidłowe działanie aplikacji. Chociaż Firefox nie może bezpośrednio łączyć się z programu Visual Studio 2008 na potrzeby debugowania, ma rozszerzenie o nazwie Firebug, które może służyć do debugowania stron. Firebug można pobrać bezpłatnie, przechodząc do [http://www.getfirebug.com](http://www.getfirebug.com).

Firebug zapewnia w pełni funkcjonalne środowisko debugowania, które może służyć do przechodzenia przez wiersz kodu według linii, uzyskiwania dostępu do wszystkich skryptów używanych na stronie, wyświetlania struktur modelu DOM, wyświetlania stylów CSS, a nawet śledzenia zdarzeń, które pojawiają się na stronie. Po zainstalowaniu programu Firebug można uzyskać dostęp, wybierając pozycję Narzędzia Firebug Otwórz Firebug w menu Firefox. Podobnie jak pomocnik programowania w sieci Web, Firebug jest używana bezpośrednio w przeglądarce, chociaż może być również używana jako aplikacja autonomiczna.

Po uruchomieniu Firebug punkty przerwania można ustawić w dowolnym wierszu pliku JavaScript, niezależnie od tego, czy skrypt jest osadzony na stronie, czy nie. Aby ustawić punkt przerwania, najpierw Załaduj odpowiednią stronę, którą chcesz debugować w przeglądarce Firefox. Po załadowaniu strony wybierz skrypt do debugowania z listy rozwijanej skrypty Firebug. Zostaną wyświetlone wszystkie skrypty używane przez stronę. Punkt przerwania jest ustawiany przez kliknięcie w obszarze szarego zasobnika Firebug w wierszu, w którym punkt przerwania powinien wyglądać tak jak w programie Visual Studio 2008.

Po ustawieniu punktu przerwania w Firebug można wykonać akcję wymaganą do wykonania skryptu, który musi być debugowany, na przykład klikając przycisk lub odświeżając przeglądarkę w celu wyzwolenia zdarzenia OnLoad. Wykonanie zostanie automatycznie zatrzymane w wierszu zawierającym punkt przerwania. Na rysunku nr 9 przedstawiono przykład punktu przerwania, który został wyzwolony w Firebug.

[![obsługi punktów przerwania w Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Rysunek 9**. Obsługa punktów przerwania w Firebug.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))

Po osiągnięciu punktu przerwania możesz wkroczyć lub przekroczyć kod za pomocą przycisków strzałek. Po przejściu przez kod zmienne skryptów są wyświetlane w prawej części debugera, co pozwala na wyświetlanie wartości i przechodzenie do szczegółów w obiektach. Firebug zawiera również listę rozwijaną stos wywołań, aby wyświetlić kroki wykonywania skryptu, które doprowadziły do debugowanego wiersza.

Firebug zawiera również okno konsoli, które może służyć do testowania różnych instrukcji skryptu, obliczania zmiennych i wyświetlania wyników śledzenia. Dostęp do niego można uzyskać, klikając kartę konsoli w górnej części okna Firebug. Debugowaną stronę można także sprawdzić, aby zobaczyć jej strukturę i zawartość DOM, klikając kartę Inspekcja. Po umieszczeniu wskaźnika myszy nad różnymi elementami modelu DOM wyświetlanymi w oknie Inspektora odpowiednia część strony zostanie wyróżniona, co ułatwia sprawdzanie, gdzie element jest używany na stronie. Wartości atrybutów skojarzonych z danym elementem można zmienić na "Live", aby eksperymentować z zastosowaniem różnych szerokości, stylów itp. do elementu. Jest to świetna funkcja, która powoduje, że nie trzeba stale przełączać się między edytorem kodu źródłowego a przeglądarką Firefox, aby zobaczyć, jak proste zmiany wpływają na stronę.

Na rysunku nr 10 przedstawiono przykład używania inspektora DOM do lokalizowania pola tekstowego o nazwie txtCountry na stronie. Inspektora Firebug można także użyć do wyświetlania stylów CSS używanych na stronie, jak również zdarzeń, które wystąpiły, takich jak śledzenie ruchów myszy, kliknięć przycisków i więcej.

[![za pomocą Inspektora DOM Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Ilustracja 10**. Używanie inspektora dom Firebug.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))

Firebug zapewnia lekki sposób na szybkie debugowanie strony bezpośrednio w przeglądarce Firefox oraz doskonałe narzędzie do sprawdzania różnych elementów na stronie.

## <a name="debugging-support-in-aspnet-ajax"></a>Obsługa debugowania w ASP.NET AJAX

Biblioteka ASP.NET AJAX zawiera wiele różnych klas, których można użyć do uproszczenia procesu dodawania funkcji AJAX do strony sieci Web. Za pomocą tych klas można zlokalizować elementy na stronie i manipulować nimi, dodawać nowe kontrolki, wywoływać usługi sieci Web i nawet obsługiwać zdarzenia. Biblioteka ASP.NET AJAX zawiera również klasy, których można użyć do usprawnienia procesu debugowania stron. W tej sekcji należy wprowadzić do klasy sys. Debug i zobaczyć, jak można jej używać w aplikacjach.

*Korzystanie z klasy sys. Debug*

Klasa sys. Debug (Klasa JavaScript znajdująca się w przestrzeni nazw sys) może służyć do wykonywania kilku różnych funkcji, w tym pisania danych wyjściowych śledzenia, wykonywania potwierdzeń kodu i wymuszania błędu, aby można było go debugować. Jest ona używana w szerokim stopniu w plikach debugowania biblioteki AJAX ASP.NET (domyślnie instalowanych w katalogu C:\Program Files\Microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) w celu wykonania testów warunkowych ( wywołane potwierdzenia), które gwarantują prawidłowe przekazywanie parametrów do funkcji, obiekty te zawierają oczekiwane dane i zapisują instrukcje śledzenia.

Klasa sys. Debug udostępnia kilka różnych funkcji, których można użyć do obsługi śledzenia, potwierdzeń kodu lub niepowodzeń, jak pokazano w tabeli 1.

**Tabela 1. Sys. Debug — funkcje klas.**

| **Nazwa funkcji** | **Opis** |
| --- | --- |
| Assert (warunek, komunikat, displayCaller) | Potwierdza, że parametr condition ma wartość true. Jeśli testowany warunek ma wartość false, do wyświetlenia wartości parametru komunikatu zostanie użyte okno komunikatu. Jeśli parametr displayCaller ma wartość true, Metoda również wyświetla informacje o wywołującym. |
| clearTrace() | Wymazuje dane wyjściowe instrukcji z operacji śledzenia. |
| Niepowodzenie (komunikat) | Powoduje zatrzymanie wykonywania i przerwanie działania programu Debugger. Parametr Message może służyć do zapewnienia przyczyny niepowodzenia. |
| Trace (komunikat) | Zapisuje parametr komunikatu w danych wyjściowych śledzenia. |
| traceDump (obiekt, nazwa) | Wyprowadza dane obiektu w formacie możliwym do odczytu. Parametru name można użyć, aby podać etykietę zrzutu śledzenia. Wszystkie obiekty podrzędne w ramach zrzutu obiektu będą zapisywane domyślnie. |

Śledzenie po stronie klienta może być używane w podobny sposób, jak funkcja śledzenia dostępna w ASP.NET. Dzięki temu różne komunikaty mogą być łatwo widoczne bez zakłócania przepływu aplikacji. Lista 5 zawiera przykład użycia funkcji sys. Debug. Trace do zapisywania w dzienniku śledzenia. Ta funkcja po prostu przyjmuje komunikat, który powinien zostać zapisany jako parametr.

**Lista 5. Za pomocą funkcji sys. Debug. Trace.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

W przypadku uruchomienia kodu pokazanego na liście 5 nie zobaczysz żadnych danych wyjściowych śledzenia na stronie. Jedynym sposobem, aby zobaczyć, jest korzystanie z okna konsoli dostępnego w programie Visual Studio .NET, Pomocniku projektowania sieci Web lub Firebug. Jeśli chcesz zobaczyć dane wyjściowe śledzenia na stronie, musisz dodać tag TextArea i nadać mu identyfikator TraceConsole, jak pokazano poniżej:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Wszystkie instrukcje sys. Debug. Trace na stronie zostaną zazapisywane w obszarze TraceConsole TextArea.

W przypadkach, w których chcesz zobaczyć dane zawarte w obiekcie JSON, można użyć funkcji traceDump klasy sys. Debug. Ta funkcja przyjmuje dwa parametry, łącznie z obiektem, który powinien być zrzucany do konsoli śledzenia, oraz nazwę, która może służyć do identyfikowania obiektu w danych wyjściowych śledzenia. Lista 6 pokazuje przykład użycia funkcji traceDump.

**Lista 6. Za pomocą funkcji sys. Debug. traceDump.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Rysunek 11 przedstawia dane wyjściowe wywołania funkcji sys. Debug. traceDump. Należy zauważyć, że oprócz pisania danych obiektu osoba zapisuje również dane obiektu podrzędnego adresu.

Oprócz śledzenia, Klasa sys. Debug może być również używana do wykonywania potwierdzeń kodu. Potwierdzenia są używane do testowania, że określone warunki są spełnione, gdy aplikacja jest uruchomiona. Wersja do debugowania skryptów biblioteki AJAX ASP.NET zawiera kilka instrukcji Assert do testowania różnych warunków.

Lista 7 zawiera przykład użycia funkcji sys. Debug. Assert do testowania warunku. Kod sprawdza, czy obiekt adresu ma wartość null przed aktualizacją obiektu osoby.

[![dane wyjściowe funkcji sys. Debug. traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Ilustracja 11**. dane wyjściowe funkcji sys. Debug. traceDump.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))

**Lista 7. Za pomocą funkcji Debug. Assert.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Są przesyłane trzy parametry, w tym warunek do obliczenia, komunikat, który ma być wyświetlany, jeśli potwierdzenie zwróci wartość false i wskazuje, czy mają być wyświetlane informacje o wywołującym. W przypadkach, gdy potwierdzenie nie powiedzie się, zostanie wyświetlony komunikat, a także informacje o wywołującym, jeśli trzeci parametr był prawdziwy. Na rysunku 12 przedstawiono przykład okna dialogowego błędu, które pojawia się, jeśli potwierdzenie pokazane na liście 7 nie powiedzie się.

Końcową funkcją do pokrycia jest sys. Debug. Niepowodzenie. Jeśli chcesz wymusić niepowodzenie kodu w konkretnym wierszu skryptu, możesz dodać metodę sys. Debug. Fail zamiast instrukcji debugera zwykle używanej w aplikacjach JavaScript. Funkcja sys. Debug. Failure akceptuje pojedynczy parametr ciągu, który reprezentuje przyczynę niepowodzenia, jak pokazano poniżej:

[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]

[![komunikat o niepowodzeniu pliku sys. Debug. Assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Ilustracja 12**. komunikat o błędzie: sys. Debug. Assert.  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))

Po napotkaniu instrukcji sys. Debug. Fail podczas wykonywania skryptu, wartość parametru komunikatu będzie wyświetlana w konsoli aplikacji debugowania, takiej jak Visual Studio 2008, i pojawi się monit o debugowanie aplikacji. Jednym z przypadków, gdy może to być bardzo przydatne, gdy nie można ustawić punktu przerwania w programie Visual Studio 2008 na skrypcie wbudowanym, ale kod powinien zostać zatrzymany w określonym wierszu, aby można było sprawdzić wartość zmiennych.

*Opis właściwości ScriptMode kontrolki ScriptManager*

Biblioteka ASP.NET AJAX obejmuje wersje skryptów Debug i Release, które są domyślnie instalowane w katalogu C:\Program Files\Microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0. Skrypty debugowania są sformatowane w dobrze, łatwe do odczytania i mają kilka sys. Debug. w tym przypadku wywołania Assert są rozłożone na siebie, podczas gdy skrypty wydania mają odstępy odłączone i użycie klasy sys. Debug jest oszczędne w celu zminimalizowania ich całkowitego rozmiaru.

Kontrolka ScriptManager dodana do stron ASP.NET AJAX odczytuje atrybut debugowania elementu kompilacji w pliku Web. config, aby określić, które wersje skryptów biblioteki mają zostać załadowane. Można jednak kontrolować, czy skrypty debugowania lub zwolnienia są ładowane (skrypty biblioteki lub własne skrypty niestandardowe), zmieniając właściwość ScriptMode. Skryptmode akceptuje Wyliczenie ScriptMode, którego członkowie zawierają funkcję Auto, Debug, Release i Inherit.

Wartość domyślna dla elementu ScriptMode jest wartością domyślną, co oznacza, że element ScriptManager sprawdzi atrybut Debug w pliku Web. config. Gdy debug ma wartość false, element ScriptManager załaduje wersję publikacji ASP.NET AJAX Library scripts. Gdy debugowanie jest prawdziwe, wersja debugowana skryptów zostanie załadowana. Zmiana właściwości ScriptMode na Release lub debug spowoduje wymuszenie załadowania odpowiednich skryptów przez element ScriptManager, niezależnie od tego, jaka wartość atrybutu Debug znajduje się w pliku Web. config. Lista 8 pokazuje przykład użycia formantu ScriptManager do ładowania skryptów debugowania z biblioteki ASP.NET AJAX.

**Lista 8. Ładowanie skryptów debugowania przy użyciu ScriptManager**.

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Możesz również ładować różne wersje (debugowanie lub wydanie) własnych skryptów niestandardowych przy użyciu właściwości scripts ScriptManager wraz ze składnikiem odwołanie do skryptu, jak pokazano na liście 9.

**Lista 9. Ładowanie skryptów niestandardowych przy użyciu ScriptManager.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> W przypadku ładowania skryptów niestandardowych przy użyciu składnika odwołanie do skryptu należy powiadomić element ScriptManager, gdy skrypt zakończył ładowanie, dodając następujący kod u dołu skryptu:

[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Kod wyświetlany na liście 9 informuje element ScriptManager o wyszukiwaniu wersji debugowania skryptu osoby, aby automatycznie wyszukać osobę. Debug. js zamiast Person. js. Jeśli plik Person. Debug. js nie zostanie znaleziony, zostanie zgłoszony błąd.

W przypadkach, gdy ma być załadowana wersja Debug lub Release niestandardowego skryptu na podstawie wartości właściwości ScriptMode ustawionej w formancie ScriptManager, można ustawić właściwość ScriptMode formantu odwołanie do skryptu, która ma dziedziczyć. Spowoduje to załadowanie właściwej wersji skryptu niestandardowego na podstawie właściwości ScriptMode elementu ScriptManager, jak pokazano na liście 10. Ponieważ właściwość ScriptMode kontrolki ScriptManager jest ustawiona na Debuguj, skrypt osoby. Debug. js zostanie załadowany i użyty na stronie.

**Lista 10. Dziedziczenie elementu ScriptMode z ScriptManager dla skryptów niestandardowych.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Odpowiednio przy użyciu właściwości ScriptMode można łatwiej debugować aplikacje i uprościć cały proces. Skrypty wydania biblioteki AJAX ASP.NET są raczej trudne do przechodzenia i odczytywane, ponieważ formatowanie kodu zostało usunięte, podczas gdy skrypty debugowania są sformatowane do celów debugowania.

## <a name="conclusion"></a>Wniosek

Technologia ASP.NET AJAX firmy Microsoft zapewnia solidną podstawę do tworzenia aplikacji obsługujących technologię AJAX, które mogą ulepszyć ogólne środowisko użytkownika końcowego. Jednak podobnie jak w przypadku wszelkich technologii programowania, błędy i inne problemy z aplikacjami będą miały miejsce. Informacje o różnych dostępnych opcjach debugowania mogą zaoszczędzić dużo czasu i spowodować bardziej stabilny produkt.

W tym artykule wprowadzono kilka różnych technik debugowania stron ASP.NET AJAX, w tym programu Internet Explorer z programem Visual Studio 2008, pomocnika projektowania sieci Web i Firebug. Te narzędzia mogą uprościć ogólny proces debugowania, ponieważ można uzyskać dostęp do danych zmiennych, przeanalizować je przez wiersz kodu i wyświetlić instrukcje śledzenia. Oprócz różnych omówionych narzędzi debugowania przedstawiono również sposób użycia klasy sys. Debug biblioteki ASP.NET AJAX w aplikacji oraz sposób, w jaki Klasa ScriptManager może służyć do ładowania debugowania lub wydań wersji skryptów.

## <a name="bio"></a>Materiał

Dan Wahlin (Firma Microsoft jest najbardziej cennym specjalistą dla usług ASP.NET i XML Web Services) jest instruktorem programowania platformy .NET i konsultantem architektury w ramach szkolenia technicznego ([www.interfacett.com)](http://www.interfacett.com). Dan zawarto XML for ASP.NET Developers Web site ([www.XMLforASP.NET](http://www.XMLforASP.NET)), znajduje się w biurze z głośnikem INETA i mówisz do kilku konferencji. Dan współpracujący z profesjonalnym systemem Windows DNA (Wrox), ASP.NET: porady, samouczki i kod (Sams), ASP.NET 1,1 rozwiązań z zakresu testów, profesjonalne ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 MVP Hacks i utworzone XML dla ASP.NET deweloperów (Sams). Gdy nie piszesz kodu, artykuły lub książki, Dan można napisać i nagrać muzykę oraz Basketball z jej żona i dzieci.

Scott Cate pracował z technologiami sieci Web firmy Microsoft od 1997 i jest prezydentem myKB.com ([www.myKB.com](http://www.myKB.com)), w którym wyspecjalizowany jest pisanie aplikacji opartych na ASP.NET, które są zgodne z podstawowymi rozwiązaniami oprogramowania. W witrynie Scotta można skontaktować się z pocztą e-mail na [scott.cate@myKB.com](mailto:scott.cate@myKB.com) lub w blogu w witrynie [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Ubiegł](understanding-asp-net-ajax-web-services.md)

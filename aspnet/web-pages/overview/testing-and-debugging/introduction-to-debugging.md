---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Wprowadzenie do debugowania ASP.NET stron sieci Web (Razor) | Microsoft Docs
author: Rick-Anderson
description: Debugowanie to proces znajdowania i naprawiania błędów na stronach kodowych. W tym rozdziale przedstawiono niektóre narzędzia i techniki, których można użyć do debugowania i Analyz...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: ae7d871e56326610c043dc20fe6e0919e1b4ac89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624370"
---
# <a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Wprowadzenie do debugowania ASP.NET stron sieci Web (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono różne sposoby debugowania stron w witrynie internetowej ASP.NET Web Pages (Razor). Debugowanie to proces znajdowania i naprawiania błędów na stronach kodowych.
>
> **Dowiesz się:**
>
> - Jak wyświetlić informacje, które ułatwiają analizowanie i debugowanie stron.
> - Jak korzystać z narzędzi debugowania w programie Visual Studio.
>
>
> Są to funkcje ASP.NET wprowadzone w artykule:
>
> - Pomocnik `ServerInfo`.
> - pomocnik `ObjectInfo`.
>
>
> ## <a name="software-versions"></a>Wersje oprogramowania
>
>
> - ASP.NET strony sieci Web (Razor) 3
> - Program Visual Studio 2013
>
>
> Ten samouczek działa również z ASP.NET Web Pages 2. Możesz użyć programu WebMatrix 3, ale Zintegrowany debuger nie jest obsługiwany.

Ważnym aspektem rozwiązywania problemów związanych z błędami i problemami w kodzie jest uniknięcie ich w pierwszej kolejności. Można to zrobić, umieszczając sekcje kodu, które mogą spowodować błędy w blokach `try/catch`. Aby uzyskać więcej informacji, zapoznaj się z sekcją dotyczącą obsługi błędów [wprowadzanych w ASP.NET programowanie sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

Pomocnik `ServerInfo` jest narzędziem diagnostycznym, które zawiera przegląd informacji o środowisku serwera sieci Web, który hostuje Twoją stronę. Przedstawiono w nim również informacje o żądaniu HTTP, które są wysyłane, gdy przeglądarka żąda strony. Pomocnik `ServerInfo` wyświetla bieżącą tożsamość użytkownika, typ przeglądarki, która złożyła żądanie itd. Tego rodzaju informacje mogą pomóc w rozwiązywaniu typowych problemów.

1. Utwórz nową stronę sieci Web o nazwie *ServerInfo. cshtml*.
2. Na końcu strony tuż przed tagiem zamykającym `</body>` Dodaj `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Możesz dodać kod `ServerInfo` w dowolnym miejscu na stronie. Jednak dodanie go na końcu spowoduje, że dane wyjściowe będą oddzielone od innych zawartości strony, co ułatwia ich odczytywanie.

    > [!NOTE]
    >
    > **Ważne** Przed przeniesieniem stron sieci Web na serwer produkcyjny należy usunąć dowolny kod diagnostyczny ze stron sieci Web. Ma to zastosowanie do pomocnika `ServerInfo`, a także innych technik diagnostycznych w tym artykule, które obejmują dodawanie kodu do strony. Nie chcesz, aby osoby odwiedzające witrynę sieci Web widzieli informacje o nazwie serwera, nazwach użytkowników, ścieżkach na serwerze i podobnych szczegółach, ponieważ ten typ informacji może być przydatny dla osób mających złośliwy cel.
3. Zapisz stronę i uruchom ją w przeglądarce.

    ![Debugging-1](introduction-to-debugging/_static/image1.jpg)

    Pomocnik `ServerInfo` wyświetla cztery tabele informacji na stronie:

   - Konfiguracja serwera. Ta sekcja zawiera informacje na temat hostingu serwera sieci Web, w tym nazwy komputera, używanej wersji programu ASP.NET, nazwy domeny i czasu serwera.
   - Zmienne serwera ASP.NET. Ta sekcja zawiera szczegółowe informacje dotyczące wielu szczegółów protokołu HTTP (nazywanych zmiennymi HTTP) i wartości, które są częścią każdego żądania strony sieci Web.
   - Informacje o środowisku uruchomieniowym protokołu HTTP. Ta sekcja zawiera szczegółowe informacje na temat wersji platformy Microsoft .NETej, w której działa strona sieci Web, ścieżki, szczegółów dotyczących pamięci podręcznej itd. (W miarę zdobywania w programie [ASP.NET programowanie sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET stron sieci Web korzystających z składnia Razor są oparte na technologii serwera sieci Web ASP.NET firmy Microsoft, która jest wbudowana w rozbudowana Biblioteka programistyczna oprogramowania o nazwie .NET Framework.)
   - Zmienne środowiskowe. Ta sekcja zawiera listę wszystkich lokalnych zmiennych środowiskowych i ich wartości na serwerze sieci Web.

     Pełny opis wszystkich informacji o serwerze i żądaniu wykracza poza zakres tego artykułu, ale można zobaczyć, że pomocnik `ServerInfo` zwraca wiele informacji diagnostycznych. Aby uzyskać więcej informacji na temat wartości zwracanych przez `ServerInfo`, zobacz [rozpoznane zmienne środowiskowe](https://technet.microsoft.com/library/dd560744(WS.10).aspx) w witrynie sieci Web Microsoft TechNet i [zmiennych serwera IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) w witrynie MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Osadzanie wyrażeń wyjściowych w celu wyświetlenia wartości strony

Innym sposobem, aby zobaczyć, co się dzieje w kodzie, jest osadzenie wyrażeń wyjściowych na stronie. Jak wiadomo, możesz bezpośrednio wyprowadzić wartość zmiennej przez dodanie elementu, takiego jak `@myVariable` lub `@(subTotal * 12)` do strony. W przypadku debugowania można umieścić te wyrażenia danych wyjściowych w punktach strategicznych w kodzie. Dzięki temu można zobaczyć wartości zmiennych kluczowych lub wynik obliczeń, gdy strona zostanie uruchomiona. Po zakończeniu debugowania można usunąć wyrażenia lub dodać do nich komentarz. Ta procedura ilustruje typowy sposób użycia osadzonych wyrażeń do ułatwienia debugowania strony.

1. Utwórz nową stronę sieci Web o nazwie *OutputExpression. cshtml*.
2. Zastąp zawartość strony następującym:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    W przykładzie używa się instrukcji `switch`, aby sprawdzić wartość zmiennej `weekday`, a następnie wyświetlić inny komunikat wyjściowy w zależności od dnia tygodnia. W tym przykładzie blok `if` w ramach pierwszego bloku kodu arbitralnie zmienia dzień tygodnia, dodając jeden dzień do bieżącej wartości dnia tygodnia. Jest to błąd wprowadzony w celach ilustracji.
3. Zapisz stronę i uruchom ją w przeglądarce.

    Na stronie zostanie wyświetlony komunikat dotyczący nieprawidłowego dnia tygodnia. Każdy dzień tygodnia, w którym faktycznie jest, zobaczysz komunikat przez jeden dzień później. Chociaż w tym przypadku wiesz, dlaczego komunikat jest wyłączony (ponieważ kod świadomie ustawia nieprawidłową wartość dnia), w rzeczywistości często trudno jest wiedzieć, gdzie elementy w kodzie są błędne. Aby debugować, należy dowiedzieć się, co dzieje się z wartościami kluczowych obiektów i zmiennych, takimi jak `weekday`.
4. Dodaj wyrażenia wyjściowe, wstawiając `@weekday` jak pokazano w dwóch miejscach wskazywanych przez komentarze w kodzie. Te wyrażenia wyjściowe będą wyświetlały wartości zmiennej w tym momencie wykonywania kodu.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Zapisz i Uruchom stronę w przeglądarce.

    Na stronie jest wyświetlany pierwszy dzień tygodnia, a następnie zaktualizowany dzień tygodnia, który wynika z dodawania jednego dnia, a następnie wynikowy komunikat z instrukcji `switch`. Dane wyjściowe z dwóch wyrażeń zmiennych (`@weekday`) nie zawierają spacji między dniami, ponieważ nie dodano żadnych tagów `<p>` HTML do danych wyjściowych. wyrażenia są przeznaczone tylko do testowania.

    ![Debugging-2](introduction-to-debugging/_static/image2.jpg)

    Teraz można zobaczyć, gdzie występuje błąd. Po pierwszym wyświetleniu zmiennej `weekday` w kodzie zostanie wyświetlony właściwy dzień. Gdy wyświetlasz go po raz drugi, po bloku `if` w kodzie, dzień jest wyłączony o jeden. Wiadomo, że coś się stało między pierwszym i drugim wyglądem zmiennej dnia tygodnia. Jeśli była to prawdziwa usterka, ten rodzaj podejścia pomoże Ci zawęzić lokalizację kodu powodującego problem.
6. Popraw kod na stronie, usuwając dwa dodane wyrażenia wyjściowe i usuwając kod, który zmienia dzień tygodnia. Pozostały pełny blok kodu wygląda podobnie do poniższego przykładu:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Uruchom stronę w przeglądarce. Tym razem zobaczysz prawidłowy komunikat wyświetlany dla rzeczywistego dnia tygodnia.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Wyświetlanie wartości obiektów przy użyciu pomocnika ObjectInfo

Pomocnik `ObjectInfo` wyświetla typ i wartość każdego przekazanego obiektu. Można jej użyć do wyświetlenia wartości zmiennych i obiektów w kodzie (takich jak w przypadku wyrażeń wyjściowych w poprzednim przykładzie), a ponadto można wyświetlić informacje o typie danych dotyczące obiektu.

1. Otwórz plik o nazwie *OutputExpression. cshtml* , który został utworzony wcześniej.
2. Zastąp cały kod na stronie następującym blokiem kodu:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Zapisz i Uruchom stronę w przeglądarce.

    ![Debugging-4](introduction-to-debugging/_static/image3.jpg)

    W tym przykładzie pomocnik `ObjectInfo` wyświetla dwa elementy:

   - Typ. Dla pierwszej zmiennej typ jest `DayOfWeek`. Dla drugiej zmiennej typ jest `String`.
   - Wartość. W tym przypadku, ponieważ na stronie jest już wyświetlana wartość zmiennej Greetings, wartość jest wyświetlana ponownie po przekazanie zmiennej do `ObjectInfo`.

     W przypadku bardziej złożonych obiektów pomocnik `ObjectInfo` może wyświetlić więcej informacji &#8212; zasadniczo, można wyświetlić typy i wartości wszystkich właściwości obiektu.

## <a name="using-debugging-tools-in-visual-studio"></a>Korzystanie z narzędzi debugowania w programie Visual Studio

Aby uzyskać bardziej kompleksowe środowisko debugowania, użyj [programu Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Za pomocą programu Visual Studio można ustawić punkt przerwania w kodzie w wierszu, który chcesz sprawdzić.

![Ustaw punkt przerwania](introduction-to-debugging/_static/image1.png)

Podczas testowania witryny sieci Web kod wykonywany zostaje zatrzymany w punkcie przerwania.

![dostęp do punktu przerwania](introduction-to-debugging/_static/image2.png)

Można przeanalizować bieżące wartości zmiennych i krok po kroku przez kod po wierszu.

![Zobacz wartości](introduction-to-debugging/_static/image3.png)

Aby uzyskać informacje o używaniu zintegrowanego debugera w programie Visual Studio do debugowania stron ASP.NET Razor, zobacz [programowanie ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Dodatkowe materiały

- [Programowanie ASP.NET stron sieci Web (Razor) przy użyciu programu Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Zmienne serwera IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Rozpoznawane zmienne środowiskowe](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)

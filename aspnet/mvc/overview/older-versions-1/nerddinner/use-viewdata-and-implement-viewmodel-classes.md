---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Korzystanie z ViewData i implementowanie klas ViewModel | Microsoft Docs
author: microsoft
description: Krok 6 pokazuje, jak włączyć obsługę bogatszych scenariuszy edycji formularzy, a także omówiono dwa podejścia, których można użyć do przekazywania danych z kontrolerów do widoków:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: ca9775417c2e25952511a73096fb76d5d4edaea2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541609"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Korzystanie z podejścia ViewData i implementowanie klas ViewModel

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 6 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 6 pokazuje, jak włączyć obsługę bogatszych scenariuszy edycji formularzy, a także omówiono dwa podejścia, których można użyć do przekazywania danych z kontrolerów do widoków: ViewData i ViewModel.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner krok 6: ViewData i ViewModel

Omówiono kilka scenariuszy post formularzy i omówiono sposób implementacji obsługi tworzenia, aktualizowania i usuwania (CRUD). Teraz zajmiemy się implementacją DinnersController i umożliwimy obsługę bogatszych scenariuszy edycji formularzy. W tym celu omawiamy dwie podejścia, których można użyć do przekazywania danych z kontrolerów do widoków: ViewData i ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Przekazywanie danych z kontrolerów do widoków-szablonów

Jedną z cech definicji wzorca MVC jest ścisłe "separacja obaw", które ułatwiają wymuszanie między różnymi składnikami aplikacji. Modele, kontrolery i widoki mają dobrze zdefiniowane role i obowiązki, a także komunikują się między sobą. Pozwala to na podwyższenie możliwości testowania i ponownego użycia kodu.

Gdy klasa kontrolera zdecyduje się na renderowanie odpowiedzi HTML z powrotem do klienta, jest odpowiedzialna za jawne przekazanie do szablonu widoku wszystkich danych wymaganych do renderowania odpowiedzi. Szablony widoków nigdy nie wykonują żadnych operacji pobierania danych ani logiki aplikacji — a zamiast tego ograniczają się tylko do kodu renderowania, który jest oparty na modelu/danych przekazanego do niego przez kontroler.

Teraz dane modelu przekazywane przez naszą klasę DinnersController do naszych szablonów widoku są proste i proste — do przodu — lista obiektów obiadu w przypadku indeksu () i pojedynczy obiekt obiadu w przypadku szczegółów (), Edit (), Create () i Delete (). Po dodaniu większej liczby funkcji interfejsu użytkownika do aplikacji często będziemy musieli przekazać więcej niż tylko te dane, aby renderować odpowiedzi HTML w naszych szablonach widoków. Na przykład możemy chcieć zmienić pole "Country" w naszym Edytuj i utworzyć widoki z pola tekstowego HTML na DropDownList. Zamiast nakodować twardy listę rozwijaną nazw krajów w szablonie widoku, możemy wygenerować ją z listy obsługiwanych krajów, które wypełniamy dynamicznie. Będziemy musieli przekazać zarówno obiekt obiadu *, jak i* listę obsługiwanych krajów z naszego kontrolera do naszego szablonu widoku.

Przyjrzyjmy się na dwa sposoby tego celu.

### <a name="using-the-viewdata-dictionary"></a>Korzystanie z słownika ViewData

Klasa bazowa kontrolera uwidacznia Właściwość słownika "ViewData", która może służyć do przekazywania dodatkowych elementów danych z kontrolerów do widoków.

Na przykład w celu obsługi scenariusza, w którym chcemy zmienić pole tekstowe "kraj" w naszym widoku edycji z pola tekstowego HTML na DropDownList, możemy zaktualizować metodę akcji Edytuj (), aby przekazać (oprócz obiektu obiad) obiekt SelectList, który może być używany jako m odelu krajów DropDownList.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Konstruktor SelectList powyżej akceptuje listę countów, aby wypełnić polecenie Drop-downlist with, a także aktualnie wybraną wartość.

Następnie możemy zaktualizować nasz szablon widoku Edytuj. aspx, aby użyć metody pomocnika html. DropDownList () zamiast metody pomocnika html. TextBox (), która została użyta wcześniej:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Powyższa metoda pomocnika html. DropDownList () przyjmuje dwa parametry. Pierwsza to nazwa elementu formularza HTML do wyprowadzenia. Drugi jest modelem "SelectList", który został przesłany za pośrednictwem słownika ViewData. Używamy słowa kluczowego C# "As" do rzutowania typu w słowniku jako SelectList.

Po uruchomieniu aplikacji i uzyskaniu dostępu do adresu URL */Dinners/Edit/1* w naszej przeglądarce zobaczymy, że nasz interfejs użytkownika edytowania został zaktualizowany tak, aby wyświetlał DropDownList krajów zamiast pola tekstowego:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Ponieważ renderuje również szablon widoku edycji z metody HTTP-POST Edit (w scenariuszach w przypadku wystąpienia błędów), chcemy upewnić się, że aktualizujemy również tę metodę, aby dodać SelectList do ViewData, gdy szablon widoku jest renderowany w scenariuszach błędów:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

A teraz nasz scenariusz edytowania DinnersController obsługuje DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Używanie wzorca ViewModel

Podejście słownikowe ViewData jest korzystne i łatwe do zaimplementowania. Niektórzy deweloperzy nie używają słowników opartych na ciągach, ale ponieważ literówki mogą prowadzić do błędów, które nie będą przechwytywane w czasie kompilacji. Słownik ViewData o nieokreślonym typie wymaga również użycia operatora "As" lub odlewania przy użyciu języka o jednoznacznie określonym typie C# , takiego jak w szablonie widoku.

Alternatywnym podejściem, którego można użyć, jest często nazywane wzorcem "ViewModel". W przypadku korzystania z tego wzorca tworzymy klasy o jednoznacznie określonym typie, które są zoptymalizowane pod kątem naszych scenariuszy widoku, i które uwidaczniają właściwości wartości dynamicznych/zawartości wymaganej przez nasze szablony widoków. Nasze klasy kontrolera mogą następnie wypełnić i przekazać te klasy zoptymalizowane pod kątem widoku do naszego szablonu widoku. Zapewnia to bezpieczeństwo typu, sprawdzanie czasu kompilacji i IntelliSense w edytorze w szablonach widoków.

Na przykład, aby włączyć scenariusze edytowania formularza obiadu, możemy utworzyć klasę "DinnerFormViewModel", jak poniżej, która uwidacznia dwie właściwości o jednoznacznie określonym typie: obiekt obiadu i model SelectList, który jest wymagany do wypełnienia krajów DropDownList:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Następnie możemy zaktualizować metodę akcji Edit () w celu utworzenia DinnerFormViewModel przy użyciu obiektu obiadu, który pobieramy z naszego repozytorium, a następnie przekazać go do naszego szablonu widoku:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Następnie zaktualizujemy nasz szablon widoku tak, aby oczekiwał "DinnerFormViewModel" zamiast obiektu "obiad", zmieniając atrybut "Inherits" w górnej części strony Edit. aspx, na przykład:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Po wykonaniu tej czynności funkcja IntelliSense właściwości "model" w naszym szablonie widoku zostanie zaktualizowana w celu odzwierciedlenia modelu obiektów typu DinnerFormViewModel, który przekazujemy:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Następnie możemy zaktualizować nasz kod widoku, aby go wycofać. Zwróć uwagę na to, jak nie zmieniamy nazw elementów wejściowych, które tworzysz (elementy formularza będą nadal nazywane "tytułem", "kraj"), ale aktualizujemy metody pomocników HTML w celu pobrania wartości przy użyciu klasy DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Zaktualizujemy również nasze metody edycji post, aby używać klasy DinnerFormViewModel podczas renderowania błędów:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Możemy również zaktualizować nasze metody akcji Create (), aby ponownie użyć dokładnie tej samej klasy *DinnerFormViewModel* , aby umożliwić krajom DropDownList w tych krajach. Poniżej znajduje się implementacja protokołu HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Poniżej znajduje się implementacja metody Create protokołu HTTP-POST:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Teraz zarówno nasze ekrany edycji, jak i tworzenie obsługują downlists do wybierania kraju.

### <a name="custom-shaped-viewmodel-classes"></a>Niestandardowe klasy ViewModel

W powyższym scenariuszu Klasa DinnerFormViewModel bezpośrednio ujawnia obiekt modelu obiadu jako właściwość wraz z właściwością SelectList model. Takie podejście działa prawidłowo w przypadku scenariuszy, w których interfejs użytkownika HTML, który chcemy utworzyć w naszym szablonie widoku, odpowiada stosunkowo ścisłemu obiektowi modelu domeny.

W przypadku scenariuszy, w których nie jest to przypadek, jedną z opcji, której można użyć, jest utworzenie klasy ViewModel o kształcie niestandardowym, której model obiektów jest bardziej zoptymalizowany do użycia przez widok — i który może wyglądać zupełnie inaczej od obiektu modelu domeny. Na przykład może potencjalnie uwidocznić różne nazwy właściwości i/lub zagregowane właściwości zebrane z wielu obiektów modelu.

Klasy ViewModel w kształcie niestandardowym mogą być używane zarówno do przekazywania danych z kontrolerów do widoków do renderowania, jak i do obsługi danych formularza opublikowanych z powrotem do metody akcji kontrolera. W tym późniejszym scenariuszu może istnieć Metoda akcji aktualizująca obiekt ViewModel z danymi opublikowanymi w formie, a następnie użycie wystąpienia ViewModel w celu zamapowania lub pobrania rzeczywistego obiektu modelu domeny.

Klasy ViewModel w kształcie niestandardowym mogą zapewniać dużą elastyczność i są coś do zbadania dowolnego czasu, aby znaleźć kod renderowania w szablonach widoku lub kod post formularza wewnątrz metod akcji, rozpoczynając od zbyt skomplikowanego. Jest to często znak, który nie odpowiada Twoim modelom domeny w sposób niewidoczny dla generowanego interfejsu użytkownika i że pośrednia niestandardowa Klasa ViewModel może pomóc.

### <a name="next-step"></a>Następny krok

Teraz przyjrzyjmy się sposobom używania częściowych i stron głównych do ponownego użycia i udostępnienia interfejsu użytkownika w naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [dalej](re-use-ui-using-master-pages-and-partials.md)

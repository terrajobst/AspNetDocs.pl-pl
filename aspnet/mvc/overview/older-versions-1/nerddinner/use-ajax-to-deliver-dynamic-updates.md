---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych | Microsoft Docs
author: microsoft
description: Krok 10 implementuje obsługę zalogowanych użytkowników w celu założenia swojego zainteresowania na obiad przy użyciu opartego na technologii AJAX podejścia zintegrowanego w ramach szczegółów obiadu...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600850"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a>Korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 10 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 10 implementuje obsługę zalogowanych użytkowników, aby dowiedzieć się, jak wziąć udział w obiadie przy użyciu podejścia opartego na technologii AJAX zintegrowanego na stronie szczegółów obiadu.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner krok 10: akceptowane jest włączenie funkcji AJAX

Zaimplementujmy teraz obsługę zalogowanych użytkowników, aby dopuścić do wzięcia udziału w obiadie. Włączmy to przy użyciu podejścia opartego na technologii AJAX zintegrowanego na stronie szczegółów obiadu.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Wskazuje, czy użytkownik jest w usłudze RSVP

Użytkownicy mogą odwiedzać adres URL */Dinners/Details/[ID*], aby wyświetlić szczegółowe informacje dotyczące konkretnego obiadu:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Metoda akcja szczegóły () jest zaimplementowana tak, jak:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Pierwszym krokiem w celu zaimplementowania obsługi protokołu RSVP będzie dodanie metody pomocnika "IsUserRegistered (username)" do naszego obiektu obiadu (w ramach klasy Dinner.cs częściowej utworzonej wcześniej). Ta metoda pomocnika zwraca wartość true lub false, w zależności od tego, czy użytkownik jest obecnie RSVP w przypadku obiadu:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Następnie można dodać poniższy kod do naszego szablonu widoku szczegółów. aspx, aby wyświetlić odpowiedni komunikat wskazujący, czy użytkownik jest zarejestrowany, czy nie dla zdarzenia:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

A teraz po zarejestrowaniu się obiadu dla użytkowników zostanie wyświetlony następujący komunikat:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Po odwiedzeniu obiadu nie są one zarejestrowane, zobaczysz następujący komunikat:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementowanie metody akcji Register

Dodajmy teraz funkcje niezbędne do umożliwienia użytkownikom zawieszania się na obiad ze strony szczegółów.

Aby zaimplementować ten element, utworzymy nową klasę "RSVPController", klikając prawym przyciskiem myszy katalog \Controllers i wybierając polecenie menu Dodaj kontroler&gt;.

Zaimplementujmy metodę akcji "Register" w ramach nowej klasy RSVPController, która przyjmuje identyfikator dla obiadu jako argument, pobiera odpowiedni obiekt obiadu i sprawdza, czy zalogowany użytkownik znajduje się obecnie na liście użytkowników, którzy zostali zarejestrowani. nie dodaje obiektu RSVP dla nich:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Zwróć uwagę na to, jak zwracamy prosty ciąg jako dane wyjściowe metody akcji. Ten komunikat został osadzony w szablonie widoku — ale ponieważ jest to małe użycie metody pomocnika Content () w klasie podstawowej kontrolera i zwrócenie komunikatu ciągu jak powyżej.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Wywoływanie metody akcji RSVPForEvent przy użyciu technologii AJAX

Użyjemy technologii AJAX do wywołania metody rejestracji z naszego widoku szczegółów. Zaimplementowanie tego jest bardzo proste. Najpierw dodamy dwa odwołania do biblioteki skryptów:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

Pierwsza Biblioteka odwołuje się do podstawowej biblioteki skryptów po stronie klienta ASP.NET AJAX. Ten plik ma około 24k (skompresowany) i zawiera podstawowe funkcje AJAX po stronie klienta. Druga biblioteka zawiera funkcje narzędziowe, które integrują się z wbudowanymi metodami pomocnika AJAX ASP.NET MVC (których użyjemy wkrótce).

Następnie możemy zaktualizować kod widoku, który dodałeś wcześniej, tak aby zamiast wycofać komunikat "nie zarejestrowano dla tego zdarzenia". zamiast tego zostanie wyświetlona próba wywołania AJAX, które wywołuje nasze metody akcji RSVPForEvent na naszym kontrolerze RSVP i RSVP użytkownika:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Użyta powyżej metoda pomocnika AJAX. ActionLink () jest wbudowana w ASP.NET MVC i jest podobna do metody pomocnika html. ActionLink (), z wyjątkiem sytuacji, gdy nie wykonuje się standardowej nawigacji, wykonuje wywołanie AJAX do metody akcji po kliknięciu łącza. Powyżej wywołujemy metodę akcji "Register" na kontrolerze "RSVP" i przekazując DinnerID jako parametr "ID". Końcowy parametr AjaxOptions, który przekazujemy, wskazuje, że chcemy pobrać zawartość zwróconą z metody akcji i zaktualizować element HTML &lt;DIV&gt; na stronie, której identyfikator to "rsvpmsg".

A teraz po przejściu użytkownika do obiadu, którego nie zarejestrowano, zobaczysz link do usługi RSVP dla tego:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Jeśli klikniesz link "RSVP dla tego zdarzenia", nastąpi wywołanie AJAX do metody Register na kontrolerze RSVP, a po jej zakończeniu zobaczysz zaktualizowany komunikat podobny do poniższego:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

Przepustowość sieci i ruch związany z wykonywaniem tego wywołania AJAX jest naprawdę lekki. Gdy użytkownik kliknie łącze "RSVP dla tego zdarzenia", na adres URL */Dinners/Register/1* , który wygląda jak poniżej, zostanie wysłane małe żądanie HTTP POST.

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

A odpowiedź z naszej metody działania Register jest po prostu:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

To lekkie wywołanie jest szybkie i będzie działało nawet za pośrednictwem wolnej sieci.

### <a name="adding-a-jquery-animation"></a>Dodawanie animacji jQuery

Wdrożone funkcje AJAX działają dobrze i szybko. Czasami może to być spowodowane tym, że użytkownik może nie zauważyć, że łącze RSVP zostało zastąpione nowym tekstem. Aby wynik był nieco bardziej oczywisty, możemy dodać prostą animację, aby zwrócić uwagę na komunikat dotyczący aktualizacji.

Domyślny szablon projektu ASP.NET MVC obejmuje jQuery — doskonałe (i bardzo popularne) biblioteki języka JavaScript Open Source, które są również obsługiwane przez firmę Microsoft. jQuery udostępnia wiele funkcji, w tym całkiem do wyboru język DOM HTML i bibliotekę efektów.

Aby użyć jQuery, najpierw dodamy do niego odwołanie do skryptu. Ze względu na to, że będziemy używać platformy jQuery w różnych miejscach w naszej witrynie, dodamy odwołanie do skryptu w naszej witrynie. główny plik strony głównej, tak aby wszystkie strony mogły go używać.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Porada: Upewnij się, że zainstalowano poprawkę IntelliSense języka JavaScript dla programu VS 2008 z dodatkiem SP1, która umożliwia zaawansowaną obsługę funkcji IntelliSense dla plików JavaScript (w tym jQuery). Można go pobrać z: http://tinyurl.com/vs2008javascripthotfix*

Kod zapisany przy użyciu JQuery często używa globalnej metody JavaScript "$ ()", która pobiera jeden lub więcej elementów HTML przy użyciu selektora CSS. Na przykład *$ ("#rsvpmsg")* wybiera każdy element HTML o identyfikatorze rsvpmsg, podczas gdy *$ (". coś")* wybierze wszystkie elementy z nazwą klasy CSS "coś". Możesz również napisać bardziej zaawansowane zapytania, takie jak "Zwróć wszystkie sprawdzone przyciski radiowe" przy użyciu zapytania selektora, takiego jak: *$ ("Input [@type= Radio] [@checked]")* .

Po wybraniu elementów możesz wywoływać metody na nich w celu podjęcia działania, takich jak ukrywanie: *$ ("#rsvpmsg"). Hide ();*

W naszym scenariuszu RSVP zdefiniujemy prostą funkcję języka JavaScript o nazwie "AnimateRSVPMessage", która wybiera&gt; "rsvpmsg" &lt;DIV, i Animuj rozmiar zawartości tekstowej. Poniższy kod uruchamia mały tekst, a następnie powoduje zwiększenie przez 400 milisekund czasu:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Możemy następnie skorzystać z tej funkcji języka JavaScript, która będzie wywoływana po pomyślnym ukończeniu wywołania AJAX przez przekazanie jej nazwy do naszej metody pomocnika AJAX. ActionLink () (za pomocą właściwości zdarzenia AjaxOptions "onSuccess"):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

A teraz, gdy zostanie kliknięte łącze "RSVP dla tego zdarzenia", a nasze wywołanie AJAX zostanie zakończone pomyślnie, komunikat zawartości wysłany z powrotem zostanie animowany i powiększony:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Oprócz podania zdarzenia "onSuccess" obiekt AjaxOptions uwidacznia zdarzenia onbegin, OnFailure i OnComplete, które można obsłużyć (wraz z różnymi innymi właściwościami i opcjami użytecznymi).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Czyszczenie — Refaktoryzacja widoku częściowego RSVP

Nasz szablon widoku szczegółów rozpoczyna się od dłuższego czasu, który jest nieco trudniejszy do zrozumienia. Aby poprawić czytelność kodu, Zakończmy Tworzenie widoku częściowego — RSVPStatus. ascx — który hermetyzuje wszystkie kody widoku RSVP dla naszej strony szczegółów.

Można to zrobić, klikając prawym przyciskiem myszy folder \Views\Dinners, a następnie wybierając polecenie menu Widok Dodaj&gt;. Będziemy korzystać z obiektu obiadu jako swojej jednoznacznie wpisanej ViewModel. Następnie możemy skopiować/wkleić zawartość RSVP z naszego widoku Szczegóły. aspx.

Po wykonaniu tej czynności utworzymy również inny widok częściowy — EditAndDeleteLinks. ascx — który hermetyzuje nasz kod widoku linku Edytuj i Usuń. Będziemy również korzystać z obiektu obiadu jako swojej jednoznacznie wpisanej ViewModel, a następnie skopiuj/wklej logikę Edytuj i Usuń z naszego widoku Szczegóły. aspx.

Nasz szablon widoku szczegółów może następnie zawierać dwa wywołania metody html. RenderPartial () na dole:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Pozwala to na odczytywanie i konserwowanie oczyszczarki kodu.

### <a name="next-step"></a>Następny krok

Teraz przyjrzyjmy się sposobom, w jaki możemy jeszcze bardziej wykorzystać technologię AJAX i dodać obsługę mapowania interaktywnego do naszej aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](secure-applications-using-authentication-and-authorization.md)
> [dalej](use-ajax-to-implement-mapping-scenarios.md)

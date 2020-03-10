---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Zapobieganie atakom polegającym na iniekcji kodu JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Zapobiegaj atakom z użyciem kodu JavaScript i atakami z obsługą skryptów między lokacjami. W tym samouczku Stephen Walther wyjaśnia, jak można łatwo cofnąć...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: dfe09085f26c62c566649bc6f570aa25367a0f07
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624097"
---
# <a name="preventing-javascript-injection-attacks-vb"></a>Zapobieganie atakom polegającym na wstrzyknięciu kodu JavaScript (VB)

Autor [Stephen Walther](https://github.com/StephenWalther)

[Pobierz plik PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Zapobiegaj atakom z użyciem kodu JavaScript i atakami z obsługą skryptów między lokacjami. W tym samouczku Stephen Walther wyjaśnia, w jaki sposób można łatwo wypaczać ataki przez kodowanie HTML.

Celem tego samouczka jest wyjaśnienie, jak można zapobiec atakom z iniekcją kodu JavaScript w aplikacjach ASP.NET MVC. W tym samouczku omówiono dwa podejścia do obrony witryny sieci Web przed atakami polegającymi na iniekcji kodu JavaScript. Dowiesz się, jak zapobiegać atakom z użyciem kodu JavaScript przez kodowanie wyświetlanych danych. Dowiesz się również, jak zapobiegać atakom z użyciem kodu JavaScript przez zakodowanie akceptowanych danych.

## <a name="what-is-a-javascript-injection-attack"></a>Co to jest atak z iniekcją kodu JavaScript?

Za każdym razem, gdy zaakceptujesz dane wejściowe użytkownika i ponownie wyświetlasz dane wejściowe użytkownika, możesz otworzyć witrynę internetową, aby uzyskać ataki kodu JavaScript. Sprawdźmy konkretną aplikację, która jest otwarta do ataków iniekcji kodu JavaScript.

Załóżmy, że utworzono witrynę internetową opinii klienta (patrz rysunek 1). Klienci mogą odwiedzić witrynę internetową i wprowadzić Opinie na ich temat, korzystając z Twoich produktów. Gdy klient prześle swoją opinię, opinia jest ponownie wyświetlana na stronie opinii.

[Witryna sieci Web opinii klientów ![](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Ilustracja 01**. Witryna internetowa opinii klientów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image3.png))

Witryna internetowa opinii klientów używa `controller` z listy 1. Ten `controller` zawiera dwie akcje o nazwie `Index()` i `Create()`.

**Lista 1 — `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

Metoda `Index()` wyświetla widok `Index`. Ta metoda przekazuje wszystkie poprzednie opinie klientów do widoku `Index`, pobierając informacje zwrotne z bazy danych (przy użyciu zapytania LINQ to SQL).

Metoda `Create()` tworzy nowy element opinii i dodaje go do bazy danych. Komunikat wprowadzony przez klienta w formularzu jest przesyłany do metody `Create()` w parametrze komunikatu. Zostanie utworzony element opinii, a wiadomość zostanie przypisana do właściwości `Message` elementu opinii. Element opinii jest przesyłany do bazy danych za pomocą wywołania metody `DataContext.SubmitChanges()`. Na koniec odmowa zostanie przekierowana z powrotem do widoku `Index`, w którym jest wyświetlana cała opinia.

Widok `Index` jest zawarty w liście 2.

**Lista 2 — `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

Widok `Index` ma dwie sekcje. Górna sekcja zawiera rzeczywisty formularz opinii klientów. Dolna sekcja zawiera... Każda pętla, która przechodzi przez wszystkie poprzednie elementy opinii klientów i wyświetla EntryDate i właściwości wiadomości dla każdego elementu opinii.

Witryna internetowa opinii klientów jest prostą witryną sieci Web. Niestety, witryna internetowa jest otwarta do ataków iniekcji kodu JavaScript.

Załóżmy, że wprowadzasz następujący tekst do formularza opinii klientów:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Ten tekst przedstawia skrypt JavaScript, który wyświetla okno komunikatu alertu. Gdy ktoś wyśle ten skrypt do formularza opinii, komunikat <em>Boo!</em> zostanie wyświetlona za każdym razem, gdy ktoś odwiedzi witrynę internetową opinii klientów w przyszłości (patrz rysunek 2).

[![iniekcja kodu JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Ilustracja 02**: iniekcja kodu JavaScript ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image6.png))

Teraz początkową odpowiedzią na ataki kodu JavaScript może być Apathy. Może się okazać, że ataki kodu JavaScript są po prostu typem *ataku.* Może się zdarzyć, że nikt nie może wykonać niczego naprawdę akcja przez zatwierdzenie ataku polegającego na iniekcji kodu JavaScript.

Niestety haker może robić naprawdę akcja rzeczy, wprowadzając kod JavaScript do witryny sieci Web. Aby wykonać atak między lokacjami (XSS), można użyć ataku polegającego na iniekcji języka JavaScript. W przypadku ataku za skrypty między lokacjami można wykraść poufne informacje o użytkownikach i wysłać je do innej witryny sieci Web.

Na przykład haker może użyć ataku polegającego na iniekcji kodu JavaScript w celu kradzieży wartości plików cookie w przeglądarce od innych użytkowników. Jeśli informacje poufne — takie jak hasła, numery kart kredytowych lub numery ubezpieczenia społecznego są przechowywane w plikach cookie przeglądarki, haker może użyć ataku wstrzykiwania kodu JavaScript, aby wykraść te informacje. Lub, jeśli użytkownik wprowadzi poufne informacje w polu formularza zawartym na stronie, która została naruszona przez atak JavaScript, haker może użyć wstrzykniętego kodu JavaScript do pobrania danych formularza i wysłania ich do innej witryny sieci Web.

*Należy obawialiśmy*. Należy wziąć pod istotny wpływ na ataki kodu JavaScript i chronić poufne informacje użytkownika. W dwóch następnych sekcjach omówiono dwie techniki, za pomocą których można chronić aplikacje ASP.NET MVC przed atakami polegającymi na iniekcji kodu JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Podejście #1: kodowanie HTML w widoku

Jedną z prostych metod zapobiegania atakom z użyciem kodu JavaScript jest kodowanie HTML wszelkich danych wprowadzonych przez użytkowników witryny sieci Web po ponownym wyświetleniu danych w widoku. Zaktualizowany widok `Index` na liście 3 jest zgodny z tym podejściem.

**Lista 3 — `Index.aspx` (kodowanie HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Zwróć uwagę, że wartość `feedback.Message` jest zakodowana HTML przed wyświetleniem wartości przy użyciu następującego kodu:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Co oznacza kod HTML kodowania ciągu? Podczas kodowania ciągu HTML niebezpieczne znaki, takie jak `<` i `>`, są zamieniane na odwołania do jednostek HTML, takie jak `&lt;` i `&gt;`. Tak więc, gdy ciąg `<script>alert("Boo!")</script>` jest zakodowana w języku HTML, zostanie przekonwertowany na `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. Zakodowany ciąg nie jest już wykonywany jako skrypt JavaScript, gdy jest interpretowany przez przeglądarkę. Zamiast tego otrzymujesz niegroźną stronę na rysunku 3.

[![ataku w języku JavaScript](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Ilustracja 03**: atak spowodowany przez sprawność języka JavaScript ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](preventing-javascript-injection-attacks-vb/_static/image9.png))

Zwróć uwagę, że w widoku `Index` na liście 3 jest zaszyfrowana tylko wartość `feedback.Message`. Wartość `feedback.EntryDate` nie jest zaszyfrowana. Wystarczy zakodować dane wprowadzone przez użytkownika. Ponieważ wartość EntryDate została wygenerowana w kontrolerze, nie musisz kodować kodu HTML tej wartości.

## <a name="approach-2-html-encode-in-the-controller"></a>Podejście #2: kodowanie HTML w kontrolerze

Zamiast danych kodowania HTML podczas wyświetlania danych w widoku, można kodować dane HTML bezpośrednio przed przesłaniem danych do bazy danych programu. Drugie podejście jest podejmowane w przypadku `controller` na liście 4.

**Lista 4 — `HomeController.cs` (kodowanie HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Zwróć uwagę, że wartość komunikatu jest zakodowana w formacie HTML przed przesłaniem wartości do bazy danych w ramach akcji `Create()`. Gdy komunikat jest ponownie wyświetlany w widoku, komunikat jest zakodowany w formacie HTML i żaden kod JavaScript wprowadzony w komunikacie nie jest wykonywany.

Zwykle należy preferować pierwsze podejście omówione w tym samouczku w porównaniu z tym drugim podejściem. Problem z tym drugim podejściem polega na tym, że dane zakodowane w formacie HTML są kodowane w bazie danych. Innymi słowy, dane bazy danych są 'dirtied za pomocą zabawnych znaków.

Dlaczego to zły? Jeśli kiedykolwiek zajdzie potrzeba wyświetlenia danych bazy danych znajdujących się poza stroną sieci Web, wystąpią problemy. Na przykład nie można już łatwo wyświetlać danych w aplikacji Windows Forms.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było zascarenie się z potencjalnym atakiem z użyciem kodu JavaScript. W tym samouczku omówiono dwie podejścia do obrony aplikacji ASP.NET MVC przed atakami polegającymi na iniekcji kodu JavaScript: możesz kodować HTML dane przesłane przez użytkownika w widoku lub można kodować HTML dane przesyłane przez użytkownika w kontrolerze.

> [!div class="step-by-step"]
> [Wstecz](authenticating-users-with-windows-authentication-vb.md)

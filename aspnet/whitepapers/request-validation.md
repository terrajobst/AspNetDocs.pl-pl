---
uid: whitepapers/request-validation
title: Żądanie weryfikacji — zapobieganie atakom skryptu | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano funkcję walidacji żądania ASP.NET, gdzie Domyślnie aplikacja uniemożliwia przetwarzanie niekodowanej zawartości HTML...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640848"
---
# <a name="request-validation---preventing-script-attacks"></a>Żądanie walidacji — zapobieganie atakom za pomocą skryptów

> W tym dokumencie opisano funkcję walidacji żądania ASP.NET, gdzie Domyślnie aplikacja nie może przetwarzać niekodowanej zawartości HTML przesłanej do serwera. Tę funkcję sprawdzania poprawności żądania można wyłączyć, gdy aplikacja została zaprojektowana w celu bezpiecznego przetworzenia danych HTML.
> 
> Dotyczy ASP.NET 1,1 i ASP.NET 2,0.

Zażądaj zweryfikowania funkcji ASP.NET od wersji 1,1, uniemożliwiając serwerowi akceptowanie zawartości zawierającej niezakodowany kod HTML. Ta funkcja została zaprojektowana w taki sposób, aby zapobiec atakom polegającym na wstrzyknięciu skryptów, zgodnie z którymi kod skryptu klienta lub HTML może być nieświadomie przesłany do serwera, przechowywany i przedstawiony innym użytkownikom. Zdecydowanie zalecamy, aby sprawdzać poprawność wszystkich danych wejściowych i kodu HTML w razie potrzeby.

Można na przykład utworzyć stronę sieci Web, która żąda adresu e-mail użytkownika, a następnie przechowywać ten adres e-mail w bazie danych. Jeśli użytkownik wprowadzi &lt;skrypt&gt;alertu ("Hello from Script")&lt;/SCRIPT&gt; zamiast poprawnego adresu e-mail, po wyświetleniu tych danych ten skrypt można wykonać, jeśli zawartość nie została prawidłowo zaszyfrowana. Funkcja walidacji żądania ASP.NET zapobiega tym samym.

## <a name="why-this-feature-is-useful"></a>Dlaczego ta funkcja jest przydatna

W wielu lokacjach nie ma informacji, że są one otwarte dla prostych ataków na wstrzyknięcie skryptu. Bez względu na to, czy celem tego ataku jest odtworzenie witryny przez wyświetlanie kodu HTML lub potencjalnie wykonanie skryptu klienta w celu przekierowania użytkownika do lokacji hakera, ataki przed iniekcją skryptów są problemem, który deweloperzy sieci Web muszą będą konkurować o.

Ataki z iniekcją skryptów to zainteresowania wszystkich deweloperów sieci Web, niezależnie od tego, czy korzystają z ASP.NET, ASP, czy innych technologii programistycznych dla sieci Web.

Funkcja walidacji żądań ASP.NET pozwala aktywnie zapobiegać atakom, nie zezwalając na przetworzenie niezakodowanej zawartości HTML na serwerze, chyba że deweloper zdecyduje się zezwolić na tę zawartość.

## <a name="what-to-expect-error-page"></a>Oczekiwane: Strona błędu

Poniższy zrzut ekranu przedstawia przykładowy kod ASP.NET:

![](request-validation/_static/image1.png)

Uruchomienie tego kodu powoduje prostą stronę, która umożliwia wprowadzanie tekstu w polu tekstowym, a następnie kliknięcie tego przycisku i wyświetlenie tekstu w kontrolce etykieta:

![](request-validation/_static/image2.png)

Jednak były to skrypty JavaScript, takie jak `<script>alert("hello!")</script>`, które mają zostać wprowadzone i przesłane, możemy uzyskać wyjątek:

![](request-validation/_static/image3.png)

Komunikat o błędzie stwierdza, że wykryto wartość "potencjalnie niebezpieczne żądanie. formularz" i zawiera więcej szczegółów w opisie, jak dokładnie to, co się stało, i jak zmienić zachowanie. Na przykład:

Podczas sprawdzania poprawności żądania wykryto potencjalnie niebezpieczną wartość wejściową klienta i przetwarzanie żądania zostało przerwane. Ta wartość może wskazywać na próbę złamania zabezpieczeń aplikacji, na przykład atak skryptowy między lokacjami. Sprawdzanie poprawności żądania można wyłączyć, ustawiając `validateRequest=false` w dyrektywie page lub w sekcji konfiguracji. Jednak zdecydowanie zaleca się, aby aplikacja jawnie sprawdzał wszystkie dane wejściowe w tym przypadku.

## <a name="disabling-request-validation-on-a-page"></a>Wyłączanie weryfikacji żądań na stronie

Aby wyłączyć weryfikację żądań na stronie, należy ustawić atrybut `validateRequest` dyrektywy Page na `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Po wyłączeniu walidacji żądania można przesłać zawartość na stronę; Projektant strony jest odpowiedzialny za zapewnienie, że zawartość jest prawidłowo zaszyfrowana lub przetworzona.

## <a name="disabling-request-validation-for-your-application"></a>Wyłączanie weryfikacji żądań dla aplikacji

Aby wyłączyć weryfikację żądań dla aplikacji, należy zmodyfikować lub utworzyć plik Web. config dla aplikacji i ustawić atrybut validateRequest sekcji `<pages />`, aby `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Jeśli chcesz wyłączyć weryfikację żądań dla wszystkich aplikacji na serwerze, możesz wprowadzić tę modyfikację do pliku Machine. config.

> [!CAUTION]
> Gdy weryfikacja żądania jest wyłączona, zawartość może zostać przesłana do aplikacji; Deweloper aplikacji jest odpowiedzialny za zapewnienie, że zawartość jest prawidłowo zakodowana lub przetwarzana.

Poniższy kod został zmodyfikowany, aby wyłączyć weryfikację żądania:

![](request-validation/_static/image4.png)

Teraz, jeśli Poniższy kod JavaScript został wprowadzony do pola tekstowego `<script>alert("hello!")</script>` wynik:

![](request-validation/_static/image5.png)

Aby temu zapobiec, z wyłączeniem walidacji żądania, musimy zakodować zawartość HTML.

## <a name="how-to-html-encode-content"></a>Jak kodować zawartość HTML

Jeśli wyłączono weryfikację żądań, dobrym sposobem jest kodowanie HTML zawartości, która będzie przechowywana do użytku w przyszłości. Kodowanie HTML automatycznie zastępuje wszystkie "&lt;" lub "&gt;" (wraz z innymi innymi symbolami) z odpowiadającą im reprezentacją zakodowaną w kodzie HTML. Na przykład element "&lt;" jest zastępowany przez element "&amp;lt;" i "&gt;" został zastąpiony przez "&amp;gt;". Przeglądarki używają tych specjalnych kodów do wyświetlania "&lt;" lub "&gt;" w przeglądarce.

Zawartość można łatwo zakodować HTML na serwerze za pomocą interfejsu API `Server.HtmlEncode(string)`. Zawartość można również łatwo zdekodować HTML, czyli przywrócić ją z powrotem do standardowego kodu HTML przy użyciu metody `Server.HtmlDecode(string)`.

![](request-validation/_static/image6.png)

Wyniki:

![](request-validation/_static/image7.png)

---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Korzystanie z CAPTCHA w celu uniemożliwienia botów korzystania z witryny ASP.NET Web Razor) | Microsoft Docs
author: microsoft
description: W tym artykule wyjaśniono, jak używać ReCaptcha (miary zabezpieczeń), aby zapobiec wykonywaniu zadań przez automatyczne programy (botów) na stronach sieci Web ASP.NET (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547048"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Korzystanie z CAPTCHA w celu uniemożliwienia botów z używania Twojej witryny ASP.NET Web Razor)

przez [firmę Microsoft](https://github.com/microsoft)

> W tym artykule wyjaśniono, jak używać ReCaptcha (miary zabezpieczeń) w celu zapobiegania wykonywaniu zadań przez automatyczne programy (botów) w witrynie sieci Web ASP.NET (Razor).
> 
> **Dowiesz się:** 
> 
> - Jak dodać test CAPTCHA do lokacji.
> 
> Są to funkcje ASP.NET wprowadzone w artykule:
> 
> - Pomocnik `ReCaptcha`.
> 
> > [!NOTE]
> > Informacje przedstawione w tym artykule dotyczą ASP.NET Web Pages 1,0 i Web Pages 2.

## <a name="about-captchas"></a>Informacje o CAPTCHAs

Za każdym razem, gdy zezwolisz użytkownikom na rejestrację w swojej witrynie, a nawet po prostu wprowadź nazwę i adres URL (na przykład komentarz bloga), możesz zapełnić fałszywe nazwy. Są one często pozostawiane przez zautomatyzowane programy (botów), które próbują opuścić adresy URL w każdej z witryn sieci Web, które mogą znaleźć. (Częstą motywacją jest opublikowanie adresów URL produktów do sprzedaży).

Aby upewnić się, że użytkownik jest osobą rzeczywistą, a nie programem komputerowym, za pomocą *CAPTCHA* można sprawdzić poprawność użytkowników podczas rejestrowania lub wprowadzania ich nazwy i witryny. CAPTCHA oznacza całkowicie zautomatyzowany, publiczny test Turing do informowania komputerów i ludzi. CAPTCHA jest testem *wyzwania-odpowiedzi* , w którym użytkownik jest proszony o to, że jest to bardzo proste dla osoby, które nie jest trudne do wykonania przez zautomatyzowany program. Najczęściej spotykanym typem CAPTCHA jest to, gdzie widzisz pewne zniekształcane litery i prosisz o ich wpisywanie. (Zniekształcenie powinny być trudne do odszyfrowania liter przez botów).

## <a name="adding-a-recaptcha-test"></a>Dodawanie testu ReCaptcha

Na stronach ASP.NET można użyć pomocnika `ReCaptcha`, aby renderować test CAPTCHA, który jest oparty na usłudze ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). Pomocnik `ReCaptcha` wyświetla obraz dwóch zniekształconych wyrazów, które użytkownicy muszą wprowadzić poprawnie przed zweryfikowaniem strony. Odpowiedź użytkownika jest weryfikowana przez usługę ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Zarejestruj swoją witrynę sieci Web pod adresem ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po zakończeniu rejestracji otrzymasz klucz publiczny i klucz prywatny.
2. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).
3. Jeśli nie masz jeszcze pliku *\_AppStart. cshtml* , w folderze głównym witryny sieci Web Utwórz plik o nazwie *\_AppStart. cshtml*.
4. Dodaj następujące `Recaptcha` ustawienia pomocnika w pliku *\_AppStart. cshtml* : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Ustaw `PublicKey` i `PrivateKey` właściwości przy użyciu własnych kluczy publicznych i prywatnych.
6. Zapisz plik *\_AppStart. cshtml* i zamknij go.
7. W folderze głównym witryny sieci Web Utwórz nową stronę o nazwie *reCAPTCHA. cshtml*.
8. Zastąp istniejącą zawartość następującym: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Uruchom stronę *reCAPTCHA. cshtml* w przeglądarce. Jeśli wartość `PrivateKey` jest prawidłowa, na stronie zostanie wyświetlona kontrolka ReCaptcha i przycisk. Jeśli klucz nie został ustawiony globalnie w *\_AppStart. html*, na stronie zostanie wyświetlony komunikat o błędzie. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Wprowadź słowa dla testu. Jeśli przejdziesz test ReCaptcha, zobaczysz komunikat z tym efektem. W przeciwnym razie zostanie wyświetlony komunikat o błędzie, a kontrolka ReCaptcha zostanie wyświetlona ponownie.

> [!NOTE]
> Jeśli komputer znajduje się w domenie, która używa serwera proxy, może być konieczne skonfigurowanie elementu `defaultproxy` pliku *Web. config* . W poniższym przykładzie przedstawiono plik *Web. config* z elementem `defaultproxy` skonfigurowanym tak, aby umożliwić działanie usługi reCAPTCHA.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Dostosowywanie zachowania całej witryny dla witryn ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Witryna ReCaptcha](https://www.google.com/recaptcha)

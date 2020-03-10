---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Zapobieganie atakom typu OpenC#przekierowania () | Microsoft Docs
author: jongalloway
description: W tym samouczku wyjaśniono, jak można zapobiec atakom typu Open przekierowania w aplikacjach ASP.NET MVC. W tym samouczku omówiono wprowadzone zmiany...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: cfa635d4fd14d031993c5b452325cbe334f82dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538172"
---
# <a name="preventing-open-redirection-attacks-c"></a>Zapobieganie atakom na otwarte przekierowywanie (C#)

przez [Jan Galloway](https://github.com/jongalloway)

> W tym samouczku wyjaśniono, jak można zapobiec atakom typu Open przekierowania w aplikacjach ASP.NET MVC. W tym samouczku omówiono zmiany wprowadzone w elementu AccountController w ASP.NET MVC 3 i zademonstrowano, jak można zastosować te zmiany w istniejących aplikacjach ASP.NET MVC 1,0 i 2.

## <a name="what-is-an-open-redirection-attack"></a>Co to jest atak typu "Open przekierowania"?

Dowolna aplikacja sieci Web, która przekierowuje do adresu URL, który jest określony za pośrednictwem żądania, takiego jak QueryString lub dane formularza, może zostać naruszona w celu przekierowania użytkowników do zewnętrznego, złośliwego adresu URL. Takie manipulowanie nazywa się atakiem typu Open przekierowania.

Za każdym razem, gdy logika aplikacji przekieruje do określonego adresu URL, należy sprawdzić, czy adres URL przekierowania nie został naruszony. Logowanie używane w domyślnej elementu AccountController zarówno dla ASP.NET MVC 1,0, jak i ASP.NET MVC 2 jest podatne na ataki typu Open przekierowania. Na szczęście można łatwo aktualizować istniejące aplikacje, aby używać poprawek z wersji zapoznawczej ASP.NET MVC 3.

Aby zrozumieć lukę w zabezpieczeniach, przyjrzyjmy się działaniu przekierowania logowania w domyślnym projekcie aplikacji sieci Web MVC 2 ASP.NET. W tej aplikacji próba odwiedzenia akcji kontrolera, która ma atrybut [autoryzuje], spowoduje przekierowanie nieautoryzowanych użytkowników do widoku/Account/LogOn. To przekierowanie do/Account/LogOn będzie zawierać parametr returnUrl QueryString, dzięki czemu można zwrócić użytkownika do pierwotnie żądanego adresu URL po pomyślnym zalogowaniu się.

Na poniższym zrzucie ekranu można zobaczyć, że próba uzyskania dostępu do widoku/Account/ChangePassword, gdy nie jest zalogowany, spowoduje przekierowanie do/Account/LogOn? ReturnUrl =% 2fAccount% 2fChangePassword% 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Ilustracja 01**: Strona logowania z otwartym przekierowaniem

Ponieważ parametr QueryString ReturnUrl nie jest zweryfikowany, osoba atakująca może ją zmodyfikować, aby wstrzyknąć dowolny adres URL do parametru w celu przeprowadzenia ataku typu Open przekierowania. Aby to zademonstrować, można zmodyfikować parametr ReturnUrl na [http://bing.com](http://bing.com), więc otrzymany adres URL logowania będzie/Account/LogOn? ReturnUrl =<http://www.bing.com/>. Po pomyślnym zalogowaniu się w witrynie nastąpi przekierowanie do [http://bing.com](http://bing.com). Ponieważ przekierowanie nie jest zweryfikowane, może to oznaczać złośliwą witrynę, która próbuje nakłonić użytkownika.

### <a name="a-more-complex-open-redirection-attack"></a>Bardziej skomplikowany atak typu Open przekierowania

Ataki typu Open przekierowania są szczególnie niebezpieczne, ponieważ osoba atakująca wie, że próbujemy zalogować się do określonej witryny sieci Web, co sprawia, że firma Microsoft jest narażona na [ataki wyłudzania informacji](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Na przykład osoba atakująca może wysyłać złośliwe wiadomości e-mail do użytkowników witryny sieci Web przy próbie przechwycenia ich haseł. Przyjrzyjmy się tym, jak to działa w witrynie NerdDinner. (Należy zauważyć, że witryna Live NerdDinner została zaktualizowana w celu ochrony przed atakami typu Open przekierowania).

Najpierw osoba atakująca wysyła nam link do strony logowania w witrynie NerdDinner, która zawiera przekierowanie do sfałszowanej strony:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Zwróć uwagę, że zwrotny adres URL wskazuje na nerddiner.com, w którym brakuje "n" na obiad wyrazu. W tym przykładzie jest to domena, którą osoba atakująca kontroluje. Gdy uzyskujemy dostęp do powyższego linku, nastąpi przekierowanie do strony logowania do NerdDinner.com.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Ilustracja 02**: Strona logowania NerdDinner z otwartym przekierowaniem

Po poprawnym zalogowaniu się akcja logowania ASP.NET MVC elementu AccountController przekierowuje nas do adresu URL określonego w parametrze QueryString returnUrl. W tym przypadku jest to adres URL wprowadzony przez atakującego, który jest [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). O ile nie jest to niezwykle zauważalne, prawdopodobnie nie będzie to konieczne, szczególnie dlatego, że osoba atakująca ma ostrożność, aby upewnić się, że ich sfałszowana strona wygląda podobnie jak legalna Strona logowania. Ta strona logowania zawiera komunikat o błędzie z żądaniem ponownego zalogowania. Clumsy nam, musimy mieć wpisane hasło.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Ilustracja 03**: ekran logowania NerdDinner sfałszowany

Po przepisaniu nazwy użytkownika i hasła na stronie logowania sfałszowane informacje są zapisywane i wysyłane z powrotem do zaufanej witryny NerdDinner.com. W tym momencie lokacja NerdDinner.com została już uwierzytelniona, więc Strona logowania sfałszowana może zostać przekierowana bezpośrednio do tej strony. Oznacza to, że osoba atakująca ma swoją nazwę użytkownika i hasło, a firma Microsoft nie uważa, że została przez nas udostępniona.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Spojrzenie na kod zagrożony w akcji logowania elementu AccountController

Poniżej przedstawiono kod akcji logowania w aplikacji ASP.NET MVC 2. Należy pamiętać, że po pomyślnym zalogowaniu kontroler zwraca przekierowanie do returnUrl. Można sprawdzić, czy nie jest wykonywana żadna Walidacja w odniesieniu do parametru returnUrl.

**Wyświetlanie listy 1 – ASP.NET akcji logowania MVC 2 w `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Teraz przyjrzyjmy się zmianom akcji logowania ASP.NET MVC 3. Ten kod został zmieniony, aby sprawdzić poprawność parametru returnUrl przez wywołanie nowej metody w klasie pomocnika system. Web. MVC. URL o nazwie `IsLocalUrl()`.

**Lista 2 – ASP.NET akcji logowania MVC 3 w `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Ten element został zmieniony w celu zweryfikowania parametru zwrotnego adresu URL, wywołując nową metodę w klasie pomocnika system. Web. MVC. URL `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Ochrona aplikacji ASP.NET MVC 1,0 i MVC 2

Możemy wykorzystać zmiany ASP.NET MVC 3 w istniejących aplikacjach ASP.NET MVC 1,0 i 2 przez dodanie metody pomocnika IsLocalUrl () i zaktualizowanie akcji logowania w celu sprawdzenia poprawności parametru returnUrl.

Metoda UrlHelper IsLocalUrl () faktycznie wywołuje metodę w programie System. Web. Webpages, ponieważ ta Walidacja jest również używana przez aplikacje ASP.NET Web Pages.

**Lista 3 — Metoda IsLocalUrl () z ASP.NET MVC 3 UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Metoda IsUrlLocalToHost zawiera rzeczywistą logikę walidacji, jak pokazano na liście 4.

**Wyświetlanie listy 4 – IsUrlLocalToHost () metody z klasy System. Web. Webpages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

W naszej aplikacji ASP.NET MVC 1,0 lub 2 dodamy do elementu AccountController metodę IsLocalUrl (), ale zaleca się dodanie jej do innej klasy pomocnika, jeśli to możliwe. Wprowadzimy dwie małe zmiany w wersji ASP.NET MVC 3 IsLocalUrl (), dzięki czemu będzie ona działała wewnątrz elementu AccountController. Najpierw zmienimy ją z metody publicznej na prywatną metodę, ponieważ metody publiczne w kontrolerach są dostępne jako akcje kontrolera. Następnie zmodyfikujemy wywołanie sprawdzające Host adresu URL względem hosta aplikacji. To wywołanie umożliwia użycie lokalnego pola RequestContext w klasie UrlHelper. Zamiast tego. RequestContext. HttpContext. Request. URL. host, użyjemy tego. Request. URL. host. Poniższy kod przedstawia zmodyfikowaną metodę IsLocalUrl () do użycia z klasą kontrolera w aplikacjach ASP.NET MVC 1,0 i 2.

**Lista 5 — Metoda IsLocalUrl (), która jest modyfikowana do użycia z klasą kontrolera MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Teraz, gdy metoda IsLocalUrl () jest na miejscu, możemy wywołać ją z naszej akcji logowania, aby sprawdzić poprawność parametru returnUrl, jak pokazano w poniższym kodzie.

**Lista 6 — zaktualizowana metoda logowania, która sprawdza poprawność parametru returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Teraz możemy przetestować atak typu Open przekierowania, próbując zalogować się przy użyciu zewnętrznego zwrotnego adresu URL. Użyjmy/Account/LogOn? ReturnUrl =<http://www.bing.com/> ponownie.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Rysunek 04**: testowanie zaktualizowanej akcji logowania

Po pomyślnym zalogowaniu zostanie przekierowany do akcji kontrolera domu/indeksu, a nie zewnętrznego adresu URL.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Ilustracja 05**. podlegające atakom typu "Otwieranie przekierowania"

## <a name="summary"></a>Podsumowanie

Ataki typu Open przekierowania mogą wystąpić, gdy adresy URL przekierowania są przesyłane jako parametry w adresie URL aplikacji. Szablon ASP.NET MVC 3 zawiera kod chroniący przed atakami typu Open przekierowania. Można dodać ten kod z modyfikacją ASP.NET MVC 1,0 i 2 aplikacji. Aby chronić przed atakami typu "Otwieranie przekierowań" podczas logowania się do aplikacji ASP.NET 1,0 i 2, Dodaj metodę IsLocalUrl () i sprawdź poprawność parametru returnUrl w akcji logowania.

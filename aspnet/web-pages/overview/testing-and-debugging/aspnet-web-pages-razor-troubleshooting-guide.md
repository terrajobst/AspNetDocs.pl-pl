---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Przewodnik rozwiązywania problemów z ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano problemy, które mogą wystąpić podczas pracy z ASP.NET Web Pages (Razor) i proponowanymi rozwiązaniami. Wersje oprogramowania ASP.NET Web strona...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585765"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano problemy, które mogą wystąpić podczas pracy z ASP.NET Web Pages (Razor) i proponowanymi rozwiązaniami.
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2 i ASP.NET Web Pages 1,0.

Ten temat zawiera następujące sekcje:

- [Problemy z uruchomionymi stronami](#Issues_Running_.cshtml_Pages)
- [Problemy związane z kodem Razor](#IssuesWithRazorCode)
- [Problemy z zabezpieczeniami i członkostwem](#membership)
- [Problemy z wysyłaniem poczty E-mail](#email)
- [Dodatkowe zasoby](#AdditionalResources)

Ogólne pytania można znaleźć w temacie [ASP.NET Web Pages (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problemy z uruchomionymi stronami

Różne problemy mogą uniemożliwiać poprawne działanie stron *. cshtml* i *. vbhtml* . W tej sekcji wymieniono typowe komunikaty o błędach i możliwe przyczyny.

### <a name="http-error-403---forbidden-access-is-denied"></a>Błąd HTTP 403 — Dostęp zabroniony: odmowa dostępu

*Nie masz uprawnień do wyświetlenia tego katalogu lub strony przy użyciu podanych poświadczeń.*

Ten błąd może wystąpić, jeśli na serwerze nie jest uruchomiona poprawna wersja .NET Framework. Upewnij się, że na komputerze, na którym działa serwer (lokalnie lub zdalnie), jest zainstalowany co najmniej .NET Framework 4. Upewnij się również, że sama aplikacja jest skonfigurowana do uruchamiania odpowiedniej wersji.

Jeśli ten problem występuje lokalnie podczas pracy w programie WebMatrix, kliknij obszar roboczy **lokacja** , a następnie w widoku drzewa kliknij pozycję **Ustawienia**. Na liście **Wybierz wersję .NET Framework** wybierz pozycję **.NET 4 (zintegrowane)** . Jeśli ta wersja jest już ustawiona, spróbuj uruchomić program WebMatrix jako administrator.

Upewnij się, że w katalogu głównym witryny sieci Web znajduje się przynajmniej jeden plik *. cshtml* .

Jeśli ten błąd występuje, gdy serwer sieci Web znajduje się na serwerze zdalnym, skontaktuj się z administratorem serwera. Upewnij się, że na serwerze jest zainstalowany .NET Framework 4 lub nowszy. Upewnij się również, że aplikacja działa w puli aplikacji, która jest skonfigurowana do korzystania z tej wersji platformy the.NET Framework.

Jeśli masz kontrolę nad serwerem, upewnij się, że jest uruchomiona poprawna wersja .NET Framework. Możesz również spróbować naprawić instalację, uruchamiając polecenie `aspnet_regiis -iru`. (Na przykład po zainstalowaniu usług IIS po zainstalowaniu .NET Framework usługi IIS nie będą prawidłowo skonfigurowane do uruchamiania stron ASP.NET). Aby uzyskać więcej informacji, zobacz [ASP.NET narzędzia rejestracji usług IIS (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Błąd HTTP 403,14 — dostęp zabroniony

*Serwer sieci Web jest skonfigurowany do wyświetlania zawartości tego katalogu.*

Ten błąd może wystąpić, Jeśli zażądasz zasobu, który jest chroniony (taki jak plik *Web. config* ) lub który znajduje się w folderze chronionym (na przykład *aplikacja\_dane* lub *kod\_aplikacji*).

### <a name="http-error-40417---not-found"></a>Błąd HTTP 404,17 — nie znaleziono

*Żądana zawartość wydaje się być skryptem i nie będzie obsługiwana przez procedurę obsługi pliku statycznego.*

Ten błąd może wystąpić, jeśli serwer nie jest prawidłowo skonfigurowany do korzystania z .NET Framework 4 lub nowszego, w związku z czym nie rozpoznaje kodu w blokach `@{ }`. Zobacz opis wcześniejszy dla *błędu HTTP 403 — Dostęp zabroniony: odmowa dostępu*.

### <a name="http-error-4047---not-found"></a>Błąd HTTP 404,7 — nie znaleziono

*W module filtrowania żądań skonfigurowano odrzucanie rozszerzenia pliku*

Ten błąd może wystąpić, jeśli rozszerzenia *cshtml* lub *VBHTML* zostały jawnie zablokowane na serwerze. Objawem tego problemu jest to, że adresy URL działają, gdy nie zawierają rozszerzenia, ale nie działają adresy URL zawierające *. cshtml* lub *. vbhtml* . Możliwe rozwiązanie to ponowne włączenie rozszerzeń w pliku *Web. config* witryny. Poniższy przykład pokazuje, jak włączyć rozszerzenie *. cshtml* .

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Błąd HTTP 404,8 — nie znaleziono

*W module filtrowania żądań skonfigurowano odrzucanie ścieżki w adresie URL zawierającym sekcję hiddenSegment.*

Ten błąd może wystąpić, Jeśli zażądasz zasobu, który jest chroniony (taki jak plik *Web. config* ) lub który znajduje się w folderze chronionym (na przykład *aplikacja\_dane* lub *kod\_aplikacji*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Ten typ strony nie jest obsługiwany (błąd serwera w aplikacji "/")

Zobacz opis wcześniejszy dla błędu HTTP 404,17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problemy związane z kodem Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Nazwa "*Class*" nie istnieje w bieżącym kontekście

Często przyczyną tego błędu jest to, że `class` odwołuje się do pomocnika, ale pomocnik nie jest zainstalowany. Na przykład jeśli spróbujesz użyć pomocnika, ale jeśli nie zainstalowano pakietu z narzędzia NuGet, zobaczysz ten błąd. Aby znaleźć i zainstalować pomocnika, Skorzystaj z galerii w programie WebMatrix.

Jeśli zainstalowano pomocnika, ale strona nadal nie rozpoznaje go, spróbuj dodać instrukcję dodawania `using` do kodu. W instrukcji `using` odwołuje się do przestrzeni nazw, która zawiera pomocnika. Na przykład podstawowe pomocnicy, które znajdują się w pakiecie pomocników sieci Web ASP.NET, znajdują się w przestrzeni nazw `System.Web.Helpers`. W górnej części strony, w której chcesz korzystać z pomocnika, Dodaj następujący wiersz:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problemy z zabezpieczeniami i członkostwem

W przypadku korzystania z wbudowanego systemu zabezpieczeń (Membership) na stronach sieci Web ASP.NET (Razor) mogą wystąpić następujące problemy.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Aby wywołać tę metodę, właściwość "Membership. Provider" musi być wystąpieniem elementu "ExtendedMembershipProvider"

Ten błąd może wskazywać, że nie skonfigurowano żadnej klasy `AspNetSqlMembershipProvider`. (Objaw polega na tym, że lokacja działa prawidłowo lokalnie, ale zgłasza ten błąd podczas publikowania go na serwerze dostawcy hostingu). Jedną z następujących przyczyn tego problemu jest jawne włączenie prostej przynależności do pliku *Web. config* witryny:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problemy z wysyłaniem poczty E-mail

Problemy z wysyłaniem wiadomości e-mail mogą być trudne do debugowania. Początkowy problem może być niemożliwa, ponieważ nie można nawiązać połączenia z serwerem SMTP. Jeśli połączenie zakończy się pomyślnie, ASP.NET komunikat do serwera SMTP. Mogą jednak występować problemy z samym komunikatem, który uniemożliwia serwerowi SMTP wysyłanie go.

Jeśli aplikacja nie wyśle wiadomości e-mail pomyślnie, spróbuj wykonać następujące czynności:

- Nazwa serwera SMTP często jest taka sama jak `smtp.provider.com` lub `smtp.provider.net`. Jednak w przypadku opublikowania lokacji w ramach dostawcy hostingu nazwa serwera SMTP może być `localhost`. Ta sytuacja występuje, ponieważ po opublikowaniu i uruchomieniu lokacji na serwerze dostawcy serwer SMTP może być lokalnym z perspektywy aplikacji. Ta zmiana nazw serwerów może oznaczać konieczność zmiany nazwy serwera SMTP w ramach procesu publikowania.
- Numer portu to zwykle 25. Jednak niektórzy dostawcy wymagają użycia portu 587 lub innego portu. Należy skontaktować się z właścicielem serwera SMTP, jakiego numeru portu należy używać.
- Upewnij się, że używasz odpowiednich poświadczeń. Jeśli witryna została opublikowana w ramach dostawcy hostingu, Użyj poświadczeń, które zostały wskazane przez dostawcę jako wiadomości e-mail. Te poświadczenia mogą się różnić od poświadczeń, które są używane do publikowania.
- Czasami nie potrzebujesz poświadczeń. W przypadku wysyłania wiadomości e-mail przy użyciu osobistego usługodawcy internetowego Twój dostawca poczty e-mail może już znać Twoje poświadczenia. Po opublikowaniu programu może być konieczne użycie innych poświadczeń niż podczas testowania na komputerze lokalnym.
- Jeśli Twój dostawca poczty e-mail używa szyfrowania, ustaw `WebMail.EnableSsl` na `true`.

Jeśli wystąpi błąd podczas wysyłania wiadomości e-mail, może zostać wyświetlony standardowy komunikat o błędzie ASP.NET, który wygląda następująco:

![ASP.NET komunikat o błędzie, gdy występuje problem z wiadomościami e-mail](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Możesz również debugować problemy z wysyłaniem wiadomości e-mail przy użyciu bloku `try-catch`, jak w poniższym przykładzie. W przypadku korzystania z bloku `try-catch` ASP.NET nie wyświetla standardowych komunikatów o błędach. Zamiast tego można przechwycić błąd w części `catch` bloku.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Zastąp odpowiednie wartości `your-SMTP-server-name`i tak dalej. Oto niektóre z komunikatów o błędach, które mogą zostać wyświetlone w następujący sposób:

- *Niepowodzenie wysyłania wiadomości e-mail.*

    lub

    *Próba nawiązania połączenia nie powiodła się, ponieważ połączona Strona nie odpowiedziała prawidłowo po upływie określonego czasu lub nawiązane połączenie nie powiodło się, ponieważ podłączony host nie odpowiedział*

    Ten błąd zazwyczaj oznacza, że aplikacja nie może połączyć się z serwerem SMTP. Sprawdź nazwę serwera i numer portu.
- *Skrzynka pocztowa nie jest dostępna. Odpowiedź serwera to: 5.1.0 &lt;someuser@invaliddomain&gt; Sender odrzucone: nieprawidłowa domena nadawcy*

    Ten komunikat może wskazywać, że adres `From` jest niepoprawny lub nie istnieje.
- *Określony ciąg nie ma formy wymaganej dla adresu e-mail.*

    Ten błąd może wskazywać, że wartości właściwości `To` lub `From` nie są rozpoznawane jako adresy e-mail. (ASP.NET nie może sprawdzić, czy adres e-mail jest prawidłowy, czy jest w poprawnym formacie, na przykład *name@domain.com* .)

> [!NOTE]
> Usuń znacznik, który wyświetla błąd (`@errorMessage`) przed opublikowaniem strony w działającej witrynie. Nie jest dobrym pomysłem, aby umożliwić użytkownikom wyświetlanie komunikatów o błędach uzyskanych z serwera.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[ASP.NET Web Pages (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix i ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum w witrynie ASP.NET

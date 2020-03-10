---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Dodawanie zabezpieczeń i członkostwa do witryny ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale przedstawiono sposób zabezpieczania witryny sieci Web w taki sposób, aby niektóre strony były dostępne tylko dla osób logujących się. (Zobacz też, jak tworzyć strony określona...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 0be3e767a42939a3c343f6d4a730eb1d9a6b367c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628388"
---
# <a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Dodawanie zabezpieczeń i członkostwa do witryny ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak zabezpieczyć witrynę sieci Web ASP.NET (Razor) w taki sposób, aby niektóre strony były dostępne tylko dla osób logujących się. (Zobaczysz również, jak utworzyć strony, do których każdy może uzyskać dostęp).
> 
> **Dowiesz się:** 
> 
> - Jak utworzyć witrynę sieci Web, która ma stronę rejestracji i stronę logowania, tak aby w przypadku niektórych stron można było ograniczyć dostęp tylko do elementów członkowskich.
> - Jak utworzyć publiczne i tylko Członkowskie strony.
> - Jak zdefiniować role, które są grupami, które mają różne uprawnienia zabezpieczeń w danej lokacji, oraz sposób przypisywania użytkowników do roli.
> - Jak używać CAPTCHA do zapobiegania automatycznemu programowi (botów) tworzenia kont członków.
>   
> 
> Są to funkcje ASP.NET wprowadzone w artykule:
> 
> - Szablon witryny programu WebMatrix **Starter** .
> - Pomocnik `WebSecurity` i Klasa `Roles`.
> - Pomocnik `ReCaptcha`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - WebMatrix 3
> - Biblioteka pomocników sieci Web ASP.NET

Możesz skonfigurować witrynę sieci Web tak, aby użytkownicy mogli się do niej &#8212; zalogować, aby witryna obsługiwała *członkostwo*. Może to być przydatne z wielu powodów. Na przykład witryna może zawierać strony, które powinny być dostępne tylko dla członków. W niektórych przypadkach może być konieczne zalogowanie się użytkowników w celu wysłania opinii lub opuszczenia komentarza.

Nawet jeśli witryna internetowa obsługuje członkostwo, użytkownicy nie muszą być musieli zalogować się przed użyciem niektórych stron w witrynie. Użytkownicy, którzy nie są zalogowani, są znani jako *Użytkownicy anonimowi*.

Użytkownik może zarejestrować się w witrynie sieci Web, a następnie zalogować się do witryny. Witryna sieci Web wymaga nazwy użytkownika (adresu e-mail) i hasła, aby potwierdzić, że użytkownicy są do nich potwierdzą. Ten proces logowania i potwierdzania tożsamości użytkownika jest znany jako *uwierzytelnianie*.

Możesz skonfigurować zabezpieczenia i członkostwo na różne sposoby:

- Jeśli używasz programu WebMatrix, prosta metoda polega na utworzeniu nowej witryny na podstawie szablonu **witryny Starter** . Ten szablon jest już skonfigurowany pod kątem zabezpieczeń i członkostwa, ma już stronę rejestracji, stronę logowania i tak dalej.

    Witryna utworzona przez szablon zawiera również opcję umożliwiającą użytkownikom logowanie się przy użyciu zewnętrznej witryny, takiej jak Facebook, Google lub Twitter.
- Jeśli chcesz dodać zabezpieczenia do istniejącej lokacji lub jeśli nie chcesz używać szablonu **witryny Starter** , możesz utworzyć własną stronę rejestracji, stronę logowania i tak dalej.

Ten artykuł koncentruje się na pierwszej opcji, &mdash; jak dodać zabezpieczenia przy użyciu szablonu **witryny Starter** . Zawiera on również podstawowe informacje dotyczące implementowania własnych zabezpieczeń, a następnie zawiera linki do dodatkowych informacji na temat tego, jak to zrobić. Informacje o sposobie włączania zewnętrznych nazw logowania, które opisano szczegółowo w osobnym artykule.

## <a name="creating-website-security-using-the-starter-site-template"></a>Tworzenie zabezpieczeń witryny sieci Web przy użyciu szablonu witryny Starter

W programie WebMatrix można użyć szablonu **witryny Starter** , aby utworzyć witrynę sieci Web, która zawiera następujące elementy:

- Baza danych, która jest używana do przechowywania nazw użytkowników i haseł dla członków.
- Strona rejestracji, na której można zarejestrować anonimowe (nowe) użytkowników.
- Strona logowania i wylogowania.
- Strona odzyskiwania i resetowania hasła.

Poniższa procedura opisuje sposób tworzenia lokacji i konfigurowania jej.

1. Uruchom program WebMatrix i na stronie **Szybki Start** wybierz pozycję **Witryna z szablonu**.
2. Wybierz szablon **lokacja początkowa** , a następnie kliknij przycisk **OK**. W programie WebMatrix zostanie utworzona nowa witryna.
3. W lewym okienku kliknij selektor obszaru roboczego **pliki** .
4. W folderze głównym witryny sieci Web otwórz plik *\_AppStart. cshtml* , który jest plikiem specjalnym, który jest używany do przechowywania ustawień globalnych. Zawiera on kilka instrukcji, które są oznaczone jako komentarze przy użyciu znaków `//`:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Te instrukcje służą do konfigurowania pomocnika `WebMail`, który może służyć do wysyłania wiadomości e-mail. System członkostwa może wysyłać wiadomości e-mail z potwierdzeniem, gdy użytkownicy zarejestrują się lub kiedy chcą zmienić hasła. (Na przykład po zarejestrowaniu się użytkowników otrzymasz wiadomość e-mail zawierającą link, który można kliknąć, aby zakończyć proces rejestracji).

    Wysyłanie poczty e-mail wymaga dostępu do serwera SMTP, zgodnie z opisem w temacie [Dodawanie poczty e-mail do witryny ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202899). Ustawienia poczty e-mail będą przechowywane w tym centralnym pliku *\_AppStart. cshtml* , aby nie trzeba było ich wielokrotnie zakodować do każdej strony, która może wysyłać wiadomości e-mail. (Nie trzeba konfigurować ustawień SMTP w celu skonfigurowania bazy danych rejestracji; wymagane są tylko ustawienia protokołu SMTP, jeśli chcesz sprawdzić poprawność użytkowników z aliasu poczty e-mail i pozwolić użytkownikom na resetowanie zapomnianego hasła).
5. Usuń komentarz z instrukcji, usuwając `//` z przed każdym z nich.

    Jeśli nie chcesz konfigurować potwierdzenia wiadomości e-mail, możesz pominąć ten krok i następny krok. Jeśli wartości SMTP nie są ustawione, nowe konto jest natychmiast dostępne bez potwierdzenia wiadomości e-mail.
6. Zmodyfikuj następujące ustawienia związane z pocztą e-mail w kodzie:

   - Ustaw `WebMail.SmtpServer` na nazwę serwera SMTP, do którego masz dostęp.
   - Pozostaw `WebMail.EnableSsl` ustawiony na `true`. To ustawienie zabezpiecza poświadczenia wysyłane do serwera SMTP przez ich szyfrowanie.
   - Ustaw `WebMail.UserName` na nazwę użytkownika dla konta serwera SMTP.
   - Ustaw `WebMail.Password` na hasło dla konta serwera SMTP.
   - Ustaw `WebMail.From` na własny adres e-mail. Jest to adres e-mail, z którego wiadomość jest wysyłana.

     > [!NOTE] 
     > 
     > **Porada** Aby uzyskać dodatkowe informacje na temat wartości tych właściwości, zobacz [Konfigurowanie ustawień poczty e-mail](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) w obszarze [Dostosowywanie zachowania całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Zapisz i Zamknij *\_AppStart. cshtml*.
8. Uruchom *domyślną stronę. cshtml* w przeglądarce.

    ![zabezpieczenia — członkostwo 2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Jeśli zostanie wyświetlony komunikat o błędzie informujący o tym, że właściwość musi być wystąpieniem `ExtendedMembershipProvider`, lokacja może nie być skonfigurowana do używania systemu członkostwa ASP.NET stron sieci Web (SimpleMembership). Może się tak zdarzyć, gdy serwer dostawcy hostingu jest skonfigurowany inaczej niż serwer lokalny. Aby rozwiązać ten problem, Dodaj następujący element do pliku *Web. config* witryny:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Dodaj ten element jako element podrzędny elementu `<configuration>` i jako element równorzędny elementu `<system.web>`.
9. W prawym górnym rogu strony kliknij link **zarejestruj** . Zostanie wyświetlona strona *register. cshtml* .
10. Wprowadź nazwę użytkownika i hasło, a następnie kliknij pozycję **zarejestruj**.

    ![zabezpieczenia — członkostwo 3](16-adding-security-and-membership/_static/image2.png)

    Po utworzeniu witryny sieci Web z poziomu szablonu **witryny startowej** baza danych o nazwie *StarterSite. sdf* została utworzona w folderze *danych\_aplikacji* . Podczas rejestracji informacje o użytkowniku są dodawane do bazy danych programu. W przypadku ustawienia wartości SMTP komunikat jest wysyłany na używany adres e-mail, aby można było zakończyć rejestrację.

    ![security-membership-4](16-adding-security-and-membership/_static/image3.png)
11. Przejdź do programu poczty e-mail i Znajdź komunikat, który będzie zawierał kod potwierdzenia i hiperłącze do witryny.
12. Kliknij hiperlink, aby aktywować Twoje konto. Hiperłącze potwierdzenia otwiera stronę potwierdzenia rejestracji.

    ![zabezpieczenia — członkostwo 5](16-adding-security-and-membership/_static/image4.png)
13. Kliknij link **Zaloguj** , a następnie zaloguj się przy użyciu zarejestrowanego konta.

      Po zalogowaniu łącza **logowania** i **rejestrowania** są zastępowane przez łącze do **wylogowania** . Nazwa logowania jest wyświetlana jako link. (Link umożliwia przejście do strony, na której można zmienić hasło).

      ![zabezpieczenia — członkostwo — 6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Domyślnie ASP.NET strony sieci Web wysyłają poświadczenia do serwera w postaci zwykłego tekstu (jako tekst czytelny dla użytkownika). Lokacja produkcyjna powinna używać bezpiecznego protokołu HTTP (https://, znanego również jako *Secure Sockets Layer* lub SSL), aby szyfrować poufne informacje wymieniane z serwerem. Można wysyłać wymagane wiadomości e-mail przy użyciu protokołu SSL, ustawiając `WebMail.EnableSsl=true` jak w poprzednim przykładzie. Aby uzyskać więcej informacji na temat protokołu SSL, zobacz [Zabezpieczanie komunikacji w sieci Web: Certificates, SSL i https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Dodatkowe funkcje członkostwa w lokacji

Lokacja zawiera inne funkcje, które umożliwiają użytkownikom zarządzanie swoimi kontami. Użytkownicy mogą wykonywać następujące czynności:

- Zmień swoje hasła. Po zalogowaniu się można kliknąć nazwę użytkownika (czyli link). Spowoduje to przejście do strony, na której można utworzyć nowe hasło (*Account/ChangePassword. cshtml*).
- Odzyskaj zapomniane hasło. Na stronie logowania znajduje się łącze (**czy zapomnisz hasła?** ), które umożliwia użytkownikom na stronie (*Account/ForgotPassword. cshtml*) wprowadzanie adresu e-mail. Witryna wysyła do nich wiadomość e-mail zawierającą link, który można kliknąć, aby ustawić nowe hasło (*Account/PasswordReset. cshtml*).

Możesz również pozwolić użytkownikom na logowanie się przy użyciu zewnętrznej witryny, jak wyjaśniono w dalszej części.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Tworzenie strony tylko członkowskiej

W danym momencie każdy może przejść do dowolnej strony w witrynie sieci Web. Może jednak być konieczne posiadanie stron, które są dostępne tylko dla osób zalogowanych (czyli do członków). ASP.NET umożliwia tworzenie stron, do których można uzyskać dostęp tylko przez zalogowanych członków. Typowo, jeśli użytkownicy anonimowi próbują uzyskać dostęp do strony tylko członkowie, przekierują je na stronę logowania.

W tej procedurze utworzysz folder, który będzie zawierać strony, które są dostępne tylko dla zalogowanych użytkowników.

1. W katalogu głównym lokacji Utwórz nowy folder. (Na wstążce kliknij strzałkę pod **nową** , a następnie wybierz pozycję **Nowy folder**).
2. Nadaj nazwę nowym *członkom*folderu.
3. W folderze *Członkowie* Utwórz nową stronę o nazwie *MembersInformation. cshtml*.
4. Zastąp istniejącą zawartość następującym kodem i znacznikiem:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Ten kod sprawdza Właściwość `IsAuthenticated` obiektu `WebSecurity`, która zwraca `true`, jeśli użytkownik zalogował się. Jeśli użytkownik nie jest zalogowany, kod wywołuje `Response.Redirect`, aby wysłać użytkownika do strony *login. cshtml* w folderze *Account* .

    Adres URL przekierowania zawiera `returnUrl` wartość ciągu zapytania, która używa `Request.Url.LocalPath` do ustawiania ścieżki bieżącej strony. Jeśli ustawisz wartość `returnUrl` w ciągu zapytania, na przykład w tym (a jeśli adres URL powrotu jest ścieżką lokalną), Strona logowania zwróci użytkowników na Tę stronę po zalogowaniu się.

    Kod ustawia również *\_strony SiteLayout. cshtml* jako stronę układu. (Aby uzyskać więcej informacji o stronach układu, zobacz [Tworzenie spójnego układu w witrynach stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Uruchom lokację programu. Jeśli użytkownik jest nadal zalogowany, kliknij przycisk **Wyloguj** w górnej części strony.
6. W przeglądarce Zażądaj strony */Members/MembersInformation*. Na przykład adres URL może wyglądać następująco:

    `http://localhost:38366/Members/MembersInformation`

    (Numer portu (38366) prawdopodobnie będzie inny w adresie URL.

    Nastąpi przekierowanie do strony *login. cshtml* , ponieważ nie zalogowano się.
7. Zaloguj się przy użyciu utworzonego wcześniej konta. Nastąpi przekierowanie do strony *MembersInformation* . Ponieważ użytkownik jest zalogowany, zobaczysz zawartość strony.

Aby zabezpieczyć dostęp do wielu stron, można to zrobić:

- Dodaj sprawdzanie zabezpieczeń do każdej strony.
- Utwórz stronę *\_PageStart. cshtml* w folderze, w którym są przechowywane chronione strony, a następnie Dodaj sprawdzanie zabezpieczeń. Strona *\_PageStart. cshtml* działa jako rodzaj strony globalnej dla wszystkich stron w folderze. Ta technika została omówiona bardziej szczegółowo w temacie [Dostosowywanie zachowania dla całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Tworzenie zabezpieczeń dla grup użytkowników (ról)

Jeśli witryna ma wiele elementów członkowskich, nie ma potrzeby sprawdzania uprawnień dla każdego użytkownika indywidualnie przed wyświetleniem strony. Zamiast tego można utworzyć grupy lub *role*, do których należą poszczególne składowe. Następnie można sprawdzić uprawnienia na podstawie roli. W tej sekcji utworzysz rolę&quot; administrator &quot;, a następnie utworzysz stronę, która jest dostępna dla użytkowników należących do tej roli (należących do niej).

System członkostwa ASP.NET jest skonfigurowany do obsługi ról. Niemniej jednak, w przeciwieństwie do rejestracji członkostwa i logowania, szablon **witryny Starter** nie zawiera stron, które ułatwiają zarządzanie rolami. (Zarządzanie rolami to zadanie administracyjne, a nie zadanie użytkownika). Można jednak dodawać grupy bezpośrednio w bazie danych członkostwa w programie WebMatrix.

1. W programie WebMatrix kliknij selektor obszaru roboczego **bazy danych** .
2. W lewym okienku Otwórz węzeł *StarterSite. sdf* , Otwórz węzeł **tabele** , a następnie kliknij dwukrotnie pozycję tabela *stron sieci Web\_role* .

    ![zabezpieczenia — członkostwo-7](16-adding-security-and-membership/_static/image6.png)
3. Dodaj rolę o nazwie &quot;administrator&quot;. Pole *RoleId* jest wypełniane automatycznie. (Jest to klucz podstawowy i został ustawiony jako pole identyfikacji, jak wyjaśniono w temacie [wprowadzenie do pracy z bazą danych w witrynach sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Zwróć uwagę na to, co jest wartością dla pola *RoleId* . (Jeśli jest to pierwsza definiowana rola, będzie ona 1).

    ![zabezpieczenia — członkostwo-8](16-adding-security-and-membership/_static/image7.png)
5. Zamknij tabelę *ról\_strony sieci Web* .
6. Otwórz tabelę *profilów* użytkownika.
7. Zanotuj wartość parametru *UserID* jednego lub większej liczby użytkowników w tabeli, a następnie zamknij tabelę.
8. Otwórz tabelę *webpages\_UserInRoles* i wprowadź w tabeli wartość *UserID* oraz *RoleID* . Na przykład, aby wprowadzić użytkownika 2 do roli&quot; administrator &quot;, wprowadź następujące wartości:

    ![zabezpieczenia — członkostwo-9](16-adding-security-and-membership/_static/image8.png)
9. Zamknij tabelę *webpages\_UsersInRoles* .

    Teraz, gdy masz zdefiniowane role, możesz skonfigurować stronę, która jest dostępna dla użytkowników należących do tej roli.
10. W folderze głównym witryny sieci Web Utwórz nową stronę o nazwie *AdminError. cshtml* i Zastąp istniejącą zawartość następującym kodem. Będzie to strona, do której użytkownicy są przekierowywani, jeśli nie mają uprawnień dostępu do strony.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. W folderze głównym witryny sieci Web Utwórz nową stronę o nazwie *AdminOnly. cshtml* i Zastąp istniejący kod następującym kodem:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Metoda `Roles.IsUserInRole` zwraca `true`, jeśli bieżący użytkownik jest członkiem określonej roli (w tym przypadku rolą "Administrator").
12. Uruchom w przeglądarce polecenie *default. cshtml* , ale nie Zaloguj się. (Jeśli użytkownik jest już zalogowany, Wyloguj się).
13. Na pasku adresu przeglądarki Dodaj *AdminOnly* w adresie URL. (Innymi słowy, zażądaj pliku *AdminOnly. cshtml* ). Nastąpi przekierowanie do strony *AdminError. cshtml* , ponieważ obecnie nie jesteś zalogowany jako użytkownik w roli&quot; administrator &quot;.
14. Wróć do *default. cshtml* i zaloguj się jako użytkownik dodany do roli&quot; administrator &quot;.
15. Przejdź do strony *AdminOnly. cshtml* . Ta strona zostanie wyświetlona.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Zapobieganie dołączeniu automatycznych programów do witryny sieci Web

Strona logowania nie będzie zatrzymywać automatycznych programów (czasami nazywanych w *sieci Web robotów* lub *botów*) na potrzeby rejestrowania w witrynie sieci Web. W tej procedurze opisano, jak włączyć test ReCaptcha na stronie rejestracji.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Zarejestruj swoją witrynę sieci Web pod adresem ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po zakończeniu rejestracji otrzymasz klucz publiczny i klucz prywatny.
2. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).
3. W folderze *konto* Otwórz plik o nazwie *register. cshtml*.
4. W kodzie w górnej części strony Znajdź następujące wiersze i Usuń ich komentarz, usuwając `//` znaki komentarza:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Zastąp `PRIVATE_KEY` własnym kluczem prywatnym ReCaptcha.
6. W znaczniku strony Usuń `@*` i `*@` znaki komentarza z dookoła następujących wierszy w znaczniku strony:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Zastąp `PUBLIC_KEY` kluczem.
8. Jeśli jeszcze tego nie zrobiono, Usuń element `<div>`, który zawiera tekst rozpoczynający się od "aby włączyć weryfikację CAPTCHA...". (Usuń cały `<div>` element i jego zawartość).

9. Uruchom w przeglądarce *wartość default. cshtml* . Jeśli użytkownik jest zalogowany do witryny, kliknij link **Wyloguj** .
10. Kliknij link **zarejestruj** i przetestuj rejestrację przy użyciu testu CAPTCHA.

     ![zabezpieczenia — członkostwo — 10](16-adding-security-and-membership/_static/image9.png)

Aby uzyskać więcej informacji na temat pomocnika `ReCaptcha`, zobacz [Korzystanie z Catpcha w celu zapobiegania automatycznemu programowi (botów) korzystania z witryny sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Zezwalanie użytkownikom na logowanie się przy użyciu zewnętrznej witryny

Szablon **witryny Starter** zawiera kod i znaczniki umożliwiające użytkownikom logowanie się przy użyciu usług Facebook, Windows Live, Twitter, Google lub Yahoo. Domyślnie ta funkcja nie jest włączona. Ogólna procedura korzystania z umożliwiania użytkownikom logowania się przy użyciu tych zewnętrznych dostawców to:

- Zdecyduj, które lokacje zewnętrzne mają być obsługiwane.
- W razie potrzeby przejdź do tej witryny i skonfiguruj aplikację logowania. (Na przykład trzeba to zrobić, aby zezwolić na logowanie do serwisu Facebook).
- W danej lokacji Skonfiguruj dostawcę. W większości przypadków trzeba tylko usunąć komentarz z kodu w pliku *\_AppStart. cshtml* .
- Dodaj adiustację do strony rejestracji, która umożliwia użytkownikom łączenie się z witryną zewnętrzną w celu zalogowania się. Zazwyczaj można skopiować potrzebne znaczniki i nieco zmienić tekst.

Instrukcje krok po kroku można znaleźć w temacie [Włączanie logowania z witryn zewnętrznych w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Gdy użytkownik zaloguje się z innej lokacji, użytkownik powróci do swojej witryny i *skojarzy* to logowanie z witryną. W efekcie spowoduje to utworzenie wpisu członkostwa w witrynie na potrzeby logowania zewnętrznego użytkownika. Dzięki temu można używać zwykłych obiektów członkostwa (takich jak role) z logowaniem zewnętrznym.

## <a name="adding-security-to-an-existing-website"></a>Dodawanie zabezpieczeń do istniejącej witryny sieci Web

Procedura opisana wcześniej w tym artykule polega na użyciu szablonu **witryny Starter** jako podstawy zabezpieczeń witryny sieci Web. Jeśli nie jest to zalecane w przypadku uruchamiania z poziomu szablonu **witryny** początkowej lub kopiowania odpowiednich stron z witryny na podstawie tego szablonu, można zaimplementować ten sam typ zabezpieczeń w swojej lokacji, samodzielnie tworząc kodowanie. Tworzysz te same typy stron — rejestrację, logowanie i tak dalej, a następnie korzystaj z pomocników i klas, aby skonfigurować członkostwo.

Proces podstawowy został opisany w blogu w tym artykule [, który stanowi najbardziej podstawowy sposób implementacji zabezpieczeń ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). Większość pracy odbywa się przy użyciu następujących metod i właściwości pomocnika `WebSecurity`:

- [WebSecurty. UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity. CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Te metody pozwalają określić, czy ktoś jest już zarejestrowany i zarejestrować.
- [WebSecurty. IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Ta właściwość umożliwia określenie, czy bieżący użytkownik jest zalogowany. Jest to przydatne w przypadku przekierowywania użytkowników do strony logowania, jeśli nie zostały one jeszcze zalogowane.
- [WebSecurity. Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity. Wyloguj](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Te metody rejestrują użytkownika w lub na zewnątrz.
- [WebSecurity. CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Ta właściwość jest przydatna do wyświetlania nazwy logowania bieżącego użytkownika (Jeśli użytkownik jest zalogowany).
- [WebSecurity. ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Ta metoda jest przydatna, jeśli skonfigurujesz potwierdzenie poczty e-mail na potrzeby rejestracji. (Szczegółowe informacje znajdują się w wpisie w blogu [przy użyciu funkcji potwierdzenie dla zabezpieczeń ASP.NET stron sieci Web](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267)).

Aby zarządzać rolami, można użyć [ról](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) i klas [członkostwa](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) , zgodnie z opisem w wpisie w blogu.

## <a name="additional-resources"></a>Dodatkowe materiały

- [Dostosowywanie zachowania dla całej witryny](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Zabezpieczanie komunikacji internetowej: certyfikaty, SSL i https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [Najbardziej podstawowy sposób implementacji zabezpieczeń ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) i [korzystania z funkcji potwierdzania w celu zabezpieczenia stron sieci Web ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Są to wpisy w blogu opisujące sposób implementacji funkcji członkostwa ASP.NET bez użycia szablonu **witryny Starter** .
- [Włączanie logowania z zewnętrznych witryn w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Dokumentacja interfejsu API klasy WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Dokumentacja interfejsu API klasy SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Dokumentacja interfejsu API klasy SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)

---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Wysyłanie wiadomości E-mail z witryny ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale wyjaśniono, jak wysłać zautomatyzowaną wiadomość e-mail z witryny sieci Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 23e9717329525fb5a0ed505c9dc94505d4f9dbbe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634716"
---
# <a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Wysyłanie wiadomości E-mail z witryny ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak wysłać wiadomość e-mail z witryny internetowej w przypadku używania stron sieci Web ASP.NET (Razor).
> 
> Zawartość:
> 
> - Jak wysłać wiadomość e-mail z witryny sieci Web.
> - Jak dołączyć plik do wiadomości e-mail.
> 
> Jest to funkcja ASP.NET wprowadzona w artykule:
> 
> - Pomocnik `WebMail`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Wysyłanie wiadomości E-mail z witryny sieci Web

Istnieje wiele powodów, dla których może być konieczne wysłanie wiadomości e-mail z witryny sieci Web. Użytkownik może wysyłać wiadomości potwierdzające do użytkowników lub wysyłać do siebie powiadomienia (na przykład o zarejestrowaniu nowego użytkownika). Pomocnik `WebMail` ułatwia wysyłanie wiadomości e-mail.

Aby korzystać z pomocnika `WebMail`, musisz mieć dostęp do serwera SMTP. (Protokół SMTP ma *Simple Mail Transfer Protocol*.) Serwer SMTP jest serwerem poczty e-mail, który przesyła dalej komunikaty do serwera &#8212; odbiorcy, jest to wychodząca Strona poczty e-mail. Jeśli używasz dostawcy hostingu dla witryny sieci Web, prawdopodobnie skonfigurujesz go za pomocą poczty e-mail i poinformujemy o tym, co to jest nazwa serwera SMTP. Jeśli pracujesz w sieci firmowej, administrator lub dział IT zazwyczaj mogą uzyskać informacje o serwerze SMTP, którego można użyć. Jeśli pracujesz w domu, możesz nawet sprawdzić za pomocą zwykłego dostawcy poczty e-mail, który będzie mógł poinformować o nazwie serwera SMTP. Zazwyczaj potrzebne są:

- Nazwa serwera SMTP.
- Numer portu. To prawie zawsze 25. Jednak usługodawca internetowy może wymagać użycia portu 587. Jeśli używasz protokołu SSL (Secure Sockets Layer) do obsługi poczty e-mail, może być potrzebny inny port. Skontaktuj się z dostawcą poczty e-mail.
- Poświadczenia (nazwa użytkownika, hasło).

W tej procedurze utworzysz dwie strony. Pierwsza strona zawiera formularz, który umożliwia użytkownikom wprowadzanie opisu, tak jakby był wypełniany w formularzu technicznym. Na pierwszej stronie przesyłane są informacje do drugiej strony. Na drugiej stronie kod wyodrębnia informacje o użytkowniku i wysyła wiadomość e-mail. Zostanie również wyświetlony komunikat potwierdzający, że został odebrany raport o problemach.

![Image](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Aby zachować ten przykład prosty, kod inicjuje `WebMail` pomocnika bezpośrednio na stronie, w której jest używana. Jednak w przypadku rzeczywistych witryn sieci Web lepiej jest umieścić kod inicjujący podobny do tego w pliku globalnym, aby zainicjować pomocnika `WebMail` dla wszystkich plików w witrynie sieci Web. Aby uzyskać więcej informacji, zobacz [Dostosowywanie zachowania całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).

1. Utwórz nową witrynę sieci Web.
2. Dodaj nową stronę o nazwie *EmailRequest. cshtml* i Dodaj następujące znaczniki: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Zwróć uwagę, że atrybut `action` elementu form został ustawiony na *ProcessRequest. cshtml*. Oznacza to, że formularz zostanie przesłany do tej strony zamiast z powrotem do bieżącej strony.
3. Dodaj nową stronę o nazwie *ProcessRequest. cshtml* do witryny sieci Web i Dodaj następujący kod i znacznik:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    W kodzie otrzymujesz wartości pól formularza, które zostały przesłane do strony. Następnie należy wywołać metodę `Send` pomocnika `WebMail`, aby utworzyć i wysłać wiadomość e-mail. W takim przypadku wartości, które mają być używane, składają się z tekstu połączonego z wartościami, które zostały przesłane z formularza.

    Kod tej strony znajduje się w bloku `try/catch`. Jeśli z jakiegoś powodu próba wysłania wiadomości e-mail nie działa (na przykład ustawienia nie są prawidłowe), kod w bloku `catch` jest uruchamiany i ustawia zmienną `errorMessage` na błąd, który wystąpił. (Aby uzyskać więcej informacji na temat bloków `try/catch` lub tagu `<text>`, zobacz [wprowadzenie do ASP.NET stron sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors)).

    W treści strony, jeśli zmienna `errorMessage` jest pusta (wartość domyślna), użytkownik zobaczy komunikat informujący o wysłaniu wiadomości e-mail. Jeśli zmienna `errorMessage` ma wartość true, użytkownik zobaczy komunikat, że wystąpił problem podczas wysyłania komunikatu.

    Zwróć uwagę, że w części strony, na której jest wyświetlany komunikat o błędzie, istnieje dodatkowy test: `if(debuggingFlag)`. Jest to zmienna, którą można ustawić na wartość true, jeśli masz problemy z wysyłaniem poczty e-mail. Gdy `debuggingFlag` ma wartość true, a jeśli wystąpi problem z wysłaniem wiadomości e-mail, zostanie wyświetlony dodatkowy komunikat o błędzie z informacjami o tym, co ASP.NET zgłosiła podczas próby wysłania wiadomości e-mail. Ostrzeżenie uczciwe, chociaż: komunikaty o błędach, które ASP.NETą raporty, gdy nie można wysłać wiadomości e-mail mogą być ogólne. Na przykład jeśli ASP.NET nie może skontaktować się z serwerem SMTP (na przykład z powodu błędu w nazwie serwera), błąd jest `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Ważne** Po otrzymaniu komunikatu o błędzie z obiektu wyjątku (`ex` w kodzie *) nie należy rutynowo* przekazywać tego komunikatu do użytkowników. Obiekty wyjątków często zawierają informacje, które nie powinny być widoczne dla użytkowników i które mogą nawet stanowić lukę w zabezpieczeniach. Dlatego ten kod zawiera zmienną `debuggingFlag`, która jest używana jako przełącznik do wyświetlania komunikatu o błędzie i dlaczego zmienna domyślnie ma wartość false. Należy ustawić tę zmienną na true (i w związku z tym wyświetlić komunikat o błędzie) *tylko* wtedy, gdy występuje problem z wysyłaniem poczty e-mail i należy debugować. Po usunięciu wszelkich problemów Ustaw `debuggingFlag` z powrotem na wartość false.

    Zmodyfikuj następujące ustawienia związane z pocztą e-mail w kodzie:

   - Ustaw `your-SMTP-host` na nazwę serwera SMTP, do którego masz dostęp.
   - Ustaw `your-user-name-here` na nazwę użytkownika dla konta serwera SMTP.
   - Ustaw `your-account-password` na hasło dla konta serwera SMTP.
   - Ustaw `your-email-address-here` na własny adres e-mail. Jest to adres e-mail, z którego wiadomość jest wysyłana. (Niektórzy dostawcy poczty e-mail nie umożliwiają określania innego adresu `From` i będą używać nazwy użytkownika jako adresu `From`).

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Konfigurowanie ustawień poczty E-mail
     > 
     > Może być wyzwaniem czasami, aby upewnić się, że masz odpowiednie ustawienia dla serwera SMTP, numeru portu i tak dalej. Oto kilka porad:
     > 
     > - Nazwa serwera SMTP często jest taka sama jak `smtp.provider.com` lub `smtp.provider.net`. Jednak w przypadku opublikowania lokacji w ramach dostawcy hostingu nazwa serwera SMTP może być `localhost`. Jest to spowodowane tym, że po opublikowaniu i uruchomieniu lokacji na serwerze dostawcy serwer poczty e-mail może być lokalnym z perspektywy aplikacji. Ta zmiana nazw serwerów może oznaczać konieczność zmiany nazwy serwera SMTP w ramach procesu publikowania.
     > - Numer portu to zwykle 25. Jednak niektórzy dostawcy wymagają użycia portu 587 lub innego portu.
     > - Upewnij się, że używasz odpowiednich poświadczeń. Jeśli witryna została opublikowana w ramach dostawcy hostingu, Użyj poświadczeń, które zostały wskazane przez dostawcę jako wiadomości e-mail. Mogą się one różnić od poświadczeń używanych do publikowania.
     > - Czasami nie potrzebujesz poświadczeń. W przypadku wysyłania wiadomości e-mail przy użyciu osobistego usługodawcy internetowego Twój dostawca poczty e-mail może już znać Twoje poświadczenia. Po opublikowaniu programu może być konieczne użycie innych poświadczeń niż podczas testowania na komputerze lokalnym.
     > - Jeśli Twój dostawca poczty e-mail używa szyfrowania, musisz ustawić `WebMail.EnableSsl`, aby `true`.
4. Uruchom stronę *EmailRequest. cshtml* w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem).
5. Wprowadź nazwę i opis problemu, a następnie kliknij przycisk **Prześlij** . Nastąpi przekierowanie do strony *ProcessRequest. cshtml* , która potwierdzi komunikat, a następnie wyśle wiadomość e-mail. 

    ![Image](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Wysyłanie pliku przy użyciu poczty E-mail

Możesz również wysyłać pliki dołączone do wiadomości e-mail. W tej procedurze utworzysz plik tekstowy i dwie strony HTML. Plik tekstowy będzie używany jako załącznik wiadomości e-mail.

1. W witrynie sieci Web Dodaj nowy plik tekstowy i nadaj mu nazwę plik *. txt*.
2. Skopiuj poniższy tekst i wklej go w pliku: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Utwórz stronę o nazwie *SendFile. cshtml* i Dodaj następujące znaczniki: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Utwórz stronę o nazwie *ProcessFile. cshtml* i Dodaj następujące znaczniki: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Zmodyfikuj następujące ustawienia związane z pocztą e-mail w kodzie z przykładu:

    - Ustaw `your-SMTP-host` na nazwę serwera SMTP, do którego masz dostęp.
    - Ustaw `your-user-name-here` na nazwę użytkownika dla konta serwera SMTP.
    - Ustaw `your-email-address-here` na własny adres e-mail. Jest to adres e-mail, z którego wiadomość jest wysyłana.
    - Ustaw `your-account-password` na hasło dla konta serwera SMTP.
    - Ustaw `target-email-address-here` na własny adres e-mail. (Tak jak wcześniej, zazwyczaj wysyłamy wiadomość e-mail do kogoś innego, ale w celu testowania można wysłać ją do siebie).
6. Uruchom stronę *SendFile. cshtml* w przeglądarce.
7. Wprowadź swoje imię i nazwisko, wiersz tematu oraz nazwę pliku tekstowego do dołączenia (*plik. txt*).
8. Kliknij przycisk `Submit`. Jak wcześniej, nastąpi przekierowanie do strony *ProcessFile. cshtml* , która potwierdzi komunikat, a następnie wysyła wiadomość e-mail z dołączonym plikiem.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Dostosowywanie zachowania całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)

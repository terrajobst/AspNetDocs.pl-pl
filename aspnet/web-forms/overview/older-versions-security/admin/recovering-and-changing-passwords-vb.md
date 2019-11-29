---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Odzyskiwanie i zmienianie haseł (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET obejmuje dwie kontrolki sieci Web ułatwiające odzyskiwanie i zmienianie haseł. Kontrolka PasswordRecovery umożliwia odzyskanie utraconej sieci PA przez osobę odwiedzającą...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ed1df9455af94a86ce59ecc06c55846a4880596
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618757"
---
# <a name="recovering-and-changing-passwords-vb"></a>Odzyskiwanie i zmienianie haseł (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET obejmuje dwie kontrolki sieci Web ułatwiające odzyskiwanie i zmienianie haseł. Kontrolka PasswordRecovery umożliwia odzyskanie utraconego hasła przez osobę odwiedzającą. Kontrolka ChangePassword umożliwia użytkownikowi zaktualizowanie hasła. Podobnie jak w przypadku innych kontrolek sieci Web powiązanych z logowaniem w tej serii samouczków, formanty PasswordRecovery i ChangePassword działają z platformą członkowską w tle w celu resetowania lub modyfikowania haseł użytkowników.

## <a name="introduction"></a>Wprowadzenie

Między witrynami sieci Web mojego banku, firmą narzędziowej, firmą telefonicznej, kontami e-mail i spersonalizowanymi portalami sieci Web, podobnie jak większość osób, można pamiętać o dziesiątych hasłach. Dzięki temu wiele poświadczeń do znają tych dni nie zdarza się, aby użytkownicy zapominali hasła. Aby obsłużyć to konto, witryny sieci Web, które oferują konta użytkowników, muszą mieć możliwość odzyskania hasła przez użytkownika. Ten proces zazwyczaj polega na wygenerowanym nowym, losowym haśle i wysyłaniu ich pocztą e-mail na adres e-mail użytkownika w pliku. Po otrzymaniu nowego hasła większość użytkowników Wróć do witryny i zmieni hasło z losowo wygenerowanego elementu na bardziej zapamiętanie.

ASP.NET obejmuje dwie kontrolki sieci Web ułatwiające odzyskiwanie i zmienianie haseł. Kontrolka PasswordRecovery umożliwia odzyskanie utraconego hasła przez osobę odwiedzającą. Kontrolka ChangePassword umożliwia użytkownikowi zaktualizowanie hasła. Podobnie jak w przypadku innych kontrolek sieci Web powiązanych z logowaniem w tej serii samouczków, formanty PasswordRecovery i ChangePassword działają z platformą członkowską w tle w celu resetowania lub modyfikowania haseł użytkowników.

W tym samouczku będziemy analizować przy użyciu tych dwóch kontrolek. Zobaczymy również, jak programowo zmienić i zresetować hasło użytkownika za pomocą metod `ChangePassword` i `ResetPassword` klasy `MembershipUser`.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Krok 1. ułatwienia dla użytkowników odzyskanie utraconych haseł

Wszystkie witryny sieci Web, które obsługują konta użytkowników, muszą zapewnić użytkownikom pewne mechanizmy odzyskiwania zapomnianych haseł. Dobra wiadomość polega na tym, że implementowanie takich funkcji w programie ASP.NET jest breezee przez kontrolkę sieci Web PasswordRecovery. Formant PasswordRecovery renderuje interfejs, który monituje użytkownika o nazwę użytkownika i, w razie potrzeby, odpowiedź na swoje pytanie zabezpieczające. Następnie wyśle wiadomość e-mail z hasłem użytkownika.

> [!NOTE]
> Ze względu na to, że wiadomości e-mail są przesyłane przez sieć w postaci zwykłego tekstu, występują zagrożenia bezpieczeństwa związane z wysyłaniem hasła użytkownika za pośrednictwem poczty e-mail.

Kontrolka PasswordRecovery składa się z trzech widoków:

- **Nazwa użytkownika** — poprosi odwiedzających o ich nazwę użytkownika. Jest to widok początkowy.
- **Pytanie**— wyświetla nazwę użytkownika i pytanie zabezpieczające jako tekst oraz pole tekstowe, dla którego użytkownik może wprowadzić odpowiedź na swoje pytanie zabezpieczające.
- **Sukces**— wyświetla komunikat informujący użytkownika, że jego hasło zostało wysłane pocztą e-mail.

Wyświetlane widoki i akcje wykonywane przez formant PasswordRecovery zależą od następujących ustawień konfiguracji członkostwa:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Ustawienie `RequiresQuestionAndAnswer` struktury członkostwa wskazuje, czy użytkownicy muszą określić pytanie zabezpieczające i odpowiedź podczas rejestrowania konta. Zgodnie z opisem w <a id="_msoanchor_1"> </a>samouczku [*Tworzenie kont użytkowników*](../membership/creating-user-accounts-vb.md) , jeśli `RequiresQuestionAndAnswer` ma wartość true (domyślnie), interfejs formancie CreateUserWizard zawiera kontrolki TextBox dla pytania zabezpieczeń i odpowiedzi dla nowego użytkownika; Jeśli `RequiresQuestionAndAnswer` ma wartość false, nie są zbierane takie informacje. Podobnie, jeśli `RequiresQuestionAndAnswer` ma wartość true, formant PasswordRecovery wyświetla widok pytania po wprowadzeniu przez użytkownika nazwy użytkownika; hasło jest odzyskiwane tylko wtedy, gdy użytkownik wprowadzi poprawną odpowiedź zabezpieczeń. Jeśli `RequiresQuestionAndAnswer` ma wartość false, formant PasswordRecovery przenosi się bezpośrednio z widoku UserName do widoku sukces.

Jeśli użytkownik podał swoją nazwę użytkownika lub nazwę użytkownika i odpowiedź zabezpieczeń, jeśli `RequiresQuestionAndAnswer` ma wartość true — PasswordRecovery wyśle wiadomość e-mail z hasłem. Jeśli opcja `EnablePasswordRetrieval` ma wartość PRAWDA, użytkownik otrzyma wiadomość e-mail o bieżącym haśle. Jeśli jest ustawiona na wartość false, a `EnablePasswordReset` jest ustawiona na wartość true, formant PasswordRecovery generuje nowe, losowe hasło dla użytkownika i wysyła do nich wiadomości e-mail z nowym hasłem. Jeśli oba `EnablePasswordRetrieval` i `EnablePasswordReset` mają wartość false, formant PasswordRecovery zgłasza wyjątek.

> [!NOTE]
> Należy przypomnieć, że `SqlMembershipProvider` przechowuje hasła użytkowników w jednym z trzech formatów: Clear, Hashed (wartość domyślna) lub szyfrowany. Używany mechanizm magazynu zależy od ustawień konfiguracji członkostwa; Aplikacja demonstracyjna używa formatu hasła z mieszaniem. W przypadku korzystania z formatu hasła z wartością skrótu opcja `EnablePasswordRetrieval` musi być ustawiona na wartość false, ponieważ system nie może określić rzeczywistego hasła użytkownika z wersji skrótu przechowywanej w bazie danych.

Rysunek 1 ilustruje, w jaki sposób konfiguracja członkostwa ma wpływ na interfejs i zachowanie PasswordRecovery.

[![RequiresQuestionAndAnswer, EnablePasswordRetrieval i EnablePasswordReset wpływają na wygląd i zachowanie kontrolki PasswordRecovery](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Rysunek 1**. `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`i `EnablePasswordReset` wpływają na wygląd i zachowanie kontrolki PasswordRecovery ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image3.png))

> [!NOTE]
> <a id="_msoanchor_2"> </a>W samouczku [*Tworzenie schematu członkostwa w SQL Server*](../membership/creating-the-membership-schema-in-sql-server-vb.md) został skonfigurowany dostawca członkostwa przez ustawienie `RequiresQuestionAndAnswer` na true, `EnablePasswordRetrieval` na wartość false, a `EnablePasswordReset` na wartość true.

### <a name="using-the-passwordrecovery-control"></a>Korzystanie z formantu PasswordRecovery

Przyjrzyjmy się kontrolce PasswordRecovery na stronie ASP.NET. Otwórz `RecoverPassword.aspx` i przeciągnij i upuść kontrolkę PasswordRecovery z przybornika do projektanta; Ustaw `ID`, aby `RecoverPwd`. Podobnie jak w przypadku kontrolek sieci Web Login i formancie CreateUserWizard, w widokach kontrolki PasswordRecovery są renderowane rozbudowany interfejs złożony, który obejmuje etykiety, pola tekstowe, przyciski i kontrolki walidacji. Wygląd widoków można dostosować za pomocą właściwości stylu kontrolki lub przez konwersję widoków do szablonów. Jestem to ćwiczenie dla interesującego czytnika.

Gdy użytkownik odwiedza tę stronę, będzie wprowadzić swoją nazwę użytkownika i kliknąć przycisk Prześlij. Ze względu na to, że właściwość `RequiresQuestionAndAnswer` ma wartość true w ustawieniach konfiguracji członkostwa, w kontrolce PasswordRecovery zostanie wyświetlony widok pytania. Po wprowadzeniu przez użytkownika prawidłowej odpowiedzi na zabezpieczenia i kliknięciu przycisku Prześlij formant PasswordRecovery zaktualizuje hasło użytkownika do losowo generowanego konta i wyśle wiadomość e-mail na adres e-mail w pliku. Wszystkie te możliwości były możliwe bez konieczności pisania jednego wiersza kodu.

Przed przetestowaniem tej strony istnieje jedna końcowa konfiguracja do zaplanowania: musimy określić ustawienia dostarczania poczty w `Web.config`. Kontrolka PasswordRecovery opiera się na tych ustawieniach do wysyłania wiadomości e-mail.

Konfiguracja dostarczania poczty jest określana za pomocą [elementu`<mailSettings>`](https://msdn.microsoft.com/library/w355a94k.aspx) [elementu`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx). Użyj [elementu`<smtp>`](https://msdn.microsoft.com/library/ms164240.aspx) , aby wskazać metodę dostarczania i domyślny adres od. Poniższy znacznik konfiguruje ustawienia poczty do używania serwera SMTP sieci o nazwie `smtp.example.com` na porcie 25 i z poświadczeniami nazwy użytkownika/hasła dla nazwy użytkownika i hasła.

> [!NOTE]
> `<system.net>` jest elementem podrzędnym głównego elementu `<configuration>` i elementem równorzędnym `<system.web>`. W związku z tym nie należy umieszczać elementu `<system.net>` w elemencie `<system.web>`; Zamiast tego należy umieścić go na tym samym poziomie.

[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Oprócz korzystania z serwera SMTP w sieci można także określić katalog, w którym mają być wysyłane wiadomości e-mail.

Po skonfigurowaniu ustawień SMTP odwiedź stronę `RecoverPassword.aspx` za pośrednictwem przeglądarki. Najpierw spróbuj wprowadzić nazwę użytkownika, która nie istnieje w magazynie użytkownika. Jak pokazano na rysunku 2, formant PasswordRecovery wyświetla komunikat informujący o tym, że nie można uzyskać dostępu do informacji o użytkowniku. Tekst komunikatu można dostosować za pomocą [właściwości`UserNameFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)kontrolki.

[![zostanie wyświetlony komunikat o błędzie w przypadku wprowadzenia nieprawidłowej nazwy użytkownika](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Rysunek 2**. komunikat o błędzie jest wyświetlany po wprowadzeniu nieprawidłowej nazwy użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image6.png))

Teraz wprowadź nazwę użytkownika. Użyj nazwy użytkownika konta w systemie przy użyciu adresu e-mail, do którego masz dostęp, i której odpowiedzi zabezpieczeń znasz. Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku Prześlij formant PasswordRecovery wyświetla swój widok pytania. Podobnie jak w przypadku wyświetlania nazwy użytkownika, w przypadku wprowadzenia niepoprawnej odpowiedzi formant PasswordRecovery wyświetli komunikat o błędzie (patrz rysunek 3). Użyj [właściwości`QuestionFailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) , aby dostosować ten komunikat o błędzie.

[![komunikat o błędzie jest wyświetlany, gdy użytkownik wprowadzi nieprawidłową odpowiedź zabezpieczeń](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Rysunek 3**. komunikat o błędzie jest wyświetlany, gdy użytkownik wprowadzi nieprawidłową odpowiedź zabezpieczeń ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image9.png))

Na koniec Wprowadź poprawną odpowiedź zabezpieczeń, a następnie kliknij przycisk Prześlij. W tle formant PasswordRecovery generuje losowe hasło, przypisuje go do konta użytkownika, wysyła wiadomość e-mail z informacją o nowym haśle (zobacz rysunek 4), a następnie wyświetla widok powodzenia.

[![użytkownik wysyła wiadomość E-mail z nowym hasłem](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Rysunek 4**. użytkownik otrzymuje wiadomość E-mail z nowym hasłem ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image12.png))

### <a name="customizing-the-email"></a>Dostosowywanie wiadomości E-mail

Domyślna wiadomość e-mail wysłana przez formant PasswordRecovery jest raczej matowa (zobacz rysunek 4). Wiadomość jest wysyłana z konta określonego w atrybucie `from` elementu `<smtp>` z hasłem podmiotu i treścią zwykłego tekstu:

Wróć do witryny i zaloguj się przy użyciu poniższych informacji.

Nazwa użytkownika: *nazwa_użytkownika*

Hasło: *hasło*

Ten komunikat można programowo dostosować za pomocą procedury obsługi zdarzeń dla [zdarzenia`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)formantu PasswordRecovery lub deklaratywnie za pomocą [Właściwości`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Skorzystajmy z obu tych opcji.

Zdarzenie `SendingMail` jest wyzwalane bezpośrednio przed wysłaniem wiadomości e-mail, a Ostatnia szansa na programowe dostosowanie wiadomości e-mail. Gdy to zdarzenie jest zgłaszane, program obsługi zdarzeń przekazuje obiekt typu [`MailMessageEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), którego właściwość `Message` zawiera odwołanie do wysyłanej wiadomości e-mail.

Utwórz procedurę obsługi zdarzeń dla zdarzenia `SendingMail` i Dodaj następujący kod, który programowo dodaje `webmaster@example.com` do listy DW.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

Wiadomość e-mail można również skonfigurować za pomocą środków deklaratywnych. Właściwość `MailDefinition` PasswordRecovery jest obiektem typu [`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). Klasa `MailDefinition` oferuje Host właściwości związanych z pocztą e-mail, w tym `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`i innych. Dla elementów uruchomieniowych Ustaw [właściwość`Subject`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) na bardziej opisową wartość inną niż używana domyślnie (hasło), np. hasło zostało zresetowane...

Aby dostosować treść wiadomości e-mail, należy utworzyć osobny plik szablonu wiadomości e-mail zawierający zawartość treści. Zacznij od utworzenia nowego folderu w witrynie sieci Web o nazwie `EmailTemplates`. Następnie Dodaj nowy plik tekstowy do tego folderu o nazwie `PasswordRecovery.txt` i Dodaj następującą zawartość:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Zwróć uwagę na użycie symboli zastępczych `<%UserName%>` i `<%Password%>`. Formant PasswordRecovery automatycznie zastępuje te dwa symbole zastępcze nazwą użytkownika i odzyskanym hasłem przed wysłaniem wiadomości e-mail.

Na koniec wskaż [właściwość`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) `MailDefinition`do utworzonego szablonu wiadomości e-mail (`~/EmailTemplates/PasswordRecovery.txt`).

Po wprowadzeniu tych zmian ponownie odwiedź stronę `RecoverPassword.aspx` i wprowadź nazwę użytkownika i odpowiedź zabezpieczeń. Powinna zostać wyświetlona wiadomość e-mail, która wygląda podobnie do przedstawionej na rysunku 5. Zwróć uwagę, że `webmaster@example.com` to DW i czy temat i treść zostały zaktualizowane.

[![Zaktualizowano temat, treść i lista DW](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Rysunek 5**: Zaktualizowano temat, treść i listę DW ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image15.png))

Aby wysłać wiadomość e-mail sformatowaną w formacie HTML [`IsBodyHtml`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) do wartości true (wartość domyślna to false) i zaktualizować szablon wiadomości e-mail w celu uwzględnienia kodu HTML.

Właściwość `MailDefinition` nie jest unikatowa dla klasy PasswordRecovery. Ponieważ zobaczymy w kroku 2, formant ChangePassword również oferuje Właściwość `MailDefinition`. Ponadto formant formancie CreateUserWizard zawiera taką właściwość, którą można skonfigurować, aby automatycznie wysyłać powitalną wiadomość e-mail do nowych użytkowników.

> [!NOTE]
> Obecnie nie ma żadnych linków w nawigacji po lewej stronie w celu uzyskania dostępu do strony `RecoverPassword.aspx`. Użytkownik chce tylko odwiedzać tę stronę, jeśli nie może pomyślnie zalogować się do witryny. W związku z tym zaktualizuj stronę `Login.aspx`, aby dołączyć łącze do strony `RecoverPassword.aspx`.

### <a name="programmatically-resetting-a-users-password"></a>Programowo Resetowanie hasła użytkownika

Podczas resetowania hasła użytkownika, formant PasswordRecovery wywołuje [metodę`ResetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)obiektu `MembershipUser`. Ta metoda ma dwa przeciążenia:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** — resetuje hasło użytkownika. Użyj tego przeciążenia, jeśli `RequiresQuestionAndAnswer` ma wartość false.
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** — resetuje hasło użytkownika tylko wtedy, gdy podana *securityAnswer* jest poprawna. Użyj tego przeciążenia, jeśli `RequiresQuestionAndAnswer` ma wartość true.

Oba przeciążenia zwracają nowe generowane losowo hasło.

Podobnie jak w przypadku innych metod w środowisku członkostwa, Metoda `ResetPassword` deleguje do skonfigurowanego dostawcy. `SqlMembershipProvider` wywołuje `aspnet_Membership_ResetPassword` procedurę składowaną, przekazując dane o nazwie użytkownika, nowym haśle i dostarczonej odpowiedzi hasła, między innymi polami. Procedura składowana zapewnia, że odpowiedź hasła jest zgodna, a następnie aktualizuje hasło użytkownika.

Kilka informacji o implementacji niskiego poziomu:

- Zablokowany użytkownik nie może zresetować swojego hasła. Jednak niezatwierdzony użytkownik może. W samouczku <a id="_msoanchor_3"> </a> [*odblokowywanie i zatwierdzanie kont użytkowników*](unlocking-and-approving-user-accounts-vb.md) zawarto szczegółowe informacje o zablokowanych i zatwierdzonych Stanach.
- Jeśli odpowiedź hasła jest niepoprawna, liczba prób odpowiedzenia hasła użytkownika nie powiodła się. Jeśli w określonym przedziale czasu podano określoną liczbę nieprawidłowych odpowiedzi zabezpieczeń, użytkownik zostanie zablokowany.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Słowo określające sposób generowania losowych haseł

Losowo generowane hasła wyświetlane w wiadomościach e-mail w ilustracjach 4 i 5 są tworzone przez [metodę`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)klasy członkostwa. Ta metoda akceptuje dwie liczby całkowite parametrów i *numberOfNonAlphanumericCharacters* -i zwraca ciąg o *długości* co najmniej *znaków, który* ma co najmniej *numberOfNonAlphanumericCharacters* znaki inne niż alfanumeryczne. Gdy ta metoda jest wywoływana z poziomu klas członkostwa lub kontrolek sieci Web związanych z logowaniem, wartości tych dwóch parametrów są określane przez `MinRequiredPasswordLength` konfiguracji członkostwa i właściwości `MinRequiredNonalphanumericCharacters`, dla których ustawiono odpowiednio 7 i 1.

Metoda `GeneratePassword` używa kryptografii o silnej liczbie losowej, aby upewnić się, że nie ma żadnych informacji o tym, jakie znaki losowe są zaznaczone. Ponadto `GeneratePassword` jest `Public`, co oznacza, że można go używać bezpośrednio z aplikacji ASP.NET, jeśli konieczne jest wygenerowanie losowych ciągów lub haseł.

> [!NOTE]
> Klasa `SqlMembershipProvider` zawsze generuje losowe hasło o długości co najmniej 14 znaków, więc jeśli `MinRequiredPasswordLength` jest mniejsza niż 14, jego wartość jest ignorowana.

## <a name="step-2-changing-passwords"></a>Krok 2. zmiana hasła

Hasła generowane losowo są trudne do zapamiętania. Rozważ hasło pokazane na rysunku 4: `WWGUZv(f2yM:Bd`. Spróbuj wykonać zatwierdzenie w pamięci. Niezbędny do wypowiedzenia, gdy użytkownik zostanie wysłany losowo wygenerowane hasło tego sortowania, chce zmienić hasło na coś bardziej zapamiętania.

Użyj kontrolki ChangePassword, aby utworzyć interfejs użytkownika w celu zmiany jego hasła. Podobnie jak w przypadku kontrolki PasswordRecovery, kontrolka ChangePassword składa się z dwóch widoków: Zmień hasło i powodzenie. W widoku Zmień hasło użytkownik jest monitowany o ich stare i nowe hasło. W przypadku podania poprawnego starego hasła i nowego hasła spełniającego wymagania minimalnej długości i znaków niealfanumerycznych kontrolka ChangePassword aktualizuje hasło użytkownika i wyświetla widok powodzenia.

> [!NOTE]
> Kontrolka ChangePassword modyfikuje hasło użytkownika, wywołując [metodę`ChangePassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)obiektu `MembershipUser`. Metoda ChangePassword akceptuje dwa `String` parametry wejściowe- *oldPassword* i *NoweHasło*-i aktualizuje konto użytkownika za pomocą elementu *NoweHasło*, przy założeniu, że podana *oldPassword* jest poprawna.

Otwórz stronę `ChangePassword.aspx` i Dodaj kontrolkę ChangePassword do strony, nadając jej nazwę `ChangePwd`. W tym momencie widok Projekt powinien wyświetlić widok zmiana hasła (patrz rysunek 6). Podobnie jak w przypadku kontrolki PasswordRecovery, można przełączać się między widokami za pośrednictwem tagu inteligentnego kontrolki. Co więcej, wygląd tych widoków można dostosowywać za pomocą właściwości w ramach konkretnego stylu lub przez konwersję do szablonu.

[![dodać kontrolki ChangePassword do strony](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Ilustracja 6**. Dodawanie kontrolki ChangePassword do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image18.png))

Kontrolka ChangePassword może zaktualizować hasło aktualnie zalogowanego użytkownika *lub* hasło innego, określonego użytkownika. Jak pokazano na rysunku 6, domyślny widok zmiany hasła renderuje tylko trzy kontrolki TextBox: jeden dla starego hasła i dwa dla nowego hasła. Ten interfejs domyślny służy do aktualizowania hasła aktualnie zalogowanego użytkownika.

Aby użyć kontrolki ChangePassword do zaktualizowania hasła innego użytkownika, ustaw [właściwość`DisplayUserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) kontrolki na wartość true. Spowoduje to dodanie czwartego pola tekstowego do strony, monitując o nazwę użytkownika, którego hasło ma zostać zmienione.

Ustawienie wartości true dla `DisplayUserName` jest przydatne, jeśli użytkownik chce zezwolić na zalogowanie się użytkownika przed zalogowaniem się. Tak, myślę, że nie ma nic niewłaściwego, aby użytkownik mógł się zalogować przed zmianą hasła. W związku z tym pozostaw `DisplayUserName` ustawić na wartość false (domyślnie). W przypadku podejmowania tej decyzji zasadniczo jest to, że użytkownicy anonimowi nie mogą uzyskać dostępu do tej strony. Zaktualizuj reguły autoryzacji adresów URL w witrynie, aby uniemożliwić anonimowym użytkownikom odwiedzenie `ChangePassword.aspx`. Jeśli zachodzi potrzeba odświeżenia pamięci w składni reguły autoryzacji adresów URL, zapoznaj się <a id="_msoanchor_4"> </a>z artykułem samouczek dotyczący [*autoryzacji na podstawie użytkownika*](../membership/user-based-authorization-vb.md) .

> [!NOTE]
> Może się wydawać, że właściwość `DisplayUserName` jest przydatna, aby umożliwić administratorom zmianę haseł innych użytkowników. Jednak nawet jeśli `DisplayUserName` ma wartość true, poprawne stare hasło musi być znane i wprowadzone. Będziemy mówić o technikach umożliwiających administratorom zmianę haseł użytkowników w kroku 3.

Odwiedź stronę `ChangePassword.aspx` za pomocą przeglądarki i Zmień hasło. Należy zauważyć, że komunikat o błędzie jest wyświetlany w przypadku wprowadzenia nowego hasła, które nie spełnia wymagań dotyczących długości hasła i znaków innych niż alfanumeryczne określonych w konfiguracji członkostwa (patrz rysunek 7).

[![dodać kontrolki ChangePassword do strony](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Rysunek 7**. Dodawanie kontrolki ChangePassword do strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image21.png))

Po wprowadzeniu poprawnego starego hasła i prawidłowego nowego hasła hasło zalogowanego użytkownika zostanie zmienione i zostanie wyświetlony widok powodzenie.

### <a name="sending-a-confirmation-email"></a>Wysyłanie wiadomości E-mail z potwierdzeniem

Domyślnie kontrolka ChangePassword nie wysyła wiadomości e-mail do użytkownika, którego hasło zostało właśnie zaktualizowane. Jeśli chcesz wysłać wiadomość e-mail, po prostu Skonfiguruj Właściwość `MailDefinition` kontrolki. Skonfigurujmy kontrolkę ChangePassword w taki sposób, aby użytkownik mógł wysłać wiadomość e-mail sformatowaną w formacie HTML, która zawiera nowe hasło.

Zacznij od utworzenia nowego pliku w folderze `EmailTemplates` o nazwie `ChangePassword.htm`. Dodaj następujący znacznik:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Następnie ustaw właściwości `MailDefinition` `BodyFileName`, `IsBodyHtml`i `Subject` kontrolki ChangePassword na wartość ~/EmailTemplates/ChangePassword.htm, true, a hasło zostało zmienione odpowiednio.

Po wprowadzeniu tych zmian ponownie odwiedź stronę i Zmień hasło. Tym razem kontrolka ChangePassword wysyła niestandardową wiadomość e-mail w formacie HTML do adresu e-mail użytkownika w pliku (zobacz rysunek 8).

[![wiadomość E-mail informuje użytkownika, że jego hasło zostało zmienione](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Ilustracja 8**. wiadomość E-mail informuje użytkownika, że jego hasło zostało zmienione ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image24.png))

## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Krok 3. umożliwienie administratorom zmiany haseł użytkowników

Typową funkcją w aplikacjach, która obsługuje konta użytkowników, jest możliwość zmiany haseł przez użytkownika administracyjnego. Czasami ta funkcja jest wymagana, ponieważ system nie ma możliwości zmiany haseł przez użytkowników. W takim przypadku jedynym sposobem na odzyskanie zapomnianego hasła przez administratora będzie przypisanie mu nowego hasła. W przypadku kontrolek PasswordRecovery i ChangePassword Użytkownicy administracyjni nie muszą jednak być obciążona zmianami haseł użytkowników, ponieważ mogą oni wykonać te czynności.

Ale co zrobić, jeśli klient zauważa, że użytkownicy administracyjni powinni mieć możliwość zmiany haseł innych użytkowników? Niestety, dodanie tej funkcji może być trochę pracy. Aby zmienić hasło użytkownika, należy dostarczyć zarówno stare, jak i nowe hasło do metody `ChangePassword` `MembershipUser` obiektu, ale administrator nie powinien znać hasła użytkownika, aby go zmodyfikować.

Obejście polega na pierwszym zresetowaniu hasła użytkownika, a następnie zmodyfikowaniu go do nowego hasła przy użyciu kodu w następujący sposób:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Ten kod zaczyna się od pobrania informacji o *nazwie*użytkownika, która jest użytkownikiem, którego hasło administrator chce zmienić. Następnie zostanie wywołana metoda `ResetPassword`, która przypisuje użytkownikowi nowe losowe hasło. To generowane losowo hasło jest zwracane przez metodę i przechowywane w zmiennej `resetPwd`. Teraz, znając hasło użytkownika, możemy zmienić je za pośrednictwem wywołania `ChangePassword`.

Problem polega na tym, że ten kod działa tylko wtedy, gdy skonfigurowano konfigurację systemu członkostwa, która `RequiresQuestionAndAnswer` ma wartość false. Jeśli `RequiresQuestionAndAnswer` ma wartość true (prawda), ponieważ jest to aplikacja, Metoda `ResetPassword` musi zostać przeniesiona na odpowiedź zabezpieczeń, w przeciwnym razie zgłosi wyjątek.

Jeśli platforma członkowska jest skonfigurowana tak, aby wymagała pytania zabezpieczającego i odpowiedzi, a mimo że klient zauważa, że administratorzy mogą zmieniać hasła użytkowników, dostępne są trzy opcje:

- Zgłoś swoje ręce w powietrzu i poinformuj klienta, że jest to tylko jeden element, który nie może zostać wykonany.
- Ustaw `RequiresQuestionAndAnswer` na wartość false. Powoduje to mniej bezpieczną aplikację. Załóżmy, że użytkownik szkodliwa uzyskał dostęp do skrzynki odbiorczej poczty e-mail innego użytkownika. Prawdopodobnie naruszony użytkownik opuścił swoją pracę, aby przejść do obiadu i nie zablokowała stacji roboczej ani nie mógł uzyskać dostępu do poczty e-mail z terminalu publicznego i nie wylogować się. W obu przypadkach użytkownik szkodliwa może odwiedzić stronę `RecoverPassword.aspx` i wprowadzić nazwę użytkownika. Następnie system wyśle wiadomość e-mail do odzyskania hasła bez monitowania o odpowiedź zabezpieczeń.
- Pomiń warstwę abstrakcji utworzoną przez strukturę członkostwa i pracuj bezpośrednio z bazą danych SQL Server. Schemat członkostwa zawiera procedurę przechowywaną o nazwie `aspnet_Membership_SetPassword`, która ustawia hasło użytkownika i nie wymaga odpowiedzi zabezpieczeń ani starego hasła w celu wykonania zadania.

Żadna z tych opcji nie jest szczególnie atrakcyjna, ale jest to tak samo, jak okres istnienia dewelopera.

Na przyszłość i zaimplementowano trzecie podejście, pisząc kod, który pomija klasy `Membership` i `MembershipUser` i działa bezpośrednio w odniesieniu do bazy danych `SecurityTutorials`.

> [!NOTE]
> Dzięki bezpośredniej pracy z bazą danych hermetyzacja dostarczana przez strukturę członkostwa jest rozmieszczona. Ta decyzja wiąże nas z `SqlMembershipProvider`, co sprawia, że nasz kod nie jest przenośny. Ponadto ten kod może nie funkcjonować zgodnie z oczekiwaniami w przyszłych wersjach programu ASP.NET, jeśli schemat członkostwa ulegnie zmianie. Takie podejście to obejście problemu i, takie jak większość obejść, nie stanowi przykładu najlepszych rozwiązań.

Kod zawiera kilka nieatrakcyjnych bitów i jest dość długi. W związku z tym nie chcę niczego zapamiętać w tym samouczku z dokładniejszym badaniem. Jeśli chcesz dowiedzieć się więcej, Pobierz kod dla tego samouczka i odwiedź stronę `~/Administration/ManageUsers.aspx`. Ta strona, która została utworzona w <a id="_msoanchor_5"> </a> [poprzednim samouczku](building-an-interface-to-select-one-user-account-from-many-vb.md), zawiera listę poszczególnych użytkowników. Zaktualizowaliśmy widok GridView w celu uwzględnienia linku do strony `UserInformation.aspx`, przekazując do niej nazwę użytkownika przy użyciu QueryString. Na stronie `UserInformation.aspx` są wyświetlane informacje o wybranym użytkowniku i pola tekstowe dotyczące zmiany hasła (zobacz rysunek 9).

Po wprowadzeniu nowego hasła potwierdź je w drugim polu tekstowym, a następnie kliknij przycisk Aktualizuj użytkownika, wystąpiło odświeżenie i `aspnet_Membership_SetPassword` procedury składowanej, aktualizując hasło użytkownika. Zachęcam tych czytelników do uzyskania bardziej znajomości kodu i ponowienia działania w celu uwzględnienia wysyłania wiadomości e-mail do użytkownika, którego hasło zostało zmienione.

[![administrator może zmienić hasło użytkownika](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Rysunek 9**. Administrator może zmienić hasło użytkownika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](recovering-and-changing-passwords-vb/_static/image27.png))

> [!NOTE]
> Strona `UserInformation.aspx` aktualnie działa tylko wtedy, gdy struktura członkostwa jest skonfigurowana do przechowywania haseł w formacie czystym lub mającym wartość skrótu. Brak kodu do zaszyfrowania nowego hasła, chociaż Zapraszamy do dodania tej funkcji. Zalecanym sposobem dodania niezbędnego kodu jest użycie dekompilator — informacjeego, takiego jak [reflektora](http://www.aisto.com/roeder/dotnet/) , aby poznać kod źródłowy metod w .NET Framework; Zacznij od zbadania metody `ChangePassword` klasy `SqlMembershipProvider`. Jest to technika użyta do napisania kodu służącego do tworzenia skrótu hasła.

## <a name="summary"></a>Podsumowanie

ASP.NET oferuje dwie kontrolki ułatwiające użytkownikom zarządzanie swoimi hasłami. Kontrolka PasswordRecovery jest przydatna dla tych, którzy zapominają hasła. W zależności od konfiguracji struktury członkostwa użytkownik ma wiadomość e-mail z istniejącym hasłem lub nowym, losowo wygenerowanym hasłem. Kontrolka ChangePassword umożliwia użytkownikowi zaktualizowanie hasła.

Podobnie jak w przypadku kontrolek login i formancie CreateUserWizard, formanty PasswordRecovery i ChangePassword renderują rozbudowany interfejs użytkownika bez konieczności pisania Lick deklaratywnego znacznika lub wiersza kodu. Jeśli domyślny interfejs użytkownika nie spełnia Twoich potrzeb, można dostosować go za pomocą różnych właściwości stylu. Alternatywnie interfejsy formantów mogą być konwertowane do szablonów, aby jeszcze bardziej precyzyjnie kontrolować. W tle te kontrolki używają interfejsu API członkostwa, wywołując metody `ResetPassword` i `ChangePassword` obiektu `MembershipUser`.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Kontrolki ChangePassword — Szybki Start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Przewodniki Szybki Start dotyczące kontrolek PasswordRecovery](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Wysyłanie wiadomości E-mail w ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` często zadawane pytania](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci dla liderów w tym samouczku obejmują Michael Emmings i suchi Banerjee. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [dalej](unlocking-and-approving-user-accounts-vb.md)

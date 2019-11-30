---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Odblokowywanie i zatwierdzanie kont użytkowników (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak utworzyć stronę sieci Web dla administratorów w celu zarządzania Stanami zablokowanymi i zatwierdzonymi przez użytkowników. Zobaczymy również, jak zatwierdzić nowych użytkowników o...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: c19f7dfac0ddd12c2b4f3388a71a8ca0f71cbb18
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74580213"
---
# <a name="unlocking-and-approving-user-accounts-c"></a>Odblokowywanie i zatwierdzanie kont użytkowników (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> W tym samouczku pokazano, jak utworzyć stronę sieci Web dla administratorów w celu zarządzania Stanami zablokowanymi i zatwierdzonymi przez użytkowników. Zobaczymy również, jak zatwierdzić nowych użytkowników po zweryfikowaniu ich adresów e-mail.

## <a name="introduction"></a>Wprowadzenie

Wraz z nazwą użytkownika, hasłem i wiadomością e-mail każde konto użytkownika ma dwa pola stanu, które określają, czy użytkownik może zalogować się do witryny: zablokowany i zatwierdzony. Użytkownik zostanie automatycznie zablokowany, jeśli poda nieprawidłowe poświadczenia określoną liczbę razy w ciągu określonej liczby minut (ustawienia domyślne blokują użytkownika po 5 nieprawidłowych próbach logowania w ciągu 10 minut). Stan zatwierdzony jest użyteczny w scenariuszach, w których niektóre akcje muszą wykonywanych, zanim nowy użytkownik będzie mógł zalogować się do witryny. Na przykład użytkownik może chcieć najpierw zweryfikować swój adres e-mail lub zostać zatwierdzony przez administratora przed zalogowaniem się.

Ponieważ zablokowany lub niezatwierdzony użytkownik nie może się zalogować, ma tylko naturalny wpływ na to, w jaki sposób można zresetować te Stany. Usługa ASP.NET nie zawiera żadnych wbudowanych funkcji ani kontrolek sieci Web do zarządzania stanem zablokowanym i zatwierdzonym przez użytkowników, w części ponieważ te decyzje muszą być obsługiwane w oparciu o lokację. Niektóre lokacje mogą automatycznie zatwierdzać wszystkie nowe konta użytkowników (zachowanie domyślne). Inne osoby mają uprawnienia administratora zatwierdzania nowych kont lub nie zatwierdzają użytkowników, dopóki nie odwiedzą linku wysyłanego na adres e-mail podany podczas rejestracji. Ponadto niektóre lokacje mogą blokować użytkowników, dopóki administrator resetuje swój stan, podczas gdy inne Lokacje wysyłają wiadomość e-mail do zablokowanego użytkownika przy użyciu adresu URL, który można odwiedzać w celu odblokowania konta.

W tym samouczku pokazano, jak utworzyć stronę sieci Web dla administratorów w celu zarządzania Stanami zablokowanymi i zatwierdzonymi przez użytkowników. Zobaczymy również, jak zatwierdzić nowych użytkowników po zweryfikowaniu ich adresów e-mail.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Krok 1. Zarządzanie Stanami zablokowanymi i zatwierdzonymi przez użytkowników

<a id="Tutorial12"> </a>W obszarze [*Budowanie interfejsu w celu wybrania jednego konta użytkownika z wielu*](building-an-interface-to-select-one-user-account-from-many-cs.md) samouczków została zbudowana Strona, która wystawiła listę wszystkich kont użytkowników w stronicowanym, filtrowanym widoku GridView. Siatka zawiera informacje o nazwie i adresie e-mail każdego z nich, zatwierdzonych i zablokowanych Stanach, bez względu na to, czy są one obecnie w trybie online, oraz komentarze dotyczące użytkownika. Aby zarządzać Stanami zatwierdzonymi i zablokowanymi przez użytkowników, można umożliwić edytowanie tej siatki. Aby zmienić stan zatwierdzenia użytkownika, administrator najpierw zlokalizuje konto użytkownika, a następnie edytuje odpowiedni wiersz GridView, zaznaczając lub czyszcząc zaznaczenie pola wyboru zatwierdzone. Alternatywnie możemy zarządzać Stanami zatwierdzonymi i zablokowanymi za pomocą oddzielnej strony ASP.NET.

Na potrzeby tego samouczka użyjemy dwóch ASP.NETych stron: `ManageUsers.aspx` i `UserInformation.aspx`. Na tym etapie `ManageUsers.aspx` są wyświetlane konta użytkowników w systemie, podczas gdy `UserInformation.aspx` umożliwia administratorowi zarządzanie Stanami zatwierdzonych i zablokowanych dla określonego użytkownika. Pierwszą kolejnością biznesową jest rozszerzenie GridView w `ManageUsers.aspx`, aby uwzględnić HyperLinkField, który jest renderowany jako kolumna łączy. Chcemy, aby każdy link wskazywał `UserInformation.aspx?user=UserName`, gdzie *username* jest nazwą użytkownika do edycji.

> [!NOTE]
> Jeśli pobrano kod dla <a id="Tutorial13"> </a>samouczka [*odzyskiwanie i zmienianie haseł*](recovering-and-changing-passwords-cs.md) , można zauważyć, że strona `ManageUsers.aspx` zawiera już zestaw linków "Zarządzaj", a strona `UserInformation.aspx` udostępnia interfejs do zmiany hasła wybranego użytkownika. Nie postanowiły one replikować tej funkcji w kodzie skojarzonym z tym samouczkiem, ponieważ działały one przez obejście interfejsu API członkostwa i działa bezpośrednio z bazą danych SQL Server, aby zmienić hasło użytkownika. Ten samouczek rozpoczyna się od zera ze stroną `UserInformation.aspx`.

### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Dodawanie linków "Zarządzaj" do`UserAccounts`GridView

Otwórz stronę `ManageUsers.aspx` i Dodaj HyperLinkField do `UserAccounts` GridView. Ustaw właściwość `Text` HyperLinkField na wartość "Zarządzaj" i jej właściwości `DataNavigateUrlFields` i `DataNavigateUrlFormatString` na `UserName` i "UserInformation. aspx? User ={0}". Te ustawienia służą do konfigurowania HyperLinkField w taki sposób, aby wszystkie hiperlinki wyświetlały tekst "Zarządzaj", ale każdy link przekazuje odpowiednią wartość *nazwy użytkownika* do kwerendy QueryString.

Po dodaniu HyperLinkField do widoku GridView Poświęć chwilę, aby wyświetlić stronę `ManageUsers.aspx` za pomocą przeglądarki. Jak pokazano na rysunku 1, każdy wiersz GridView zawiera teraz link "Zarządzaj". Łącze "Zarządzaj" Bruce punkty do `UserInformation.aspx?user=Bruce`, w którym łącze "Zarządzaj" dla punktów Dave ma `UserInformation.aspx?user=Dave`.

[![HyperLinkField dodaje](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Rysunek 1**. HyperLinkField dodaje link "Zarządzaj" dla każdego konta użytkownika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image3.png))

Teraz utworzysz interfejs użytkownika i kod dla strony `UserInformation.aspx`, ale najpierw będziemy mówić o tym, jak programowo zmienić stan zablokowanych i zatwierdzonych przez użytkownika. [Klasa`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) ma właściwości [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) i [`IsApproved`](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). Właściwość `IsLockedOut` jest tylko do odczytu. Nie ma mechanizmu programistycznego zablokowania użytkownika; Aby odblokować użytkownika, użyj [metody`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)klasy `MembershipUser`. Właściwość `IsApproved` jest czytelna i zapisywalna. Aby zapisać wszelkie zmiany tej właściwości, musimy wywołać [metodę`UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)klasy `Membership`, przekazując w zmodyfikowanym obiekcie `MembershipUser`.

Ponieważ właściwość `IsApproved` jest czytelna i zapisywalna, formant CheckBox jest prawdopodobnie najlepszym elementem interfejsu użytkownika do konfigurowania tej właściwości. Pole wyboru nie będzie jednak działało dla właściwości `IsLockedOut`, ponieważ administrator nie może zablokować użytkownika. może tylko odblokować użytkownika. Odpowiedni interfejs użytkownika dla właściwości `IsLockedOut` to przycisk, który po kliknięciu powoduje odblokowanie konta użytkownika. Ten przycisk należy włączyć tylko wtedy, gdy użytkownik jest zablokowany.

### <a name="creating-theuserinformationaspxpage"></a>Tworzenie strony`UserInformation.aspx`

Teraz można przystąpić do wdrożenia interfejsu użytkownika w `UserInformation.aspx`. Otwórz Tę stronę i Dodaj następujące kontrolki sieci Web:

- Kontrolka HyperLink, która po kliknięciu zwraca wartość administrator na stronie `ManageUsers.aspx`.
- Etykieta kontrolka sieci Web do wyświetlania nazwy wybranego użytkownika. Ustaw `ID` tej etykiety, aby `UserNameLabel` i wyczyścić jej Właściwość `Text`.
- Kontrolka CheckBox o nazwie `IsApproved`. Ustaw jej Właściwość `AutoPostBack` na `true`.
- Kontrolka etykieta do wyświetlania daty ostatniego zablokowania użytkownika. Nadaj jej nazwę `LastLockedOutDateLabel` i wyczyść jej Właściwość `Text`.
- Przycisk odblokowywania użytkownika. Nadaj jej nazwę `UnlockUserButton` i ustaw jej Właściwość `Text` na wartość "Odblokuj użytkownika".
- Kontrolka etykieta do wyświetlania komunikatów o stanie, takich jak "stan zatwierdzenia użytkownika, została zaktualizowana". Nadaj tej kontrolce nazwę `StatusMessage`, wyczyść jej Właściwość `Text` i ustaw jej Właściwość `CssClass` na `Important`. (Klasa `Important` CSS jest zdefiniowana w pliku arkusza stylów `Styles.css`; wyświetla odpowiedni tekst w postaci dużej, czerwonej czcionki).

Po dodaniu tych formantów widok Projekt w programie Visual Studio powinien wyglądać podobnie do zrzutu ekranu na rysunku 2.

[![utworzyć interfejs użytkownika dla UserInformation. aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Rysunek 2**. Tworzenie interfejsu użytkownika dla `UserInformation.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image6.png))

Po zakończeniu pracy z interfejsem użytkownika, następnym zadaniem jest ustawienie pola wyboru `IsApproved` i innych kontrolek na podstawie informacji o wybranym użytkowniku. Utwórz procedurę obsługi zdarzeń dla zdarzenia `Load` strony i Dodaj następujący kod:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Powyższy kod rozpocznie się, upewniając się, że jest to pierwsza wizyta na stronie, a nie kolejne ogłoszenie zwrotne. Następnie odczytuje nazwę użytkownika przekazaną za pośrednictwem pola `user` QueryString i pobiera informacje o tym koncie użytkownika za pośrednictwem metody `Membership.GetUser(username)`. Jeśli nie podano nazwy użytkownika za pomocą ciągu QueryString lub jeśli określony użytkownik nie został znaleziony, administrator zostanie wysłany z powrotem do strony `ManageUsers.aspx`.

Wartość `UserName` `MembershipUser` obiektu jest następnie wyświetlana w `UserNameLabel`, a pole wyboru `IsApproved` jest zaznaczone na podstawie wartości właściwości `IsApproved`.

[Właściwość`LastLockoutDate`](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) obiektu `MembershipUser` zwraca wartość `DateTime` wskazującą, kiedy użytkownik był ostatnio zablokowany. Jeśli użytkownik nigdy nie został zablokowany, zwracana wartość zależy od dostawcy członkostwa. Po utworzeniu nowego konta `SqlMembershipProvider` ustawia pole `LastLockoutDate` tabeli `aspnet_Membership` na `1754-01-01 12:00:00 AM`. Powyższy kod wyświetla pusty ciąg w `LastLockoutDateLabel`, jeśli właściwość `LastLockoutDate` występuje przed rokiem 2000; w przeciwnym razie część daty właściwości `LastLockoutDate` zostanie wyświetlona w etykiecie. Właściwość `Enabled` `UnlockUserButton'` s jest ustawiona na stan zablokowany użytkownika, co oznacza, że ten przycisk będzie włączany tylko wtedy, gdy użytkownik jest zablokowany.

Poświęć chwilę na przetestowanie strony `UserInformation.aspx` za pomocą przeglądarki. Oczywiście należy zacząć od `ManageUsers.aspx` i wybrać konto użytkownika, które ma być zarządzane. Po przybyciu do `UserInformation.aspx`należy zauważyć, że pole wyboru `IsApproved` jest zaznaczone tylko wtedy, gdy użytkownik jest zatwierdzony. Jeśli użytkownik został kiedykolwiek zablokowany, zostanie wyświetlona Ostatnia zablokowana Data. Przycisk Odblokuj użytkownika jest aktywny tylko wtedy, gdy użytkownik jest aktualnie zablokowany. Zaznaczenie lub usunięcie zaznaczenia pola wyboru `IsApproved` lub kliknięcie przycisku Odblokuj użytkownika spowoduje odświeżenie, ale nie wprowadzono żadnych modyfikacji na koncie użytkownika, ponieważ mamy jeszcze do utworzenia obsługi zdarzeń dla tych zdarzeń.

Wróć do programu Visual Studio i Utwórz programy obsługi zdarzeń dla zdarzenia `CheckedChanged` pola wyboru `IsApproved` i zdarzenia `Click` `UnlockUser` przycisku. W obsłudze zdarzeń `CheckedChanged` ustaw właściwość `IsApproved` użytkownika na Właściwość `Checked` pola wyboru, a następnie Zapisz zmiany za pośrednictwem wywołania do `Membership.UpdateUser`. W obsłudze zdarzeń `Click` po prostu wywołaj metodę `UnlockUser` obiektu `MembershipUser`. W obu programach obsługi zdarzeń wyświetla odpowiedni komunikat w etykiecie `StatusMessage`.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Testowanie strony`UserInformation.aspx`

W przypadku tych programów obsługi zdarzeń odwiedzanie strony i niezatwierdzonego użytkownika. Jak pokazano na rysunku 3, na stronie powinien zostać wyświetlony krótki komunikat informujący o tym, że właściwość `IsApproved` użytkownika została pomyślnie zmodyfikowana.

[![Krzysztof został niezatwierdzony](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Rysunek 3**: Krzysztof został niezatwierdzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image9.png))

Następnie wyloguj się, a następnie spróbuj zalogować się jako użytkownik, którego konto zostało właśnie niezatwierdzone. Ponieważ użytkownik nie jest zatwierdzony, nie może się zalogować. Domyślnie kontrolka logowania wyświetla ten sam komunikat, jeśli użytkownik nie może się zalogować, bez względu na przyczynę. Jednak w <a id="Tutorial6"> </a>temacie [*Weryfikowanie poświadczeń użytkownika względem magazynu użytkowników z członkostwem*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) zapoznaniu się z udoskonaleniem kontrolki logowania, aby wyświetlić bardziej odpowiedni komunikat. Jak pokazano na rysunku 4, Krzysztof zostanie pokazany komunikat wyjaśniający, że nie może się zalogować, ponieważ jego konto nie jest jeszcze zatwierdzone.

[![Krzysztof nie może zalogować się, ponieważ jego konto jest niezatwierdzone](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Rysunek 4**: Krzysztof nie może zalogować się, ponieważ jego konto jest niezatwierdzone ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image12.png))

Aby przetestować zablokowaną funkcję, spróbuj zalogować się jako zatwierdzony użytkownik, ale Użyj nieprawidłowego hasła. Powtórz ten proces, gdy będzie to konieczne, dopóki konto użytkownika nie zostanie zablokowane. Kontrolka logowania została również zaktualizowana w celu wyświetlenia komunikatu niestandardowego w przypadku próby zalogowania się z zablokowanego konta. Wiadomo, że konto zostało zablokowane po rozpoczęciu wyświetlania następującego komunikatu na stronie logowania: "Twoje konto zostało zablokowane z powodu zbyt dużej liczby nieudanych prób logowania. Skontaktuj się z administratorem, aby konto zostało odblokowane. "

Wróć do strony `ManageUsers.aspx` i kliknij link Zarządzanie dla zablokowanego użytkownika. Jak pokazano na rysunku 5, powinna zostać wyświetlona wartość w `LastLockedOutDateLabel` przycisk Odblokuj użytkownika powinien być włączony. Kliknij przycisk Odblokuj użytkownika, aby odblokować konto użytkownika. Po odblokowaniu użytkownika będą mogli zalogować się ponownie.

[![Dave został zablokowany przez system](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Rysunek 5**: Dave został zablokowany z systemu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image15.png))

## <a name="step-2-specifying-new-users-approved-status"></a>Krok 2: określanie stanu zatwierdzenia nowego użytkownika

Stan zatwierdzony jest użyteczny w scenariuszach, w których należy wykonać pewne działania, zanim nowy użytkownik będzie mógł zalogować się i uzyskać dostęp do funkcji specyficznych dla użytkownika w witrynie. Na przykład może być używana prywatna witryna sieci Web, w której wszystkie strony, z wyjątkiem stron logowania i rejestracji, są dostępne tylko dla użytkowników uwierzytelnionych. Ale co się stanie, jeśli jest to nieoczekiwane działanie w witrynie sieci Web, znajduje się Strona rejestracji i tworzy konto? Aby temu zapobiec, można przenieść stronę rejestracji do folderu `Administration` i wymagać od administratora ręcznego utworzenia każdego konta. Alternatywnie można zezwolić wszystkim na rejestrację, ale uniemożliwić dostęp do lokacji, dopóki administrator nie zatwierdzi konta użytkownika.

Domyślnie formant formancie CreateUserWizard zatwierdza nowe konta. To zachowanie można skonfigurować przy użyciu [właściwości`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)kontrolki. Ustaw tę właściwość na `true`, aby nie zatwierdzać nowych kont użytkowników.

> [!NOTE]
> Domyślnie formant formancie CreateUserWizard automatycznie loguje się do nowego konta użytkownika. To zachowanie jest podyktowane przez [właściwość`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)formantu. Ponieważ niezatwierdzeni użytkownicy nie mogą zalogować się do lokacji, gdy `DisableCreatedUser` jest `true` nowe konto użytkownika nie jest zalogowany do lokacji, niezależnie od wartości właściwości `LoginCreatedUser`.

Jeśli programowe tworzenie nowych kont użytkowników odbywa się za pomocą metody `Membership.CreateUser`, aby utworzyć niezatwierdzone konto użytkownika, użyj jednego z przeciążeń akceptujących wartość właściwości `IsApproved` nowego użytkownika jako parametr wejściowy.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Krok 3. zatwierdzanie użytkowników przez zweryfikowanie ich adresów E-mail

Wiele witryn sieci Web, które obsługują konta użytkowników, nie zatwierdza nowych użytkowników do momentu zweryfikowania adresu e-mail podanego podczas rejestracji. Ten proces weryfikacji jest często używany do odbotów, spamerów i innych ne'erów, ponieważ wymaga unikatowego, zweryfikowanego adresu e-mail i dodaje dodatkowy krok procesu rejestracji. Przy użyciu tego modelu po każdym zarejestrowaniu się nowego użytkownika zostanie wysłana wiadomość e-mail zawierająca link do strony weryfikacji. Odwiedzając link, użytkownik udowodnił, że otrzymał wiadomość e-mail, i w związku z tym, że podany adres e-mail jest prawidłowy. Strona weryfikacji jest odpowiedzialna za zatwierdzenie użytkownika. Może to nastąpić automatycznie, co spowoduje zatwierdzenie dowolnego użytkownika, który osiągnie Tę stronę, lub tylko wtedy, gdy użytkownik poda pewne dodatkowe informacje, takie jak [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Aby obsłużyć ten przepływ pracy, najpierw należy zaktualizować stronę tworzenia konta, tak aby nowi użytkownicy nie mieli zatwierdzenia. Otwórz stronę `EnhancedCreateUserWizard.aspx` w folderze `Membership` i ustaw właściwość `DisableCreatedUser` formantu formancie CreateUserWizard na `true`.

Następnie musimy skonfigurować formant formancie CreateUserWizard, aby wysłać wiadomość e-mail do nowego użytkownika z instrukcjami dotyczącymi sposobu weryfikacji konta. W szczególności dołączymy link w wiadomości e-mail do strony `Verification.aspx` (która została jeszcze utworzona), przekazując do `UserId` nowego użytkownika za pośrednictwem QueryString. Strona `Verification.aspx` będzie odszukać określonego użytkownika i oznaczyć je jako zatwierdzone.

### <a name="sending-a-verification-email-to-new-users"></a>Wysyłanie weryfikacyjnej wiadomości E-mail do nowych użytkowników

Aby wysłać wiadomość e-mail z formantu formancie CreateUserWizard, należy odpowiednio skonfigurować jej Właściwość `MailDefinition`. Zgodnie z opisem w <a id="Tutorial13"> </a> [poprzednim samouczku](recovering-and-changing-passwords-cs.md), kontrolki ChangePassword i PasswordRecovery zawierają [Właściwość`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) , która działa w taki sam sposób jak kontrolka formancie CreateUserWizard.

> [!NOTE]
> Aby użyć właściwości `MailDefinition`, należy określić opcje dostarczania poczty w `Web.config`. Aby uzyskać więcej informacji, zobacz [wysyłanie poczty e-mail w ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

Zacznij od utworzenia nowego szablonu wiadomości e-mail o nazwie `CreateUserWizard.txt` w folderze `EmailTemplates`. Użyj następującego tekstu dla szablonu:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Ustaw właściwość `MailDefinition'` s `BodyFileName` na wartość "~/EmailTemplates/CreateUserWizard.txt" i jej Właściwość `Subject` na "Witamy w mojej witrynie sieci Web! Aktywuj konto ".

Należy pamiętać, że szablon wiadomości e-mail `CreateUserWizard.txt` zawiera symbol zastępczy `<%VerificationUrl%>`. Jest to miejsce, w którym zostanie umieszczony adres URL strony `Verification.aspx`. Formancie CreateUserWizard automatycznie zastępuje symbole zastępcze `<%UserName%>` i `<%Password%>` za pomocą nazwy użytkownika i hasła nowego konta, ale nie ma wbudowanego symbolu zastępczego `<%VerificationUrl%>`. Musimy ręcznie zastąpić go odpowiednim adresem URL weryfikacji.

Aby to osiągnąć, Utwórz procedurę obsługi zdarzeń dla [zdarzenia`SendingMail`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) formancie CreateUserWizard i Dodaj następujący kod:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

Zdarzenie `SendingMail` wyzwalane po zdarzeniu `CreatedUser`, co oznacza, że przez czas wykonania powyższego zdarzenia przez program obsługi nowego konta użytkownika zostało już utworzone. Możemy uzyskać dostęp do wartości `UserId` nowego użytkownika przez wywołanie metody `Membership.GetUser`, przekazanie `UserName` wprowadzone do formantu formancie CreateUserWizard. Następnie zostanie utworzony adres URL weryfikacji. Instrukcja `Request.Url.GetLeftPart(UriPartial.Authority)` zwraca część `http://yourserver.com` adresu URL; `Request.ApplicationPath` zwraca ścieżkę, w której aplikacja jest odblokowana. Adres URL weryfikacji jest następnie definiowany jako `Verification.aspx?ID=userId`. Te dwa ciągi są następnie łączone w celu utworzenia kompletnego adresu URL. Na koniec treść wiadomości e-mail (`e.Message.Body`) zawiera wszystkie wystąpienia `<%VerificationUrl%>` zastąpione pełnym adresem URL.

Efektem netto jest fakt, że nowi użytkownicy nie są zatwierdzeni, co oznacza, że nie mogą zalogować się do lokacji. Ponadto automatycznie wysyłają wiadomość e-mail z linkiem do adresu URL weryfikacji (zobacz rysunek 6).

[![nowy użytkownik otrzymuje wiadomość E-mail z linkiem do adresu URL weryfikacji](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Ilustracja 6**. nowy użytkownik otrzymuje wiadomość E-mail z linkiem do adresu URL weryfikacji ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image18.png))

> [!NOTE]
> Domyślny krok formancie CreateUserWizard formantu formancie CreateUserWizard wyświetla komunikat informujący użytkownika, że konto zostało utworzone i wyświetla przycisk Kontynuuj. Kliknięcie tej opcji spowoduje przejście użytkownika do adresu URL określonego przez właściwość `ContinueDestinationPageUrl` formantu. Formancie CreateUserWizard w `EnhancedCreateUserWizard.aspx` jest skonfigurowany do wysyłania nowych użytkowników do `~/Membership/AdditionalUserInfo.aspx`, które monitują użytkownika o ich miejscowość zamieszkania, adres URL strony głównej i podpis. Ponieważ te informacje mogą być dodawane tylko przez zalogowanych użytkowników, warto zaktualizować tę właściwość, aby wysyłać użytkowników z powrotem do strony głównej witryny (`~/Default.aspx`). Ponadto należy rozszerzyć stronę `EnhancedCreateUserWizard.aspx` lub krok formancie CreateUserWizard, aby poinformować użytkownika o tym, że wiadomość e-mail została wysłana, a ich konto nie zostanie aktywowane do momentu wykonania instrukcji przedstawionych w tej wiadomości e-mail. Te zmiany należy pozostawić jako ćwiczenie dla czytnika.

### <a name="creating-the-verification-page"></a>Tworzenie strony weryfikacji

Ostatnim zadaniem jest utworzenie strony `Verification.aspx`. Dodaj tę stronę do folderu głównego, kojarząc ją ze stroną wzorcową `Site.master`. Tak jak w przypadku większości poprzednich stron zawartości dodanych do lokacji, Usuń kontrolkę zawartości odwołującą się do `LoginContent` ContentPlaceHolder, aby strona zawartości używała domyślnej zawartości strony wzorcowej.

Dodaj kontrolkę sieci Web etykieta do strony `Verification.aspx`, ustaw jej `ID` na `StatusMessage` i wyczyść jej właściwość Text. Następnie Utwórz procedurę obsługi zdarzeń `Page_Load` i Dodaj następujący kod:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

Za pomocą powyższego kodu Sprawdzamy, czy `UserId` dostarczone przez QueryString istnieje, że jest to prawidłowa `Guid` wartość i że odwołuje się do istniejącego konta użytkownika. Jeśli wszystkie te sprawdzenia zostaną zakończone, konto użytkownika zostanie zatwierdzone; w przeciwnym razie zostanie wyświetlony odpowiedni komunikat o stanie.

Rysunek 7 przedstawia stronę `Verification.aspx`, gdy jest odwiedzana przez przeglądarkę.

[![konto nowego użytkownika jest teraz zatwierdzone](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Rysunek 7**. konto nowego użytkownika zostało już zatwierdzone ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](unlocking-and-approving-user-accounts-cs/_static/image21.png))

## <a name="summary"></a>Podsumowanie

Wszystkie konta użytkowników członkostwa mają dwa stany, które określają, czy użytkownik może zalogować się do witryny: `IsLockedOut` i `IsApproved`. Aby użytkownik mógł się zalogować, muszą być `true` obie te właściwości.

Stan zablokowany użytkownika służy jako środek zabezpieczeń, aby zmniejszyć prawdopodobieństwo obniżenia przez hakera do lokacji za pomocą metod bezstanowych. W konkretnym momencie użytkownik jest zablokowany, jeśli istnieje pewna liczba nieprawidłowych prób logowania w określonym oknie. Te granice można konfigurować za pomocą ustawień dostawcy członkostwa w `Web.config`.

Stan zatwierdzony jest często używany jako środek, aby uniemożliwić nowym użytkownikom zalogowanie się do momentu, gdy niektóre akcje nie transpired. Być może lokacja wymaga, aby nowe konta były najpierw zatwierdzone przez administratora lub, jak zostało to opisane w kroku 3 przez zweryfikowanie adresu e-mail.

Szczęśliwe programowanie!

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](recovering-and-changing-passwords-cs.md)
> [dalej](building-an-interface-to-select-one-user-account-from-many-vb.md)

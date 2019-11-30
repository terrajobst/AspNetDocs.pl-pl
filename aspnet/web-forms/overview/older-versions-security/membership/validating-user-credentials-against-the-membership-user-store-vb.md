---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: Weryfikowanie poświadczeń użytkownika względem magazynu użytkowników członkostwa (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku sprawdzimy, jak sprawdzać poprawność poświadczeń użytkownika względem magazynu użytkowników członkostwa przy użyciu metod programistycznych i kontroli logowania....
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: 37574e4cdc86f518d01d12da58cc2862bc77d463
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643244"
---
# <a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Walidacja poświadczeń użytkownika względem magazynu użytkowników członkostwa (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> W tym samouczku sprawdzimy, jak sprawdzać poprawność poświadczeń użytkownika względem magazynu użytkowników członkostwa przy użyciu metod programistycznych i kontroli logowania. Dowiesz się również, jak dostosować wygląd i zachowanie kontrolki logowania.

## <a name="introduction"></a>Wprowadzenie

<a id="Tutorial05"> </a>W [poprzednim samouczku](creating-user-accounts-vb.md) pokazano, jak utworzyć nowe konto użytkownika w środowisku członkostwa. Najpierw zawarto programowo utworzyć konta użytkowników za pomocą metody `CreateUser` `Membership` klasy, a następnie sprawdzić ją przy użyciu kontrolki sieci Web formancie CreateUserWizard. Na stronie logowania jest obecnie sprawdzane poprawność podanych poświadczeń, a na stałe kodowanej listy par nazw i haseł. Musimy zaktualizować logikę strony logowania, aby sprawdzać poprawność poświadczeń względem magazynu użytkowników struktury członkostwa.

Podobnie jak w przypadku tworzenia kont użytkowników, poświadczenia mogą być sprawdzane programowo lub deklaratywnie. Interfejs API członkostwa zawiera metodę programistycznego weryfikowania poświadczeń użytkownika względem magazynu użytkownika. I ASP.NET są dostarczane z kontrolką sieci Web logowania, która renderuje interfejs użytkownika w postaci pól tekstowych dla nazwy użytkownika i hasła oraz przycisku, aby się zalogować.

W tym samouczku sprawdzimy, jak sprawdzać poprawność poświadczeń użytkownika względem magazynu użytkowników członkostwa przy użyciu metod programistycznych i kontroli logowania. Dowiesz się również, jak dostosować wygląd i zachowanie kontrolki logowania. Zacznijmy!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Krok 1. Weryfikowanie poświadczeń względem magazynu użytkowników członkostwa

W przypadku witryn sieci Web korzystających z uwierzytelniania formularzy użytkownik loguje się do witryny sieci Web, odwiedzając stronę logowania i wprowadzając swoje poświadczenia. Te poświadczenia są następnie porównywane z magazynem użytkowników. Jeśli są prawidłowe, użytkownik otrzymuje bilet uwierzytelniania formularzy, czyli token zabezpieczający, który wskazuje tożsamość i autentyczność osoby odwiedzającej.

Aby sprawdzić poprawność użytkownika względem struktury członkostwa, użyj [metody`ValidateUser`](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)klasy `Membership`. Metoda `ValidateUser` przyjmuje dwa parametry wejściowe — `username` i `password`-i zwraca wartość logiczną wskazującą, czy poświadczenia są prawidłowe. Podobnie jak w przypadku metody `CreateUser`, która została sprawdzona w poprzednim samouczku, Metoda `ValidateUser` deleguje rzeczywistą weryfikację do skonfigurowanego dostawcy członkostwa.

`SqlMembershipProvider` sprawdza poprawność podanych poświadczeń przez uzyskanie hasła określonego użytkownika za pomocą `aspnet_Membership_GetPasswordWithFormat` procedury składowanej. Należy przypomnieć, że `SqlMembershipProvider` przechowuje hasła użytkowników przy użyciu jednego z trzech formatów: Clear, Encrypted lub hash. Procedura składowana `aspnet_Membership_GetPasswordWithFormat` zwraca hasło w formacie nieprzetworzonym. W przypadku haseł szyfrowanych lub skrótów `SqlMembershipProvider` przekształca wartość `password` przekazywaną do metody `ValidateUser` do jej równoważnego stanu zaszyfrowanego lub skrótu, a następnie porównuje ją z informacjami zwracanymi z bazy danych. Jeśli hasło przechowywane w bazie danych jest zgodne z sformatowanym hasłem wprowadzonym przez użytkownika, poświadczenia są prawidłowe.

Zaktualizujmy nasze strony logowania (~/`Login.aspx`), aby sprawdzać poprawność podanych poświadczeń względem magazynu użytkowników struktury członkostwa. Ta strona logowania została utworzona z powrotem w <a id="Tutorial02"> </a> [*Omówienie samouczka dotyczącego uwierzytelniania formularzy*](../introduction/an-overview-of-forms-authentication-vb.md) , tworzenia interfejsu zawierającego dwa pola tekstowe dla nazwy użytkownika i hasła, pole Zapamiętaj mi i przycisk Zaloguj (patrz rysunek 1). Kod sprawdza poprawność wprowadzonych poświadczeń względem twardej listy par nazw i haseł (Scott/Password, Jisun/Password i sam/Password). <a id="Tutorial03"> </a>W samouczku [*konfiguracja uwierzytelniania formularzy i Tematy zaawansowane*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) — Zaktualizowaliśmy kod strony logowania, aby przechowywać dodatkowe informacje we właściwości `UserData` biletu uwierzytelniania formularzy.

[![interfejs strony logowania zawiera dwa pola tekstowe, formant CheckBoxList i przycisk](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Rysunek 1**. interfejs strony logowania zawiera dwa pola tekstowe, formant CheckBoxList i przycisk ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))

Interfejs użytkownika strony logowania może pozostawać niezmieniony, ale musimy zamienić `Click` obsługi zdarzeń w ramach tego przycisku logowania z kodem, który weryfikuje użytkownika względem magazynu użytkowników struktury członkostwa. Zaktualizuj procedurę obsługi zdarzeń, tak aby kod pojawił się w następujący sposób:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Ten kod jest niezwykle prosty. Zaczynamy od wywołania metody `Membership.ValidateUser`, przekazując w podanej nazwie użytkownika i haśle. Jeśli ta metoda zwróci wartość true, użytkownik jest zalogowany do lokacji za pomocą metody RedirectFromLoginPage klasy `FormsAuthentication`. (Zgodnie z opisem w <a id="Tutorial02"> </a>temacie Omówienie samouczka [*dotyczącego uwierzytelniania formularzy*](../introduction/an-overview-of-forms-authentication-vb.md) `FormsAuthentication.RedirectFromLoginPage` tworzy bilet uwierzytelniania formularzy, a następnie przekierowuje użytkownika do odpowiedniej strony). Jeśli poświadczenia są nieprawidłowe, jednak zostanie wyświetlona etykieta `InvalidCredentialsMessage`, informujący użytkownika, że jego nazwa użytkownika lub hasło są niepoprawne.

To wszystko.

Aby sprawdzić, czy strona logowania działa zgodnie z oczekiwaniami, spróbuj zalogować się przy użyciu jednego z kont użytkowników utworzonych w poprzednim samouczku. Lub, jeśli jeszcze nie utworzono konta, przejdź na stronę `~/Membership/CreatingUserAccounts.aspx`.

> [!NOTE]
> Gdy użytkownik wprowadzi swoje poświadczenia i prześle formularz strony logowania, poświadczenia, łącznie z jego hasłem, są przesyłane przez Internet do serwera sieci Web w *postaci zwykłego tekstu*. Oznacza to, że każdy haker będący w trakcie wykrywania ruchu sieciowego zobaczy nazwę użytkownika i hasło. Aby tego uniknąć, należy zaszyfrować ruch sieciowy przy użyciu [protokołu SSL (Secure Sockets)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Dzięki temu poświadczenia (a także znacznik HTML całej strony) są szyfrowane od momentu opuszczenia przeglądarki do momentu odebrania ich przez serwer sieci Web.

### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Jak struktura członkostwa obsługuje nieprawidłowe próby logowania

Gdy osoba odwiedzająca dociera do strony logowania i przesyła swoje poświadczenia, przeglądarka wysyła żądanie HTTP do strony logowania. Jeśli poświadczenia są prawidłowe, odpowiedź HTTP obejmuje bilet uwierzytelniania w pliku cookie. W związku z tym haker próbujący przerwać przerwanie w lokacji może utworzyć program, który w sposób wyczerpujący wysyła żądania HTTP do strony logowania przy użyciu prawidłowej nazwy użytkownika i odgadnięcia hasła. Jeśli wartość hasła jest poprawna, na stronie logowania zostanie zwrócony plik cookie biletu uwierzytelniania, po którym program wie, że ma stumbled na prawidłowej parze nazwy użytkownika/hasła. Dzięki wykorzystaniu pełnego programu może być możliwe Stumble na hasło użytkownika, zwłaszcza jeśli hasło jest słabe.

Aby zapobiec takim atakom, platforma członkowska blokuje użytkownika, jeśli istnieje pewna liczba nieudanych prób logowania w określonym czasie. Dokładne parametry można konfigurować za pomocą następujących dwóch ustawień konfiguracji dostawcy członkostwa:

- `maxInvalidPasswordAttempts` — określa, ile nieprawidłowych prób dotyczących hasła jest dozwolonych dla użytkownika w danym okresie, zanim konto zostanie zablokowane. Wartość domyślna to 5.
- `passwordAttemptWindow` — wskazuje czas w minutach, w którym określona liczba nieudanych prób logowania spowoduje, że konto zostanie zablokowane. Wartość domyślna to 10.

Jeśli użytkownik został zablokowany, nie może się zalogować do momentu odblokowania jego konta przez administratora. Gdy użytkownik jest zablokowany, Metoda `ValidateUser` *zawsze* będzie zwracać `False`, nawet jeśli podano prawidłowe poświadczenia. Chociaż to zachowanie zmniejsza prawdopodobieństwo przerwania przez hakera do lokacji za pomocą metod odrządzenia, może to spowodować zablokowanie prawidłowego użytkownika, który po prostu zapomniał hasło, lub przypadkowo ma blokadę lub ma zły dzień wpisywania.

Niestety, nie istnieje wbudowane narzędzie do odblokowywania konta użytkownika. W celu odblokowania konta można modyfikować bazę danych bezpośrednio, zmieniając pole `IsLockedOut` w tabeli `aspnet_Membership` dla odpowiedniego konta użytkownika lub tworząc interfejs oparty na sieci Web zawierający listę zablokowanych kont z opcjami umożliwiającymi ich odblokowanie. Będziemy analizować tworzenie interfejsów administracyjnych na potrzeby wykonywania typowych zadań dotyczących kont użytkowników i ról w przyszłym samouczku.

> [!NOTE]
> Jedną z minusem metody `ValidateUser` jest to, że gdy podane poświadczenia są nieprawidłowe, nie zawiera żadnych wyjaśnień dotyczących przyczyn. Poświadczenia mogą być nieprawidłowe, ponieważ w magazynie użytkownika nie ma pasującej pary username/Password lub użytkownik nie został jeszcze zatwierdzony lub użytkownik został zablokowany. W kroku 4 zobaczysz, jak wyświetlić bardziej szczegółowy komunikat dla użytkownika, gdy próba logowania nie powiedzie się.

## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Krok 2. zbieranie poświadczeń za pomocą kontrolki sieci Web logowania

[Kontrolka sieci Web logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) renderuje domyślny interfejs użytkownika bardzo podobny do tego, który został utworzony z <a id="Tutorial02"> </a>powrotem w Omówienie samouczka [*dotyczącego uwierzytelniania formularzy*](../introduction/an-overview-of-forms-authentication-vb.md) . Użycie kontrolki logowania umożliwia nam utworzenie interfejsu w celu zebrania poświadczeń osoby odwiedzającej. Ponadto kontrolka logowania automatycznie loguje użytkownika (przy założeniu, że przesłane poświadczenia są prawidłowe), dzięki czemu nie trzeba pisać kodu.

Zaktualizujmy `Login.aspx`, zastępując ręcznie utworzony interfejs i kod przy użyciu kontrolki logowania. Zacznij od usunięcia istniejącego znacznika i kodu w `Login.aspx`. Licencjobiorca może usunąć to prawo lub po prostu dodać komentarz do niego. Aby dodać komentarz do znaczników deklaratywnych, należy ująć ją w ograniczniki `<%--` i `--%>`. Można wprowadzić te ograniczniki ręcznie lub, jak pokazano na rysunku 2, możesz zaznaczyć tekst do skomentowania, a następnie kliknąć ikonę komentarza wybrane wiersze na pasku narzędzi. Analogicznie, można użyć ikony komentarz w wybranym wierszu, aby dodać komentarz do zaznaczonego kodu w klasie powiązanej z kodem.

[![Komentarze do istniejącego deklaratywnego znacznika i kodu źródłowego w Login. aspx](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Rysunek 2**: komentarz dotyczący istniejącego deklaratywnego znacznika i kodu źródłowego w Login. aspx ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))

> [!NOTE]
> Ikona komentarz z wybranych linii jest niedostępna podczas wyświetlania znaczników deklaratywnych w programie Visual Studio 2005. Jeśli nie używasz programu Visual Studio 2008, konieczne będzie ręczne dodanie `<%--` i ograniczników `--%>`.

Następnie przeciągnij kontrolkę login z przybornika na stronę i ustaw jej Właściwość `ID` na `myLogin`. W tym momencie ekran powinien wyglądać podobnie do rysunku 3. Należy zauważyć, że domyślny interfejs kontrolki logowania zawiera kontrolki tekstowe dla nazwy użytkownika i hasła, pole wyboru Zapamiętaj mnie o następnym czasie i przycisk Zaloguj. Istnieją również `RequiredFieldValidator` kontrolki dla dwóch pól tekstowych.

[![dodać kontrolkę logowania do strony](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Rysunek 3**. Dodawanie kontrolki logowania do strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))

Wszystko gotowe! Po kliknięciu przycisku Zaloguj się w kontrolce logowania zostanie przeprowadzone ogłaszanie zwrotne, a kontrolka logowania wywoła metodę `Membership.ValidateUser`, przekazując wprowadzoną nazwę użytkownika i hasło. Jeśli poświadczenia są nieprawidłowe, w formancie logowania zostanie wyświetlony komunikat z informacją. Jeśli jednak poświadczenia są prawidłowe, wówczas formant logowania tworzy bilet uwierzytelniania formularzy i przekierowuje użytkownika do odpowiedniej strony.

Kontrolka logowania używa czterech czynników do określenia odpowiedniej strony, aby przekierować użytkownika do pomyślnego zalogowania:

- Czy kontrolka logowania znajduje się na stronie logowania zgodnie z definicją `loginUrl` ustawienia w konfiguracji uwierzytelniania formularzy; wartość domyślna tego ustawienia to `Login.aspx`
- Obecność `ReturnUrl` ciągu QueryString
- Wartość [właściwości`DestinationUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx) kontrolki logowania
- Wartość `defaultUrl` określona w ustawieniach konfiguracji uwierzytelniania formularzy; wartość domyślna tego ustawienia to default. aspx

Rysunek 4 przedstawia sposób, w jaki formant logowania używa tych czterech parametrów do uzyskania odpowiedniej decyzji dotyczącej strony.

[![dodać kontrolkę logowania do strony](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Ilustracja 4**. Dodawanie kontrolki logowania do strony ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))

Poświęć chwilę na przetestowanie kontroli logowania, odwiedzając witrynę za pomocą przeglądarki i logując się jako istniejący użytkownik w strukturze członkostwa.

Interfejs renderowanej kontrolki logowania jest wysoce konfigurowalny. Istnieje wiele właściwości, które wpływają na wygląd; Co więcej, formant logowania można przekonwertować na szablon w celu precyzyjnej kontroli nad układem elementów interfejsu użytkownika. Pozostała część tego kroku przeanalizuje sposób dostosowywania wyglądu i układu.

### <a name="customizing-the-login-controls-appearance"></a>Dostosowywanie wyglądu kontrolki logowania

Domyślne ustawienia właściwości kontrolki logowania renderują interfejs użytkownika z tytułem (logowanie), polem tekstowym i formantami etykiet dla danych wejściowych nazwy użytkownika i hasła, pole wyboru Zapamiętaj do następnego czasu i przycisk Zaloguj. Wygląd tych elementów można konfigurować za pomocą licznych właściwości kontrolki logowania. Ponadto dodatkowe elementy interfejsu użytkownika — takie jak link do strony, aby utworzyć nowe konto użytkownika — można dodać, ustawiając właściwość lub dwie.

Poświęć kilka minut, aby Gussy wygląd naszego formantu logowania. Ponieważ strona `Login.aspx` zawiera już tekst w górnej części strony mówiącej o logowaniu, tytuł kontrolki logowania jest zbędny. W związku z tym Wyczyść wartość [właściwości`TitleText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) , aby usunąć tytuł kontrolki logowania.

Nazwa użytkownika: i hasło: etykiety po lewej stronie dwóch formantów TextBox można dostosować odpowiednio do właściwości [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) i [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx). Zmieńmy nazwę użytkownika na: etykieta, aby odczytać nazwę użytkownika:. Etykiety i style TextBox można konfigurować odpowiednio przy użyciu właściwości [`LabelStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) i [`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx).

Właściwość Text pola wyboru Zapamiętaj mnie o następnym czasie można ustawić za pomocą [właściwości`RememberMeText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)kontrolki logowania, a jej domyślny stan zaznaczenia jest konfigurowalny za pomocą [Właściwości`RememberMeSet`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (która domyślnie ma wartość false). Przejdź dalej i ustaw właściwość `RememberMeSet` na wartość true, aby pole wyboru Zapamiętaj mnie o następnym czasie będzie domyślnie zaznaczone.

Kontrolka logowania oferuje dwie właściwości służące do dostosowywania układu formantów interfejsu użytkownika. [Właściwość`TextLayout`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) wskazuje, czy nazwa użytkownika: i hasło: etykiety pojawiają się po lewej stronie odpowiednich pól tekstowych (domyślnie) lub nad nimi. [Właściwość`Orientation`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) wskazuje, czy dane wejściowe nazwy użytkownika i hasła są umiejscowione w pionie (jeden powyżej drugiego) czy poziomo. Muszę pozostawić te dwie właściwości jako domyślne, ale zachęcamy do wypróbowania tych dwóch właściwości jako wartości innych niż domyślne, aby zobaczyć wynikowy efekt.

> [!NOTE]
> W następnej sekcji Konfigurowanie układu kontrolki logowania będzie wyglądało przy użyciu szablonów do definiowania precyzyjnego układu elementów interfejsu użytkownika kontrolki układu.

Zapakuj ustawienia właściwości kontrolki logowania, ustawiając właściwości [`CreateUserText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) i [`CreateUserUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) tak, aby nie były jeszcze zarejestrowane? Utwórz konto! i odpowiednio `~/Membership/CreatingUserAccounts.aspx`. Spowoduje to dodanie hiperłącza do interfejsu kontrolki logowania wskazującego stronę utworzoną w <a id="Tutorial05"> </a> [poprzednim samouczku](creating-user-accounts-vb.md). Właściwości [`HelpPageText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) i [`HelpPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) kontrolki logowania oraz właściwości [`PasswordRecoveryText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) i [`PasswordRecoveryUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) działają w taki sam sposób, co renderuje linki do strony pomocy i strony odzyskiwania hasła.

Po wprowadzeniu tych zmian we właściwościach deklaracyjne znaczniki i wygląd kontrolki logowania powinny wyglądać podobnie do przedstawionej na rysunku 5.

[![wartości właściwości kontrolki logowania są podyktowane ich wyglądem](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Rysunek 5**. wartości właściwości kontrolki logowania są podyktowane ich wyglądem ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))

### <a name="configuring-the-login-controls-layout"></a>Konfigurowanie układu kontrolki logowania

Domyślny interfejs użytkownika kontrolki sieci Web logowania określa interfejs w `<table>`HTML. Ale co zrobić, jeśli potrzebujemy bardziej precyzyjnej kontroli nad renderowanymi danymi wyjściowymi? Być może chcemy zastąpić `<table>` serią `<div>`. Lub co zrobić, jeśli nasza aplikacja wymaga dodatkowych poświadczeń do uwierzytelniania? Wiele finansowych witryn sieci Web, na przykład, wymaga od użytkowników podania nazwy użytkownika i hasła, ale również osobistego numeru identyfikacyjnego (PIN) lub innych informacji identyfikacyjnych. Ze względu na to, że można dokonać konwersji kontrolki logowania do szablonu, z którego możemy jawnie zdefiniować znaczniki deklaratywne interfejsu.

Aby zaktualizować kontrolkę logowania w celu zbierania dodatkowych poświadczeń, musimy wykonać dwie czynności:

1. Zaktualizuj interfejs kontrolki logowania, aby uwzględnić kontrolki sieci Web w celu zebrania dodatkowych poświadczeń.
2. Zastąp wewnętrzną logikę uwierzytelniania kontrolki logowania, aby użytkownik był uwierzytelniany tylko wtedy, gdy ich nazwa użytkownika i hasło są prawidłowe, a ich dodatkowe poświadczenia są również poprawne.

Aby wykonać pierwsze zadanie, musimy przekonwertować formant logowania na szablon i dodać wymagane kontrolki sieci Web. Podobnie jak w przypadku drugiego zadania logika uwierzytelniania kontrolki logowania może zostać zastąpiona przez utworzenie programu obsługi zdarzeń dla [zdarzenia`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)formantu.

Zaktualizujmy kontrolę logowania, aby monitował użytkowników o podanie nazwy użytkownika, hasła i adresu e-mail, a następnie uwierzytelniać użytkownika tylko wtedy, gdy podany adres e-mail jest zgodny z adresem e-mail w pliku. Najpierw musimy przekonwertować interfejs kontroli logowania na szablon. W tagu inteligentnym kontrolki logowania wybierz opcję Konwertuj na szablon.

[![skonwertować kontrolkę logowania na szablon](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Ilustracja 6**. konwertowanie kontrolki logowania na szablon ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))

> [!NOTE]
> Aby przywrócić kontrolę logowania do jej wersji sprzed szablonu, kliknij link Zresetuj z tagu inteligentnego kontrolki.

Konwersja kontrolki logowania na szablon dodaje `LayoutTemplate` do znaczników deklaratywnych kontrolki za pomocą elementów HTML i kontrolek sieci Web definiujących interfejs użytkownika. Jak pokazano na rysunku 7, konwersja kontrolki na szablon usuwa wiele właściwości z okno Właściwości, takich jak `TitleText`, `CreateUserUrl`i tak dalej, ponieważ te wartości właściwości są ignorowane przy użyciu szablonu.

[![mniej właściwości są dostępne, gdy kontrolka logowania zostanie przekonwertowana na szablon](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Rysunek 7**. gdy kontrolka logowania zostanie przekonwertowana na szablon ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png)), są dostępne mniej właściwości.

Znaczniki HTML w `LayoutTemplate` mogą być modyfikowane w razie konieczności. Podobnie możesz dodać do szablonu nowe kontrolki sieci Web. Jednak ważne jest, aby podstawowe kontrolki sieci Web kontroli logowania pozostawały w szablonie i zachować przypisane wartości `ID`. W szczególności nie usuwaj ani nie zmieniaj nazw `UserName` lub `Password` pól text, `RememberMe` pole wyboru, przycisk `LoginButton`, etykieta `FailureText` lub kontrolki `RequiredFieldValidator`.

Aby zebrać adres e-mail osoby odwiedzającej, musimy dodać do szablonu pole tekstowe. Dodaj następujące znaczniki deklaratywne między wierszem tabeli (`<tr>`), który zawiera pole tekstowe `Password` i wiersz tabeli zawierający pole wyboru Zapamiętaj mnie o następnym czasie:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Po dodaniu pola tekstowego `Email` przejdź do strony za pomocą przeglądarki. Jak pokazano na rysunku 8, interfejs użytkownika kontrolki logowania zawiera teraz trzecie pole tekstowe.

[![kontrolka logowania zawiera teraz pole tekstowe dla adresu E-mail użytkownika](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Ilustracja 8**. kontrolka logowania zawiera teraz pole tekstowe dla adresu e-mail użytkownika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))

W tym momencie kontrolka logowania nadal używa metody `Membership.ValidateUser`, aby sprawdzić poprawność podanych poświadczeń. Odpowiadająca wartość wprowadzona w polu tekstowym `Email` nie ma znaczenia, czy użytkownik może się zalogować. W kroku 3 zawarto informacje na temat przesłonięcia logiki uwierzytelniania kontrolki logowania w taki sposób, że poświadczenia są uznawane za prawidłowe, jeśli nazwa użytkownika i hasło są prawidłowe, a podany adres e-mail jest zgodny z adresem e-mail w pliku.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Krok 3. Modyfikowanie logiki uwierzytelniania kontrolki logowania

Gdy osoba odwiedzająca poda swoje poświadczenia i klika przycisk Zaloguj, przebiega zwrotne, a kontrola logowania postępuje zgodnie z przepływem pracy uwierzytelniania. Przepływ pracy jest uruchamiany przez podnoszenie poziomu [zdarzenia`LoggingIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Wszystkie programy obsługi zdarzeń powiązane z tym zdarzeniem mogą anulować operację logowania przez ustawienie właściwości `e.Cancel` na `True`.

Jeśli operacja logowania nie zostanie anulowana, przepływ pracy postępuje, wywołując [zdarzenie`Authenticate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Jeśli istnieje program obsługi zdarzeń dla zdarzenia `Authenticate`, jest on odpowiedzialny za określenie, czy podane poświadczenia są prawidłowe. Jeśli nie określono obsługi zdarzeń, kontrolka logowania używa metody `Membership.ValidateUser`, aby określić ważność poświadczeń.

Jeśli podane poświadczenia są prawidłowe, zostanie utworzony bilet uwierzytelniania formularzy, zostanie zgłoszone [zdarzenie`LoggedIn`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) i użytkownik zostanie przekierowany do odpowiedniej strony. Jeśli jednak poświadczenia są uznawane za nieprawidłowe, zostanie zgłoszone [zdarzenie`LoginError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) i zostanie wyświetlony komunikat informujący użytkownika, że ich poświadczenia były nieprawidłowe. Domyślnie w przypadku niepowodzenia kontrolki logowania po prostu ustawi właściwość Text kontrolki `FailureText` etykiety na komunikat o błędzie (próba logowania nie powiedzie się. Spróbuj ponownie. Jeśli jednak [właściwość`FailureAction`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) kontrolki logowania jest ustawiona na wartość `RedirectToLoginPage`, kontrolka logowania wystawia `Response.Redirect` do strony logowania, która dołącza parametr QueryString `loginfailure=1` (co powoduje wyświetlenie komunikatu o błędzie przez formant logowania).

Rysunek 9 oferuje wykres przepływu dla przepływu pracy uwierzytelniania.

[![przepływu pracy uwierzytelniania kontrolki logowania](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Rysunek 9**: przepływ pracy uwierzytelniania kontrolki logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))

> [!NOTE]
> Jeśli zastanawiasz się, gdy korzystasz z opcji strony `RedirectToLogin` `FailureAction`, weź pod uwagę następujący scenariusz. Teraz nasza Strona główna `Site.master` ma obecnie tekst, Hello, dziwną nazwę wyświetlaną w lewej kolumnie podczas odwiedzania przez użytkownika anonimowego, ale Wyobraź sobie, że chcemy zamienić ten tekst na kontrolkę logowania. Dzięki temu anonimowy użytkownik może zalogować się z dowolnej strony w witrynie, zamiast wymagać od nich bezpośredniego odwiedzenia strony logowania. Jeśli jednak użytkownik nie mógł się zalogować za pomocą kontrolki logowania renderowanej przez stronę wzorcową, warto przekierować je do strony logowania (`Login.aspx`), ponieważ ta strona może zawierać dodatkowe instrukcje, linki i inną pomoc, taką jak linki do utworzenia nowego konta lub pobranie utraconego hasła, które nie zostały dodane do strony głównej.

### <a name="creating-theauthenticateevent-handler"></a>Tworzenie procedury obsługi zdarzeń`Authenticate`

Aby można było zalogować się do niestandardowej logiki uwierzytelniania, musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `Authenticate` formantu logowania. Utworzenie procedury obsługi zdarzeń dla zdarzenia `Authenticate` spowoduje wygenerowanie następującej definicji programu obsługi zdarzeń:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Jak widać, obsługa zdarzeń `Authenticate` jest przenoszona jako obiekt typu [`AuthenticateEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) jako drugi parametr wejściowy. Klasa `AuthenticateEventArgs` zawiera właściwość Boolean o nazwie `Authenticated`, która jest używana do określenia, czy podane poświadczenia są prawidłowe. Nasze zadanie, następnie napisz tutaj kod, który określa, czy podane poświadczenia są prawidłowe, i aby odpowiednio ustawić właściwość `e.Authenticate`.

### <a name="determining-and-validating-the-supplied-credentials"></a>Określanie i weryfikowanie podanych poświadczeń

Użyj właściwości [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) i [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) kontrolki logowania, aby określić poświadczenia nazwy użytkownika i hasła wprowadzone przez użytkownika. Aby określić wartości wprowadzone do dowolnych dodatkowych kontrolek sieci Web (takich jak `Email` TextBox dodane w poprzednim kroku), użyj `LoginControlID.FindControl`(" *`controlID`* "), aby uzyskać odwołanie programistyczne do formantu sieci Web w szablonie, którego właściwość `ID` jest równa *`controlID`* . Aby na przykład uzyskać odwołanie do pola tekstowego `Email`, użyj następującego kodu:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Aby sprawdzić poprawność poświadczeń użytkownika, należy wykonać dwie czynności:

1. Upewnij się, że podana nazwa użytkownika i hasło są prawidłowe.
2. Upewnij się, że wprowadzony adres e-mail jest zgodny z adresem e-mail w pliku, gdy użytkownik próbuje się zalogować

Aby osiągnąć pierwsze sprawdzenie, możemy po prostu użyć metody `Membership.ValidateUser`, jak pokazano w kroku 1. W przypadku drugiego sprawdzenia musimy określić adres e-mail użytkownika, aby można było porównać go z adresem e-mail wprowadzonym w kontrolce TextBox. Aby uzyskać informacje o określonym użytkowniku, użyj [metody`GetUser`](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)klasy `Membership`.

Metoda `GetUser` ma wiele przeciążeń. Jeśli używany bez przekazywania parametrów, zwraca informacje o aktualnie zalogowanym użytkowniku. Aby uzyskać informacje o określonym użytkowniku, wywołaj `GetUser` przekazywania ich nazwy użytkownika. W obu przypadkach `GetUser` zwraca [obiekt`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), który ma właściwości, takie jak `UserName`, `Email`, `IsApproved`, `IsOnline`i tak dalej.

Poniższy kod implementuje te dwa testy. Jeśli obie wartości są przekazywane, `e.Authenticate` jest ustawiona na `True`, w przeciwnym razie zostanie przypisany `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Gdy ten kod jest na miejscu, spróbuj zalogować się jako prawidłowy użytkownik, wprowadzając poprawną nazwę użytkownika, hasło i adres e-mail. Spróbuj ponownie, ale ten czas celowo całkowicie Użycie nieprawidłowego adresu e-mail (patrz rysunek 10). Na koniec wypróbuj go przy użyciu innej nazwy użytkownika. W pierwszym przypadku należy pomyślnie zalogować się do witryny, ale w ostatnich dwóch przypadkach powinien zostać wyświetlony komunikat nieprawidłowe poświadczenia kontrolki logowania.

[Nie można zalogować ![Tito w przypadku podania niepoprawnego adresu E-mail](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Rysunek 10**: Tito nie może się zalogować w przypadku podania niepoprawnego adresu E-mail ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))

> [!NOTE]
> Zgodnie z opisem w sekcji jak platforma członkowska obsługuje nieprawidłowe próby logowania w kroku 1, gdy metoda `Membership.ValidateUser` jest wywoływana i przekazała nieprawidłowe poświadczenia, śledzi nieprawidłowe próby logowania i blokuje użytkownika, jeśli przekroczą określony próg nieprawidłowych prób w określonym przedziale czasu. Ponieważ nasza logika uwierzytelniania niestandardowego wywołuje metodę `ValidateUser`, nieprawidłowe hasło dla prawidłowej nazwy użytkownika spowoduje zwiększenie nieprawidłowego licznika nieudanych prób logowania, ale ten licznik nie zostanie zwiększony w przypadku, gdy nazwa użytkownika i hasło są prawidłowe, ale adres e-mail jest nieprawidłowy. To zachowanie jest odpowiednie, ponieważ jest mało prawdopodobne, że haker wie o Poznaniu nazwy użytkownika i hasła, ale musi użyć technik w postaci pełnego siły, aby określić adres e-mail użytkownika.

## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Krok 4. ulepszanie komunikatu o nieprawidłowych poświadczeniach kontrolki logowania

Gdy użytkownik próbuje zalogować się przy użyciu nieprawidłowych poświadczeń, w kontrolce logowania zostanie wyświetlony komunikat z informacją o niepomyślnej próbie logowania. W szczególności formant Wyświetla komunikat określony przez jego [właściwość`FailureText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), która ma wartość domyślną próby logowania nie powiodło się. Spróbuj ponownie.

Odwołaj się, że istnieje wiele powodów, dla których poświadczenia użytkownika mogą być nieprawidłowe:

- Nazwa użytkownika może nie istnieć
- Nazwa użytkownika istnieje, ale hasło jest nieprawidłowe
- Nazwa użytkownika i hasło są prawidłowe, ale użytkownik nie jest jeszcze zatwierdzony
- Nazwa użytkownika i hasło są prawidłowe, ale użytkownik jest zablokowany (najprawdopodobniej z powodu przekroczenia liczby nieudanych prób logowania w określonym przedziale czasu)

Mogą jednak wystąpić inne przyczyny użycia niestandardowej logiki uwierzytelniania. Na przykład w przypadku kodu napisanego w kroku 3 Nazwa użytkownika i hasło mogą być prawidłowe, ale adres e-mail może być niepoprawny.

Niezależnie od tego, dlaczego poświadczenia są nieprawidłowe, kontrolka logowania wyświetla ten sam komunikat o błędzie. Brak opinii może być mylący dla użytkownika, którego konto nie zostało jeszcze zatwierdzone lub które zostało zablokowane. W przypadku niewielkiej liczby zadań w tym przypadku możemy wyświetlić bardziej odpowiedni komunikat w kontrolce logowania.

Za każdym razem, gdy użytkownik próbuje zalogować się przy użyciu nieprawidłowych poświadczeń, formant logowania zgłosi zdarzenie `LoginError`. Przejdź dalej i Utwórz procedurę obsługi zdarzeń dla tego zdarzenia i Dodaj następujący kod:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

Powyższy kod jest uruchamiany przez ustawienie właściwości `FailureText` kontrolki logowania na wartość domyślną (próba logowania zakończyła się niepowodzeniem. Spróbuj ponownie. Następnie sprawdza, czy podana nazwa użytkownika jest mapowana na istniejące konto użytkownika. W takim przypadku sprawdza `IsLockedOut` `MembershipUser` obiektu i właściwości `IsApproved`, aby określić, czy konto zostało zablokowane, czy nie zostało jeszcze zatwierdzone. W obu przypadkach Właściwość `FailureText` jest aktualizowana do odpowiedniej wartości.

Aby przetestować ten kod, celowo całkowicie próbę zalogowania się jako istniejący użytkownik, ale użyto nieprawidłowego hasła. Zrób to pięć razy w wierszu w ciągu 10 minut, a konto zostanie zablokowane. Jak pokazano na rysunku 11, kolejne próby logowania będą zawsze kończyć się niepowodzeniem (nawet przy użyciu poprawnego hasła), ale teraz zostaną wyświetlone bardziej opisowe konto zostało zablokowane z powodu zbyt wielu nieudanych prób logowania. Skontaktuj się z administratorem w celu odblokowania konta.

[![Tito wykonał zbyt wiele nieudanych prób logowania i została zablokowana](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Rysunek 11**. Tito wykonano zbyt wiele nieudanych prób logowania i została ona zablokowana ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))

## <a name="summary"></a>Podsumowanie

Wcześniej w tym samouczku Strona logowania została zweryfikowana przy użyciu podanych poświadczeń w oparciu o ustaloną na stałe listę par nazw i haseł. W tym samouczku Zaktualizowaliśmy stronę, aby sprawdzić poprawność poświadczeń względem struktury członkostwa. W kroku 1 wykorzystano programowo metodę `Membership.ValidateUser`. W kroku 2 został zastąpiony ręcznie utworzony interfejs użytkownika i kod przy użyciu kontrolki logowania.

Kontrolka logowania renderuje standardowy interfejs użytkownika logowania i automatycznie weryfikuje poświadczenia użytkownika względem struktury członkostwa. Ponadto w przypadku prawidłowych poświadczeń kontrola logowania podpisuje użytkownika w programie za pomocą uwierzytelniania formularzy. Krótko mówiąc, w pełni funkcjonalne środowisko użytkownika logowania jest dostępne przez wystarczy przeciąganie kontrolki logowania na stronę, bez konieczności dodatkowego znacznika lub kodu. Co więcej, kontrola logowania jest wysoce dostosowywana, co pozwala na bardziej precyzyjne kontrolowanie zarówno renderowanego interfejsu użytkownika, jak i logiki uwierzytelniania.

W tym momencie osoby odwiedzające witrynę sieci Web mogą utworzyć nowe konto użytkownika i zalogować się do witryny, ale zapoznajemy się z ograniczeniem dostępu do stron na podstawie uwierzytelnionego użytkownika. Obecnie każdy użytkownik, uwierzytelniony lub anonimowy może wyświetlić każdą stronę w naszej witrynie. Wraz z kontrolowaniem dostępu do stron naszej witryny w przypadku użytkowników indywidualnych firma Microsoft może mieć pewne strony, których funkcjonalność zależy od użytkownika. W następnym samouczku pokazano, jak ograniczyć dostęp i funkcjonalność na stronie na podstawie zalogowanego użytkownika.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Wyświetlanie komunikatów niestandardowych do zablokowanych i niezatwierdzonych użytkowników](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Badanie członkostwa, ról i profilu w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Instrukcje: Tworzenie strony logowania ASP.NET](https://msdn.microsoft.com/library/ms178331.aspx)
- [Dokumentacja techniczna dotycząca kontroli logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Korzystanie z kontrolek logowania](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka zostali Teresa Murphy i Michael Olivero. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-user-accounts-vb.md)
> [dalej](user-based-authorization-vb.md)

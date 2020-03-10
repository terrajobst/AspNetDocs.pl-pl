---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Tworzenie kont użytkowników (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku opisano użycie platformy członkostwa (za pośrednictwem SqlMembershipProvider) w celu utworzenia nowych kont użytkowników. Zobaczymy, jak utworzyć nową firmę...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 01be198c329f372ddcd529ad8a369f2d3426a9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586822"
---
# <a name="creating-user-accounts-vb"></a>Tworzenie kont użytkowników (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> W tym samouczku opisano użycie platformy członkostwa (za pośrednictwem SqlMembershipProvider) w celu utworzenia nowych kont użytkowników. Zobaczymy, jak tworzyć nowych użytkowników programowo i za pomocą środowiska ASP. Wbudowana kontrolka formancie CreateUserWizard netto.

## <a name="introduction"></a>Wprowadzenie

<a id="_msoanchor_1"> </a>W [poprzednim samouczku](creating-the-membership-schema-in-sql-server-vb.md) zainstalowano schemat usług aplikacji w bazie danych, która dodała tabele, widoki i procedury składowane, które są niezbędne `SqlMembershipProvider` i `SqlRoleProvider`. Ta infrastruktura została utworzona w przypadku pozostałej części samouczków w tej serii. W tym samouczku opisano użycie platformy członkostwa (za pośrednictwem `SqlMembershipProvider`) do tworzenia nowych kont użytkowników. Zobaczymy, jak tworzyć nowych użytkowników programowo i za pomocą środowiska ASP. Wbudowana kontrolka formancie CreateUserWizard netto.

Oprócz uczenia się, jak tworzyć nowe konta użytkowników, należy również zaktualizować witrynę internetową demonstracyjną, która  *<a id="_msoanchor_2">[ ](../introduction/an-overview-of-forms-authentication-vb.md)</a>została najpierw utworzona w omówieniu samouczka dotyczącego uwierzytelniania formularzy* , a następnie udoskonaloną w samouczku  *<a id="_msoanchor_3">[ ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)</a>konfiguracja uwierzytelniania formularzy i Tematy zaawansowane* . Nasza demonstracyjna aplikacja sieci Web zawiera stronę logowania, która sprawdza poświadczenia użytkowników pod kątem zakodowanych par nazw użytkownika/hasła. Ponadto `Global.asax` zawiera kod, który tworzy niestandardowe obiekty `IPrincipal` i `IIdentity` dla użytkowników uwierzytelnionych. Zaktualizujemy stronę logowania, aby sprawdzić poprawność poświadczeń użytkowników względem struktury członkostwa, a następnie usunąć niestandardowy podmiot zabezpieczeń i logikę tożsamości.

Zacznijmy!

## <a name="the-forms-authentication-and-membership-checklist"></a>Lista kontrolna uwierzytelniania i członkostwa formularzy

Przed rozpoczęciem pracy z platformą członkostwa Poświęć chwilę na zapoznanie się z ważnymi krokami, które zostały podjęte w celu osiągnięcia tego punktu. W przypadku korzystania z platformy członkostwa z `SqlMembershipProvider` w scenariuszu uwierzytelniania opartego na formularzach należy wykonać następujące czynności przed implementacją funkcji członkostwa w aplikacji sieci Web:

1. **Włączanie uwierzytelniania opartego na formularzach.** Zgodnie z opisem w  *<a id="_msoanchor_4">[ ](../introduction/an-overview-of-forms-authentication-vb.md)</a>temacie Omówienie uwierzytelniania formularzy*uwierzytelnianie formularzy jest włączane przez edytowanie `Web.config` i Ustawianie atrybutu `mode` elementu `<authentication>` na `Forms`. Przy włączonej uwierzytelnianiu formularzy każde żądanie przychodzące jest analizowane pod kątem *biletu uwierzytelniania formularzy*, który, jeśli jest obecny, identyfikuje żądającego.
2. **Dodaj schemat usług aplikacji do odpowiedniej bazy danych.** W przypadku korzystania z `SqlMembershipProvider` musimy zainstalować schemat usług aplikacji w bazie danych programu. Zazwyczaj ten schemat jest dodawany do tej samej bazy danych, która zawiera model danych aplikacji. *Tworzenie schematu członkostwa w SQL Server samouczka z użyciem narzędzia `aspnet_regsql.exe` do osiągnięcia tego celu. <a id="_msoanchor_5">[ ](creating-the-membership-schema-in-sql-server-vb.md)</a>*
3. **Dostosuj ustawienia aplikacji sieci Web, aby odwoływać się do bazy danych z kroku 2.** *Tworzenie schematu członkostwa w SQL Server* samouczku przedstawiono dwa sposoby konfigurowania aplikacji sieci Web, aby `SqlMembershipProvider` używała bazy danych wybranej w kroku 2: modyfikując `LocalSqlServer` nazwę parametrów połączenia; lub przez dodanie nowego zarejestrowanego dostawcy do listy dostawców struktury członkostwa i dostosowanie tego nowego dostawcy do korzystania z bazy danych z kroku 2.

Podczas kompilowania aplikacji sieci Web, która używa `SqlMembershipProvider` i uwierzytelniania opartego na formularzach, należy wykonać te trzy kroki przed użyciem klasy `Membership` lub kontrolek sieci Web ASP.NET login. Ponieważ zostały już wykonane te kroki w poprzednich samouczkach, jesteśmy gotowi do rozpoczęcia korzystania z platformy członkostwa!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1. Dodawanie nowych stron ASP.NET

W tym samouczku i następnych trzech będziemy przeanalizować różne funkcje i możliwości związane z członkostwem. Potrzebujemy serii stron ASP.NET, aby zaimplementować tematy zbadane w ramach tych samouczków. Utwórzmy te strony, a następnie plik mapy witryny `(Web.sitemap)`.

Zacznij od utworzenia nowego folderu w projekcie o nazwie `Membership`. Następnie Dodaj pięć nowych stron ASP.NET do folderu `Membership`, łącząc każdą stronę ze stroną wzorcową `Site.master`. Nazwij strony:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

W tym momencie Eksplorator rozwiązań projektu powinien wyglądać podobnie do zrzutu ekranu pokazanego na rysunku 1.

[![pięć nowych stron zostało dodanych do folderu Membership](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Rysunek 1**. Dodano pięć nowych stron do folderu `Membership` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image3.png))

Każda Strona powinna, w tym momencie, mieć dwie kontrolki zawartości, jedną dla każdej strony głównej: `MainContent` i `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Odwołaj się do `LoginContent` domyślnego znaczników elementu ContentPlaceHolder wyświetla link do logowania lub wylogowywania lokacji w zależności od tego, czy użytkownik jest uwierzytelniony. Jednak obecność `Content2` kontrolki zawartości zastępuje domyślną adiustację strony wzorcowej. Zgodnie z opisem w  *<a id="_msoanchor_6">[ ](../introduction/an-overview-of-forms-authentication-vb.md)</a>temacie Omówienie samouczka dotyczącego uwierzytelniania formularzy* jest to przydatne na stronach, w których nie chcesz wyświetlać opcji związanych z logowaniem w lewej kolumnie.

Jednak w przypadku tych pięciu stron chcemy wyświetlić domyślne znaczniki strony głównej dla `LoginContent` ContentPlaceHolder. W związku z tym usuń znaczniki deklaratywne dla kontrolki zawartości `Content2`. Po wykonaniu tej czynności każdy znacznik pięciu stron powinien zawierać tylko jedną kontrolkę zawartości.

## <a name="step-2-creating-the-site-map"></a>Krok 2. Tworzenie mapy witryny

Wszystkie, ale najbardziej proste witryny sieci Web, muszą implementować niektóre formy interfejsu użytkownika nawigacyjnego. Interfejs użytkownika nawigacji może być prostą listą linków do różnych sekcji witryny. Alternatywnie te linki można rozmieścić w menu lub widokach drzewa. Jako deweloperzy stron, Tworzenie interfejsu użytkownika nawigacyjnego jest tylko połowami historii. Potrzebujemy również niektórych środków do zdefiniowania struktury logicznej lokacji w sposób umożliwiający ich konserwację i aktualizację. Po dodaniu nowych stron lub usunięciu istniejących stron chcemy mieć możliwość zaktualizowania jednego źródła — mapy witryny i modyfikacji tych zmian w interfejsie użytkownika nawigacji między lokacjami.

Te dwa zadania — Definiowanie mapy witryny i implementowanie interfejsu użytkownika nawigacyjnego na podstawie mapy witryny — są łatwe w użyciu w ramach struktury mapy witryny i kontrolek sieci Web nawigacji dodanych w programie ASP.NET w wersji 2,0. Platforma mapy witryny pozwala deweloperowi definiować mapę witryny, a następnie uzyskiwać do niej dostęp za pomocą programistycznego interfejsu API ( [klasy`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Wbudowane kontrolki sieci Web nawigacji obejmują [kontrolkę menu](https://msdn.microsoft.com/library/bz09dy46.aspx), [formant TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)i [formant ścieżki mapy witryny](https://msdn.microsoft.com/library/3eafky27.aspx).

Podobnie jak w przypadku struktur członkostwa i ról, struktura mapy witryny jest skompilowana korzystającego [model dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Zadaniem klasy dostawcy mapy witryny jest generowanie struktury w pamięci używanej przez klasę `SiteMap` z trwałego magazynu danych, takiego jak plik XML lub tabela bazy danych. .NET Framework jest dostarczany z domyślnym dostawcą mapy witryny, który odczytuje dane mapy witryny z pliku XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)) i jest to dostawca, który będzie używany w tym samouczku. W przypadku niektórych alternatywnych implementacji dostawcy mapy witryny zapoznaj się z sekcją dalsze odczyty na końcu tego samouczka.

Domyślny dostawca mapy witryny oczekuje poprawnie sformatowanego pliku XML o nazwie `Web.sitemap`, aby istniał katalog główny. Ponieważ korzystamy z tego domyślnego dostawcy, musimy dodać taki plik i zdefiniować strukturę mapy witryny w odpowiednim formacie XML. Aby dodać plik, kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy element. W oknie dialogowym Dodaj plik typu mapa witryny o nazwie `Web.sitemap`.

[![dodać pliku o nazwie Web. sitemap do katalogu głównego projektu](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Rysunek 2**. Dodawanie pliku o nazwie `Web.sitemap` do katalogu głównego projektu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image6.png))

Plik mapy witryny XML definiuje strukturę witryny sieci Web jako hierarchię. Ta relacja hierarchiczna jest modelowana w pliku XML za pośrednictwem pochodzenie elementów `<siteMapNode>`. `Web.sitemap` musi rozpoczynać się od `<siteMap>` węzła nadrzędnego, który ma dokładnie jeden `<siteMapNode>` podrzędny. Ten element najwyższego poziomu `<siteMapNode>` reprezentuje katalog główny hierarchii i może mieć dowolną liczbę węzłów podrzędnych. Każdy element `<siteMapNode>` musi zawierać atrybut `title` i opcjonalnie może zawierać atrybuty `url` i `description` między innymi; Każdy niepusty atrybut `url` musi być unikatowy.

Wprowadź następujący kod XML do pliku `Web.sitemap`:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Powyższy znacznik mapy witryny definiuje hierarchię pokazaną na rysunku 3.

[![mapa witryny reprezentuje hierarchiczną strukturę nawigacyjną](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Rysunek 3**. Mapa witryny reprezentuje hierarchiczną strukturę nawigacyjną ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image9.png))

## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Krok 3. aktualizowanie strony głównej w celu uwzględnienia interfejsu użytkownika nawigacyjnego

ASP.NET zawiera szereg formantów sieci Web związanych z nawigacją w celu projektowania interfejsu użytkownika. Obejmują one menu, TreeView i kontrolki ścieżki mapy witryny. Kontrolki menu i TreeView renderują strukturę mapy witryny odpowiednio do menu lub drzewa, natomiast ścieżki mapy witryny wyświetla pasek nawigacyjny, który pokazuje odwiedzany węzeł, a także jego elementy nadrzędne. Dane mapy lokacji można powiązać z innymi kontrolkami sieci Web danych za pomocą SiteMapDataSource i mogą być dostępne programowo za pośrednictwem klasy `SiteMap`.

Ponieważ szczegółowe omówienie struktury mapy witryny i kontrolek nawigacji wykracza poza zakres tej serii samouczków, a nie poświęcają czasu na korzystanie z własnego interfejsu użytkownika nawigacyjnego, zamiast tego należy zażyczyć ten, który jest używany w *[pracy z danymi w](../../data-access/index.md)* serii samouczków ASP.NET 2,0, który używa kontrolki wzmacniak do wyświetlania listy punktowanej z podwójną listą linków nawigacji, jak pokazano na rysunku 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Dodawanie dwupoziomowej listy łączy w lewej kolumnie

Aby utworzyć ten interfejs, Dodaj następujące znaczniki deklaratywne do lewej kolumny strony wzorcowej `Site.master`, w której tekst do zrobienia: zostanie przemieszczony w menu... obecnie znajduje się.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Powyższe znaczniki wiążą kontrolkę wzmacniak o nazwie `menu` z SiteMapDataSource, która zwraca hierarchię mapy witryny zdefiniowaną w `Web.sitemap`. Ponieważ [właściwość`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) kontrolki SiteMapDataSource jest ustawiona na wartość false, zaczyna zwracać hierarchię mapy witryny, rozpoczynając od elementów potomnych węzła głównego. Wzmacniak wyświetla każdy z tych węzłów (obecnie tylko członkostwo) w `<li>` elementu. Innym, wzmacniak wewnętrzny wyświetla elementy podrzędne bieżącego węzła na zagnieżdżonej liście nieuporządkowanej.

Rysunek 4 przedstawia renderowane dane wyjściowe ze znacznikiem mapy witryny utworzoną w kroku 2. Wzmacniak renderuje znaczniki listy nieuporządkowane, reguły kaskadowego arkusza stylów zdefiniowane w `Styles.css` są odpowiedzialne za układ estetycznie-atrakcyjne. Aby uzyskać bardziej szczegółowy opis sposobu działania powyższych znaczników, zapoznaj się z samouczkiem [strony wzorcowe i nawigacja w witrynie](https://asp.net/learn/data-access/tutorial-03-vb.aspx) .

[![nawigacyjny interfejs użytkownika jest renderowany przy użyciu zagnieżdżonych list nieuporządkowanych](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Ilustracja 4**. interfejs użytkownika nawigacyjnego jest renderowany przy użyciu zagnieżdżonych list nieuporządkowanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image12.png))

### <a name="adding-breadcrumb-navigation"></a>Dodawanie nawigacji nawigacyjnej

Poza listą linków w lewej kolumnie, przyjrzyjmy się także stronom [nawigacyjnym](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Pasek nawigacyjny jest elementem interfejsu użytkownika nawigacji, który szybko pokazuje użytkowników ich bieżącej pozycji w hierarchii lokacji. Kontrolka ścieżki mapy witryny korzysta z struktury mapy witryny w celu określenia lokalizacji bieżącej strony w mapie witryny, a następnie wyświetlenie stron do odłączenia na podstawie tych informacji.

W celu dodania elementu `<span>` do nagłówka strony wzorcowej `<div>` elementu i Ustaw nowy atrybut `class` elementu `<span>` do stron nadrzędnych. (Klasa `Styles.css` zawiera regułę dla klasy do stron nadrzędnych). Następnie Dodaj ścieżki mapy witryny do tego nowego elementu `<span>`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Rysunek 5 przedstawia dane wyjściowe ścieżki mapy witryny podczas odwiedzania `~/Membership/CreatingUserAccounts.aspx`.

[![pasek nawigacyjny wyświetla bieżącą stronę i jej elementy nadrzędne na mapie witryny](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Rysunek 5**. pasek nawigacyjny wyświetla bieżącą stronę i jej elementy nadrzędne na mapie witryny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image15.png))

## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Krok 4. Usuwanie niestandardowego podmiotu zabezpieczeń i logiki tożsamości

W samouczku  *<a id="_msoanchor_7">[ ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)</a>konfiguracja uwierzytelniania formularzy i Tematy zaawansowane* przedstawiono sposób kojarzenia niestandardowych obiektów Principal i Identity z uwierzytelnionym użytkownikiem. W tym celu należy utworzyć procedurę obsługi zdarzeń w `Global.asax` dla zdarzenia `PostAuthenticateRequest` aplikacji uruchamianego po pomyślnym uwierzytelnieniu przez `FormsAuthenticationModule` użytkownika. W tym obsłudze zdarzeń zamieniono `GenericPrincipal` i `FormsIdentity` obiekty dodane przez `FormsAuthenticationModule` z `CustomPrincipal` i `CustomIdentity` obiektów utworzonych w tym samouczku.

Chociaż niestandardowe obiekty Principal i Identity są przydatne w niektórych scenariuszach, w większości przypadków `GenericPrincipal` i `FormsIdentity` obiekty są wystarczające. W związku z tym uważam, że wartościowa powróci do zachowania domyślnego. Wprowadź tę zmianę, usuwając lub dodając komentarz do programu obsługi zdarzeń `PostAuthenticateRequest` lub usuwając plik `Global.asax` całkowicie.

> [!NOTE]
> Po dodaniu lub usunięciu kodu w `Global.asax`należy dodać komentarz do kodu w `Default.aspx's` klasie związanej z kodem, która rzutuje Właściwość `User.Identity` na wystąpienie `CustomIdentity`.

## <a name="step-5-programmatically-creating-a-new-user"></a>Krok 5. programowe tworzenie nowego użytkownika

Aby utworzyć nowe konto użytkownika za pomocą platformy członkostwa, użyj [metody`CreateUser`](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)klasy `Membership`. Ta metoda zawiera parametry wejściowe dla nazwy użytkownika, hasła i innych pól związanych z użytkownikiem. W wywołaniu zostaje delegowane utworzenie nowego konta użytkownika do skonfigurowanego dostawcy członkostwa, a następnie zwraca [obiekt`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) reprezentujący właśnie utworzone konto użytkownika.

Metoda `CreateUser` ma cztery przeciążenia, a każdy z nich akceptuje inną liczbę parametrów wejściowych:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Te cztery przeciążenia różnią się od ilości zbieranych informacji. Pierwsze Przeciążenie, na przykład, wymaga tylko nazwy użytkownika i hasła dla nowego konta użytkownika, a drugi także wymaga adresu e-mail użytkownika.

Te przeciążenia istnieją, ponieważ informacje konieczne do utworzenia nowego konta użytkownika zależą od ustawień konfiguracji dostawcy członkostwa. W samouczku  *<a id="_msoanchor_8">[ ](creating-the-membership-schema-in-sql-server-vb.md)</a>Tworzenie schematu członkostwa w programie SQL Server* sprawdzono Określanie ustawień konfiguracji dostawcy członkostwa w programie `Web.config`. Tabela 2 zawiera pełną listę ustawień konfiguracji.

Jednym z tych ustawień konfiguracji dostawcy członkostwa, które mają wpływ na to, jakie przeciążenia `CreateUser` mogą być używane są ustawienia `requiresQuestionAndAnswer`. Jeśli `requiresQuestionAndAnswer` jest ustawiona na `true` (domyślnie), a następnie podczas tworzenia nowego konta użytkownika należy określić pytanie zabezpieczające i odpowiedź. Te informacje są później używane, jeśli użytkownik musi zresetować lub zmienić hasło. W tym czasie są wyświetlane pytania zabezpieczające i muszą one wprowadzić poprawną odpowiedź w celu zresetowania lub zmiany hasła. W związku z tym, jeśli `requiresQuestionAndAnswer` jest ustawiona na `true`, wywołanie jednego z dwóch pierwszych przeciążeń `CreateUser` powoduje wyjątek z powodu braku pytania zabezpieczającego i odpowiedzi. Ponieważ nasza aplikacja jest obecnie skonfigurowana do wymagania zabezpieczeń i odpowiedzi, należy użyć jednego z dwóch ostatnich przeciążeń podczas programistycznego tworzenia użytkownika.

Aby zilustrować przy użyciu metody `CreateUser`, Utwórzmy interfejs użytkownika, w którym zostanie wyświetlony monit o podanie nazwy, hasła, wiadomości e-mail i odpowiedzi na wstępnie zdefiniowane pytanie zabezpieczające. Otwórz stronę `CreatingUserAccounts.aspx` w folderze `Membership` i Dodaj następujące kontrolki sieci Web do kontrolki zawartość:

- Pole tekstowe o nazwie `Username`
- Pole tekstowe o nazwie `Password`, którego właściwość `TextMode` jest ustawiona na `Password`
- Pole tekstowe o nazwie `Email`
- Etykieta o nazwie `SecurityQuestion` z wyczyszczoną właściwością `Text`
- Pole tekstowe o nazwie `SecurityAnswer`
- Przycisk o nazwie `CreateAccountButton` którego właściwość `Text` została ustawiona do tworzenia konta użytkownika
- Kontrolka etykiety o nazwie `CreateAccountResults` z wyczyszczoną właściwością `Text`

W tym momencie ekran powinien wyglądać podobnie do zrzutu ekranu pokazanego na rysunku 6.

[![dodać różne kontrolki sieci Web do strony CreatingUserAccounts. aspx](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Ilustracja 6**. Dodawanie różnych kontrolek sieci Web do `CreatingUserAccounts.aspx Page` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image18.png))

Etykieta `SecurityQuestion` i pole tekstowe `SecurityAnswer` są przeznaczone do wyświetlania wstępnie zdefiniowanego pytania zabezpieczającego i zbierania odpowiedzi użytkownika. Należy zauważyć, że zarówno pytanie zabezpieczeń, jak i odpowiedź są przechowywane w zależności od użytkownika, więc można zezwolić każdemu użytkownikowi na Definiowanie własnych pytań zabezpieczających. Jednak na potrzeby tego przykładu postanowiono użyć uniwersalnego pytania zabezpieczającego, czyli: jaki jest Twój ulubiony kolor?

Aby zaimplementować to wstępnie zdefiniowane pytanie zabezpieczające, należy dodać stałą do klasy powiązanej z kodem strony o nazwie `passwordQuestion`, przypisując jej pytanie zabezpieczające. Następnie w obsłudze zdarzeń `Page_Load` Przypisz tę stałą do właściwości `Text` etykiety `SecurityQuestion`:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `CreateAccountButton'` s `Click` i Dodaj następujący kod:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

Procedura obsługi zdarzeń `Click` uruchamiana przez zdefiniowanie zmiennej o nazwie `createStatus` typu [`MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` to Wyliczenie wskazujące stan operacji `CreateUser`. Na przykład jeśli konto użytkownika zostanie utworzone pomyślnie, wystąpienie `MembershipCreateStatus` wystąpienia zostanie ustawione na wartość `Success;` z drugiej strony, jeśli operacja nie powiedzie się, ponieważ istnieje już użytkownik o tej samej nazwie użytkownika, zostanie ona ustawiona na wartość `DuplicateUserName`. W `CreateUser` przeciążenia, które używamy, musimy przekazać wystąpienie `MembershipCreateStatus` do metody. Ten parametr jest ustawiony na odpowiednią wartość w metodzie `CreateUser` i można sprawdzić jego wartość po wywołaniu metody, aby określić, czy konto użytkownika zostało pomyślnie utworzone.

Po wywołaniu `CreateUser`, przekazanie `createStatus`, instrukcja `Select Case` jest używana do wyprowadzania odpowiedniego komunikatu w zależności od wartości przypisanej do `createStatus`. Ilustracje 7 przedstawiają dane wyjściowe w przypadku pomyślnego utworzenia nowego użytkownika. Rysunki 8 i 9 przedstawiają dane wyjściowe, jeśli konto użytkownika nie zostało utworzone. Na rysunku nr 8 osoba odwiedzająca wprowadziła hasło z pięcioma literami, które nie spełnia wymagań siły hasła wpisanych w ustawieniach konfiguracji dostawcy członkostwa. Na rysunku nr 9 osoba odwiedzająca próbuje utworzyć konto użytkownika przy użyciu istniejącej nazwy użytkownika (utworzonej na rysunku 7).

[![pomyślnie utworzono nowe konto użytkownika](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Rysunek 7**: pomyślnie utworzono nowe konto użytkownika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image21.png))

[![konto użytkownika nie zostanie utworzone, ponieważ podane hasło jest zbyt słabe](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Ilustracja 8**. konto użytkownika nie zostało utworzone, ponieważ podane hasło jest zbyt słabe ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image24.png))

[![konto użytkownika nie zostanie utworzone, ponieważ nazwa użytkownika jest już używana](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Rysunek 9**: konto użytkownika nie zostało utworzone, ponieważ nazwa użytkownika jest już używana ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image27.png))

> [!NOTE]
> Być może zastanawiasz się, jak określić powodzenie lub niepowodzenie podczas korzystania z jednego z dwóch pierwszych `CreateUser` przeciążeń metod, żadna z nich nie ma parametru typu `MembershipCreateStatus`. Te pierwsze dwa przeciążenia zgłaszają [wyjątek`MembershipCreateUserException`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) w wyniku awarii, który zawiera [właściwość`StatusCode`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) typu `MembershipCreateStatus`.

Po utworzeniu kilku kont użytkowników Sprawdź, czy konta zostały utworzone przez wystawienie zawartości `aspnet_Users` i `aspnet_Membership` tabel w bazie danych `SecurityTutorials.mdf`. Jak pokazano na rysunku 10, dodaliśmy dwóch użytkowników za pośrednictwem strony `CreatingUserAccounts.aspx`: Tito i Bruce.

[![w magazynie użytkowników członkostwa są dwaj użytkownicy: Tito i Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Rysunek 10**. w magazynie użytkowników członkostwa są dwaj użytkownicy: Tito i Bruce ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image30.png))

Magazyn użytkowników członkostwa zawiera teraz informacje o kontach Bruce i Tito, ale zaimplementowano jeszcze funkcję umożliwiającą Bruce lub Tito logowanie się do lokacji. Obecnie `Login.aspx` sprawdza poprawność poświadczeń użytkownika względem zakodowanego zestawu par nazw użytkownika/hasła — *nie sprawdza poprawności* podanych poświadczeń względem struktury członkostwa. W przypadku wyświetlenia nowych kont użytkowników w `aspnet_Users`, a tabele `aspnet_Membership` będą wystarczające. W następnym samouczku  *<a id="_msoanchor_9">[ ](validating-user-credentials-against-the-membership-user-store-vb.md)</a>sprawdzanie poprawności poświadczeń użytkownika względem magazynu użytkowników członkostwa*spowoduje zaktualizowanie strony logowania w celu weryfikacji względem magazynu członkostwa.

> [!NOTE]
> Jeśli nie widzisz żadnych użytkowników w bazie danych `SecurityTutorials.mdf`, może to być spowodowane tym, że aplikacja sieci Web używa domyślnego dostawcy członkostwa, `AspNetSqlMembershipProvider`, który korzysta z bazy danych `ASPNETDB.mdf` jako magazynu użytkownika. Aby określić, czy jest to problem, kliknij przycisk Odśwież w Eksplorator rozwiązań. Jeśli baza danych o nazwie `ASPNETDB.mdf` została dodana do folderu `App_Data`, jest to problem. Wróć do kroku 4  *<a id="_msoanchor_10">[ ](creating-the-membership-schema-in-sql-server-vb.md)</a>tworzenia schematu członkostwa w SQL Server* samouczku, aby uzyskać instrukcje dotyczące prawidłowego konfigurowania dostawcy członkostwa.

W większości przypadków tworzenia scenariuszy kont użytkowników osoba odwiedzająca jest prezentowana z interfejsem, aby wprowadzić nazwę użytkownika, hasło, adres e-mail i inne istotne informacje. w tym momencie zostanie utworzone nowe konto. W tym kroku zawarto informacje na temat tworzenia takiego interfejsu za pomocą metody `Membership.CreateUser`, aby programowo dodać nowe konto użytkownika na podstawie danych wejściowych użytkownika. Nasz kod, ale właśnie utworzył nowe konto użytkownika. Nie wykonano żadnych akcji uzupełniania, takich jak logowanie użytkownika do witryny w ramach właśnie utworzonego konta użytkownika lub wysyłanie potwierdzenia wiadomości e-mail do użytkownika. Te dodatkowe kroki wymagają dodatkowego kodu w obsłudze zdarzeń `Click` przycisku.

ASP.NET jest dostarczany z kontrolką formancie CreateUserWizard, która jest przeznaczona do obsługi procesu tworzenia konta użytkownika, od renderowania interfejsu użytkownika na potrzeby tworzenia nowych kont użytkowników, do tworzenia konta w środowisku członkostwa i wykonywania konta zadania tworzenia, takie jak wysyłanie wiadomości e-mail z potwierdzeniem i rejestrowanie samego użytkownika utworzonego w lokacji. Użycie formantu formancie CreateUserWizard jest proste, ponieważ przeciągając kontrolkę formancie CreateUserWizard z przybornika na stronę, a następnie ustawiając kilka właściwości. W większości przypadków nie trzeba pisać jednego wiersza kodu. Ta kontrolka Nifty zostanie szczegółowo omówione w kroku 6.

Jeśli nowe konta użytkowników są tworzone tylko za pośrednictwem typowej strony sieci Web tworzenia konta, prawdopodobnie trzeba będzie napisać kod, który używa metody `CreateUser`, ponieważ formant formancie CreateUserWizard będzie prawdopodobnie spełniał Twoje potrzeby. Jednak Metoda `CreateUser` jest użyteczna w scenariuszach, w których potrzebne jest wysoce dostosowane środowisko użytkownika tworzenia konta lub kiedy trzeba programistycznie tworzyć nowe konta użytkowników za pomocą alternatywnego interfejsu. Na przykład może istnieć strona, która umożliwia użytkownikowi przekazywanie pliku XML, który zawiera informacje o użytkowniku z innej aplikacji. Strona może analizować zawartość przekazanego pliku XML i utworzyć nowe konto dla każdego użytkownika reprezentowanego w kodzie XML przez wywołanie metody `CreateUser`.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Krok 6. Tworzenie nowego użytkownika za pomocą kontrolki formancie CreateUserWizard

ASP.NET jest dostarczany z liczbą kontrolek sieci Web logowania. Te kontrolki mają na celu pomoc w wielu typowych scenariuszach dotyczących kont użytkowników i związanych z logowaniem. [Formant formancie CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) to jedna taka kontrolka, która została zaprojektowana w celu zaprezentowania interfejsu użytkownika w celu dodania nowego konta użytkownika do struktury członkostwa.

Podobnie jak w przypadku wielu innych kontrolek sieci Web związanych z logowaniem, formancie CreateUserWizard można używać bez konieczności pisania jednego wiersza kodu. Intuicyjnie udostępnia interfejs użytkownika na podstawie ustawień konfiguracyjnych dostawcy członkostwa i wewnętrznie wywołuje metodę `CreateUser` klasy `Membership` po wprowadzeniu niezbędnych informacji przez użytkownika i kliknięciu przycisku Utwórz użytkownika. Kontrolka formancie CreateUserWizard jest niezwykle dostosowywalna. Istnieje Host zdarzeń, które są uruchamiane na różnych etapach procesu tworzenia konta. W razie potrzeby możemy utworzyć programy obsługi zdarzeń w celu dodania logiki niestandardowej do przepływu pracy tworzenia konta. Co więcej, wygląd formancie CreateUserWizard jest bardzo elastyczny. Istnieje wiele właściwości, które definiują wygląd interfejsu domyślnego; w razie potrzeby formant może zostać przekonwertowany na szablon lub dodatkowe kroki rejestracji użytkownika mogą zostać dodane.

Zacznijmy od przyjrzeć się domyślnym interfejsem i zachowaniem formantu formancie CreateUserWizard. Następnie będziemy dowiedzieć się, jak dostosować wygląd za pomocą właściwości i zdarzeń kontrolki.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Badanie domyślnego interfejsu i zachowań formancie CreateUserWizard

Wróć do strony `CreatingUserAccounts.aspx` w folderze `Membership`, przełącz się do trybu projektowania lub podziału, a następnie Dodaj kontrolkę formancie CreateUserWizard w górnej części strony. Kontrolka formancie CreateUserWizard jest zarejestrowana w sekcji kontrolki logowania przybornika. Po dodaniu kontrolki ustaw jej Właściwość `ID` na `RegisterUser`. Ponieważ zrzut ekranu na rysunku 11 pokazuje, formancie CreateUserWizard renderuje interfejs przy użyciu pól tekstowych dla nowego użytkownika, hasła, adresu e-mail i pytania zabezpieczającego.

[![formant formancie CreateUserWizard renderuje ogólny interfejs użytkownika tworzenia](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Ilustracja 11**. kontrolka formancie CreateUserWizard renderuje ogólny interfejs użytkownika tworzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image33.png))

Poświęć chwilę na porównanie domyślnego interfejsu użytkownika wygenerowanego przez formant formancie CreateUserWizard z interfejsem, który został utworzony w kroku 5. W przypadku osób uruchamiających formant formancie CreateUserWizard umożliwia osobie odwiedzającej określenie pytania zabezpieczeń i odpowiedzi, natomiast nasz ręcznie utworzony interfejs użył wstępnie zdefiniowanego pytania zabezpieczającego. Interfejs formantu formancie CreateUserWizard również zawiera kontrolki walidacji, podczas gdy nie zaimplementowano jeszcze walidacji pól formularza interfejsu. Interfejs formancie CreateUserWizard Control zawiera pole tekstowe Potwierdź hasło (wraz z CompareValidator, aby upewnić się, że tekst wprowadzony hasłem i pola tekstowe porównania hasła są równe).

Interesujący jest sposób, w jaki formant formancie CreateUserWizard sprawdza ustawienia konfiguracji dostawcy członkostwa podczas renderowania interfejsu użytkownika. Na przykład pola tekstowe pytania zabezpieczeń i odpowiedzi są wyświetlane tylko wtedy, gdy `requiresQuestionAndAnswer` ma wartość true. Podobnie formancie CreateUserWizard automatycznie dodaje kontrolkę RegularExpressionValidator, aby upewnić się, że wymagania dotyczące siły hasła zostały spełnione i ustawi `ErrorMessage` i `ValidationExpression` właściwości na podstawie ustawień konfiguracji `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`i `passwordStrengthRegularExpression`.

Formant formancie CreateUserWizard, jak jego nazwa oznacza, pochodzi od [kontrolki kreatora](https://msdn.microsoft.com/library/s2etd1ek.aspx). Formanty kreatora zostały zaprojektowane w celu zapewnienia interfejsu do wykonywania zadań wieloetapowych. Kontrolka kreatora może mieć dowolną liczbę `WizardSteps`, z których każdy jest szablonem, który definiuje kontrolki HTML i sieci Web dla tego kroku. Kontrolka kreatora początkowo wyświetla pierwszy `WizardStep`, wraz z kontrolkami nawigacji, które umożliwiają użytkownikowi przechodzenie z jednego kroku do następnego lub powrót do poprzednich kroków.

Jak znaczniki deklaratywne na rysunku 11 pokazują, domyślnym interfejsem formantu formancie CreateUserWizard jest dwa `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? renderuje interfejs w celu zebrania informacji dotyczących tworzenia nowego konta użytkownika. Jest to krok przedstawiony na rysunku 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? renderuje komunikat informujący o pomyślnym utworzeniu konta.

Wygląd i zachowanie formancie CreateUserWizard można modyfikować, konwertując jeden z tych kroków na szablony lub dodając własne `WizardStep` s. Dowiesz się, jak dodać `WizardStep` do interfejsu rejestracji w samouczku *przechowywanie dodatkowych informacji o użytkownikach* .

Zobaczmy, jak działa kontrolka formancie CreateUserWizard. Odwiedź stronę `CreatingUserAccounts.aspx` za pomocą przeglądarki. Zacznij od wprowadzenia nieprawidłowych wartości do interfejsu formancie CreateUserWizard. Spróbuj wprowadzić hasło, które nie jest zgodne z wymaganiami dotyczącymi siły hasła, lub pozostawienie pustej nazwy użytkownika pole tekstowe. W formancie CreateUserWizard zostanie wyświetlony odpowiedni komunikat o błędzie. Rysunek 12 przedstawia dane wyjściowe podczas próby utworzenia użytkownika z niewystarczającym silnym hasłem.

[![formancie CreateUserWizard automatycznie wprowadza kontrolki walidacji](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Ilustracja 12**. formancie CreateUserWizard automatycznie wprowadza kontrolki walidacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image36.png))

Następnie wprowadź odpowiednie wartości w formancie CreateUserWizard, a następnie kliknij przycisk Utwórz użytkownika. Przy założeniu, że wymagane pola zostały wprowadzone, a siła hasła jest wystarczająca, formancie CreateUserWizard utworzy nowe konto użytkownika za pomocą struktury członkostwa, a następnie wyświetli interfejs `CompleteWizardStep`(Zobacz Rysunek 13). W tle formancie CreateUserWizard wywołuje metodę `Membership.CreateUser`, podobnie jak w kroku 5.

[![pomyślnie utworzono nowe konto użytkownika](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Rysunek 13**: nowe konto użytkownika zostało pomyślnie utworzone ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image39.png))

> [!NOTE]
> Jak pokazano na rysunku 13, interfejs `CompleteWizardStep`zawiera przycisk Kontynuuj. Jednak po kliknięciu tej opcji po prostu wykonuje ogłaszanie zwrotne, pozostawiając odwiedzanie na tej samej stronie. W sekcji Dostosowywanie wyglądu i zachowania formancie CreateUserWizard za pomocą jego właściwości zobaczymy, jak możesz mieć ten przycisk wysłać gościa do `Default.aspx` (lub innej strony).

Po utworzeniu nowego konta użytkownika Wróć do programu Visual Studio i sprawdź, czy `aspnet_Users` i `aspnet_Membership` tabele takie jak na rysunku nr 10, aby sprawdzić, czy konto zostało pomyślnie utworzone.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Dostosowywanie zachowania i wyglądu formancie CreateUserWizard za pomocą jego właściwości

Formancie CreateUserWizard można dostosować na różne sposoby, za pomocą właściwości, `WizardStep` s i obsługi zdarzeń. W tej sekcji zawarto informacje na temat sposobu dostosowywania wyglądu kontrolki za pomocą jej właściwości. Następna sekcja sprawdza, rozszerzając zachowanie kontrolki za pomocą programów obsługi zdarzeń.

Praktycznie cały tekst wyświetlany w domyślnym interfejsie użytkownika formantu formancie CreateUserWizard można dostosować za pomocą jego mnóstwo właściwości. Na przykład etykiety Nazwa użytkownika, hasło, Potwierdź hasło, wiadomość E-mail, pytanie zabezpieczające i zabezpieczenia odpowiedzi są wyświetlane po lewej stronie pól tekstowych można dostosować odpowiednio do właściwości [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [`ConfirmPasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [`EmailLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [`QuestionLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)i [`AnswerLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) . Podobnie są właściwości do określania tekstu dla przycisków Utwórz użytkownika i Kontynuuj w `CreateUserWizardStep` i `CompleteWizardStep`, a także jeśli te przyciski są renderowane jako przyciski, LinkButtons lub ImageButtons.

Kolory, obramowania, czcionki i inne elementy wizualne można konfigurować za pomocą hosta właściwości stylu. Sama kontrolka formancie CreateUserWizard ma wspólną Właściwość stylu formantu sieci Web — `BackColor`, `BorderStyle`, `CssClass`, `Font`i tak dalej-i istnieje wiele właściwości stylu służących do definiowania wyglądu określonych sekcji interfejsu formancie CreateUserWizard. [Właściwość`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), na przykład definiuje style dla pól tekstowych w `CreateUserWizardStep`, podczas gdy [Właściwość`TitleTextStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definiuje styl tytułu (Utwórz nowe konto).

Oprócz właściwości związanych z wyglądem istnieje wiele właściwości, które wpływają na zachowanie formantu formancie CreateUserWizard. [Właściwość`DisplayCancelButton`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), jeśli ma wartość true, wyświetla przycisk Anuluj obok przycisku Utwórz użytkownika (wartość domyślna to false). Jeśli zostanie wyświetlony przycisk Anuluj, należy również ustawić [właściwość`CancelDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), która określa stronę, do której użytkownik jest wysyłany po kliknięciu przycisku Anuluj. Jak wskazano w poprzedniej sekcji, przycisk Kontynuuj w interfejsie `CompleteWizardStep`powoduje ogłoszenie zwrotne, ale pozostawia odwiedzanie na tej samej stronie. Aby wysłać gościa do innej strony po kliknięciu przycisku Kontynuuj, wystarczy określić adres URL we [właściwości`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Zaktualizujmy `RegisterUser` kontrolkę formancie CreateUserWizard, aby wyświetlić przycisk Anuluj i aby wysłać odwiedzanie do `Default.aspx` po kliknięciu przycisków Anuluj lub Kontynuuj. Aby to osiągnąć, należy ustawić właściwość `DisplayCancelButton` na true i obie właściwości `CancelDestinationPageUrl` i `ContinueDestinationPageUrl` na wartość ~/default.aspx. Ilustracja 14 przedstawia zaktualizowany formancie CreateUserWizard, gdy jest wyświetlany za pomocą przeglądarki.

[![Element CreateUserWizardStep zawiera przycisk Anuluj](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Ilustracja 14**. `CreateUserWizardStep` zawiera przycisk Cancel ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-user-accounts-vb/_static/image42.png))

Gdy osoba odwiedzająca wprowadzi nazwę użytkownika, hasło, adres e-mail i pytanie zabezpieczające, a następnie kliknie przycisk Utwórz użytkownika, zostanie utworzone nowe konto użytkownika i użytkownik zostanie zalogowany jako nowo utworzony użytkownik. Przy założeniu, że osoba odwiedzająca stronę tworzy nowe konto dla siebie, prawdopodobnie jest to żądane zachowanie. Można jednak zezwolić administratorom na dodawanie nowych kont użytkowników. W takim przypadku konto użytkownika zostanie utworzone, ale administrator pozostanie zalogowany jako administrator (a nie jako nowo utworzone konto). To zachowanie można zmodyfikować za pomocą [właściwości`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)logicznej.

Konta użytkowników w środowisku członkostwa zawierają zatwierdzoną flagę; Użytkownicy, którzy nie są zatwierdzeni, nie mogą zalogować się do witryny. Domyślnie nowo utworzone konto jest oznaczone jako zatwierdzone, co umożliwia użytkownikowi natychmiastowe logowanie do witryny. Istnieje jednak możliwość, że nowe konta użytkowników są oznaczone jako niezatwierdzone. Być może Administrator ma ręcznie zatwierdzać nowych użytkowników przed ich zalogowaniem się; Możesz też sprawdzić, czy adres e-mail wprowadzony podczas rejestracji jest prawidłowy przed zezwoleniem użytkownikowi na zalogowanie się. Niezależnie od tego, czy nowo utworzone konto użytkownika może być oznaczone jako niezatwierdzone, ustawiając [właściwość`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) kontrolki formancie CreateUserWizard na true (wartość domyślna to false).

Inne właściwości powiązane z zachowaniem uwagi obejmują `AutoGeneratePassword` i `MailDefinition`. Jeśli [właściwość`AutoGeneratePassword`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) ma wartość True, `CreateUserWizardStep` nie wyświetla hasła i potwierdzania hasła. Zamiast tego nowo utworzone hasło użytkownika jest generowane automatycznie przy użyciu [metody`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)klasy `Membership`. Metoda `GeneratePassword` konstruuje hasło o określonej długości i wystarczającej liczbie znaków innych niż alfanumeryczne w celu spełnienia skonfigurowanych wymagań dotyczących siły hasła.

[Właściwość`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) jest przydatna, jeśli chcesz wysłać wiadomość e-mail na adres e-mail określony podczas procesu tworzenia konta. Właściwość `MailDefinition` zawiera serię podwłaściwości służącą do definiowania informacji o konstruowanej wiadomości e-mail. Właściwości te obejmują opcje takie jak `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`i `BodyFileName`. [Właściwość`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) wskazuje plik tekstowy lub HTML, który zawiera treść wiadomości e-mail. Treść obsługuje dwa wstępnie zdefiniowane symbole zastępcze: `<%UserName%>` i `<%Password%>`. Te symbole zastępcze, jeśli znajdują się w pliku `BodyFileName`, zostaną zastąpione nazwą i hasłem utworzonego przez samego użytkownika.

> [!NOTE]
> Właściwość `MailDefinition` kontrolki `CreateUserWizard` po prostu określa szczegóły dotyczące wiadomości e-mail wysyłanej po utworzeniu nowego konta. Nie zawiera żadnych szczegółowych informacji o tym, w jaki sposób wiadomość e-mail jest wysyłana (czyli czy jest używany serwer SMTP lub katalog poczty drop mail, wszystkie informacje o uwierzytelnianiu itd.). Te szczegóły niskiego poziomu należy zdefiniować w sekcji `<system.net>` w `Web.config`. Aby uzyskać więcej informacji na temat tych ustawień konfiguracji oraz wysyłania wiadomości e-mail z usługi ASP.NET 2,0 ogólniej, zobacz [często zadawane pytania dotyczące SystemNetMail.com](http://www.systemnetmail.com/) i mojego artykułu, [wysyłając wiadomość e-mail w ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Rozszerzanie zachowania formancie CreateUserWizard przy użyciu procedur obsługi zdarzeń

Kontrolka formancie CreateUserWizard wywołuje wiele zdarzeń podczas przepływu pracy. Na przykład gdy odwiedzający wprowadzi swoją nazwę użytkownika, hasło i inne odpowiednie informacje, a następnie kliknie przycisk Utwórz użytkownika, formant formancie CreateUserWizard wywołuje jego [zdarzenie`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Jeśli wystąpi problem podczas procesu tworzenia, zostanie wyzwolone [zdarzenie`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) . Jeśli jednak użytkownik zostanie utworzony pomyślnie, zostanie zgłoszone [zdarzenie`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) . Istnieją dodatkowe zdarzenia kontroli formancie CreateUserWizard, które są wywoływane, ale są to trzy najbardziej niemieckie.

W niektórych scenariuszach możemy chcieć nacisnąć przepływ pracy formancie CreateUserWizard, co możemy zrobić, tworząc procedurę obsługi zdarzeń dla odpowiedniego zdarzenia. Aby to zilustrować, przyjrzyjmy się `RegisterUser` kontrolce formancie CreateUserWizard w celu uwzględnienia niestandardowego sprawdzania poprawności nazwy użytkownika i hasła. W szczególności ulepszamy nasze formancie CreateUserWizard, dzięki czemu nazwy użytkowników nie mogą zawierać spacji wiodących ani końcowych, a nazwa użytkownika nie może pojawić się w dowolnym miejscu hasła. W skrócie chcemy uniemożliwić komuś utworzenie nazwy użytkownika, takiej jak "Scott", lub posiadania kombinacji nazwy użytkownika/hasła, takich jak Scott i Scott. 1234.

Aby to osiągnąć, utworzymy procedurę obsługi zdarzeń dla zdarzenia `CreatingUser`, aby wykonać nasze dodatkowe sprawdzenia poprawności. Jeśli podane dane nie są prawidłowe, należy anulować proces tworzenia. Należy również dodać kontrolkę sieci Web etykieta do strony, aby wyświetlić komunikat z wyjaśnieniem, że nazwa użytkownika lub hasło jest nieprawidłowe. Zacznij od dodania kontrolki etykieta pod kontrolką formancie CreateUserWizard, ustawiając jej Właściwość `ID` na `InvalidUserNameOrPasswordMessage` i jej Właściwość `ForeColor` na `Red`. Wyczyść Właściwość `Text` i ustaw dla jej właściwości `EnableViewState` i `Visible` wartość false.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `CreatingUser` formantu formancie CreateUserWizard. Aby utworzyć procedurę obsługi zdarzeń, zaznacz kontrolkę w projektancie, a następnie przejdź do okno Właściwości. W tym miejscu kliknij ikonę błyskawicy, a następnie kliknij dwukrotnie odpowiednie zdarzenie, aby utworzyć procedurę obsługi zdarzeń.

Dodaj następujący kod do programu obsługi zdarzeń `CreatingUser`:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Należy pamiętać, że nazwa użytkownika i hasło wprowadzone w kontrolce formancie CreateUserWizard są dostępne odpowiednio [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) i [`Password` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx). Te właściwości są używane w powyższej procedurze obsługi zdarzeń, aby określić, czy podana nazwa użytkownika zawiera spacje wiodące, czy końcowe oraz czy nazwa użytkownika znajduje się w haśle. Jeśli spełniony jest dowolny z tych warunków, zostanie wyświetlony komunikat o błędzie w etykiecie `InvalidUserNameOrPasswordMessage` i Właściwość `e.Cancel` programu obsługi zdarzeń jest ustawiona na `True`. Jeśli `e.Cancel` jest ustawiony na `True`, formancie CreateUserWizard krótkiego obwodu przepływu pracy, co skutecznie anuluje proces tworzenia konta użytkownika.

Rysunek 15 przedstawia zrzut ekranu `CreatingUserAccounts.aspx`, gdy użytkownik wprowadza nazwę użytkownika z spacjami wiodącymi.

[Nazwy użytkowników ![z wiodącymi lub końcowymi spacjami są niedozwolone](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Ilustracja 15**. nazwy użytkowników z spacjami wiodącymi lub końcowymi są niedozwolone ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-user-accounts-vb/_static/image45.png))

> [!NOTE]
> Zobaczymy przykład użycia zdarzenia `CreatedUser` formantu formancie CreateUserWizard w samouczku  *<a id="_msoanchor_11">[ ](storing-additional-user-information-vb.md)</a>przechowywanie dodatkowych informacji o użytkowniku* .

## <a name="summary"></a>Podsumowanie

Metoda `CreateUser` klasy `Membership` tworzy nowe konto użytkownika w środowisku członkostwa. Jest to możliwe dzięki delegowaniu wywołania do skonfigurowanego dostawcy członkostwa. W przypadku `SqlMembershipProvider`Metoda `CreateUser` dodaje rekord do tabel `aspnet_Users` i `aspnet_Membership` bazy danych.

Chociaż nowe konta użytkowników mogą być tworzone programowo (jak zostało to opisane w kroku 5), szybszym i łatwiejszym rozwiązaniem jest użycie formantu formancie CreateUserWizard. Ta kontrolka renderuje interfejs użytkownika wieloetapowego służący do zbierania informacji o użytkownikach i tworzenia nowego użytkownika w środowisku członkostwa. Na podstawie postanowień ta kontrolka używa tej samej metody `Membership.CreateUser`, jak to zostało zbadane w kroku 5, ale kontrolka tworzy interfejs użytkownika, kontrolki walidacji i reaguje na błędy tworzenia konta użytkownika bez konieczności pisania Lick kodu.

W tym momencie mamy wbudowaną funkcję tworzenia nowych kont użytkowników. Jednak strona logowania nadal jest sprawdzana pod kątem tych zakodowanych poświadczeń, które zostały określone z powrotem w drugim samouczku. <a id="_msoanchor_12"> </a>W [następnym samouczku](validating-user-credentials-against-the-membership-user-store-vb.md) będziemy aktualizować `Login.aspx` w celu zweryfikowania podanych poświadczeń użytkownika względem struktury członkostwa.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [`CreateUser` dokumentacja techniczna](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Formancie CreateUserWizard — informacje o formancie](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Tworzenie dostawcy mapy witryny opartego na systemie plików](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Tworzenie interfejsu użytkownika krok po kroku przy użyciu kontrolki kreatora ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Badanie nawigacji w witrynie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Strony wzorcowe i nawigacja w witrynie](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Dostawca mapy witryny SQL, dla którego użytkownik oczekiwał](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-the-membership-schema-in-sql-server-vb.md)
> [dalej](validating-user-credentials-against-the-membership-user-store-vb.md)

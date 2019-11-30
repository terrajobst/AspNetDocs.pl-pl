---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: Autoryzacja oparta na użytkownikach (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak ograniczyć dostęp do stron i ograniczyć funkcjonalność na poziomie strony za pomocą różnych technik.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: dfac0c6fa955e59c6ea996533f2447e89ec8d468
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588207"
---
# <a name="user-based-authorization-vb"></a>Autoryzacja oparta na użytkownikach (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> W tym samouczku dowiesz się, jak ograniczyć dostęp do stron i ograniczyć funkcjonalność na poziomie strony za pomocą różnych technik.

## <a name="introduction"></a>Wprowadzenie

Większość aplikacji sieci Web, które oferują konta użytkowników, to w części, aby ograniczyć dostęp niektórych osób odwiedzających do określonych stron w witrynie. W większości witryn messageboard online, na przykład wszyscy użytkownicy — anonimowe i uwierzytelnione — mogą wyświetlać wpisy messageboard, ale tylko uwierzytelnieni użytkownicy mogą odwiedzić stronę sieci Web, aby utworzyć nowy wpis. Mogą istnieć strony administracyjne, które są dostępne tylko dla określonego użytkownika (lub określonego zestawu użytkowników). Ponadto funkcje na poziomie strony mogą się różnić w zależności od użytkownika. Podczas wyświetlania listy wpisów Użytkownicy uwierzytelnieni są pokazani interfejs do oceny każdego wpisu, podczas gdy ten interfejs nie jest dostępny dla anonimowych odwiedzających.

ASP.NET ułatwia definiowanie reguł autoryzacji opartych na użytkownikach. Po prostu w `Web.config`można zablokować określone strony sieci Web lub całe katalogi, aby były dostępne tylko dla określonego podzbioru użytkowników. Funkcje na poziomie strony można włączać lub wyłączać na podstawie aktualnie zalogowanego użytkownika przy użyciu metod programistycznych i deklaratywnych.

W tym samouczku dowiesz się, jak ograniczyć dostęp do stron i ograniczyć funkcjonalność na poziomie strony za pomocą różnych technik. Zacznijmy!

## <a name="a-look-at-the-url-authorization-workflow"></a>Zapoznaj się z przepływem pracy autoryzacji adresu URL

Zgodnie z opisem w temacie Omówienie samouczka [*dotyczącego uwierzytelniania formularzy*](../introduction/an-overview-of-forms-authentication-vb.md) , gdy środowisko uruchomieniowe ASP.NET przetwarza żądanie dla zasobu ASP.NET, żądanie zgłasza wiele zdarzeń w cyklu życia. *Moduły HTTP* są klasami zarządzanymi, których kod jest wykonywany w odpowiedzi na konkretne zdarzenie w cyklu życia żądania. ASP.NET dostarcza wiele modułów HTTP, które wykonują podstawowe zadania w tle.

Jeden taki moduł HTTP jest [`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Zgodnie z opisem w poprzednich samouczkach główną funkcją `FormsAuthenticationModule` jest ustalenie tożsamości bieżącego żądania. W tym celu należy sprawdzić bilet uwierzytelniania formularzy, który znajduje się w pliku cookie lub osadzony w adresie URL. Ta identyfikacja odbywa się podczas [`AuthenticateRequest`go zdarzenia](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Innym ważnym modułem HTTP jest [`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), który jest wywoływany w odpowiedzi na [zdarzenie`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (które następuje po zdarzeniu `AuthenticateRequest`). `UrlAuthorizationModule` sprawdza adiustację konfiguracji w `Web.config`, aby określić, czy bieżąca tożsamość ma uprawnienia do odwiedzania określonej strony. Ten proces jest określany mianem *autoryzacji adresu URL*.

Sprawdzimy składnię reguł autoryzacji adresów URL w kroku 1, ale najpierw Przyjrzyjmy się `UrlAuthorizationModule` w zależności od tego, czy żądanie jest autoryzowane, czy nie. Jeśli `UrlAuthorizationModule` określa, że żądanie jest autoryzowane, nic nie robi i żądanie jest kontynuowane przez jego cykl życia. Jeśli jednak żądanie *nie* jest autoryzowane, `UrlAuthorizationModule` przerywa cykl życia i nakazuje obiektowi `Response` zwrócić stan [nieautoryzowany HTTP 401](http://www.checkupdown.com/status/E401.html) . W przypadku korzystania z uwierzytelniania formularzy ten stan HTTP 401 nie jest nigdy zwracany do klienta, ponieważ w przypadku wykrycia przez `FormsAuthenticationModule` stanu HTTP 401 jest modyfikowany na stronie logowania [http 302](http://www.checkupdown.com/status/E302.html) .

Rysunek 1 ilustruje przepływ pracy potoku ASP.NET, `FormsAuthenticationModule`i `UrlAuthorizationModule` po nadejściu nieautoryzowanego żądania. W szczególności rysunek 1 przedstawia żądanie anonimowego gościa `ProtectedPage.aspx`, który jest stroną, która odmówi dostępu anonimowym użytkownikom. Ponieważ odwiedzający jest anonimowy, `UrlAuthorizationModule` przerywa żądanie i zwraca stan nieautoryzowany HTTP 401. `FormsAuthenticationModule` następnie konwertuje stan 401 na stronę logowania do programu 302. Po uwierzytelnieniu użytkownika za pośrednictwem strony logowania zostaje on przekierowany do `ProtectedPage.aspx`. Tym razem `FormsAuthenticationModule` identyfikuje użytkownika na podstawie jego biletu uwierzytelniania. Teraz, gdy użytkownik jest uwierzytelniany, `UrlAuthorizationModule` zezwala na dostęp do strony.

[![przepływu pracy uwierzytelniania i autoryzacji adresów URL formularzy](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**Rysunek 1**. przepływ pracy uwierzytelniania formularzy i autoryzacji adresów URL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-vb/_static/image3.png))

Rysunek 1 przedstawia interakcję, która występuje, gdy anonimowy użytkownik próbuje uzyskać dostęp do zasobu, który nie jest dostępny dla użytkowników anonimowych. W takim przypadku anonimowe gościa są przekierowywane do strony logowania za pomocą strony, którą próbowano odwiedzać określone w ciągu QueryString. Po pomyślnym zalogowaniu się użytkownika zostanie on automatycznie przekierowany z powrotem do zasobu, który był początkowo poddany próbie wyświetlenia.

W przypadku nieautoryzowanego żądania przez użytkownika anonimowego ten przepływ pracy jest prosty i ułatwia odwiedzającemu zrozumienie, co się stało i dlaczego. Należy jednak pamiętać, że `FormsAuthenticationModule` przekieruje *dowolnego* nieautoryzowanego użytkownika do strony logowania, nawet jeśli żądanie zostało wysłane przez uwierzytelnionego użytkownika. Może to spowodować mylące środowisko użytkownika, jeśli uwierzytelniony użytkownik próbuje odwiedzić stronę, dla której nie ma uprawnień administratora.

Załóżmy, że nasza witryna sieci Web ma skonfigurowane reguły autoryzacji adresów URL w taki sposób, że strona ASP.NET `OnlyTito.aspx` była Accessibly tylko do Tito. Teraz wyobraź sobie, że sam odwiedza witrynę, loguje się, a następnie próbuje odwiedzić `OnlyTito.aspx`. `UrlAuthorizationModule` zatrzyma cykl życia żądania i zwróci stan nieautoryzowany HTTP 401, który zostanie wykryty przez `FormsAuthenticationModule`, a następnie przekierować do strony logowania. Ponieważ sam jest już zalogowany, może się zastanawiać, że został wysłany z powrotem do strony logowania. Może to oznaczać, że poświadczenia logowania zostały utracone w dowolny sposób lub wprowadzono nieprawidłowe poświadczenia. Jeśli sam ponownie wprowadzi swoje poświadczenia ze strony logowania, zostanie ono zalogowane (ponownie) i przekierowane do `OnlyTito.aspx`. `UrlAuthorizationModule` wykryje, że sam nie może odwiedzić tej strony i zostanie zwrócony do strony logowania.

Rysunek 2 przedstawia ten przepływ pracy mylącej.

[![domyślny przepływ pracy może prowadzić do mylącego cyklu](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**Rysunek 2**: domyślny przepływ pracy może prowadzić do mylącego cyklu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-vb/_static/image6.png))

Przepływ pracy przedstawiony na rysunku 2 może szybko befuddle nawet większość osób odwiedzających świadome. Zapoznaj się z sposobami, aby zapobiec tym cyklicznemu cyklowi w kroku 2.

> [!NOTE]
> ASP.NET używa dwóch mechanizmów, aby określić, czy bieżący użytkownik może uzyskać dostęp do określonej strony sieci Web: Autoryzacja adresów URL i autoryzacja plików. Autoryzacja plików jest implementowana przez [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), która określa Urząd, sprawdzając wymagane pliki list ACL. Autoryzacja plików jest najczęściej używana z uwierzytelnianiem systemu Windows, ponieważ listy ACL są uprawnieniami, które dotyczą kont systemu Windows. W przypadku korzystania z uwierzytelniania formularzy wszystkie żądania na poziomie systemu operacyjnego i systemu plików są wykonywane przez to samo konto systemu Windows, niezależnie od tego, czy użytkownik odwiedzający witrynę. Ponieważ ta seria samouczków koncentruje się na uwierzytelnianiu formularzy, nie będziemy omawiać autoryzacji plików.

### <a name="the-scope-of-url-authorization"></a>Zakres autoryzacji adresów URL

`UrlAuthorizationModule` jest kodem zarządzanym, który jest częścią środowiska uruchomieniowego ASP.NET. W wersji 7 serwera sieci Web [Internet Information Services (IIS)](https://www.iis.net/) firmy Microsoft istnieje odrębna bariera między POTOKIEM http usługi IIS a potokiem środowiska uruchomieniowego ASP.NET. W skrócie w programie IIS 6 i starszych wersjach ASP. `UrlAuthorizationModule` sieci jest wykonywana tylko wtedy, gdy żądanie jest delegowane z usług IIS do środowiska uruchomieniowego ASP.NET. Domyślnie usługi IIS przetwarzają zawartość statyczną w taki sam sposób, jak strony HTML oraz pliki CSS, JavaScript i Image, a także wyłączają żądania do środowiska uruchomieniowego ASP.NET, gdy zażądano strony z rozszerzeniem `.aspx`, `.asmx`lub `.ashx`.

Usługi IIS 7 umożliwiają jednak zintegrowane usługi IIS i potoki ASP.NET. Za pomocą kilku ustawień konfiguracji można skonfigurować usługi IIS 7 do wywoływania `UrlAuthorizationModule` dla *wszystkich* żądań, co oznacza, że reguły autoryzacji adresów URL można zdefiniować dla plików dowolnego typu. Ponadto usługi IIS 7 zawierają własny aparat autoryzacji adresów URL. Aby uzyskać więcej informacji na temat integracji ASP.NET i natywnych adresów URL usług IIS 7, zobacz [Omówienie autoryzacji adresów URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Aby zapoznać się z bardziej szczegółowym opisem integracji usług ASP.NET i IIS 7, zapoznaj się z kopią książki Shahram khosravi, *profesjonalną obsługą usług IIS 7 i ASP.NET zintegrowanym programowaniem* (ISBN: 978-0470152539).

W Nutshell, w wersjach wcześniejszych niż IIS 7, reguły autoryzacji adresów URL są stosowane tylko do zasobów obsłużonych przez środowisko uruchomieniowe ASP.NET. Jednak w przypadku usług IIS 7 możliwe jest korzystanie z funkcji autoryzacji natywnego adresu URL usług IIS lub integracji środowiska ASP. `UrlAuthorizationModule` sieci do potoku HTTP usług IIS, rozszerzając tę funkcjonalność na wszystkie żądania.

> [!NOTE]
> Istnieją pewne delikatne różnice w sposobie działania środowiska ASP. Funkcja autoryzacji adresów URL `UrlAuthorizationModule` i usług IIS 7 przetwarza reguły autoryzacji. Ten samouczek nie sprawdza funkcji autoryzacji adresów URL usług IIS 7 ani różnic w sposobie analizowania reguł autoryzacji w porównaniu do `UrlAuthorizationModule`. Aby uzyskać więcej informacji na temat tych tematów, zapoznaj się z dokumentacją usług IIS 7 w witrynie MSDN lub w witrynie [www.IIS.NET](https://www.iis.net/).

## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Krok 1. Definiowanie reguł autoryzacji adresów URL w`Web.config`

`UrlAuthorizationModule` określa, czy udzielić lub odmówić dostępu do żądanego zasobu dla określonej tożsamości na podstawie reguł autoryzacji adresów URL zdefiniowanych w konfiguracji aplikacji. Reguły autoryzacji są pisane w [`<authorization>` elemencie](https://msdn.microsoft.com/library/8d82143t.aspx) w formie `<allow>` i `<deny>` elementów podrzędnych. Każdy `<allow>` i `<deny>` element podrzędny mogą określić:

- Określony użytkownik
- Rozdzielana przecinkami lista użytkowników
- Wszyscy użytkownicy anonimowi, oznaczający znak zapytania (?)
- Wszyscy użytkownicy, oznaczona gwiazdką (\*)

W poniższym znaczniku pokazano, jak używać reguł autoryzacji adresów URL, aby umożliwić użytkownikom Tito i Scott i odrzucanie wszystkich innych:

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

Element `<allow>` definiuje dozwolonych użytkowników — Tito i Scott — podczas gdy element `<deny>` instruuje o tym, że *Wszyscy* użytkownicy są odrzucani.

> [!NOTE]
> Elementy `<allow>` i `<deny>` mogą również określać reguły autoryzacji dla ról. W przyszłości sprawdzimy autoryzację opartą na rolach w przyszłym samouczku.

Następujące ustawienie udziela dostępu innym osobom niż sam (w tym anonimowym Gościom):

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

Aby zezwolić tylko uwierzytelnionym użytkownikom, użyj następującej konfiguracji, która uniemożliwia dostęp do wszystkich anonimowych użytkowników:

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

Reguły autoryzacji są zdefiniowane w ramach elementu `<system.web>` w `Web.config` i mają zastosowanie do wszystkich zasobów ASP.NET w aplikacji sieci Web. Często, aplikacja ma różne reguły autoryzacji dla różnych sekcji. Na przykład w witrynie handlu elektronicznego wszyscy Goście mogą zapoznania produkty, zapoznaj się z tematem przeglądy produktów, Wyszukaj katalog i tak dalej. Jednak tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do wyewidencjonowania lub stron w celu zarządzania jedną historią wysyłki. Ponadto mogą istnieć części witryny, które są dostępne tylko dla wybranych użytkowników, takich jak Administratorzy lokacji.

ASP.NET ułatwia definiowanie różnych reguł autoryzacji dla różnych plików i folderów w lokacji. Reguły autoryzacji określone w pliku `Web.config` folderu głównego mają zastosowanie do wszystkich zasobów ASP.NET w lokacji. Jednak te domyślne ustawienia autoryzacji mogą zostać zastąpione dla określonego folderu przez dodanie `Web.config` z sekcją `<authorization>`.

Zaktualizujmy naszą witrynę sieci Web tak, aby tylko uwierzytelnieni użytkownicy mogli odwiedzać strony ASP.NET w folderze `Membership`. Aby to osiągnąć, należy dodać plik `Web.config` do folderu `Membership` i ustawić jego ustawienia autoryzacji, aby odmówić anonimowym użytkownikom. Kliknij prawym przyciskiem myszy folder `Membership` w Eksplorator rozwiązań, wybierz z menu kontekstowego menu Dodaj nowy element i Dodaj nowy plik konfiguracji sieci Web o nazwie `Web.config`.

[![dodać pliku Web. config do folderu Membership](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**Rysunek 3**. dodawanie pliku `Web.config` do folderu `Membership` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image9.png))

W tym momencie projekt powinien zawierać dwa `Web.config` pliki: jeden w katalogu głównym i jeden w folderze `Membership`.

[![aplikacja powinna teraz zawierać dwa pliki Web. config](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**Ilustracja 4**. aplikacja powinna teraz zawierać dwa `Web.config` pliki ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image12.png))

Zaktualizuj plik konfiguracji w folderze `Membership`, aby uniemożliwić dostęp anonimowym użytkownikom.

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

To wszystko.

Aby przetestować tę zmianę, odwiedź stronę główną w przeglądarce i upewnij się, że nastąpiło wylogowanie. Ponieważ domyślnym zachowaniem aplikacji ASP.NET jest zezwolenie wszystkim odwiedzającym, a ponieważ nie wprowadzono żadnych modyfikacji autoryzacji do pliku `Web.config` katalogu głównego, możemy odwiedzać pliki w katalogu głównym jako anonimowe gościa.

Kliknij link tworzenie kont użytkowników znajdujący się w lewej kolumnie. Spowoduje to przejście do `~/Membership/CreatingUserAccounts.aspx`. Ponieważ plik `Web.config` w folderze `Membership` definiuje reguły autoryzacji w celu zabronienia dostępu anonimowego, `UrlAuthorizationModule` przerywa żądanie i zwraca stan nieautoryzowany HTTP 401. `FormsAuthenticationModule` modyfikuje ten stan na 302, wysyłając nam do strony logowania. Zwróć uwagę, że strona, do której próbowano uzyskać dostęp (`CreatingUserAccounts.aspx`), została przeniesiona na stronę logowania za pośrednictwem parametru `ReturnUrl` QueryString.

[![, ponieważ reguły autoryzacji adresów URL zabraniają dostępu anonimowego, nastąpi przekierowanie do strony logowania](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**Rysunek 5**. ponieważ reguły autoryzacji adresów URL zabraniają dostępu anonimowego, nastąpi przekierowanie do strony logowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image15.png))

Po pomyślnym zalogowaniu zostanie przekierowany na stronę `CreatingUserAccounts.aspx`. Tym razem `UrlAuthorizationModule` zezwala na dostęp do strony, ponieważ nie są już anonimowe.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Stosowanie reguł autoryzacji adresów URL do określonej lokalizacji

Ustawienia autoryzacji zdefiniowane w `<system.web>` sekcji `Web.config` mają zastosowanie do wszystkich zasobów ASP.NET w tym katalogu i jego podkatalogach (do momentu zastąpienia przez inny plik `Web.config`). W niektórych przypadkach firma Microsoft może chcieć, że wszystkie zasoby ASP.NET w danym katalogu mają określoną konfigurację autoryzacji z wyjątkiem jednej lub dwóch określonych stron. Można to osiągnąć przez dodanie elementu `<location>` w `Web.config`, wskazujący na plik, którego reguły autoryzacji różnią się i Definiowanie jego unikatowych reguł autoryzacji.

Aby zilustrować użycie elementu `<location>`, aby zastąpić ustawienia konfiguracji określonego zasobu, Dostosuj ustawienia autoryzacji, tak aby tylko Tito mogły odwiedzać `CreatingUserAccounts.aspx`. Aby to osiągnąć, Dodaj element `<location>` do pliku `Web.config` folderu `Membership` i zaktualizuj jego znaczniki, tak aby wyglądał wyglądać następująco:

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

Element `<authorization>` w `<system.web>` definiuje domyślne reguły autoryzacji adresów URL dla zasobów ASP.NET w folderze `Membership` i jego podfolderach. Element `<location>` pozwala nam zastąpić te reguły dla określonego zasobu. W powyższym znaczniku element `<location>` odwołuje się do strony `CreatingUserAccounts.aspx` i określa reguły autoryzacji, takie jak Zezwalanie na Tito, ale odmowa innym osobom.

Aby przetestować tę zmianę autoryzacji, Zacznij od odwiedzenia witryny sieci Web jako użytkownika anonimowego. Jeśli spróbujesz odwiedzić dowolną stronę w folderze `Membership`, na przykład `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` odmówi tego żądania, a nastąpi przekierowanie do strony logowania. Po zalogowaniu się jako, Scott, można odwiedzić dowolną stronę w folderze `Membership`, *z wyjątkiem* `CreatingUserAccounts.aspx`. Próba odwiedzania `CreatingUserAccounts.aspx` zalogowany jako każda osoba, ale Tito spowoduje nieautoryzowany dostęp, przekierowując Cię z powrotem do strony logowania.

> [!NOTE]
> Element `<location>` musi znajdować się poza elementem `<system.web>` konfiguracji. Musisz użyć oddzielnego elementu `<location>` dla każdego zasobu, którego ustawienia autoryzacji chcesz przesłonić.

### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Zapoznaj się z tym, jak`UrlAuthorizationModule`używa reguł autoryzacji w celu udzielania lub odmawiania dostępu

`UrlAuthorizationModule` określa, czy autoryzować określoną tożsamość dla określonego adresu URL przez analizowanie reguł autoryzacji adresów URL po jednej naraz, rozpoczynając od pierwszej i działającej w sposób. Po znalezieniu dopasowania użytkownikowi otrzymuje lub odmówiono dostępu, w zależności od tego, czy dopasowanie zostało znalezione w `<allow>` lub `<deny>` elementu. <strong>Jeśli nie zostanie znalezione żadne dopasowanie, użytkownikowi zostanie udzielony dostęp.</strong> W związku z tym, jeśli chcesz ograniczyć dostęp, konieczne jest użycie elementu `<deny>` jako ostatniego elementu w konfiguracji autoryzacji adresów URL. <strong>W przypadku pominięcia</strong> elementu<strong>`<deny>`</strong> <strong>wszystkim użytkownikom zostanie udzielony dostęp.</strong>

Aby lepiej zrozumieć proces używany przez `UrlAuthorizationModule` do określania urzędu, należy wziąć pod uwagę przykładowe reguły autoryzacji adresów URL, które zostały wcześniej przedstawione w tym kroku. Pierwsza reguła to `<allow>` elementu, który umożliwia dostęp do Tito i Scott. Drugimi regułami jest element `<deny>`, który odmówi dostępu wszystkim osobom. W przypadku użytkowników anonimowych, `UrlAuthorizationModule` rozpocznie się z pytaniem, czy ma anonimowe lub Tito? Odpowiedź, oczywiście, nie jest, więc przechodzi do drugiej reguły. Czy w zestawie każdy jest anonimowy? Ponieważ odpowiedź tutaj ma wartość tak, reguła `<deny>` jest włączona, a użytkownik zostanie przekierowany do strony logowania. Podobnie, jeśli Jisun jest odwiedzane, `UrlAuthorizationModule` zaczyna się z pytaniem, czy Jisun albo Scott czy Tito? Ponieważ nie jest, `UrlAuthorizationModule` przechodzi do drugiego pytania, jest Jisun w zestawie każdy? Jest to, dlatego, jest również odmowa dostępu. Na koniec, jeśli Tito wizyty, pierwsze pytanie powodowane przez `UrlAuthorizationModule` jest odpowiedzią pozytywną, więc Tito udzielono dostępu.

Ponieważ `UrlAuthorizationModule` przetwarza reguły autoryzacji z góry w dół, zatrzymywanie z dowolnego dopasowania, ważne jest, aby bardziej szczegółowe reguły znajdowały się przed mniej określonymi. Oznacza to, że w celu zdefiniowania reguł autoryzacji, które zabraniają Jisun i użytkowników anonimowych, ale zezwalają wszystkim innym uwierzytelnionym użytkownikom, należy zacząć od najbardziej konkretnej reguły — ma to wpływ na Jisun, a następnie przechodzenie do określonych reguł. Użytkownicy uwierzytelnieni, ale odrzucani wszyscy użytkownicy anonimowi. Następujące reguły autoryzacji adresów URL implementują te zasady przez pierwsze odmowę Jisun, a następnie odmowę dowolnego użytkownika anonimowego. Do każdego uwierzytelnionego użytkownika innego niż Jisun zostanie udzielony dostęp, ponieważ żadna z tych instrukcji `<deny>` nie będzie zgodna.

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Krok 2. naprawianie przepływu pracy dla nieautoryzowanych użytkowników uwierzytelnionych

Jak opisano wcześniej w tym samouczku w sekcji A na stronie przepływu pracy autoryzacji adresów URL kiedykolwiek nieautoryzowane żądanie transpires, `UrlAuthorizationModule` przerywa żądanie i zwraca stan nieautoryzowany HTTP 401. Ten stan 401 jest modyfikowany przez `FormsAuthenticationModule` do stanu przekierowywania 302, który wysyła użytkownika do strony logowania. Ten przepływ pracy występuje na dowolnym nieautoryzowanym żądaniu, nawet jeśli użytkownik jest uwierzytelniony.

Zwrócenie uwierzytelnionego użytkownika na stronę logowania może je mylić, ponieważ zostały już zarejestrowane w systemie. Za pomocą małej liczby prac możemy ulepszyć ten przepływ pracy, przekierowując uwierzytelnionych użytkowników, którzy w tym celu wyróżnią próby dostępu do strony z ograniczeniami.

Zacznij od utworzenia nowej strony ASP.NET w folderze głównym aplikacji sieci Web o nazwie `UnauthorizedAccess.aspx`; nie zapomnij skojarzyć tej strony ze stroną wzorcową `Site.master`. Po utworzeniu tej strony Usuń kontrolkę zawartości odwołującą się do `LoginContent` ContentPlaceHolder, aby wyświetlić domyślną zawartość strony wzorcowej. Następnie Dodaj komunikat objaśniający sytuację, a mianowicie, że użytkownik próbował uzyskać dostęp do chronionego zasobu. Po dodaniu tego komunikatu, znaczniki deklaratywne `UnauthorizedAccess.aspx` strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

Teraz konieczna jest zmiana przepływu pracy w taki sposób, aby w przypadku, gdy uwierzytelnionego użytkownika są wysyłane do strony `UnauthorizedAccess.aspx` zamiast na stronie logowania. Logika, która przekierowuje nieautoryzowane żądania do strony logowania, jest przydzielonych w ramach prywatnej metody klasy `FormsAuthenticationModule`, więc nie można dostosować tego zachowania. Można jednak dodać własną logikę do strony logowania, która przekierowuje użytkownika do `UnauthorizedAccess.aspx`, w razie potrzeby.

Gdy `FormsAuthenticationModule` przekierowuje nieautoryzowanego gościa do strony logowania, dołącza żądany, nieautoryzowany adres URL do ciągu QueryString o nazwie `ReturnUrl`. Na przykład jeśli nieautoryzowany użytkownik próbował odwiedzać `OnlyTito.aspx`, `FormsAuthenticationModule` przekierować je do `Login.aspx?ReturnUrl=OnlyTito.aspx`. W związku z tym, jeśli strona logowania zostanie osiągnięta przez uwierzytelnionego użytkownika z QueryString, która zawiera parametr `ReturnUrl`, wiemy, że ten nieuwierzytelniony użytkownik próbuje odwiedzić stronę, której nie ma autoryzacji do wyświetlenia. W takim przypadku chcemy przekierować do `UnauthorizedAccess.aspx`.

Aby to osiągnąć, Dodaj następujący kod do programu obsługi zdarzeń `Page_Load` stronie logowania:

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

Powyższy kod przekierowuje uwierzytelnione, nieautoryzowanych użytkowników do strony `UnauthorizedAccess.aspx`. Aby wyświetlić tę logikę w działaniu, odwiedź witrynę jako osobę odwiedzającą anonimowe i kliknij link tworzenie kont użytkowników w lewej kolumnie. Spowoduje to przejście do strony `~/Membership/CreatingUserAccounts.aspx`, która w kroku 1 została skonfigurowana tak, aby zezwalać na dostęp tylko do Tito. Ponieważ użytkownicy anonimowi są zabroniona, `FormsAuthenticationModule` przekierowuje do strony logowania.

W tym momencie mamy anonimowe, więc `Request.IsAuthenticated` zwraca `False` i nie przekierowywać do `UnauthorizedAccess.aspx`. Zamiast tego zostanie wyświetlona strona logowania. Zaloguj się jako użytkownik inny niż Tito, na przykład Bruce. Po wprowadzeniu odpowiednich poświadczeń Strona logowania przekierowuje ją z powrotem do `~/Membership/CreatingUserAccounts.aspx`. Jednak ze względu na to, że ta strona jest dostępna tylko dla Tito, nie jest ona nieautoryzowana do wyświetlenia i zostanie natychmiast zwrócona na stronę logowania. Tym razem `Request.IsAuthenticated` zwraca `True` (i ciąg `ReturnUrl` QueryString istnieje), więc nastąpi przekierowanie do strony `UnauthorizedAccess.aspx`.

[![uwierzytelniony, nieautoryzowani użytkownicy są przekierowywani do UnauthorizedAccess. aspx](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**Rysunek 6**: uwierzytelniony, nieautoryzowani użytkownicy są przekierowywani do `UnauthorizedAccess.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-vb/_static/image18.png))

Ten dostosowany przepływ pracy przedstawia bardziej rozsądne i proste środowisko użytkownika przez krótki obwód cyklu przedstawiony na rysunku 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Krok 3. Ograniczanie funkcjonalności na podstawie aktualnie zalogowanego użytkownika

Autoryzacja adresów URL ułatwia określanie bardzo grubych reguł autoryzacji. Jak widać w kroku 1, z autoryzacją adresów URL możemy w zwięzły sposób określić, jakie tożsamości są dozwolone i które z nich odmówiono, aby wyświetlić określoną stronę lub wszystkie strony w folderze. W niektórych scenariuszach warto jednak zezwolić wszystkim użytkownikom na odwiedzenie strony, ale ograniczenie funkcjonalności strony na podstawie jej użytkownika.

Rozważmy przypadek witryny sieci Web handlu elektronicznego, która umożliwia uwierzytelnionym odwiedzającym przeglądanie swoich produktów. Gdy użytkownik anonimowy odwiedzi stronę produktu, zobaczy tylko informacje o produkcie i nie będzie miał możliwości opuszczenia przeglądu. Jednak uwierzytelniony użytkownik odwiedzający tę samą stronę zobaczy interfejs recenzowania. Jeśli uwierzytelniony użytkownik nie sprawdził jeszcze tego produktu, interfejs umożliwi im przesłanie przeglądu; w przeciwnym razie będzie pokazywała ich wcześniej przesłany przegląd. Aby to zrobić, na stronie produkt mogą być wyświetlane dodatkowe informacje i oferowane rozszerzone funkcje dla tych użytkowników, którzy pracują z firmą handlu elektronicznego. Na przykład na stronie produkt może znajdować się Spis zapasów oraz opcje umożliwiające edytowanie ceny i opisu produktu podczas odwiedzania przez pracownika.

Takie precyzyjne reguły autoryzacji ziarna mogą być implementowane w sposób deklaratywny lub programistyczny (lub poprzez kilka kombinacji dwóch). W następnej sekcji zobaczymy, jak zaimplementować szczegółowe autoryzację za pośrednictwem formantu widoku logowania. Po tym będziemy eksplorować techniki programistyczne. Zanim będziemy mogli zapoznać się z zastosowaniem reguł autoryzacji szczegółowych, należy najpierw utworzyć stronę, której funkcjonalność zależy od użytkownika.

Utwórzmy stronę, która wyświetla listę plików w określonym katalogu w widoku GridView. Wraz z listą nazw, rozmiarów i innych plików każdego pliku GridView będzie zawierać dwie kolumny LinkButtons: jeden widok z tytułem i jeden z tytułami. Jeśli zostanie kliknięty widok element LinkButton, zostanie wyświetlona zawartość wybranego pliku; Po kliknięciu przycisku Usuń element LinkButton plik zostanie usunięty. Najpierw utwórz Tę stronę w taki sposób, aby jej funkcje wyświetlania i usuwania były dostępne dla wszystkich użytkowników. W sekcjach przy użyciu kontrolki widoku logowania i programowego ograniczania funkcjonalności zobaczymy, jak włączyć lub wyłączyć te funkcje na podstawie użytkownika odwiedzającego stronę.

> [!NOTE]
> Strona ASP.NET do skompilowania używa kontrolki GridView do wyświetlania listy plików. Ponieważ ta seria samouczków koncentruje się na uwierzytelnianiu formularzy, autoryzacji, kontach użytkowników i rolach, nie chcę poświęcać zbyt dużo czasu na omawianie wewnętrznych czynności kontrolki GridView. Ten samouczek zawiera szczegółowe instrukcje krok po kroku dotyczące konfigurowania tej strony, dlatego nie są w stanie odszukać szczegółowych informacji o tym, dlaczego zostały wykonane pewne wybory, lub jakie konkretne właściwości mają wpływ na renderowane dane wyjściowe. Dokładne badanie kontrolki GridView można znaleźć *[w sekcji Praca z danymi w](../../data-access/index.md)* serii samouczków ASP.NET 2,0.

Zacznij od otwarcia pliku `UserBasedAuthorization.aspx` w folderze `Membership` i dodania kontrolki GridView do strony o nazwie `FilesGrid`. W tagu inteligentnym GridView kliknij link Edytuj kolumny, aby uruchomić okno dialogowe pola. W tym miejscu Usuń zaznaczenie pola wyboru Automatyczne generowanie pól w lewym dolnym rogu. Następnie Dodaj przycisk wyboru, przycisk Usuń i dwóch BoundFields z lewego górnego rogu (przyciski SELECT i DELETE można znaleźć w obszarze typu CommandField). Ustaw właściwość `SelectText` przycisku Wybierz, aby wyświetlić i `HeaderText` pierwszej BoundField i `DataField` właściwości. Ustaw właściwość `HeaderText` drugiej BoundField na rozmiar w bajtach, jej Właściwość `DataField` na długość, jej Właściwość `DataFormatString` do {0:N0} i jej Właściwość `HtmlEncode` na wartość false.

Po skonfigurowaniu kolumn GridView kliknij przycisk OK, aby zamknąć okno dialogowe pola. W okno Właściwości ustaw właściwość `DataKeyNames` GridView na `FullName`. W tym momencie znaczniki deklaratywne GridView powinny wyglądać następująco:

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

Po utworzeniu znacznika GridView można napisać kod, który pobierze pliki w określonym katalogu i powiąże je z elementem GridView. Dodaj następujący kod do programu obsługi zdarzeń `Page_Load` strony:

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

Powyższy kod używa [klasy`DirectoryInfo`](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) , aby uzyskać listę plików w folderze głównym aplikacji. [Metoda`GetFiles()`](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) zwraca wszystkie pliki w katalogu jako tablicę [obiektów`FileInfo`](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), które są następnie powiązane z elementem GridView. Obiekt `FileInfo` ma asortyment właściwości, takie jak `Name`, `Length`i `IsReadOnly`, między innymi. Jak widać na podstawie znaczników deklaratywnych, w widoku GridView są wyświetlane tylko `Name` i `Length` właściwości.

> [!NOTE]
> Klasy `DirectoryInfo` i `FileInfo` znajdują się w [przestrzeni nazw`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). W związku z tym należy najpierw podać te nazwy klas z ich nazwami przestrzeni nazw lub mieć zaimportowaną przestrzeń nazw do pliku klasy (za pośrednictwem `Imports System.IO`).

Poświęć chwilę na odwiedzenie tej strony za pomocą przeglądarki. Zostanie wyświetlona lista plików znajdujących się w katalogu głównym aplikacji. Kliknięcie dowolnego z widoków lub usunięcie LinkButtons spowoduje odświeżenie, ale nie wystąpią żadne działania, ponieważ jeszcze raz utworzymy niezbędne programy obsługi zdarzeń.

[![GridView wyświetla listę plików w katalogu głównym aplikacji sieci Web](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**Rysunek 7**. element GridView wyświetla listę plików w katalogu głównym aplikacji sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image21.png))

Potrzebujemy metody, aby wyświetlić zawartość wybranego pliku. Wróć do programu Visual Studio i Dodaj pole tekstowe o nazwie `FileContents` nad elementem GridView. Ustaw jej Właściwość `TextMode` na `MultiLine` i jej właściwości `Columns` i `Rows` odpowiednio do 95% i 10.

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń dla [zdarzenia`SelectedIndexChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) GridView i Dodaj następujący kod:

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

Ten kod używa właściwości `SelectedValue` GridView, aby określić pełną nazwę wybranego pliku. Wewnętrznie do uzyskania `SelectedValue`jest przywoływana kolekcja `DataKeys`, dlatego należy ustawić właściwość `DataKeyNames` GridView jako nazwę, jak opisano wcześniej w tym kroku. [Klasa`File`](https://msdn.microsoft.com/library/system.io.file.aspx) jest używana do odczytywania zawartości wybranego pliku do ciągu, który następnie jest przypisywany do właściwości `Text` pola tekstowego `FileContents`, co spowoduje wyświetlenie zawartości wybranego pliku na stronie.

[![zawartość wybranego pliku zostanie wyświetlona w polu tekstowym](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**Ilustracja 8**. zawartość wybranego pliku jest wyświetlana w polu tekstowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image24.png))

> [!NOTE]
> Jeśli zobaczysz zawartość pliku, który zawiera znaczniki HTML, a następnie spróbujesz wyświetlić lub usunąć plik, zostanie wyświetlony komunikat o błędzie `HttpRequestValidationException`. Dzieje się tak, ponieważ w przypadku ogłaszania zwrotnego zawartość pola tekstowego jest wysyłana z powrotem do serwera sieci Web. Domyślnie ASP.NET `HttpRequestValidationException` zgłasza błąd, gdy zostanie wykryta potencjalnie niebezpieczna zawartość ogłaszania zwrotnego, taka jak znacznik HTML. Aby wyłączyć ten błąd, wyłącz weryfikację żądań dla strony, dodając `ValidateRequest="false"` do dyrektywy `@Page`. Aby uzyskać więcej informacji na temat korzyści z weryfikacji żądań, a także działań, które należy podjąć podczas jego wyłączania, Przeczytaj [Sprawdzanie poprawności żądania — uniemożliwianie ataków na skrypty](https://asp.net/learn/whitepapers/request-validation/).

Na koniec Dodaj procedurę obsługi zdarzeń z następującym kodem dla [zdarzenia`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)GridView:

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

Kod po prostu wyświetla pełną nazwę pliku do usunięcia w `FileContents` TextBox *bez* faktycznego usunięcia pliku.

[![kliknięcie przycisku Usuń nie powoduje usunięcia pliku](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**Ilustracja 9**. kliknięcie przycisku Usuń nie powoduje usunięcia pliku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image27.png))

W kroku 1 zostały skonfigurowane reguły autoryzacji adresów URL, aby uniemożliwić użytkownikom anonimowym wyświetlanie stron w folderze `Membership`. Aby lepiej zachowywać szczegółowe uwierzytelnianie, zezwól użytkownikom anonimowym na odwiedzenie strony `UserBasedAuthorization.aspx`, ale z ograniczoną funkcjonalnością. Aby otworzyć tę stronę do uzyskania dostępu przez wszystkich użytkowników, Dodaj następujący `<location>` elementu do pliku `Web.config` w folderze `Membership`:

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

Po dodaniu tego elementu `<location>` Przetestuj nowe reguły autoryzacji adresów URL, logując się z lokacji. Jako użytkownik anonimowy powinien być uprawniony do odwiedzenia strony `UserBasedAuthorization.aspx`.

Obecnie każdy uwierzytelniony lub anonimowy użytkownik może odwiedzić stronę `UserBasedAuthorization.aspx` i wyświetlić lub usunąć pliki. Zmieńmy to tak, aby tylko uwierzytelnieni użytkownicy mogli wyświetlać zawartość pliku i tylko Tito mogą usunąć plik. Takie szczegółowe reguły autoryzacji ziarna mogą być stosowane deklaratywnie, programowo lub za pomocą kombinacji obu metod. Użyjemy podejścia deklaracyjnego, aby ograniczyć, kto może wyświetlać zawartość pliku; użyjemy podejścia programistycznego, aby ograniczyć, kto może usunąć plik.

### <a name="using-the-loginview-control"></a>Korzystanie z formantu widoku logowania

Jak widać w poprzednich samouczkach, formant widoku logowania jest przydatny do wyświetlania różnych interfejsów dla użytkowników uwierzytelnionych i anonimowych, a także oferuje łatwy sposób ukrywania funkcji, które nie są dostępne dla użytkowników anonimowych. Ponieważ użytkownicy anonimowi nie mogą wyświetlać ani usuwać plików, musimy wyświetlić `FileContents` pole tekstowe, gdy strona zostanie odwiedzana przez uwierzytelnionego użytkownika. Aby to osiągnąć, Dodaj kontrolkę widoku logowania do strony, nadaj jej nazwę `LoginViewForFileContentsTextBox`i Przenieś znaczniki deklaratywne `FileContents` TextBox do `LoggedInTemplate`kontrolki widoku logowania.

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

Kontrolki sieci Web w szablonach widoku logowania nie są już bezpośrednio dostępne z klasy związanej z kodem. Na przykład `FilesGrid` GridView `SelectedIndexChanged` i `RowDeleting` obsługi zdarzeń, obecnie odwołują się do kontrolki TextBox `FileContents` z kodem podobnym do:

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

Jednak ten kod nie jest już prawidłowy. Przenosząc `FileContents` pole tekstowe do `LoggedInTemplate` nie można bezpośrednio uzyskać dostępu do pola tekstowego. Zamiast tego należy użyć metody `FindControl("controlId")` do programistycznego odwoływania się do kontrolki. Zaktualizuj procedury obsługi zdarzeń `FilesGrid`, aby odwołać się do pola tekstowego, takiego jak:

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

Po przeniesieniu pola tekstowego do `LoggedInTemplate` widoku logowania i zaktualizowania kodu strony, aby odwołać się do pola tekstowego przy użyciu wzorca `FindControl("controlId")`, odwiedź stronę jako użytkownika anonimowego. Jak pokazano na rysunku 10, pole tekstowe `FileContents` nie jest wyświetlane. Jednak widok element LinkButton jest nadal wyświetlany.

[![formant widoku logowania renderuje tylko pole tekstowe FileContents dla uwierzytelnionych użytkowników](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**Ilustracja 10**: formant widoku logowania renderuje tylko pole tekstowe `FileContents` dla użytkowników uwierzytelnionych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image30.png))

Jednym ze sposobów ukrycia przycisku widoku dla użytkowników anonimowych jest przekonwertowanie pola GridView na TemplateField. Spowoduje to wygenerowanie szablonu, który zawiera deklaratywne znaczniki dla widoku element LinkButton. Następnie możemy dodać formant widoku logowania do TemplateField i umieścić element LinkButton w obrębie `LoggedInTemplate`widoku logowania, co spowoduje ukrycie przycisku widoku od anonimowych odwiedzających. Aby to osiągnąć, kliknij link Edytuj kolumny w tagu inteligentnym GridView, aby uruchomić okno dialogowe pola. Następnie wybierz przycisk Wybierz z listy w lewym dolnym rogu, a następnie kliknij łącze Konwertuj to pole na TemplateField. Wykonanie tej czynności spowoduje zmodyfikowanie deklaratywnego znacznika pola z:

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 Do: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 W tym momencie możemy dodać widoku logowania do TemplateField. Następujące znaczniki wyświetlają widok element LinkButton tylko dla uwierzytelnionych użytkowników. 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

Jak pokazano na rysunku 11, wynik końcowy nie jest widoczny, ponieważ kolumna widoku jest wciąż wyświetlana, mimo że widok LinkButtons w kolumnie jest ukryty. Dowiesz się, jak ukryć całą kolumnę GridView (a nie tylko element LinkButton) w następnej sekcji.

[![formant widoku logowania ukrywa LinkButtons widoku dla anonimowych odwiedzających](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**Ilustracja 11**. kontrolka widoku logowania ukrywa widok LinkButtons dla anonimowych odwiedzających ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Programowe Ograniczanie funkcjonalności

W pewnych okolicznościach, techniki deklaracyjne są niewystarczające do ograniczania funkcjonalności do strony. Na przykład dostępność niektórych funkcji strony może być zależna od kryteriów wykraczających poza to, czy użytkownik odwiedzający stronę jest anonimowy czy uwierzytelniony. W takich przypadkach różne elementy interfejsu użytkownika mogą być wyświetlane lub ukrywane za pomocą metod programistycznych.

Aby programowo ograniczyć funkcjonalność, musimy wykonać dwa zadania:

1. Określ, czy użytkownik odwiedzający stronę może uzyskać dostęp do funkcji, i
2. Programowo zmodyfikuj interfejs użytkownika w zależności od tego, czy użytkownik ma dostęp do danej funkcjonalności.

Aby zademonstrować zastosowanie tych dwóch zadań, Zezwól na Tito tylko na usunięcie plików z widoku GridView. Najpierw należy określić, czy Tito odwiedzać stronę. Po ustaleniu, musimy ukryć (lub pokazać) kolumnę usuwania GridView. Kolumny GridView są dostępne za pomocą właściwości `Columns`; kolumna jest renderowana tylko wtedy, gdy jej Właściwość `Visible` jest ustawiona na `True` (wartość domyślna).

Dodaj następujący kod do programu obsługi zdarzeń `Page_Load` przed powiązaniem danych z GridView:

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

Zgodnie z opisem w temacie [*Omówienie samouczka dotyczącego uwierzytelniania formularzy*](../introduction/an-overview-of-forms-authentication-vb.md) `User.Identity.Name` zwraca nazwę tożsamości. Odnosi się to do nazwy użytkownika wprowadzonej w kontrolce logowania. Jeśli Tito odwiedzanie strony, właściwość `Visible` drugiej kolumny GridView jest ustawiona na `True`; w przeciwnym razie jest ustawiony na `False`. Wynikiem jest fakt, że w przypadku, gdy ktoś inny niż Tito odwiedzi stronę, jest to inny użytkownik uwierzytelniony lub anonimowy użytkownik, a kolumna Usuń nie jest renderowana (patrz rysunek 12). Jednak gdy Tito odwiedzi stronę, kolumna usuwania jest obecna (Zobacz Rysunek 13).

[![kolumna usuwania nie jest renderowana, gdy jest odwiedzana przez kogoś innego niż Tito (na przykład Bruce)](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**Ilustracja 12**. kolumna usuwania nie jest renderowana, gdy jest odwiedzana przez kogoś innego niż Tito (na przykład Bruce) ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](user-based-authorization-vb/_static/image36.png))

[![kolumna usuwania jest renderowana dla Tito](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**Ilustracja 13**. kolumna usuwania jest renderowana dla Tito ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-vb/_static/image39.png))

## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Krok 4. stosowanie reguł autoryzacji do klas i metod

W kroku 3 nie zezwolił anonimowym użytkownikom na wyświetlanie zawartości pliku i zabronionej wszystkich użytkowników, ale Tito usuwanie plików. Zostało to osiągnięte przez ukrycie skojarzonych z nimi elementów interfejsu użytkownika dla nieautoryzowanych osób odwiedzających za pomocą technik deklaratywnych i programistycznych. Aby zapoznać się z prostym przykładem, dobrze ukrywając elementy interfejsu użytkownika, ale co to jest bardziej złożone lokacje, w których może być wiele różnych sposobów wykonywania tych samych funkcji? W przypadku ograniczania funkcjonalności do nieautoryzowanych użytkowników, co się stanie, jeśli zapomnię ukryć lub wyłączyć wszystkie odpowiednie elementy interfejsu użytkownika?

Łatwym sposobem zapewnienia dostępu do konkretnej funkcji przez nieautoryzowanego użytkownika jest dekorować tej klasy lub metody z [atrybutem`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Gdy środowisko uruchomieniowe platformy .NET używa klasy lub wykonuje jedną z jej metod, sprawdza, czy bieżący kontekst zabezpieczeń ma uprawnienia do używania klasy lub wykonywania metody. Atrybut `PrincipalPermission` zapewnia mechanizm, za pomocą którego możemy definiować te reguły.

Pokażmy przy użyciu atrybutu `PrincipalPermission` na `SelectedIndexChanged`ach i `RowDeleting` obsługi zdarzeń w widoku GridView, aby zabronić wykonywania przez anonimowych użytkowników i użytkowników innych niż Tito. Wystarczy dodać odpowiedni atrybut korzystającego każdej definicji funkcji:

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

Atrybut dla programu obsługi zdarzeń `SelectedIndexChanged` określa, że tylko uwierzytelnieni użytkownicy mogą wykonywać procedurę obsługi zdarzeń, gdzie jako atrybut procedury obsługi zdarzeń `RowDeleting` ogranicza wykonywanie do Tito.

> [!NOTE]
> Atrybut może być stosowany do klasy, metody, właściwości lub zdarzenia. Podczas dodawania atrybutu, musi on być częścią deklaracji klasy, metody, właściwości lub deklaracji zdarzenia. Ponieważ Visual Basic używa podziałów wierszy jako ograniczników instrukcji, atrybuty muszą być wyświetlane w tym samym wierszu co deklaracja lub bezpośrednio powyżej, z znakiem kontynuacji wiersza (podkreśleniem). W powyższym fragmencie kodu znak kontynuacji wiersza jest używany do umieszczania atrybutu w jednym wierszu i deklaracji metody na innym.

Jeśli w jakiś sposób użytkownik inny niż Tito próbuje wykonać procedurę obsługi zdarzeń `RowDeleting` lub nieuwierzytelniony użytkownik próbuje wykonać procedurę obsługi zdarzeń `SelectedIndexChanged`, środowisko uruchomieniowe platformy .NET utworzy `SecurityException`.

[![, jeśli kontekst zabezpieczeń nie jest autoryzowany do wykonania metody, zostanie zgłoszony wyjątek SecurityException](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**Ilustracja 14**. Jeśli kontekst zabezpieczeń nie jest autoryzowany do wykonania metody, zostanie zgłoszony `SecurityException` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](user-based-authorization-vb/_static/image42.png))

> [!NOTE]
> Aby zezwolić na dostęp do klasy lub metody wielu kontekstom zabezpieczeń, dekorować klasę lub metodę z atrybutem `PrincipalPermission` dla każdego kontekstu zabezpieczeń. Oznacza to, że aby umożliwić Tito i Bruce wykonywanie procedury obsługi zdarzeń `RowDeleting`, Dodaj *dwa* `PrincipalPermission` atrybuty:

[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

Oprócz stron ASP.NET wiele aplikacji ma również architekturę obejmującą różne warstwy, takie jak logika biznesowa i warstwy dostępu do danych. Te warstwy są zwykle implementowane jako biblioteki klas oraz klasy ofert i metody służące do wykonywania funkcji związanych z logiką biznesową i danymi. Atrybut `PrincipalPermission` jest przydatny do stosowania reguł autoryzacji do tych warstw.

Aby uzyskać więcej informacji na temat używania atrybutu `PrincipalPermission` do definiowania reguł autoryzacji dotyczącej klas i metod, zapoznaj się z wpisem w blogu [Scott Guthrie](https://weblogs.asp.net/scottgu/), [dodając reguły autoryzacji do warstw firmy i danych przy użyciu `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób stosowania reguł autoryzacji opartych na użytkownikach. Rozpocząłmy od ASP. Struktura autoryzacji adresów URL w sieci. W przypadku każdego żądania `UrlAuthorizationModule` aparatu ASP.NET sprawdza reguły autoryzacji adresów URL zdefiniowane w konfiguracji aplikacji, aby określić, czy tożsamość ma autoryzację dostępu do żądanego zasobu. W skrócie, Autoryzacja adresów URL ułatwia Określanie reguł autoryzacji dla określonej strony lub dla wszystkich stron w określonym katalogu.

Struktura autoryzacji adresów URL stosuje reguły autoryzacji w odniesieniu do strony. Przy autoryzacji adresu URL, tożsamość żądająca jest autoryzowana do uzyskania dostępu do określonego zasobu. Jednak wiele scenariuszy jest wywoływanych w celu uzyskania bardziej szczegółowych reguł autoryzacji. Zamiast definiować osoby, które mogą uzyskać dostęp do strony, może być konieczne umożliwienie wszystkim dostępu do strony, ale do wyświetlania różnych danych lub oferowania różnych funkcji w zależności od użytkownika odwiedzającego stronę. Autoryzacja na poziomie strony zazwyczaj obejmuje ukrywanie określonych elementów interfejsu użytkownika w celu uniemożliwienia nieautoryzowanym użytkownikom dostępu do zabronionych funkcji. Ponadto można użyć atrybutów, aby ograniczyć dostęp do klas i wykonywania metod dla określonych użytkowników.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Dodawanie reguł autoryzacji do warstw firmy i danych przy użyciu `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autoryzacja ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Zmiany między usług IIS 6 i zabezpieczeniami IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurowanie określonych plików i podkatalogów](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Ograniczanie funkcji modyfikacji danych na podstawie użytkownika](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [Przewodniki Szybki Start dotyczące kontrolek widoku logowania](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Zrozumienie autoryzacji adresu URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` dokumentacja techniczna](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Praca z danymi w ASP.NET 2,0](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](validating-user-credentials-against-the-membership-user-store-vb.md)
> [dalej](storing-additional-user-information-vb.md)

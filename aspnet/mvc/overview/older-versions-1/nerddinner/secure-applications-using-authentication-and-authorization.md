---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Zabezpieczanie aplikacji przy użyciu uwierzytelniania i autoryzacji | Microsoft Docs
author: microsoft
description: Krok 9 pokazuje, jak dodać uwierzytelnianie i autoryzację w celu zabezpieczenia naszej aplikacji NerdDinner, aby użytkownicy musieli zarejestrować się i zalogować się do witryny w celu utworzenia...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8d509c5f15bb4d5014e53b8dc2a736454238e72c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600983"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Zabezpieczanie aplikacji przy użyciu uwierzytelniania i autoryzacji

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 9 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 9 pokazuje, jak dodać uwierzytelnianie i autoryzację w celu zabezpieczenia naszej aplikacji NerdDinner, dzięki czemu użytkownicy muszą zarejestrować się i zalogować się do witryny w celu utworzenia nowych obiadów, a tylko użytkownik, który obsługuje obiad, może później edytować go.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner krok 9: uwierzytelnianie i autoryzacja

Teraz nasza aplikacja NerdDinner przyznaje każdej osobie odwiedzającej witrynę możliwość tworzenia i edytowania szczegółów każdego obiadu. Zmieńmy to tak, aby użytkownicy musieli zarejestrować się w witrynie i zalogować się do niej, aby utworzyć nowe uroczyste obiady, a następnie dodać ograniczenie, tak aby tylko użytkownik, który obsługuje obiad, mógł go później edytować.

Aby to umożliwić, użyjemy uwierzytelniania i autoryzacji do zabezpieczenia naszej aplikacji.

### <a name="understanding-authentication-and-authorization"></a>Informacje o uwierzytelnianiu i autoryzacji

*Uwierzytelnianie* to proces identyfikowania i weryfikowania tożsamości klienta uzyskującego dostęp do aplikacji. Po prostu zapoznaj się z tematem "kto" jest użytkownikiem końcowym podczas odwiedzania witryny sieci Web. ASP.NET obsługuje wiele sposobów uwierzytelniania użytkowników przeglądarki. W przypadku aplikacji internetowych sieci Web najczęściej używane podejście uwierzytelniania nazywa się "uwierzytelnianie formularzy". Uwierzytelnianie formularzy umożliwia deweloperowi tworzenie formularza logowania HTML w swojej aplikacji, a następnie Weryfikowanie nazwy użytkownika/hasła, które są przesyłane przez użytkownika końcowego do bazy danych lub innego magazynu poświadczeń haseł. Jeśli kombinacja nazwy użytkownika/hasła jest poprawna, programista może zadawać ASP.NET do wystawienia zaszyfrowanego pliku cookie protokołu HTTP w celu zidentyfikowania użytkownika w przyszłych żądaniach. Będziemy używać uwierzytelniania formularzy w naszej aplikacji NerdDinner.

*Autoryzacja* to proces ustalania, czy uwierzytelniony użytkownik ma uprawnienia dostępu do określonego adresu URL/zasobu lub wykonywania pewnych akcji. Na przykład w naszej aplikacji NerdDinner chcemy autoryzować, że tylko zalogowani użytkownicy mogą uzyskiwać dostęp do adresu URL */Dinners/Create* i tworzyć nowe obiady. Chcemy również dodać logikę autoryzacji, tak aby tylko użytkownik, który obsługuje obiad, mógł go edytować — i odmówić dostępu do edycji wszystkim innym użytkownikom.

### <a name="forms-authentication-and-the-accountcontroller"></a>Uwierzytelnianie formularzy i elementu AccountController

Domyślny szablon projektu programu Visual Studio dla ASP.NET MVC automatycznie włącza uwierzytelnianie formularzy podczas tworzenia nowych aplikacji ASP.NET MVC. Automatycznie dodaje wstępnie skompilowaną implementację strony logowania do projektu, dzięki czemu można łatwo zintegrować zabezpieczenia w obrębie lokacji.

Domyślna strona główna witryny. Master wyświetla link "Zaloguj się" w prawym górnym rogu witryny, gdy użytkownik uzyskuje do niej dostęp, nie jest uwierzytelniany:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Kliknięcie linku "Zaloguj się" spowoduje przejście użytkownika do adresu URL */Account/LogOn* :

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Odwiedzający, którzy nie zostali zarejestrowani, mogą to zrobić przez kliknięcie linku "Register" (Zarejestruj), który przeniesie je do adresu URL */Account/Register* i umożliwi im wprowadzanie szczegółowych informacji o koncie:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Kliknięcie przycisku "Zarejestruj" spowoduje utworzenie nowego użytkownika w ramach systemu członkostwa ASP.NET i uwierzytelnienie użytkownika w lokacji przy użyciu uwierzytelniania formularzy.

Gdy użytkownik jest zalogowany, witryna. Master zmieni w prawym górnym rogu strony, aby wyprowadził "Witamy [nazwa_użytkownika]!" komunikat i renderuje link "Wyloguj się" zamiast "Zaloguj się". Kliknięcie linku "Wyloguj się" wylogowuje użytkownika:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Powyższe funkcje logowania, wylogowywania i rejestracji są implementowane w klasie elementu AccountController, która została dodana do projektu przez program Visual Studio podczas tworzenia projektu. Interfejs użytkownika dla elementu AccountController jest implementowany przy użyciu szablonów widoku w katalogu \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

Klasa elementu AccountController używa systemu uwierzytelniania formularzy ASP.NET w celu wystawiania zaszyfrowanych plików cookie uwierzytelniania oraz interfejsu API członkostwa ASP.NET w celu przechowywania i weryfikowania nazw użytkowników/haseł. Interfejs API członkostwa ASP.NET jest rozszerzalny i umożliwia używanie dowolnego magazynu poświadczeń haseł. ASP.NET jest dostarczany z wbudowanymi implementacjami dostawców członkostwa, które przechowują nazwy użytkownika/hasła w bazie danych SQL lub w Active Directory.

Możemy skonfigurować dostawcę członkostwa, którego aplikacja NerdDinner powinna używać, otwierając plik "Web. config" w katalogu głównym projektu i szukając w nim sekcji &lt;Membership&gt;. Plik Web. config dodany podczas tworzenia projektu rejestruje dostawcę członkostwa SQL i konfiguruje go tak, aby korzystał z parametrów połączenia o nazwie "ApplicationServices" w celu określenia lokalizacji bazy danych.

Domyślne parametry połączenia "ApplicationServices" (określone w sekcji &lt;connectionStrings&gt; pliku Web. config) są skonfigurowane do korzystania z programu SQL Express. Wskazuje bazę danych SQL Express o nazwie "ASPNETDB. MDF "w katalogu" App\_Data "aplikacji. Jeśli ta baza danych nie istnieje przy pierwszym użyciu interfejsu API członkostwa w aplikacji, ASP.NET automatycznie utworzy bazę danych i zainicjuje w niej odpowiedni schemat bazy danych członkostwa:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Jeśli zamiast korzystania z programu SQL Express chciałem korzystać z pełnego SQL Server wystąpienia (lub nawiązać połączenie ze zdalną bazą danych), należy zaktualizować parametry połączenia "ApplicationServices" w pliku Web. config i upewnić się, że odpowiedni schemat członkostwa dodano do bazy danych, do której odwołuje się. Aby dodać odpowiedni schemat dla członkostwa i innych usług aplikacji ASP.NET do bazy danych, można uruchomić narzędzie "ASPNET\_regsql. exe" w katalogu \Windows\Microsoft.NET\Framework\v2.0.50727\.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autoryzowanie adresu URL/Dinners/Create przy użyciu filtru [autoryzować]

Nie musimy pisać żadnego kodu w celu włączenia bezpiecznego uwierzytelniania i implementacji zarządzania kontami dla aplikacji NerdDinner. Użytkownicy mogą rejestrować nowe konta przy użyciu naszej aplikacji, a także zalogować/wylogować witrynę.

Teraz możemy dodać logikę autoryzacji do aplikacji i użyć stanu uwierzytelniania oraz nazwy użytkownika osób odwiedzających, aby określić, co może i czego nie można wykonać w ramach lokacji. Zacznijmy od dodania logiki autoryzacji do metod akcji "Create" naszej klasy DinnersController. W każdym przypadku wymagane jest zalogowanie użytkowników uzyskujących dostęp do adresu URL */Dinners/Create* . Jeśli użytkownik nie jest zalogowany, przekierowuje je do strony logowania, aby mogli się zalogować.

Implementacja tej logiki jest łatwa w obsłudze. Wszystko, czego potrzebujemy do dodania atrybutu filtru [autoryzuje] do naszych metod akcji tworzenia, takich jak:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC obsługuje możliwość tworzenia "filtrów akcji", których można użyć do zaimplementowania logiki wielokrotnego użytku, która może być deklaratywnie stosowana do metod akcji. Filtr [Autoryzuj] jest jednym z wbudowanych filtrów akcji udostępnianych przez ASP.NET MVC i umożliwia deweloperom deklaratywne stosowanie reguł autoryzacji do metod akcji i klas kontrolerów.

Jeśli zastosowano bez parametrów (jak powyżej) filtr [Autoryzuj] wymusza, że użytkownik wykonujący żądanie metody akcji musi być zalogowany — i automatycznie przekieruje przeglądarkę do adresu URL logowania, jeśli nie. Podczas tego przekierowania pierwotnie żądany adres URL jest przesyłany jako argument QueryString (na przykład:/Account/LogOn? ReturnUrl =% 2fDinners% 2fCreate). Następnie elementu AccountController przekieruje użytkownika z powrotem do pierwotnie żądanego adresu URL po zalogowaniu się.

Filtr [Autoryzuj] opcjonalnie obsługuje możliwość określania właściwości "Users" lub "Roles", za pomocą której można wymagać, aby użytkownik zalogował się zarówno na liście dozwolonych użytkowników, jak i w ramach dozwolonej roli zabezpieczeń. Na przykład poniższy kod zezwala na dostęp do adresu URL/Dinners/Create tylko dwóm określonym użytkownikom, "scottgu" i "billg":

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Osadzenie określonych nazw użytkowników w kodzie może być całkiem niełatwa do utrzymania. Lepszym rozwiązaniem jest zdefiniowanie ról wyższego poziomu, które są sprawdzane przez kod, a następnie mapowania użytkowników do roli przy użyciu bazy danych lub systemu usługi Active Directory (dzięki włączeniu rzeczywistej listy mapowania użytkowników, która ma być przechowywana zewnętrznie z kodu). ASP.NET zawiera wbudowany interfejs API zarządzania rolami, a także Wbudowany zestaw dostawców ról (w tym dla SQL i Active Directory), które mogą ułatwić wykonywanie mapowania użytkownika/roli. Następnie możemy zaktualizować kod, aby umożliwić użytkownikom z określoną rolą "Administrator" dostęp do adresu URL/Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Używanie właściwości User.Identity.Name podczas tworzenia obiadów

Możemy pobrać nazwę użytkownika aktualnie zalogowanego użytkownika żądania przy użyciu właściwości User.Identity.Name uwidocznionej w klasie podstawowej kontrolera.

Wcześniej, gdy zaimplementowano wersję HTTP-POST metody akcji Create (), stałe Właściwość "HostedBy" obiadu do statycznego ciągu. Teraz możemy zaktualizować ten kod, aby użyć właściwości User.Identity.Name, a także automatycznie dodać RSVP dla hosta tworzącego obiad:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Ze względu na to, że dodaliśmy atrybut [autoryzuje] do metody Create (), ASP.NET MVC zapewnia, że metoda akcji jest wykonywana tylko wtedy, gdy użytkownik odwiedzający adres URL/Dinners/Create jest zalogowany w witrynie. W związku z tym wartość właściwości User.Identity.Name zawsze będzie zawierać prawidłową nazwę użytkownika.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Używanie właściwości User.Identity.Name podczas edytowania obiadów

Dodajmy teraz logikę autoryzacji, która ogranicza użytkowników, aby mogli edytować tylko te właściwości.

Aby Ci pomóc, najpierw dodamy metodę pomocnika "IsHostedBy (username)" do naszego obiektu obiadu (w ramach klasy częściowej Dinner.cs, która została wcześniej utworzona). Ta metoda pomocnika zwraca wartość PRAWDA lub FAŁSZ w zależności od tego, czy podana nazwa użytkownika jest zgodna z właściwością HostedBy obiadu, i hermetyzuje logikę niezbędną do wykonania porównania ciągów z uwzględnieniem wielkości liter:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Następnie dodamy atrybut [autoryzuje] do metod akcji Edit () w ramach naszej klasy DinnersController. Dzięki temu użytkownicy muszą być zalogowani w celu zażądania adresu URL */Dinners/Edit/[ID]* .

Następnie możemy dodać kod do naszych metod edycji wykorzystujących metodę pomocnika obiad. IsHostedBy (username), aby sprawdzić, czy zalogowany użytkownik jest zgodny z hostem obiadu. Jeśli użytkownik nie jest hostem, zostanie wyświetlony widok "InvalidOwner" i zakończenie żądania. Kod, aby to zrobić, wygląda jak poniżej:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Następnie można kliknąć prawym przyciskiem myszy katalog \Views\Dinners i wybrać polecenie menu Dodaj&gt;widok, aby utworzyć nowy widok "InvalidOwner". Zapełnimy go następującym komunikatem o błędzie:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

A teraz, gdy użytkownik próbuje edytować obiad, którego nie posiada, zostanie wyświetlony komunikat o błędzie:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Możemy powtórzyć te same kroki dla metod akcji Delete () w naszym kontrolerze, aby zablokować uprawnienia do usuwania obiadów, a także upewnić się, że tylko host na obiad może go usunąć.

### <a name="showinghiding-edit-and-delete-links"></a>Wyświetlanie/ukrywanie linków do edycji i usuwania

Łączymy się z metodą akcji Edytuj i Usuń dla naszej klasy DinnersController z naszego adresu URL szczegółów:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Obecnie wyświetlamy linki do akcji Edytuj i Usuń, niezależnie od tego, czy odwiedzający adres URL jest hostem obiadu. Zmieńmy to tak, aby linki były wyświetlane tylko wtedy, gdy użytkownik będący odwiedzający jest właścicielem obiadu.

Metoda akcji Details () w ramach DinnersController pobiera obiekt obiadu, a następnie przekazuje go jako obiekt modelu do naszego szablonu widoku:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Możemy zaktualizować nasz szablon widoku, aby warunkowo pokazać/ukryć linki Edytuj i Usuń za pomocą metody pomocnika obiad. IsHostedBy () podobnej do poniższego:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Następne kroki

Teraz przyjrzyjmy się, jak możemy umożliwić użytkownikom uwierzytelnionym korzystanie z usługi RSVP w przypadku obiadów przy użyciu technologii AJAX.

> [!div class="step-by-step"]
> [Poprzednie](implement-efficient-data-paging.md)
> [dalej](use-ajax-to-deliver-dynamic-updates.md)

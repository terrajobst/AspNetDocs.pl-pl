---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: Przypisywanie ról do użytkowników (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku utworzysz dwie strony ASP.NET, aby ułatwić zarządzanie użytkownikami należącymi do ról. Na pierwszej stronie znajdują się urządzenia, aby zobaczyć, co...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: 3346e47cf604ed1d4003ca83203116666e37cb1b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78571772"
---
# <a name="assigning-roles-to-users-c"></a>Przypisywanie ról do użytkowników (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> W tym samouczku utworzysz dwie strony ASP.NET, aby ułatwić zarządzanie użytkownikami należącymi do ról. Na pierwszej stronie znajdują się urządzenia, w których użytkownicy należą do danej roli, elementy, do których należy określony użytkownik, oraz możliwość przypisywania lub usuwania określonego użytkownika z danej roli. Na drugiej stronie zostanie rozszerzony formant formancie CreateUserWizard, dzięki czemu będzie on zawierał krok pozwalający określić role, do których należy nowo utworzony użytkownik. Jest to przydatne w scenariuszach, w których administrator może tworzyć nowe konta użytkowników.

## <a name="introduction"></a>Wprowadzenie

<a id="_msoanchor_1"> </a> [Poprzedni samouczek](creating-and-managing-roles-cs.md) sprawdził strukturę role i `SqlRoleProvider`znaleźliśmy, jak używać klasy `Roles` do tworzenia, pobierania i usuwania ról. Oprócz tworzenia i usuwania ról musimy mieć możliwość przypisywania lub usuwania użytkowników z roli. Niestety, usługa ASP.NET nie dostarcza żadnych kontrolek sieci Web do zarządzania użytkownikami należącymi do ról. Zamiast tego należy utworzyć własne strony ASP.NET, aby zarządzać tymi skojarzeniami. Dobra wiadomość polega na tym, że dodawanie i usuwanie użytkowników do ról jest bardzo proste. Klasa `Roles` zawiera kilka metod dodawania jednego lub kilku użytkowników do co najmniej jednej roli.

W tym samouczku utworzysz dwie strony ASP.NET, aby ułatwić zarządzanie użytkownikami należącymi do ról. Na pierwszej stronie znajdują się urządzenia, w których użytkownicy należą do danej roli, elementy, do których należy określony użytkownik, oraz możliwość przypisywania lub usuwania określonego użytkownika z danej roli. Na drugiej stronie zostanie rozszerzony formant formancie CreateUserWizard, dzięki czemu będzie on zawierał krok pozwalający określić role, do których należy nowo utworzony użytkownik. Jest to przydatne w scenariuszach, w których administrator może tworzyć nowe konta użytkowników.

Zacznijmy!

## <a name="listing-what-users-belong-to-what-roles"></a>Wyświetlanie informacji o tym, co użytkownicy należą do ról

Pierwszą kolejnością działalności w tym samouczku jest utworzenie strony sieci Web, z której użytkownicy mogą być przypisani do ról. Zanim będziemy wypróbujemy o sposobie przypisywania użytkowników do ról, najpierw koncentrują się na sposobach określania, którzy użytkownicy należą do ról. Istnieją dwa sposoby wyświetlania tych informacji: "według roli" lub "według użytkownika". Możemy zezwolić odwiedzającemu na wybranie roli, a następnie wyświetlenie wszystkich użytkowników należących do roli (wyświetlanie "według roli") lub monitowanie osoby odwiedzającej o wybranie użytkownika, a następnie wyświetlenie im ról przypisanych do tego użytkownika (wyświetlanych przez użytkownika).

Widok "według roli" jest przydatny w sytuacjach, gdy osoba odwiedzająca chce znać zestaw użytkowników należących do określonej roli; Widok "według użytkownika" jest idealnym rozwiązaniem, gdy osoba odwiedzająca musi znać określone role użytkownika. Na naszej stronie znajdują się interfejsy "według ról" i "według użytkownika".

Zaczniemy od utworzenia interfejsu "przez użytkownika". Ten interfejs będzie składać się z listy rozwijanej i listy pól wyboru. Lista rozwijana zostanie wypełniona zestawem użytkowników w systemie. pola wyboru wyliczą role. Wybranie użytkownika z listy rozwijanej spowoduje sprawdzenie ról, do których należy użytkownik. Osoba odwiedzająca stronę może zaznaczać lub usuwać zaznaczenie pól wyboru, aby dodać lub usunąć wybranego użytkownika z odpowiednich ról.

> [!NOTE]
> Używanie listy rozwijanej do wyświetlania kont użytkowników nie jest idealnym wyborem dla witryn sieci Web, w których może znajdować się setki kont użytkowników. Lista rozwijana została zaprojektowana tak, aby umożliwić użytkownikowi wybranie jednego elementu z stosunkowo krótkiej listy opcji. Szybko staje się nieporęczny, ponieważ liczba elementów listy rośnie. W przypadku tworzenia witryny sieci Web, która będzie miała potencjalnie dużą liczbę kont użytkowników, warto rozważyć użycie alternatywnego interfejsu użytkownika, takiego jak GridView z możliwością stronicowania lub interfejs z możliwością filtrowania, który wyświetla listę monitów odwiedzających o wybranie litery, a następnie tylko pokazuje użytkowników, których nazwa użytkownika rozpoczyna się od wybranej litery.

## <a name="step-1-building-the-by-user-user-interface"></a>Krok 1. Tworzenie interfejsu użytkownika "według użytkownika"

Otwórz stronę `UsersAndRoles.aspx`. W górnej części strony Dodaj kontrolkę sieci Web etykieta o nazwie `ActionStatus` i wyczyść jej Właściwość `Text`. Ta etykieta zostanie użyta w celu przekazywania informacji zwrotnych o wykonanych akcjach, wyświetlania komunikatów, takich jak "użytkownik Tito został dodany do roli administratorów" lub "użytkownik Jisun został usunięty z roli nadzoru". Aby te komunikaty były wychodzące, ustaw właściwość `CssClass` etykiety na "ważne".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Następnie Dodaj następującą definicję klasy CSS do arkusza stylów `Styles.css`:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Ta definicja CSS instruuje przeglądarkę, aby wyświetlała etykietę przy użyciu dużej, czerwonej czcionki. Rysunek 1 pokazuje ten efekt za pomocą projektanta programu Visual Studio.

[![Właściwość CssClass etykiety skutkuje dużą, czerwoną czcionką](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Rysunek 1**: Właściwość `CssClass` etykiety powoduje dużą, czerwoną czcionkę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image3.png))

Następnie Dodaj DropDownList do strony, ustaw jej Właściwość `ID` na `UserList`i ustaw dla jej właściwości `AutoPostBack` wartość true. Użyjemy tej DropDownList, aby wyświetlić listę wszystkich użytkowników w systemie. Ta DropDownList zostanie powiązana z kolekcją obiektów MembershipUser. Ponieważ chcemy, aby DropDownList wyświetlał Właściwość UserName obiektu MembershipUser (i użyć jej jako wartości elementów listy), ustaw właściwości `DataTextField` i `DataValueField` DropDownList na "UserName".

Poniżej DropDownList Dodaj wzmacniak o nazwie `UsersRoleList`. Ten wzmacniak wyświetli listę wszystkich ról w systemie jako serię pól wyboru. Zdefiniuj `ItemTemplate` wzmacniak przy użyciu następujących znaczników deklaratywnych:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

Znacznik `ItemTemplate` zawiera pojedynczy formant sieci Web CheckBox o nazwie `RoleCheckBox`. Właściwość `AutoPostBack` pola wyboru ma wartość true, a właściwość `Text` jest powiązana z `Container.DataItem`. Przyczyną jest to, że składnia DataBinding jest po prostu `Container.DataItem` jest, ponieważ struktura role zwraca listę nazw ról jako tablicę ciągów i jest to tablica ciągów, którą będziemy wiązać z Wzmacniaką. Dokładny opis dlaczego ta składnia jest używana do wyświetlania zawartości tablicy powiązanej z kontrolką sieci Web, wykracza poza zakres tego samouczka. Aby uzyskać więcej informacji na ten temat, zapoznaj się z tematem [Powiązywanie tablicy skalarnej z kontrolką sieci Web danych](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Na tym etapie deklaratywne znaczniki interfejsu "według użytkownika" powinny wyglądać podobnie do następujących:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Teraz można napisać kod, aby powiązać zestaw kont użytkowników z DropDownList i zestawem ról z Wzmacniaką. W klasie z kodem związanym ze stroną Dodaj metodę o nazwie `BindUsersToUserList` i inne nazwane `BindRolesList`, używając następującego kodu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

Metoda `BindUsersToUserList` pobiera wszystkie konta użytkowników w systemie za pomocą [metody`Membership.GetAllUsers`](https://msdn.microsoft.com/library/dy8swhya.aspx). Zwraca [obiekt`MembershipUserCollection`](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), który jest kolekcją [wystąpień`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Ta kolekcja jest następnie powiązana z `UserList` DropDownList. Wystąpienia `MembershipUser`, które korzeń kolekcje zawierają różne właściwości, takie jak `UserName`, `Email`, `CreationDate`i `IsOnline`. Aby nakazać DropDownList wyświetlić wartość właściwości `UserName`, upewnij się, że właściwości `DataTextField` `UserList` DropDownList i `DataValueField` zostały ustawione na "UserName".

> [!NOTE]
> Metoda `Membership.GetAllUsers` ma dwa przeciążenia: jeden, który nie akceptuje parametrów wejściowych i zwraca wszystkich użytkowników, a drugi, który przyjmuje wartości całkowite dla indeksu strony i rozmiaru strony, i zwraca tylko określony podzestaw użytkowników. Gdy w elemencie interfejsu użytkownika z możliwością stronicowania są wyświetlane duże ilości kont użytkowników, drugie Przeciążenie może być używane do bardziej wydajnej strony użytkowników, ponieważ zwraca tylko dokładny podzbiór kont użytkowników, a nie wszystkie z nich.

Metoda `BindRolesToList` zaczyna się od wywołania [metody`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)klasy `Roles`, która zwraca tablicę ciągów zawierającą role w systemie. Ta tablica ciągów jest następnie powiązana z Wzmacniaką.

Na koniec należy wywołać te dwie metody podczas pierwszego ładowania strony. Dodaj następujący kod do programu obsługi zdarzeń `Page_Load`:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Korzystając z tego kodu, poświęć chwilę na odwiedzenie strony za pomocą przeglądarki. ekran powinien wyglądać podobnie do rysunku 2. Wszystkie konta użytkowników są wypełniane na liście rozwijanej, a poniżej, każda rola jest wyświetlana jako pole wyboru. Ze względu na to, że ustawimy właściwości `AutoPostBack` DropDownList i CheckBoxs na true, zmiana wybranego użytkownika lub zaznaczenie lub cofnięcie zaznaczenia roli powoduje odświeżenie. Nie wykonano żadnej akcji, jednak ponieważ mamy jeszcze napisać kod, aby obsłużyć te akcje. Te zadania zostaną przedstawione w dwóch następnych sekcjach.

[![zostanie wyświetlona strona użytkownicy i role](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Rysunek 2**. na stronie są wyświetlane Użytkownicy i role ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](assigning-roles-to-users-cs/_static/image6.png))

### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Sprawdzanie ról, do których należy wybrany użytkownik

Gdy strona zostanie załadowana po raz pierwszy lub po wybraniu nowego użytkownika z listy rozwijanej, musimy zaktualizować pola wyboru `UsersRoleList`, aby dana rola pola wyboru została sprawdzona tylko wtedy, gdy wybrany użytkownik należy do tej roli. Aby to osiągnąć, należy utworzyć metodę o nazwie `CheckRolesForSelectedUser` przy użyciu następującego kodu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

Powyższy kod zaczyna się od określenia, kto jest wybranym użytkownikiem. Następnie używa [metody`GetRolesForUser(userName)`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) klasy role, aby zwrócić zestaw ról określonego użytkownika jako tablicę ciągów. Następnie są wyliczane elementy wzmacniania, a pole wyboru `RoleCheckBox` każdego elementu jest programowo przywoływane. Pole wyboru jest zaznaczone tylko wtedy, gdy rola, do której odnosi się, znajduje się w `selectedUsersRoles` tablicy ciągów.

> [!NOTE]
> Składnia `selectedUserRoles.Contains<string>(...)` nie zostanie skompilowana, jeśli używany jest program ASP.NET w wersji 2,0. Metoda `Contains<string>` jest częścią [biblioteki LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), która jest nowa dla ASP.NET 3,5. Jeśli nadal używasz programu ASP.NET w wersji 2,0, zamiast tego użyj [metody`Array.IndexOf<string>`](https://msdn.microsoft.com/library/eha9t187.aspx) .

Metoda `CheckRolesForSelectedUser` musi być wywoływana w dwóch przypadkach: podczas pierwszego ładowania strony i za każdym razem, gdy wybrany indeks `UserList` DropDownList jest zmieniany. W związku z tym należy wywołać tę metodę z programu obsługi zdarzeń `Page_Load` (po wywołaniach do `BindUsersToUserList` i `BindRolesToList`). Ponadto Utwórz procedurę obsługi zdarzeń dla zdarzenia `SelectedIndexChanged` DropDownList i Wywołaj tę metodę z tego miejsca.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Korzystając z tego kodu, można testować stronę za pomocą przeglądarki. Ponieważ jednak strona `UsersAndRoles.aspx` obecnie nie ma możliwości przypisywania użytkowników do ról, żaden użytkownik nie ma ról. Utworzymy interfejs do przypisywania użytkowników do ról na chwilę, więc możesz wykonać moje słowo, że ten kod działa i sprawdzić, czy jest to możliwe później, lub ręcznie dodać użytkowników do ról, wstawiając rekordy do tabeli `aspnet_UsersInRoles` w celu przetestowania tej funkcji teraz.

### <a name="assigning-and-removing-users-from-roles"></a>Przypisywanie i usuwanie użytkowników z ról

Gdy osoba odwiedzająca sprawdzi lub usunie zaznaczenie pola wyboru w naszym `UsersRoleList`, musimy dodać lub usunąć wybranego użytkownika z odpowiedniej roli. Właściwość `AutoPostBack` pola wyboru jest obecnie ustawiona na wartość true, co spowoduje, że w dowolnym momencie jest zaznaczone pole wyboru w tym obszarze, w którym jest sprawdzane lub niezaznaczone. Krótko mówiąc, musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `CheckChanged` pola wyboru. Ponieważ pole wyboru znajduje się w kontrolce wzmacniania, musimy ręcznie dodać instalację procedury obsługi zdarzeń. Zacznij od dodania procedury obsługi zdarzeń do klasy związanej z kodem jako metody `protected`, np.:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Będziemy ponownie pisać kod dla tego programu obsługi zdarzeń. Ale po pierwsze Przyjrzyjmy się obsłudze zdarzeń. Z pola wyboru w `ItemTemplate`ej wzmacniak Dodaj `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Ta składnia służy do nadrutu `RoleCheckBox_CheckChanged` obsługi zdarzeń do zdarzenia `CheckedChanged` `RoleCheckBox`.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

Ostatnim zadaniem jest ukończenie procedury obsługi zdarzeń `RoleCheckBox_CheckChanged`. Musimy zacząć od odwoływania się do kontrolki CheckBox, która wywołała zdarzenie, ponieważ to wystąpienie pola wyboru informuje nas o tym, jakie role zostało sprawdzone lub niezaznaczone za pośrednictwem jej `Text` i `Checked` właściwości. Korzystając z tych informacji oraz nazwy użytkownika wybranego użytkownika, dodajesz lub usuniesz użytkownika z roli za pośrednictwem metody [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) lub [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)klasy `Roles`.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

Powyższy kod jest uruchamiany przez program programowo odwołujący się do pola wyboru, które wywołało zdarzenie, które jest dostępne za pośrednictwem parametru wejściowego `sender`. Jeśli pole wyboru jest zaznaczone, wybrany użytkownik zostanie dodany do określonej roli, w przeciwnym razie zostaną usunięte z roli. W obu przypadkach etykieta `ActionStatus` wyświetla komunikat z podsumowaniem wykonywanej akcji.

Poświęć chwilę na przetestowanie tej strony za pomocą przeglądarki. Wybierz pozycję User Tito, a następnie Dodaj Tito do ról Administratorzy i kierownicy.

[![Tito został dodany do ról Administratorzy i kierownicy](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Rysunek 3**. Tito został dodany do ról Administratorzy i kierownicy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](assigning-roles-to-users-cs/_static/image9.png))

Następnie wybierz pozycję User Bruce (użytkownik) z listy rozwijanej. Istnieje ogłaszanie zwrotne i pola wyboru dla wzmacniania są aktualizowane za pośrednictwem `CheckRolesForSelectedUser`. Ponieważ Bruce nie należy jeszcze do żadnej roli, dwa pola wyboru nie są zaznaczone. Następnie Dodaj Bruce do roli kierownicy.

[![Bruce został dodany do roli kierownicy](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Rysunek 4**. Bruce został dodany do roli kierownicy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](assigning-roles-to-users-cs/_static/image12.png))

Aby kontynuować weryfikację funkcjonalności metody `CheckRolesForSelectedUser`, wybierz użytkownika innego niż Tito lub Bruce. Należy zauważyć, że pola wyboru są automatycznie niezaznaczone, co oznacza, że nie należą do żadnej roli. Wróć do Tito. Należy zaznaczyć pola wyboru Administratorzy i opiekunowie.

## <a name="step-2-building-the-by-roles-user-interface"></a>Krok 2. Tworzenie interfejsu użytkownika "według ról"

W tym momencie został ukończony interfejs "według użytkowników" i jest gotowy do rozpoczęcia pracy z interfejsem "według ról". Interfejs "według ról" poprosi użytkownika o wybranie roli z listy rozwijanej, a następnie wyświetlenie zestawu użytkowników należących do tej roli w widoku GridView.

Dodaj kolejną kontrolkę DropDownList do strony `UsersAndRoles.aspx`. Umieść ten element poniżej kontrolki wzmacniania, nadaj jej nazwę `RoleList`i ustaw dla jej właściwości `AutoPostBack` wartość true. Poniżej Dodaj widok GridView i nadaj mu nazwę `RolesUserList`. W tym widoku GridView zostaną wystawione użytkownicy, którzy należą do wybranej roli. Ustaw dla właściwości `AutoGenerateColumns` GridView wartość false, Dodaj TemplateField do kolekcji `Columns` siatki i ustaw jej Właściwość `HeaderText` na "Users". Zdefiniuj `ItemTemplate` TemplateField tak, aby wyświetlała wartość wyrażenia DataBinding `Container.DataItem` we właściwości `Text` etykiety o nazwie `UserNameLabel`.

Po dodaniu i skonfigurowaniu widoku GridView znaczniki deklaratywne interfejsu "według roli" powinny wyglądać podobnie do następujących:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Musimy wypełnić `RoleList` DropDownList z zestawem ról w systemie. W tym celu należy zaktualizować metodę `BindRolesToList` tak, aby tworzyła tablicę ciągów zwracaną przez metodę `Roles.GetAllRoles` do `RolesList` DropDownList (jak również wzmacniak `UsersRoleList`).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

Ostatnie dwa wiersze w `BindRolesToList` metodzie zostały dodane w celu powiązania zestawu ról z `RoleList` kontrolką DropDownList. Rysunek 5 przedstawia wynik końcowy, gdy jest wyświetlany za pomocą przeglądarki — lista rozwijana uzupełniona o role systemu.

[![role są wyświetlane w RoleList DropDownList](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Rysunek 5**. role są wyświetlane w `RoleList` DropDownList (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](assigning-roles-to-users-cs/_static/image15.png))

### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Wyświetlanie użytkowników należących do wybranej roli

Po pierwszym załadowaniu strony lub wybraniu nowej roli z `RoleList` DropDownList musi zostać wyświetlona lista użytkowników należących do tej roli w widoku GridView. Utwórz metodę o nazwie `DisplayUsersBelongingToRole` przy użyciu następującego kodu:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Ta metoda rozpoczyna się od pobrania wybranej roli z `RoleList` DropDownList. Następnie używa [metody`Roles.GetUsersInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) , aby pobrać tablicę ciągów nazw użytkowników należących do tej roli. Ta tablica jest następnie powiązana z `RolesUserList` GridView.

Ta metoda musi być wywoływana w dwóch przypadkach: podczas początkowego ładowania strony i gdy wybrana rola w `RoleList` DropDownList. W związku z tym zaktualizuj procedurę obsługi zdarzeń `Page_Load`, tak aby ta metoda była wywoływana po wywołaniu `CheckRolesForSelectedUser`. Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `SelectedIndexChanged` `RoleList`, a następnie Wywołaj tę metodę.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

W przypadku tego kodu w `RolesUserList` GridView powinien wyświetlać tych użytkowników, którzy należą do wybranej roli. Jak pokazano na rysunku 6, rola kierownicy składa się z dwóch członków: Bruce i Tito.

[![GridView wyświetla listę użytkowników należących do wybranej roli](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Ilustracja 6**. widok GridView wyświetla listę użytkowników należących do wybranej roli ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](assigning-roles-to-users-cs/_static/image18.png))

### <a name="removing-users-from-the-selected-role"></a>Usuwanie użytkowników z wybranej roli

Uzupełnimy widok `RolesUserList` GridView, aby zawierał kolumnę przycisków "Usuń". Kliknięcie przycisku "Usuń" dla określonego użytkownika spowoduje usunięcie ich z tej roli.

Zacznij od dodania pola Usuń pole do widoku GridView. Oznacz to pole jako po lewej stronie i zmień jego właściwość `DeleteText` z "Delete" (domyślnie) na "Remove".

[![dodać](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Rysunek 7**. Dodawanie przycisku "Usuń" do widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](assigning-roles-to-users-cs/_static/image21.png))

Po kliknięciu przycisku "Usuń" zostanie kliknięte ogłaszanie zwrotne i zostanie zgłoszone zdarzenie `RowDeleting` GridView. Musimy utworzyć procedurę obsługi zdarzeń dla tego zdarzenia i napisać kod, który usuwa użytkownika z wybranej roli. Utwórz procedurę obsługi zdarzeń, a następnie Dodaj następujący kod:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

Kod jest uruchamiany przez określenie wybranej nazwy roli. Następnie programowo odwołuje się do formantu `UserNameLabel` z wiersza, którego kliknięto przycisk "Usuń", aby określić nazwę użytkownika, który ma zostać usunięty. Użytkownik zostanie następnie usunięty z roli za pośrednictwem wywołania metody `Roles.RemoveUserFromRole`. `RolesUserList` GridView jest następnie odświeżane i wyświetlany jest komunikat za pośrednictwem kontrolki etykieta `ActionStatus`.

> [!NOTE]
> Przycisk "Usuń" nie wymaga żadnych potwierdzeń przed usunięciem użytkownika z roli. Zapraszam Cię do dodania dowolnego poziomu potwierdzenia użytkownika. Jednym z najprostszych sposobów potwierdzania akcji jest użycie okna dialogowego potwierdzenia po stronie klienta. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Dodawanie potwierdzenia po stronie klienta podczas usuwania](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

Na rysunku nr 8 przedstawiono stronę po usunięciu użytkownika Tito z grupy kierownicy.

[![Niemniej, Tito nie jest już opiekunem](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Rysunek 8**: Niemniej, Tito nie jest już nadzorcą ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image24.png))

### <a name="adding-new-users-to-the-selected-role"></a>Dodawanie nowych użytkowników do wybranej roli

Wraz z usunięciem użytkowników z wybranej roli osoba odwiedzająca Tę stronę powinna również dodać użytkownika do wybranej roli. Najlepszy interfejs do dodawania użytkownika do wybranej roli zależy od liczby kont użytkowników, które mają być używane. Jeśli witryna sieci Web będzie zawierała tylko kilka dziesiątych kont użytkowników, możesz użyć DropDownList tutaj. Jeśli istnieją tysiące kont użytkowników, należy dołączyć interfejs użytkownika, który umożliwia odwiedzanie strony za pomocą kont, wyszukiwanie określonego konta lub filtrowanie kont użytkowników w inny sposób.

Na tej stronie Przyjrzyjmy się bardzo prostemu interfejsowi, który działa niezależnie od liczby kont użytkowników w systemie. W takim przypadku użyjemy pola tekstowego z monitem osoby odwiedzającej o wpisanie nazwy użytkownika, który chce dodać do wybranej roli. Jeśli użytkownik o tej nazwie nie istnieje lub użytkownik jest już członkiem roli, zostanie wyświetlony komunikat w `ActionStatus` etykiecie. Jeśli jednak użytkownik istnieje i nie jest członkiem roli, dodamy je do roli i odświeży siatkę.

Dodaj pole tekstowe i przycisk poniżej widoku GridView. Ustaw `ID` pola tekstowego na `UserNameToAddToRole` i ustaw właściwości `ID` i `Text` przycisku, aby `AddUserToRoleButton` i "dodać użytkownika do roli".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń `Click` dla `AddUserToRoleButton` i Dodaj następujący kod:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

Większość kodu w programie obsługi zdarzeń `Click` wykonuje różne sprawdzenia poprawności. Zapewnia to, że osoba odwiedzająca podała nazwę użytkownika w polu tekstowym `UserNameToAddToRole`, że użytkownik istnieje w systemie i nie należy do wybranej roli. Jeśli którykolwiek z tych sprawdzeń nie powiedzie się, zostanie wyświetlony odpowiedni komunikat w `ActionStatus`, a program obsługi zdarzeń zostanie zakończony. Jeśli wszystkie testy zostaną zakończone, użytkownik zostanie dodany do roli za pośrednictwem metody `Roles.AddUserToRole`. Po wykonaniu tej czynności Właściwość `Text` TextBox jest wyczyszczona. widok GridView zostanie odświeżony, a w etykiecie `ActionStatus` zostanie wyświetlony komunikat informujący o pomyślnym dodaniu określonego użytkownika do wybranej roli.

> [!NOTE]
> Aby upewnić się, że określony użytkownik nie należy jeszcze do wybranej roli, używamy [metody`Roles.IsUserInRole(userName, roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), która zwraca wartość logiczną wskazującą, czy *Nazwa użytkownika* jest członkiem *roli role*. Ta metoda zostanie ponownie użyta w <a id="_msoanchor_2"> </a> [następnym samouczku](role-based-authorization-cs.md) , gdy będzie wyglądała Autoryzacja oparta na rolach.

Odwiedź stronę za pośrednictwem przeglądarki i wybierz rolę kierownicy z `RoleList` DropDownList. Spróbuj wprowadzić nieprawidłową nazwę użytkownika — powinien zostać wyświetlony komunikat z informacją, że użytkownik nie istnieje w systemie.

[![nie można dodać nieistniejącego użytkownika do roli](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Rysunek 9**: nie można dodać nieistniejącego użytkownika do roli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image27.png))

Teraz spróbuj dodać prawidłowego użytkownika. Przejdź dalej i Dodaj Tito do roli kierownicy.

[![Tito jest ponownie nadzorcą!](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Rysunek 10**: Tito jest ponownie nadzorcą!  ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](assigning-roles-to-users-cs/_static/image30.png))

## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Krok 3. Aktualizacja wielu interfejsów "według użytkownika" i "według roli"

Na stronie `UsersAndRoles.aspx` są dostępne dwa oddzielne interfejsy służące do zarządzania użytkownikami i rolami. Obecnie te dwa interfejsy działają niezależnie od siebie, więc możliwe jest, że zmiana wprowadzona w jednym interfejsie nie zostanie odzwierciedlona natychmiast w drugim. Załóżmy na przykład, że osoba odwiedzająca stronę wybierze rolę kierownicy z `RoleList` DropDownList, która zawiera listę Bruce i Tito jako członków. Następnie odwiedzający wybiera Tito z `UserList` DropDownList, który sprawdza, czy są to pola wyboru Administratorzy i opiekunowie w Wzmacniake `UsersRoleList`. Jeśli odwiedzający następnie usunie rolę kierownika z wzmacniak, Tito jest usuwana z roli kierownicy, ale ta modyfikacja nie zostanie odzwierciedlona w interfejsie "według roli". Widok GridView nadal będzie Tito jako członek roli kierownicy.

Aby rozwiązać ten problem, należy odświeżyć widok GridView za każdym razem, gdy rola jest zaznaczona lub nie została sprawdzona przez wzmacniak `UsersRoleList`. Podobnie musimy odświeżyć wzmacniak za każdym razem, gdy użytkownik zostanie usunięty lub dodany do roli z interfejsu "według roli".

Wzmacniak w interfejsie użytkownika jest odświeżany przez wywołanie metody `CheckRolesForSelectedUser`. Interfejs "według roli" można zmodyfikować w programie obsługi zdarzeń `RowDeleting` `RolesUserList` GridView i `Click` obsłudze zdarzenia `AddUserToRoleButton` przycisku. W związku z tym musimy wywołać metodę `CheckRolesForSelectedUser` z każdej z tych metod.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Podobnie widok GridView w interfejsie "według roli" jest odświeżany przez wywołanie metody `DisplayUsersBelongingToRole` i interfejsu "przez użytkownika" jest modyfikowany za pomocą programu obsługi zdarzeń `RoleCheckBox_CheckChanged`. W związku z tym musimy wywołać metodę `DisplayUsersBelongingToRole` z tej procedury obsługi zdarzeń.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Po wprowadzeniu tych drobnych zmian kodu interfejsy "według użytkownika" i "według roli" są teraz prawidłowo aktualizowane krzyżowo. Aby to sprawdzić, odwiedź stronę za pośrednictwem przeglądarki i wybierz odpowiednio Tito i nadzorców z `UserList` i `RoleList` kontrolek DropDownList. Należy pamiętać, że po usunięciu roli kierownicy dla Tito z Wzmacniakka w interfejsie użytkownika, Tito jest automatycznie usuwana z widoku GridView w interfejsie "według roli". Dodawanie Tito z powrotem do roli kierownicy z poziomu interfejsu "według roli" powoduje automatyczne ponowne sprawdzenie pola wyboru opiekunów w interfejsie użytkownika.

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Krok 4. dostosowanie formancie CreateUserWizard w celu uwzględnienia kroku "Określ role"

<a id="_msoanchor_3"> </a>W samouczku [*Tworzenie kont użytkowników*](../membership/creating-user-accounts-cs.md) pokazano, jak używać kontrolki sieci Web formancie CreateUserWizard w celu zapewnienia interfejsu do tworzenia nowego konta użytkownika. Formant formancie CreateUserWizard może być używany na jeden z dwóch sposobów:

- Oznacza to, że osoby odwiedzające mogą utworzyć własne konto użytkownika w lokacji i
- Jako środek do tworzenia nowych kont przez administratorów

W pierwszym przypadku użycia osoba odwiedzająca przechodzi do witryny i wypełnia formancie CreateUserWizard, wprowadzając informacje w celu zarejestrowania się w witrynie. W drugim przypadku administrator tworzy nowe konto dla innej osoby.

W przypadku tworzenia konta przez administratora dla innej osoby warto umożliwić administratorowi określenie ról, do których należy konto użytkownika. <a id="_msoanchor_4"> </a>W samouczku [ *przechowującym* *dodatkowe informacje o użytkowniku* ](../membership/storing-additional-user-information-cs.md) przedstawiono sposób dostosowywania formancie CreateUserWizard przez dodanie dodatkowych `WizardSteps`. Przyjrzyjmy się sposobom dodawania dodatkowego kroku do formancie CreateUserWizard w celu określenia ról nowego użytkownika.

Otwórz stronę `CreateUserWizardWithRoles.aspx` i Dodaj kontrolkę formancie CreateUserWizard o nazwie `RegisterUserWithRoles`. Ustaw właściwość `ContinueDestinationPageUrl` kontrolki na wartość "~/default.aspx". Ze względu na to, że administrator będzie używać tego formantu formancie CreateUserWizard do tworzenia nowych kont użytkowników, ustaw właściwość `LoginCreatedUser` kontrolki na wartość false. Ta właściwość `LoginCreatedUser` określa, czy użytkownik jest automatycznie zalogowany jako właśnie utworzony użytkownik, i domyślnie ma wartość true. Ustawimy ją na wartość false, ponieważ w przypadku utworzenia nowego konta przez administratora chcemy, aby był on zalogowany jako sam.

Następnie wybierz pozycję "Dodaj/Usuń `WizardSteps`..." z tagu inteligentnego formancie CreateUserWizard i Dodaj nowy `WizardStep`, ustawiając jego `ID` na `SpecifyRolesStep`. Przenieś `SpecifyRolesStep WizardStep` tak, aby był dostępny po kroku "Utwórz nowe konto", ale przed krokiem "Ukończ". Ustaw właściwość `Title` `WizardStep`na wartość "Określ role", jej Właściwość `StepType` na `Step`, a jej Właściwość `AllowReturn` na false.

[![dodać](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Ilustracja 11**. Dodawanie `WizardStep` "Określ role" do formancie CreateUserWizard ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image33.png))

Po wprowadzeniu tej zmiany deklaratywne znaczniki formancie CreateUserWizard powinny wyglądać następująco:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

W `WizardStep`"Określ role" Dodaj formant CheckBoxList o nazwie `RoleList`. Ta formant CheckBoxList wyświetli listę dostępnych ról, umożliwiając osobie odwiedzającej stronę przeszukanie ról, do których należy nowo utworzony użytkownik.

Pozostawiłem dwa zadania kodowania: najpierw należy wypełnić `RoleList` formant CheckBoxList z rolami w systemie. Po drugie należy dodać utworzonego użytkownika do wybranych ról, gdy użytkownik przejdzie z kroku "Określ role" do kroku "Complete". W programie obsługi zdarzeń `Page_Load` można wykonać pierwsze zadanie. Poniższy kod programowo odwołuje się do pola wyboru `RoleList` podczas pierwszej wizyty na stronie i wiąże z nim role w systemie.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

Powyższy kod powinien wyglądać znajomo. <a id="_msoanchor_5"> </a>W samouczku [ *przechowywanie* *dodatkowych informacji o użytkowniku* ](../membership/storing-additional-user-information-cs.md) użyto dwóch instrukcji `FindControl`, aby odwołać się do formantu sieci Web z poziomu `WizardStep`niestandardowego. Kod, który wiąże role z Formant CheckBoxList, został wykonany z wcześniej w tym samouczku.

Aby wykonać drugie zadanie programistyczne, musimy wiedzieć, że krok "Określ role" został ukończony. Odwołaj, że formancie CreateUserWizard ma zdarzenie `ActiveStepChanged`, które jest wyzwalane za każdym razem, gdy osoba odwiedzająca przechodzi z jednego kroku do drugiego. W tym miejscu możemy określić, czy użytkownik osiągnął krok "ukończony"; Jeśli tak, musimy dodać użytkownika do wybranych ról.

Utwórz procedurę obsługi zdarzeń dla zdarzenia `ActiveStepChanged` i Dodaj następujący kod:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Jeśli użytkownik już osiągnął krok "ukończony", program obsługi zdarzeń wylicza elementy `RoleList` formant CheckBoxList, a właśnie utworzony użytkownik zostanie przypisany do wybranych ról.

Odwiedź Tę stronę za pomocą przeglądarki. Pierwszym krokiem w formancie CreateUserWizard jest standardowy krok "Utwórz nowe konto", który będzie monitowany o nazwę użytkownika, hasło, adres e-mail i inne informacje o kluczu. Wprowadź informacje, aby utworzyć nowego użytkownika o nazwie Wanda.

[![utworzyć nowego użytkownika o nazwie Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Ilustracja 12**. Tworzenie nowego użytkownika o nazwie Wanda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image36.png))

Kliknij przycisk "Utwórz użytkownika". Formancie CreateUserWizard wewnętrznie wywołuje metodę `Membership.CreateUser`, tworząc nowe konto użytkownika, a następnie przechodzi do następnego kroku "Określ role". W tym miejscu są wyświetlane role systemowe. Zaznacz pole wyboru nadzorcy i kliknij przycisk Dalej.

[![uczyń Wanda członkiem roli kierownicy](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Ilustracja 13**. Udostępnianie Wanda członkom roli kierownicy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image39.png))

Kliknięcie przycisku Dalej powoduje ogłoszenie zwrotne i aktualizację `ActiveStep` do kroku "Complete". W obsłudze zdarzeń `ActiveStepChanged` ostatnio utworzone konto użytkownika jest przypisane do roli kierownicy. Aby to sprawdzić, Wróć do strony `UsersAndRoles.aspx` i wybierz pozycję kierownicy z `RoleList` DropDownList. Jak pokazano na rysunku 14, kierownicy składają się z trzech użytkowników: Bruce, Tito i Wanda.

[![Bruce, Tito i Wanda to wszyscy kierownicy](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Ilustracja 14**. Bruce, Tito i Wanda to wszyscy kierownicy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](assigning-roles-to-users-cs/_static/image42.png))

## <a name="summary"></a>Podsumowanie

Struktura ról oferuje metody pobierania informacji o rolach i metodach określonych użytkowników w celu określenia, którzy użytkownicy należą do określonej roli. Ponadto istnieje kilka metod dodawania i usuwania co najmniej jednego użytkownika do co najmniej jednej roli. W tym samouczku koncentrujemy się na dwóch następujących metodach: `AddUserToRole` i `RemoveUserFromRole`. Istnieją dodatkowe warianty umożliwiające dodanie wielu użytkowników do jednej roli i przypisanie wielu ról do jednego użytkownika.

W tym samouczku zawarto również przyjrzeć się rozszerzaniu formantu formancie CreateUserWizard w celu uwzględnienia `WizardStep`, aby określić nowo utworzone role użytkownika. Taki krok może pomóc administratorowi uprościć proces tworzenia kont użytkowników dla nowych użytkowników.

W tym momencie dowiesz się, jak tworzyć i usuwać role oraz jak dodawać i usuwać użytkowników z ról. Jednak zapoznaj się z zastosowaniem autoryzacji opartej na rolach. <a id="_msoanchor_6"> </a>W [poniższym samouczku](role-based-authorization-cs.md) opisano definiowanie reguł autoryzacji adresów URL na podstawie ról, a także Ograniczanie funkcjonalności na poziomie strony na podstawie aktualnie zalogowanych ról użytkownika.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Omówienie narzędzia administracyjnego witryny sieci Web ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Badanie ASP. Członkostwo, role i profil w sieci](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Stopniowa narzędzie do administrowania witryną sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-and-managing-roles-cs.md)
> [dalej](role-based-authorization-cs.md)

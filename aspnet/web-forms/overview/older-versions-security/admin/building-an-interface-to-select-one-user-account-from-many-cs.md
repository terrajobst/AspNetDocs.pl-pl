---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: Kompilowanie interfejsu w celu wybrania jednego konta użytkownika zC#wielu () | Microsoft Docs
author: rick-anderson
description: W tym samouczku utworzymy interfejs użytkownika ze stroną, z możliwością filtrowania. W szczególności nasz interfejs użytkownika będzie składać się z serii LinkButtons dla...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 8057cfbcd33c74376076363bc27940cebd522c08
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575999"
---
# <a name="building-an-interface-to-select-one-user-account-from-many-c"></a>Tworzenie interfejsu służącego do wybierania jednego konta użytkownika spośród wielu (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> W tym samouczku utworzymy interfejs użytkownika ze stroną, z możliwością filtrowania. W szczególności nasz interfejs użytkownika będzie składać się z serii LinkButtons do filtrowania wyników w oparciu o początkową literę nazwy użytkownika oraz kontrolkę GridView pokazującą pasujących użytkowników. Zaczniemy od wymienienia wszystkich kont użytkowników w widoku GridView. Następnie w kroku 3 zostanie dodany filtr LinkButtons. Krok 4 wygląda na stronicowanie filtrowanych wyników. Interfejs zbudowany w krokach od 2 do 4 zostanie użyty w kolejnych samouczkach do wykonywania zadań administracyjnych dla danego konta użytkownika.

## <a name="introduction"></a>Wprowadzenie

<a id="_msoanchor_1"> </a>W samouczku [*Przypisywanie ról do użytkowników*](../roles/assigning-roles-to-users-cs.md) został utworzony interfejs podstawowe dla administratora, aby wybrać użytkownika i zarządzać jego rolami. W specjalnym interfejsie przedstawiono administratora z listą rozwijaną wszystkich użytkowników. Taki interfejs jest odpowiedni w przypadku, gdy istnieją konta użytkowników, ale jest to nieporęczny dla witryn z setkami lub tysiącami kont. Siatka z możliwością filtrowania jest bardziej odpowiednim interfejsem użytkownika dla witryn sieci Web z dużymi bazami użytkowników.

W tym samouczku utworzymy taki interfejs użytkownika. W szczególności nasz interfejs użytkownika będzie składać się z serii LinkButtons do filtrowania wyników w oparciu o początkową literę nazwy użytkownika oraz kontrolkę GridView pokazującą pasujących użytkowników. Zaczniemy od wymienienia wszystkich kont użytkowników w widoku GridView. Następnie w kroku 3 zostanie dodany filtr LinkButtons. Krok 4 wygląda na stronicowanie filtrowanych wyników. Interfejs zbudowany w krokach od 2 do 4 zostanie użyty w kolejnych samouczkach do wykonywania zadań administracyjnych dla danego konta użytkownika.

Zacznijmy!

## <a name="step-1-adding-new-aspnet-pages"></a>Krok 1. Dodawanie nowych stron ASP.NET

W tym samouczku i następnych dwóch będziemy przeanalizować różne funkcje i możliwości związane z administracją. Potrzebujemy serii stron ASP.NET, aby zaimplementować tematy zbadane w ramach tych samouczków. Utwórzmy te strony i zaktualizujesz mapę witryny.

Zacznij od utworzenia nowego folderu w projekcie o nazwie `Administration`. Następnie Dodaj dwie nowe strony ASP.NET do folderu, łącząc każdą stronę ze stroną wzorcową `Site.master`. Nazwij strony:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Należy również dodać dwie strony do katalogu głównego witryny sieci Web: `ChangePassword.aspx` i `RecoverPassword.aspx`.

Te cztery strony powinny, w tym momencie, mieć dwie kontrolki zawartości, jedną dla każdej strony głównej: `MainContent` i `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

Chcemy wyświetlić domyślne znaczniki strony głównej dla `LoginContent` ContentPlaceHolder dla tych stron. W związku z tym usuń znaczniki deklaratywne dla kontrolki zawartości `Content2`. Po wykonaniu tej czynności znacznik strony powinien zawierać tylko jedną kontrolkę zawartości.

Strony ASP.NET w folderze `Administration` są przeznaczone wyłącznie dla użytkowników administracyjnych. Dodaliśmy rolę administratorów do systemu w <a id="_msoanchor_2"> </a>samouczku [*Tworzenie ról i zarządzanie nimi*](../roles/creating-and-managing-roles-cs.md) ; Ogranicz dostęp do tych dwóch stron do tej roli. W tym celu Dodaj plik `Web.config` do folderu `Administration` i skonfiguruj jego element `<authorization>`, aby przyznać użytkownikom dostęp do roli Administratorzy i odmówić wszystkim innym osobom.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

W tym momencie Eksplorator rozwiązań projektu powinien wyglądać podobnie do zrzutu ekranu pokazanego na rysunku 1.

[![cztery nowe strony, a plik Web. config został dodany do witryny sieci Web](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**Rysunek 1**. do witryny sieci Web dodano cztery nowe strony i plik `Web.config` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))

Na koniec Zaktualizuj mapę witryny (`Web.sitemap`), aby uwzględnić wpis na stronie `ManageUsers.aspx`. Dodaj następujący kod XML po `<siteMapNode>` dodanej dla samouczków ról.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

Po zaktualizowaniu mapy witryny odwiedź witrynę za pomocą przeglądarki. Jak pokazano na rysunku 2, Nawigacja po lewej stronie zawiera teraz elementy samouczków administracyjnych.

[![mapa witryny zawiera węzeł Administracja użytkownikami](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**Rysunek 2**. Mapa witryny zawiera węzeł Administracja użytkownikami ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))

## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Krok 2. Wyświetlanie listy wszystkich kont użytkowników w widoku GridView

Naszym celem tego samouczka jest utworzenie stronicowej siatki z możliwością filtrowania, za pomocą której administrator może wybrać konto użytkownika do zarządzania. Zacznijmy od wymienienia *wszystkich* użytkowników w widoku GridView. Po zakończeniu tej czynności dodamy interfejsy filtrowania i stronicowania oraz funkcje.

Otwórz stronę `ManageUsers.aspx` w folderze `Administration` i Dodaj widok GridView, ustawiając `ID` na `UserAccounts`. W tej chwili będziemy pisać kod w celu powiązania zestawu kont użytkowników z elementem GridView przy użyciu metody `GetAllUsers` klasy `Membership`. Zgodnie z opisem we wcześniejszych samouczkach Metoda GetAllUsers zwraca obiekt `MembershipUserCollection`, który jest kolekcją obiektów `MembershipUser`. Każda `MembershipUser` w kolekcji zawiera właściwości, takie jak `UserName`, `Email`, `IsApproved`i tak dalej.

Aby wyświetlić informacje o żądanym koncie użytkownika w widoku GridView, ustaw dla właściwości `AutoGenerateColumns` GridView wartość false i Dodaj BoundFields dla właściwości `UserName`, `Email`i `Comment` oraz CheckBoxFields dla właściwości `IsApproved`, `IsLockedOut`i `IsOnline`. Tę konfigurację można zastosować za pośrednictwem znaczników deklaratywnych kontrolki lub okna dialogowego pola. Rysunek 3 przedstawia zrzut ekranu okna dialogowego pola po usunięciu zaznaczenia pola wyboru Automatyczne generowanie pól, a BoundFields i CheckBoxFields zostały dodane i skonfigurowane.

[![dodać trzy BoundFields i trzy CheckBoxFields do widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**Rysunek 3**. Dodaj trzy BoundFields i trzy CheckBoxFields do widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))

Po skonfigurowaniu widoku GridView upewnij się, że jego deklaracyjne znaczniki są podobne do następujących:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

Następnie musimy napisać kod, który wiąże konta użytkowników z GridView. Utwórz metodę o nazwie `BindUserAccounts`, aby wykonać to zadanie, a następnie Wywołaj ją z programu obsługi zdarzeń `Page_Load` na pierwszej stronie odwiedzania.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

Poświęć chwilę na przetestowanie strony za pomocą przeglądarki. Jak pokazano na rysunku 4, `UserAccounts` GridView wyświetla nazwę użytkownika, adres e-mail i inne odpowiednie informacje dotyczące konta dla wszystkich użytkowników w systemie.

[![konta użytkowników są wyświetlane w widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**Ilustracja 4**. konta użytkowników są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))

## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Krok 3. filtrowanie wyników według pierwszej litery nazwy użytkownika

Obecnie `UserAccounts` GridView pokazuje *wszystkie* konta użytkowników. W przypadku witryn sieci Web mających setki lub tysiące kont użytkowników konieczne jest, aby użytkownik mógł szybko dostosowanie wyświetlone konta. Można to osiągnąć przez dodanie LinkButtons filtrowania do strony. Dodajmy 27 LinkButtons do strony: jedna zatytułowana wszystkie wraz z jedną element linkbuttoną dla każdej litery alfabetu. Jeśli osoba odwiedzająca kliknie element LinkButton wszystkie, zobaczysz wszystkich użytkowników. Kliknięcie określonej litery spowoduje wyświetlenie tylko tych użytkowników, których nazwa użytkownika rozpoczyna się od wybranej litery.

Pierwsze zadanie polega na dodaniu 27 element LinkButton kontrolek. Jedną z opcji jest możliwość jednokrotnego utworzenia 27 LinkButtons. Bardziej elastyczne podejście polega na użyciu kontrolki wzmacniak z `ItemTemplate`, która renderuje element element LinkButton, a następnie tworzy powiązanie opcji filtrowania z wzmacniak jako tablicą `string`ową.

Zacznij od dodania kontrolki wzmacniak do strony powyżej `UserAccounts` GridView. Ustaw właściwość `ID`ka wzmacniak na `FilteringUI`. Skonfiguruj szablony wzmacniania, tak aby `ItemTemplate` renderuje element element LinkButton, którego właściwości `Text` i `CommandName` są powiązane z bieżącym elementem tablicy. Jak widać w <a id="_msoanchor_3"> </a>samouczku [*Przypisywanie ról do użytkowników*](../roles/assigning-roles-to-users-cs.md) , można to zrobić za pomocą składni wiązania danych `Container.DataItem`. Użyj `SeparatorTemplate`ka, aby wyświetlić linię pionową między poszczególnymi łączami.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

Aby wypełnić ten wzmacniak przy użyciu żądanych opcji filtrowania, Utwórz metodę o nazwie `BindFilteringUI`. Należy pamiętać, aby wywołać tę metodę z programu obsługi zdarzeń `Page_Load` podczas pierwszego ładowania strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

Ta metoda określa opcje filtrowania jako elementy w `filterOptions`tablicy `string`. Dla każdego elementu w tablicy, wzmacniak będzie renderować element LinkButton z `Text` i `CommandName` właściwości przypisanych do wartości elementu tablicy.

Rysunek 5 przedstawia stronę `ManageUsers.aspx` wyświetlaną w przeglądarce.

[![wzmacniak list 27 LinkButtons filtrowania](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**Rysunek 5**. wzmacniak zawiera 27 LinkButtons filtrowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))

> [!NOTE]
> Nazwy użytkowników mogą rozpoczynać się od dowolnego znaku, w tym cyfry i znaki interpunkcyjne. Aby można było wyświetlić te konta, administrator będzie musiał użyć opcji wszystkie element LinkButton. Alternatywnie można dodać element LinkButton w celu zwrócenia wszystkich kont użytkowników, które zaczynają się od liczby. Jestem tym ćwiczeniem dla czytnika.

Kliknięcie dowolnego z LinkButtons filtrowania powoduje odświeżenie i wygenerowanie zdarzenia `ItemCommand`ka, ale nie ma żadnych zmian w siatce, ponieważ mamy jeszcze napisać kod, aby przefiltrować wyniki. Klasa `Membership` obejmuje [metodę`FindUsersByName`](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) , która zwraca te konta użytkowników, których nazwa użytkownika jest zgodna z określonym wzorcem wyszukiwania. Za pomocą tej metody można pobrać tylko te konta użytkowników, których nazwy użytkownika zaczynają się od litery określonej przez `CommandName` element LinkButton filtrowanego.

Zacznij od zaktualizowania klasy `ManageUser.aspx` stronie powiązanej z kodem, aby zawierała właściwość o nazwie `UsernameToMatch`. Ta właściwość utrzymuje ciąg filtru nazwy użytkownika na stronach ogłaszania zwrotnego:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

Właściwość `UsernameToMatch` przechowuje jej wartość przypisaną do kolekcji `ViewState` przy użyciu usługi Key UsernameToMatch. Gdy wartość tej właściwości jest odczytywana, sprawdza, czy wartość istnieje w kolekcji `ViewState`. Jeśli nie, zwraca wartość domyślną, pusty ciąg. Właściwość `UsernameToMatch` wykazuje wspólny wzorzec, w tym utrwalanie wartości w celu wyświetlenia stanu, tak aby wszelkie zmiany właściwości były utrwalane na stronach ogłaszania zwrotnego. Aby uzyskać więcej informacji na temat tego wzorca, przeczytaj artykuł [Informacje o stanie widoku ASP.NET](https://msdn.microsoft.com/library/ms972976.aspx).

Następnie zaktualizuj metodę `BindUserAccounts`, tak aby zamiast wywoływania `Membership.GetAllUsers`, wywoła `Membership.FindUsersByName`, przekazując wartość właściwości `UsernameToMatch` dołączoną do symbolu wieloznacznego SQL,%.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

Aby wyświetlić tylko tych użytkowników, których nazwa użytkownika rozpoczyna się od litery A, ustaw właściwość `UsernameToMatch` na a następnie Wywołaj `BindUserAccounts`. Spowoduje to wywołanie `Membership.FindUsersByName("A%")`, które zwróci wszystkich użytkowników, których nazwa użytkownika rozpoczyna się od A. podobnie, aby zwrócić *wszystkich* użytkowników, przypisać pusty ciąg do właściwości `UsernameToMatch`, tak aby Metoda `BindUserAccounts` wywoływała `Membership.FindUsersByName("%")`, co spowoduje zwrócenie wszystkich kont użytkowników.

Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemCommand` wzmacniak. To zdarzenie jest wywoływane za każdym razem, gdy zostanie kliknięty jeden z LinkButtons filtrów; została przeniesiona wartość `CommandName` klikniętej element LinkButton za pomocą obiektu `RepeaterCommandEventArgs`. Musimy przypisać odpowiednią wartość do właściwości `UsernameToMatch`, a następnie wywołać metodę `BindUserAccounts`. Jeśli `CommandName` to wszystko, przypisz pusty ciąg do `UsernameToMatch`, aby wyświetlić wszystkie konta użytkowników. W przeciwnym razie Przypisz wartość `CommandName` do `UsernameToMatch`.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

Korzystając z tego kodu, Przetestuj funkcję filtrowania. Po pierwszym odwiedzeniu strony wyświetlane są wszystkie konta użytkowników (zapoznaj się z powrotem do rysunku 5). Kliknięcie element LinkButton powoduje odświeżenie i filtrowanie wyników, wyświetlając tylko te konta użytkowników, które zaczynają się od.

[![użyć filtru LinkButtons do wyświetlania tych użytkowników, których nazwa użytkownika rozpoczyna się od określonej litery](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**Ilustracja 6**. Używanie filtru LinkButtons do wyświetlania tych użytkowników, których nazwa użytkownika rozpoczyna się od określonej litery ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))

## <a name="step-4-updating-the-gridview-to-use-paging"></a>Krok 4. aktualizowanie widoku GridView do korzystania z stronicowania

Widok GridView przedstawiający figury 5 i 6 wyświetla wszystkie rekordy zwrócone przez metodę `FindUsersByName`. Jeśli istnieją setki lub tysiące kont użytkowników, może to prowadzić do przeciążenia informacji podczas wyświetlania wszystkich kont (podobnie jak w przypadku kliknięcia opcji wszystkie element LinkButton lub po wstępnym odwiedzeniu strony). Aby pomóc w zaprezentowaniu kont użytkowników w bardziej zarządzanym fragmencie, Skonfigurujmy widok GridView, aby wyświetlać 10 kont użytkowników jednocześnie.

Formant GridView oferuje dwa typy stronicowania:

- **Domyślne stronicowanie** — łatwe do wdrożenia, ale niewydajne. W Nutshell z domyślnym stronicowaniem widok GridView oczekuje *wszystkich* rekordów ze źródła danych. Następnie wyświetlana jest tylko odpowiednia strona rekordów.
- **Niestandardowe stronicowanie** — wymaga więcej pracy do zaimplementowania, ale jest bardziej wydajne niż domyślne stronicowanie ze względu na stronicowanie niestandardowe źródło danych zwraca tylko dokładny zestaw rekordów do wyświetlenia.

Różnica wydajności między domyślnym i niestandardowym stronicowaniem może być dość istotna podczas stronicowania za pomocą tysięcy rekordów. Ponieważ tworzymy ten interfejs przy założeniu, że może istnieć setki lub tysiące kont użytkowników, użyjmy niestandardowego stronicowania.

> [!NOTE]
> Dokładniejsze Omówienie różnic między domyślnym i niestandardowym stronicowaniem, a także wyzwaniami związanymi z implementacją niestandardowego stronicowania można znaleźć w temacie [wydajne stronicowanie za pomocą dużych ilości danych](https://asp.net/learn/data-access/tutorial-25-cs.aspx). Aby uzyskać pewną analizę różnicy wydajności między domyślnym i niestandardowym stronicowaniem, zobacz [niestandardowe stronicowanie w ASP.NET z SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).

Aby zaimplementować niestandardowe stronicowanie, najpierw potrzebujemy pewnego mechanizmu, który umożliwia pobranie precyzyjnego podzbioru rekordów wyświetlanych przez widok GridView. Dobrym komunikatem jest to, że metoda `FindUsersByName` klasy `Membership` ma Przeciążenie, które pozwala określić indeks strony i rozmiar strony i zwraca tylko te konta użytkowników, które znajdują się w tym samym zakresie rekordów.

W szczególności to Przeciążenie ma następujący podpis: [`FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx).

Parametr *pageIndex* określa stronę kont użytkowników do zwrócenia; *PageSize* wskazuje liczbę rekordów, które mają być wyświetlane na stronie. Parametr *totalRecords* jest parametrem `out`, który zwraca liczbę wszystkich kont użytkowników w magazynie użytkownika.

> [!NOTE]
> Dane zwrócone przez `FindUsersByName` są sortowane według nazwy użytkownika; nie można dostosować kryteriów sortowania.

Widok GridView można skonfigurować tak, aby używał niestandardowego stronicowania, ale tylko wtedy, gdy jest on powiązany z kontrolką. Aby formant ObjectDataSource zaimplementował niestandardowe stronicowanie, wymaga dwóch metod: jednej, która ma przekazywać indeks wierszy początkowych i maksymalną liczbę rekordów do wyświetlenia, i zwraca dokładny podzestaw rekordów należących do tego zakresu; i Metoda zwracająca łączną liczbę rekordów, które są stronicowane przez. Przeciążenie `FindUsersByName` akceptuje indeks strony i rozmiar strony oraz zwraca łączną liczbę rekordów za pomocą `out` parametru. W związku z tym w tym miejscu występuje niezgodność interfejsu.

Jedną z opcji jest utworzenie klasy proxy, która uwidacznia interfejs, który jest oczekiwany przez element ObjectDataSource, a następnie wewnętrznie wywołuje metodę `FindUsersByName`. Inna opcja — i ta, która będzie używana w tym artykule — to Tworzenie własnego interfejsu stronicowania i używanie go zamiast wbudowanego interfejsu stronicowania GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Tworzenie pierwszego, poprzedniego, następnego, ostatniego stronicowania interfejsu

Utwórzmy interfejs stronicowania z pierwszym, poprzednim, następnym i ostatnim LinkButtons. Pierwszy element LinkButton, gdy zostanie kliknięty, przeniesie użytkownika do pierwszej strony danych, podczas gdy poprzedni spowoduje zwrócenie go do poprzedniej strony. Podobnie, następna i Ostatnia przeniesie użytkownika odpowiednio na następną i ostatnią stronę. Dodaj cztery element LinkButton kontrolki poniżej widoku GridView `UserAccounts`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

Następnie Utwórz procedurę obsługi zdarzeń dla każdego zdarzenia `Click` element LinkButton.

Rysunek 7 przedstawia cztery LinkButtons podczas wyświetlania za pomocą programu Visual Web Developer widok Projekt.

[![dodać pierwszy, poprzedni, następny i ostatni LinkButtons poniżej widoku GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**Rysunek 7**. Dodawanie pierwszych, poprzednich, następnych i ostatnich LinkButtons poniżej widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))

### <a name="keeping-track-of-the-current-page-index"></a>Śledzenie bieżącego indeksu strony

Gdy użytkownik po raz pierwszy odwiedzi stronę `ManageUsers.aspx` lub kliknie jeden z przycisków filtrowania, chcemy wyświetlić pierwszą stronę danych w widoku GridView. Gdy użytkownik kliknie jeden z LinkButtons nawigacyjnego, musi zaktualizować indeks strony. Aby zachować indeks strony i liczbę rekordów do wyświetlenia na stronie, Dodaj następujące dwie właściwości do klasy powiązanej z kodem strony:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

Podobnie jak w przypadku właściwości `UsernameToMatch` Właściwość `PageIndex` zachowuje jej wartość, aby wyświetlić stan. Właściwość `PageSize` tylko do odczytu zwraca zakodowaną wartość 10. Zapraszam czytelnika do zaktualizowania tej właściwości tak, aby korzystała z tego samego wzorca co `PageIndex`, a następnie do rozszerzenia strony `ManageUsers.aspx`, aby osoba odwiedzająca stronę mogli określić liczbę kont użytkowników, które mają być wyświetlane na stronie.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Pobieranie tylko rekordów bieżącej strony, aktualizowanie indeksu strony i włączanie i wyłączanie interfejsu stronicowania LinkButtons

Przy użyciu interfejsu stronicowania i dodanych właściwości `PageIndex` i `PageSize` są gotowe do aktualizacji metody `BindUserAccounts` tak, aby korzystała z odpowiedniego przeciążenia `FindUsersByName`. Ponadto musimy mieć tę metodę włączania lub wyłączania interfejsu stronicowania w zależności od wyświetlanej strony. Podczas przeglądania pierwszej strony danych, pierwsze i poprzednie łącza powinny być wyłączone; Następny i ostatni powinien być wyłączony podczas wyświetlania ostatniej strony.

Zaktualizuj metodę `BindUserAccounts` przy użyciu następującego kodu:

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

Należy zauważyć, że łączna liczba rekordów na stronie jest określana przez ostatni parametr metody `FindUsersByName`. Jest to parametr `out`, dlatego musimy najpierw zadeklarować zmienną do przechowywania tej wartości (`totalRecords`), a następnie prefiksu za pomocą słowa kluczowego `out`.

Po zwróceniu określonej strony kont użytkowników cztery LinkButtons są włączane lub wyłączane, w zależności od tego, czy jest wyświetlana pierwsza lub Ostatnia strona danych.

Ostatnim krokiem jest napisanie kodu dla czterech LinkButtons "`Click` obsługi zdarzeń. Te programy obsługi zdarzeń muszą zaktualizować Właściwość `PageIndex`, a następnie ponownie powiązać dane z elementem GridView za pośrednictwem wywołania `BindUserAccounts`. Pierwsze, poprzednie i następne programy obsługi zdarzeń są bardzo proste. Procedura obsługi zdarzeń `Click` dla ostatnich element LinkButton jest jednak nieco bardziej złożona, ponieważ musimy określić liczbę wyświetlanych rekordów, aby określić ostatni indeks strony.

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

Rysunki 8 i 9 przedstawiają niestandardowy interfejs stronicowania w akcji. Rysunek 8 przedstawia stronę `ManageUsers.aspx` podczas wyświetlania pierwszej strony danych dla wszystkich kont użytkowników. Należy zauważyć, że wyświetlane są tylko 10 z 13 kont. Kliknięcie następnego lub ostatniego linku powoduje ogłoszenie zwrotne, zaktualizowanie `PageIndex` do 1 i powiązanie drugiej strony kont użytkowników z siatką (patrz rysunek 9).

[![wyświetlane są pierwsze 10 kont użytkowników](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**Ilustracja 8**. wyświetlane są pierwsze 10 kont użytkowników ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))

[![kliknięciu następnego linku zostanie wyświetlona druga strona kont użytkowników](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**Ilustracja 9**. kliknięcie następnego linku powoduje wyświetlenie drugiej strony kont użytkowników ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))

## <a name="summary"></a>Podsumowanie

Administratorzy często muszą wybrać użytkownika z listy kont. W poprzednich samouczkach oglądanych przy użyciu listy rozwijanej wypełnionej użytkownikami, ale takie podejście nie jest dobrze skalowane. W tym samouczku opisano lepszą alternatywę: interfejs z możliwością filtrowania, którego wyniki są wyświetlane w widoku strony GridView. Za pomocą tego interfejsu użytkownika Administratorzy mogą szybko i wydajnie lokalizować i wybierać jedno konto użytkownika spośród tysięcy.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Niestandardowe stronicowanie w ASP.NET z SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Wydajne stronicowanie przy użyciu dużych ilości danych](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [Stopniowa narzędzie do administrowania witryną sieci Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Alicja Maziarz. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](recovering-and-changing-passwords-cs.md)

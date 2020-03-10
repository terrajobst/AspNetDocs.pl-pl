---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Przechowywanie dodatkowych informacji o użytkowniku (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku otrzymamy odpowiedź na to pytanie, tworząc bardzo podstawowe aplikację do Księgi Gości. W tym celu będziemy przeglądać różne opcje dla modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 24b96e86bc93e03d2639b73e35ed1fd1271bac5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634870"
---
# <a name="storing-additional-user-information-c"></a>Przechowywanie dodatkowych informacji dotyczących użytkowników (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> W tym samouczku otrzymamy odpowiedź na to pytanie, tworząc bardzo podstawowe aplikację do Księgi Gości. W tym celu będziemy przeglądać różne opcje modelowania informacji o użytkownikach w bazie danych, a następnie dowiedzieć się, jak skojarzyć te dane z kontami użytkowników utworzonymi przez platformę członkowską.

## <a name="introduction"></a>Wprowadzenie

ASP.NET. Struktura członkostwa w sieci oferuje elastyczny interfejs do zarządzania użytkownikami. Interfejs API członkostwa obejmuje metody sprawdzania poprawności poświadczeń, pobierania informacji o aktualnie zalogowanym użytkowniku, tworzeniu nowego konta użytkownika i usuwania konta użytkownika między innymi. Każde konto użytkownika w strukturze członkostwa zawiera tylko właściwości wymagane do weryfikacji poświadczeń i wykonywania podstawowych zadań związanych z kontem użytkownika. Jest to potwierdzone metodami i właściwościami [klasy`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), która modeluje konto użytkownika w środowisku członkostwa. Ta klasa ma właściwości, takie jak [`UserName`](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [`Email`](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)i [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)oraz metody, takie jak [`GetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) i [`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Często aplikacje muszą przechowywać dodatkowe informacje o użytkowniku, które nie są uwzględnione w strukturze członkostwa. Na przykład detaliczny handel online może wymagać podania przez każdego użytkownika adresu do wysyłki i rozliczeń, informacji o płatności, preferencji dostarczania oraz numeru telefonu kontaktu. Ponadto każde zamówienie w systemie jest skojarzone z określonym kontem użytkownika.

Klasa `MembershipUser` nie zawiera właściwości, takich jak `PhoneNumber` lub `DeliveryPreferences` lub `PastOrders`. Tak jak śledzimy informacje o użytkowniku, które są niezbędne przez aplikację i integrują się z platformą członkostwa? W tym samouczku otrzymamy odpowiedź na to pytanie, tworząc bardzo podstawowe aplikację do Księgi Gości. W tym celu będziemy przeglądać różne opcje modelowania informacji o użytkownikach w bazie danych, a następnie dowiedzieć się, jak skojarzyć te dane z kontami użytkowników utworzonymi przez platformę członkowską. Zacznijmy!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Krok 1. Tworzenie modelu danych aplikacji księgi gościa

Istnieją różne techniki, które mogą być stosowane do przechwytywania informacji o użytkownikach w bazie danych i kojarzenia ich z kontami użytkowników utworzonymi przez platformę członkostwa. Aby zilustrować te techniki, należy rozszerzyć aplikację sieci Web samouczka, aby przechwycić pewne dane dotyczące użytkownika. (Obecnie model danych aplikacji zawiera tylko tabele usług aplikacji wymaganych przez `SqlMembershipProvider`).

Utwórzmy bardzo prostą aplikację księgi gościa, w której uwierzytelniony użytkownik może opuścić komentarz. Oprócz zapisywania komentarzy w ramach księgi gościa Zezwól każdemu użytkownikowi na przechowywanie swojej miejscowości, strony głównej i podpisu. W przypadku podanej miejscowości, strony głównej i podpisu użytkownika pojawią się w każdym komunikacie, który pozostał w księdze gościa.

### <a name="adding-theguestbookcommentstable"></a>Dodawanie tabeli`GuestbookComments`

Aby można było przechwycić Komentarze księgi gościa, musimy utworzyć tabelę bazy danych o nazwie `GuestbookComments`, która ma kolumny takie jak `CommentId`, `Subject`, `Body`i `CommentDate`. Musimy również mieć każdy rekord w tabeli `GuestbookComments` odwołując się do użytkownika, który opuścił komentarz.

Aby dodać tę tabelę do bazy danych, przejdź do Eksplorator bazy danych w programie Visual Studio i przechodzenie do szczegółów `SecurityTutorials` bazie danych. Kliknij prawym przyciskiem myszy folder tabele i wybierz polecenie Dodaj nową tabelę. Spowoduje to wyświetlenie interfejsu, który umożliwia definiowanie kolumn dla nowej tabeli.

[![dodać nową tabelę do bazy danych SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Rysunek 1**. Dodawanie nowej tabeli do bazy danych `SecurityTutorials` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image3.png))

Następnie Zdefiniuj kolumny `GuestbookComments`. Zacznij od dodania kolumny o nazwie `CommentId` typu `uniqueidentifier`. Ta kolumna będzie jednoznacznie identyfikować każdy komentarz w księdze gościa, dlatego nie Zezwalaj `NULL` s i oznacz go jako klucz podstawowy tabeli. Zamiast podawania wartości dla `CommentId` pola na każdym `INSERT`, możemy wskazać, że dla tego `INSERT` pola należy automatycznie generować nową wartość `uniqueidentifier`, ustawiając wartość domyślną kolumny na `NEWID()`. Po dodaniu tego pierwszego pola, oznacz je jako klucz podstawowy i ustawienia jego wartości domyślnej, ekran powinien wyglądać podobnie do zrzutu ekranu pokazanego na rysunku 2.

[![dodać kolumny podstawowej o nazwie CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Rysunek 2**. Dodawanie kolumny podstawowej o nazwie `CommentId` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image6.png))

Następnie Dodaj kolumnę o nazwie `Subject` typu `nvarchar(50)` i kolumnę o nazwie `Body` `nvarchar(MAX)`, która nie zezwala na `NULL` s w obu kolumnach. Poniżej można dodać kolumnę o nazwie `CommentDate` typu `datetime`. Nie Zezwalaj `NULL` s i Ustaw domyślną wartość kolumny `CommentDate` na `getdate()`.

To wszystko, co ma na celu dodanie kolumny, która kojarzy konto użytkownika z każdym komentarzem w ramach księgi gościa. Jedną z opcji jest dodanie kolumny o nazwie `UserName` typu `nvarchar(256)`. Jest to odpowiedni wybór w przypadku korzystania z dostawcy członkostwa innego niż `SqlMembershipProvider`. Jednak w przypadku korzystania z `SqlMembershipProvider`, jak jesteśmy w tej serii samouczków, kolumna `UserName` w tabeli `aspnet_Users` nie gwarantuje unikalności. Klucz podstawowy tabeli `aspnet_Users` jest `UserId` i jest typu `uniqueidentifier`. W związku z tym tabela `GuestbookComments` wymaga kolumny o nazwie `UserId` typu `uniqueidentifier` (niedozwolone wartości `NULL`). Przejdź dalej i Dodaj tę kolumnę.

> [!NOTE]
> Zgodnie z opisem w samouczku [*Tworzenie schematu członkostwa w SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) , struktura członkostwa została zaprojektowana tak, aby umożliwić wielu aplikacjom sieci Web z różnymi kontami użytkowników współużytkować ten sam magazyn użytkownika. Robi to przez partycjonowanie kont użytkowników w różnych aplikacjach. Natomiast każda nazwa użytkownika musi być unikatowa w ramach aplikacji, ale ta sama nazwa użytkownika może być używana w różnych aplikacjach korzystających z tego samego magazynu użytkownika. Istnieje ograniczenie `UNIQUE` złożone w tabeli `aspnet_Users` w polach `UserName` i `ApplicationId`, ale nie tylko dla pola `UserName`. W związku z tym, istnieje możliwość, że tabela "ASPNET\_users" ma dwa (lub więcej) rekordy o tej samej wartości `UserName`. Istnieje jednak ograniczenie `UNIQUE` w polu `UserId` tabeli `aspnet_Users` (ponieważ jest kluczem podstawowym). Ograniczenie `UNIQUE` jest ważne, ponieważ bez nie możemy ustanowić ograniczenia klucza obcego między tabelami `GuestbookComments` i `aspnet_Users`.

Po dodaniu kolumny `UserId` Zapisz tabelę, klikając ikonę Zapisz na pasku narzędzi. Nazwij nową tabelę `GuestbookComments`.

Mamy do wzięcia udziału w `GuestbookComments` tabeli: należy utworzyć [ograniczenie klucza obcego](https://msdn.microsoft.com/library/ms175464.aspx) między kolumną `GuestbookComments.UserId` i kolumną `aspnet_Users.UserId`. Aby to osiągnąć, kliknij ikonę relacji na pasku narzędzi, aby uruchomić okno dialogowe relacje klucza obcego. (Możesz również uruchomić to okno dialogowe, przechodząc do menu projektanta tabel i wybierając pozycję relacje).

Kliknij przycisk Dodaj w lewym dolnym rogu okna dialogowego relacje klucza obcego. Spowoduje to dodanie nowego ograniczenia klucza obcego, chociaż nadal konieczne jest zdefiniowanie tabel, które uczestniczą w relacji.

[![użyć okna dialogowego relacje klucza obcego do zarządzania ograniczeniami klucza obcego tabeli](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Rysunek 3**. Korzystanie z okna dialogowego relacje klucza obcego do zarządzania ograniczeniami klucza obcego tabeli ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image9.png))

Następnie kliknij ikonę wielokropka w wierszu "specyfikacje tabeli i kolumny" po prawej stronie. Spowoduje to uruchomienie okna dialogowego tabele i kolumny, w którym można określić tabelę klucza podstawowego i kolumnę oraz kolumnę klucza obcego z tabeli `GuestbookComments`. W szczególności wybierz `aspnet_Users` i `UserId` jako tabelę i kolumnę klucza podstawowego i `UserId` z tabeli `GuestbookComments` jako kolumnę klucza obcego (patrz rysunek 4). Po zdefiniowaniu podstawowych i obcych tabel i kolumn, kliknij przycisk OK, aby powrócić do okna dialogowego relacje klucza obcego.

[![ustanowić ograniczenia klucza obcego między tabelami aspnet_Users i GuesbookComments](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Rysunek 4**. ustanowienie ograniczenia klucza obcego między tabelami `aspnet_Users` i `GuesbookComments` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image12.png))

W tym momencie ustanowiono ograniczenie klucza obcego. Obecność tego ograniczenia zapewnia [integralność relacyjną](http://en.wikipedia.org/wiki/Referential_integrity) między dwiema tabelami, gwarantując, że nigdy nie będzie wpisu księgi gościa odwołującego się do nieistniejącego konta użytkownika. Domyślnie ograniczenie klucza obcego nie zezwala na usunięcie rekordu nadrzędnego, jeśli istnieją odpowiednie rekordy podrzędne. Oznacza to, że jeśli użytkownik dokonuje co najmniej jednego komentarza w ramach księgi gościa, a następnie spróbujemy usunąć to konto użytkownika, usuwanie zakończy się niepowodzeniem, chyba że jego komentarze w księdze gościa zostaną najpierw usunięte.

Ograniczenia klucza obcego można skonfigurować tak, aby automatycznie usuwać skojarzone rekordy podrzędne po usunięciu rekordu nadrzędnego. Innymi słowy możemy skonfigurować to ograniczenie klucza obcego, aby wpisy księgi gości użytkownika były automatycznie usuwane po usunięciu jego konta użytkownika. Aby to osiągnąć, rozwiń sekcję "Wstawianie i aktualizowanie specyfikacji" i ustaw właściwość "Usuń regułę" na kaskadowo.

[![skonfigurować ograniczenie klucza obcego do usuwania kaskadowego](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Rysunek 5**. skonfigurowanie ograniczenia klucza obcego do usuwania kaskadowego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image15.png))

Aby zapisać ograniczenie klucza obcego, kliknij przycisk Zamknij, aby wyjść z relacji klucza obcego. Następnie kliknij ikonę Zapisz na pasku narzędzi, aby zapisać tabelę i tę relację.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Przechowywanie miejscowości, strony głównej i podpisu użytkownika

W tabeli `GuestbookComments` pokazano, jak przechowywać informacje, które współużytkują relację "jeden do wielu" z kontami użytkowników. Ponieważ każde konto użytkownika może mieć dowolną liczbę skojarzonych komentarzy, ta relacja jest modelowana przez utworzenie tabeli w celu przechowywania zestawu komentarzy, który zawiera kolumnę, która łączy ponownie każdy komentarz do określonego użytkownika. W przypadku korzystania z `SqlMembershipProvider`, ten link jest najlepszym rozwiązaniem przez utworzenie kolumny o nazwie `UserId` typu `uniqueidentifier` oraz ograniczenia klucza obcego między tą kolumną a `aspnet_Users.UserId`.

Teraz musimy skojarzyć trzy kolumny z każdym kontem użytkownika w celu przechowywania miejscowości, strony głównej i podpisu użytkownika, która będzie widoczna w komentarzach gościa. Istnieje wiele różnych sposobów osiągnięcia tego celu:

- <strong>Dodaj nowe kolumny do</strong> tabel<strong>`aspnet_Users`</strong> <strong>lub</strong> <strong>`aspnet_Membership`</strong> <strong>.</strong> Nie zalecamy tego podejścia, ponieważ modyfikuje Schemat używany przez `SqlMembershipProvider`. Ta decyzja może wrócić do Haunt podróży. Na przykład, co jeśli przyszła wersja programu ASP.NET używa innego schematu `SqlMembershipProvider`. Firma Microsoft może zawierać narzędzie do migrowania danych z ASP.NET 2,0 `SqlMembershipProvider` do nowego schematu, ale jeśli zmodyfikowano schemat ASP.NET 2,0 `SqlMembershipProvider`, taka Konwersja może nie być możliwa.

- **Użyj ASP. Struktura profilu sieci, Definiowanie właściwości profilu dla miejscowości, strony głównej i podpisu.** ASP.NET zawiera strukturę profilu, która jest przeznaczona do przechowywania dodatkowych danych specyficznych dla użytkownika. Podobnie jak w przypadku struktury członkostwa, Struktura profilu jest skompilowana korzystającego modelem dostawcy. .NET Framework jest dostarczany z `SqlProfileProvider` sthat przechowuje dane profilowe w SQL Server bazie danych. W rzeczywistości Nasza baza danych ma już tabelę używaną przez `SqlProfileProvider` (`aspnet_Profile`), ponieważ została dodana po dodaniu usług aplikacji z powrotem w <a id="_msoanchor_2"> </a>samouczku [*Tworzenie schematu członkostwa w SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) .   
  Główną zaletą platformy profilu jest to, że deweloperzy mogą definiować właściwości profilu w `Web.config` — nie trzeba pisać kodu w celu serializacji danych profilu do i z bazowego magazynu danych. Krótko mówiąc, jest niezwykle łatwe Definiowanie zestawu właściwości profilu i współdziałanie z nimi w kodzie. Jednak system profilu pozostawia dużo do pożądanego momentu, gdy przyjdzie do przechowywania wersji, więc jeśli masz aplikację, w której oczekuje się, że nowe właściwości specyficzne dla użytkownika zostaną dodane w późniejszym czasie lub istniejące do usunięcia lub zmodyfikowania, to platforma profilu nie może być  Najlepsza opcja. Ponadto `SqlProfileProvider` przechowuje właściwości profilu w sposób wysoce nieznormalizowany, co sprawia, że nie można uruchamiać zapytań bezpośrednio względem danych profilu (takich jak liczba użytkowników w domu New York).   
  Aby uzyskać więcej informacji na temat platformy profilu, zapoznaj się z sekcją "dalsze informacje" na końcu tego samouczka.

- <strong>Dodaj te trzy kolumny do nowej tabeli w bazie danych i Ustanów relację jeden do jednego między tą tabelą a</strong> <strong>`aspnet_Users`</strong> <strong>.</strong> Takie podejście obejmuje nieco więcej pracy niż w przypadku platformy profilu, ale oferuje maksymalną elastyczność w zakresie modelowania dodatkowych właściwości użytkownika w bazie danych. Jest to opcja, która zostanie użyta w tym samouczku.

Utworzymy nową tabelę o nazwie `UserProfiles`, aby zapisać miejscowość domu, stronę główną i podpis dla każdego użytkownika. Kliknij prawym przyciskiem myszy folder tabele w oknie Eksplorator bazy danych i wybierz polecenie Utwórz nową tabelę. Nadaj pierwszej kolumnie nazwę `UserId` i ustaw jej typ na `uniqueidentifier`. Nie Zezwalaj na wartości `NULL` i Oznacz kolumnę jako klucz podstawowy. Następnie Dodaj kolumny o nazwie: `HomeTown` typu `nvarchar(50)`; `HomepageUrl` typu `nvarchar(100)`; i sygnatura typu `nvarchar(500)`. Każda z tych trzech kolumn może przyjmować wartość `NULL`.

[![utworzyć tabelę profilów użytkownika](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Ilustracja 6**. tworzenie tabeli `UserProfiles` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image18.png))

Zapisz tabelę i nadaj jej nazwę `UserProfiles`. Na koniec Ustanów ograniczenie klucza obcego między polem `UserId` tabeli `UserProfiles` a polem `aspnet_Users.UserId`. Podobnie jak w przypadku ograniczenia klucza obcego między tabelami `GuestbookComments` i `aspnet_Users`, te ograniczenia są usuwane kaskadowo. Ponieważ `UserId` pole w `UserProfiles` jest kluczem podstawowym, gwarantuje to, że w tabeli `UserProfiles` nie będzie więcej niż jeden rekord dla każdego konta użytkownika. Ten typ relacji jest określany jako jeden-do-jednego.

Teraz, gdy mamy już utworzony model danych, jesteśmy gotowi do korzystania z niego. W krokach 2 i 3 zawiesz się, jak aktualnie zalogowany użytkownik może wyświetlać i edytować swoją miejscowość, stronę główną i informacje o podpisie. W kroku 4 utworzysz interfejs dla uwierzytelnionych użytkowników do przesyłania nowych komentarzy do księgi gości i wyświetlania istniejących.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Krok 2. Wyświetlanie miejscowości, strony głównej i podpisu użytkownika

Istnieją różne sposoby zezwalania aktualnie zalogowanemu użytkownikowi na wyświetlanie i edytowanie jego miejscowości, strony głównej i informacji o podpisie. Możemy ręcznie utworzyć interfejs użytkownika przy użyciu kontrolek TextBox i Label lub użyć jednej z formantów sieci Web danych, takich jak formant DetailsView. Aby wykonać `SELECT` bazy danych i instrukcje `UPDATE`, możemy napisać kod ADO.NET w klasie powiązanej z kodem lub, alternatywnie użyć podejścia deklaracyjnego z kontrolki SqlDataSource. Najlepiej, aby nasza aplikacja zawierała architekturę warstwową, którą można wywołać programowo z klasy powiązanej z kodem strony lub deklaratywnie za pośrednictwem kontrolki ObjectDataSource.

Ponieważ ta seria samouczków koncentruje się na uwierzytelnianiu formularzy, autoryzacji, kontach użytkowników i rolach, nie będzie dokładne omówienie tych różnych opcji dostępu do danych lub Dlaczego architektura warstwowa jest preferowana przez wykonywanie instrukcji SQL bezpośrednio na stronie ASP.NET. Przechodzenie przeze mnie przy użyciu widoku DetailsView i kontrolki SqlDataSource — najszybszą i najłatwiej — ale omówione koncepcje mogą być stosowane do alternatywnych formantów sieci Web i logiki dostępu do danych. Aby uzyskać więcej informacji na temat pracy z danymi w programie ASP.NET, zapoznaj się z artykułem my *[Work with Data in ASP.NET 2,0](../../data-access/index.md)* (seria samouczków).

Otwórz stronę `AdditionalUserInfo.aspx` w folderze `Membership` i Dodaj kontrolkę DetailsView do strony, ustawiając jej Właściwość `ID` na `UserProfile` i czyszcząc jej `Width` i `Height` właściwości. Rozwiń tag inteligentny DetailsView i wybierz, aby powiązać go z nową kontrolką źródła danych. Spowoduje to uruchomienie Kreatora konfiguracji źródła danych (patrz rysunek 7). W pierwszym kroku zostanie wyświetlony monit o określenie typu źródła danych. Ponieważ chcemy połączyć się bezpośrednio z bazą danych `SecurityTutorials`, wybierz ikonę bazy danych, określając `ID` jako `UserProfileDataSource`.

[![dodać nową kontrolkę kontrolki SqlDataSource o nazwie UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Rysunek 7**. Dodawanie nowej kontrolki kontrolki SqlDataSource o nazwie `UserProfileDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image21.png))

Na następnym ekranie zostanie wyświetlony komunikat z pytaniem o bazę danych do użycia. Zdefiniowano już parametry połączenia w `Web.config` dla bazy danych `SecurityTutorials`. Ta nazwa parametrów połączenia — `SecurityTutorialsConnectionString` — powinna znajdować się na liście rozwijanej. Wybierz tę opcję, a następnie kliknij przycisk Dalej.

[![wybierz pozycję SecurityTutorialsConnectionString z listy rozwijanej](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Ilustracja 8**. Wybierz `SecurityTutorialsConnectionString` z listy rozwijanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image24.png))

Na następnym ekranie poprosimy nas o określenie tabeli i kolumn do zapytania. Z listy rozwijanej wybierz tabelę `UserProfiles` i zaznacz wszystkie kolumny.

[![przywrócić wszystkich kolumn z tabeli UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Ilustracja 9**. przywrócenie wszystkich kolumn z tabeli `UserProfiles` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image27.png))

Bieżące zapytanie na rysunku 9 zwraca *wszystkie* rekordy w `UserProfiles`, ale chcemy tylko w rekordzie aktualnie zalogowanego użytkownika. Aby dodać klauzulę `WHERE`, kliknij przycisk `WHERE`, aby wyświetlić okno dialogowe Dodawanie klauzuli `WHERE` (patrz rysunek 10). W tym miejscu możesz wybrać kolumnę, według której ma być odfiltrowanie, operator i Źródło parametru filtru. Wybierz `UserId` jako kolumnę i "=" jako operatora.

Niestety, nie istnieje wbudowane Źródło parametrów do zwrócenia wartości `UserId` aktualnie zalogowanego użytkownika. Ta wartość będzie potrzebna programowo. W związku z tym Ustaw dla źródłowej listy rozwijanej wartość "Brak", kliknij przycisk Dodaj, aby dodać parametr, a następnie kliknij przycisk OK.

[![dodać parametru filtru w kolumnie UserId](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Ilustracja 10**. Dodawanie parametru filtru w kolumnie `UserId` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image30.png))

Po kliknięciu przycisku OK nastąpi powrót do ekranu pokazanego na rysunku 9. Tym razem kwerenda SQL w dolnej części ekranu powinna zawierać klauzulę `WHERE`. Kliknij przycisk Dalej, aby przejść do ekranu "test zapytania". W tym miejscu możesz uruchomić zapytanie i wyświetlić wyniki. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio tworzy formant kontrolki SqlDataSource na podstawie ustawień określonych w kreatorze. Ponadto ręcznie dodaje BoundFields do widoku DetailsView dla każdej kolumny zwróconej przez `SelectCommand`kontrolki SqlDataSource. Nie ma potrzeby wyświetlania pola `UserId` w widoku DetailsView, ponieważ użytkownik nie musi znać tej wartości. Można usunąć to pole bezpośrednio z znaczników deklaratywnych kontrolki DetailsView lub klikając łącze "Edytuj pola" z tagu inteligentnego.

W tym momencie znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Musimy programowo ustawić parametr `UserId` kontrolki kontrolki SqlDataSource na bieżąco zalogowanego użytkownika `UserId` przed wybraniem danych. Można to osiągnąć przez utworzenie programu obsługi zdarzeń dla zdarzenia `Selecting` kontrolki SqlDataSource i dodanie następującego kodu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Powyższy kod rozpocznie się, uzyskując odwołanie do aktualnie zalogowanego użytkownika, wywołując metodę `GetUser` klasy `Membership`. Zwraca obiekt `MembershipUser`, którego właściwość `ProviderUserKey` zawiera `UserId`. Wartość `UserId` jest następnie przypisywana do parametru `@UserId` kontrolki SqlDataSource.

> [!NOTE]
> Metoda `Membership.GetUser()` zwraca informacje o aktualnie zalogowanym użytkowniku. Jeśli użytkownik anonimowy zostanie odwiedzany na stronie, zwróci wartość `null`. W takim przypadku spowoduje to `NullReferenceException` w następującym wierszu kodu podczas próby odczytania właściwości `ProviderUserKey`. Oczywiście nie trzeba martwić się o `Membership.GetUser()` zwrócenie `null` wartości na stronie `AdditionalUserInfo.aspx`, ponieważ w poprzednim samouczku skonfigurowano autoryzację adresów URL, tak aby tylko uwierzytelnieni użytkownicy mieli dostęp do zasobów ASP.NET w tym folderze. Jeśli chcesz uzyskać dostęp do informacji o aktualnie zalogowanym użytkowniku na stronie z dozwolonym dostępem anonimowym, upewnij się, że obiekt`null MembershipUser` nie jest zwracany z metody `GetUser()` przed odwołaniem do jej właściwości.

Jeśli odwiedzasz stronę `AdditionalUserInfo.aspx` za pomocą przeglądarki, zostanie wyświetlona pusta strona, ponieważ nie dodaliśmy żadnych wierszy do tabeli `UserProfiles`. W kroku 6 dowiesz się, jak dostosować formant formancie CreateUserWizard, aby automatycznie dodać nowy wiersz do tabeli `UserProfiles` po utworzeniu nowego konta użytkownika. Na razie konieczne będzie ręczne utworzenie rekordu w tabeli.

Przejdź do Eksplorator bazy danych w programie Visual Studio i rozwiń folder Tables (tabele). Kliknij prawym przyciskiem myszy tabelę `aspnet_Users` i wybierz polecenie "Pokaż dane tabeli", aby wyświetlić rekordy w tabeli. Wykonaj tę samą czynność dla tabeli `UserProfiles`. Rysunek 11 przedstawia te wyniki w przypadku sąsiadujących pionowo. W mojej bazie danych są obecnie `aspnet_Users` rekordy dla Bruce, Fred i Tito, ale w tabeli `UserProfiles` nie ma żadnych rekordów.

[![zostanie wyświetlona zawartość aspnet_Users i tabel profilów użytkownika](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Ilustracja 11**: zawartość `aspnet_Users` i `UserProfiles` tabel są wyświetlane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image33.png))

Dodaj nowy rekord do tabeli `UserProfiles` przez ręczne wpisanie wartości dla pól `HomeTown`, `HomepageUrl`i `Signature`. Najprostszym sposobem uzyskania prawidłowej wartości `UserId` w nowym rekordzie `UserProfiles` jest wybranie pola `UserId` z określonego konta użytkownika w tabeli `aspnet_Users` i skopiowanie go i wklejenie do pola `UserId` w `UserProfiles`. Rysunek 12 przedstawia tabelę `UserProfiles` po dodaniu nowego rekordu do Bruce.

[![rekord został dodany do profilów użytkownika dla Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Ilustracja 12**. rekord został dodany do `UserProfiles` dla Bruce ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image36.png))

Wróć do strony `AdditionalUserInfo.aspx`, która została zarejestrowana jako Bruce. Jak pokazano na rysunku 13, są wyświetlane ustawienia Bruce.

[![aktualnie odwiedzany użytkownik pokazał swoje ustawienia](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Ilustracja 13**: aktualnie odwiedzany użytkownik pokazał swoje ustawienia ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image39.png))

> [!NOTE]
> Przejdź dalej i ręcznie Dodaj rekordy w tabeli `UserProfiles` dla każdego użytkownika z członkostwem. W kroku 6 dowiesz się, jak dostosować formant formancie CreateUserWizard, aby automatycznie dodać nowy wiersz do tabeli `UserProfiles` po utworzeniu nowego konta użytkownika.

## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Krok 3. umożliwienie użytkownikowi edytowania swojej miejscowości, strony głównej i podpisu

W tym momencie aktualnie zalogowany użytkownik może wyświetlać swoje ustawienia miejscowości, strony głównej i podpisu, ale nie mogą ich modyfikować. Zaktualizujmy kontrolkę DetailsView, aby można było edytować dane.

Pierwszą czynnością, którą należy wykonać, jest dodanie `UpdateCommand` dla kontrolki SqlDataSource, określenie instrukcji `UPDATE` do wykonania i odpowiadających jej parametrów. Wybierz kontrolki SqlDataSource, a następnie w okno Właściwości kliknij wielokropek obok właściwości UpdateQuery, aby wyświetlić okno dialogowe Edytor parametrów i polecenia. Wprowadź następującą instrukcję `UPDATE` do pola tekstowego:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Następnie kliknij przycisk "Odśwież parametry", który spowoduje utworzenie parametru w kolekcji `UpdateParameters` kontrolki kontrolki SqlDataSource dla każdego z parametrów w instrukcji `UPDATE`. Pozostaw Źródło dla wszystkich parametrów ustawione na brak, a następnie kliknij przycisk OK, aby zakończyć okno dialogowe.

[![określić właściwość UpdateCommand i UpdateParameters elementu kontrolki SqlDataSource](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Ilustracja 14**. Określanie `UpdateCommand` i `UpdateParameters` programu kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image42.png))

Ze względu na dodane przez nas zmiany w kontrolce kontrolki SqlDataSource, kontrolka DetailsView może teraz obsługiwać edycję. W tagu inteligentnym DetailsView zaznacz pole wyboru "Włącz edycję". Spowoduje to dodanie CommandField do kolekcji `Fields` formantu z właściwością `ShowEditButton` ustawioną na wartość true. Renderuje przycisk Edytuj, gdy element DetailsView jest wyświetlany w trybie tylko do odczytu, a przyciski Aktualizuj i Anuluj wyświetlają się w trybie edycji. Zamiast wymagać od użytkownika kliknięcia przycisku Edytuj, mimo że możemy ustawić renderowanie DetailsView w stanie "zawsze edytowalne" przez ustawienie [właściwości`DefaultMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) formantu detailsview na `Edit`.

Po wprowadzeniu tych zmian znaczniki deklaratywne kontrolki DetailsView powinny wyglądać podobnie do następujących:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Zwróć uwagę na dodanie właściwości CommandField i `DefaultMode`.

Przejdź dalej i przetestuj Tę stronę za pomocą przeglądarki. Podczas odwiedzania z użytkownikiem, który ma odpowiadający mu rekord w `UserProfiles`, ustawienia użytkownika są wyświetlane w interfejsie edytowalnym.

[![widoku DetailsView renderuje edytowalny interfejs](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Ilustracja 15**. widok DetailsView renderuje edytowalny interfejs ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image45.png))

Spróbuj zmienić wartości i kliknij przycisk Aktualizuj. Pojawia się tak, jakby nic się nie stało. Istnieje ogłaszanie zwrotne, a wartości są zapisywane w bazie danych, ale nie ma wizualizacji wizualnej, która wystąpiła zapis.

Aby to naprawić, Wróć do programu Visual Studio i Dodaj kontrolkę etykieta powyżej widoku DetailsView. Ustaw jej `ID` na `SettingsUpdatedMessage`, jej Właściwość `Text` na wartość "Twoje ustawienia zostały zaktualizowane", a jej właściwości `Visible` i `EnableViewState` są `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Musimy wyświetlić etykietę `SettingsUpdatedMessage` za każdym razem, gdy widok DetailsView zostanie zaktualizowany. Aby to osiągnąć, Utwórz procedurę obsługi zdarzeń dla zdarzenia `ItemUpdated` DetailsView i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Wróć do strony `AdditionalUserInfo.aspx` za pomocą przeglądarki i zaktualizuj dane. Tym razem będzie wyświetlany przydatny komunikat o stanie.

[![podczas aktualizowania ustawień jest wyświetlany krótki komunikat](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Rysunek 16**. podczas aktualizowania ustawień jest wyświetlany krótki komunikat ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image48.png))

> [!NOTE]
> Interfejs edytowania kontrolki DetailsView pozostawia dużo do pożądanego. Używa pól tekstowych o rozmiarze standardowym, ale pole podpisu powinno być prawdopodobnie wielowierszowym polem tekstowym. RegularExpressionValidator należy użyć, aby upewnić się, że adres URL strony głównej, jeśli został wprowadzony, zaczyna się od "http://" lub "https://". Ponadto, ponieważ kontrolka DetailsView ma właściwość `DefaultMode` ustawioną na `Edit`, przycisk Anuluj nie robi nic. Należy je usunąć lub, po kliknięciu, przekierować użytkownika do innej strony (takiej jak `~/Default.aspx`). Te ulepszenia należy pozostawić jako ćwiczenie dla czytnika.

### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Dodawanie linku do strony`AdditionalUserInfo.aspx`na stronie wzorcowej

Obecnie witryna sieci Web nie udostępnia żadnych linków do strony `AdditionalUserInfo.aspx`. Jedynym sposobem osiągnięcia tego celu jest wprowadzenie adresu URL strony bezpośrednio na pasku adresu przeglądarki. Dodajmy link do tej strony na stronie wzorcowej `Site.master`.

Odwołaj, że strona wzorcowa zawiera kontrolkę sieci Web widoku logowania w `LoginContent` ContentPlaceHolder, która wyświetla różne znaczniki dla uwierzytelnionych i anonimowych odwiedzających. Zaktualizuj `LoggedInTemplate` formantu widoku logowania, aby dołączyć łącze do strony `AdditionalUserInfo.aspx`. Po wprowadzeniu tych zmian znaczniki deklaratywne kontrolki widoku logowania powinny wyglądać podobnie do następujących:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Zwróć uwagę na dodanie `lnkUpdateSettings` formantu HyperLink do `LoggedInTemplate`. W przypadku tego linku Użytkownicy uwierzytelnieni mogą szybko przechodzić do strony, aby wyświetlić i zmodyfikować ustawienia miejscowości głównej, strony głównej i podpisu.

## <a name="step-4-adding-new-guestbook-comments"></a>Krok 4. Dodawanie nowych komentarzy do Księgi Gości

Strona `Guestbook.aspx` to miejsce, w którym użytkownicy uwierzytelnieni mogą przeglądać księgę gościa i zostawiać komentarz. Zacznijmy od utworzenia interfejsu w celu dodania nowych komentarzy do Księgi Gości.

Otwórz stronę `Guestbook.aspx` w programie Visual Studio i Skonstruuj interfejs użytkownika składający się z dwóch kontrolek TextBox, jeden dla tematu nowego komentarza i jeden dla jego treści. Ustaw właściwość `ID` pierwszej kontrolki TextBox na `Subject` i jej Właściwość `Columns` na 40; Skonfiguruj `ID` drugiej `Body`, `TextMode` do `MultiLine`, a jego `Width` i `Rows` właściwości odpowiednio do "95%" i 8. Aby ukończyć interfejs użytkownika, Dodaj kontrolkę sieci Web przycisku o nazwie `PostCommentButton` i ustaw jej Właściwość `Text` na "Opublikuj swój komentarz".

Ponieważ każdy komentarz księgi gościa wymaga tematu i treści, Dodaj element RequiredFieldValidator dla każdego z pól tekstowych. Ustaw właściwość `ValidationGroup` tych kontrolek na wartość "EnterComment"; Analogicznie ustaw właściwość `ValidationGroup` kontrolki `PostCommentButton` na wartość "EnterComment". Aby uzyskać więcej informacji na temat środowiska ASP. Kontrolki walidacji sieci, sprawdzanie [poprawności formularza w ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), odrzucanie [kontrolek weryfikacji w ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx), a następnie [samouczek kontroli serwera weryfikacji](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) na [w3schools](http://www.w3schools.com/).

Po rozpoczęciu interfejsu użytkownika znaczniki deklaratywne strony powinny wyglądać podobnie do następujących:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Gdy interfejs użytkownika zostanie ukończony, następnym zadaniem jest wstawienie nowego rekordu do tabeli `GuestbookComments`, gdy zostanie kliknięty `PostCommentButton`. Można to zrobić na kilka sposobów: możemy napisać kod ADO.NET w obsłudze zdarzeń `Click` przycisku. na stronie można dodać kontrolkę kontrolki SqlDataSource, skonfigurować jej `InsertCommand`, a następnie wywołać metodę `Insert` z programu obsługi zdarzeń `Click`. można też utworzyć warstwę środkową, która była odpowiedzialna za wstawianie nowych komentarzy do księgi gości i wywoływać tę funkcję z programu obsługi zdarzeń `Click`. Ponieważ oglądamy kontrolki SqlDataSource w kroku 3, użyjmy tutaj kodu ADO.NET.

> [!NOTE]
> Klasy ADO.NET używane do programistycznego dostępu do danych z bazy danych Microsoft SQL Server znajdują się w przestrzeni nazw `System.Data.SqlClient`. Może zajść potrzeba zaimportowania tej przestrzeni nazw do klasy powiązanej z kodem strony (tj., `using System.Data.SqlClient;`).

Utwórz procedurę obsługi zdarzeń dla zdarzenia `Click` `PostCommentButton`i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

Procedura obsługi zdarzeń `Click` uruchamia się, sprawdzając, czy dane dostarczone przez użytkownika są prawidłowe. Jeśli tak nie jest, obsługa zdarzeń kończy się przed wstawieniem rekordu. Przy założeniu, że podane dane są prawidłowe, obecnie zalogowany użytkownik `UserId` wartość zostanie pobrana i zapisana w `currentUserId` zmiennej lokalnej. Ta wartość jest wymagana, ponieważ podczas wstawiania rekordu do `GuestbookComments`musi podać wartość `UserId`.

Poniższe parametry połączenia dla bazy danych `SecurityTutorials` są pobierane z `Web.config` i zostanie określona instrukcja SQL `INSERT`. Następnie zostanie utworzony i otwarty obiekt `SqlConnection`. Następnie tworzony jest obiekt `SqlCommand` i wartości parametrów używanych w zapytaniu `INSERT` są przypisywane. Następnie zostanie wykonana instrukcja `INSERT` i zamknięto połączenie. Na końcu programu obsługi zdarzeń, `Subject` i `Body` textpól, `Text` właściwości są wyczyszczone, aby wartości użytkownika nie były utrwalane przez ogłaszanie zwrotne.

Przejdź dalej i przetestuj Tę stronę w przeglądarce. Ponieważ ta strona znajduje się w `Membership` folderze, nie jest ona dostępna dla anonimowych odwiedzających. W związku z tym musisz najpierw zalogować się (jeśli jeszcze nie zostało to zrobione). Wprowadź wartość do `Subject` i `Body` pola tekstowe, a następnie kliknij przycisk `PostCommentButton`. Spowoduje to dodanie nowego rekordu do `GuestbookComments`. W przypadku ogłaszania zwrotnego temat i treść podane przez użytkownika są czyszczone z pól tekstowych.

Po kliknięciu przycisku `PostCommentButton` nie ma żadnych opinii wizualnych, że komentarz został dodany do księgi gościa. Nadal musimy zaktualizować Tę stronę, aby wyświetlić istniejące Komentarze do Księgi Gości, którą wykonamy w kroku 5. Po tym celu komentarz dodany do programu będzie wyświetlany na liście komentarzy, co zapewnia odpowiednią opinię wizualną. Na razie upewnij się, że komentarz do księgi gościa został zapisany, sprawdzając zawartość tabeli `GuestbookComments`.

Rysunek 17 przedstawia zawartość tabeli `GuestbookComments` po lewej stronie dwóch komentarzy.

[![można zobaczyć Komentarze księgi gościa w tabeli GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Ilustracja 17**. Komentarze księgi gościa są widoczne w tabeli `GuestbookComments` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image51.png))

> [!NOTE]
> Jeśli użytkownik spróbuje wstawić komentarz księgi gościa, który zawiera potencjalnie niebezpieczne znaczniki, takie jak HTML – ASP.NET, zgłosi `HttpRequestValidationException`. Aby dowiedzieć się więcej o tym wyjątku, dlaczego został zgłoszony i jak zezwolić użytkownikom na przesyłanie potencjalnie niebezpiecznych wartości, zapoznaj się z oficjalnym [dokumentem weryfikacji żądania](../../../../whitepapers/request-validation.md).

## <a name="step-5-listing-the-existing-guestbook-comments"></a>Krok 5. Wyświetlanie istniejących komentarzy do Księgi Gości

Oprócz pozostawiania komentarzy użytkownik odwiedzający stronę `Guestbook.aspx` powinien również mieć możliwość wyświetlania istniejących komentarzy księgi gościa. Aby to osiągnąć, Dodaj kontrolkę ListView o nazwie `CommentList` w dolnej części strony.

> [!NOTE]
> Formant ListView jest nowy do ASP.NET w wersji 3,5. Jest ona przeznaczona do wyświetlania listy elementów w bardzo dostosowywalnym i elastycznym układzie, ale nadal oferuje wbudowaną edycję, wstawianie, usuwanie, stronicowanie i sortowanie funkcji, takich jak GridView. Jeśli używasz ASP.NET 2,0, należy zamiast tego użyć kontrolki DataList lub wzmacniak. Aby uzyskać więcej informacji na temat korzystania z elementu ListView, zobacz wpis w blogu [Scott Guthrie](https://weblogs.asp.net/scottgu/), [formant ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)i mój artykuł, [Wyświetlanie danych za pomocą kontrolki ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Otwórz tag inteligentny elementu ListView i z listy rozwijanej wybierz źródło danych Powiąż formant z nowym źródłem danych. Jak opisano w kroku 2, spowoduje to uruchomienie Kreatora konfiguracji źródła danych. Wybierz ikonę bazy danych, nadaj jej kontrolki SqlDataSource `CommentsDataSource`i kliknij przycisk OK. Następnie wybierz z listy rozwijanej `SecurityTutorialsConnectionString` parametry połączenia i kliknij przycisk Dalej.

W tym punkcie w kroku 2 podano dane do zapytania przez pobranie tabeli `UserProfiles` z listy rozwijanej i wybranie kolumn do zwrócenia (zapoznaj się z powrotem do rysunku 9). Tym razem chcemy umieścić instrukcję SQL, która ściąga nie tylko rekordy z `GuestbookComments`, ale również miejscowość, stronę główną, podpis i nazwę użytkownika komentarza użytkownika. W związku z tym wybierz przycisk radiowy "Określ niestandardową instrukcję SQL lub procedurę składowaną" i kliknij przycisk Dalej.

Spowoduje to wyświetlenie ekranu "Definiowanie niestandardowych instrukcji lub procedur składowanych". Kliknij przycisk Konstruktor zapytań, aby utworzyć graficznie zapytanie. Konstruktor zapytań uruchamia się, monitując o określenie tabel, z których chcemy wysyłać zapytania. Zaznacz tabele `GuestbookComments`, `UserProfiles`i `aspnet_Users` i kliknij przycisk OK. Spowoduje to dodanie wszystkich trzech tabel do powierzchni projektowej. Ponieważ istnieją ograniczenia klucza obcego między tabelami `GuestbookComments`, `UserProfiles`i `aspnet_Users`, Konstruktor zapytań automatycznie `JOIN` te tabele.

To wszystko, co pozostanie, aby określić kolumny do zwrócenia. Z tabeli `GuestbookComments` wybierz kolumny `Subject`, `Body`i `CommentDate`; Zwróć kolumny `HomeTown`, `HomepageUrl`i `Signature` z tabeli `UserProfiles`; i zwróć `UserName` z `aspnet_Users`. Należy również dodać "`ORDER BY CommentDate DESC`" na końcu zapytania `SELECT`, tak aby ostatnie wpisy zostały zwrócone jako pierwsze. Po dokonaniu tych wyborów interfejs Konstruktor zapytań powinien wyglądać podobnie do zrzutu ekranu na rysunku 18.

[![skonstruowane zapytanie sprzęga tabele GuestbookComments, UserProfiles i aspnet_Users](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Ilustracja 18**. skonstruowane zapytanie `JOIN` s `GuestbookComments`, `UserProfiles`i tabele `aspnet_Users` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image54.png))

Kliknij przycisk OK, aby zamknąć okno Konstruktor zapytań i powrócić do ekranu "Definiuj niestandardowe instrukcje lub procedury składowane". Kliknij przycisk Dalej, aby przejść do ekranu "test zapytania", gdzie można wyświetlić wyniki zapytania, klikając przycisk Test zapytania. Gdy wszystko będzie gotowe, kliknij przycisk Zakończ, aby zakończyć pracę Kreatora konfiguracji źródła danych.

Po zakończeniu pracy Kreatora konfiguracji źródła danych w kroku 2, skojarzona kolekcja `Fields` formantu DetailsView została zaktualizowana tak, aby zawierała BoundField dla każdej kolumny zwróconej przez `SelectCommand`. Element ListView, jednak pozostaje niezmieniony; nadal musimy zdefiniować układ. Układ elementu ListView można utworzyć ręcznie za pośrednictwem jego znaczników deklaratywnych lub z opcji "Konfiguruj ListView" w tagu inteligentnym. Zazwyczaj preferuję Definiowanie adiustacji, ale Używaj dowolnej metody najbardziej naturalnej.

Zakończył się przy użyciu następujących `LayoutTemplate`, `ItemTemplate`i `ItemSeparatorTemplate` dla mojego formantu ListView:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

`LayoutTemplate` definiuje adiustację emitowaną przez formant, podczas gdy `ItemTemplate` renderuje każdy element zwracany przez kontrolki SqlDataSource. Oznakowane `ItemTemplate`jest umieszczane w kontrolce `itemPlaceholder` `LayoutTemplate`. Oprócz `itemPlaceholder`, `LayoutTemplate` zawiera kontrolkę formantu DataPager, która ogranicza widok ListView do wyświetlania tylko 10 komentarzy Księgi Gości na stronie (domyślnie) i renderuje interfejs stronicowania.

My `ItemTemplate` wyświetla każdy podmiot komentarza księgi gościa w elemencie `<h4>` z treścią znajdującą się poniżej tematu. Należy zauważyć, że składnia używana do wyświetlania treści przyjmuje dane zwrócone przez instrukcję `Eval("Body")` DataBinding, konwertuje ją na ciąg i zastępuje podziały wierszy przy użyciu elementu `<br />`. Ta konwersja jest wymagana w celu wyświetlenia podziałów wierszy wprowadzonych podczas przesyłania komentarza, ponieważ odstęp jest ignorowany przez HTML. Podpis użytkownika jest wyświetlany poniżej treści kursywy, po którym następuje miejscowość domu użytkownika, link do swojej strony głównej, datę i godzinę utworzenia komentarza oraz nazwę użytkownika osoby, która opuściła komentarz.

Poświęć chwilę na wyświetlenie strony za pomocą przeglądarki. W tym miejscu powinny być widoczne Komentarze dodane do księgi gościa w kroku 5.

[![Księgi Gości. aspx teraz wyświetla Komentarze księgi gościa](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Ilustracja 19**. `Guestbook.aspx` teraz wyświetla Komentarze księgi gościa ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image57.png))

Spróbuj dodać nowy komentarz do Księgi Gości. Po kliknięciu przycisku `PostCommentButton` zostanie wyświetlona strona z powrotem i komentarz zostanie dodany do bazy danych, ale formant ListView nie zostanie zaktualizowany w celu wyświetlenia nowego komentarza. Można to naprawić za pomocą jednej z:

- Aktualizowanie programu obsługi zdarzeń `Click` `PostCommentButton` przycisku, aby wywołuje metodę `DataBind()` formantu ListView po wstawieniu nowego komentarza do bazy danych lub
- Ustawianie właściwości `EnableViewState` kontrolki ListView na `false`. To podejście działa z powodu wyłączenia stanu widoku kontrolki, dlatego należy ponownie powiązać dane bazowe przy każdym ogłaszaniu zwrotnym.

W witrynie sieci Web samouczka do pobrania z tego samouczka są zilustrowane obie techniki. Właściwość `EnableViewState` formantu ListView do `false` i kod niezbędny do programowego ponownego powiązania danych z ListView znajduje się w programie obsługi zdarzeń `Click`, ale jest oznaczony jako komentarz.

> [!NOTE]
> Obecnie Strona `AdditionalUserInfo.aspx` umożliwia użytkownikowi wyświetlanie i edytowanie ustawień miejscowości głównej, strony głównej i podpisu. Może być warto zaktualizować `AdditionalUserInfo.aspx`, aby wyświetlić komentarze księgi gości zalogowanego użytkownika. Oznacza to, że oprócz sprawdzania i modyfikowania informacji użytkownik może odwiedzić stronę `AdditionalUserInfo.aspx`, aby zobaczyć, jakie komentarze w witrynie gościa zostały wprowadzone w przeszłości. Jestem to ćwiczenie dla interesującego czytnika.

## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Krok 6. dostosowanie formantu formancie CreateUserWizard w celu uwzględnienia interfejsu dla miejscowości głównej, strony głównej i podpisu

Zapytanie `SELECT` używane przez stronę `Guestbook.aspx` używa `INNER JOIN` do łączenia powiązanych rekordów między tabelami `GuestbookComments`, `UserProfiles`i `aspnet_Users`. Jeśli użytkownik, który nie ma rekordu w `UserProfiles`, tworzy komentarz do księgi gościa, komentarz nie będzie wyświetlany w elemencie ListView, ponieważ `INNER JOIN` zwraca tylko `GuestbookComments` rekordy, gdy istnieją pasujące rekordy w `UserProfiles` i `aspnet_Users`. W kroku 3, jeśli użytkownik nie ma rekordu w `UserProfiles` nie może wyświetlić ani edytować swoich ustawień na stronie `AdditionalUserInfo.aspx`.

Bez względu na to, że z powodu naszych decyzji projektowych należy pamiętać, że każde konto użytkownika w systemie członkostwa ma pasujący rekord w tabeli `UserProfiles`. Co chcemy zrobić dla odpowiedniego rekordu, który ma zostać dodany do `UserProfiles` za każdym razem, gdy nowe konto użytkownika członkostwa zostanie utworzone za pomocą formancie CreateUserWizard.

Zgodnie z opisem w samouczku [*Tworzenie kont użytkowników*](creating-user-accounts-cs.md) po utworzeniu nowego konta użytkownika członkostwa formant formancie CreateUserWizard wywołuje jego [zdarzenie`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Możemy utworzyć procedurę obsługi zdarzeń dla tego zdarzenia, pobrać identyfikator użytkownika dla właśnie utworzonego elementu, a następnie wstawić rekord do tabeli `UserProfiles` z wartościami domyślnymi dla kolumn `HomeTown`, `HomepageUrl`i `Signature`. Co więcej, można monitować użytkownika o te wartości przez dostosowanie interfejsu kontrolki formancie CreateUserWizard w celu uwzględnienia dodatkowych pól tekstowych.

Najpierw Spójrzmy na to, jak dodać nowy wiersz do tabeli `UserProfiles` w programie obsługi zdarzeń `CreatedUser` z wartościami domyślnymi. Poniżej przedstawiono sposób dostosowywania interfejsu użytkownika kontrolki formancie CreateUserWizard w celu uwzględnienia dodatkowych pól formularza w celu zebrania miejscowości, strony głównej i podpisu nowego użytkownika.

### <a name="adding-a-default-row-touserprofiles"></a>Dodawanie wiersza domyślnego do`UserProfiles`

W samouczku [*Tworzenie kont użytkowników*](creating-user-accounts-cs.md) dodaliśmy formant formancie CreateUserWizard do strony `CreatingUserAccounts.aspx` w folderze `Membership`. Aby formant formancie CreateUserWizard mógł dodać rekord do tabeli `UserProfiles` podczas tworzenia konta użytkownika, należy zaktualizować funkcje kontrolki formancie CreateUserWizard. Zamiast wprowadzać te zmiany na stronie `CreatingUserAccounts.aspx`, Spróbujmy dodać nową kontrolkę formancie CreateUserWizard do `EnhancedCreateUserWizard.aspx` strony i wprowadzić modyfikacje w tym samouczku.

Otwórz stronę `EnhancedCreateUserWizard.aspx` w programie Visual Studio i przeciągnij kontrolkę formancie CreateUserWizard z przybornika na stronę. Ustaw właściwość `ID` formantu formancie CreateUserWizard na `NewUserWizard`. Zgodnie z opisem w <a id="_msoanchor_5"> </a>samouczku [*Tworzenie kont użytkowników*](creating-user-accounts-cs.md) , domyślny interfejs użytkownika programu formancie CreateUserWizard poprosi osoby odwiedzającej o niezbędne informacje. Po podaniu tych informacji formant wewnętrznie tworzy nowe konto użytkownika w strukturze członkostwa, bez konieczności pisania jednego wiersza kodu.

Kontrolka formancie CreateUserWizard wywołuje wiele zdarzeń podczas przepływu pracy. Gdy osoba odwiedzająca dostarczy informacje o żądaniu i wyśle formularz, formant formancie CreateUserWizard początkowo uruchamia jego [zdarzenie`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Jeśli wystąpi problem podczas procesu tworzenia, zostanie wyzwolone [zdarzenie`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) . Jeśli jednak użytkownik zostanie utworzony pomyślnie, zostanie zgłoszone [zdarzenie`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) . <a id="_msoanchor_6"> </a>W samouczku [*Tworzenie kont użytkowników*](creating-user-accounts-cs.md) utworzyliśmy procedurę obsługi zdarzeń dla zdarzenia `CreatingUser`, aby upewnić się, że podana nazwa użytkownika nie zawiera spacji wiodących lub końcowych, a nazwa użytkownika nie pojawia się w dowolnym miejscu hasła.

Aby dodać wiersz do tabeli `UserProfiles` dla samego użytkownika, musimy utworzyć procedurę obsługi zdarzeń dla zdarzenia `CreatedUser`. Po uruchomieniu zdarzenia `CreatedUser` konto użytkownika zostało już utworzone w strukturze członkostwa, umożliwiając nam pobranie wartości identyfikatora użytkownika konta.

Utwórz procedurę obsługi zdarzeń dla zdarzenia `CreatedUser` `NewUserWizard`i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Powyższy kod jest pobierany przez pobranie identyfikatora użytkownika konta, które właśnie dodano. Jest to realizowane za pomocą metody `Membership.GetUser(username)` do zwrócenia informacji o konkretnym użytkowniku, a następnie przy użyciu właściwości `ProviderUserKey` do pobrania ich identyfikatora użytkownika. Nazwa użytkownika wprowadzona przez użytkownika w kontrolce formancie CreateUserWizard jest dostępna za pośrednictwem jego [właściwości`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Następnie parametry połączenia są pobierane z `Web.config` i zostanie określona instrukcja `INSERT`. Wymagane obiekty ADO.NET są tworzone i wykonywane jest polecenie. Kod przypisuje wystąpienie [`DBNull`](https://msdn.microsoft.com/library/system.dbnull.aspx) do parametrów `@HomeTown`, `@HomepageUrl`i `@Signature`, które mają wpływ wstawiania wartości `NULL` bazy danych dla pól `HomeTown`, `HomepageUrl`i `Signature`.

Odwiedź stronę `EnhancedCreateUserWizard.aspx` za pomocą przeglądarki i Utwórz nowe konto użytkownika. Po wykonaniu tej czynności Wróć do programu Visual Studio i sprawdź zawartość `aspnet_Users` i `UserProfiles` tabel (na przykład z powrotem na rysunku 12). Powinno zostać wyświetlone nowe konto użytkownika w `aspnet_Users` i odpowiadający mu wiersz `UserProfiles` (z wartościami `NULL` dla `HomeTown`, `HomepageUrl`i `Signature`).

[![dodano nowe konto użytkownika i rekord profilów użytkowników](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Ilustracja 20**. Dodano nowe konto użytkownika i rekord `UserProfiles` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image60.png))

Gdy osoba odwiedzająca dostarczyła nowe informacje o koncie i kliknie przycisk "Utwórz użytkownika", zostanie utworzone konto użytkownika i wiersz dodany do tabeli `UserProfiles`. Następnie formancie CreateUserWizard wyświetla `CompleteWizardStep`, który wyświetla komunikat o powodzeniu i przycisk Kontynuuj. Kliknięcie przycisku Kontynuuj powoduje odświeżenie, ale nie podjęto żadnej akcji, pozostawiając użytkownikowi zablokowany na stronie `EnhancedCreateUserWizard.aspx`.

Możemy określić adres URL, do którego ma zostać wysłany użytkownik po kliknięciu przycisku Kontynuuj za pomocą [właściwości`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)kontrolki formancie CreateUserWizard. Ustaw właściwość `ContinueDestinationPageUrl` na wartość "~/Membership/AdditionalUserInfo.aspx". Spowoduje to `AdditionalUserInfo.aspx`nowego użytkownika, w którym można wyświetlać i aktualizować ustawienia.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Dostosowywanie interfejsu formancie CreateUserWizard w celu wyświetlenia monitu o podanie miejscowości, strony głównej i podpisu nowego użytkownika

Domyślny interfejs formantu formancie CreateUserWizard jest wystarczający w przypadku prostych scenariuszy tworzenia kont, w których zbierane są tylko podstawowe informacje o kontach użytkownika, hasła i wiadomości e-mail. Ale co zrobić, jeśli chcemy monitować odwiedzających o podanie swojej miejscowości domowej, strony głównej i podpisu podczas tworzenia konta? Można dostosować interfejs formantu formancie CreateUserWizard, aby zbierać dodatkowe informacje podczas rejestracji, a informacje te mogą być używane w programie obsługi zdarzeń `CreatedUser`, aby wstawić dodatkowe rekordy do źródłowej bazy danych.

Formant formancie CreateUserWizard rozszerza formant kreatora ASP.NET, który jest formantem umożliwiającym deweloperowi strony Definiowanie serii uporządkowanych `WizardSteps`. Kontrolka kreatora renderuje aktywny krok i udostępnia interfejs nawigacji, który umożliwia odwiedzającemu przechodzenie przez te kroki. Kontrolka kreatora doskonale nadaje się do podziału długiego zadania na kilka krótkich kroków. Aby uzyskać więcej informacji na temat kontrolki kreatora, zobacz [Tworzenie interfejsu użytkownika krok po kroku przy użyciu kontrolki kreatora ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Domyślne oznakowanie formantu formancie CreateUserWizard definiuje dwie `WizardSteps`: `CreateUserWizardStep` i `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

Pierwszy `WizardStep`, `CreateUserWizardStep`, renderuje interfejs, który jest monitowany o nazwę użytkownika, hasło, adres e-mail itd. Gdy odwiedzający wprowadzi te informacje i kliknie pozycję "Utwórz użytkownika", zostanie wyświetlona `CompleteWizardStep`, która wyświetla komunikat o powodzeniu i przycisk Kontynuuj.

Aby dostosować interfejs formantu formancie CreateUserWizard w celu uwzględnienia dodatkowych pól formularza, możemy:

- <strong>Utwórz co najmniej jeden nowy</strong> <strong>`WizardStep`</strong> <strong>s, aby zawierać dodatkowe elementy interfejsu użytkownika</strong>. Aby dodać nowy `WizardStep` do formancie CreateUserWizard, kliknij link "Dodaj/Usuń `WizardSteps`" z taga inteligentnego, aby uruchomić Edytor kolekcji `WizardStep`. W tym miejscu możesz dodać, usunąć lub zmienić kolejność kroków w kreatorze. Jest to podejście, które będziemy używać w tym samouczku.

- <strong>Przekonwertuj</strong> <strong>`CreateUserWizardStep`</strong> <strong>na`WizardStep`do edycji</strong> <strong>.</strong> Spowoduje to zamienienie `CreateUserWizardStep` z odpowiednikiem `WizardStep`, którego znacznik definiuje interfejs użytkownika, który odpowiada `CreateUserWizardStep`. Konwertując `CreateUserWizardStep` na `WizardStep` możemy zmienić położenie formantów lub dodać do tego kroku dodatkowe elementy interfejsu użytkownika. Aby skonwertować `CreateUserWizardStep` lub `CompleteWizardStep` do `WizardStep`edytowalnego, kliknij link "Dostosuj tworzenie kroku użytkownika" lub "Dostosuj krok ukończenia" z tagu inteligentnego kontrolki.

- **Użyj kombinacji powyższych dwóch opcji.**

Należy pamiętać, że formant formancie CreateUserWizard wykonuje proces tworzenia konta użytkownika po kliknięciu przycisku "Utwórz użytkownika" w ramach jego `CreateUserWizardStep`. Nie ma znaczenia, czy istnieją dodatkowe `WizardStep` s po `CreateUserWizardStep`, czy nie.

Podczas dodawania niestandardowego `WizardStep` do formantu formancie CreateUserWizard w celu zbierania dodatkowych danych wejściowych użytkownika, `WizardStep` niestandardowych można umieścić przed lub po `CreateUserWizardStep`. Jeśli jest wcześniejsza niż `CreateUserWizardStep`, dodatkowe dane wejściowe użytkownika zebrane z niestandardowego `WizardStep` są dostępne dla programu obsługi zdarzeń `CreatedUser`. Jeśli jednak `WizardStep` niestandardowe nastąpi po `CreateUserWizardStep` wtedy, gdy zostanie wyświetlone niestandardowe `WizardStep`, nowe konto użytkownika zostało już utworzone i zdarzenie `CreatedUser` zostało już wyzwolone.

Rysunek 21 pokazuje przepływ pracy, gdy dodany `WizardStep` poprzedza `CreateUserWizardStep`. Ponieważ dodatkowe informacje o użytkowniku zostały zebrane przez czas wygenerowanego zdarzenia `CreatedUser`, wystarczy zaktualizować procedurę obsługi zdarzeń `CreatedUser`, aby pobrać te dane wejściowe i użyć ich dla wartości parametrów `INSERT` instrukcji (a nie `DBNull.Value`).

[![przepływ pracy formancie CreateUserWizard, gdy dodatkowy element WizardStep poprzedza Element CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Ilustracja 21**. przepływ pracy formancie CreateUserWizard, gdy dodatkowe `WizardStep` poprzedza `CreateUserWizardStep` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image63.png))

Jeśli `WizardStep` niestandardowe zostanie umieszczona *po* `CreateUserWizardStep`, proces tworzenia konta użytkownika będzie odbywać się, zanim użytkownik będzie musiał wprowadzić swoją miejscowość, stronę główną lub podpis. W takim przypadku te dodatkowe informacje muszą zostać wstawione do bazy danych po utworzeniu konta użytkownika, jak pokazano na rysunku 22.

[![przepływ pracy formancie CreateUserWizard, gdy dodatkowy element WizardStep jest dostępny po Element CreateUserWizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Ilustracja 22**: przepływ pracy formancie CreateUserWizard, gdy dodatkowa `WizardStep` jest dostępna po `CreateUserWizardStep` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](storing-additional-user-information-cs/_static/image66.png))

Przepływ pracy przedstawiony na rysunku 22 oczekuje na wstawienie rekordu do tabeli `UserProfiles` do momentu zakończenia kroku 2. Jeśli osoba odwiedzająca zamknie swoją przeglądarkę po kroku 1, zostanie osiągnięty stan, w którym utworzono konto użytkownika, ale żaden rekord nie został dodany do `UserProfiles`. Obejście polega na zapisaniu rekordu z `NULL` lub wartościami domyślnymi wstawionymi do `UserProfiles` w programie obsługi zdarzeń `CreatedUser` (który jest uruchamiany po kroku 1), a następnie zaktualizować ten rekord po zakończeniu kroku 2. Dzięki temu do konta użytkownika zostanie dodany rekord `UserProfiles`, nawet jeśli użytkownik zakończy proces rejestracji w połowie.

Na potrzeby tego samouczka utworzymy nowy `WizardStep`, który występuje po `CreateUserWizardStep`, ale przed `CompleteWizardStep`. Najpierw pobierz element WizardStep w miejscu i skonfigurowano, a następnie poczekamy na kod.

W tagu inteligentnym kontrolki formancie CreateUserWizard wybierz pozycję "Dodaj/Usuń `WizardStep` s", co spowoduje wyświetlenie okna dialogowego edytora kolekcji `WizardStep`. Dodaj nowe `WizardStep`, ustawiając jego `ID` na `UserSettings`, jego `Title` do "ustawień" i jego `StepType` do `Step`. Następnie umieść ją tak, aby była dostępna po `CreateUserWizardStep` ("Utwórz nowe konto") i przed `CompleteWizardStep` ("Complete"), jak pokazano na rysunku 23.

[![dodać nowego elementu WizardStep do kontrolki formancie CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Ilustracja 23**. dodawanie nowego `WizardStep` do formantu formancie CreateUserWizard ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](storing-additional-user-information-cs/_static/image69.png))

Kliknij przycisk OK, aby zamknąć okno dialogowe edytora kolekcji `WizardStep`. Nowy `WizardStep` jest dowodem przez zaktualizowane znaczniki deklaratywne kontrolki formancie CreateUserWizard:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Zanotuj nowy element `<asp:WizardStep>`. Musimy dodać interfejs użytkownika w celu zebrania w tym miejscu miejscowości głównej, strony głównej i podpisu nowego użytkownika. Możesz wprowadzić tę zawartość w składni deklaracyjnej lub za pomocą projektanta. Aby skorzystać z projektanta, wybierz krok "Twoje ustawienia" z listy rozwijanej w tagu inteligentnym, aby zobaczyć krok w projektancie.

> [!NOTE]
> Wybranie kroku za pomocą listy rozwijanej tag inteligentny aktualizuje [właściwość`ActiveStepIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)formantu formancie CreateUserWizard, która określa indeks kroku początkowego. W związku z tym, jeśli ta lista rozwijana zostanie użyta w celu edytowania kroku "Twoje ustawienia" w projektancie, należy ustawić ją z powrotem na "Utwórz nowe konto", aby ten krok był wyświetlany, gdy użytkownicy po raz pierwszy odwiedzią stronę `EnhancedCreateUserWizard.aspx`.

Utwórz interfejs użytkownika w kroku "Ustawienia", który zawiera trzy kontrolki TextBox o nazwach `HomeTown`, `HomepageUrl`i `Signature`. Po skonstruowaniu tego interfejsu, znaczniki deklaratywne formancie CreateUserWizard powinny wyglądać podobnie do następujących:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Przejdź dalej i odwiedź tę stronę za pośrednictwem przeglądarki i Utwórz nowe konto użytkownika, określając wartości miejscowości głównej, strony głównej i podpisu. Po zakończeniu `CreateUserWizardStep` konto użytkownika zostanie utworzone w środowisku członkostwa i zostanie uruchomiony program obsługi zdarzeń `CreatedUser`, który dodaje nowy wiersz do `UserProfiles`, ale z wartością `NULL` bazy danych dla `HomeTown`, `HomepageUrl`i `Signature`. Wartości wprowadzone dla miejscowości głównej, strony głównej i podpisu nigdy nie są używane. Wynik netto to nowe konto użytkownika z rekordem `UserProfiles`, którego pola `HomeTown`, `HomepageUrl`i `Signature` nie zostały jeszcze określone.

Musimy wykonać kod po kroku "Ustawienia", który przyjmuje wartości miejscowości, honepage i podpisu wprowadzone przez użytkownika, a następnie aktualizuje odpowiedni rekord `UserProfiles`. Za każdym razem, gdy użytkownik przejdzie między krokami w kontrolce kreatora, wyzwalane jest [zdarzenie`ActiveStepChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) kreatora. Możemy utworzyć procedurę obsługi zdarzeń dla tego zdarzenia i zaktualizować tabelę `UserProfiles` po zakończeniu kroku "Ustawienia".

Dodaj procedurę obsługi zdarzeń dla zdarzenia `ActiveStepChanged` formancie CreateUserWizard i Dodaj następujący kod:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Powyższy kod zaczyna się od określenia, czy właśnie został osiągnięty krok "ukończony". Ponieważ krok "Ukończ" występuje bezpośrednio po kroku "Ustawienia", wtedy, gdy osoba odwiedzająca osiągnie krok "Ukończ", oznacza to, że właśnie zakończył krok "Twoje ustawienia".

W takim przypadku musimy programowo odwoływać się do formantów TextBox w `UserSettings WizardStep`. W tym celu należy najpierw użyć metody `FindControl`, aby programowo odwoływać się do `UserSettings WizardStep`, a następnie ponownie uzyskać odwołanie do pól tekstowych w `WizardStep`. Po przywróceniu pól tekstowych można wykonać instrukcję `UPDATE`. Instrukcja `UPDATE` ma taką samą liczbę parametrów jak instrukcja `INSERT` w programie obsługi zdarzeń `CreatedUser`, ale w tym miejscu użyto miejscowości głównej, strony głównej i wartości podpisu dostarczonych przez użytkownika.

Korzystając z tej procedury obsługi zdarzeń, odwiedź stronę `EnhancedCreateUserWizard.aspx` za pośrednictwem przeglądarki i Utwórz nowe konto użytkownika, określając wartości dla miejscowości, strony głównej i podpisu. Po utworzeniu nowego konta należy przekierować do strony `AdditionalUserInfo.aspx`, w której wyświetlane są tylko informacje o miejscowości, stronie głównej i podpisie.

> [!NOTE]
> Witryna sieci Web zawiera obecnie dwie strony, z których odwiedzający może utworzyć nowe konto: `CreatingUserAccounts.aspx` i `EnhancedCreateUserWizard.aspx`. Witryna sieci Web i strona logowania są wskazywane na stronie `CreatingUserAccounts.aspx`, ale strona `CreatingUserAccounts.aspx` nie monituje użytkownika o podanie miejscowości, strony głównej ani informacji o podpisie i nie dodaje odpowiedniego wiersza do `UserProfiles`. W związku z tym zaktualizuj stronę `CreatingUserAccounts.aspx` tak, aby zapewniała tę funkcję, lub zaktualizuj stronę mapy witryny i strony logowania, aby odwoływała się do `EnhancedCreateUserWizard.aspx` zamiast `CreatingUserAccounts.aspx`. Jeśli wybierzesz ostatnią opcję, pamiętaj o zaktualizowaniu pliku `Web.config` folderu `Membership`, aby umożliwić anonimowym użytkownikom dostęp do strony `EnhancedCreateUserWizard.aspx`.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono metody modelowania danych, które są związane z kontami użytkowników w ramach platformy członkostwa. W szczególności znaleźliśmy jednostki modelowania, które współdzielą relację jeden-do-wielu z kontami użytkowników, a także dane, które współużytkują relację jeden do jednego. Ponadto dodaliśmy, jak te powiązane informacje mogą być wyświetlane, wstawiane i aktualizowane, a niektóre przykłady używają formantu kontrolki SqlDataSource i innych osób korzystających z kodu ADO.NET.

W tym samouczku zakończymy pracę nad kontami użytkowników. Począwszy od następnego samouczka, będziemy mogli zwrócić uwagę na role. W przypadku kolejnych kilku samouczków zobaczymy strukturę role, zobacz jak utworzyć nowe role, jak przypisać role do użytkowników, jak określić role, do których należy użytkownik, i jak zastosować autoryzację opartą na rolach.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Uzyskiwanie dostępu do danych i aktualizowanie ich w programie ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Kontrolka kreatora ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Tworzenie interfejsu użytkownika krok po kroku przy użyciu kontrolki kreatora ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Tworzenie niestandardowych parametrów kontrolki DataSource](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Dostosowywanie formantu formancie CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Przewodnik Szybki Start dotyczący kontrolek DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Wyświetlanie danych za pomocą kontrolki ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Przetrwanie kontroli walidacji w programie ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Edytowanie danych wstawiania i usuwania](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Walidacja formularza w ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Zbieranie informacji o niestandardowych zarejestrowaniu użytkownika](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profile w programie ASP.NET 2,0](http://www.odetocode.com/Articles/440.aspx)
- [Formant ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Profile użytkowników — Szybki Start](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](user-based-authorization-cs.md)
> [dalej](creating-the-membership-schema-in-sql-server-vb.md)

---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Kompilowanie modelu przy użyciu walidacji reguł firmy | Microsoft Docs
author: microsoft
description: Krok 3 pokazuje, jak utworzyć model, za pomocą którego można wysyłać zapytania i aktualizować bazę danych dla naszej aplikacji NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 6ebf1b71c089229ba9139ff7dc788b8978724046
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541665"
---
# <a name="build-a-model-with-business-rule-validations"></a>Budowanie modelu z walidacją reguł biznesowych

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 3 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 3 pokazuje, jak utworzyć model, za pomocą którego można wysyłać zapytania i aktualizować bazę danych dla naszej aplikacji NerdDinner.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner krok 3: kompilowanie modelu

W strukturze kontrolera widoku modelu pojęcie "model" odnosi się do obiektów, które reprezentują dane aplikacji, a także odpowiedniej logiki domeny, która integruje sprawdzanie poprawności i reguły biznesowe. Model jest na wiele sposobów "serca" aplikacji opartej na MVC i w miarę jak zostanie on rozmieszczony w sposób bardziej podstawowy.

Platforma ASP.NET MVC obsługuje używanie dowolnych technologii dostępu do danych, a deweloperzy mogą wybierać spośród różnych zaawansowanych opcji danych platformy .NET, aby zaimplementować ich modele, w tym: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, Subsonic, WilsonORM lub tylko RAW ADO. NET DataReader lub zestawy danych.

W naszej aplikacji NerdDinner będziemy używać LINQ to SQL, aby utworzyć prosty model, który jest ściśle zbliżony do naszego projektu bazy danych i dodaje niestandardową logikę sprawdzania poprawności i reguły biznesowe. Następnie zaimplementujemy klasę repozytorium, która pomaga w rozróżnieniu implementacji trwałości danych od pozostałej części aplikacji, i umożliwi nam łatwe przetestowanie go.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL to obiekt ORM (mapowanie relacyjne obiektów), który jest dostarczany jako część .NET 3,5.

LINQ to SQL zapewnia łatwy sposób mapowania tabel bazy danych na klasy .NET, z których możemy się odkodować. W naszej aplikacji NerdDinner będzie ona używana do mapowania obiadów i tabel RSVP w naszej bazie danych na obiad i RSVP klasy. Kolumny obiadów i tabel RSVP będą odpowiadały właściwościom na obiad i RSVP klas. Każdy obiekt obiadu i RSVP będzie reprezentował osobny wiersz w ramach obiadów lub tabel RSVP w bazie danych.

LINQ to SQL pozwala uniknąć konieczności ręcznego konstruowania instrukcji SQL w celu pobierania i aktualizowania obiektów obiadu i RSVP z danymi bazy danych. Zamiast tego zdefiniujemy klasy obiadu i RSVP, jak są one mapowane do/z bazy danych oraz relacje między nimi. LINQ to SQL następnie będzie obsługiwać generowanie odpowiedniej logiki wykonywania SQL do użycia w czasie wykonywania, gdy będziemy korzystać z nich.

Możemy użyć obsługi języka LINQ w języku VB i C# napisać zapytania ekspresowe, które pobierają obiekty obiadu i RSVP z bazy danych. Pozwala to zminimalizować ilość kodu danych, który należy napisać, i umożliwia nam tworzenie czystych aplikacji.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Dodawanie LINQ to SQL klas do naszego projektu

Rozpocznie się po kliknięciu prawym przyciskiem myszy folderu "models" w naszym projekcie i wybraniu polecenia menu **dodaj&gt;nowy element** :

![](build-a-model-with-business-rule-validations/_static/image1.png)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie nowego elementu". Będziemy filtrować według kategorii "dane" i wybieramy w niej szablon "LINQ to SQL Classes":

![](build-a-model-with-business-rule-validations/_static/image2.png)

Nazwamy elementu "NerdDinner" i kliknij przycisk "Dodaj". Program Visual Studio doda plik NerdDinner. dbml w katalogu \Models, a następnie otworzy projektanta obiektów relacyjnych LINQ to SQL Object:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Tworzenie klas modelu danych za pomocą LINQ to SQL

LINQ to SQL pozwala nam szybko utworzyć klasy modelu danych z istniejącego schematu bazy danych. W tym celu należy otworzyć bazę danych NerdDinner w Eksplorator serwera i wybrać tabele, które chcemy modelować:

![](build-a-model-with-business-rule-validations/_static/image4.png)

Następnie możemy przeciągnąć tabele na powierzchnię projektanta LINQ to SQL. Gdy wykonamy tę LINQ to SQL automatycznie utworzysz klasy obiadu i RSVP przy użyciu schematu tabel (z właściwościami klasy, które są mapowane na kolumny tabeli bazy danych):

![](build-a-model-with-business-rule-validations/_static/image5.png)

Domyślnie Projektant LINQ to SQL automatycznie "zmienia" nazwy tabel i kolumn podczas tworzenia klas opartych na schemacie bazy danych. Na przykład: tabela "obiady" w naszym przykładzie powyżej spowodowało powstanie klasy "obiad". Takie nazywanie klas pomaga zapewnić spójność naszych modeli z konwencjami nazewnictwa platformy .NET, a zazwyczaj znajduje się w tym, że projektant naprawił to wygodne (szczególnie w przypadku dodawania wielu tabel). Jeśli nie podoba Ci się nazwa klasy lub właściwości, które generuje Projektant, możesz zawsze zastąpić ją i zmienić na dowolną żądaną nazwę. Można to zrobić, edytując nazwę jednostki/właściwości w obrębie projektanta lub modyfikując ją za pośrednictwem siatki właściwości.

Domyślnie Projektant LINQ to SQL sprawdza także relacje między kluczami podstawowymi i kluczami obcym tabel, a na podstawie nich automatycznie tworzy domyślne "skojarzenia relacji" między różnymi tworzonymi przez siebie klasami modelu. Na przykład podczas przeciągania obiadów i tabel protokołu RSVP do projektanta LINQ to SQL skojarzenie relacji jeden-do-wielu między nimi zostało wywnioskowane na podstawie faktu, że tabela protokołu RSVP ma klucz obcy do tabeli obiadów (jest to wskazywane przez strzałkę w Projektant):

![](build-a-model-with-business-rule-validations/_static/image6.png)

Powyższe skojarzenie spowoduje, że LINQ to SQL dodać do klasy RSVP właściwość o jednoznacznie określonym typie "obiad", którą deweloperzy mogą wykorzystać w celu uzyskania dostępu do obiadu skojarzonego z daną RSVP. Spowoduje to również, że Klasa obiadu ma mieć Właściwość kolekcji "RSVP", która umożliwia deweloperom pobieranie i aktualizowanie obiektów RSVP skojarzonych z określonym obiadem.

Poniżej można zobaczyć przykład funkcji IntelliSense w programie Visual Studio podczas tworzenia nowego obiektu RSVP i dodawania go do kolekcji RSVP w Obiadie. Zwróć uwagę, jak LINQ to SQL automatycznie dodać kolekcję "RSVP" w obiekcie obiadu:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Dodając obiekt RSVP do kolekcji RSVP w Obiadie, informujemy LINQ to SQL o skojarzeniu z kluczem obcym relacji między obiadem a wierszem RSVP w naszej bazie danych:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Jeśli nie chcesz, aby Projektant utworzył model lub nazwane skojarzenie tabeli, możesz go zastąpić. Po prostu kliknij strzałkę skojarzenia w Projektancie i uzyskaj dostęp do jego właściwości za pośrednictwem siatki właściwości, aby zmienić nazwę, usunąć lub zmodyfikować ją. W naszej aplikacji NerdDinner, chociaż domyślne reguły skojarzenia działają dobrze dla klas modelu danych, które tworzysz, i można po prostu użyć zachowania domyślnego.

### <a name="nerddinnerdatacontext-class"></a>Klasa NerdDinnerDataContext

Program Visual Studio automatycznie utworzy klasy platformy .NET, które reprezentują modele i relacje baz danych zdefiniowane za pomocą projektanta LINQ to SQL. Klasa DataContext LINQ to SQL jest również generowana dla każdego pliku projektanta LINQ to SQL dodanego do rozwiązania. Ze względu na to, że nazywamy nasze LINQ to SQL element klasy "NerdDinner", utworzona klasa DataContext zostanie wywołana "NerdDinnerDataContext". Ta klasa NerdDinnerDataContext jest podstawowym sposobem, w jaki będziemy współdziałać z bazą danych.

Nasze klasy NerdDinnerDataContext udostępniają dwie właściwości — "obiad" i "RSVP" — reprezentujące dwie tabele, które są modelowane w bazie danych. Możemy użyć C# do zapisywania zapytań LINQ względem tych właściwości, aby wykonywać zapytania i pobierać obiekty obiadu i RSVP z bazy danych.

Poniższy kod ilustruje sposób tworzenia wystąpienia obiektu NerdDinnerDataContext i wykonywania zapytania LINQ względem niego w celu uzyskania sekwencji obiadów występujących w przyszłości. Program Visual Studio udostępnia pełną technologię IntelliSense podczas pisania zapytania LINQ, a obiekty zwracane z tego typu są jednoznacznie wpisane i obsługują technologię IntelliSense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Oprócz umożliwienia nam wykonywania zapytań dotyczących obiektów obiad i RSVP, NerdDinnerDataContext automatycznie śledzi wszelkie zmiany, które następnie przejdziemy na obiad i obiekty RSVP, do których pobieramy. Za pomocą tej funkcji można łatwo zapisywać zmiany z powrotem w bazie danych — bez konieczności pisania kodu jawnej aktualizacji SQL.

Na przykład poniższy kod pokazuje, jak używać zapytania LINQ do pobierania pojedynczego obiektu obiadu z bazy danych, aktualizować dwie właściwości obiadu, a następnie zapisywać zmiany z powrotem w bazie danych:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

Obiekt NerdDinnerDataContext w powyższym kodzie automatycznie przeprowadził automatyczne śledzenie zmian właściwości wprowadzonych do obiektu obiadu, który został przez nas pobrany. Po wywołaniu metody "SubmitChanges ()" spowoduje to wykonanie odpowiedniej instrukcji "UPDATE" programu SQL Server w bazie danych, aby zachować zaktualizowane wartości z powrotem.

### <a name="creating-a-dinnerrepository-class"></a>Tworzenie klasy DinnerRepository

W przypadku małych aplikacji czasami warto mieć kontrolery działające bezpośrednio dla LINQ to SQLj klasy DataContext i osadzić zapytania LINQ na kontrolerach. Aplikacje są coraz większe, ale takie podejście jest uciążliwe do utrzymania i przetestowania. Może to również prowadzić do duplikowania tych samych zapytań LINQ w wielu miejscach.

Jednym z rozwiązań, które może ułatwić zachowanie aplikacji i przetestowanie, jest użycie wzorca "Repository". Klasa repozytorium pomaga hermetyzować zapytania dotyczące danych i logikę trwałości oraz deabstrakcyjne szczegóły implementacji trwałości danych z aplikacji. Oprócz tworzenia oczyszczarki kodu aplikacji Używanie wzorca repozytorium może ułatwić zmianę implementacji usługi Data Storage w przyszłości, a także ułatwić testowanie jednostkowe aplikacji bez konieczności rzeczywistej bazy danych.

W naszej aplikacji NerdDinner zdefiniujemy klasę DinnerRepository z następującym podpisem:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Uwaga: w dalszej części tego rozdziału wyodrębnimy interfejs IDinnerRepository z tej klasy i włączysz dla niego iniekcję zależności na naszych kontrolerach. Aby zacząć od, należy zacząć proste i działać bezpośrednio z klasą DinnerRepository.*

Aby zaimplementować tę klasę, należy kliknąć prawym przyciskiem myszy nasz folder "models" i wybrać polecenie menu **dodaj&gt;nowego elementu** . W oknie dialogowym Dodaj nowy element wybierz szablon "Klasa" i nadaj mu nazwę "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

Możemy wdrożyć nasze klasy DinnerRepository, korzystając z poniższego kodu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Pobieranie, aktualizowanie, wstawianie i usuwanie przy użyciu klasy DinnerRepository

Teraz, po utworzeniu naszej klasy DinnerRepository, przyjrzyjmy się kilku przykładom kodu, które demonstrują typowe zadania, które możemy nam wykonać:

#### <a name="querying-examples"></a>Przykłady zapytań

Poniższy kod pobiera pojedynczy obiad przy użyciu wartości DinnerID:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Poniższy kod pobiera wszystkie nadchodzące obiady i pętle na ich temat:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Przykłady wstawiania i aktualizacji

Poniższy kod ilustruje dodanie dwóch nowych obiadów. Dodatki/modyfikacje repozytorium nie są zatwierdzane do bazy danych do momentu wywołania metody "Save ()". LINQ to SQL automatycznie zawija wszystkie zmiany w transakcji bazy danych — w związku z tym wszystkie zmiany są wykonywane lub żadne z nich nie są wykonywane, gdy repozytorium zapisuje:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Poniższy kod pobiera istniejący obiekt obiadu i modyfikuje dwie właściwości. Zmiany są zatwierdzane z powrotem do bazy danych, gdy metoda "Save ()" jest wywoływana w naszym repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Poniższy kod pobiera obiad, a następnie dodaje do niego RSVP. Wykonuje to przy użyciu kolekcji RSVP w obiekcie obiadu, który LINQ to SQL utworzony dla nas (ponieważ istnieje relacja podstawowego klucza/klucza obcego między dwoma w bazie danych). Ta zmiana jest utrwalana w bazie danych jako nowy wiersz tabeli RSVP w przypadku wywołania metody "Save ()" w repozytorium:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Usuń przykład

Poniższy kod pobiera istniejący obiekt obiadu, a następnie oznacza go jako usunięty. Gdy metoda "Save ()" jest wywoływana w repozytorium, spowoduje to usunięcie z powrotem do bazy danych:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integrowanie logiki reguł sprawdzania poprawności i reguły biznesowej z klasami modelu

Integrowanie logiki reguł sprawdzania poprawności i reguły biznesowej jest kluczową częścią wszystkich aplikacji, które współdziałają z danymi.

#### <a name="schema-validation"></a>Sprawdzanie poprawności schematu

Gdy klasy modelu są definiowane przy użyciu projektanta LINQ to SQL, typy danych właściwości w klasach modelu danych odpowiadają typem danych w tabeli bazy. Na przykład: Jeśli kolumna "EventDate" w tabeli obiadów ma wartość "DateTime", Klasa modelu danych utworzona przez LINQ to SQL będzie typu "DateTime" (który jest wbudowanym typem danych .NET). Oznacza to, że podczas próby przypisania wartości całkowitej lub wartości logicznej do jej z kodu zostanie wyświetlony komunikat o błędzie, a próba niejawnego przekonwertowania nieprawidłowego typu ciągu na go w czasie wykonywania zostanie podjęta automatycznie.

LINQ to SQL również automatycznie obsługuje ucieczki wartości SQL w przypadku korzystania z ciągów, co pomaga chronić przed atakami polegającymi na iniekcji kodu SQL podczas korzystania z niego.

#### <a name="validation-and-business-rule-logic"></a>Sprawdzanie poprawności i logika reguły biznesowej

Sprawdzanie poprawności schematu jest przydatne jako pierwszy krok, ale jest rzadko wystarczające. Najpopularniejsze scenariusze wymagają możliwości określenia bogatszej logiki walidacji, która może obejmować wiele właściwości, wykonywać kod i często ma świadomość stanu modelu (na przykład: czy jest tworzony/Updated/Deleted, czy w ramach stanu specyficznego dla domeny, takiego jak "zarchiwizowany"). Istnieją rozmaite różne wzorce i struktury, które mogą służyć do definiowania i stosowania reguł walidacji do klas modeli, a istnieje kilka platform opartych na architekturze .NET, które mogą być używane w tym celu. Możesz użyć całkiem dowolnej z nich w aplikacjach ASP.NET MVC.

Na potrzeby naszej aplikacji NerdDinner będziemy używać stosunkowo prostej i prostej wzorca, w której zostanie ujawniona Właściwość IsValid i Metoda GetRuleViolations () w naszym obiekcie modelu obiadu. Właściwość IsValid zwróci wartość true lub false w zależności od tego, czy sprawdzanie poprawności i reguły biznesowe są prawidłowe. Metoda GetRuleViolations () zwróci listę błędów reguł.

Będziemy implementować IsValid i GetRuleViolations () dla naszego modelu obiadu przez dodanie "klasy częściowej" do projektu. Klasy częściowe mogą służyć do dodawania metod/właściwości/zdarzeń do klas obsługiwanych przez projektanta VS (na przykład w przypadku klasy obiadu wygenerowanej przez projektanta LINQ to SQL) i pomóc w uniknięciu tego narzędzia w kodzie. Do projektu można dodać nową klasę częściową, klikając prawym przyciskiem myszy folder \Models, a następnie wybierając polecenie menu "Dodaj nowy element". Następnie możemy wybrać szablon "Klasa" w oknie dialogowym "Dodawanie nowego elementu" i nadaj mu nazwę Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Kliknięcie przycisku "Dodaj" spowoduje dodanie pliku Dinner.cs do projektu i otwarcie go w środowisku IDE. Następnie możemy zaimplementować podstawową regułę/strukturę wymuszania walidacji przy użyciu poniższego kodu:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Kilka informacji o powyższym kodzie:

- Klasa obiadu jest poprzedzona słowem kluczowym "częściowe", co oznacza, że kod zawarty w nim zostanie połączony z klasą wygenerowaną/obsługiwaną przez projektanta LINQ to SQL i skompilowana w jednej klasie.
- Klasa RuleViolation jest klasą pomocników dodawaną do projektu, który umożliwia nam dostarczenie szczegółowych informacji o naruszeniu reguły.
- Metoda obiad. GetRuleViolations () powoduje, że nasze weryfikacje i reguły biznesowe są oceniane (wkrótce zaimplementujemy je). Następnie zwraca sekwencję obiektów RuleViolation, które zawierają więcej szczegółowych informacji o błędach reguł.
- Właściwość obiad. IsValid udostępnia wygodną Właściwość pomocnika, która wskazuje, czy obiekt obiadu ma wszystkie aktywne RuleViolations. Może być aktywnie sprawdzana przez dewelopera przy użyciu obiektu obiadu w dowolnym momencie (i nie zgłasza wyjątku).
- Metoda częściowa "obiad. OnValidate ()" jest punktem zaczepienia, który LINQ to SQL zapewnia, że wszyscy z nich otrzymają powiadomienie, gdy obiekt obiadu zostanie utrwalony w bazie danych. Nasze wdrożenie OnValidate () powyżej gwarantuje, że obiad nie ma RuleViolations przed jego zapisaniem. Jeśli jest w nieprawidłowym stanie, zgłasza wyjątek, co spowoduje, że LINQ to SQL przerywa transakcji.

To podejście zapewnia prostą strukturę, którą możemy zintegrować weryfikację i reguły biznesowe. Teraz dodajmy poniższe reguły do naszej metody GetRuleViolations ():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Używamy funkcji "yield return", C# aby zwrócić sekwencję dowolnych RuleViolations. Pierwsze sześć reguł sprawdza powyżej po prostu wymuszenie, że właściwości ciągu na Obiadie nie mogą mieć wartości null ani być puste. Ostatnia reguła jest nieco bardziej interesująca i wywołuje metodę pomocnika PhoneValidator. IsValidNumber (), którą można dodać do naszego projektu, aby sprawdzić, czy format numeru ContactPhone jest zgodny z krajem obiadu.

Możemy użyć. Obsługa wyrażenia regularnego w sieci w celu zaimplementowania tej obsługi sprawdzania poprawności telefonu. Poniżej znajduje się prosta implementacja PhoneValidator, którą możemy dodać do naszego projektu, który pozwala nam dodawać testy wzorca wyrażenia regularnego specyficzne dla kraju:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Obsługa walidacji i naruszeń logiki biznesowej

Teraz, po dodaniu powyższego kodu sprawdzania poprawności i reguły biznesowej, zawsze, gdy spróbujemy utworzyć lub zaktualizować obiad, zostaną ocenione i wymuszone reguły logiki walidacji.

Deweloperzy mogą napisać kod jak poniżej, aby aktywnie określić, czy obiekt obiadu jest prawidłowy, i pobrać listę wszystkich naruszeń w nim bez ponoszenia wyjątków:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Jeśli podjęto próbę zapisania obiadu w nieprawidłowym stanie, zostanie zgłoszony wyjątek podczas wywoływania metody Save () na DinnerRepository. Dzieje się tak, ponieważ LINQ to SQL automatycznie wywołuje nasz obiad. OnValidate () — Metoda częściowa przed zapisaniem zmian obiadu i dodaliśmy kod do obiadu. OnValidate (), aby zgłosić wyjątek, jeśli jakiekolwiek naruszenia reguł istnieją na Obiadie. Możemy przechwycić ten wyjątek i ponownie pobrać listę naruszeń, które należy rozwiązać:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Ponieważ nasze reguły sprawdzania poprawności i biznesowe zostały zaimplementowane w ramach naszej warstwy modelu, a nie w warstwie interfejsu użytkownika, zostaną one zastosowane i użyte we wszystkich scenariuszach w naszej aplikacji. Możemy później zmienić lub dodać reguły biznesowe oraz cały kod, który współdziała z naszymi obiektami obiadu.

Możliwość zmiany reguł firmy w jednym miejscu, bez konieczności wprowadzania zmian w całej aplikacji i logiki interfejsu użytkownika, jest znakiem dobrze nadanej aplikacji i korzyścią, którą może pomóc architektura MVC.

### <a name="next-step"></a>Następny krok

Mamy już model, za pomocą którego możemy wysyłać zapytania i aktualizować bazę danych.

Teraz Dodajmy do projektu niektóre kontrolery i widoki, których można użyć do utworzenia środowiska interfejsu użytkownika HTML.

> [!div class="step-by-step"]
> [Poprzednie](create-a-database.md)
> [dalej](use-controllers-and-views-to-implement-a-listingdetails-ui.md)

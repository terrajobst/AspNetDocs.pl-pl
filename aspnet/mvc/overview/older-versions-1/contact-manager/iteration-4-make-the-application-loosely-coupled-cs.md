---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Iteracja #4 — Przekształć aplikację w luźną (C#) | Microsoft Docs'
author: microsoft
description: W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Dla...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: ce8e3c4ff8a59be9f2f572813db599604216119d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544458"
---
# <a name="iteration-4--make-the-application-loosely-coupled-c"></a>#4 iteracji — Przekształć aplikację w luźny sposób (C#)

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Tworzenie aplikacji do zarządzania kontaktami ASP.NET MVCC#()

W tej serii samouczków tworzymy całą aplikację do zarządzania kontaktami od początku do końca. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazw, numerów telefonów i adresów e-mail — w celu uzyskania listy osób.

Aplikacja została utworzona przez wiele iteracji. W przypadku każdej iteracji stopniowo ulepszamy aplikację. Celem tej wielu iteracji jest umożliwienie zrozumienia przyczyny każdej zmiany.

- #1 iteracji — Utwórz aplikację. W pierwszej iteracji tworzymy Menedżera kontaktów w najprostszy sposób. Dodaliśmy obsługę podstawowych operacji bazy danych: Tworzenie, odczytywanie, aktualizowanie i usuwanie (CRUD).

- Iteracja #2 — Zwiększ wygląd aplikacji. W tej iteracji ulepszamy wygląd aplikacji, modyfikując domyślną stronę wzorcową widoku MVC ASP.NET i kaskadowy arkusz stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzeciej iteracji zostanie dodana podstawowa Walidacja formularza. Uniemożliwiamy użytkownikom przesyłanie formularza bez wykonywania wymaganych pól formularza. Sprawdzamy również adresy e-mail i numery telefonów.

- Iteracja #4 — możliwość swobodnego łączenia aplikacji. W tej czwartej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład Refaktoryzacja naszej aplikacji używa wzorca repozytorium i wzorca iniekcji zależności.

- #5 iteracji — Utwórz testy jednostkowe. W piątej iteracji upraszczamy obsługę i modyfikację naszej aplikacji przez dodanie testów jednostkowych. Tworzymy klasy modelu danych i kompilujemy testy jednostkowe dla naszych kontrolerów i logiki walidacji.

- Iteracja #6 — Użyj programowania opartego na testach. W tej szóstej iteracji Dodaliśmy nowe funkcje do naszej aplikacji, pisząc testy jednostkowe jako pierwsze i pisząc kod na testach jednostkowych. W tej iteracji dodamy grupy kontaktów.

- Iteracja #7 — Dodawanie funkcji AJAX. W siódmej iteracji poprawimy czas reakcji i wydajność naszej aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Ta iteracja

W tej czwartej iteracji aplikacji Contact Manager Refaktoryzacja aplikacji spowoduje, że aplikacja nie będzie bardziej luźno powiązana. Gdy aplikacja jest luźno sprzężona, można zmodyfikować kod w jednej części aplikacji bez konieczności modyfikowania kodu w innych częściach aplikacji. Coraz częściej połączone aplikacje są bardziej odporne na zmiany.

Obecnie cała logika dostępu do danych i walidacji używana przez aplikację Menedżer kontaktów jest zawarta w klasach kontrolerów. Jest to niewłaściwy pomysł. Za każdym razem, gdy zajdzie potrzeba modyfikacji jednej części aplikacji, ryzyko wprowadzenia usterek do innej części aplikacji. Na przykład w przypadku modyfikacji logiki walidacji ryzyko wprowadzenia nowych usterek do logiki dostępu do danych lub kontrolera.

> [!NOTE] 
> 
> (SRP), Klasa nigdy nie powinna mieć więcej niż jedną przyczynę do zmiany. Mieszanie kontrolerów, sprawdzania poprawności i logiki bazy danych jest olbrzymim naruszeniem pojedynczej zasady odpowiedzialności.

Istnieje kilka powodów, dla których może być konieczne zmodyfikowanie aplikacji. Może być konieczne dodanie nowej funkcji do aplikacji, może być konieczne naprawienie usterki w aplikacji lub może zajść potrzeba zmodyfikowania sposobu implementacji funkcji aplikacji. Aplikacje są rzadko statyczne. Są one skłonne do wzrostu i mutacji w czasie.

Załóżmy na przykład, że użytkownik zdecyduje się zmienić sposób implementacji warstwy dostępu do danych. Teraz aplikacja Contact Manager używa Entity Framework firmy Microsoft w celu uzyskania dostępu do bazy danych. Można jednak zdecydować się na migrację do nowej lub alternatywnej technologii dostępu do danych, takiej jak ADO.NET Data Services lub NHibernate. Jednak ze względu na to, że kod dostępu do danych nie jest odizolowany od kodu sprawdzania poprawności i kontrolera, nie ma możliwości modyfikacji kodu dostępu do danych w aplikacji bez modyfikowania innego kodu, który nie jest bezpośrednio związany z dostępem do danych.

Gdy aplikacja jest luźno powiązana, można wprowadzać zmiany w jednej części aplikacji bez dotykania innych części aplikacji. Można na przykład przełączyć technologie dostępu do danych bez modyfikowania logiki walidacji lub kontrolera.

W tej iteracji wykorzystujemy kilka wzorców projektowych oprogramowania, które umożliwiają nam refaktoryzację naszej aplikacji Contact Manager do bardziej luźno połączonej aplikacji. Gdy wszystko będzie gotowe, Menedżer kontaktów wykupił t Zrób wszystko, co nie było wcześniej zrobione. Jednak będzie można łatwiej zmieniać aplikację w przyszłości.

> [!NOTE] 
> 
> Refaktoryzacja to proces ponownego zapisywania aplikacji w taki sposób, że nie utraci żadnych istniejących funkcji.

## <a name="using-the-repository-software-design-pattern"></a>Używanie wzorca projektowego oprogramowania do repozytorium

Pierwsza zmiana polega na wykorzystaniu wzorca projektowego oprogramowania zwanego wzorcem repozytorium. Użyjemy wzorca repozytorium, aby odizolować nasz kod dostępu do danych od reszty naszej aplikacji.

Implementacja wzorca repozytorium wymaga wykonania następujących dwóch kroków:

1. Tworzenie interfejsu
2. Utwórz konkretną klasę, która implementuje interfejs

Najpierw musimy utworzyć interfejs, który opisuje wszystkie metody dostępu do danych, które muszą zostać wykonane. Interfejs IContactManagerRepository jest zawarty w aukcji 1. Ten interfejs opisuje pięć metod: DeleteContact (), EditContact (), GetContact i ListContacts ().

**Lista 1 — Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Następnie musimy utworzyć konkretną klasę, która implementuje interfejs IContactManagerRepository. Ponieważ korzystamy z Entity Framework firmy Microsoft w celu uzyskania dostępu do bazy danych, utworzymy nową klasę o nazwie EntityContactManagerRepository. Ta klasa jest zawarta w liście 2.

**Lista 2 — Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Należy zauważyć, że Klasa EntityContactManagerRepository implementuje interfejs IContactManagerRepository. Klasa implementuje wszystkie pięć metod opisanych przez ten interfejs.

Możesz się zastanawiać, dlaczego musimy bother z interfejsem. Dlaczego musimy utworzyć interfejs i klasę, która go implementuje?

Z jednym wyjątkiem pozostała część naszej aplikacji będzie współpracująca z interfejsem, a nie z konkretną klasą. Zamiast wywoływania metod uwidocznionych przez klasę EntityContactManagerRepository, wywołamy metody udostępniane przez interfejs IContactManagerRepository.

Dzięki temu można zaimplementować interfejs z nową klasą bez konieczności modyfikowania pozostałej części naszej aplikacji. Na przykład w pewnym czasie może być konieczne zaimplementowanie klasy DataServicesContactManagerRepository implementującej interfejs IContactManagerRepository. Klasa DataServicesContactManagerRepository może używać ADO.NET Data Services, aby uzyskać dostęp do bazy danych, a nie Entity Framework Microsoft.

Jeśli nasz kod aplikacji jest zaprogramowany przy użyciu interfejsu IContactManagerRepository zamiast konkretnej klasy EntityContactManagerRepository, możemy przełączyć konkretne klasy bez modyfikowania żadnego z pozostałych naszych kodów. Można na przykład przełączać się z klasy EntityContactManagerRepository do klasy DataServicesContactManagerRepository bez modyfikowania dostępu do danych ani logiki walidacji.

Programowanie względem interfejsów (abstrakcji) zamiast konkretnych klas sprawia, że nasza aplikacja jest bardziej odporna na zmianę.

> [!NOTE] 
> 
> Możesz szybko utworzyć interfejs z klasy konkretnej w programie Visual Studio, wybierając refaktoryzację opcji menu, Wyodrębnij interfejs. Na przykład, można najpierw utworzyć klasę EntityContactManagerRepository, a następnie użyć polecenie Wyodrębnij interfejs, aby automatycznie wygenerować interfejs IContactManagerRepository.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Korzystanie z wzorca tworzenia oprogramowania do iniekcji zależności

Po przeprowadzeniu migracji kodu dostępu do danych do osobnej klasy repozytorium należy zmodyfikować nasz kontroler Contact, aby używał tej klasy. Będziemy korzystać ze wzorca projektowego oprogramowania zwanego iniekcją zależności, aby użyć klasy repozytorium w naszym kontrolerze.

Zmodyfikowany kontroler kontaktu znajduje się na liście 3.

**Lista 3 — Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Zwróć uwagę, że kontroler Contact na liście 3 ma dwa konstruktory. Pierwszy Konstruktor przekazuje konkretne wystąpienie interfejsu IContactManagerRepository do drugiego konstruktora. Klasa kontrolera kontaktów używa *iniekcji zależności konstruktora*.

To i miejsce, w którym jest używana Klasa EntityContactManagerRepository, znajduje się w pierwszym konstruktorze. Pozostała część klasy używa interfejsu IContactManagerRepository zamiast konkretnej klasy EntityContactManagerRepository.

Dzięki temu można łatwo przełączać implementacje klasy IContactManagerRepository w przyszłości. Jeśli chcesz użyć klasy DataServicesContactRepository zamiast klasy EntityContactManagerRepository, wystarczy zmodyfikować pierwszego konstruktora.

Iniekcja zależności konstruktora sprawia, że klasa kontrolera kontaktu bardzo weryfikowalne. W testach jednostkowych można utworzyć wystąpienie kontrolera Contact, przekazując implementację IContactManagerRepository klasy. Ta funkcja iniekcji zależności będzie bardzo ważna dla nas w następnej iteracji podczas kompilowania testów jednostkowych dla aplikacji Contact Manager.

> [!NOTE] 
> 
> Jeśli chcesz całkowicie oddzielić klasę kontrolera Contact od konkretnej implementacji interfejsu IContactManagerRepository, możesz skorzystać z platformy obsługującej iniekcję zależności, np. StructureMap lub Microsoft Entity Framework (MEF). Korzystając z zalet platformy iniekcji zależności, nigdy nie trzeba odwoływać się do konkretnej klasy w kodzie.

## <a name="creating-a-service-layer"></a>Tworzenie warstwy usług

Być może zauważono, że nasza logika walidacji jest nadal mieszana przy użyciu logiki kontrolera w zmodyfikowanej klasie kontrolera z listy 3. Z tego samego powodu dobrym pomysłem jest wyizolowanie logiki dostępu do danych. dobrym pomysłem jest odizolowanie logiki walidacji.

Aby rozwiązać ten problem, można utworzyć oddzielną [*warstwę usług*](http://martinfowler.com/eaaCatalog/serviceLayer.html). Warstwa usługi jest oddzielną warstwą, którą można wstawiać między naszymi klasami kontrolera i repozytorium. Warstwa usługi zawiera naszą logikę biznesową, w tym całą logikę walidacji.

ContactManagerService znajduje się na liście 4. Zawiera logikę walidacji z klasy kontrolera Contact.

**Lista 4 — Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Należy zauważyć, że Konstruktor dla ContactManagerService wymaga elementu ValidationDictionary. Warstwa usługi komunikuje się z warstwą kontrolera za pomocą tego ValidationDictionary. Szczegółowo omawiamy ValidationDictionary w poniższej sekcji podczas omawiania wzorca dekoratora.

Należy zauważyć, że ContactManagerService implementuje interfejs IContactManagerService. Należy zawsze dążyć do programowania interfejsów zamiast konkretnych klas. Inne klasy w aplikacji menedżera kontaktów nie współpracują z klasą ContactManagerService. Zamiast tego z jednym wyjątkiem, pozostała część aplikacji Menedżer kontaktów jest zaprogramowana na interfejs IContactManagerService.

Interfejs IContactManagerService jest zawarty w liście 5.

**Lista 5 — Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

Zmodyfikowana Klasa kontrolera kontaktów jest zawarta w liście 6. Zwróć uwagę, że kontroler kontaktu nie współdziała już z repozytorium ContactManager. Zamiast tego kontroler kontaktu współdziała z usługą ContactManager. Każda warstwa jest izolowana możliwie jak najwięcej z innych warstw.

**Lista 6 — Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Nasza aplikacja nie działa już afoul z pojedynczą regułą odpowiedzialności (SRP). Kontroler kontaktu na liście 6 został oddzielonych każdą odpowiedzialnością inną niż kontrola przepływu wykonywania aplikacji. Cała logika walidacji została usunięta z kontrolera Contact i wypchnięcia do warstwy usługi. Cała logika bazy danych została przekazana do warstwy repozytorium.

## <a name="using-the-decorator-pattern"></a>Używanie wzorca Dekoratora

Chcemy, aby całkowicie oddzielić warstwę usług od naszej warstwy kontrolera. W zasadzie powinna być możliwe skompilowanie naszej warstwy usług w osobnym zestawie z naszej warstwy kontrolera bez konieczności dodawania odwołania do naszej aplikacji MVC.

Jednak nasza warstwa usług musi mieć możliwość przekazywania komunikatów o błędach walidacji z powrotem do warstwy kontrolera. Jak można włączyć komunikację między warstwą usługi a komunikatami o błędach weryfikacji bez sprzęgania kontrolera i warstwy usług? Możemy wykorzystać Wzorzec projektowy oprogramowania o nazwie [wzorzec dekoratora](http://en.wikipedia.org/wiki/Decorator_pattern).

Kontroler używa ModelStateDictionary o nazwie ModelState do reprezentowania błędów walidacji. W związku z tym może być skłonny do przekazania ModelState z warstwy kontrolera do warstwy usług. Jednak użycie ModelState w warstwie usług spowoduje, że warstwa usług zależała od funkcji platformy MVC ASP.NET. Może to być niewłaściwe, ponieważ, Someday, możesz chcieć użyć warstwy usługi z aplikacją WPF zamiast aplikacji ASP.NET MVC. W takim przypadku nie chcesz odwoływać się do struktury ASP.NET MVC, aby użyć klasy ModelStateDictionary.

Wzorzec dekoratora umożliwia Zawijanie istniejącej klasy w nowej klasie w celu zaimplementowania interfejsu. Nasz projekt Menedżera kontaktów zawiera klasę ModelStateWrapper zawartą w liście 7. Klasa ModelStateWrapper implementuje interfejs na liście 8.

**Lista 7 — Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Lista 8-Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Jeśli chcesz przejrzeć listę 5, zobaczysz, że warstwa usługi ContactManager korzysta wyłącznie z interfejsu IValidationDictionary. Usługa ContactManager nie jest zależna od klasy ModelStateDictionary. Gdy kontroler kontaktu tworzy usługę ContactManager, kontroler zawija swój ModelState w następujący sposób:

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Podsumowanie

W tej iteracji nie dodano żadnych nowych funkcji do aplikacji Contact Manager. Celem tej iteracji była Refaktoryzacja aplikacji Contact Manager, dzięki czemu można łatwiej ją konserwować i modyfikować.

Najpierw wprowadziliśmy Wzorzec projektowy oprogramowania repozytorium. Wszystkie kody dostępu do danych zostały zmigrowane do oddzielnej klasy repozytorium ContactManager.

Wykryliśmy również logikę walidacji z naszej logiki kontrolera. Utworzyliśmy oddzielną warstwę usług, która zawiera cały kod weryfikacyjny. Warstwa kontrolera współdziała z warstwą usługi, a warstwa usługi współdziała z warstwą repozytorium.

Po utworzeniu warstwy usługi korzystamy ze wzorca dekoratora, aby odizolować ModelState od naszej warstwy usług. W naszej warstwie usług zaprogramowanymy do interfejsu IValidationDictionary zamiast ModelState.

Wreszcie wykorzystano Wzorzec projektowy oprogramowania o nazwie wzorzec iniekcji zależności. Ten wzorzec pozwala nam na programowanie interfejsów (abstrakcji) zamiast konkretnych klas. Implementacja wzorca projektowania iniekcji zależności sprawia również, że nasz kod weryfikowalne. W następnej iteracji dodamy testy jednostkowe do projektu.

> [!div class="step-by-step"]
> [Poprzednie](iteration-3-add-form-validation-cs.md)
> [dalej](iteration-5-create-unit-tests-cs.md)

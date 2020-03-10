---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Tworzenie warstwy dostępu do danych | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 0fcf050474a57be9ed53ec0783a6d6b7dde2bf4c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544927"
---
# <a name="create-the-data-access-layer"></a>Tworzenie warstwy dostępu do danych

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku opisano sposób tworzenia, uzyskiwania dostępu i przeglądania danych z bazy danych przy użyciu formularzy sieci Web ASP.NET i Code First Entity Framework. Ten samouczek kompiluje się w poprzednim samouczku "Tworzenie projektu" i jest częścią serii samouczków dotyczących sklepu Wingtip. Po ukończeniu tego samouczka utworzysz grupę klas dostępu do danych, które znajdują się w folderze *modele* projektu.

## <a name="what-youll-learn"></a>Zawartość:

- Jak utworzyć modele danych.
- Jak zainicjować i wypełniać bazę danych.
- Jak zaktualizować i skonfigurować aplikację do obsługi bazy danych.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Oto funkcje wprowadzone w samouczku:

- Entity Framework Code First
- LocalDB
- Adnotacje danych

## <a name="creating-the-data-models"></a>Tworzenie modeli danych

[Entity Framework](https://msdn.microsoft.com/data/aa937723) to struktura mapowania obiektów (ORM). Umożliwia ona korzystanie z danych relacyjnych jako obiektów, eliminując większość kodu dostępu do danych, który zwykle jest potrzebny do zapisu. Za pomocą Entity Framework można wydać zapytania przy użyciu [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), a następnie pobrać i manipulować danymi jako obiektami silnie wpisaną. LINQ zawiera wzorce do wykonywania zapytań i aktualizowania danych. Korzystanie z Entity Framework pozwala skupić się na tworzeniu reszty aplikacji, a nie skoncentrowaniu się na podstawowych podstawach dostępu do danych. W dalszej części tej serii samouczków pokazano, jak używać danych w celu wypełnienia nawigacji i zapytań dotyczących produktów.

Entity Framework obsługuje model programistyczny o nazwie *Code First*. Code First umożliwia definiowanie modeli danych przy użyciu klas. Klasa to konstrukcja, która umożliwia tworzenie własnych typów niestandardowych przez grupowanie zmiennych innych typów, metod i zdarzeń. Można mapować klasy do istniejącej bazy danych lub użyć ich do wygenerowania bazy danych. W tym samouczku utworzysz modele danych przez zapisanie klas modelu danych. Następnie można Entity Framework utworzyć bazę danych na bieżąco z tych nowych klas.

Zacznij od utworzenia klas jednostek, które definiują modele danych dla aplikacji formularzy sieci Web. Następnie utworzysz klasę kontekstu, która zarządza klasami jednostek i zapewnia dostęp do danych w bazie danych. Utworzysz również klasę inicjatora, która będzie używana do wypełniania bazy danych.

### <a name="entity-framework-and-references"></a>Entity Framework i odwołania

Domyślnie Entity Framework jest uwzględniana podczas tworzenia nowej **aplikacji sieci web ASP.NET** przy użyciu szablonu **formularzy sieci Web** . Entity Framework można instalować, odinstalowywać i aktualizować jako pakiet NuGet.

Ten pakiet NuGet obejmuje następujące zestawy **środowiska uruchomieniowego** w ramach projektu:

- EntityFramework. dll — wszystkie typowe kody środowiska uruchomieniowego używane przez Entity Framework
- EntityFramework. SqlServer. dll — dostawca Microsoft SQL Server dla Entity Framework

### <a name="entity-classes"></a>Klasy jednostek

Klasy tworzone w celu zdefiniowania schematu danych są nazywane klasami jednostek. Jeśli dopiero zaczynasz Projektowanie bazy danych, pomyśl o klasach jednostek jako definicje tabel bazy danych. Każda właściwość w klasie Określa kolumnę w tabeli bazy danych. Klasy te zapewniają uproszczony, relacyjny interfejs między kodem zorientowanym na obiekt i strukturą tabeli relacyjnej bazy danych.

W tym samouczku rozpocznie się dodawanie prostych klas jednostek reprezentujących schematy dla produktów i kategorii. Klasa Products zawiera definicje dla każdego produktu. Nazwa każdego z elementów członkowskich klasy Product będzie `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`i `Category`. Klasa kategorii będzie zawierać definicje dla każdej kategorii, do której może należeć produkt, np. samochodu, łodzi lub płaszczyzny. Nazwa każdego z elementów członkowskich klasy kategorii będzie `CategoryID`, `CategoryName`, `Description`i `Products`. Każdy produkt należy do jednej z kategorii. Te klasy jednostek zostaną dodane do folderu istniejące *modele* projektu.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** -&gt; **nowy element**. 

    ![Tworzenie warstwy dostępu do danych — menu nowy element](create_the_data_access_layer/_static/image1.png)

   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. W **obszarze C# Wizualizacja** z **zainstalowanego** okienka po lewej stronie wybierz pozycję **kod**. 

    ![Tworzenie warstwy dostępu do danych — menu nowy element](create_the_data_access_layer/_static/image2.png)
3. Wybierz pozycję **Klasa** w środkowym okienku i nadaj jej nazwę *Product.cs*.
4. Kliknij pozycję **Add** (Dodaj).  
   Nowy plik klasy zostanie wyświetlony w edytorze.
5. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Utwórz inną klasę przez powtórzenie kroków od 1 do 4, jednak Nadaj nazwę nowej klasie *Category.cs* i Zastąp kod domyślny następującym kodem:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Jak wspomniano wcześniej, Klasa `Category` reprezentuje typ produktu, który ma być sprzedawany przez aplikację (np. <a id="a"></a>&quot;samochodów&quot;, &quot;łodzi&quot;, &quot;rakiet&quot;, i tak dalej), a Klasa `Product` reprezentuje poszczególne produkty (zabawki) w bazie danych. Każde wystąpienie obiektu `Product` będzie odpowiadać wierszowi w tabeli relacyjnej bazy danych, a każda właściwość klasy Product zostanie zamapowana na kolumnę w tabeli relacyjnej bazy danych. W dalszej części tego samouczka przejrzyjesz dane produktu zawarte w bazie danych programu.

### <a name="data-annotations"></a>Adnotacje danych

Być może zauważono, że niektórzy członkowie klas mają atrybuty określające szczegóły dotyczące elementu członkowskiego, takie jak `[ScaffoldColumn(false)]`. Są to *Adnotacje danych*. Atrybuty adnotacji danych opisują sposób sprawdzania poprawności danych wejściowych użytkownika dla tego elementu członkowskiego, określania jego formatowania i określania sposobu modelowania podczas tworzenia bazy danych.

### <a name="context-class"></a>Context — Klasa

Aby rozpocząć korzystanie z klas do uzyskiwania dostępu do danych, należy zdefiniować klasę kontekstową. Jak wspomniano wcześniej, Klasa kontekstu zarządza klasami jednostek (takimi jak Klasa `Product` i Klasa `Category`) i zapewnia dostęp do danych w bazie danych.

Ta procedura powoduje dodanie nowej C# klasy kontekstu do folderu *models* .

1. Kliknij prawym przyciskiem myszy folder *modele* , a następnie wybierz pozycję **Dodaj** -&gt; **nowy element**.   
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Wybierz pozycję **Klasa** w środkowym okienku, nadaj jej nazwę *ProductContext.cs* i kliknij przycisk **Dodaj**.
3. Zastąp domyślny kod zawarty w klasie następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Ten kod dodaje `System.Data.Entity` przestrzeń nazw, dzięki czemu masz dostęp do wszystkich podstawowych funkcji Entity Framework, które obejmują możliwość wykonywania zapytań, wstawiania, aktualizowania i usuwania danych przez pracę z silnie określonymi obiektami.

Klasa `ProductContext` reprezentuje kontekst bazy danych produktu Entity Framework, który obsługuje pobieranie, przechowywanie i aktualizowanie wystąpień klasy `Product` w bazie danych. Klasa `ProductContext` pochodzi od klasy bazowej `DbContext` dostarczonej przez Entity Framework.

### <a name="initializer-class"></a>Klasa inicjatora

Musisz uruchomić pewną logikę niestandardową, aby zainicjować bazę danych przy pierwszym użyciu kontekstu. Pozwoli to na dodanie danych inicjatora do bazy danych, dzięki czemu można natychmiast wyświetlić produkty i kategorie.

Ta procedura powoduje dodanie nowej C# klasy inicjatora do folderu *models* .

1. Utwórz kolejną `Class` w folderze *modele* i nadaj jej nazwę *ProductDatabaseInitializer.cs*.
2. Zastąp domyślny kod zawarty w klasie następującym kodem:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Jak widać w powyższym kodzie, po utworzeniu i zainicjowaniu bazy danych Właściwość `Seed` zostanie zastąpiona i ustawiona. Po ustawieniu właściwości `Seed`, wartości z kategorii i produktów są używane do wypełniania bazy danych. Jeśli spróbujesz zaktualizować dane inicjatora, modyfikując powyższy kod po utworzeniu bazy danych, podczas uruchamiania aplikacji sieci Web nie będą wyświetlane żadne aktualizacje. Przyczyną jest użycie w powyższym kodzie implementacji klasy `DropCreateDatabaseIfModelChanges`, aby rozpoznać, czy model (schemat) został zmieniony przed zresetowaniem danych inicjatora. Jeśli nie wprowadzono żadnych zmian do `Category` i `Product` klas jednostek, baza danych nie zostanie zainicjowana ponownie przy użyciu danych inicjatora.

> [!NOTE] 
> 
> Jeśli chcesz, aby baza danych była ponownie tworzona przy każdym uruchomieniu aplikacji, można użyć klasy `DropCreateDatabaseAlways` zamiast klasy `DropCreateDatabaseIfModelChanges`. Jednak w tej serii samouczków należy użyć klasy `DropCreateDatabaseIfModelChanges`.

W tym punkcie w tym samouczku będziesz mieć folder *modele* z czterema nowymi klasami i jedną domyślną klasą:

![Tworzenie folderu modele warstwy dostępu do danych](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Konfigurowanie aplikacji do korzystania z modelu danych

Teraz, po utworzeniu klas, które reprezentują dane, musisz skonfigurować aplikację tak, aby korzystała z klas. W pliku *Global. asax* należy dodać kod, który inicjuje model. W pliku *Web. config* dodawane są informacje informujące aplikację, która baza danych ma być używana do przechowywania danych reprezentowanych przez nowe klasy danych. Plik *Global. asax* może służyć do obsługi zdarzeń lub metod aplikacji. Plik *Web. config* pozwala kontrolować konfigurację aplikacji sieci Web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Aktualizowanie pliku Global. asax

Aby zainicjować modele danych podczas uruchamiania aplikacji, należy zaktualizować procedurę obsługi `Application_Start` w pliku *Global.asax.cs* .

> [!NOTE] 
> 
> W Eksplorator rozwiązań można wybrać plik *Global. asax* lub plik *Global.asax.cs* , aby edytować plik *Global.asax.cs* .

1. Dodaj następujący kod wyróżniony kolorem żółtym do metody `Application_Start` w pliku *Global.asax.cs* .   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Przeglądarka musi obsługiwać kod wyróżniony przy użyciu języka HTML5 podczas wyświetlania tej serii samouczków w przeglądarce.

Jak pokazano w powyższym kodzie, gdy aplikacja zostanie uruchomiona, aplikacja określa inicjator, który zostanie uruchomiony podczas pierwszego dostępu do danych. Do uzyskania dostępu do obiektu `Database` i obiektu `ProductDatabaseInitializer` są wymagane dwa dodatkowe przestrzenie nazw.

 Modyfikowanie pliku Web. config 

Mimo że usługa Entity Framework Code First wygeneruje bazę danych dla Ciebie w domyślnej lokalizacji, gdy baza danych zostanie wypełniona danymi o inicjatorze, dodanie własnych informacji o połączeniu do aplikacji zapewnia kontrolę nad lokalizacją bazy danych. To połączenie z bazą danych należy określić przy użyciu parametrów połączenia w pliku *Web. config* aplikacji w katalogu głównym projektu. Dodając nowe parametry połączenia, można skierować lokalizację bazy danych (*wingtiptoys. mdf*) do skompilowania w katalogu danych aplikacji ( *\_danych aplikacji*), a nie w domyślnej lokalizacji. Wprowadzenie tej zmiany umożliwi znalezienie i inspekcję pliku bazy danych w dalszej części tego samouczka.

1. W **Eksplorator rozwiązań**Znajdź i Otwórz plik *Web. config* .
2. Dodaj parametry połączenia wyróżnione kolorem żółtym do sekcji `<connectionStrings>` w pliku *Web. config* w następujący sposób:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Gdy aplikacja jest uruchamiana po raz pierwszy, spowoduje skompilowanie bazy danych w lokalizacji określonej przez parametry połączenia. Ale przed uruchomieniem aplikacji Utwórzmy ją w pierwszej kolejności.

## <a name="building-the-application"></a>Kompilowanie aplikacji

Aby upewnić się, że wszystkie klasy i zmiany w aplikacji sieci Web działają, należy skompilować aplikację.

1. Z menu **Debuguj** wybierz polecenie **Build WingtipToys**.  
 Zostanie wyświetlone okno **dane wyjściowe** , a jeśli wszystko zostało prawidłowo *wykonane* , zobaczysz komunikat o błędzie.  

    ![Tworzenie warstwy dostępu do danych — okna danych wyjściowych](create_the_data_access_layer/_static/image4.png)

Jeśli wystąpi błąd, ponownie sprawdź powyższe kroki. Informacje w oknie **danych wyjściowych** wskazują, który plik ma problem i gdzie w pliku jest wymagana zmiana. Te informacje umożliwią określenie, jakie części powyższych kroków należy przejrzeć i naprawić w projekcie.

## <a name="summary"></a>Podsumowanie

W tym samouczku dla serii, w której utworzono model danych, a także dodano kod, który będzie używany do inicjowania i wypełniania bazy danych. Aplikacja została również skonfigurowana do korzystania z modeli danych podczas uruchamiania aplikacji.

W następnym samouczku należy zaktualizować interfejs użytkownika, dodać nawigację i pobrać dane z bazy danych. Spowoduje to, że baza danych zostanie automatycznie utworzona na podstawie klas jednostek utworzonych w tym samouczku.

## <a name="additional-resources"></a>Dodatkowe materiały

[Entity Framework przegląd](https://msdn.microsoft.com/library/bb399567.aspx)   
[Przewodnik początkującego ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Programowanie Code First przy użyciu Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (wideo)   
[Code First relacje między interfejsem API Fluent](https://msdn.microsoft.com/data/hh134698)   
[Code First adnotacje danych](https://msdn.microsoft.com/data/gg193958)  
[Ulepszenia produktywności Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Poprzednie](create-the-project.md)
> [dalej](ui_and_navigation.md)

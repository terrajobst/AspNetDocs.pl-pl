---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Wskazówki dotyczące laboratorium: jeden ASP.NET: Integrowanie formularzy sieci Web ASP.NET, MVC i Web API | Microsoft Docs'
author: rick-anderson
description: ASP.NET to struktura służąca do kompilowania witryn sieci Web, aplikacji i usług przy użyciu specjalistycznych technologii, takich jak MVC, Web API i inne. Z rozszerzeniem ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623201"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Ćwiczenia praktyczne: One ASP.NET: integrowanie wzorców ASP.NET Web Forms, MVC i Web API

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

> ASP.NET to struktura służąca do kompilowania witryn sieci Web, aplikacji i usług przy użyciu specjalistycznych technologii, takich jak MVC, Web API i inne. Wraz z ASP.NET ekspansji pojawiły się od momentu jego utworzenia, a wyrażone w tym przypadku muszą mieć zintegrowane technologie, podczas pracy w kierunku **jednego ASP.NET**.
> 
> W Visual Studio 2013 wprowadzono nowy ujednolicony system projektu, który umożliwia tworzenie aplikacji i używanie wszystkich technologii ASP.NET w jednym projekcie. Ta funkcja eliminuje konieczność wyboru jednej technologii na początku projektu i naklejenia z nią, a zamiast tego zaleca użycie wielu platform ASP.NET w ramach jednego projektu.
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Tworzenie witryny sieci Web opartej na **jednym** typie projektu ASP.NET
- Korzystanie z różnych platform **ASP.NET** , takich jak **MVC** i **Web API** w tym samym projekcie
- Zidentyfikuj główne składniki aplikacji **ASP.NET**
- Korzystanie z platformy tworzenia **szkieletów ASP.NET** do automatycznego tworzenia kontrolerów i widoków w celu wykonywania operacji CRUD na podstawie klas modelu
- Udostępnianie tego samego zestawu informacji w formatach maszynowych i czytelnych przez człowieka przy użyciu odpowiedniego narzędzia dla każdego zadania

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:

- [Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/) lub nowszego
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

Aby można było uruchomić ćwiczenia w tym ćwiczeniu, należy najpierw skonfigurować środowisko.

1. Otwórz Eksploratora Windows i przejdź do folderu **źródłowego** laboratorium.
2. Kliknij prawym przyciskiem myszy pozycję **Setup. cmd** i wybierz polecenie **Uruchom jako administrator** , aby uruchomić proces instalacji, który skonfiguruje środowisko i zainstaluje fragmenty kodu programu Visual Studio dla tego laboratorium.
3. Jeśli zostanie wyświetlone okno dialogowe Kontrola konta użytkownika, potwierdź akcję, aby wykonać operację.

> [!NOTE]
> Upewnij się, że wszystkie zależności dla tego laboratorium zostały sprawdzone przed uruchomieniem Instalatora.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Używanie fragmentów kodu

W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu. Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, do których można uzyskać dostęp z poziomu Visual Studio 2013, aby uniknąć konieczności ręcznego dodawania go.

> [!NOTE]
> Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze **BEGIN** w ćwiczeniu, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych. Należy pamiętać, że fragmenty kodu dodawane podczas wykonywania nie są dostępne w tych rozwiązaniach i mogą nie działać do czasu ukończenia ćwiczenia. Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder **końcowy** zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu. Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Tworzenie nowego projektu formularzy sieci Web](#Exercise1)
2. [Tworzenie kontrolera MVC przy użyciu szkieletu](#Exercise2)
3. [Tworzenie kontrolera interfejsu API sieci Web przy użyciu szkieletu](#Exercise3)

Szacowany czas wykonywania tego laboratorium: **60 minut**

> [!NOTE]
> Po pierwszym uruchomieniu programu Visual Studio należy wybrać jedną z wstępnie zdefiniowanych kolekcji ustawień. Każda wstępnie zdefiniowana kolekcja jest zaprojektowana tak, aby była zgodna z konkretnym stylem deweloperskim i określa układy okien, zachowanie edytora, fragmenty kodu IntelliSense i opcje okna dialogowego. Procedury przedstawione w tym laboratorium opisują akcje niezbędne do wykonania danego zadania w programie Visual Studio, gdy jest używana **Ogólna kolekcja ustawień deweloperskich** . W przypadku wybrania innej kolekcji ustawień dla środowiska programistycznego mogą wystąpić różnice w czynnościach, które należy wziąć pod uwagę.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Ćwiczenie 1: Tworzenie nowego projektu formularzy sieci Web

W tym ćwiczeniu zostanie utworzona nowa witryna formularzy sieci Web w Visual Studio 2013 przy użyciu **jednego ASP.NET** ujednoliconego środowiska projektowego, dzięki któremu można łatwo zintegrować składniki Web Forms, MVC i Web API w tej samej aplikacji. Następnie zostanie zbadane wygenerowane rozwiązanie i zidentyfikowanie jego części, a wreszcie zostanie wyświetlona witryna sieci Web w działaniu.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Zadanie 1 — Tworzenie nowej witryny przy użyciu jednego środowiska ASP.NET

W tym zadaniu rozpocznie się tworzenie nowej witryny sieci Web w programie Visual Studio w oparciu o typ projektu **ASP.NET** . **Jedna ASP.NET** łączy wszystkie technologie ASP.NET i pozwala na ich mieszanie i dopasowanie odpowiednio do potrzeb. Następnie zostaną rozpoznane różne składniki formularzy Web Forms, MVC i Web API, które znajdują się obok siebie w aplikacji.

1. Otwórz **Visual Studio Express 2013 dla sieci Web** i wybierz pozycję **plik | Nowy projekt...** , aby rozpocząć nowe rozwiązanie.

    ![Tworzenie nowego projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Tworzenie nowego projektu*
2. W oknie dialogowym **Nowy projekt** wybierz pozycję **aplikacja sieci Web ASP.NET** w obszarze **Wizualizacja C# |** Upewnij się, że jest zaznaczona **.NET Framework 4,5** . Nazwij projekt *MyHybridSite*, wybierz **lokalizację** i kliknij przycisk **OK**.

    ![Nowy projekt aplikacji sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Tworzenie nowego projektu aplikacji sieci Web ASP.NET*
3. W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **formularze sieci Web** i wybierz opcje **MVC** i **Web API** . Upewnij się również, że opcja **uwierzytelniania** jest ustawiona na **konta poszczególnych użytkowników**. Kliknij przycisk **OK** , aby kontynuować.

    ![Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składnika Web API i składników MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Tworzenie nowego projektu za pomocą szablonu formularzy sieci Web, w tym składnika Web API i składników MVC*
4. Teraz możesz zapoznać się ze strukturą wygenerowanego rozwiązania.

    ![Eksplorowanie wygenerowanego rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Eksplorowanie wygenerowanego rozwiązania*

    1. **Konto:** Ten folder zawiera strony formularza sieci Web, które umożliwiają rejestrację i logowanie do kont użytkowników aplikacji oraz zarządzanie nimi. Ten folder jest dodawany w przypadku wybrania opcji uwierzytelnianie **poszczególnych kont użytkowników** podczas konfiguracji szablonu projektu formularzy sieci Web.
    2. **Modele:** Ten folder będzie zawierać klasy, które reprezentują dane aplikacji.
    3. **Kontrolery** i **widoki**: te foldery są wymagane dla składników **interfejsu API sieci Web** **ASP.NET MVC** i ASP.NET. W następnych ćwiczeniach znajdziesz technologie MVC i interfejsu API sieci Web.
    4. **Default. aspx**, **Contact. aspx** i **Informacje o plikach. aspx** są wstępnie zdefiniowanymi stronami formularzy sieci Web, których można użyć jako punktów początkowych w celu utworzenia stron specyficznych dla aplikacji. Logika programowania tych plików znajduje się w osobnym pliku, zwanym &quot;&quot; pliku, który ma &quot;. aspx. vb&quot; lub &quot;. aspx.cs&quot; rozszerzenia (w zależności od używanego języka). Logika związany z kodem jest uruchamiana na serwerze i dynamicznie tworzy dane wyjściowe HTML dla strony.
    5. Strony **site. Master** i **site. Mobile. Master** definiują wygląd i działanie oraz standardowe zachowanie wszystkich stron w aplikacji.
5. Kliknij dwukrotnie plik **default. aspx** , aby poznać zawartość strony.

    ![Eksplorowanie domyślnej strony. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Eksplorowanie domyślnej strony. aspx*

    > [!NOTE]
    > Dyrektywa **Page** w górnej części pliku definiuje atrybuty strony formularzy sieci Web. Na przykład atrybut **MasterPageFile** określa ścieżkę do strony głównej — w tym przypadku strona *site. Master* i atrybut **Inherits** definiuje klasę powiązany kod dla strony do dziedziczenia. Ta klasa znajduje się w pliku określonym przez atrybut **CodeBehind** .
    > 
    > Formant **ASP: zawartość** przechowuje rzeczywistą zawartość strony (tekst, znacznik i kontrolki) i jest zamapowana na kontrolkę **ASP: ContentPlaceHolder** na stronie wzorcowej. W takim przypadku zawartość strony będzie renderowana wewnątrz kontrolki *kontrolka mainContent* zdefiniowanej na stronie *site. Master* .
6. Rozwiń węzeł **aplikacji\_Start** i zwróć uwagę na plik **WebApiConfig.cs** . Program Visual Studio uwzględniał ten plik w wygenerowanym rozwiązaniu, ponieważ dołączono internetowy interfejs API podczas konfigurowania projektu przy użyciu jednego szablonu ASP.NET.
7. Otwórz plik **WebApiConfig.cs** . W klasie *WebApiConfig* znajduje się konfiguracja skojarzona z interfejsem API sieci Web, która mapuje trasy protokołu HTTP na **Kontrolery interfejsu API sieci Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Otwórz plik **RouteConfig.cs** . Wewnątrz metody *RegisterRoutes* znajduje się konfiguracja skojarzona z MVC, która mapuje trasy protokołu HTTP na **Kontrolery MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — Uruchamianie rozwiązania

W tym zadaniu zostanie uruchomione wygenerowane rozwiązanie, zbadasz aplikację i niektóre jej funkcje, takie jak ponowne zapisywanie adresów URL i uwierzytelnianie wbudowane.

1. Aby uruchomić rozwiązanie, naciśnij klawisz **F5** lub kliknij przycisk **Start** znajdujący się na pasku narzędzi. Strona główna aplikacji powinna być otwarta w przeglądarce.

    ![Uruchamianie rozwiązania](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Sprawdź, czy strony formularzy sieci Web są wywoływane. W tym celu Dołącz **/Contact.aspx** do adresu URL na pasku adresu i naciśnij klawisz **Enter**.

    ![Przyjazne adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Przyjazne adresy URL*

    > [!NOTE]
    > Jak widać, adres URL zmieni się na **/Contact**. Począwszy od **ASP.NET 4**, możliwości routingu adresów URL zostały dodane do formularzy sieci Web, dzięki czemu można pisać adresy URL, takie jak *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* , a nie *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* . Aby uzyskać więcej informacji, zobacz [Routing adresów URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Teraz zostanie zbadany przepływ uwierzytelniania zintegrowany z aplikacją. W tym celu kliknij pozycję **zarejestruj** w prawym górnym rogu strony.

    ![Rejestrowanie nowego użytkownika](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Rejestrowanie nowego użytkownika*
4. Na stronie **Rejestr** wprowadź **nazwę użytkownika** i **hasło**, a następnie kliknij pozycję **zarejestruj**.

    ![Strona rejestracji](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Strona rejestracji*
5. Aplikacja rejestruje nowe konto, a użytkownik jest uwierzytelniany.

    ![Użytkownik uwierzytelniony](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Użytkownik uwierzytelniony*
6. Wróć do programu Visual Studio i naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Ćwiczenie 2: Tworzenie kontrolera MVC przy użyciu szkieletu

W tym ćwiczeniu wykorzystasz platformę tworzenia szkieletów ASP.NET zapewnianą przez program Visual Studio, aby utworzyć kontroler ASP.NET MVC 5 z akcjami i widokami Razor do wykonywania operacji CRUD, bez konieczności pisania pojedynczego wiersza kodu. Proces tworzenia szkieletu będzie używać Entity Framework Code First do wygenerowania kontekstu danych i schematu bazy danych w bazie danych SQL.

**Informacje o Entity Framework Code First**

Entity Framework (EF) to Mapowanie obiektowo-relacyjne (ORM), które umożliwia tworzenie aplikacji do uzyskiwania dostępu do danych przez Programowanie przy użyciu modelu aplikacji koncepcyjnej zamiast programowania bezpośrednio przy użyciu relacyjnego schematu magazynu.

Przepływ pracy modelowania Entity Framework Code First umożliwia korzystanie z własnych klas domeny do reprezentowania modelu, na którym opiera się program Dr podczas wykonywania zapytań, śledzenia zmian i aktualizowania funkcji. Korzystając z przepływu pracy tworzenia Code First, nie musisz rozpoczynać swojej aplikacji, tworząc bazę danych lub określając schemat. Zamiast tego można napisać standardowe klasy .NET, które definiują najbardziej odpowiednie obiekty modelu domeny dla aplikacji, a Entity Framework utworzy bazę danych.

> [!NOTE]
> Więcej informacji na temat Entity Framework można znaleźć [tutaj](../../../entity-framework.md).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Zadanie 1 — Tworzenie nowego modelu

Teraz zdefiniujemy klasy **Person** , która będzie modelem używanym przez proces tworzenia szkieletu, aby utworzyć kontroler MVC i widoki. Zacznij od utworzenia klasy modelu **osoby** i CRUD operacje w kontrolerze zostaną automatycznie utworzone przy użyciu funkcji tworzenia szkieletów.

1. Otwórz **Visual Studio Express 2013 dla sieci Web** i rozwiązania **MyHybridSite. sln** znajdującego się w folderze **Source/Ex2-MvcScaffolding/BEGIN** . Alternatywnie możesz kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.
2. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **modele** projektu **MyHybridSite** i wybierz polecenie **Dodaj | Klasa...** .

    ![Dodawanie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Dodawanie klasy modelu osoby*
3. W oknie dialogowym **Dodaj nowy element** nazwij plik *Person.cs* i kliknij przycisk **Dodaj**.

    ![Tworzenie klasy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Tworzenie klasy modelu osoby*
4. Zastąp zawartość pliku **Person.cs** następującym kodem. Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.

    (Fragment kodu- *BringingTogetherOneAspNet-Ex2-PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **MyHybridSite** i wybierz polecenie **Kompiluj**lub naciśnij **klawisze CTRL + SHIFT + B** , aby skompilować projekt.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Zadanie 2 — Tworzenie kontrolera MVC

Teraz, gdy jest tworzony model **osoby** , użyjesz szkieletu ASP.NET MVC z Entity Framework, aby utworzyć akcje i widoki kontrolera CRUD dla **osoby**.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **controllers (kontrolery** ) projektu **MyHybridSite** i wybierz polecenie **Dodaj | Nowy element szkieletowy..** .

    ![Tworzenie nowego kontrolera szkieletowego](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Tworzenie nowego kontrolera szkieletowego*
2. W oknie dialogowym **Dodawanie szkieletu** wybierz **kontroler MVC 5 z widokami, używając Entity Framework** a następnie kliknij przycisk **Dodaj.**

    ![Wybieranie kontrolera MVC 5 z widokami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Wybieranie kontrolera MVC 5 z widokami i Entity Framework*
3. Ustaw *MvcPersonController* jako **nazwę kontrolera**, wybierz opcję **Użyj akcji kontrolerów asynchronicznych** i wybierz **osobę (MyHybridSite. models)** jako **klasę modelu**.

    ![Dodawanie kontrolera MVC z szkieletem](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Dodawanie kontrolera MVC z szkieletem*
4. W obszarze **Klasa kontekstu danych**kliknij pozycję **nowy kontekst danych..** .

    ![Tworzenie nowego kontekstu danych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Tworzenie nowego kontekstu danych*
5. W oknie dialogowym **nowy kontekst danych** Nazwij nowy kontekst danych *PersonContext* i kliknij przycisk **Dodaj**.

    ![Tworzenie nowego PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Tworzenie nowego typu PersonContext*
6. Kliknij przycisk **Dodaj** , aby utworzyć nowy kontroler dla **osoby** z szkieletem. Program Visual Studio wygeneruje następnie akcje kontrolera, kontekst danych osoby i widoki Razor.

    ![Po utworzeniu kontrolera MVC przy użyciu szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Po utworzeniu kontrolera MVC przy użyciu szkieletu*
7. Otwórz plik **MvcPersonController.cs** w folderze **controllers** . Zauważ, że metody akcji CRUD zostały wygenerowane automatycznie.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Zaznaczając pole wyboru **Użyj akcji kontrolera asynchronicznego** z opcji tworzenia szkieletów w poprzednich krokach, program Visual Studio generuje asynchroniczne metody akcji dla wszystkich akcji, które obejmują dostęp do kontekstu danych osoby. Zalecane jest używanie asynchronicznych metod akcji dla długotrwałych żądań niezwiązanych z procesorem CPU, aby uniknąć zablokowania pracy serwera sieci Web podczas przetwarzania żądania.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — Uruchamianie rozwiązania

W tym zadaniu zostanie ponownie uruchomione rozwiązanie, aby sprawdzić, czy widoki dla **osób** działają zgodnie z oczekiwaniami. Dodasz nową osobę, aby sprawdzić, czy została pomyślnie zapisana w bazie danych.

1. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.
2. Przejdź do **/MvcPerson**. Widok szkieletu, który pokazuje listę osób, powinna zostać wyświetlona.
3. Kliknij przycisk **Utwórz nowy** , aby dodać nową osobę.

    ![Przechodzenie do widoków szkieletowych MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Przechodzenie do widoków szkieletowych MVC*
4. W widoku **Utwórz** Podaj **nazwę** i **wiek** osoby, a następnie kliknij pozycję **Utwórz**.

    ![Dodawanie nowej osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Dodawanie nowej osoby*
5. Nowa osoba zostanie dodana do listy. Na liście element kliknij pozycję **szczegóły** , aby wyświetlić widok szczegółów osoby. Następnie w widoku **szczegółów** kliknij przycisk **wstecz do listy** , aby wrócić do widoku listy.

    ![Widok szczegółów osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Widok szczegółów osoby*
6. Kliknij link **Usuń** , aby usunąć osobę. W widoku **Usuń** kliknij pozycję **Usuń** , aby potwierdzić operację.

    ![Usuwanie osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Usuwanie osoby*
7. Wróć do programu Visual Studio i naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Ćwiczenie 3: Tworzenie kontrolera interfejsu API sieci Web za pomocą szkieletu

Struktura internetowego interfejsu API jest częścią stosu ASP.NET i została zaprojektowana w celu ułatwienia wdrażania usług HTTP, zazwyczaj wysyłania i otrzymywania danych w formacie JSON lub XML za pośrednictwem interfejsu API RESTful.

W tym ćwiczeniu ponownie użyjesz szkieletu ASP.NET, aby wygenerować kontroler interfejsu API sieci Web. Użyjesz tej samej **osoby** i **PersonContext** z poprzedniego ćwiczenia, aby zapewnić te same dane osobom w formacie JSON. Zobaczysz, jak można uwidocznić te same zasoby na różne sposoby w tej samej aplikacji ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Zadanie 1 — Tworzenie kontrolera interfejsu API sieci Web

W tym zadaniu utworzysz nowy **kontroler interfejsu API sieci Web** , który będzie uwidaczniał dane osoby w formacie, który jest przeznaczony do użycia maszynowego, na przykład JSON.

1. Jeśli nie zostało to jeszcze zrobione, Otwórz **Visual Studio Express 2013 dla sieci Web** i Otwórz rozwiązanie **MyHybridSite. sln** znajdujące się w folderze **Source/Ex3-WebAPI/BEGIN** . Alternatywnie możesz kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.

    > [!NOTE]
    > Jeśli zaczniesz od rozwiązania BEGIN 3, naciśnij **kombinację klawiszy Ctrl + Shift + B** , aby skompilować rozwiązanie.
2. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder **controllers (kontrolery** ) projektu **MyHybridSite** i wybierz polecenie **Dodaj | Nowy element szkieletowy..** .

    ![Tworzenie nowego kontrolera szkieletowego](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Tworzenie nowego kontrolera szkieletowego*
3. W oknie dialogowym **Dodawanie szkieletu** wybierz pozycję **interfejs API sieci Web** w okienku po lewej stronie, a następnie kliknij pozycję **Kontroler sieci Web API 2 z akcjami, używając Entity Framework** w środkowym okienku, a następnie klikając przycisk **Dodaj.**

    ![Wybieranie kontrolera Web API 2 z akcjami i Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Wybieranie kontrolera Web API 2 z akcjami i Entity Framework")

    *Wybieranie kontrolera Web API 2 z akcjami i Entity Framework*
4. Ustaw *ApiPersonController* jako **nazwę kontrolera**, wybierz opcję **Użyj akcji kontrolerów asynchronicznych** i wybierz **osobę (MyHybridSite. models)** i **PersonContext (MyHybridSite. models)** jako odpowiednio **model** i klasy **kontekstu danych** . Następnie kliknij pozycję **Dodaj**.

    ![Dodawanie kontrolera interfejsu API sieci Web z szkieletem](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Dodawanie kontrolera interfejsu API sieci Web z szkieletem")

    *Dodawanie kontrolera interfejsu API sieci Web z szkieletem*
5. Program Visual Studio wygeneruje następnie klasę **ApiPersonController** z czterema akcjami CRUD do pracy z danymi.

    ![Po utworzeniu kontrolera interfejsu API sieci Web przy użyciu szkieletu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Po utworzeniu kontrolera interfejsu API sieci Web przy użyciu szkieletu")

    *Po utworzeniu kontrolera interfejsu API sieci Web przy użyciu szkieletu*
6. Otwórz plik **ApiPersonController.cs** i sprawdź *metodę akcji* GetActions. Ta metoda wysyła zapytanie do pola DB typu **PersonContext** w celu uzyskania danych dotyczących osób.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Teraz Zwróć uwagę na komentarz powyżej definicji metody. Zawiera identyfikator URI, który uwidacznia tę akcję, która zostanie użyta w następnym zadaniu.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Domyślnie internetowy interfejs API jest skonfigurowany do przechwytywania zapytań do ścieżki */API* , aby uniknąć kolizji z kontrolerami MVC. Jeśli chcesz zmienić to ustawienie, zapoznaj się z tematem [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — Uruchamianie rozwiązania

W tym zadaniu będziesz używać **narzędzi deweloperskich** programu Internet Explorer F12, aby sprawdzić pełną odpowiedź z kontrolera interfejsu API sieci Web. Zobaczysz, jak można przechwytywać ruch sieciowy, aby uzyskać więcej informacji o danych aplikacji.

> [!NOTE]
> Upewnij **się, że** na pasku narzędzi programu Visual Studio jest wybrany program **Internet Explorer** .
> 
> ![Opcja programu Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **Narzędzia programistyczne F12** mają szeroki zestaw funkcji, które nie są omówione w tym ćwiczeniu. Jeśli chcesz dowiedzieć się więcej na ten temat, zobacz [Korzystanie z narzędzi programistycznych F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).

1. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.

    > [!NOTE]
    > Aby prawidłowo wykonać to zadanie, aplikacja musi mieć dane. Jeśli baza danych jest pusta, możesz wrócić do zadania 3 w ćwiczeniu 2 i postępować zgodnie z instrukcjami dotyczącymi tworzenia nowej osoby przy użyciu widoków MVC.
2. W przeglądarce naciśnij klawisz **F12** , aby otworzyć panel **Narzędzia deweloperskie** . Naciśnij klawisz **CTRL** + **4** lub kliknij ikonę **sieci** , a następnie kliknij przycisk Zielona strzałka, aby rozpocząć przechwytywanie ruchu sieciowego.

    ![Inicjowanie przechwytywania sieci Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Inicjowanie przechwytywania sieci Web API")

    *Inicjowanie przechwytywania sieci Web API*
3. Dołącz **interfejs API/ApiPerson** do adresu URL na pasku adresu przeglądarki. Teraz sprawdzisz szczegóły odpowiedzi z **ApiPersonController**.

    ![Pobieranie danych osoby za poorednictwem internetowego interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Pobieranie danych osoby za poorednictwem internetowego interfejsu API")

    *Pobieranie danych osoby za poorednictwem internetowego interfejsu API*

    > [!NOTE]
    > Po zakończeniu pobierania zostanie wyświetlony monit o wykonanie akcji z pobranym plikiem. Pozostaw otwarte okno dialogowe, aby móc oglądać zawartość odpowiedzi za pomocą okna narzędzia deweloperów.
4. Teraz sprawdzimy treść odpowiedzi. Aby to zrobić, kliknij kartę **szczegóły** , a następnie kliknij pozycję **treść odpowiedzi**. Możesz sprawdzić, czy pobrane dane to lista obiektów o **identyfikatorze**właściwości, **nazwie** i **wieku** odpowiadającemu klasie **Person** .

    ![Wyświetlanie treści odpowiedzi interfejsu API sieci Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Wyświetlanie treści odpowiedzi interfejsu API sieci Web")

    *Wyświetlanie treści odpowiedzi interfejsu API sieci Web*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Zadanie 3 — Dodawanie stron pomocy interfejsu API sieci Web

Podczas tworzenia interfejsu API sieci Web warto utworzyć stronę pomocy, aby inni deweloperzy wiedzieli, jak wywołać interfejs API. Możesz tworzyć i aktualizować strony dokumentacji ręcznie, ale lepiej jest generować je automatycznie, aby uniknąć konieczności wykonywania zadań konserwacyjnych. W tym zadaniu zostanie użyty pakiet NuGet do automatycznego generowania stron pomocy interfejsu API sieci Web w rozwiązaniu.

1. W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.
2. W oknie **konsola Menedżera pakietów** wykonaj następujące polecenie:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > Pakiet **Microsoft. ASPNET. WebApi. HelpPage** instaluje niezbędne zestawy i dodaje widoki MVC dla stron pomocy w folderze **Areas/HelpPage** .

    ![Obszar HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Obszar HelpPage")

    *Obszar HelpPage*
3. Domyślnie strony pomocy mają ciągi zastępcze dla dokumentacji. Możesz użyć komentarzy dokumentacji XML, aby utworzyć dokumentację. Aby włączyć tę funkcję, Otwórz plik **HelpPageConfig.cs** znajdujący się w folderze **Areas/HelpPage/App\_Start** i Usuń komentarz z następującego wiersza:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **MyHybridSite**, wybierz pozycję **Właściwości** i kliknij kartę **kompilacja** .

    ![Karta kompilacja](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Sekcja kompilacji")

    *Karta kompilacja*
5. W obszarze **dane wyjściowe**wybierz pozycję **plik dokumentacji XML**. W polu edycji wpisz **aplikacja\_dane/XmlDocument. XML**.

    ![Sekcja danych wyjściowych na karcie kompilacja](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Sekcja danych wyjściowych na karcie kompilacja")

    *Sekcja danych wyjściowych na karcie kompilacja*
6. Naciśnij klawisz **CTRL** + **S** , aby zapisać zmiany.
7. Otwórz plik **ApiPersonController.cs** z folderu **controllers** .
8. Wprowadź nowy wiersz między sygnaturą metody *getludzie* i komentarzem *//Get API/ApiPerson* , a następnie wpisz trzy ukośniki.

    > [!NOTE]
    > Program Visual Studio automatycznie wstawia elementy XML, które definiują dokumentację metody.
9. Dodaj tekst podsumowania i wartość zwracaną dla metody *getludzie* . Powinien wyglądać podobnie do poniższego.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie.
11. Dołącz **/help** do adresu URL na pasku adresu, aby przejść do strony pomocy.

    ![Strona pomocy interfejsu API sieci Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "Strona pomocy interfejsu API sieci Web ASP.NET")

    *Strona pomocy interfejsu API sieci Web ASP.NET*

    > [!NOTE]
    > Główna Zawartość strony jest tabelą interfejsów API pogrupowanych według kontrolera. Wpisy tabeli są generowane dynamicznie przy użyciu interfejsu **IApiExplorer** . W przypadku dodania lub zaktualizowania kontrolera interfejsu API tabela zostanie automatycznie zaktualizowana przy następnym kompilowaniu aplikacji.
    > 
    > Kolumna **API** zawiera listę metod http i względnego identyfikatora URI. Kolumna **Description** zawiera informacje, które zostały wyodrębnione z dokumentacji metody.
12. Należy zauważyć, że opis dodany powyżej definicji metody jest wyświetlany w kolumnie Opis.

    ![Opis metody interfejsu API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Opis metody interfejsu API")

    *Opis metody interfejsu API*
13. Kliknij jedną z metod interfejsu API, aby przejść do strony zawierającej bardziej szczegółowe informacje, w tym przykładowe treści odpowiedzi.

    ![Strona informacji szczegółowych](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Strona informacji szczegółowych")

    *Strona informacji szczegółowych*

---

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Dzięki zakończeniu tego praktycznego laboratorium wiesz, jak:

- Utwórz nową aplikację sieci Web przy użyciu jednego środowiska ASP.NET w Visual Studio 2013
- Integruj wiele technologii ASP.NET w jeden projekt
- Generowanie kontrolerów MVC i widoków z klas modelu za pomocą tworzenia szkieletów ASP.NET
- Generuj Kontrolery interfejsu API sieci Web, które wykorzystują funkcje, takie jak programowanie asynchroniczne i dostęp do danych za pomocą Entity Framework
- Automatycznie Generuj strony pomocy interfejsu API sieci Web dla Twoich kontrolerów

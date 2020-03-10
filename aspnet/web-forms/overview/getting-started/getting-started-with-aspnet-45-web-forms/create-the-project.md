---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Utwórz projekt | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 62918b17f42e54dfe4e45a08927b1039dcbb7012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78571989"
---
# <a name="create-the-project"></a>Tworzenie projektu

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku utworzysz, przeglądasz i uruchomisz projekt domyślny w programie Visual Studio, który umożliwi zapoznanie się z funkcjami ASP.NET. Należy również zapoznać się ze środowiskiem programu Visual Studio.

## <a name="what-youll-learn"></a>Zawartość:

- Jak utworzyć nowy projekt formularzy sieci Web.
- Struktura pliku projektu formularzy sieci Web.
- Jak uruchomić projekt w programie Visual Studio.
- Różne funkcje domyślnej aplikacji formularzy sieci Web.
- Niektóre podstawowe informacje na temat używania środowiska programu Visual Studio.

## <a name="creating-the-project"></a>Tworzenie projektu

1. Otwórz program Visual Studio.
2. Wybierz pozycję **Nowy projekt** z menu **plik** w programie Visual Studio. 

    ![Utwórz projekt — element menu Nowy projekt](create-the-project/_static/image1.png)
3. Wybierz kolejno pozycje **Szablony** -&gt;  **C# Visual** -&gt; szablon **sieci Web** po lewej stronie.
4. Wybierz szablon **aplikacji sieci Web ASP.NET** w środkowej kolumnie.  
 Ta seria samouczków korzysta z .NET Framework 4.5.2.
5. Nazwij projekt *WingtipToys* i wybierz przycisk **OK** . 

    ![Tworzenie projektu — okno dialogowe Nowy projekt](create-the-project/_static/image2.png)

    > [!NOTE]
    > Nazwa projektu w tej serii samouczków to **WingtipToys**. Zaleca się użycie tej *dokładnej* nazwy projektu, aby kod podany w całej serii samouczków działa zgodnie z oczekiwaniami.

6. Kliknij przycisk **Zmień uwierzytelnianie**. Wybierz pozycję **indywidualne konta użytkowników** , a następnie kliknij przycisk **OK** .

7. Wybierz szablon **formularzy sieci Web** i kliknij przycisk **OK** .

    ![Tworzenie projektu — nowy szablon projektu](create-the-project/_static/image3.png)

Tworzenie projektu zajmie trochę czasu. Gdy wszystko będzie gotowe, Otwórz stronę **default. aspx** .

![Tworzenie projektu — nowy szablon projektu](create-the-project/_static/image4.png)

Możesz przełączać się między widokiem **projektu** a widokiem **źródła** , wybierając opcję w dolnej części okna środkowego. Widok **projektu** przedstawia ASP.NET stron sieci Web, stron wzorcowych, stron zawartości, stron HTML i kontrolek użytkownika przy użyciu widoku niemal-WYSIWYG. Widok **źródła** wyświetla znaczniki HTML dla strony sieci Web, którą można edytować.

> [!TIP] 
> 
> **Informacje o strukturach ASP.NET**
> 
> Formularze sieci Web ASP.NET umożliwiają tworzenie dynamicznych witryn internetowych przy użyciu znanych modeli typu "przeciągnij i upuść". Powierzchnia projektowa i setki kontrolek i składników pozwalają na szybkie tworzenie zaawansowanych, zaawansowanych witryn opartych na interfejsie użytkownika z dostępem do danych. Wingtip zabawka jest oparta na formularzach sieci Web ASP.NET, ale wiele koncepcji, które uczysz się w tej serii samouczków, ma zastosowanie do wszystkich ASP.NET.
> 
> ASP.NET oferuje cztery podstawowe platformy programistyczne:
> 
> - [ASP.NET Web Forms](../../../index.md)  
>  Struktura formularzy sieci Web jest przeznaczona dla deweloperów, którzy preferują programowanie deklaracyjne i oparte na kontroli, takie jak Microsoft Windows Forms (WinForms) i WPF/XAML/Silverlight. Oferuje model programistyczny oparty na projektancie w języku WYSIWYG, więc jest to popularne dla deweloperów szukających środowiska tworzenia aplikacji w sieci Web. Jeśli jesteś nowym programem do programowania w sieci Web i znasz tradycyjne narzędzia programistyczne klienta Microsoft RAD (na przykład dla Visual Basic i wizualizacji C#), możesz szybko utworzyć aplikację sieci Web bez obsługi języka HTML i języka JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC są przeznaczone dla deweloperów, którzy są zainteresowani wzorcami i zasadami, takimi jak Programowanie oparte na testach, oddzielenie problemów, niewersja kontroli (IoC) i iniekcja zależności (DI). Ta struktura zachęca do rozdzielania warstwy logiki biznesowej aplikacji sieci Web od jej warstwy prezentacji.
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  ASP.NET strony sieci Web są przeznaczone dla deweloperów, którzy chcą utworzyć prostą historię sieci Web, wraz z wierszami języka PHP. W modelu Web Pages utworzysz strony HTML, a następnie dodasz kod oparty na serwerze na stronie w celu dynamicznego sterowania sposobem renderowania tego znacznika. Strony sieci Web są przeznaczone specjalnie do uproszczonej struktury i są najłatwiejszym punktem wejścia do ASP.NET dla osób znających kod HTML, ale mogą nie mieć szerokiego doświadczenia programistycznego — na przykład uczniów lub hobby. Jest to dobre rozwiązanie dla deweloperów sieci Web, którzy znają język PHP lub podobne platformy, aby zacząć korzystać z ASP.NET.
> - [ASP.NET aplikacji jednostronicowej](../../../../single-page-application/index.md)  
>  Aplikacja jednostronicowa ASP.NET (SPA) ułatwia tworzenie aplikacji, które obejmują znaczące interakcje po stronie klienta przy użyciu języka HTML 5, CSS 3 i JavaScript. Aktualizacja ASP.NET and Web Tools 2012,2 dostarcza nowy szablon służący do tworzenia aplikacji jednostronicowych przy użyciu narzędzia separowania. js i interfejsu API sieci Web ASP.NET. Oprócz nowego szablonu SPA nowe szablony SPA utworzone przez społeczność są również dostępne do pobrania.
> 
> Oprócz czterech głównych platform programistycznych ASP.NET oferuje również dodatkowe technologie, które są ważne, aby mieć świadomość i znać, ale nie zostały omówione w tej serii samouczków:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) — środowisko do tworzenia usług http, które docierają do szerokiego zakresu klientów, w tym przeglądarek i urządzeń przenośnych.
> - [ASP.NET sygnalizujący](../../../../signalr/index.md) — biblioteka, która ułatwia tworzenie funkcji sieci Web w czasie rzeczywistym.

### <a name="reviewing-the-project"></a>Przeglądanie projektu

W programie Visual Studio okno **Eksplorator rozwiązań** umożliwia zarządzanie plikami dla projektu. Spójrzmy na foldery, które zostały dodane do aplikacji w **Eksplorator rozwiązań**. Szablon aplikacji sieci Web dodaje podstawową strukturę folderów:

![Tworzenie projektu — Eksplorator rozwiązań](create-the-project/_static/image5.png)

Program Visual Studio tworzy początkowe foldery i pliki dla projektu. Pierwsze pliki, do których będziesz pracować w dalszej części tego samouczka, są następujące:

| **Plik** | **Przeznaczenie** |
| --- | --- |
| *Default. aspx* | Zazwyczaj pierwsza strona wyświetlana, gdy aplikacja jest uruchamiana w przeglądarce. |
| *Site. Master* | Strona, która umożliwia tworzenie spójnego układu i używanie standardowego zachowania dla stron w aplikacji. |
| *Global. asax* | Opcjonalny plik, który zawiera kod odpowiadający zdarzeniom na poziomie aplikacji i na poziomie sesji wywoływanym przez ASP.NET lub moduły HTTP. |
| *Plik Web. config* | Dane konfiguracji aplikacji. |

### <a name="running-the-default-web-application"></a>Uruchamianie domyślnej aplikacji sieci Web

Domyślna aplikacja sieci Web zapewnia bogate środowisko w oparciu o wbudowaną funkcję i pomoc techniczną. Bez żadnych zmian w domyślnym projekcie formularzy sieci Web aplikacja jest gotowa do uruchomienia w lokalnej przeglądarce sieci Web.

1. Naciśnij klawisz ***F5*** w programie Visual Studio.   
 Aplikacja zostanie utworzona i wyświetlona w przeglądarce sieci Web.  

    ![Tworzenie projektu — strona domyślna](create-the-project/_static/image6.png)
2. Po zakończeniu przeglądania działającej aplikacji Zamknij okno przeglądarki.

W tej domyślnej aplikacji sieci Web istnieją trzy główne strony: *default. aspx* (Home), *about. aspx*i *Contact. aspx*. Każdą z tych stron można uzyskać od górnego paska nawigacyjnego. W folderze kont znajdują się również dwie dodatkowe strony, na stronie register. aspx i login. aspx. Te dwie strony umożliwiają korzystanie z funkcji członkostwa ASP.NET w celu tworzenia, przechowywania i weryfikowania poświadczeń użytkownika.

## <a name="aspnet-web-forms-background"></a>Tło formularzy sieci Web ASP.NET

ASP.NET Web Forms są stronami opartymi na technologii Microsoft ASP.NET, w których kod uruchomiony na serwerze dynamicznie generuje dane wyjściowe stron sieci Web do przeglądarki lub urządzenia klienckiego. Na stronie formularzy sieci Web ASP.NET jest automatycznie renderowany prawidłowy kod HTML zgodny z przeglądarką dla funkcji, takich jak style, układ i tak dalej. Formularze sieci Web są zgodne z dowolnym językiem obsługiwanym przez środowisko uruchomieniowe języka wspólnego platformy .NET, takim jak C#Microsoft Visual Basic i Microsoft Visual. Ponadto formularze sieci Web są oparte na [platformie Microsoft .NET](https://msdn.microsoft.com/vstudio/aa496123), która zapewnia takie korzyści, jak środowisko zarządzane, bezpieczeństwo typów i dziedziczenie.

Po uruchomieniu strony formularzy sieci Web ASP.NET Strona przechodzi przez cykl życia, w którym wykonuje serię etapów przetwarzania. Te kroki obejmują inicjalizację, tworzenie wystąpienia kontrolek, przywracanie i konserwowanie stanu, uruchamianie kodu programu obsługi zdarzeń oraz renderowanie. Tak samo jak w przypadku ASP.NETych formularzy sieci Web, ważne jest, aby zrozumieć [cykl życia strony ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) , dzięki czemu można napisać kod na odpowiednim etapie cyklu życia dla danego efektu.

Gdy serwer sieci Web odbiera żądanie dla strony, odnajdzie stronę, przetwarza ją, wysyła do przeglądarki, a następnie odrzuca wszystkie informacje o stronie. Jeśli użytkownik zażąda tej samej strony ponownie, serwer powtarza całą sekwencję, przetwarzanie strony od podstaw. W innym przypadku serwer nie ma pamięci stron, które zostały przetworzone — strony nie są bezstanowe. Struktura strony ASP.NET automatycznie obsługuje zadanie utrzymania stanu strony i jego kontrolek, a także zapewnia jawne sposoby utrzymania stanu informacji specyficznych dla aplikacji.

> [!TIP] 
> 
> **Funkcje aplikacji sieci Web w szablonie aplikacji formularzy sieci Web**
> 
> Szablon aplikacji ASP.NET Web Forms oferuje bogaty zestaw wbudowanych funkcji. Zawiera ona nie tylko stronę *Home. aspx* , stronę *about. aspx* , stronę *Contact. aspx* , ale również zawiera funkcje członkostwa, które rejestrują użytkowników i zapisują swoje poświadczenia, dzięki czemu mogą logować się do witryny sieci Web. To omówienie zawiera więcej informacji na temat niektórych funkcji zawartych w szablonie aplikacji ASP.NET Web Forms i sposobu ich użycia w aplikacji Wingtip zabawki.
> 
> **Członkostwo**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) Tożsamość przechowuje poświadczenia użytkowników w bazie danych utworzonej przez aplikację. Gdy użytkownicy logują się, aplikacja sprawdza poprawność swoich poświadczeń, odczytując bazę danych. Folder *konta* projektu zawiera pliki implementujące różne części członkostwa: rejestrowanie, logowanie, zmiana hasła i Autoryzowanie dostępu. Ponadto ASP.NET Web Forms obsługuje uwierzytelnianie OAuth i OpenID Connect. Te ulepszenia uwierzytelniania umożliwiają użytkownikom logowanie się do witryny przy użyciu istniejących poświadczeń, takich jak Facebook, Twitter, Windows Live i Google.
> 
> ![Tworzenie projektu — Eksplorator rozwiązań (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> Domyślnie szablon tworzy bazę danych członkostwa przy użyciu domyślnej nazwy bazy danych w wystąpieniu SQL Server Express LocalDB, serwer bazy danych programistycznej, który jest dostarczany z Visual Studio Express 2013 dla sieci Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) to uproszczona wersja SQL Server, która ma wiele funkcji programowalności bazy danych SQL Server. SQL Server Express działa w trybie użytkownika i ma szybką instalację o zerowej konfiguracji, która ma krótką listę wymagań wstępnych instalacji. W Microsoft SQL Server każda baza danych lub kod języka Transact-SQL można przenieść z SQL Server Express LocalDB do SQL Server i SQL Azure bez żadnych kroków uaktualniania. Dlatego SQL Server Express LocalDB może służyć jako środowisko deweloperskie dla aplikacji przeznaczonych dla wszystkich wersji SQL Server. SQL Server Express LocalDB umożliwia korzystanie z funkcji takich jak procedury składowane, funkcje zdefiniowane przez użytkownika i agregacje, integracja .NET Frameworka, typy przestrzenne i inne, które nie są dostępne w SQL Server Compact.
> 
> **Strony wzorcowe**
> 
> [Strona wzorcowa ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) definiuje spójny wygląd i zachowanie dla wszystkich stron w aplikacji. Układ strony wzorcowej jest scalany z zawartością z pojedynczej strony zawartości, aby utworzyć końcową stronę, którą widzi użytkownik. W aplikacji Wingtip zabawki należy zmodyfikować stronę wzorcową *lokacja. Master* , tak aby wszystkie strony w witrynie sieci Web Wingtip zabawki miały ten sam logo charakterystyczne i pasek nawigacyjny.
> 
> **HTML5**
> 
> Szablon aplikacji ASP.NET Web Forms obsługuje [HTML5](http://www.w3schools.com/html/html5_intro.asp), czyli najnowszą wersję języka HTML Markup Language. Program HTML5 obsługuje nowe elementy i funkcje, które ułatwiają tworzenie witryn sieci Web.
> 
> **Modernizacja**
> 
> W przypadku przeglądarek, które nie obsługują języka HTML5, można użyć programu [unowocześnienie](http://www.modernizr.com/). Modernizacja to biblioteka języka JavaScript typu open source, która umożliwia wykrywanie, czy przeglądarka obsługuje funkcje HTML5, i włączać je, jeśli nie. W szablonie aplikacji ASP.NET Web Forms program modernizacja jest instalowany jako pakiet NuGet.
> 
> **Bootstrap**
> 
> Szablony projektów Visual Studio 2013 używają [Bootstrap](http://getbootstrap.com/), układu i struktury z motywem utworzonym przez serwis Twitter. Funkcja ładowania początkowego korzysta z CSS3 w celu zapewnienia, że program umożliwia dynamiczne dostosowanie do różnych rozmiarów okna przeglądarki. Możesz również użyć funkcji obsługi motywów Bootstrap, aby łatwo zmienić wygląd i działanie aplikacji. Domyślnie szablon aplikacji sieci Web ASP.NET w Visual Studio 2013 zawiera polecenie Bootstrap jako pakiet NuGet.
> 
> **Pakiety NuGet**
> 
> Szablon aplikacji ASP.NET Web Forms zawiera zestaw pakietów [NuGet](http://www.nuget.org/) . Te pakiety zapewniają składniki funkcji w postaci bibliotek i narzędzi typu open source. Istnieje szeroka gama pakietów, które ułatwiają tworzenie i testowanie aplikacji. Program Visual Studio ułatwia dodawanie, usuwanie i aktualizowanie pakietów NuGet. Deweloperzy mogą również tworzyć i dodawać pakiety do narzędzia NuGet.
> 
> ![Tworzenie projektu — okno dialogowe programu NuGet](create-the-project/_static/image8.png)
> 
> Podczas instalacji pakietu NuGet kopiuje pliki do rozwiązania i automatycznie wprowadza wszelkie zmiany, takie jak Dodawanie odwołań i zmiana konfiguracji skojarzonej z aplikacją sieci Web. Jeśli zdecydujesz się usunąć bibliotekę, pakiet NuGet usunie pliki i wycofa wszelkie zmiany wprowadzone w projekcie, aby nie było żadnego bałaganu. Pakiet NuGet jest dostępny w menu **Narzędzia** w programie Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) to szybka i zwięzła biblioteka języka JavaScript, która upraszcza przechodzenie do dokumentów HTML, obsługę zdarzeń, animowanie i interakcje AJAX na potrzeby szybkiego tworzenia aplikacji internetowych. Biblioteka jQuery JavaScript jest dołączona do szablonu aplikacji ASP.NET Web Forms jako pakiet NuGet.
> 
> **Niezauważalna weryfikacja**
> 
> Wbudowane kontrolki walidatora zostały skonfigurowane do korzystania z niewygodnego języka JavaScript dla logiki walidacji po stronie klienta. Znacznie zmniejsza to ilość kodu JavaScript renderowanego wewnętrznie w znaczniku strony i zmniejsza całkowity rozmiar strony. Niedyskretna weryfikacja jest dodawana globalnie do szablonu aplikacji ASP.NET Web Forms na podstawie ustawienia w &lt;appSettings&gt; element pliku *Web. config* w katalogu głównym aplikacji.
> 
> **Entity Framework Code First**
> 
> Oprócz funkcji w szablonie aplikacji ASP.NET Web Forms, aplikacja Wingtip zabawki używa [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), która jest biblioteką NuGet, która umożliwia programowanie zorientowane na kod podczas pracy z danymi. Po prostu tworzy część bazy danych aplikacji na podstawie kodu, który napiszesz. Korzystając z Entity Framework, pobierasz dane i manipulujesz nimi jako obiekty silnie wpisane. Dzięki temu można skoncentrować się na logice biznesowej w aplikacji, a nie na szczegółach sposobu uzyskiwania dostępu do danych.
> 
> Aby uzyskać dodatkowe informacje na temat zainstalowanych bibliotek i pakietów zawartych w szablonie formularzy sieci Web ASP.NET, zobacz listę zainstalowanych pakietów NuGet. W tym celu w programie Visual Studio Utwórz nowy projekt formularzy sieci Web, wybierz pozycję **narzędzia** > **menedżer pakietów NuGet** > **Zarządzaj pakietami NuGet dla rozwiązania**, a następnie wybierz pozycję **zainstalowane pakiety** w oknie dialogowym **Zarządzanie pakietami NuGet** .

### <a name="touring-visual-studio"></a>Przewodnik po programie Visual Studio

Podstawowe okna programu Visual Studio obejmują **Eksplorator rozwiązań**, **Eksplorator serwera** (**Eksplorator bazy danych** w programie Express), **okno właściwości**, **Przybornik**, **pasek narzędzi**i **okno dokumentu**.

![Tworzenie projektu — okno dialogowe programu NuGet](create-the-project/_static/image9.png)

Aby uzyskać więcej informacji na temat programu Visual Studio, zobacz [przewodnik wizualny do Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Podsumowanie

W tym samouczku została utworzona, sprawdzona i uruchomiona domyślna aplikacja formularzy sieci Web. Sprawdzono różne funkcje domyślnej aplikacji formularzy sieci Web i przedstawiono podstawowe informacje dotyczące korzystania ze środowiska programu Visual Studio. W poniższych samouczkach utworzysz warstwę dostępu do danych.

## <a name="additional-resources"></a>Dodatkowe materiały

[Wybieranie odpowiedniego modelu programowania](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Projekty aplikacji sieci Web i projekty witryn sieci web](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET stron formularzy sieci Web — Omówienie](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Poprzednie](introduction-and-overview.md)
> [dalej](create_the_data_access_layer.md)

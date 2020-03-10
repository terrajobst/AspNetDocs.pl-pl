---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Wstrzykiwanie zależności ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Uwaga: w tym ćwiczeniu praktycznym założono, że masz podstawową wiedzę na temat filtrów ASP.NET MVC i ASP.NET MVC 4. Jeśli nie korzystasz wcześniej z filtrów ASP.NET MVC 4, Rec...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78560614"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>ASP.NET MVC 4 — wstrzykiwanie zależności

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

W tym laboratorium praktycznym założono, że masz podstawową wiedzę na temat **ASP.NET MVC** i **ASP.NET MVC 4**. Jeśli wcześniej nie korzystasz z **filtrów ASP.NET MVC 4** , zalecamy przechodzenie do programu **ASP.NETej akcji niestandardowej MVC** .

> [!NOTE]
> Wszystkie przykładowe kod i fragmenty kodu są dołączone do zestawu szkoleniowego dla sieci Web Camp, dostępnego w [wersji Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzny dla tego laboratorium jest dostępny pod kątem [ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

W modelu **programowania zorientowanego obiektowo** obiekty współpracują z modelem współpracy, w którym znajdują się Współautorzy i konsumenci. Naturalnie ten model komunikacji generuje zależności między obiektami i składnikami, co utrudnia zarządzanie w przypadku wzrostu złożoności.

![Zależności klas i złożoność modelu](aspnet-mvc-4-dependency-injection/_static/image1.png "Zależności klas i złożoność modelu")

*Zależności klas i złożoność modelu*

Prawdopodobnie wiesz o **wzorcu fabryki** i rozdzieleniu między interfejsem a implementacją przy użyciu usług, gdzie obiekty klienckie są często odpowiedzialne za lokalizację usługi.

Wzorzec iniekcji zależności jest konkretną implementacją nieużywanej kontroli. **Inversion of Control (IOC)** oznacza, że obiekty nie tworzą innych obiektów, na których polegają na wykonaniu ich pracy. Zamiast tego uzyskują wymagane obiekty ze źródła zewnętrznego (na przykład plik konfiguracyjny XML).

**Iniekcja zależności (di)** oznacza, że jest to wykonywane bez interwencji obiektu, zazwyczaj przez składnik platformy, który przekazuje parametry konstruktora i ustawia właściwości.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Wzorzec projektowania iniekcji zależności (DI)

Na wysokim poziomie, celem iniekcji zależności jest to, że Klasa klienta (np. *Golf*) potrzebuje elementu, który spełnia interfejs (np. *IClub*). Nie ma opieki, jaki jest konkretny typ (np. *WoodClub, IronClub, WedgeClub* lub *PutterClub*), chce, aby ktoś inny obsłużył to (np. dobry *Caddy*). Program rozpoznawania zależności w ASP.NET MVC umożliwia zarejestrowanie logiki zależności w innym miejscu (np. w kontenerze lub w *torbie Trefl*).

![Diagram iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image2.png "Ilustracja iniekcji zależności")

*Wstrzykiwanie zależności — analogowe Źródło golfa*

Zalety używania wzorca iniekcji zależności i Inwersja kontroli są następujące:

- Redukuje sprzęganie klas
- Zwiększa użycie kodu
- Zwiększa łatwość utrzymania kodu
- Usprawnia testowanie aplikacji

> [!NOTE]
> Wstrzykiwanie zależności jest czasami porównywane z wzorcem wzorca konstrukcji abstrakcyjnej, ale istnieje niewielka różnica między obydwoma podejściami. Program DI ma strukturę działającą w celu rozwiązywania zależności przez wywoływanie fabryk i zarejestrowanych usług.

Teraz, po zrozumieniu wzorca iniekcji zależności, dowiesz się, jak go zastosować w ASP.NET MVC 4. Aby włączyć usługę dostępu do bazy danych, należy rozpocząć korzystanie z iniekcji zależności na **kontrolerach** . Następnie należy zastosować iniekcję zależności do **widoków** w celu użycia usługi i wyświetlenia informacji. Na koniec należy rozszerzyć filtry DI to ASP.NET MVC 4, wstawiając niestandardowy filtr akcji do rozwiązania.

W tym ćwiczeniu dowiesz się, jak:

- Integrowanie ASP.NET MVC 4 z Unity dla iniekcji zależności przy użyciu pakietów NuGet
- Używanie iniekcji zależności wewnątrz kontrolera ASP.NET MVC
- Używanie iniekcji zależności wewnątrz widoku ASP.NET MVC
- Użyj iniekcji zależności wewnątrz filtru akcji ASP.NET MVC

> [!NOTE]
> W tym laboratorium jest używany pakiet NuGet Unity. Mvc3 do rozpoznawania zależności, ale możliwe jest dostosowanie wszelkich platform iniekcji zależności do współdziałania z ASP.NET MVC 4.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku B: Using fragmenty kodu](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Ćwiczenie 1: wstrzyknięcie kontrolera](#Exercise1)
2. [Ćwiczenie 2: wprowadzanie widoku](#Exercise2)
3. [Ćwiczenie 3: wprowadzanie filtrów](#Exercise3)

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

Szacowany czas wykonywania tego laboratorium: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Ćwiczenie 1: wstrzyknięcie kontrolera

W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w kontrolerach ASP.NET MVC przez integrację aparatu Unity z użyciem pakietu NuGet. Z tego powodu usługi są uwzględniane w kontrolerach MvcMusicStore w celu oddzielenia logiki od dostępu do danych. Usługi spowodują utworzenie nowej zależności w konstruktorze kontrolera, który zostanie rozwiązany przy użyciu iniekcji zależności przy pomocy **aparatu Unity**.

Takie podejście pokazuje, jak generować mniej połączone aplikacje, które są bardziej elastyczne i łatwiejsze do utrzymania i testowania. Dowiesz się również, jak zintegrować ASP.NET MVC z programem Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Usługa Storemanager — informacje

Magazyn MVC Music udostępniony w rozwiązaniu BEGIN obejmuje teraz usługę, która zarządza danymi kontrolera magazynu o nazwie **StoreService**. Poniżej znajdziesz implementację usługi magazynu. Zwróć uwagę, że wszystkie metody zwracają jednostki modelu.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** z rozwiązania BEGIN korzysta teraz z **StoreService**. Wszystkie odwołania do danych zostały usunięte z **StoreController**i teraz możliwe jest zmodyfikowanie bieżącego dostawcy dostępu do danych bez zmiany żadnej metody korzystającej z **StoreService**.

Poniżej znajdują się informacje o tym, że implementacja **StoreController** ma zależność z **StoreService** wewnątrz konstruktora klasy.

> [!NOTE]
> Zależność wprowadzona w tym ćwiczeniu jest związana z **niewersją kontroli** (IOC).
> 
> Konstruktor klasy **StoreController** odbiera parametr typu **IStoreService** , który jest niezbędny do wykonywania wywołań usługi z wewnątrz klasy. Jednak **StoreController** nie implementuje domyślnego konstruktora (bez parametrów), które każdy kontroler musi mieć do pracy z ASP.NET MVC.
> 
> Aby rozwiązać zależność, kontroler musi być utworzony przez fabrykę abstrakcyjną (Klasa, która zwraca dowolny obiekt określonego typu).

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Jeśli Klasa podejmie próbę utworzenia StoreController bez wysłania obiektu usługi, wystąpi błąd, ponieważ nie ma zadeklarowanego konstruktora bezparametrowego.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Zadanie 1 — Uruchamianie aplikacji

W tym zadaniu uruchomisz aplikację BEGIN, która obejmuje usługę w kontrolerze magazynu, która oddziela dostęp do danych z logiki aplikacji.

Podczas uruchamiania aplikacji zostanie wyświetlony wyjątek, ponieważ usługa kontrolera nie zostanie domyślnie przeniesiona jako parametr:

1. Otwórz rozwiązanie **BEGIN** znajdujące się w **Source\Ex01-Injecting Controller\Begin**.

   1. Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Naciśnij **klawisze CTRL + F5** , aby uruchomić aplikację bez debugowania. Zostanie wyświetlony komunikat o błędzie &quot;**dla tego obiektu nie zdefiniowano konstruktora bez parametrów**&quot;:

    ![Wystąpił błąd podczas uruchamiania aplikacji ASP.NET MVC BEGIN](aspnet-mvc-4-dependency-injection/_static/image3.png "Wystąpił błąd podczas uruchamiania aplikacji ASP.NET MVC BEGIN")

    *Wystąpił błąd podczas uruchamiania aplikacji ASP.NET MVC BEGIN*
3. Zamknij okno przeglądarki.

W poniższych krokach zostanie wykonane rozwiązanie dotyczące magazynu utworów muzycznych, aby wstrzyknąć zależność wymaganą przez ten kontroler.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Zadanie 2 — w tym środowisko Unity w rozwiązaniu MvcMusicStore

W tym zadaniu zostanie uwzględniony pakiet NuGet **Unity. Mvc3** do rozwiązania.

> [!NOTE]
> Pakiet Unity. Mvc3 został zaprojektowany dla ASP.NET MVC 3, ale jest w pełni zgodny z ASP.NET MVC 4.
> 
> Unity jest lekkim, rozszerzalnym kontenerem iniekcji zależności z opcjonalną obsługą przechwycenia wystąpienia i typu. Jest to kontener ogólnego przeznaczenia do użycia w dowolnym typie aplikacji .NET. Zapewnia ona wszystkie typowe funkcje dostępne w mechanizmie iniekcji zależności, w tym: Tworzenie obiektów, abstrakcję wymagań przez określenie zależności w czasie wykonywania i elastyczność, przez odroczenie konfiguracji składnika do kontenera.

1. Zainstaluj pakiet NuGet **Unity. Mvc3** w projekcie **MvcMusicStore** . W tym celu Otwórz **konsolę Menedżera pakietów** z **widoku** | **inne okna**.
2. Uruchom następujące polecenie.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalowanie pakietu NuGet aparatu Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalowanie pakietu NuGet aparatu Unity. Mvc3")

    *Instalowanie pakietu NuGet aparatu Unity. Mvc3*
3. Po zainstalowaniu pakietu **Unity. Mvc3** Eksploruj pliki i foldery, które są automatycznie dodawane, aby uprościć konfigurację środowiska Unity.

    ![Zainstalowano pakiet Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Zainstalowano pakiet Unity. Mvc3")

    *Zainstalowano pakiet Unity. Mvc3*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a>Zadanie 3 — rejestrowanie aparatu Unity w aplikacji Global.asax.cs\_Start

W tym zadaniu zaktualizujesz **aplikację\_startową** znajdującą się w **Global.asax.cs** w celu wywołania inicjatora programu inicjującego środowisko uruchomieniowe Unity, a następnie zaktualizujesz plik programu inicjującego, który zostanie użyty do iniekcji zależności.

1. Teraz nastąpi podłączenie do programu inicjującego, który jest plikiem inicjującym kontener aparatu Unity i program rozpoznawania zależności. Aby to zrobić, Otwórz **Global.asax.cs** i Dodaj następujący wyróżniony kod w **aplikacji\_Start** .

    (Fragment kodu — *ASP.net-test wtrysku zależności-Ex01-Initialize Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Otwórz plik **Bootstrapper.cs** .
3. Uwzględnij następujące przestrzenie nazw: **MvcMusicStore. Services** i **MusicStore. controllers**.

    (Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex01-program inicjujący Dodawanie przestrzeni nazw*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Zastąp zawartość metody **BuildUnityContainer** następującym kodem, który rejestruje kontroler magazynu i usługę magazynu.

    (Fragment kodu — *ASP.neta wtrysku zależności Ex01-Register — kontroler i usługa magazynu*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — Uruchamianie aplikacji

W tym zadaniu uruchomisz aplikację w celu sprawdzenia, czy można ją teraz załadować po dołączeniu aparatu Unity.

1. Naciśnij klawisz **F5** , aby uruchomić aplikację, aplikacja powinna teraz ładować bez wyświetlania komunikatu o błędzie.

    ![Uruchamianie aplikacji z iniekcją zależności](aspnet-mvc-4-dependency-injection/_static/image6.png "Uruchamianie aplikacji z iniekcją zależności")

    *Uruchamianie aplikacji z iniekcją zależności*
2. Przejdź do **/Store**. Spowoduje to wywołanie **StoreController**, który jest teraz tworzony przy użyciu **aparatu Unity**.

    ![Sklep MVC Music](aspnet-mvc-4-dependency-injection/_static/image7.png "Sklep MVC Music")

    *Sklep MVC Music*
3. Zamknij okno przeglądarki.

W poniższych ćwiczeniach dowiesz się, jak zwiększyć zakres iniekcji zależności, aby używać go w widokach ASP.NET MVC i filtrach akcji.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Ćwiczenie 2: wprowadzanie widoku

W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w widoku z nowymi funkcjami integracji ASP.NET MVC 4 for Unity. W tym celu w widoku przeglądania sklepu zostanie wywołana Usługa niestandardowa, w której zostanie wyświetlony komunikat i obraz poniżej.

Następnie można zintegrować projekt z aparatem Unity i utworzyć niestandardowy program rozpoznawania zależności, aby wstrzyknąć zależności.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Zadanie 1 — Tworzenie widoku korzystającego z usługi

W tym zadaniu utworzysz widok, który wykonuje wywołanie usługi, aby wygenerować nową zależność. Usługa programu składa się z prostej usługi obsługi komunikatów zawartej w tym rozwiązaniu.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **Source\Ex02-Injecting View\Begin** . W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
      > 
      > Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Uwzględnij klasy **MessageService.cs** i **IMessageService.cs** znajdujące się w folderze **Source \Assets** w **/Services**. Aby to zrobić, kliknij prawym przyciskiem myszy folder **usługi** i wybierz pozycję **Dodaj istniejący element**. Przejdź do lokalizacji plików i Uwzględnij je.

    ![Dodawanie usługi komunikatów i interfejsu usługi](aspnet-mvc-4-dependency-injection/_static/image8.png "Dodawanie usługi komunikatów i interfejsu usługi")

    *Dodawanie usługi komunikatów i interfejsu usługi*

    > [!NOTE]
    > Interfejs **IMessageService** definiuje dwie właściwości zaimplementowane przez klasę **MessageService** . Te właściwości —**Message** i **ImageUrl**— PRZECHOWUJĄ komunikat i adres URL obrazu, który będzie wyświetlany.
3. Utwórz folder **/Pages** w folderze głównym projektu, a następnie Dodaj istniejącą klasę **MyBasePage.cs** z **Source\Assets**. Strona podstawowa, z której będzie dziedziczyć, ma następującą strukturę.

    ![Folder stron](aspnet-mvc-4-dependency-injection/_static/image9.png "Folder stron")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Otwórz widok **Przeglądaj. cshtml** z folderu **/views/Store** i uczyń go dziedziczonym z **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. W widoku **przeglądania** Dodaj wywołanie do **MessageService** w celu wyświetlenia obrazu i wiadomości pobranej przez usługę.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Zadanie 2 — łącznie z niestandardowym programem rozpoznawania zależności i niestandardowym aktywatorem strony widoku

W poprzednim zadaniu została wprowadzona nowa zależność wewnątrz widoku w celu wykonania w nim wywołania usługi. Teraz można rozwiązać ten zależność przez implementację interfejsów wstrzykiwania zależności ASP.NET MVC **IViewPageActivator** i **IDependencyResolver**. W rozwiązaniu zostaną uwzględnione implementacje **IDependencyResolver** , które będą obsługiwać pobieranie usługi przy użyciu aparatu Unity. Następnie dołączysz kolejną niestandardową implementację interfejsu **IViewPageActivator** , która będzie rozwiązywać Tworzenie widoków.

> [!NOTE]
> Ponieważ ASP.NET MVC 3, implementacja dla iniekcji zależności miała Uproszczone interfejsy do rejestracji usług. **IDependencyResolver** i **IViewPageActivator** są częścią funkcji ASP.NET MVC 3 dla iniekcji zależności.
> 
> **-IDependencyResolver** interfejs zastępuje poprzednią IMvcServiceLocator. Implementacje elementu IDependencyResolver muszą zwracać wystąpienie usługi lub kolekcji usług.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interfejs zapewnia bardziej precyzyjną kontrolę nad sposobem tworzenia wystąpień stron widoku przy użyciu iniekcji zależności. Klasy, które implementują interfejs **IViewPageActivator** , mogą tworzyć wystąpienia widoku przy użyciu informacji kontekstowych.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. Utwórz folder**fabryks** w folderze głównym projektu.
2. Uwzględnij **CustomViewPageActivator.cs** do rozwiązania z folderu **/sources/Assets/** do **Factors** . Aby to zrobić, kliknij prawym przyciskiem myszy folder **/Factories** , a następnie wybierz pozycję **Dodaj | Istniejący element** , a następnie wybierz pozycję **CustomViewPageActivator.cs**. Ta klasa implementuje interfejs **IViewPageActivator** do przechowywania kontenera Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** jest odpowiedzialny za zarządzanie tworzeniem widoku przy użyciu kontenera Unity.
3. Dołącz plik **UnityDependencyResolver.cs** z folderu **/sources/Assets** do **/Factories** . Aby to zrobić, kliknij prawym przyciskiem myszy folder **/Factories** , a następnie wybierz pozycję **Dodaj | Istniejący element** , a następnie wybierz plik **UnityDependencyResolver.cs** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > Klasa **UnityDependencyResolver** jest niestandardowym DependencyResolver dla aparatu Unity. Gdy nie można znaleźć usługi wewnątrz kontenera Unity, podstawowy program rozpoznawania nazw to invocated.

W następujących zadaniach obie implementacje zostaną zarejestrowane w celu umożliwienia modelowi znajomości lokalizacji usług i widoków.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Zadanie 3 — rejestrowanie dla iniekcji zależności w kontenerze Unity

W tym zadaniu zostaną umieszczone wszystkie poprzednie elementy, aby umożliwić iniekcję zależności.

Do teraz Twoje rozwiązanie ma następujące elementy:

- Widok **przeglądania** , który dziedziczy z **MyBaseClass** i korzysta z **MessageService**.
- Pośrednia Klasa-**MyBaseClass**-, która ma iniekcję zależności zadeklarowaną dla interfejsu usługi.
- Usługa- **MessageService** -i jej interfejs **IMessageService**.
- Niestandardowy program rozpoznawania zależności dla aparatu Unity- **UnityDependencyResolver** — który zajmuje się pobieraniem usługi.
- Aktywator strony widoku — **CustomViewPageActivator** — tworzy stronę.

Aby wstrzyknąć widok **przeglądania** , można teraz zarejestrować niestandardowy program rozpoznawania zależności w kontenerze aparatu Unity.

1. Otwórz plik **Bootstrapper.cs** .
2. Zarejestruj wystąpienie elementu **MessageService** w kontenerze Unity, aby zainicjować usługę:

    (Fragment kodu — *ASP.NETe iniekcja zależności — Ex02 — rejestrowanie usługi komunikatów*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Dodaj odwołanie do przestrzeni nazw **MvcMusicStore. Factors** .

    (Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex02-Factors*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Zarejestruj **CustomViewPageActivator** jako aktywator strony widoku w kontenerze Unity:

    (Fragment kodu — *ASP.NET wtrysku zależności-Ex02-Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Zamień domyślny program rozpoznawania zależności ASP.NET MVC 4 z wystąpieniem **UnityDependencyResolver**. Aby to zrobić, Zastąp zawartość metody **Initialize** następującym kodem:

    (Fragment kodu — *ASP.NET-Ex02-test wtrysku zależności — Aktualizacja programu rozpoznawania zależności*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC udostępnia domyślną klasę programu rozpoznawania zależności. Aby można było korzystać z niestandardowych elementów rozpoznawania zależności jako tej, która została utworzona dla aparatu Unity, należy wymienić ten program rozpoznawania nazw.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — Uruchamianie aplikacji

W tym zadaniu uruchomisz aplikację w celu sprawdzenia, czy przeglądarka sklepu korzysta z usługi i pokazuje obraz oraz pobraną wiadomość:

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.
2. Kliknij pozycję **Rock** w menu gatuneks i zobacz, jak **MessageService** został wstrzykiwany do widoku i załadował Komunikat powitalny i obraz. W tym przykładzie wprowadzamy do &quot;**skalą**&quot;:

    ![Sklep MVC Music — iniekcja widoku](aspnet-mvc-4-dependency-injection/_static/image10.png "Sklep MVC Music — iniekcja widoku")

    *Sklep MVC Music — iniekcja widoku*
3. Zamknij okno przeglądarki.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Ćwiczenie 3: wprowadzanie filtrów akcji

Zgodnie z poprzednimi **niestandardowymi akcjami** w laboratorium — filtry dostosowania i iniekcji zostały wprowadzone przez użytkownika. W tym ćwiczeniu dowiesz się, jak wstrzyknąć filtry z iniekcją zależności przy użyciu kontenera Unity. W tym celu należy dodać do rozwiązania do sklepu muzycznego filtr akcji niestandardowej, który będzie śledzić działanie lokacji.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Zadanie 1 — w tym filtr śledzenia w rozwiązaniu

W tym zadaniu dojdziesz do funkcji filtrowania akcji niestandardowych w sklepie muzyka. Ponieważ koncepcje filtrów niestandardowych są już traktowane w poprzednim &quot;ych niestandardowych filtrach akcji&quot;, wystarczy dołączyć klasę Filter z folderu Assets tego laboratorium, a następnie utworzyć dostawcę filtrów dla aparatu Unity:

1. Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **Source\Ex03-wstrzyknięcie akcji Filter\Begin** . W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
      > 
      > Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dołącz plik **TraceActionFilter.cs** z folderu **/sources/Assets** do **/filters** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Ten filtr akcji niestandardowej wykonuje śledzenie ASP.NET. Aby uzyskać więcej informacji, możesz sprawdzić, &quot;ASP.NET lokalny i dynamiczny filtrów akcji&quot; Lab.
3. Dodaj pustą klasę **FilterProvider.cs** do projektu w folderze **/Filters.**
4. Dodaj przestrzenie nazw **System. Web. MVC** i **Microsoft. Practices. Unity** w **FilterProvider.cs**.

    (Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03-Filter Provider Dodawanie przestrzeni nazw*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Ustaw klasę jako dziedziczą z interfejsu **IFilterProvider** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Dodaj właściwość **IUnityContainer** w klasie **FilterProvider** , a następnie Utwórz konstruktora klasy w celu przypisania kontenera.

    (Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03 — Konstruktor dostawcy filtrów*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Konstruktor klasy dostawcy filtrów nie tworzy **nowego** obiektu wewnątrz. Kontener jest przekazaniem jako parametr, a zależność jest rozwiązany przez Unity.
7. W klasie **FilterProvider** Zaimplementuj metodę **getfilters** z interfejsu **IFilterProvider** .

    (Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03-Filter Provider Getfilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Zadanie 2 — rejestrowanie i włączanie filtru

W tym zadaniu zostanie włączone śledzenie witryn. W tym celu należy zarejestrować filtr w metodzie **Bootstrapper.cs BuildUnityContainer** , aby rozpocząć śledzenie:

1. Otwórz **plik Web. config** znajdujący się w katalogu głównym projektu i Włącz śledzenie śledzenia w grupie system. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Otwórz **Bootstrapper.cs** w katalogu głównym projektu.
3. Dodaj odwołanie do przestrzeni nazw **MvcMusicStore. filters** .

    (Fragment kodu — *ASP.NET wtrysk zależności Lab-Ex03-program inicjujący Dodawanie przestrzeni nazw*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Wybierz metodę **BuildUnityContainer** i zarejestruj filtr w kontenerze Unity. Konieczne będzie zarejestrowanie dostawcy filtrów oraz filtru akcji.

    (Fragment kodu — *ASP.NET-Ex03-Register-FilterProvider i ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — Uruchamianie aplikacji

W tym zadaniu uruchomisz aplikację i przetestujesz, że filtr akcji niestandardowej jest śledzony dla działania:

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.
2. Kliknij pozycję **Rock** w menu gatuneks. Jeśli chcesz, możesz przejść do kolejnych gatunków.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music Store*
3. Przejdź do **/Trace.axd** , aby wyświetlić stronę śledzenia aplikacji, a następnie kliknij pozycję **Wyświetl szczegóły**.

    ![Dziennik śledzenia aplikacji](aspnet-mvc-4-dependency-injection/_static/image12.png "Dziennik śledzenia aplikacji")

    *Dziennik śledzenia aplikacji*

    ![Śledzenie aplikacji — szczegóły żądania](aspnet-mvc-4-dependency-injection/_static/image13.png "Śledzenie aplikacji — szczegóły żądania")

    *Śledzenie aplikacji — szczegóły żądania*
4. Zamknij okno przeglądarki.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Korzystając z tego praktycznego laboratorium, wiesz już, jak używać iniekcji zależności w ASP.NET MVC 4 przez integrację aparatu Unity przy użyciu pakietu NuGet. Aby to osiągnąć, można użyć iniekcji zależności wewnątrz kontrolerów, widoków i filtrów akcji.

Omówione zostały następujące pojęcia:

- Funkcje wstrzykiwania zależności ASP.NET MVC 4
- Integracja aparatu Unity przy użyciu pakietu NuGet Unity. Mvc3
- Iniekcja zależności w kontrolerach
- Iniekcja zależności w widokach
- Iniekcja zależności filtrów akcji

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do obszaru [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169) (Ustawienia — Integracje i usługi). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *Kafelek VS Express for Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-dependency-injection/_static/image19.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-dependency-injection/_static/image20.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-dependency-injection/_static/image21.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-dependency-injection/_static/image22.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
2. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-dependency-injection/_static/image24.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

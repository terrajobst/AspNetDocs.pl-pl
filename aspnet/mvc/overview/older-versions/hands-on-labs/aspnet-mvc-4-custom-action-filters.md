---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtry akcji niestandardowych ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC udostępnia filtry akcji do wykonywania logiki filtrowania przed wywołaniem lub po wywołaniu metody akcji. Filtry akcji są atrybutami niestandardowymi określona...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579696"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 — niestandardowe filtry akcji

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

ASP.NET MVC udostępnia filtry akcji do wykonywania logiki filtrowania przed wywołaniem lub po wywołaniu metody akcji. Filtry akcji są atrybutami niestandardowymi, które zapewniają deklaratywne środki do dodawania działań przed akcją i po akcji do metod akcji kontrolera.

W tym ćwiczeniu można utworzyć niestandardowy atrybut filtru akcji w rozwiązaniu MvcMusicStore w celu przechwycenia żądań kontrolera i zarejestrowania działania lokacji w tabeli bazy danych. Można dodać filtr rejestrowania przez iniekcję do dowolnego kontrolera lub akcji. Na koniec zostanie wyświetlony widok dziennika, w którym zostanie wyświetlona lista odwiedzających.

W ramach tego ćwiczenia praktycznego założono, że masz podstawową wiedzę na temat **ASP.NET MVC**. Jeśli wcześniej nie korzystasz z **ASP.NET MVC** , zalecamy przechodzenie przez **ASP.NETe podstawowe wskazówki dotyczące MVC 4** .

> [!NOTE]
> Wszystkie przykładowe kod i fragmenty kodu są dołączone do zestawu szkoleniowego dla sieci Web Camp, dostępnego w [wersji Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzny dla tego laboratorium jest dostępny w [niestandardowych filtrach akcji ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Utwórz atrybut niestandardowego filtru akcji, aby zwiększyć możliwości filtrowania
- Stosowanie niestandardowego atrybutu filtru przez iniekcję do określonego poziomu
- Globalne rejestrowanie filtrów akcji niestandardowych

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

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku C: Using fragmenty kodu](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Ćwiczenie 1: akcje rejestrowania](#Exercise1)
2. [Ćwiczenie 2: Zarządzanie wieloma filtrami akcji](#Exercise2)

Szacowany czas wykonywania tego laboratorium: **30 minut**.

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Ćwiczenie 1: akcje rejestrowania

W tym ćwiczeniu dowiesz się, jak utworzyć niestandardowy filtr dziennika akcji przy użyciu dostawców filtrów ASP.NET MVC 4. W tym celu należy zastosować filtr rejestrowania do witryny MusicStore, która będzie rejestrować wszystkie działania na wybranych kontrolerach.

Filtr rozszerzy **ActionFilterAttributeClass** i zastąpi metodę **OnActionExecuting** , aby przechwycić każde żądanie, a następnie wykonać akcje rejestrowania. Informacje kontekstowe dotyczące żądań HTTP, wykonywania metod, wyników i parametrów zostaną dostarczone przez klasę ASP.NET MVC **ActionExecutingContext** **.**

> [!NOTE]
> ASP.NET MVC 4 ma także domyślnych dostawców filtrów, których można użyć bez tworzenia filtru niestandardowego. ASP.NET MVC 4 udostępnia następujące typy filtrów:
> 
> - Filtr **autoryzacji** , który podejmuje decyzje dotyczące zabezpieczeń, aby wykonać metodę akcji, taką jak uwierzytelnianie lub Walidacja właściwości żądania.
> - Filtr **akcji** , który otacza wykonywanie metody akcji. Ten filtr może wykonywać dodatkowe przetwarzanie, takie jak dostarczanie dodatkowych danych do metody akcji, badanie wartości zwracanej lub anulowanie wykonywania metody akcji
> - Filtr **wyników** , który otacza wykonywanie obiektu ActionResult. Ten filtr może wykonać dodatkowe przetwarzanie wyniku, na przykład modyfikując odpowiedź HTTP.
> - Filtr **wyjątku** , który jest wykonywany, jeśli wystąpił nieobsługiwany wyjątek w metodzie działania, rozpoczynając od filtrów autoryzacji i kończąc na wykonaniu wyniku. Filtry wyjątków mogą służyć do zadań takich jak rejestrowanie lub wyświetlanie strony błędu.
> 
> Więcej informacji o dostawcach filtrów można znaleźć w tym łączu MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informacje o funkcji rejestrowania aplikacji ze sklepu MVC Music

To rozwiązanie do przechowywania utworów muzycznych ma nową tabelę modelu danych do rejestrowania witryn, **ActionLog**z następującymi polami: nazwa kontrolera, który odebrał żądanie, o nazwie Action, IP klienta i sygnatura czasowa.

![Model danych. Tabela ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Model danych. Tabela ActionLog.")

*Model danych — tabela ActionLog*

Rozwiązanie udostępnia widok ASP.NET MVC dla dziennika akcji, który można znaleźć w **MvcMusicStores/views/ActionLog**:

![Widok dziennika akcji](aspnet-mvc-4-custom-action-filters/_static/image2.png "Widok dziennika akcji")

*Widok dziennika akcji*

W przypadku tej struktury cała służbowa będzie skoncentrowana na żądaniu na kontrolerze i przeprowadzeniu rejestrowania przy użyciu filtrowania niestandardowego.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Zadanie 1 — Tworzenie niestandardowego filtru do przechwytywania żądania kontrolera

W tym zadaniu utworzysz klasę niestandardowego atrybutu filtru, która będzie zawierać logikę rejestrowania. W tym celu zostanie rozszerzona Klasa ASP.NET MVC **ActionFilterAttribute** i zaimplementowano Interfejs **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** jest klasą bazową dla wszystkich filtrów atrybutów. Udostępnia następujące metody, aby wykonać określoną logikę po wykonaniu akcji kontrolera i przed nią:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): tuż przed wywołaniem metody Action.
> - **OnActionExecuted**(ActionExecutedContext filterContext): po wywołaniu metody akcji i przed wykonaniem wyniku (przed renderowaniem widoku).
> - **OnResultExecuting**(ResultExecutingContext filterContext): tuż przed wykonaniem wyniku (przed renderowaniem widoku).
> - **OnResultExecuted**(ResultExecutedContext filterContext): po wykonaniu wyniku (po wyrenderowaniu widoku).
> 
> Zastępowanie którejkolwiek z tych metod w klasie pochodnej, można wykonać własny kod filtrowania.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **\Source\Ex01-LoggingActions\Begin** .

   1. Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
      > 
      > Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dodaj nową C# klasę do folderu **filters** i nadaj jej nazwę *CustomActionFilter.cs*. W tym folderze będą przechowywane wszystkie filtry niestandardowe.
3. Otwórz **CustomActionFilter.cs** i Dodaj odwołanie do przestrzeni nazw **System. Web. MVC** i **MvcMusicStore. models** :

    (Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex1-CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Dziedzicz klasę **CustomActionFilter** z **ActionFilterAttribute** , a następnie ustaw klasę **CustomActionFilter** implementującą interfejs **IActionFilter** .

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Utwórz klasę **CustomActionFilter** , zastępując metodę **OnActionExecuting** i Dodaj wymaganą logikę w celu zarejestrowania wykonywania filtru. W tym celu Dodaj następujący wyróżniony kod w klasie **CustomActionFilter** .

    (Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex1-LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > Metoda **OnActionExecuting** używa **Entity Framework** , aby dodać nowy rejestr ActionLog. Tworzy i wypełnia nowe wystąpienie jednostki z informacjami kontekstu z **filterContext**.
    > 
    > Więcej informacji na temat klasy **ControllerContext** można znaleźć w [witrynie MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Zadanie 2 — wprowadzanie interceptora kodu do klasy kontrolera magazynu

W tym zadaniu zostanie dodany filtr niestandardowy przez wstrzyknięcie go do wszystkich klas kontrolera i akcji kontrolerów, które będą rejestrowane. Na potrzeby tego ćwiczenia Klasa kontrolera magazynu będzie miała dziennik.

Metoda **OnActionExecuting** z niestandardowego filtru **ActionLogFilterAttribute** jest uruchamiana po wywołaniu wstrzykniętego elementu.

Istnieje również możliwość przechwycenia konkretnej metody kontrolera.

1. Otwórz **StoreController** w **MvcMusicStore\Controllers** i Dodaj odwołanie do przestrzeni nazw **filters** :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Wstrzyknąć filtr niestandardowy **CustomActionFilter** do klasy **StoreController** przez dodanie atrybutu **[CustomActionFilter]** przed deklaracją klasy.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Gdy filtr jest wstrzykiwany do klasy kontrolera, wszystkie jego działania są również wstrzykiwane. Jeśli chcesz zastosować filtr tylko dla zestawu akcji, musisz wstrzyknąć **[CustomActionFilter]** do każdego z nich:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — Uruchamianie aplikacji

W tym zadaniu zostanie przetestowane, że filtr rejestrowania działa. Uruchomisz aplikację i przejdziesz do sklepu, a następnie sprawdzisz zarejestrowane działania.

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.
2. Przejdź do **/ActionLog** , aby wyświetlić stan początkowy widoku dziennika:

    ![Stan śledzenia dzienników przed aktywnością strony](aspnet-mvc-4-custom-action-filters/_static/image3.png "Stan śledzenia dzienników przed aktywnością strony")

    *Stan śledzenia dzienników przed aktywnością strony*

   > [!NOTE]
   > Domyślnie zawsze będzie wyświetlany jeden element, który jest generowany podczas pobierania istniejących gatunków menu.
   > 
   > W celach prostoty czyścimy tabelę **ActionLog** za każdym razem, gdy aplikacja zostanie uruchomiona, aby wyświetlić tylko dzienniki poszczególnych zadań.
   > 
   > Aby zapisać dziennik historyczny dla wszystkich akcji wykonywanych w ramach kontrolera magazynu, może być konieczne usunięcie następującego kodu z **sesji\_Metoda Start** (w klasie **Global. asax** ).
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.
4. Przejdź do **/ActionLog** i jeśli dziennik jest pusty, naciśnij klawisz **F5** , aby odświeżyć stronę. Sprawdź, czy Twoje wizyty zostały śledzone:

    ![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image4.png "Dziennik akcji z zarejestrowanym działaniem")

    *Dziennik akcji z zarejestrowanym działaniem*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Ćwiczenie 2: Zarządzanie wieloma filtrami akcji

W tym ćwiczeniu zostanie dodany drugi filtr akcji niestandardowej do klasy StoreController i zostanie określona kolejność wykonywania obu filtrów. Następnie zaktualizujesz kod w celu zarejestrowania globalnie filtru.

Istnieją różne opcje, które należy wziąć pod uwagę podczas definiowania kolejności wykonywania filtrów. Na przykład Właściwość Order i zakres filters:

Można zdefiniować **zakres** dla każdego z filtrów, na przykład można określić zakres wszystkich filtrów akcji do uruchomienia w ramach **zakresu kontrolera**i wszystkie filtry autoryzacji do uruchomienia w **zakresie globalnym**. Zakresy mają zdefiniowane zamówienie wykonania.

Ponadto każdy filtr akcji ma Właściwość Order, która jest używana do określenia kolejności wykonywania w zakresie filtru.

Aby uzyskać więcej informacji na temat kolejności wykonywania filtrów akcji niestandardowych, odwiedź ten artykuł w witrynie MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Zadanie 1. Tworzenie nowego niestandardowego filtru akcji

W tym zadaniu utworzysz nowy niestandardowy filtr akcji, aby wstrzyknąć do klasy StoreController, jak zarządzać kolejnością wykonywania filtrów.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **\Source\Ex02-ManagingMultipleActionFilters\Begin** . W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

    1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
    2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
    3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

        > [!NOTE]
        > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
        > 
        > Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dodaj nową C# klasę do folderu **filters** i nadaj jej nazwę *MyNewCustomActionFilter.cs*
3. Otwórz **MyNewCustomActionFilter.cs** i Dodaj odwołanie do przestrzeni nazw **System. Web. MVC** i **MvcMusicStore. models** :

    (Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex2-MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Zastąp domyślną deklarację klasy poniższym kodem.

    (Fragment kodu — *niestandardowe filtry akcji ASP.NET MVC 4-Ex2-MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Ten filtr akcji niestandardowej jest prawie taki sam jak ten, który został utworzony w poprzednim ćwiczeniu. Główną różnicą jest to, że ma *&quot;zarejestrowany przez atrybut&quot;* aktualizowany przy użyciu tej nowej klasy, aby zidentyfikować filtr zarejestrowany w dzienniku.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Zadanie 2: wprowadzanie nowego przechwycenia kodu do klasy StoreController

W tym zadaniu dodasz nowy filtr niestandardowy do klasy StoreController i uruchomisz rozwiązanie, aby sprawdzić, jak oba filtry współpracują ze sobą.

1. Otwórz klasę **StoreController** znajdującą się w **MvcMusicStore\Controllers** i wstrzyknąć nowy filtr niestandardowy **MyNewCustomActionFilter** do klasy **StoreController** , jak pokazano w poniższym kodzie.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Teraz uruchom aplikację, aby zobaczyć, jak działają te dwa niestandardowe filtry akcji. Aby to zrobić, naciśnij klawisz **F5** i poczekaj na uruchomienie aplikacji.
3. Przejdź do **/ActionLog** , aby wyświetlić stan początkowy widoku dziennika.

    ![Stan śledzenia dzienników przed aktywnością strony](aspnet-mvc-4-custom-action-filters/_static/image5.png "Stan śledzenia dzienników przed aktywnością strony")

    *Stan śledzenia dzienników przed aktywnością strony*
4. Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.
5. Sprawdź ten czas; wizyty zostały śledzone dwa razy: jeden raz dla każdego z filtrów akcji niestandardowych, które zostały dodane w klasie **StorageController** .

    ![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image6.png "Dziennik akcji z zarejestrowanym działaniem")

    *Dziennik akcji z zarejestrowanym działaniem*
6. Zamknij okno przeglądarki.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Zadanie 3. Zarządzanie porządkowaniem filtrów

W tym zadaniu dowiesz się, jak zarządzać kolejnością wykonywania filtrów za pomocą właściwości Order.

1. Otwórz klasę **StoreController** znajdującą się w **MvcMusicStore\Controllers** i określ Właściwość **Order** w obu filtrach, jak pokazano poniżej.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Teraz sprawdź, w jaki sposób filtry są wykonywane, w zależności od wartości właściwości Order. Zobaczysz, że filtr o najmniejszej wartości kolejności (**CustomActionFilter**) to pierwszy, który jest wykonywany. Naciśnij klawisz **F5** i poczekaj na uruchomienie aplikacji.
3. Przejdź do **/ActionLog** , aby wyświetlić stan początkowy widoku dziennika.

    ![Stan śledzenia dzienników przed aktywnością strony](aspnet-mvc-4-custom-action-filters/_static/image7.png "Stan śledzenia dzienników przed aktywnością strony")

    *Stan śledzenia dzienników przed aktywnością strony*
4. Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.
5. Sprawdź, czy w tym czasie wizyty zostały śledzone uporządkowane według wartości kolejności filtrów: **CustomActionFilter** Logs '.

    ![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image8.png "Dziennik akcji z zarejestrowanym działaniem")

    *Dziennik akcji z zarejestrowanym działaniem*
6. Teraz zaktualizujesz wartość kolejności filtrów i sprawdź, jak zmienia się kolejność rejestrowania. W klasie **StoreController** zaktualizuj wartość kolejności filtrów, jak pokazano poniżej.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Uruchom aplikację ponownie, naciskając klawisz **F5**.
8. Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.
9. Sprawdź, czy w tym czasie najpierw pojawiają się dzienniki utworzone za pomocą filtru **MyNewCustomActionFilter** .

    ![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image9.png "Dziennik akcji z zarejestrowanym działaniem")

    *Dziennik akcji z zarejestrowanym działaniem*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Zadanie 4. rejestrowanie filtrów globalnie

W tym zadaniu aktualizujesz rozwiązanie w celu zarejestrowania nowego filtru (**MyNewCustomActionFilter**) jako filtru globalnego. W ten sposób zostanie wyzwolone przez wszystkie akcje wykonywane w aplikacji, a nie tylko w StoreController, jak w poprzednim zadaniu.

1. W klasie **StoreController** Usuń atrybut **[MyNewCustomActionFilter]** i Właściwość Order z **[CustomActionFilter]** . Powinien wyglądać następująco:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Otwórz plik **Global. asax** i znajdź **aplikację\_Start** . Należy zauważyć, że za każdym razem, gdy aplikacja uruchamia ją, rejestruje filtry globalne przez wywołanie metody **RegisterGlobalFilters** w klasie **FilterConfig** .

    ![Rejestrowanie filtrów globalnych w Global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Rejestrowanie filtrów globalnych w Global. asax")

    *Rejestrowanie filtrów globalnych w Global. asax*
3. Otwórz plik **FilterConfig.cs** w folderze **Start\_** .
4. Dodaj odwołanie do programu System. Web. MVC; Korzystanie z MvcMusicStore. filters; obszaru.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aktualizowanie metody **RegisterGlobalFilters** Dodawanie filtru niestandardowego. Aby to zrobić, Dodaj wyróżniony kod:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Uruchom aplikację, naciskając klawisz **F5**.
7. Kliknij jeden z **gatunków** z menu i wykonaj kilka akcji, takich jak przeglądanie dostępnego albumu.
8. Sprawdź, czy teraz **[MyNewCustomActionFilter]** jest wstrzykiwany w HomeController i ActionLogController.

    ![Dziennik akcji z zarejestrowanym działaniem](aspnet-mvc-4-custom-action-filters/_static/image11.png "Dziennik akcji z zarejestrowanym działaniem")

    *Dziennik akcji z zarejestrowanym działaniem globalnym*

> [!NOTE]
> Ponadto tę aplikację można wdrożyć w witrynach sieci Web systemu Windows Azure, które zostały opisane w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Dzięki zakończeniu tego praktycznego laboratorium wiesz już, jak zwiększyć filtr akcji do wykonywania akcji niestandardowych. Dowiesz się również, jak wstrzyknąć dowolny filtr do kontrolerów strony. Zostały użyte następujące pojęcia:

- Jak utworzyć niestandardowe filtry akcji przy użyciu klasy ASP.NET MVC ActionFilterAttribute
- Jak wstrzyknąć filtry do kontrolerów ASP.NET MVC
- Jak zarządzać kolejnością filtrowania przy użyciu właściwości Order
- Jak zarejestrować filtry globalnie

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do obszaru [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169) (Ustawienia — Integracje i usługi). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *Kafelek VS Express for Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web na podstawie portal zarządzania systemu Windows Azure i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy dostępnej w systemie Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web z poziomu portalu systemu Windows Azure

1. Przejdź do [Portal zarządzania systemu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > System Windows Azure umożliwia bezpłatne hostowanie witryn sieci Web 10 ASP.NET, a następnie skalowanie ich w miarę wzrostu ruchu. Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do systemu Windows Azure Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Zaloguj się do systemu Windows Azure Portal")

    *Zaloguj się do usługi Windows Azure portal zarządzania*
2. Na pasku poleceń kliknij pozycję **Nowy** .

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Tworzenie nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**. Następnie wybierz opcję **szybkie tworzenie** . Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryna sieci Web systemu Windows Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią. Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web w witrynie sieci Web systemu Windows Azure spoza portalu. Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](aspnet-mvc-4-custom-action-filters/_static/image19.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")

    *Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*
4. Poczekaj na utworzenie nowej **witryny sieci Web** .
5. Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** . Sprawdź, czy nowa witryna sieci Web działa.

    ![Przeglądanie w nowej witrynie sieci Web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Przeglądanie w nowej witrynie sieci Web")

    *Przeglądanie w nowej witrynie sieci Web*

    ![Uruchomiona witryna sieci Web](aspnet-mvc-4-custom-action-filters/_static/image21.png "Uruchomiona witryna sieci Web")

    *Uruchomiona witryna sieci Web*
6. Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania witryną sieci Web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Otwieranie stron zarządzania witryną sieci Web")

    *Otwieranie stron zarządzania witryną sieci Web*
7. Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web w witrynie sieci Web systemu Windows Azure dla każdej z włączonych metod publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji. **Program Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów do publikowania aplikacji sieci Web w usłudze Microsoft Azure Websites.

    ![Pobieranie profilu publikowania witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Pobieranie profilu publikowania witryny sieci Web")

    *Pobieranie profilu publikowania witryny sieci Web*
8. Pobierz plik profilu publikowania do znanej lokalizacji. W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web w witrynach sieci Web systemu Windows Azure z programu Visual Studio.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image24.png "Zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer. Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.

1. Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database. Serwery SQL Database z subskrypcji można wyświetlić w portalu zarządzania systemu Windows Azure w obszarze **bazy danych SQL** | **serwery** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń. Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach. Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.

    ![Pulpit nawigacyjny serwera SQL Database](aspnet-mvc-4-custom-action-filters/_static/image25.png "Pulpit nawigacyjny serwera SQL Database")

    *Pulpit nawigacyjny serwera SQL Database*
2. W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera. W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](aspnet-mvc-4-custom-action-filters/_static/image26.png).

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Dodawanie adresu IP klienta*
3. Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.

    ![Potwierdź zmiany](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.

    ![Publikowanie aplikacji](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publikowanie aplikacji")

    *Publikowanie witryny sieci Web*
2. Zaimportuj profil publikowania zapisany w pierwszym zadaniu.

    ![Importowanie profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij pozycję **Weryfikuj połączenie**. Po zakończeniu walidacji kliknij przycisk **dalej**.

    > [!NOTE]
    > Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.

    ![Weryfikowanie połączenia](aspnet-mvc-4-custom-action-filters/_static/image31.png "Weryfikowanie połączenia")

    *Weryfikowanie połączenia*
4. Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).

    ![Konfiguracja narzędzia Web Deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Konfiguracja narzędzia Web Deploy")

    *Konfiguracja narzędzia Web Deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .
   - W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.
   - W polu **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nową nazwę bazy danych.

     ![Konfigurowanie parametrów połączenia docelowego](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurowanie parametrów połączenia docelowego")

     *Konfigurowanie parametrów połączenia docelowego*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-custom-action-filters/_static/image34.png "Tworzenie ciągu bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie. Następnie kliknij przycisk **Next** (Dalej).

    ![Parametry połączenia wskazujące SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Parametry połączenia wskazujące SQL Database")

    *Parametry połączenia wskazujące SQL Database*
8. Na stronie **Podgląd** kliknij przycisk **Publikuj**.

    ![Publikowanie aplikacji sieci Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publikowanie aplikacji sieci Web")

    *Publikowanie aplikacji sieci Web*
9. Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-custom-action-filters/_static/image37.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-custom-action-filters/_static/image38.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-custom-action-filters/_static/image39.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-custom-action-filters/_static/image40.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
2. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-custom-action-filters/_static/image42.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

---
title: Tworzenie pierwszej aplikacji składniki Razor
author: guardrex
description: Tworzenie składników Razor aplikacji krok po kroku i pojęcia dotyczące podstawowych składników Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57070871"
---
# <a name="build-your-first-razor-components-app"></a>Tworzenie pierwszej aplikacji składniki Razor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

W tym samouczku pokazano, jak utworzyć aplikację ze składnikami Razor i pokazuje podstawowe pojęcia składniki Razor. Można korzystać z tego samouczka przy użyciu obu składników Razor na podstawie projektu (obsługiwane w programie .NET Core 3.0 lub nowszej) lub projektu na podstawie Blazor (obsługiwane w przyszłej wersji programu .NET Core).

Środowisko za pomocą platformy ASP.NET Core Razor składników (*zalecane*):

* Postępuj zgodnie ze wskazówkami w <xref:razor-components/get-started> do tworzenia projektu na podstawie składników Razor.
* Nadaj projektowi nazwę `RazorComponents`.
* Rozwiązanie z wieloma projektami jest tworzona z szablonu Razor składników. Projekt Razor składników zostanie wygenerowany jako *RazorComponents.App*.

Aby uzyskać doświadczenie w korzystaniu z Blazor:

* Postępuj zgodnie ze wskazówkami w <xref:spa/blazor/get-started> do tworzenia projektu na podstawie Blazor.
* Nadaj projektowi nazwę `Blazor`.
* Rozwiązania jednego projektu jest tworzona z szablonu Blazor.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample)). Zobacz następujące tematy dotyczące wymagań wstępnych:

## <a name="build-components"></a>Tworzenie składników

1. Przejdź do wszystkich aplikacji trzy strony: Strona główna licznik i pobierania danych. Te strony są implementowane przez pliki Razor w *stron* folderu: *Index.cshtml*, *Counter.cshtml*, i *FetchData.cshtml*.

1. Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.

1. Zapoznania się z implementacją składnikiem licznika w *Counter.cshtml* pliku.

   *Pages/Counter.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   Interfejs użytkownika składnika licznika jest zdefiniowana za pomocą kodu HTML. Logika renderowania dynamicznego (na przykład pętli, warunkowych, wyrażeń) zostanie dodany przy użyciu osadzonych C# składni o nazwie [Razor](xref:mvc/views/razor). Kod znaczników HTML i C# logiki renderowania są konwertowane na klasę składnika w czasie kompilacji. Nazwa wygenerowanej klasy .NET zgodny z nazwą pliku.

   Elementy członkowskie klasy składników są zdefiniowane w `@functions` bloku. W `@functions` blokować stan składnika (właściwości, pola) i metod są określone dla obsługi zdarzeń lub Definiowanie logiki innych składników. Te elementy członkowskie są następnie używane w ramach składnika renderowania logiki oraz obsługi zdarzeń.

   Gdy **kliknij mnie** wybrany przycisk:

   * Zarejestrowane składnik licznika `onclick` program obsługi jest wywoływany ( `IncrementCount` metody).
   * Składnik licznika generuje drzewo jego renderowania.
   * Nowe drzewo renderowania w porównaniu do poprzedniego.
   * Modyfikacje do modelu DOM (Document Object) są stosowane. Liczba wyświetlanych jest aktualizowana.

1. Modyfikowanie C# logiki licznika składnika zwiększenie liczby dwa, a nie jeden.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. Ponownie skompiluj i uruchom aplikację, aby zobaczyć zmiany. Wybierz **kliknij mnie** przycisk i zwiększa licznik przez dwa.

## <a name="use-components"></a>Używanie składników

Uwzględnij składnika w innym składniku przy użyciu składni notacji HTML.

1. Dodaj składnik licznika do aplikacji, składnik indeksu (strona główna), dodając `<Counter />` elementu składnik indeksu.

   Jeśli używasz Blazor dla tego środowiska, składnik ankiety monitu (`<SurveyPrompt>` elementu) znajduje się w składnik indeksu. Zastąp `<SurveyPrompt>` element z `<Counter>` elementu.

   *Pages/Index.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. Ponownie skompiluj i uruchom aplikację. Strona główna ma swój własny licznika.

## <a name="component-parameters"></a>Parametry składnika

Składniki mogą także mieć parametrów. Składnik parametry są definiowane za pomocą właściwości niepubliczne klasy składnika ozdobione `[Parameter]`. Używanie atrybutów, aby określić argumenty dla składnika w znacznikach.

1. Aktualizowanie składnika `@functions` C# kodu:

   * Dodaj `IncrementAmount` ozdobione właściwość `[Parameter]` atrybutu.
   * Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.

   *Pages/Counter.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu. Ustaw wartość do zwiększenia licznika dziesięciu.

   *Pages/Index.cshtml*:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. Ponownie załaduj stronę. Licznik strony głównej rośnie przez dziesięć za każdym razem **kliknij mnie** przycisk jest zaznaczony. Licznik na *licznika* stronie zwiększa o jeden.

## <a name="route-to-components"></a>Kierowanie do składników

`@page` Dyrektywę w górnej części *Counter.cshtml* pliku Określa, że ten składnik jest routingu punkt końcowy. Składnik licznika obsługuje żądania wysyłane do `/Counter`. Bez `@page` dyrektywy, składnik nie obsługuje żądania trasowane, ale nadal można używać składnika przez inne składniki.

## <a name="dependency-injection"></a>Wstrzykiwanie zależności

Zarejestrowane w kontenerze usługi app Services są dostępne dla składników za pomocą [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Wstrzyknięcie usług do składnika za pomocą `@inject` dyrektywy.

Sprawdź dyrektywy składnika FetchData (*Pages/FetchData.cshtml*). `@inject` Dyrektywy służy do dodania wystąpienia programu `WeatherForecastService` usługi do składnika:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

`WeatherForecastService` Usługi jest zarejestrowana jako [pojedyncze](xref:fundamentals/dependency-injection#service-lifetimes), więc jedno wystąpienie usługi jest dostępne w całej aplikacji.

Składnik FetchData używa usługi wprowadzonego jako `ForecastService`, aby pobrać tablicę `WeatherForecast` obiektów:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

A [ @foreach ](/dotnet/csharp/language-reference/keywords/foreach-in) pętli jest używany do renderowania każde wystąpienie prognozy jako wiersz w tabeli danych o pogodzie:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a>Tworzenie listy zadań do wykonania

Dodaj nową stronę do aplikacji, która implementuje listy zadań do wykonania prostego.

1. Dodaj pusty plik do *stron* folder o nazwie *Todo.cshtml*.

1. Podaj początkowe znaczniki dla strony:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Na stronie zadań do wykonania należy dodać do paska nawigacyjnego.

   Składnik NavMenu (*Shared/NavMenu.csthml*) jest używana w układzie aplikacji. Układy są składniki, które pozwalają uniknąć duplikowania zawartości w aplikacji. Aby uzyskać więcej informacji, zobacz <xref:razor-components/layouts>.

   Dodaj `<NavLink>` strony Todo, dodając następujące znaczniki elementu listy poniżej istniejące elementy listy w *Shared/NavMenu.csthml* pliku:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Ponownie skompiluj i uruchom aplikację. Odwiedź nową stronę Todo, aby upewnić się, czy działa link do strony zadań do wykonania.

1. Dodaj *TodoItem.cs* pliku w folderze głównym projektu na potrzeby przechowywania klasa, która reprezentuje element todo. Należy użyć następującego C# kod `TodoItem` klasy:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. Wróć do części Todo (*Todo.cshtml*):

   * Dodaj pole do zadań do wykonania w `@functions` bloku. Składnik Todo to pole jest używane do zarządzania stanem listy rzeczy do zrobienia.
   * Dodaj listę nieuporządkowaną znaczników i `foreach` pętli do renderowania każdego elementu todo, jako element listy.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. Aplikacja wymaga elementów interfejsu użytkownika do dodawania zadań do wykonania do listy. Dodaj przycisk poniżej listy i tekstowych danych wejściowych:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. Ponownie skompiluj i uruchom aplikację. Gdy **dodawania zadań do wykonania** przycisk jest zaznaczony, nic się nie dzieje, ponieważ program obsługi zdarzeń nie jest powiązaną przycisku.

1. Dodaj `AddTodo` metodę, aby składnik zadań do wykonania i zarejestruj go dla przycisku kliknie, za pomocą `onclick` atrybutu:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   `AddTodo` C# Metoda jest wywoływana po wybraniu przycisku.

1. Aby uzyskać tytuł elementu nowych zadań do wykonania, należy dodać `newTodo` ciąg pola i powiązać wartość wprowadzania przy użyciu tekstu `bind` atrybutu:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. Aktualizacja `AddTodo` metody w celu dodania `TodoItem` o określonym tytule do listy. Wyczyść wartość wprowadzania tekstu, ustawiając `newTodo` na pusty ciąg:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. Ponownie skompiluj i uruchom aplikację. Niektórych zadań do wykonania należy dodać do listy rzeczy do zrobienia, aby przetestować nowy kod.

1. Tekst tytułu dla każdego elementu todo zyski, można edytować i pola wyboru mogą pomóc użytkownikowi informacje o ukończonych elementów. Dodaj dane wejściowe pola wyboru dla każdego elementu todo i powiązać jej wartość na `IsDone` właściwości. Zmiana `@todo.Title` do `<input>` powiązany element `@todo.Title`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. Aby sprawdzić, czy te wartości są powiązane, należy zaktualizować `<h1>` nagłówek, aby wyświetlić liczbę zadań do wykonania, które nie są kompletne (`IsDone` jest `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Ukończone składnika Todo (*Todo.cshtml*):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. Ponownie skompiluj i uruchom aplikację. Dodaj do testowania nowego kodu do wykonania.

## <a name="publish-and-deploy-the-app"></a>Publikowanie i wdrażanie aplikacji

Aby opublikować aplikację, zobacz <xref:host-and-deploy/razor-components/index#publish-the-app>.

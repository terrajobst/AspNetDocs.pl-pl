---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Samouczek: Wprowadzenie z sygnałem 1. x i MVC 4 | Microsoft Docs'
author: bradygaster
description: Użyj ASP.NET Signaler i ASP.NET MVC 4 do kompilowania aplikacji czatu w czasie rzeczywistym.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579570"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Samouczek: wprowadzenie do usługi SignalR 1.x i wzorca MVC 4

[Fletcher Patryk](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym samouczku pokazano, jak za pomocą sygnalizacji ASP.NET utworzyć aplikację czatu w czasie rzeczywistym. Dodasz sygnał do aplikacji MVC 4 i utworzysz widok rozmowy do wysyłania i wyświetlania komunikatów.

## <a name="overview"></a>Omówienie

W tym samouczku przedstawiono tworzenie aplikacji sieci Web w czasie rzeczywistym za pomocą sygnalizacji ASP.NET oraz ASP.NET MVC 4. Samouczek używa tego samego kodu aplikacji czatu, co w [samouczku wprowadzenie](tutorial-getting-started-with-signalr.md), ale pokazuje, jak dodać go do aplikacji MVC 4 na podstawie szablonu internetowego.

W tym temacie przedstawiono następujące zadania programistyczne sygnalizujące:

- Dodawanie biblioteki sygnałów do aplikacji MVC 4.
- Tworzenie klasy centrów do wypychania zawartości do klientów.
- Używanie biblioteki Sygnalizującer na stronie sieci Web do wysyłania komunikatów i wyświetlania aktualizacji z centrum.

Poniższy zrzut ekranu przedstawia ukończoną aplikację czatu działającą w przeglądarce.

![Wystąpienia rozmów](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Poszczególne

- [Konfigurowanie projektu](#setup)
- [Uruchamianie przykładu](#run)
- [Sprawdzanie kodu](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

Wymagania wstępne:

- Visual Studio 2010 z dodatkiem SP1, Visual Studio 2012 lub Visual Studio 2012 Express. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET downloads](https://www.asp.net/downloads) , aby uzyskać bezpłatne narzędzie programistyczne programu Visual Studio 2012 Express.
- W przypadku programu Visual Studio 2010 zainstaluj [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

W tej sekcji pokazano, jak utworzyć aplikację ASP.NET MVC 4, dodać bibliotekę sygnałów i utworzyć aplikację czatu.

1. 1. W programie Visual Studio Utwórz aplikację ASP.NET MVC 4, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.

        > [!NOTE]
        > W programie VS 2010 wybierz pozycję **.NET Framework 4** w kontrolce listy rozwijanej wersji platformy. Kod sygnalizujący działa na .NET Framework wersjach 4 i 4,5.

        ![Tworzenie sieci Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Wybierz szablon aplikacja internetowa, usuń zaznaczenie opcji **Utwórz projekt testu jednostkowego**, a następnie kliknij przycisk OK.

         ![Utwórz witrynę internetową MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Otwórz **narzędzia > menedżerze pakietów NuGet > konsolę Menedżera pakietów** i uruchom następujące polecenie. Ten krok powoduje dodanie do projektu zestawu plików skryptów i odwołań do zestawów, które włączają funkcję sygnalizującą.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. W **Eksplorator rozwiązań** rozwiń folder skrypty. Zwróć uwagę, że biblioteki skryptów dla sygnalizującego zostały dodane do projektu.

         ![Odwołania do biblioteki](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj | Nowy folder**i Dodaj nowy folder o nazwie **Hubs**.
      6. Kliknij prawym przyciskiem myszy folder **Hubs** , a następnie kliknij pozycję **Dodaj |** I Utwórz nową C# klasę o nazwie **ChatHub.cs**. Ta klasa będzie używana jako centrum serwera sygnalizującego, który wysyła komunikaty do wszystkich klientów.

> [!NOTE]
> Jeśli używasz programu Visual Studio 2012 i zainstalowano [aktualizację ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), możesz użyć nowego szablonu elementu sygnalizującego, aby utworzyć klasę centrum. W tym celu kliknij prawym przyciskiem myszy folder **Hubs** , a następnie kliknij pozycję **Dodaj | Nowy element**, wybierz pozycję **centrum sygnałów Klasa (v1)** i nadaj klasie **ChatHub.cs**.

1. Zastąp kod w klasie **ChatHub** następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Otwórz plik **Global. asax** dla projektu i Dodaj wywołanie do metody `RouteTable.Routes.MapHubs();` jako pierwszy wiersz kodu w metodzie `Application_Start`. Ten kod rejestruje domyślną trasę dla centrów sygnałów i musi być wywoływana przed zarejestrowaniem innych tras. Zakończono `Application_Start` Metoda wygląda podobnie do poniższego przykładu.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Edytuj klasę `HomeController` znalezioną w obszarze **controllers/HomeController. cs** i Dodaj następującą metodę do klasy. Ta metoda zwraca widok **rozmowy** , który zostanie utworzony w późniejszym kroku.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Kliknij prawym przyciskiem myszy w właśnie utworzonej metodzie `Chat` i kliknij przycisk **Dodaj widok** , aby utworzyć nowy plik widoku.
5. W oknie dialogowym **Dodawanie widoku** upewnij się, że pole wyboru jest zaznaczone, aby **użyć układu lub strony wzorcowej** (wyczyść inne pola wyboru), a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie widoku](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edytuj nowy plik widoku o nazwie **Chat. cshtml**. Po tagu &lt;H2&gt; wklej następującą sekcję &lt;DIV&gt; i `@section scripts` blok kodu na stronie. Ten skrypt umożliwia stronie wysyłanie komunikatów rozmowy i wyświetlanie komunikatów z serwera. Pełny kod widoku rozmowy zostanie wyświetlony w poniższym bloku kodu.

    > [!IMPORTANT]
    > Po dodaniu sygnalizującego i innych bibliotek skryptów do projektu programu Visual Studio Menedżer pakietów może instalować wersje skryptów, które są nowsze niż wersje przedstawione w tym temacie. Upewnij się, że odwołania do skryptu w kodzie są zgodne z wersjami bibliotek skryptów zainstalowanych w projekcie.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Zapisz wszystko** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchamianie przykładu

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania.
2. W linii adresowej przeglądarki Dołącz **/Home/chat** do adresu URL strony domyślnej dla projektu. Strona rozmowy zostanie załadowana w wystąpieniu przeglądarki i zostanie wyświetlony komunikat z prośbą o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Wprowadź nazwę użytkownika.
4. Skopiuj adres URL z linii adresowej przeglądarki i użyj go, aby otworzyć dwa więcej wystąpień przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
5. W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **Wyślij**. Komentarze powinny być wyświetlane we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze. Centrum emituje Komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.
6. Poniższy zrzut ekranu przedstawia aplikację czatu działającą w przeglądarce.

    ![Przeglądarki rozmowy](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji. Ten węzeł jest widoczny w trybie debugowania, jeśli używasz programu Internet Explorer jako przeglądarki. Istnieje plik skryptu o nazwie **Hubs** , który dynamicznie generuje Biblioteka sygnałów w czasie wykonywania. Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera. Jeśli używasz przeglądarki innej niż Internet Explorer, możesz również uzyskać dostęp do pliku **centrów** dynamicznych, przechodząc do niego bezpośrednio, na przykład http://mywebsite/signalr/hubs.

    ![Wygenerowany skrypt centralny](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Sprawdzanie kodu

Aplikacja rozmowy sygnalizująca pokazuje dwa podstawowe zadania programistyczne sygnalizujące: tworzenie centrum jako głównego obiektu koordynacji na serwerze i używanie biblioteki sygnalizującej jQuery do wysyłania i odbierania wiadomości.

### <a name="signalr-hubs"></a>Centra sygnałów

W przykładzie kodu Klasa **ChatHub** pochodzi od klasy **Microsoft. ASPNET. Signaler. Hub** . Wyprowadzanie z klasy **centrów** to przydatny sposób tworzenia aplikacji sygnalizującej. Można tworzyć metody publiczne w klasie centrów, a następnie uzyskiwać dostęp do tych metod, wywołując je ze skryptów jQuery na stronie sieci Web.

W kodzie rozmowy klienci wywołują metodę **ChatHub. Send** w celu wysłania nowej wiadomości. Koncentrator z kolei wysyła komunikat do wszystkich klientów, wywołując **klientów. ALL. addNewMessageToPage**.

Metoda **send** ilustruje kilka pojęć centrum:

- Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.
- Właściwość **Microsoft. ASPNET. signaler. Hub** umożliwia dostęp do wszystkich klientów podłączonych do tego centrum.
- Wywołaj funkcję jQuery na kliencie (na przykład funkcję `addNewMessageToPage`), aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Sygnalizujący i jQuery

Plik widoku **Chat. cshtml** w przykładowym kodzie pokazuje, w jaki sposób używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów. Zasadnicze zadania w kodzie tworzą odwołanie do automatycznie generowanego serwera proxy dla centrum, deklarując funkcję, którą serwer może wywoływać, aby wypchnąć zawartość do klientów i rozpocząć połączenie w celu wysyłania komunikatów do centrum.

Poniższy kod deklaruje serwer proxy dla centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> W jQuery odwołanie do klasy serwera i jej członków jest w notacji CamelCase przypadku. Przykładowy kod odwołuje się C# do klasy **ChatHub** w jQuery jako **ChatHub**. Jeśli chcesz odwołać się do klasy `ChatHub` w platformie jQuery przy użyciu konwencjonalnej wielkości liter języka C#Pascal jak w programie, edytuj plik klasy ChatHub.cs. Dodaj instrukcję `using`, aby odwoływać się do przestrzeni nazw `Microsoft.AspNet.SignalR.Hubs`. Następnie Dodaj atrybut `HubName` do klasy `ChatHub`, na przykład `[HubName("ChatHub")]`. Na koniec zaktualizuj odwołanie jQuery do klasy `ChatHub`.

Poniższy kod przedstawia sposób tworzenia funkcji wywołania zwrotnego w skrypcie. Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta. Opcjonalne wywołanie funkcji `htmlEncode` pokazuje sposób kodowania zawartości wiadomości w formacie HTML przed wyświetleniem jej na stronie jako sposób, aby zapobiec iniekcji skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Poniższy kod pokazuje, jak otworzyć połączenie z centrum. Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie rozmowy.

> [!NOTE]
> Takie podejście zapewnia, że połączenie zostanie nawiązane przed wykonaniem procedury obsługi zdarzeń.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z tym, że sygnalizujący jest platforma do tworzenia aplikacji sieci Web w czasie rzeczywistym. Poznasz również kilka zadań programistycznych sygnalizujących: jak dodać odbiornik do aplikacji ASP.NET, jak utworzyć klasę centrum oraz jak wysyłać i odbierać komunikaty z centrum.

Aby dowiedzieć się więcej na temat zaawansowanych pojęć związanych z założeniami, odwiedź następujące witryny dotyczące kodu źródłowego i zasobów usługi sygnalizującego:

- [Projekt sygnalizującego](http://signalr.net)
- [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)
- [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

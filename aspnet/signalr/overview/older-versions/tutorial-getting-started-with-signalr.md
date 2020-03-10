---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Samouczek: Wprowadzenie z sygnałem 1. x | Microsoft Docs'
author: bradygaster
description: Użyj sygnalizującego ASP.NET, aby skompilować aplikację czatu w czasie rzeczywistym na stronie HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623544"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a>Samouczek: wprowadzenie do usługi SignalR 1.x

[Fletcher Patryk](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Dodasz sygnał do pustej aplikacji sieci Web ASP.NET i utworzysz stronę HTML do wysyłania i wyświetlania komunikatów.

## <a name="overview"></a>Omówienie

Ten samouczek przedstawia programowanie sygnałów przez pokazanie sposobu tworzenia prostej aplikacji czatu opartej na przeglądarce. Dodasz bibliotekę sygnałów do pustej aplikacji sieci Web ASP.NET, utworzysz klasę centrum do wysyłania komunikatów do klientów i utworzysz stronę HTML, która umożliwia użytkownikom wysyłanie i odbieranie komunikatów rozmowy. Aby poznać podobny samouczek, w którym pokazano, jak utworzyć aplikację czatu w MVC 4 przy użyciu widoku MVC, zobacz [wprowadzenie z sygnalizującer i MVC 4](index.md).

> [!NOTE]
> W tym samouczku zostanie użyta wersja programu System (1. x). Aby uzyskać szczegółowe informacje na temat zmian między sygnałami 1. x i 2,0, zobacz [uaktualnianie projektów sygnalizujących 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).

Sygnalizujący to biblioteka .NET open source do tworzenia aplikacji sieci Web, które wymagają interakcji z użytkownikiem na żywo lub aktualizacji danych w czasie rzeczywistym. Przykładami mogą być aplikacje społecznościowe, gry wielodostępne, Współpraca biznesowa i wiadomości, Pogoda lub aktualizacje finansowe. Są one często nazywane aplikacjami w czasie rzeczywistym.

Sygnalizujący upraszcza proces tworzenia aplikacji w czasie rzeczywistym. Zawiera bibliotekę serwera ASP.NET i bibliotekę klienta JavaScript, która ułatwia zarządzanie połączeniami klienta i serwera oraz wypychanie aktualizacji zawartości do klientów. Do istniejącej aplikacji ASP.NET można dodać bibliotekę sygnalizującą w celu uzyskania funkcjonalności w czasie rzeczywistym.

Samouczek ilustruje następujące zadania programistyczne sygnalizujące:

- Dodawanie biblioteki sygnałów do aplikacji sieci Web ASP.NET.
- Tworzenie klasy centrów do wypychania zawartości do klientów.
- Używanie biblioteki Sygnalizującer na stronie sieci Web do wysyłania komunikatów i wyświetlania aktualizacji z centrum.

Poniższy zrzut ekranu przedstawia aplikację czatu działającą w przeglądarce. Każdy nowy użytkownik może publikować komentarze i wyświetlać komentarze dodane po przyłączeniu do rozmowy przez użytkownika.

![Wystąpienia rozmów](tutorial-getting-started-with-signalr/_static/image1.png)

Poszczególne

- [Konfigurowanie projektu](#setup)
- [Uruchamianie przykładu](#run)
- [Sprawdzanie kodu](#code)
- [Następne kroki](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web ASP.NET, dodać program sygnalizujący i utworzyć aplikację czatu.

Wymagania wstępne:

- Program Visual Studio 2010 z dodatkiem SP1 lub 2012. Jeśli nie masz programu Visual Studio, zobacz [ASP.NET downloads](https://www.asp.net/downloads) , aby uzyskać bezpłatne narzędzie programistyczne programu Visual Studio 2012 Express.
- [Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941). W przypadku programu Visual Studio 2012 ten Instalator dodaje nowe funkcje ASP.NET, w tym szablony sygnałów do programu Visual Studio. W przypadku programu Visual Studio 2010 z dodatkiem SP1 Instalator nie jest dostępny, ale można wykonać ten samouczek, instalując pakiet NuGet sygnalizujący zgodnie z opisem w kroku instalacji.

Poniższe kroki używają programu Visual Studio 2012 do utworzenia pustej aplikacji sieci Web ASP.NET i dodania biblioteki sygnałów:

1. W programie Visual Studio Utwórz pustą aplikację sieci Web ASP.NET.

    ![Tworzenie pustej sieci Web](tutorial-getting-started-with-signalr/_static/image2.png)
2. Otwórz **konsolę Menedżera pakietów** , wybierając pozycję **Narzędzia | Menedżer pakietów NuGet | Konsola Menedżera pakietów**. Wprowadź następujące polecenie w oknie konsoli:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    To polecenie powoduje zainstalowanie najnowszej wersji programu sygnalizującego 1. x.
3. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj | Klasa**. Nadaj nowej klasie nazwę **ChatHub**.
4. W **Eksplorator rozwiązań** rozwiń węzeł skrypty. Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.

    ![Odwołania do biblioteki](tutorial-getting-started-with-signalr/_static/image3.png)
5. Zastąp kod w klasie **ChatHub** następującym kodem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij polecenie **Dodaj | Nowy element**. W oknie dialogowym **Dodaj nowy element** wybierz pozycję **globalna Klasa aplikacji** , a następnie kliknij przycisk **Dodaj**.

    ![Dodaj globalny](tutorial-getting-started-with-signalr/_static/image4.png)
7. Dodaj następujące instrukcje `using` po podanych instrukcjach `using` w klasie Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Dodaj następujący wiersz kodu w metodzie `Application_Start` klasy globalnej w celu zarejestrowania trasy domyślnej dla centrów sygnałów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie kliknij polecenie **Dodaj | Nowy element**. W oknie dialogowym **Dodaj nowy element** wybierz pozycję Strona HTML, a następnie kliknij przycisk **Dodaj**.
10. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy właśnie UTWORZONĄ stronę HTML, a następnie kliknij pozycję **Ustaw jako stronę startową**.
11. Zastąp domyślny kod na stronie HTML następującym kodem.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Zapisz wszystko** dla projektu.

<a id="run"></a>

## <a name="run-the-sample"></a>Uruchamianie przykładu

1. Naciśnij klawisz F5, aby uruchomić projekt w trybie debugowania. Strona HTML zostanie załadowana w wystąpieniu przeglądarki i zostanie wyświetlony komunikat z prośbą o nazwę użytkownika.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image5.png)
2. Wprowadź nazwę użytkownika.
3. Skopiuj adres URL z linii adresowej przeglądarki i użyj go, aby otworzyć dwa więcej wystąpień przeglądarki. W każdym wystąpieniu przeglądarki wprowadź unikatową nazwę użytkownika.
4. W każdym wystąpieniu przeglądarki Dodaj komentarz, a następnie kliknij przycisk **Wyślij**. Komentarze powinny być wyświetlane we wszystkich wystąpieniach przeglądarki.

    > [!NOTE]
    > Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze. Centrum emituje Komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.

    Poniższy zrzut ekranu przedstawia aplikację czatu działającą w trzech wystąpieniach przeglądarki, z której wszystkie są aktualizowane, gdy jedno wystąpienie wysyła komunikat:

    ![Przeglądarki rozmowy](tutorial-getting-started-with-signalr/_static/image6.png)
5. W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji. Istnieje plik skryptu o nazwie **Hubs** , który dynamicznie generuje Biblioteka sygnałów w czasie wykonywania. Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera.

    ![Wygenerowany skrypt centralny](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Sprawdzanie kodu

Aplikacja rozmowy sygnalizująca pokazuje dwa podstawowe zadania programistyczne sygnalizujące: tworzenie centrum jako głównego obiektu koordynacji na serwerze i używanie biblioteki sygnalizującej jQuery do wysyłania i odbierania wiadomości.

### <a name="signalr-hubs"></a>Centra sygnałów

W przykładzie kodu Klasa **ChatHub** pochodzi od klasy **Microsoft. ASPNET. Signaler. Hub** . Wyprowadzanie z klasy **centrów** to przydatny sposób tworzenia aplikacji sygnalizującej. Można tworzyć metody publiczne w klasie centrów, a następnie uzyskiwać dostęp do tych metod, wywołując je ze skryptów jQuery na stronie sieci Web.

W kodzie rozmowy klienci wywołują metodę **ChatHub. Send** w celu wysłania nowej wiadomości. Koncentrator z kolei wysyła komunikat do wszystkich klientów, wywołując **klientów. ALL. broadcastMessage**.

Metoda **send** ilustruje kilka pojęć centrum:

- Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.
- Do uzyskiwania dostępu do wszystkich klientów podłączonych do tego centrum należy używać właściwości dynamicznej **Microsoft. ASPNET. signaler. Hub**
- Wywołaj funkcję jQuery na kliencie (na przykład funkcję `broadcastMessage`), aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Sygnalizujący i jQuery

Na stronie HTML w przykładzie kodu pokazano, jak używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów. Zasadnicze zadania w kodzie deklarują serwer proxy, aby odwołać się do centrum, deklarując funkcję, którą serwer może wywoływać, aby wypchnąć zawartość do klientów i rozpocząć połączenie w celu wysyłania komunikatów do centrum.

Poniższy kod deklaruje serwer proxy dla centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> W jQuery odwołanie do klasy serwera i jej członków jest w notacji CamelCase przypadku. Przykładowy kod odwołuje się C# do klasy **ChatHub** w jQuery jako **ChatHub**.

Poniższy kod przedstawia sposób tworzenia funkcji wywołania zwrotnego w skrypcie. Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta. Dwa wiersze, w których kod HTML koduje zawartość przed wyświetleniem, są opcjonalne i pokazują prostą metodę zapobiegania iniekcji skryptu.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Poniższy kod pokazuje, jak otworzyć połączenie z centrum. Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie HTML.

> [!NOTE]
> Takie podejście umożliwia upewnienie się, że połączenie zostało nawiązane przed wykonaniem procedury obsługi zdarzeń.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z tym, że sygnalizujący jest platforma do tworzenia aplikacji sieci Web w czasie rzeczywistym. Poznasz również kilka zadań programistycznych sygnalizujących: jak dodać odbiornik do aplikacji ASP.NET, jak utworzyć klasę centrum oraz jak wysyłać i odbierać komunikaty z centrum.

Przykładową aplikację można utworzyć w tym samouczku lub innych aplikacjach sygnalizujących dostępnych za pośrednictwem Internetu, wdrażając je u dostawcy hostingu. Firma Microsoft oferuje bezpłatny hosting w sieci Web dla maksymalnie 10 witryn sieci Web w ramach bezpłatnego [konta wersji próbnej platformy Microsoft Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Aby zapoznać się z przewodnikiem dotyczącym wdrażania przykładowej aplikacji sygnalizującej, zobacz temat [publikowanie wprowadzenie próbnika jako witryny sieci Web systemu Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Aby uzyskać szczegółowe informacje na temat wdrażania projektu sieci Web programu Visual Studio w witrynie sieci Web systemu Windows Azure, zobacz [wdrażanie aplikacji ASP.NET w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Uwaga: transport WebSocket nie jest obecnie obsługiwany w witrynach sieci Web systemu Windows Azure. Gdy transport WebSocket nie jest dostępny, sygnalizujący używa innych dostępnych transportów, zgodnie z opisem w sekcji Transports w [temacie Wprowadzenie do sygnalizacji](index.md).

Aby dowiedzieć się więcej na temat zaawansowanych pojęć związanych z założeniami, odwiedź następujące witryny dotyczące kodu źródłowego i zasobów usługi sygnalizującego:

- [Projekt sygnalizującego](http://signalr.net)
- [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)
- [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

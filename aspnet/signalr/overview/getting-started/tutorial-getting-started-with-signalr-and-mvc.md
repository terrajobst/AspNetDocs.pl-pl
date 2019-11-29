---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Samouczek: rozmowa w czasie rzeczywistym z sygnałami 2 i MVC 5 | Microsoft Docs'
author: bradygaster
description: W tym samouczku pokazano, jak za pomocą ASP.NET sygnalizujący 2 utworzyć aplikację czatu w czasie rzeczywistym. Dodajesz sygnał do aplikacji MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600484"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Samouczek: rozmowa w czasie rzeczywistym z sygnałami 2 i MVC 5

W tym samouczku pokazano, jak za pomocą ASP.NET sygnalizujący 2 utworzyć aplikację czatu w czasie rzeczywistym. Dodajesz sygnalizację do aplikacji MVC 5 i utworzysz widok rozmowy w celu wysyłania i wyświetlania komunikatów.

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchamianie przykładu
> * Sprawdzanie kodu

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i programowaniem w sieci Web** .

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji pokazano, jak za pomocą programu Visual Studio 2017 i sygnalizacji 2 utworzyć pustą aplikację ASP.NET MVC 5, dodać bibliotekę sygnałów i utworzyć aplikację czatu.

1. W programie Visual Studio Utwórz aplikację C# ASP.NET, która jest przeznaczona dla .NET Framework 4,5, nadaj jej nazwę SignalRChat, a następnie kliknij przycisk OK.

    ![Tworzenie sieci Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. W obszarze **Nowa aplikacja sieci Web ASP.NET — SignalRMvcChat**wybierz pozycję **MVC** , a następnie wybierz pozycję **Zmień uwierzytelnianie**.

1. W obszarze **Zmień uwierzytelnianie**wybierz pozycję **bez uwierzytelniania** i kliknij przycisk **OK**.

    ![Wybierz bez uwierzytelniania](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. W obszarze **Nowa aplikacja sieci Web ASP.NET — SignalRMvcChat**wybierz **przycisk OK**.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.

1. W obszarze **Dodaj nowy element — SignalRChat**wybierz pozycję **zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .

1. Nadaj klasie nazwę *ChatHub* i Dodaj ją do projektu.

    Ten krok tworzy plik klasy *ChatHub.cs* i dodaje zestaw plików skryptów oraz odwołania do zestawów, które obsługują sygnalizujący do projektu.

1. Zastąp kod w nowym pliku klasy *ChatHub.cs* tym kodem:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie Dodaj **klasę** > .

1. Nazwij nową klasę *uruchamiania* i Dodaj ją do projektu.

1. Zastąp kod w pliku klasy *Startup.cs* tym kodem:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. W **Eksplorator rozwiązań**wybierz pozycję **Kontrolery** > **HomeController.cs**.

1. Dodaj tę metodę do *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Ta metoda zwraca widok **rozmowy** , który został utworzony w późniejszym kroku.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **widoki** > **Strona główna**, a następnie wybierz polecenie **Dodaj** **Widok** >  .

1. W polu **Dodaj widok**Nadaj nazwę nowemu widokowi **rozmowy** i wybierz pozycję **Dodaj**.

1. Zastąp zawartość **czatu. cshtml** tym kodem:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.

    Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów mógł zainstalować nowszą wersję skryptów sygnalizującego.

1. Sprawdź, czy odwołania do skryptu w bloku kodu odpowiadają wersji plików skryptów w projekcie.

    Odwołania do skryptu z oryginalnego bloku kodu:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Jeśli nie są zgodne, zaktualizuj plik *. cshtml* .

1. Na pasku menu wybierz pozycję **plik** > **Zapisz wszystko**.

## <a name="run-the-sample"></a>Uruchamianie przykładu

1. Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Po otwarciu przeglądarki wprowadź nazwę swojej tożsamości rozmowy.

1. Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.

1. W każdej przeglądarce wprowadź unikatową nazwę.

1. Teraz Dodaj komentarz i wybierz pozycję **Wyślij**. Powtórz te czynności w innych przeglądarkach. Komentarze pojawiają się w czasie rzeczywistym.

    > [!NOTE]
    > Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze. Centrum emituje Komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.

    Zobacz, jak działa aplikacja czatu w trzech różnych przeglądarkach. Po wysłaniu komunikatów Anand i Susan wszystkie przeglądarki są aktualizowane w czasie rzeczywistym:

    ![Wszystkie trzy przeglądarki wyświetlają tę samą historię rozmowy](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji. Istnieje plik skryptu o nazwie *Hubs* , który jest generowany przez bibliotekę sygnałów w czasie wykonywania. Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera.

    ![wygenerowany automatycznie skrypt centrów w węźle dokumenty skryptu](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Sprawdzanie kodu

Aplikacja rozmowy sygnalizującej pokazuje dwa podstawowe zadania programistyczne sygnalizujące. Pokazano, jak utworzyć centrum. Serwer używa tego centrum jako głównego obiektu koordynacji. Do wysyłania i odbierania wiadomości centrum jest wykorzystywana Biblioteka sygnałów sygnalizujących.

### <a name="signalr-hubs-in-the-chathubcs"></a>Centra sygnałów w ChatHub.cs

W przykładzie kodu Klasa `ChatHub` dziedziczy z klasy `Microsoft.AspNet.SignalR.Hub`. Wyprowadzanie z klasy `Hub` jest użytecznym sposobem tworzenia aplikacji sygnalizującej. Można tworzyć metody publiczne w klasie centrów, a następnie uzyskiwać dostęp do tych metod, wywołując je ze skryptów na stronie sieci Web.

W kodzie rozmowy klienci wywołują metodę `ChatHub.Send` w celu wysłania nowej wiadomości. Koncentrator z kolei wysyła komunikat do wszystkich klientów, wywołując `Clients.All.addNewMessageToPage`.

Metoda `Send` ilustruje kilka pojęć centrum:

* Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.

* Użyj właściwości dynamicznej `Microsoft.AspNet.SignalR.Hub.Clients`, aby komunikować się ze wszystkimi klientami podłączonymi do tego centrum.

* Wywołaj funkcję na kliencie (na przykład funkcję `addNewMessageToPage`), aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>Sygnalizujący i jQuery Chat. cshtml

Plik widoku *Chat. cshtml* w przykładowym kodzie pokazuje, w jaki sposób używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów.  Kod wykonuje wiele ważnych zadań. Tworzy odwołanie do automatycznie generowanego serwera proxy dla centrum, deklaruje funkcję, którą serwer może wywoływać do wypychania zawartości do klientów i uruchamia połączenie w celu wysyłania komunikatów do centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> W języku JavaScript odwołanie do klasy serwera i jej członków jest w camelCase. Przykładowy kod odwołuje się C# do klasy `ChatHub` w języku JavaScript jako `chatHub`.

W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta. Opcjonalne wywołanie funkcji `htmlEncode` pokazuje sposób kodowania zawartości wiadomości w formacie HTML przed wyświetleniem jej na stronie. Jest to sposób, aby zapobiec iniekcji skryptu.

Ten kod otwiera połączenie z centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> To podejście zapewnia, że połączenie zostanie nawiązane przed wykonaniem procedury obsługi zdarzeń.

Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie rozmowy.

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:

* [Projekt sygnalizującego](http://signalr.net)

* [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)

* [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchomiono przykład
> * Zbadano kod

Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci Web, która używa ASP.NET sygnalizującego 2, aby zapewnić funkcję obsługi komunikatów o wysokiej częstotliwości.
> [!div class="nextstepaction"]
> [Aplikacja internetowa z obsługą komunikatów o wysokiej częstotliwości](tutorial-high-frequency-realtime-with-signalr.md)
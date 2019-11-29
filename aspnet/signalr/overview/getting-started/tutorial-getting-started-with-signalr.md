---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Samouczek: rozmowa w czasie rzeczywistym z sygnałem 2 | Microsoft Docs'
author: bradygaster
description: Ten samouczek pokazuje, jak utworzyć aplikację do obsługi rozmów w czasie rzeczywistym przy użyciu SignalR. Dodajesz sygnał do pustej aplikacji sieci Web ASP.NET.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600470"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Samouczek: rozmowa w czasie rzeczywistym z sygnałem 2

W tym samouczku pokazano, jak za pomocą programu sygnalizujący utworzyć aplikację czatu w czasie rzeczywistym. Dodajesz sygnał do pustej aplikacji sieci Web ASP.NET i utworzysz stronę HTML, aby wysyłać i wyświetlać komunikaty.

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchamianie przykładu
> * Sprawdzanie kodu

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* [Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i programowaniem w sieci Web** .

## <a name="set-up-the-project"></a>Konfigurowanie projektu

W tej sekcji pokazano, jak za pomocą programu Visual Studio 2017 i sygnalizacji 2 utworzyć pustą aplikację sieci Web ASP.NET, dodać program sygnalizujący i utworzyć aplikację czatu.

1. W programie Visual Studio Utwórz aplikację sieci Web ASP.NET.

    ![Tworzenie sieci Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. W oknie **nowy ASP.NET Project-SignalRChat** , pozostaw **puste** zaznaczone i wybierz **przycisk OK**.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.

1. W obszarze **Dodaj nowy element — SignalRChat**wybierz pozycję **zainstalowane** > **Visual C#**  > **sieci Web** > **sygnalizujący** , a następnie wybierz pozycję **Klasa centrum sygnałów (v2)** .

1. Nadaj klasie nazwę *ChatHub* i Dodaj ją do projektu.

    Ten krok tworzy plik klasy *ChatHub.cs* i dodaje zestaw plików skryptów oraz odwołania do zestawów, które obsługują sygnalizujący do projektu.

1. Zastąp kod w nowym pliku klasy *ChatHub.cs* tym kodem:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj** > **nowy element**.

1. W obszarze **Dodaj nowy element — SignalRChat** wybierz pozycję **zainstalowane** >  **C# Visual** > **Web** , a następnie wybierz pozycję **Owin klasy startowej**.

1. Nazwij klasę *uruchamiania* i Dodaj ją do projektu.

1. Zastąp domyślny kod w klasie *startowej* tym kodem:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **stronę HTML**.

1. Nazwij nowy *indeks* strony i wybierz **przycisk OK**.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy UTWORZONĄ stronę HTML i wybierz polecenie **Ustaw jako stronę startową**.

1. Zastąp domyślny kod na stronie HTML tym kodem:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. W **Eksplorator rozwiązań**rozwiń węzeł **skrypty**.

    Biblioteki skryptów dla jQuery i sygnalizujących są widoczne w projekcie.

    > [!IMPORTANT]
    > Menedżer pakietów mógł zainstalować nowszą wersję skryptów sygnalizującego.

1. Sprawdź, czy odwołania do skryptu w bloku kodu odpowiadają wersji plików skryptów w projekcie.

    Odwołania do skryptu z oryginalnego bloku kodu:

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Jeśli nie są zgodne, zaktualizuj plik *. html* .

1. Na pasku menu wybierz pozycję **plik** > **Zapisz wszystko**.

## <a name="run-the-sample"></a>Uruchamianie przykładu

1. Na pasku narzędzi Włącz **debugowanie skryptów** , a następnie wybierz przycisk Odtwórz, aby uruchomić przykład w trybie debugowania.

    ![Wprowadź nazwę użytkownika](tutorial-getting-started-with-signalr/_static/image3.png)

1. Po otwarciu przeglądarki wprowadź nazwę swojej tożsamości rozmowy.

1. Skopiuj adres URL z przeglądarki, Otwórz dwie inne przeglądarki i wklej adresy URL do pasków adresów.

1. W każdej przeglądarce wprowadź unikatową nazwę.

1. Teraz Dodaj komentarz i wybierz pozycję **Wyślij**. Powtórz te czynności w innych przeglądarkach. Komentarze pojawiają się w czasie rzeczywistym.

    > [!NOTE]
    > Ta prosta aplikacja czatu nie utrzymuje kontekstu dyskusji na serwerze. Centrum emituje Komentarze do wszystkich bieżących użytkowników. Użytkownicy, którzy dołączą do rozmowy później zobaczą komunikaty dodawane od momentu dołączenia.

    Zobacz, jak działa aplikacja czatu w trzech różnych przeglądarkach. Po wysłaniu komunikatów Anand i Susan wszystkie przeglądarki są aktualizowane w czasie rzeczywistym:

    ![Wszystkie trzy przeglądarki wyświetlają tę samą historię rozmowy](tutorial-getting-started-with-signalr/_static/image4.png)

1. W **Eksplorator rozwiązań**Sprawdź, czy węzeł **dokumenty skryptu** dla uruchomionej aplikacji. Istnieje plik skryptu o nazwie *Hubs* , który jest generowany przez bibliotekę sygnałów w czasie wykonywania. Ten plik zarządza komunikacją między skryptami jQuery i kodem po stronie serwera.

    ![wygenerowany automatycznie skrypt centrów w węźle dokumenty skryptu](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Sprawdzanie kodu

Aplikacja SignalRChat demonstruje dwa podstawowe zadania programistyczne sygnalizujące. Pokazano, jak utworzyć centrum. Serwer używa tego centrum jako głównego obiektu koordynacji. Do wysyłania i odbierania wiadomości centrum jest wykorzystywana Biblioteka sygnałów sygnalizujących.

### <a name="signalr-hubs-in-the-chathubcs"></a>Centra sygnałów w ChatHub.cs

W powyższym przykładzie kodu Klasa `ChatHub` dziedziczy z klasy `Microsoft.AspNet.SignalR.Hub`. Wyprowadzanie z klasy `Hub` jest użytecznym sposobem tworzenia aplikacji sygnalizującej. Metody publiczne można utworzyć w klasie centrów, a następnie użyć tych metod, wywołując je ze skryptów na stronie sieci Web.

W kodzie rozmowy klienci wywołują metodę `ChatHub.Send` w celu wysłania nowej wiadomości. Następnie centrum wysyła komunikat do wszystkich klientów, wywołując `Clients.All.broadcastMessage`.

Metoda `Send` ilustruje kilka pojęć centrum:

* Zadeklaruj metody publiczne w centrum, aby klienci mogli je wywoływać.

* Użyj właściwości dynamicznej `Microsoft.AspNet.SignalR.Hub.Clients`, aby komunikować się ze wszystkimi klientami podłączonymi do tego centrum.

* Wywołaj funkcję na kliencie (na przykład funkcję `broadcastMessage`), aby zaktualizować klientów.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>Sygnalizujący i jQuery w pliku index. html

Na stronie *index. html* w przykładzie kodu pokazano, jak używać biblioteki sygnalizującej jQuery do komunikowania się z centrum sygnałów. Kod wykonuje wiele ważnych zadań. Deklaruje on serwer proxy do odwoływania się do centrum, deklaruje funkcję, którą serwer może wywoływać do wypychania zawartości do klientów i uruchamia połączenie w celu wysyłania komunikatów do centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> W języku JavaScript odwołanie do klasy serwera i jej elementów członkowskich musi być camelCase. Przykładowy kod odwołuje się C# do klasy *ChatHub* w języku JavaScript jako `chatHub`.

W tym bloku kodu utworzysz funkcję wywołania zwrotnego w skrypcie.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Klasa centrum na serwerze wywołuje tę funkcję, aby wypchnąć aktualizacje zawartości do każdego klienta. Te dwa wiersze, w których zawartość HTML jest zakodowana przed wyświetleniem, są opcjonalne i pokazują dobry sposób, aby zapobiec iniekcji skryptu.

Ten kod otwiera połączenie z centrum.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Takie podejście zapewnia, że kod nawiązuje połączenie przed wykonaniem procedury obsługi zdarzeń.

Kod uruchamia połączenie, a następnie przekazuje go do funkcji, aby obsłużyć zdarzenie kliknięcia na przycisku **Wyślij** na stronie HTML.

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji o sygnalizacji, zobacz następujące zasoby:

* [Projekt sygnalizującego](http://signalr.net)

* [Usługi GitHub i przykłady dla sygnałów](https://github.com/SignalR/SignalR)

* [Strona typu wiki sygnalizująca](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Następne kroki

W tym samouczku:

> [!div class="checklist"]
> * Konfigurowanie projektu
> * Uruchomiono przykład
> * Zbadano kod

Przejdź do następnego artykułu, aby dowiedzieć się, jak używać sygnałów i MVC 5.
> [!div class="nextstepaction"]
> [Sygnalizujący 2 i MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)
---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Samouczek: samodzielne hostowanie sygnałów | Microsoft Docs'
author: bradygaster
description: W tym samouczku pokazano, jak utworzyć serwer z własnym sygnałem 2 i jak połączyć się z nim za pomocą klienta JavaScript. Wersje oprogramowania używane w samouczku V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558682"
---
# <a name="tutorial-signalr-self-host"></a>Samouczek: samodzielne hostowanie usługi SignalR

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> W tym samouczku pokazano, jak utworzyć serwer z własnym sygnałem 2 i jak połączyć się z nim za pomocą klienta JavaScript.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Korzystanie z programu Visual Studio 2012 z tym samouczkiem
>
>
> Aby użyć programu Visual Studio 2012 z tym samouczkiem, wykonaj następujące czynności:
>
> - Zaktualizuj [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.
> - Zainstaluj [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - W Instalatorze platformy sieci Web Wyszukaj i zainstaluj **ASP.NET and Web Tools 2013,1 dla programu Visual Studio 2012**. Spowoduje to zainstalowanie szablonów programu Visual Studio dla klas sygnalizujących, takich jak **Hub**.
> - Niektóre szablony (takie jak **Klasa startowa Owin**) nie będą dostępne; w takim przypadku zamiast tego należy użyć pliku klasy.
>
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Omówienie

Serwer sygnalizujący jest zwykle hostowany w aplikacji ASP.NET w usługach IIS, ale może być również własnym hostem (na przykład w aplikacji konsoli lub usługi systemu Windows) przy użyciu biblioteki samoobsługowej. Ta biblioteka, podobnie jak w przypadku wszystkich sygnałów 2, jest oparta na OWIN ([Otwórz interfejs sieci Web dla platformy .NET](http://owin.org)). OWIN definiuje abstrakcję między serwerami sieci Web .NET i aplikacjami sieci Web. OWIN oddziela aplikację sieci Web od serwera, co sprawia, że OWIN jest idealnym rozwiązaniem do samodzielnego udostępniania aplikacji sieci Web we własnym procesie poza programem IIS.

Przyczyny braku hostingu w usługach IIS obejmują:

- Środowiska, w których usługi IIS są niedostępne lub niepożądane, takie jak istniejąca farma serwerów bez usług IIS.
- Należy unikać narzutu na wydajność usług IIS.
- Funkcja sygnalizująca jest dodawana do istniejącej aplikacji działającej w ramach usługi systemu Windows, roli procesu roboczego platformy Azure lub innego procesu.

Jeśli rozwiązanie jest opracowywane jako własne hosty ze względu na wydajność, zaleca się również testowanie aplikacji hostowanej w usługach IIS w celu określenia korzyści z wydajności.

Ten samouczek zawiera następujące sekcje:

- [Tworzenie serwera](#server)
- [Uzyskiwanie dostępu do serwera za pomocą klienta JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Tworzenie serwera

W tym samouczku utworzysz serwer, który jest hostowany w aplikacji konsolowej, ale serwer może być hostowany w dowolnym procesie, takim jak usługa systemu Windows lub rola proces roboczy platformy Azure. Przykładowy kod służący do hostowania serwera sygnalizującego w usłudze systemu Windows znajduje się [w temacie sygnalizujący hosting samoobsługowy w usłudze systemu Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Otwórz Visual Studio 2013 z uprawnieniami administratora. Wybierz **plik**, **Nowy projekt**. Wybierz pozycję **Windows** w **węźle C# Wizualizacja** w okienku **Szablony** , a następnie wybierz szablon **Aplikacja konsolowa** . Nadaj nowemu projektowi nazwę "SignalRSelfHost" i kliknij przycisk **OK**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Otwórz konsolę Menedżera pakietów NuGet, wybierając kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.
3. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    To polecenie dodaje biblioteki samoobsługowe do projektu sygnalizujące 2.
4. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    To polecenie dodaje bibliotekę Microsoft. Owin. CORS do projektu. Ta biblioteka będzie używana do obsługi wielu domen, która jest wymagana w przypadku aplikacji, które obsługują sygnalizację hosta i klienta strony sieci Web w różnych domenach. Ponieważ będziesz hostować serwer sygnalizujący i klienta sieci Web na różnych portach, oznacza to, że między domenami musi być włączona komunikacja między tymi składnikami.
5. Zastąp zawartość Program.cs następującym kodem.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    Powyższy kod zawiera trzy klasy:

    - **Program**, w tym Metoda **Main** definiująca ścieżkę podstawową wykonania. W tej metodzie aplikacja sieci Web typu **Startup** jest uruchamiana z określonym adresem URL (`http://localhost:8080`). Jeśli w punkcie końcowym wymagane są zabezpieczenia, można zaimplementować protokół SSL. Aby uzyskać więcej informacji [, zobacz jak: Konfigurowanie portu z certyfikatem SSL](https://msdn.microsoft.com/library/ms733791.aspx) .
    - **Uruchamianie**, Klasa zawierająca konfigurację dla serwera sygnalizującego (jedyną konfiguracją używaną w tym samouczku to wywołanie `UseCors`) i wywołanie `MapSignalR`, które tworzy trasy dla wszystkich obiektów Hub w projekcie.
    - **MyHub**, Klasa centrum sygnalizująca, którą aplikacja będzie dostarczać klientom. Ta klasa ma jedną metodę, **wysyłając**, którą klienci będą dzwonić w celu rozgłaszania komunikatów do wszystkich innych podłączonych klientów.
6. Skompiluj i uruchom aplikację. Adres, na którym uruchomiony jest serwer powinien być wyświetlany w oknie konsoli.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Jeśli wykonanie nie powiedzie się z wyjątkiem `System.Reflection.TargetInvocationException was unhandled`, konieczne będzie ponowne uruchomienie programu Visual Studio z uprawnieniami administratora.
8. Przed przejściem do następnej sekcji Zatrzymaj aplikację.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Uzyskiwanie dostępu do serwera za pomocą klienta JavaScript

W tej sekcji użyjesz tego samego klienta JavaScript z [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md). Po wprowadzeniu tylko jednej modyfikacji klient będzie mógł jawnie zdefiniować adres URL centrum. W przypadku aplikacji samoobsługowych serwer może nie mieć takiego samego adresu jak adres URL połączenia (z powodu zwrotnych serwerów proxy i modułów równoważenia obciążenia), dlatego należy jawnie zdefiniować adres URL.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj**, **Nowy projekt**. Wybierz węzeł **Sieć Web** , a następnie wybierz szablon **aplikacja sieci Web ASP.NET** . Nadaj projektowi nazwę "JavascriptClient" i kliknij przycisk **OK**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Wybierz **pusty** szablon i pozostaw pozostałe opcje, które nie zostały zaznaczone. Wybierz pozycję **Utwórz projekt**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. W konsoli Menedżera pakietów wybierz projekt "JavascriptClient" z listy rozwijanej **Projekt domyślny** i wykonaj następujące polecenie:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    To polecenie powoduje zainstalowanie bibliotek sygnalizujących i JQuery, które będą potrzebne w kliencie programu.
4. Kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj**, **nowy element**. Wybierz węzeł **Sieć Web** , a następnie wybierz pozycję Strona HTML. Nadaj stronie nazwę **default. html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Zamień zawartość nowej strony HTML na następujący kod. Sprawdź, czy odwołania do skryptów są zgodne ze skryptami w folderze skryptów projektu.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    Poniższy kod (wyróżniony w powyższym przykładzie kodu) jest dodatkiem klienta używanym w samouczku pobieranie z gwiazdką (oprócz uaktualnienia kodu do wersji 2 beta). Ten wiersz kodu jawnie ustawia podstawowy adres URL połączenia dla sygnalizującego na serwerze.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Ustaw projekty startowe..** .. Wybierz przycisk radiowy **wiele projektów startowych** , a następnie ustaw **akcję** **uruchamiania**obu projektów.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Kliknij prawym przyciskiem myszy pozycję "default. html" i wybierz pozycję **Ustaw jako stronę początkową**.
8. Uruchom aplikację. Zostanie uruchomiony serwer i strona. Może być konieczne ponowne załadowanie strony sieci Web (lub wybranie opcji **Kontynuuj** w debugerze), jeśli strona zostanie załadowana przed uruchomieniem serwera.
9. W przeglądarce Podaj nazwę użytkownika po wyświetleniu monitu. Skopiuj adres URL strony na inną kartę lub okno przeglądarki i podaj inną nazwę użytkownika. Będzie można wysyłać wiadomości z jednego okienka przeglądarki do drugiego, jak w samouczku Wprowadzenie.

---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Aplikacje sygnalizujące testy jednostkowe | Microsoft Docs
author: bradygaster
description: W tym artykule opisano sposób korzystania z funkcji testów jednostkowych modułu sygnalizującego 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578674"
---
# <a name="unit-testing-signalr-applications"></a>Testowanie jednostkowe aplikacji SignalR

[Fletcher Patryka](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym artykule opisano korzystanie z funkcji testowania jednostkowego modułu sygnalizującego 2.
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Aplikacje sygnalizujące testy jednostkowe

Aby utworzyć testy jednostkowe dla aplikacji sygnalizującej, można użyć funkcji testów jednostkowych w sygnale 2. Sygnał 2 zawiera interfejs [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , który może służyć do tworzenia obiektu makiety do symulowania metod centrów do testowania.

W tej sekcji dodasz testy jednostkowe dla aplikacji utworzonej w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu [XUnit.NET](https://github.com/xunit/xunit) i [MOQ](https://github.com/Moq/moq4).

XUnit.net zostanie użyta do kontroli testu; MOQ zostanie użyta do utworzenia obiektu [makiety](http://en.wikipedia.org/wiki/Mock_object) do testowania. W razie potrzeby można użyć innych struktur do imitacji; [NSubstitute](http://nsubstitute.github.io/) jest również dobrym wyborem. W tym samouczku pokazano, jak skonfigurować obiekt makiety na dwa sposoby: najpierw przy użyciu obiektu `dynamic` (wprowadzony w .NET Framework 4), a drugi przy użyciu interfejsu.

### <a name="contents"></a>Spis treści

Ten samouczek zawiera następujące sekcje.

- [Testy jednostkowe z dynamiczną](#dynamic)
- [Testy jednostkowe według typu](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Testy jednostkowe z dynamiczną

W tej sekcji dodasz test jednostkowy dla aplikacji utworzonej w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu obiektu dynamicznego.

1. Zainstaluj [rozszerzenie XUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) dla Visual Studio 2013.
2. Ukończ [samouczek wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md)lub Pobierz ukończoną aplikację z [galerii kodu MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Jeśli używasz wersji pobierania Wprowadzenie aplikacji, Otwórz **konsolę Menedżera pakietów** i kliknij przycisk **Przywróć** , aby dodać pakiet sygnalizujący do projektu.

    ![Przywróć pakiety](unit-testing-signalr-applications/_static/image1.png)
4. Dodaj projekt do rozwiązania dla testu jednostkowego. Kliknij prawym przyciskiem myszy rozwiązanie w **Eksplorator rozwiązań** i wybierz polecenie **Dodaj**, **Nowy projekt..** .. W **C#** węźle wybierz węzeł **systemu Windows** . Wybierz **bibliotekę klas**. Nazwij nowy projekt **TestLibrary** i kliknij przycisk **OK**.

    ![Utwórz bibliotekę testową](unit-testing-signalr-applications/_static/image2.png)
5. Dodaj odwołanie w projekcie biblioteki testowej do projektu SignalRChat. Kliknij prawym przyciskiem myszy projekt **TestLibrary** i wybierz polecenie **Dodaj**, **odwołanie...** . Wybierz węzeł **projekty** w węźle **rozwiązanie** i sprawdź **SignalRChat**. Kliknij przycisk **OK**.

    ![Dodaj odwołanie do projektu](unit-testing-signalr-applications/_static/image3.png)
6. Dodaj pakiety sygnalizujące, MOQ i XUnit do projektu **TestLibrary** . W **konsoli Menedżera pakietów**Ustaw listę rozwijaną **domyślny projekt** na **TestLibrary**. Uruchom następujące polecenia w oknie konsoli:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Zainstaluj pakiety](unit-testing-signalr-applications/_static/image4.png)
7. Utwórz plik testowy. Kliknij prawym przyciskiem myszy projekt **TestLibrary** , a następnie kliknij przycisk **Dodaj...** , **Klasa**. Nadaj nowej klasie nazwę **tests.cs**.
8. Zastąp zawartość Tests.cs następującym kodem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    W powyższym kodzie klient testowy jest tworzony przy użyciu obiektu `Mock` z biblioteki [MOQ](https://github.com/Moq/moq4) , typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w sygnalizacji 2,1, przypisz `dynamic` parametru typu). Interfejs `IHubCallerConnectionContext` jest obiektem proxy, za pomocą którego są wywoływane metody na kliencie. Funkcja `broadcastMessage` jest następnie definiowana dla klienta makiety, dzięki czemu może być wywoływana przez klasę `ChatHub`. Aparat testowy następnie wywołuje metodę `Send` klasy `ChatHub`, która z kolei wywołuje funkcję `broadcastMessage` zamakietowania.
9. Skompiluj rozwiązanie, naciskając klawisz **F6**.
10. Uruchom test jednostkowy. W programie Visual Studio wybierz kolejno opcje **test**, **Windows**i **Eksplorator testów**. W oknie Eksplorator testów kliknij prawym przyciskiem myszy pozycję **HubsAreMockableViaDynamic** i wybierz polecenie **Uruchom wybrane testy**.

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image5.png)
11. Sprawdź, czy test zakończono testy, sprawdzając dolne okienko w oknie Eksplorator testów. Zostanie wyświetlone okno pokazujące, że test zakończono.

    ![Test zakończony niepowodzenie](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Testy jednostkowe według typu

W tej sekcji dodasz test dla aplikacji utworzonej w [samouczku wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu interfejsu, który zawiera metodę do przetestowania.

1. Wykonaj kroki 1-7 w ramach [testów jednostkowych za pomocą](#dynamic) powyższego samouczka dynamicznego.
2. Zastąp zawartość Tests.cs następującym kodem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    W powyższym kodzie tworzony jest interfejs definiujący sygnaturę metody `broadcastMessage`, dla której aparat testowy utworzy klienta programu. Klient, który jest następnie tworzony przy użyciu obiektu `Mock`, typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w sygnalizującer 2,1, przypisz `dynamic` parametru typu). Interfejs `IHubCallerConnectionContext` jest obiektem proxy, za pomocą którego są wywoływane metody na kliencie.

    Następnie test tworzy wystąpienie `ChatHub`, a następnie tworzy wersję makiety metody `broadcastMessage`, która z kolei jest wywoływana przez wywołanie metody `Send` w centrum.
3. Skompiluj rozwiązanie, naciskając klawisz **F6**.
4. Uruchom test jednostkowy. W programie Visual Studio wybierz kolejno opcje **test**, **Windows**i **Eksplorator testów**. W oknie Eksplorator testów kliknij prawym przyciskiem myszy pozycję **HubsAreMockableViaDynamic** i wybierz polecenie **Uruchom wybrane testy**.

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image7.png)
5. Sprawdź, czy test zakończono testy, sprawdzając dolne okienko w oknie Eksplorator testów. Zostanie wyświetlone okno pokazujące, że test zakończono.

    ![Test zakończony niepowodzenie](unit-testing-signalr-applications/_static/image8.png)

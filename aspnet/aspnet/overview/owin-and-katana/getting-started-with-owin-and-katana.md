---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Wprowadzenie z OWIN i Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584673"
---
# <a name="getting-started-with-owin-and-katana"></a>Wprowadzenie do korzystania z interfejsu OWIN i projektu Katana

według [Jan Wasson](https://github.com/MikeWasson)

[Otwórz interfejs sieci Web dla platformy .NET (Owin)](http://owin.org/) definiuje abstrakcję między serwerami sieci Web i aplikacjami sieci Web programu .NET. Łącząc serwer sieci Web z aplikacji, OWIN ułatwia tworzenie oprogramowania pośredniczącego na potrzeby programowania w sieci Web dla platformy .NET. Ponadto OWIN ułatwiają przenoszenie aplikacji sieci Web na inne hosty&#8212;na przykład samodzielnego udostępniania w usłudze systemu Windows lub w innym procesie.

OWIN to specyfikacja będąca własnością Wspólnoty, a nie implementacja. Projekt Katana to zestaw składników OWIN typu open source opracowanych przez firmę Microsoft. Ogólne omówienie programów OWIN i Katana można znaleźć w temacie [Omówienie projektu Katana](an-overview-of-project-katana.md). W tym artykule przeskoczę do kodu, aby rozpocząć pracę.

W tym samouczku jest używana [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale można również użyć programu Visual Studio 2012. Niektóre z tych kroków różnią się w programie Visual Studio 2012, który należy zauważyć poniżej.

## <a name="host-owin-in-iis"></a>OWIN hosta w usługach IIS

W tej sekcji będziemy hostować OWIN w usługach IIS. Ta opcja zapewnia elastyczność i możliwość tworzenia potoku OWIN wraz z zestawem dojrzałych funkcji usług IIS. Przy użyciu tej opcji aplikacja OWIN jest uruchamiana w potoku żądania ASP.NET.

Najpierw utwórz nowy projekt aplikacji sieci Web ASP.NET. (W programie Visual Studio 2012 użyj pustego typu projektu aplikacji sieci Web ASP.NET).

![](getting-started-with-owin-and-katana/_static/image1.png)

W oknie dialogowym **Nowy projekt ASP.NET** wybierz **pusty** szablon.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Dodawanie pakietów NuGet

Następnie Dodaj wymagane pakiety NuGet. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Dodaj klasę uruchomieniową

Następnie Dodaj klasę uruchomieniową OWIN. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt, a następnie wybierz pozycję **Dodaj**, a następnie wybierz pozycję **nowy element**. W oknie dialogowym **Dodaj nowy element** wybierz pozycję **Klasa startowa Owin**. Aby uzyskać więcej informacji na temat konfigurowania klasy startowej, zobacz [Owin Starting Class Detection](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Dodaj następujący kod do metody `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Ten kod dodaje proste oprogramowanie pośredniczące do potoku OWIN, zaimplementowane jako funkcja, która odbiera wystąpienie **Microsoft. Owin. IOwinContext** . Gdy serwer odbiera żądanie HTTP, potok OWIN wywołuje oprogramowanie pośredniczące. Oprogramowanie pośredniczące ustawia typ zawartości odpowiedzi i zapisuje treść odpowiedzi.

> [!NOTE]
> Szablon klasy uruchamiania OWIN jest dostępny w Visual Studio 2013. Jeśli używasz programu Visual Studio 2012, wystarczy dodać nową pustą klasę o nazwie `Startup1`i wkleić ją w poniższym kodzie:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Uruchom aplikację

Naciśnij klawisz F5, aby rozpocząć debugowanie. Program Visual Studio otworzy okno przeglądarki, aby `http://localhost:*port*/`. Strona powinna wyglądać następująco:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Samoobsługowe OWIN w aplikacji konsolowej

Można łatwo przekonwertować tę aplikację z hostingu usług IIS na własne hosty w procesie niestandardowym. W przypadku hostingu usług IIS program IIS działa zarówno jako serwer HTTP, jak i proces obsługujący usługę. W przypadku samoobsługi aplikacja tworzy proces i używa klasy **odbiornika HttpListener** jako serwera http.

W programie Visual Studio Utwórz nową aplikację konsolową. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Dodaj do projektu klasę `Startup1` z części 1 tego samouczka. Nie trzeba modyfikować tej klasy.

Zaimplementuj metodę `Main` aplikacji w następujący sposób.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Po uruchomieniu aplikacji konsolowej serwer uruchamia nasłuchiwanie `http://localhost:9000`. Jeśli przejdziesz do tego adresu w przeglądarce internetowej, zostanie wyświetlona strona "Hello World".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Dodaj diagnostykę OWIN

Pakiet Microsoft. Owin. Diagnostics zawiera oprogramowanie pośredniczące, które przechwytuje Nieobsłużone wyjątki i wyświetla stronę HTML z informacjami o błędzie. Ta strona działa podobnie jak strona błędu ASP.NET, która jest czasami nazywana "[żółtym ekranem zgonu](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Podobnie jak w przypadku YSOD, strona błędu Katana jest przydatna podczas opracowywania, ale dobrym rozwiązaniem jest wyłączenie jej w trybie produkcyjnym.

Aby zainstalować pakiet diagnostyczny w projekcie, wpisz następujące polecenie w oknie Konsola Menedżera pakietów:

`install-package Microsoft.Owin.Diagnostics –Pre`

Zmień kod w metodzie `Startup1.Configuration` w następujący sposób:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Teraz użyj kombinacji klawiszy CTRL + F5, aby uruchomić aplikację bez debugowania, aby program Visual Studio nie przerywał tego wyjątku. Aplikacja zachowuje się tak samo jak wcześniej, dopóki nie przejdziesz do `http://localhost/fail`, w którym aplikacja zgłosi wyjątek. Strona błędu oprogramowanie pośredniczące przechwytuje wyjątek i wyświetla stronę HTML z informacjami o błędzie. Możesz kliknąć karty, aby zobaczyć zmienne środowiskowe, ciąg zapytania, pliki cookie, nagłówek żądania i OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Następne kroki

- [Wykrywanie klasy początkowej OWIN](owin-startup-class-detection.md)
- [Korzystanie z OWIN do samoobsługowego hostowania interfejsu API sieci Web ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Korzystanie z OWIN do obsługi sygnałów z dowolnego hosta](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)

---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Obsługa błędów ASP.NET | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: f420be369801208fa875d9a60e6e154afbe84aa7
ms.sourcegitcommit: b67ffd5b2c5cff01ec4c8eb12a21f693f2e11887
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995308"
---
# <a name="aspnet-error-handling"></a>Obsługa błędów platformy ASP.NET

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawkiC#()](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku zmodyfikujesz przykładową aplikację Wingtip zabawki w celu uwzględnienia obsługi błędów i rejestrowania błędów. Obsługa błędów umożliwi aplikacji bezpieczne obsłudze błędów i wyświetlenie komunikatów o błędach. Rejestrowanie błędów pozwoli znaleźć i naprawić błędy, które wystąpiły. Ten samouczek jest oparty na poprzednim samouczku "Routing adresów URL" i jest częścią serii samouczków Wingtip.

## <a name="what-youll-learn"></a>Dowiesz się:

- Jak dodać globalną obsługę błędów do konfiguracji aplikacji.
- Jak dodać obsługę błędów na poziomie aplikacji, strony i kodu.
- Jak rejestrować błędy w celu późniejszego przeglądu.
- Jak wyświetlić komunikaty o błędach, które nie naruszają zabezpieczeń.
- Jak zaimplementować rejestrowanie błędów modułów i programów obsługi (ELMAH) rejestrowania błędów.

## <a name="overview"></a>Omówienie

Aplikacje ASP.NET muszą być w stanie obsługiwać błędy występujące podczas wykonywania w spójny sposób. ASP.NET używa środowiska uruchomieniowego języka wspólnego (CLR), które zapewnia sposób powiadamiania aplikacji o błędach w jednolity sposób. Wyjątek jest zgłaszany w przypadku wystąpienia błędu. Wyjątek jest dowolnym błędem, warunkiem lub nieoczekiwanym zachowaniem napotykanym przez aplikację.

W .NET Framework wyjątek jest obiektem, który dziedziczy z `System.Exception` klasy. Wyjątek jest generowany z obszaru kodu, w którym wystąpił problem. Wyjątek jest przekazywać stos wywołań do miejsca, w którym aplikacja udostępnia kod do obsługi wyjątku. Jeśli aplikacja nie obsługuje wyjątku, w przeglądarce zostanie wymuszone wyświetlenie szczegółów błędu.

Najlepszym rozwiązaniem jest obsługa błędów na poziomie `Try` kodu w / `Catch` / `Finally` blokach w kodzie. Spróbuj umieścić te bloki, aby użytkownik mógł rozwiązać problemy w kontekście, w którym występują. Jeśli bloki obsługi błędów są zbyt daleko od miejsca wystąpienia błędu, utrudniają użytkownikom informacje potrzebne do rozwiązania problemu.

### <a name="exception-class"></a>Klasa wyjątku

Klasa wyjątku jest klasą bazową, z której są dziedziczone wyjątki. Większość obiektów wyjątków to wystąpienia niektórych klas pochodnych klasy wyjątku, takich jak `SystemException` Klasa `IndexOutOfRangeException` , Klasa lub `ArgumentNullException` Klasa. Klasa wyjątku ma właściwości, takie jak `StackTrace` Właściwość `InnerException` , właściwość i `Message` właściwość, które zawierają określone informacje o błędzie, który wystąpił.

### <a name="exception-inheritance-hierarchy"></a>Hierarchia dziedziczenia wyjątków

Środowisko uruchomieniowe ma podstawowy zestaw wyjątków wynikający z `SystemException` klasy, którą środowisko uruchomieniowe zgłasza po napotkaniu wyjątku. Większość klas, które dziedziczą z klasy wyjątku, takich jak `IndexOutOfRangeException` Klasa `ArgumentNullException` i Klasa, nie implementuje dodatkowych elementów członkowskich. W związku z tym najważniejsze informacje dotyczące wyjątku można znaleźć w hierarchii wyjątków, nazwa wyjątku i informacje zawarte w wyjątku.

### <a name="exception-handling-hierarchy"></a>Hierarchia obsługi wyjątków

W aplikacji formularzy sieci Web ASP.NET wyjątki mogą być obsługiwane w oparciu o określoną hierarchię obsługi. Wyjątek można obsłużyć na następujących poziomach:

- Poziom aplikacji
- Poziom strony
- Poziom kodu

Gdy aplikacja obsługuje wyjątki, dodatkowe informacje o wyjątku dziedziczone z klasy wyjątku mogą być często pobierane i wyświetlane dla użytkownika. Oprócz aplikacji, strony i poziomu kodu, można również obsługiwać wyjątki na poziomie modułu HTTP i przy użyciu niestandardowej obsługi programu IIS.

### <a name="application-level-error-handling"></a>Obsługa błędów na poziomie aplikacji

Błędy domyślne można obsłużyć na poziomie aplikacji, modyfikując konfigurację aplikacji lub dodając `Application_Error` procedurę obsługi w pliku *Global. asax* aplikacji.

Można obsługiwać błędy domyślne i błędy HTTP przez dodanie `customErrors` sekcji do pliku *Web. config* . `customErrors` Sekcja pozwala określić domyślną stronę, do której użytkownicy będą przekierowywani po wystąpieniu błędu. Umożliwia również określanie poszczególnych stron dla określonych błędów kodu stanu.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Niestety, w przypadku użycia konfiguracji w celu przekierowania użytkownika na inną stronę nie ma szczegółów dotyczących błędu, który wystąpił.

Można jednak zastosować pułapki błędów, które występują w dowolnym miejscu w aplikacji, dodając `Application_Error` kod do programu obsługi w pliku *Global. asax* .

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Obsługa zdarzeń błędów na poziomie strony

Procedura obsługi na poziomie strony zwraca użytkownika do strony, na której wystąpił błąd, ale ponieważ wystąpienia formantów nie są utrzymywane, nie będą już niczego na stronie. Aby podać szczegóły błędu użytkownikowi aplikacji, należy zapisać szczegóły błędu na stronie.

Zwykle do rejestrowania nieobsłużonych błędów lub przełączenia użytkownika do strony, która może wyświetlać przydatne informacje, jest używana procedura obsługi błędów na poziomie strony.

Ten przykład kodu pokazuje procedurę obsługi dla zdarzenia błędu na stronie sieci Web ASP.NET. Ta procedura obsługi przechwytuje wszystkie wyjątki, które nie zostały `try` jeszcze obsłużone / `catch` w blokach na stronie.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Po obsłudze błędu należy wyczyścić go przez wywołanie `ClearError` metody obiektu serwera (`HttpServerUtility` klasy), w przeciwnym razie zostanie wyświetlony błąd, który wcześniej wystąpił.

### <a name="code-level-error-handling"></a>Obsługa błędów na poziomie kodu

Instrukcja try-catch składa się z bloku try, po którym następuje co najmniej jedna klauzula catch, która określa programy obsługi dla różnych wyjątków. Gdy wyjątek jest zgłaszany, środowisko uruchomieniowe języka wspólnego (CLR) szuka instrukcji catch, która obsługuje ten wyjątek. Jeśli aktualnie wykonywana metoda nie zawiera bloku catch, środowisko CLR przegląda metodę, która wywołała bieżącą metodę i tak dalej, stos wywołań. Jeśli nie zostanie znaleziony żaden blok catch, środowisko CLR wyświetli komunikat o nieobsługiwanym wyjątku dla użytkownika i zakończy wykonywanie programu.

Poniższy przykład kodu przedstawia typowy `try` sposób użycia / `catch` / programudoobsługibłędów`finally` .

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

W powyższym kodzie blok try zawiera kod, który należy chronić przed możliwym wyjątkiem. Blok jest wykonywany do momentu wyrzucania wyjątku lub zakończono pomyślne zakończenie bloku. Jeśli wystąpi `IOException` wyjątek lub wyjątek, wykonanie jest przenoszone na inną stronę. `FileNotFoundException` Następnie kod zawarty w bloku finally jest wykonywany, niezależnie od tego, czy wystąpił błąd, czy nie.

## <a name="adding-error-logging-support"></a>Dodawanie obsługi rejestrowania błędów

Przed dodaniem obsługi błędów do przykładowej aplikacji Wingtip zabawki należy dodać obsługę rejestrowania błędów przez dodanie `ExceptionUtility` klasy do folderu *logiki* . Dzięki temu, za każdym razem, gdy aplikacja obsłuży błąd, szczegóły błędu zostaną dodane do pliku dziennika błędów.

1. Kliknij prawym przyciskiem myszy folder *logiki* , a następnie wybierz polecenie **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz grupę szablony  - **kodu** **wizualizacji C#**  &gt; po lewej stronie. Następnie wybierz pozycję **Klasa**z listy Środkowej i nadaj jej nazwę **ExceptionUtility.cs**.
3. Wybierz **Dodaj**. Zostanie wyświetlony nowy plik klasy.
4. Zastąp istniejący kod następującym:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Gdy wystąpi wyjątek, wyjątek może zostać zapisany w pliku dziennika wyjątków przez wywołanie `LogException` metody. Ta metoda przyjmuje dwa parametry, obiekt wyjątku i ciąg zawierający szczegóły dotyczące źródła wyjątku. Dziennik wyjątków jest zapisywana w pliku dziennika *błędów* w folderze *dane aplikacji\_* .

### <a name="adding-an-error-page"></a>Dodawanie strony błędu

W przykładowej aplikacji Wingtip zabawki zostanie użyta jedna strona do wyświetlania błędów. Strona błędu została zaprojektowana w celu wyświetlenia bezpiecznego komunikatu o błędzie dla użytkowników witryny. Jeśli jednak użytkownik jest deweloperem wysyłającym żądanie HTTP, które jest obsługiwane lokalnie na komputerze, na którym znajduje się kod, dodatkowe szczegóły błędu zostaną wyświetlone na stronie błędu.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**Wingtip zabawki**) **w Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj**  - &gt; **nowy element**.   
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Wybierz grupę **Visual C#**   -Webtemplates po lewej stronie. &gt; Z listy środkowej wybierz pozycję **formularz sieci Web ze stroną wzorcową**i nadaj jej nazwę **ErrorPage. aspx**.
3. Kliknij przycisk **Dodaj**.
4. Wybierz plik *site. Master* jako stronę wzorcową, a następnie wybierz przycisk **OK**.
5. Zastąp istniejący znacznik następującym:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Zastąp istniejący kod związany z kodem (*ErrorPage.aspx.cs*), tak aby pojawił się w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Gdy zostanie wyświetlona strona błędu, `Page_Load` program obsługi zdarzeń jest wykonywany. W programie `Page_Load` obsługi jest określana lokalizacja, w której został po raz pierwszy obsłużony komunikat o błędzie. Następnie ostatni błąd, który wystąpił, jest określany przez `GetLastError` wywołanie metody obiektu serwera. Jeśli ten wyjątek już nie istnieje, zostanie utworzony wyjątek generyczny. Następnie, jeśli żądanie HTTP zostało wykonane lokalnie, wyświetlane są wszystkie szczegóły błędu. W takim przypadku tylko komputer lokalny, na którym działa aplikacja sieci Web, będzie widział te szczegóły błędu. Po wyświetleniu informacji o błędzie zostanie dodany błąd do pliku dziennika, a na serwerze zostanie wyczyszczony błąd.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Wyświetlanie nieobsłużonych komunikatów o błędach dla aplikacji

Dodanie `customErrors` sekcji do pliku *Web. config* pozwala szybko obsługiwać proste błędy występujące w całej aplikacji. Możesz również określić sposób obsługi błędów na podstawie ich wartości kodu stanu, takich jak 404 — nie znaleziono pliku.

#### <a name="update-the-configuration"></a>Aktualizowanie konfiguracji

Zaktualizuj konfigurację, dodając `customErrors` sekcję do pliku *Web. config* .

1. W **Eksplorator rozwiązań**Znajdź i Otwórz plik *Web. config* w katalogu głównym przykładowej aplikacji programu Wingtip zabawki.
2. Dodaj sekcję do pliku *Web. config* w `<system.web>` węźle w następujący sposób: `customErrors`   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Zapisz plik *Web. config* .

`customErrors` Sekcja określa tryb, który jest ustawiony na wartość "on". Określa `defaultRedirect`również, który wskazuje aplikacji, do której ma przechodzić po wystąpieniu błędu. Ponadto został dodany konkretny element błędu, który określa, jak obsłużyć błąd 404, gdy strona nie zostanie znaleziona. W dalszej części tego samouczka dodasz dodatkową obsługę błędów, która spowoduje przechwycenie szczegółów błędu na poziomie aplikacji.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić aplikację, aby zobaczyć zaktualizowane trasy.

1. Naciśnij klawisz **F5** , aby uruchomić przykładową aplikację Wingtip zabawki.  
 Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .
2. Wprowadź następujący adres URL w przeglądarce (Upewnij się, że używasz numeru portu):  
    `https://localhost:44300/NoPage.aspx`
3. Przejrzyj *ErrorPage. aspx* , który jest wyświetlany w przeglądarce. 

    ![Obsługa błędów ASP.NET-błąd podczas znajdowania strony](aspnet-error-handling/_static/image1.png)

Po zażądaniu strony *nopage. aspx* , która nie istnieje, na stronie błędu zostanie wyświetlony prosty komunikat o błędzie i szczegółowe informacje o błędzie, jeśli są dostępne dodatkowe szczegóły. Jeśli jednak użytkownik zażądał nieistniejącej strony z lokalizacji zdalnej, na stronie błędu zostanie wyświetlony tylko komunikat o błędzie w kolorze czerwonym.

### <a name="including-an-exception-for-testing-purposes"></a>Dołączenie wyjątku do celów testowych

Aby sprawdzić, w jaki sposób aplikacja będzie działać po wystąpieniu błędu, można celowo utworzyć warunki błędów w ASP.NET. W przykładowej aplikacji Wingtip zabawki zostanie zgłoszony wyjątek testu, gdy domyślna strona zostanie załadowana, aby zobaczyć, co się dzieje.

1. Otwórz kod znajdujący się na stronie *default. aspx* w programie Visual Studio.   
   Zostanie wyświetlona strona z kodem *default.aspx.cs* .
2. W programie `Page_Load` obsługi Dodaj kod, aby program obsługi pojawił się w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Istnieje możliwość tworzenia różnych typów wyjątków. W powyższym kodzie tworzysz, `InvalidOperationException` Kiedy zostanie załadowana strona *default. aspx* .

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację, aby zobaczyć, jak aplikacja obsługuje wyjątek.

1. Naciśnij **klawisze CTRL + F5** , aby uruchomić przykładową aplikację Wingtip zabawki.  
 Aplikacja zgłasza InvalidOperationException. 

    > [!NOTE] 
    > 
    > Naciśnij **klawisze CTRL + F5** , aby wyświetlić stronę bez dzielenia na kod, aby wyświetlić Źródło błędu w programie Visual Studio.
2. Przejrzyj *ErrorPage. aspx* , który jest wyświetlany w przeglądarce. 

    ![Obsługa błędów ASP.NET — strona błędu](aspnet-error-handling/_static/image2.png)

Jak widać w szczegółach błędu, w pliku `customError` *Web. config* został Zalewka niezależna od wyjątku.

### <a name="adding-application-level-error-handling"></a>Dodawanie obsługi błędów na poziomie aplikacji

Zamiast Zalewka wyjątku przy użyciu `customErrors` sekcji w pliku *Web. config* , w której można uzyskać niewiele informacji o wyjątku, można Zalewka błędu na poziomie aplikacji i pobrać szczegóły błędu.

1. W **Eksplorator rozwiązań**Znajdź i otwórz plik *Global.asax.cs* .
2. Dodaj program **obsługi\_błędów aplikacji** , tak aby pojawił się w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Gdy w aplikacji wystąpi błąd, `Application_Error` program obsługi jest wywoływany. W ramach tej procedury obsługi zostanie pobrany i sprawdzony ostatni wyjątek. Jeśli wyjątek nie został obsłużony, a wyjątek zawiera szczegóły wyjątku wewnętrznego ( `InnerException` czyli nie ma wartości null), aplikacja przenosi wykonywanie do strony błędu, w której są wyświetlane szczegóły wyjątku.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Możesz uruchomić aplikację, aby wyświetlić dodatkowe szczegóły błędu dostarczone przez obsługę wyjątku na poziomie aplikacji.

1. Naciśnij **klawisze CTRL + F5** , aby uruchomić przykładową aplikację Wingtip zabawki.  
 Aplikacja zgłasza `InvalidOperationException` .
2. Przejrzyj *ErrorPage. aspx* , który jest wyświetlany w przeglądarce. 

    ![Obsługa błędów ASP.NET — błąd poziomu aplikacji](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Dodawanie obsługi błędów na poziomie strony

Obsługę błędów na poziomie strony można dodać do strony przy użyciu dodawania `ErrorPage` atrybutu `@Page` do dyrektywy strony lub przez dodanie `Page_Error` procedury obsługi zdarzeń do kodu powiązanego ze stroną. W tej sekcji `Page_Error` dodasz procedurę obsługi zdarzeń, która spowoduje przeniesienie wykonania na stronę *ErrorPage. aspx* .

1. W **Eksplorator rozwiązań**Znajdź i otwórz plik *default.aspx.cs* .
2. `Page_Error` Dodaj program obsługi, aby kod został wyświetlony w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Gdy na stronie wystąpi błąd, `Page_Error` zostaje wywołana procedura obsługi zdarzeń. W ramach tej procedury obsługi zostanie pobrany i sprawdzony ostatni wyjątek. `InvalidOperationException` W`Page_Error` przypadku wystąpienia procedury obsługi zdarzeń przenosi wykonanie na stronę błędu, na której są wyświetlane szczegóły wyjątku.

#### <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić aplikację, aby zobaczyć zaktualizowane trasy.

1. Naciśnij **klawisze CTRL + F5** , aby uruchomić przykładową aplikację Wingtip zabawki.  
 Aplikacja zgłasza `InvalidOperationException` .
2. Przejrzyj *ErrorPage. aspx* , który jest wyświetlany w przeglądarce. 

    ![Obsługa błędów ASP.NET — błąd poziomu strony](aspnet-error-handling/_static/image4.png)
3. Zamknij okno przeglądarki.

### <a name="removing-the-exception-used-for-testing"></a>Usuwanie wyjątku używanego do testowania

Aby umożliwić działanie przykładowej aplikacji Wingtip zabawki bez zgłaszania wyjątku, który został dodany wcześniej w tym samouczku, Usuń wyjątek.

1. Otwórz kod znajdujący się na stronie *default. aspx* .
2. W programie `Page_Load` obsługi Usuń kod, który zgłasza wyjątek, aby program obsługi pojawił się w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Dodawanie rejestrowania błędów na poziomie kodu

Jak wspomniano wcześniej w tym samouczku, możesz dodać instrukcje try/catch, aby spróbować uruchomić sekcję kodu i obsłużyć pierwszy błąd, który wystąpił. W tym przykładzie nastąpi zapisanie szczegółów błędu w pliku dziennika błędów, aby można było później przejrzeć ten błąd.

1. W **Eksplorator rozwiązań**w folderze *logiki* znajdź i Otwórz plik *PayPalFunctions.cs* .
2. `HttpCall` Zaktualizuj metodę, aby kod pojawił się w następujący sposób:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Powyższy kod wywołuje `LogException` metodę, która jest zawarta `ExceptionUtility` w klasie. Plik klasy *ExceptionUtility.cs* został dodany do folderu *logiki* wcześniej w tym samouczku. `LogException` Metoda przyjmuje dwa parametry. Pierwszy parametr jest obiektem wyjątku. Drugi parametr jest ciągiem używanym do rozpoznawania źródła błędu.

### <a name="inspecting-the-error-logging-information"></a>Inspekcja informacji rejestrowania błędów

Jak wspomniano wcześniej, można użyć dziennika błędów, aby określić, które błędy w aplikacji powinny zostać ustalone jako pierwsze. Oczywiście tylko błędy, które zostały zalewki i zapisane w dzienniku błędów, zostaną zarejestrowane.

1. W **Eksplorator rozwiązań**Znajdź i Otwórz plik *dziennika błędów. txt* w folderze *dane aplikacji\_* .   
 Może być konieczne wybranie opcji "**Pokaż wszystkie pliki**" lub opcji "**Odśwież**" w górnej części **Eksplorator rozwiązań** , aby wyświetlić plik *dziennika błędów* .
2. Przejrzyj dziennik błędów wyświetlany w programie Visual Studio: 

    ![Obsługa błędów ASP.NET-dziennik błędów. txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Bezpieczne komunikaty o błędach

Należy **pamiętać** , że gdy aplikacja wyświetla komunikaty o błędach, nie powinno zawierać informacji o tym, że złośliwy użytkownik może pomóc w ataku aplikacji. Jeśli na przykład aplikacja nie powiodła się, próbuje dokonać zapisu w bazie danych, nie powinna być wyświetlana komunikat o błędzie zawierający nazwę użytkownika, którego używa. Z tego powodu w kolorze czerwonym zostanie wyświetlony ogólny komunikat o błędzie. Wszystkie dodatkowe szczegóły błędu są wyświetlane tylko dla deweloperów na komputerze lokalnym.

## <a name="using-elmah"></a>Korzystanie z programu ELMAH

ELMAH (moduły rejestrowania błędów modułów i programów obsługi) to funkcja rejestrowania błędów, którą można podłączyć do aplikacji ASP.NET jako pakiet NuGet. Program ELMAH zapewnia następujące możliwości:

- Rejestrowanie nieobsłużonych wyjątków.
- Strona sieci Web, aby wyświetlić cały dziennik odkodowych nieobsłużonych wyjątków.
- Strona sieci Web, aby wyświetlić wszystkie szczegółowe informacje o każdym zarejestrowanym wyjątku.
- Powiadomienie e-mail o każdym błędzie w momencie wystąpienia problemu.
- Źródło danych RSS z ostatnich 15 błędów z dziennika.

Przed rozpoczęciem pracy z programem ELMAH należy go zainstalować. Jest to łatwe w użyciu Instalator pakietu *NuGet* . Jak wspomniano wcześniej w tej serii samouczków, pakiet NuGet jest rozszerzeniem programu Visual Studio, które ułatwia Instalowanie i aktualizowanie bibliotek i narzędzi typu open source w programie Visual Studio.

1. W programie Visual Studio w menu **Narzędzia** wybierz pozycję >  **Menedżer pakietów NuGet** **Zarządzaj pakietami NuGet dla rozwiązania**. 

    ![Obsługa błędów ASP.NET — zarządzanie pakietami NuGet dla rozwiązania](aspnet-error-handling/_static/image6.png)
2. Okno dialogowe **Zarządzanie pakietami NuGet** jest wyświetlane w programie Visual Studio.
3. W oknie dialogowym **Zarządzanie pakietami NuGet** rozwiń pozycję **online** po lewej stronie, a następnie wybierz pozycję **NuGet.org**. Następnie Znajdź i zainstaluj pakiet **ELMAH** z listy dostępnych pakietów online. 

    ![Obsługa błędów ASP.NET — pakiet NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. Musisz mieć połączenie z Internetem, aby pobrać pakiet.
5. W oknie dialogowym **Wybierz projekty** upewnij się, że zaznaczenie opcji **WingtipToys** jest zaznaczone, a następnie kliknij przycisk **OK**. 

    ![Obsługa błędów ASP.NET — okno dialogowe Wybieranie projektów](aspnet-error-handling/_static/image8.png)
6. W razie konieczności kliknij przycisk **Zamknij** w oknie dialogowym **Zarządzanie pakietami NuGet** .
7. Jeśli program Visual Studio żąda ponownego załadowania otwartych plików, wybierz opcję "**tak na wszystkie**".
8. Pakiet ELMAH dodaje wpisy dla siebie w pliku *Web. config* w katalogu głównym projektu. Jeśli program Visual Studio wyświetli monit z pytaniem, czy chcesz ponownie załadować zmodyfikowany plik *Web. config* , kliknij przycisk **tak**.

ELMAH jest teraz gotowy do przechowywania wszelkich nieobsłużonych błędów.

### <a name="viewing-the-elmah-log"></a>Wyświetlanie dziennika ELMAH

Wyświetlanie dziennika ELMAH jest proste, ale najpierw należy utworzyć nieobsłużony wyjątek, który będzie rejestrowany w dzienniku ELMAH.

1. Naciśnij **klawisze CTRL + F5** , aby uruchomić przykładową aplikację Wingtip zabawki.
2. Aby napisać nieobsługiwany wyjątek do dziennika ELMAH, przejdź do przeglądarki pod następującym adresem URL (przy użyciu numeru portu):  
    `https://localhost:44300/NoPage.aspx`Zostanie wyświetlona strona błędu.
3. Aby wyświetlić dziennik ELMAH, przejdź do przeglądarki pod następującym adresem URL (przy użyciu numeru portu):  
    `https://localhost:44300/elmah.axd`

    ![Obsługa błędów ASP.NET — dziennik błędów ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku zawarto informacje na temat obsługi błędów na poziomie aplikacji, na poziomie strony i na poziomie kodu. Przedstawiono również sposób rejestrowania obsłużonych i nieobsłużonych błędów na potrzeby późniejszego przeglądu. Dodano narzędzie ELMAH, aby zapewnić rejestrowanie wyjątków i powiadomienia do aplikacji przy użyciu programu NuGet. Ponadto zawarto informacje o ważności bezpiecznych komunikatów o błędach.

## <a name="tutorial-series-conclusion"></a>Wniosek z serii samouczków

Dziękujemy za następujące kwestie. Mam nadzieję, że ten zestaw samouczków pomaga lepiej poznać korzystanie z formularzy sieci Web ASP.NET. Aby uzyskać więcej informacji na temat funkcji formularzy sieci Web dostępnych w ASP.NET 4,5 i Visual Studio 2013, zobacz [ASP.NET and Web Tools informacji Visual Studio 2013 o wersji](../../../../visual-studio/overview/2013/release-notes.md). Pamiętaj również, aby zapoznać się z samouczkiem wymienionym w sekcji **następne kroki** i defintely Wypróbuj [bezpłatną wersję próbną platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

![Dziękujemy-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o wdrażaniu aplikacji sieci Web w celu Microsoft Azure, zobacz [wdrażanie bezpiecznej aplikacji Web Forms ASP.NET z członkostwem, uwierzytelnianiem OAuth i SQL Database do witryny sieci Web systemu Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Bezpłatna wersja próbna

[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)  
 Opublikowanie witryny sieci Web w Microsoft Azure spowoduje zaoszczędzenie czasu, konserwacji i wydatków. Jest to szybki proces wdrażania aplikacji sieci Web na platformie Azure. Jeśli musisz zachować i monitorować aplikację sieci Web, platforma Azure oferuje wiele narzędzi i usług. Zarządzanie danymi, ruchem, tożsamościami, kopiami zapasowymi, przesyłaniem danych, multimediami i wydajnością na platformie Azure. Wszystkie te dane są dostępne w bardzo opłacalny sposób.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Rejestrowanie szczegółów błędu za pomocą monitorowania kondycji ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Potwierdzanie

Chcę podziękowanie następujące osoby, które złożyły znaczące wkłady w zawartość tej serii samouczków:

- [Alberto Poblacion, MVP &amp; MCT, Hiszpania](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Holandia](http://blog.alexthissen.nl/) (Twitter: [@alexthissen](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brazylia](http://msmvps.com/blogs/bsonnino) (Twitter: [@bsonnino](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brazylia](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (Twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))
- [Jan Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (Twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Jan ostry, USA](http://www.930solutions.com/) (Twitter: [@mrsharps](http://twitter.com/mrsharps))
- Jan Pope
- [Mitchel sprzedawcy, USA](http://www.mitchelsellers.com/) (Twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tomasz Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Wkłady społecznościowe

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Przykład kodu powiązanego z programem Visual Studio 2012 w witrynie MSDN: [Wingtip — zabawki](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- Kuba Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Przykład kodu powiązanego z programem Visual Studio 2012 w witrynie MSDN: [Seria samouczka formularzy sieci Web ASP.NET 4,5 w Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo — współautor Microsoft Technical odbiorca (Twitter @driazevedo:)  
  Visual Studio 2012 translation: [Iniciando com ASP.NET Web Forms 4,5-Parte 1-Introdução e Visão geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Poprzednie](url-routing.md)

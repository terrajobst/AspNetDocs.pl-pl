---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Debugowanie procedur składowanych (VB) | Microsoft Docs
author: rick-anderson
description: Wersje Visual Studio Professional i Team System umożliwiają ustawianie punktów przerwania i wykonywanie kroków w procedurach składowanych w ramach SQL Server i przechowywanie przechowywanych debugowania...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 13d213ef4baf493a4f05a82daae8d2dc3b0aa61b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78551276"
---
# <a name="debugging-stored-procedures-vb"></a>Debugowanie procedur składowanych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) lub [Pobierz plik PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Wersje Visual Studio Professional i Team System umożliwiają ustawienie punktów przerwania i przechodzenie do procedur składowanych w ramach SQL Server, dzięki czemu debugowanie procedur składowanych jest łatwe jak Debugowanie kodu aplikacji. W tym samouczku przedstawiono bezpośrednie debugowanie bazy danych i debugowanie aplikacji procedur składowanych.

## <a name="introduction"></a>Wprowadzenie

Program Visual Studio oferuje bogate środowisko debugowania. Za pomocą kilku naciśnięć klawiszy lub kliknięć myszą można użyć punktów przerwania do zatrzymania wykonywania programu i sprawdzenia jego stanu i przepływu sterowania. Wraz z kodem aplikacji debugowania program Visual Studio oferuje obsługę debugowania procedur składowanych z poziomu SQL Server. Podobnie jak punkty przerwania można ustawić w kodzie klasy ASP.NET lub klasy logiki biznesowej, tak aby można było ich umieścić w procedurach składowanych.

W tym samouczku omówiono procedurę składowaną z Eksplorator serwera w programie Visual Studio, a także jak ustawiać punkty przerwania, które są trafień, gdy procedura składowana zostanie wywołana z działającej aplikacji ASP.NET.

> [!NOTE]
> Niestety, procedury składowane można wykonać tylko w wersjach Professional i Team Systems programu Visual Studio. W przypadku korzystania z programu Visual Web Developer lub standardowej wersji programu Visual Studio, można zapoznać się z instrukcjami, które są niezbędne do debugowania procedur składowanych, ale nie będzie można replikować tych kroków na maszynie.

## <a name="sql-server-debugging-concepts"></a>SQL Server pojęć dotyczących debugowania

Microsoft SQL Server 2005 został zaprojektowany w celu zapewnienia integracji ze [środowiskiem uruchomieniowym języka wspólnego (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), który jest używany przez wszystkie zestawy .NET. W związku z tym SQL Server 2005 obsługuje obiekty zarządzanej bazy danych. Oznacza to, że można tworzyć obiekty bazy danych, takie jak procedury składowane i funkcje zdefiniowane przez użytkownika (UDF) jako metody klasy Visual Basic. Dzięki temu te procedury składowane i UDF mogą korzystać z funkcji w .NET Framework i z własnych klas niestandardowych. Oczywiście SQL Server 2005 zapewnia również obsługę obiektów usługi T-SQL Database.

SQL Server 2005 oferuje obsługę debugowania zarówno dla obiektów T-SQL, jak i zarządzanych baz danych. Jednak te obiekty mogą być debugowane tylko za poorednictwem wersji programu Visual Studio 2005 Professional i Team Systems. W tym samouczku sprawdzimy debugowanie obiektów bazy danych T-SQL. W kolejnym samouczku pokazano, jak debugować zarządzane obiekty bazy danych.

[Omówienie debugowania języka T-SQL i środowiska CLR w blogu SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) z poziomu [zespołu integracji SQL Server 2005 dla środowiska CLR](https://blogs.msdn.com/sqlclr/default.aspx) wyróżnia trzy sposoby debugowania SQL Server 2005 obiektów z programu Visual Studio:

- **Bezpośrednie debugowanie bazy danych (DDD)** — od Eksplorator serwera możemy przejść do dowolnego obiektu bazy danych T-SQL, takiego jak procedury składowane i UDF. Sprawdzimy DDD w kroku 1.
- **Debugowanie aplikacji** — można ustawić punkty przerwania w obiekcie bazy danych, a następnie uruchomić naszą aplikację ASP.NET. Gdy obiekt bazy danych jest wykonywany, punkt przerwania zostanie trafiony i kontrola zostanie włączona. Należy pamiętać, że z debugowaniem aplikacji nie można wkroczyć do obiektu bazy danych z kodu aplikacji. Należy jawnie ustawić punkty przerwania w tych procedurach składowanych lub UDF, gdy chcemy, aby debuger zatrzymał działanie. Debugowanie aplikacji jest badane począwszy od kroku 2.
- **Debugowanie z SQL Server Project** -Visual Studio Professional i Team Systems edition zawiera SQL Server typ projektu, który jest często używany do tworzenia obiektów zarządzanych baz danych. Będziemy badali użycie SQL Server projektów i debugowanie ich zawartości w następnym samouczku.

Program Visual Studio może debugować procedury składowane w lokalnych i zdalnych wystąpieniach SQL Server. Lokalne wystąpienie SQL Server to takie, które jest zainstalowane na tym samym komputerze co program Visual Studio. Jeśli używana baza danych SQL Server nie znajduje się na komputerze deweloperskim, jest uznawana za wystąpienie zdalne. Dla tych samouczków używamy lokalnych wystąpień SQL Server. Debugowanie procedur składowanych w zdalnym wystąpieniu programu SQL Server wymaga większej liczby kroków konfiguracyjnych niż podczas debugowania procedur składowanych w wystąpieniu lokalnym.

Jeśli używasz lokalnego SQL Server wystąpienia, możesz zacząć od kroku 1 i wykonać kroki opisane w tym samouczku na końcu. Jeśli jest używane zdalne wystąpienie SQL Server, należy najpierw upewnić się, że podczas debugowania na komputerze deweloperskim zostanie zarejestrowane konto użytkownika systemu Windows, które ma SQL Server logowaniu w wystąpieniu zdalnym. Ponadto zarówno identyfikator logowania bazy danych, jak i identyfikator logowania bazy danych używane do łączenia się z bazą danych z działającej aplikacji ASP.NET muszą być członkami roli `sysadmin`. Więcej informacji na temat konfigurowania programu Visual Studio i SQL Server do debugowania wystąpienia zdalnego znajduje się w sekcji debugowanie obiektów T-SQL Database w zdalnych wystąpieniach.

Na koniec należy pamiętać, że obsługa debugowania dla obiektów usługi T-SQL Database nie jest funkcją zaawansowaną jako obsługa debugowania dla aplikacji .NET. Na przykład warunki i filtry punktu przerwania nie są obsługiwane. dostępne są tylko podzbiór okien debugowania, ale nie można używać elementów Edit i Continue. okno bezpośrednie jest renderowane jako bezużyteczne i tak dalej. Aby uzyskać więcej informacji [, zobacz ograniczenia dotyczące poleceń debugera i funkcji](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) .

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Krok 1. bezpośrednie przechodzenie do procedury składowanej

Program Visual Studio ułatwia bezpośrednią debugowanie obiektu bazy danych. Pozwól, jak używać funkcji bezpośrednie debugowanie bazy danych (DDD), aby przejść do procedury składowanej `Products_SelectByCategoryID` w bazie danych Northwind. Jako że jego nazwa oznacza, `Products_SelectByCategoryID` zwraca informacje o produkcie dla określonej kategorii; został on utworzony w [istniejących procedurach składowanych dla danego TableAdaptersego samouczka zestawu danych](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Zacznij od przechodzenia do Eksplorator serwera i rozwiń węzeł bazy danych Northwind. Następnie przejdź do szczegółów w folderze procedury składowane, kliknij prawym przyciskiem myszy `Products_SelectByCategoryID` procedury składowanej i wybierz opcję wejdź do procedury składowanej z menu kontekstowego. Spowoduje to uruchomienie debugera.

Ponieważ `Products_SelectByCategoryID` procedury składowanej oczekuje `@CategoryID` parametr wejściowy, zostanie wyświetlony monit o podanie tej wartości. Wprowadź wartość 1, która zwróci informacje o napojach.

![Użyj wartości 1 dla parametru @CategoryID](debugging-stored-procedures-vb/_static/image1.png)

**Rysunek 1**. Użyj wartości 1 dla parametru `@CategoryID`

Po podaniu wartości parametru `@CategoryID` wykonywana jest procedura składowana. W przeciwieństwie do ukończenia, debuger zatrzymuje wykonywanie na pierwszej instrukcji. Zwróć uwagę na żółtą strzałkę na marginesie wskazującą bieżącą lokalizację w procedurze składowanej. Można wyświetlać i edytować wartości parametrów za pomocą okno wyrażeń kontrolnych lub przez umieszczenie kursora nad nazwą parametru w procedurze składowanej.

[![debuger został zatrzymany na pierwszej instrukcji procedury składowanej](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Rysunek 2**. debuger został zatrzymany na pierwszej instrukcji procedury składowanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](debugging-stored-procedures-vb/_static/image4.png))

Aby krokowo wykonać procedurę składowaną o jednej instrukcji w danym momencie, kliknij przycisk Przekrocz na pasku narzędzi lub naciśnij klawisz F10. Procedura składowana `Products_SelectByCategoryID` zawiera jedną instrukcję `SELECT`, więc naciśnięcie klawisza F10 spowoduje przekroczenie pojedynczej instrukcji i zakończenie wykonywania procedury składowanej. Po zakończeniu procedury składowanej jej dane wyjściowe pojawią się w oknie danych wyjściowych, a debuger zostanie przerwany.

> [!NOTE]
> Debugowanie T-SQL odbywa się na poziomie instrukcji; nie można przejść do instrukcji `SELECT`.

## <a name="step-2-configuring-the-website-for-application-debugging"></a>Krok 2. Konfigurowanie witryny sieci Web na potrzeby debugowania aplikacji

Debugowanie procedury składowanej bezpośrednio z Eksplorator serwera jest przydatne w wielu scenariuszach, które są bardziej interesujące podczas debugowania procedury składowanej, gdy jest wywoływana z naszej aplikacji ASP.NET. Możemy dodać punkty przerwania do procedury składowanej z poziomu programu Visual Studio, a następnie rozpocząć debugowanie aplikacji ASP.NET. Gdy procedura składowana z punktami przerwania jest wywoływana z aplikacji, wykonywanie zostanie zatrzymane w punkcie przerwania i będzie można wyświetlać i zmieniać wartości parametrów procedury składowanej oraz krokowo, podobnie jak w kroku 1.

Zanim będziemy mogli rozpocząć debugowanie procedur składowanych wywoływanych z aplikacji, musimy nakazać aplikacji sieci Web ASP.NET integrację z debugerem SQL Server. Zacznij od kliknięcia prawym przyciskiem myszy nazwy witryny sieci Web w Eksplorator rozwiązań (`ASPNET_Data_Tutorial_74_VB`). Wybierz opcję strony właściwości z menu kontekstowego, wybierz pozycję Opcje uruchomienia po lewej stronie, a następnie zaznacz pole wyboru SQL Server w sekcji debugery (zobacz rysunek 3).

[![zaznaczyć pole wyboru SQL Server na stronach właściwości aplikacji](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Rysunek 3**. Zaznacz pole wyboru SQL Server na stronach właściwości aplikacji (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](debugging-stored-procedures-vb/_static/image7.png))

Ponadto należy zaktualizować parametry połączenia z bazą danych używane przez aplikację, aby pule połączeń były wyłączone. Gdy połączenie z bazą danych jest zamknięte, odpowiedni obiekt `SqlConnection` jest umieszczany w puli dostępnych połączeń. Podczas ustanawiania połączenia z bazą danych można pobrać dostępny obiekt połączenia z tej puli zamiast tworzyć i ustanawiać nowe połączenie. Ta Pula obiektów połączeń jest ulepszeniem wydajności i jest domyślnie włączona. Jednak podczas debugowania chcemy wyłączyć buforowanie połączeń, ponieważ infrastruktura debugowania nie jest poprawnie ponownie ustanawiana podczas pracy z połączeniem, które zostało wykonane z puli.

Aby wyłączyć buforowanie połączeń, zaktualizuj `NORTHWNDConnectionString` w `Web.config` tak, aby obejmował `Pooling=false` ustawienia.

[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Po zakończeniu debugowania SQL Server za pośrednictwem aplikacji ASP.NET upewnij się, że pula połączeń zostanie przywrócona, usuwając ustawienie `Pooling` z parametrów połączenia (lub ustawiając je na `Pooling=true`).

W tym momencie aplikacja ASP.NET została skonfigurowana, aby umożliwić programowi Visual Studio debugowanie SQL Server obiektów bazy danych po wywołaniu za pomocą aplikacji sieci Web. Wszystkie te, które pozostają teraz, to dodanie punktu przerwania do procedury składowanej i rozpoczęcie debugowania.

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Krok 3. Dodawanie punktu przerwania i debugowania

Otwórz `Products_SelectByCategoryID` procedury składowanej i ustaw punkt przerwania na początku instrukcji `SELECT`, klikając margines w odpowiednim miejscu lub umieszczając kursor na początku instrukcji `SELECT` i naciskając klawisz F9. Jak pokazano na rysunku 4, punkt przerwania pojawia się jako czerwony okrąg na marginesie.

[![ustawić punktu przerwania w procedurze składowanej Products_SelectByCategoryID](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Rysunek 4**. Ustawianie punktu przerwania w procedurze składowanej `Products_SelectByCategoryID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image10.png))

Aby obiekt bazy danych SQL był debugowany za pomocą aplikacji klienckiej, należy bezwzględnie skonfigurować bazę danych do obsługi debugowania aplikacji. Gdy po raz pierwszy ustawisz punkt przerwania, to ustawienie powinno być automatycznie włączane, ale ostrożne sprawdzanie podwójne. Kliknij prawym przyciskiem myszy węzeł `NORTHWND.MDF` w Eksplorator serwera. Menu kontekstowe powinno zawierać element menu wyboru o nazwie debugowanie aplikacji.

![Upewnij się, że opcja debugowania aplikacji jest włączona](debugging-stored-procedures-vb/_static/image11.png)

**Rysunek 5**. Upewnij się, że opcja debugowania aplikacji jest włączona

Po włączeniu zestawu punktów przerwania i włączonej opcji debugowania aplikacji jesteśmy gotowi do debugowania procedury składowanej w przypadku wywołania z aplikacji ASP.NET. Uruchom Debuger, przechodząc do menu Debuguj i wybierając pozycję Rozpocznij debugowanie, naciskając klawisz F5 lub klikając zieloną ikonę odtwarzania na pasku narzędzi. Spowoduje to uruchomienie debugera i uruchomienie witryny sieci Web.

Procedura składowana `Products_SelectByCategoryID` została utworzona w [istniejących procedurach składowanych dla tego samouczka z określonym zestawem danych s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) . Odpowiadająca jej Strona sieci Web (`~/AdvancedDAL/ExistingSprocs.aspx`) zawiera widok GridView, który wyświetla wyniki zwrócone przez tę procedurę składowaną. Odwiedź Tę stronę za pomocą przeglądarki. Po osiągnięciu strony punkt przerwania w `Products_SelectByCategoryID` procedury składowanej zostanie trafiony i kontrolka zwróci do programu Visual Studio. Podobnie jak w kroku 1, można przejść przez instrukcje procedury składowanej i wyświetlić i zmodyfikować wartości parametrów.

[![Strona ExistingSprocs. aspx zawiera początkowo napoje](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Ilustracja 6**. Strona `ExistingSprocs.aspx` początkowo wyświetla napoje (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](debugging-stored-procedures-vb/_static/image14.png))

[![punkt przerwania procedury składowanej s został osiągnięty](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Rysunek 7**: punkt przerwania procedury składowanej s został osiągnięty ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](debugging-stored-procedures-vb/_static/image17.png))

Jak okno wyrażeń kontrolnych na rysunku 7 pokazuje wartość parametru `@CategoryID` to 1. Dzieje się tak, ponieważ na stronie `ExistingSprocs.aspx` początkowo są wyświetlane produkty w kategorii napoje, która ma `CategoryID` wartość 1. Wybierz inną kategorię z listy rozwijanej. Spowoduje to wygenerowanie ogłaszania zwrotnego i ponowne uruchomienie `Products_SelectByCategoryID` procedury składowanej. Punkt przerwania został ponownie trafiony, ale ten czas `@CategoryID` wartość parametru s odzwierciedla wybrany element listy rozwijanej `CategoryID`.

[![wybrać inną kategorię z listy rozwijanej](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Ilustracja 8**. Wybierz inną kategorię z listy rozwijanej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](debugging-stored-procedures-vb/_static/image20.png))

[![parametr @CategoryID odzwierciedla kategorię wybraną ze strony sieci Web](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Ilustracja 9**. parametr `@CategoryID` odzwierciedla kategorię wybraną ze strony sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](debugging-stored-procedures-vb/_static/image23.png))

> [!NOTE]
> Jeśli punkt przerwania w `Products_SelectByCategoryID` procedurze składowanej nie zostanie trafiony podczas odwiedzania `ExistingSprocs.aspx` strony, upewnij się, że pole wyboru SQL Server zostało zaznaczone w sekcji debugera na stronie właściwości aplikacji ASP.NET, że pule połączeń zostały wyłączone i że opcja debugowania aplikacji bazy danych jest włączona. Jeśli nadal występują problemy, uruchom ponownie program Visual Studio i spróbuj ponownie.

## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Debugowanie obiektów T-SQL Database w wystąpieniach zdalnych

Debugowanie obiektów bazy danych za pomocą programu Visual Studio jest dość proste, gdy wystąpienie bazy danych SQL Server znajduje się na tym samym komputerze co program Visual Studio. Jeśli jednak SQL Server i program Visual Studio znajdują się na różnych maszynach, w celu poprawnego działania wszystkiego konieczna jest pewna konfiguracja. Istnieją dwa podstawowe zadania, do których prowadzimy:

- Upewnij się, że logowanie używane do nawiązania połączenia z bazą danych za pośrednictwem ADO.NET należy do roli `sysadmin`.
- Upewnij się, że konto użytkownika systemu Windows używane przez program Visual Studio na komputerze deweloperskim jest prawidłowym kontem logowania SQL Server, które należy do roli `sysadmin`.

Pierwszy krok jest stosunkowo nieskomplikowany. Najpierw Zidentyfikuj konto użytkownika używane do nawiązania połączenia z bazą danych z aplikacji ASP.NET, a następnie z SQL Server Management Studio Dodaj to konto logowania do roli `sysadmin`.

Drugie zadanie wymaga, aby konto użytkownika systemu Windows używane do debugowania aplikacji było prawidłową nazwą logowania w zdalnej bazie danych. Jednak prawdopodobnie konto systemu Windows, które zostało zalogowane na stacji roboczej, nie jest prawidłową nazwą logowania w SQL Server. Zamiast dodawać określone konto logowania do SQL Server, lepszym wyborem będzie wyznaczenie konta użytkownika systemu Windows jako konta debugowania SQL Server. Następnie w celu debugowania obiektów bazy danych zdalnego wystąpienia SQL Server można uruchomić program Visual Studio przy użyciu poświadczeń konta logowania systemu Windows.

Przykład powinien ułatwić wyjaśnienie rzeczy. Załóżmy, że istnieje konto systemu Windows o nazwie `SQLDebug` w domenie systemu Windows. To konto należy dodać do zdalnego wystąpienia SQL Server jako prawidłowe logowanie i jako element członkowski roli `sysadmin`. Następnie, aby debugować wystąpienie SQL Server zdalnego z programu Visual Studio, musimy uruchomić program Visual Studio jako użytkownik `SQLDebug`. Można to zrobić, logując się z naszej stacji roboczej, logując się jako `SQLDebug`, a następnie uruchamiając program Visual Studio, ale prostszym podejściem jest zalogowanie się do naszej stacji roboczej przy użyciu własnych poświadczeń, a następnie użycie `runas.exe` do uruchomienia programu Visual Studio jako użytkownik `SQLDebug`. `runas.exe` umożliwia wykonywanie konkretnej aplikacji w ramach Guise innego konta użytkownika. Aby uruchomić program Visual Studio jako `SQLDebug`, w wierszu polecenia można wprowadzić następującą instrukcję:

[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Aby zapoznać się z bardziej szczegółowym wyjaśnieniem tego procesu, zobacz [William R. Vaughn](http://betav.com/BLOG/billva/) s *hitchhiker s w programie Visual Studio i SQL Server, siódmej wersji* , jak również [: Ustawianie uprawnień SQL Server dla debugowania](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Jeśli na komputerze deweloperskim jest uruchomiony system Windows XP z dodatkiem Service Pack 2, należy skonfigurować zaporę połączenia internetowego, aby zezwalała na zdalne debugowanie. Artykuł [How to: Enable SQL Server 2005 Debug](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) uwagi, że dotyczy to dwóch kroków: (a) na komputerze hosta programu Visual Studio, należy dodać `Devenv.exe` do listy wyjątków i otworzyć port TCP 135; i (b) na komputerze zdalnym (SQL), należy otworzyć port TCP 135 i dodać `sqlservr.exe` do listy wyjątków. Jeśli zasady domeny wymagają komunikacji sieciowej za pomocą protokołu IPSec, należy otworzyć porty UDP 4500 i UDP 500.

## <a name="summary"></a>Podsumowanie

Oprócz zapewniania obsługi debugowania kodu aplikacji .NET program Visual Studio udostępnia również różne opcje debugowania dla SQL Server 2005. W tym samouczku przedstawiono dwie z tych opcji: bezpośrednie debugowanie bazy danych i debugowanie aplikacji. Aby bezpośrednio debugować obiekt bazy danych T-SQL, zlokalizuj obiekt za pomocą Eksplorator serwera a następnie kliknij go prawym przyciskiem myszy i wybierz polecenie Wkrocz do. Spowoduje to uruchomienie debugera i zatrzymanie pierwszej instrukcji w obiekcie bazy danych, w tym momencie można wykonać krokowo przez instrukcje obiektu i wyświetlić i zmodyfikować wartości parametrów. W kroku 1 Ta metoda została użyta, aby przejść do procedury składowanej `Products_SelectByCategoryID`.

Debugowanie aplikacji umożliwia ustawianie punktów przerwania bezpośrednio w obiektach bazy danych. Gdy obiekt bazy danych z punktami przerwania jest wywoływany z aplikacji klienckiej (takiej jak aplikacja sieci Web ASP.NET), program zostaje zatrzymany, gdy debuger przejęcia. Debugowanie aplikacji jest przydatne, ponieważ bardziej jasno pokazuje działanie aplikacji, która powoduje wywoływanie określonego obiektu bazy danych. Jednak wymaga to nieco większej konfiguracji i instalacji niż bezpośrednie debugowanie bazy danych.

Obiekty bazy danych mogą być również debugowane za poorednictwem projektów SQL Server. Będziemy przeglądać projekty SQL Server i sposoby ich używania do tworzenia i debugowania obiektów zarządzanych baz danych w następnym samouczku.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](protecting-connection-strings-and-other-configuration-information-vb.md)
> [dalej](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)

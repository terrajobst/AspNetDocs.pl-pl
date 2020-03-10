---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Wskazówki dotyczące laboratorium: obsługa usługi Azure Websites: zarządzanie zmianami i skalowaniem | Microsoft Docs'
author: rick-anderson
description: W tym laboratorium dowiesz się, jak Microsoft Azure ułatwiają tworzenie i wdrażanie witryn sieci Web w środowisku produkcyjnym.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: c88bae40a8aa092037c0b359ee391acaf161cf10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624272"
---
# <a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Ćwiczenia praktyczne: łatwe w obsłudze witryny internetowe platformy Azure: zarządzanie zmianami i skalowaniem

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

> Microsoft Azure ułatwia tworzenie i wdrażanie witryn sieci Web w środowisku produkcyjnym. Ale nie jesteś gotowy, gdy aplikacja jest aktywna, zaczynasz od razu zacząć pracę. Musisz obsługiwać zmieniające się wymagania, aktualizacje bazy danych, skalowanie i nie tylko. Na szczęście Azure App Service został objęty dużą ilością funkcji, które ułatwiają bezproblemowe działanie witryn.
>
> Platforma Azure oferuje bezpieczne i elastyczne opcje programowania, wdrażania i skalowania dla aplikacji sieci Web dowolnej wielkości. Korzystaj z istniejących narzędzi do tworzenia i wdrażania aplikacji bez konieczności zarządzania infrastrukturą.
>
> Zapewnij sobie produkcyjną aplikację sieci Web w kilka minut, korzystając z łatwego wdrażania zawartości utworzonej przy użyciu ulubionego narzędzia deweloperskiego. Istniejącą witrynę można wdrożyć bezpośrednio z kontroli źródła z obsługą usługi **git**, **GitHub**, **BitBucket**, **TFS**i nawet usługi **Dropbox**. Wdrażaj bezpośrednio z ulubionych środowiska IDE lub skryptów przy użyciu **programu PowerShell** w systemie Windows lub w narzędziach **interfejsu wiersza polecenia** uruchomionych w dowolnym systemie operacyjnym. Po wdrożeniu witryny są stale aktualne dzięki obsłudze ciągłego wdrażania.
>
> Platforma Azure oferuje skalowalne i niezawodne rozwiązania magazynu w chmurze, kopii zapasowych i odzyskiwania dla dowolnych danych, dużych i małych. W przypadku wdrażania aplikacji w środowisku produkcyjnym usługi magazynu, takie jak tabele, obiekty blob i bazy danych SQL, ułatwiają skalowanie aplikacji w chmurze.
>
> W przypadku baz danych SQL ważne jest, aby zachować aktualność bazy danych podczas wdrażania nowych wersji aplikacji. Dzięki **migracje Code First platformy Entity Frameworkom**rozwój i wdrażanie modelu danych zostały uproszczone, aby aktualizować środowiska w kilka minut. W ramach tego praktycznego laboratorium zostaną wyświetlone różne tematy, które można napotkać podczas wdrażania aplikacji sieci Web w środowiskach produkcyjnych w Microsoft Azure.
>
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).
>
> Aby uzyskać więcej szczegółowych informacji dotyczących tego tematu, zobacz [Tworzenie aplikacji w chmurze w świecie przy użyciu książki elektronicznej platformy Azure](building-real-world-cloud-apps-with-windows-azure/introduction.md).

<a id="Overview"></a>
## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Włączanie migracji Entity Framework przy użyciu istniejącego modelu
- Zaktualizuj model obiektów i bazę danych odpowiednio przy użyciu migracji Entity Framework
- Wdrażanie do Azure App Service przy użyciu usługi git
- Wycofywanie do poprzedniego wdrożenia przy użyciu portalu zarządzania platformy Azure
- Skalowanie aplikacji sieci Web przy użyciu usługi Azure Storage
- Konfigurowanie automatycznego skalowania aplikacji sieci Web przy użyciu usługi Azure portal zarządzania
- Tworzenie i Konfigurowanie projektu testu obciążenia w programie Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:

- [Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/) lub nowszego
- [Zestaw Azure SDK dla programu .NET 2,2](https://www.microsoft.com/windowsazure/sdk/)
- [System kontroli wersji GIT](http://git-scm.com/download)
- Subskrypcja Microsoft Azure

    - Zarejestruj się, aby skorzystać z [bezpłatnej wersji próbnej](https://aka.ms/watk-freetrial)
    - Jeśli jesteś Visual Studio Professional, Test Professional, Premium lub Ultimate z subskrypcją MSDN lub Platformy MSDN, Aktywuj teraz [korzyść MSDN](https://aka.ms/watk-msdn) , aby rozpocząć opracowywanie i testowanie na platformie Azure
    - [BizSpark](https://aka.ms/watk-bizspark) członkowie automatycznie otrzymują korzyść platformy Azure za pomocą subskrypcji Visual Studio Ultimate z subskrypcją MSDN
    - Członkowie programu [Microsoft Partner Network](https://aka.ms/watk-mpn) Cloud Essentials otrzymują bezpłatnie środki na korzystanie z platformy Azure

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

Aby można było uruchomić ćwiczenia w tym ćwiczeniu, należy najpierw skonfigurować środowisko.

1. Otwórz Eksploratora Windows i przejdź do folderu **źródłowego** laboratorium.
2. Kliknij prawym przyciskiem myszy pozycję **Setup. cmd** i wybierz polecenie **Uruchom jako administrator** , aby uruchomić proces instalacji, który skonfiguruje środowisko i zainstaluje fragmenty kodu programu Visual Studio dla tego laboratorium.
3. Jeśli zostanie wyświetlone okno dialogowe Kontrola konta użytkownika, potwierdź akcję, aby wykonać operację.

> [!NOTE]
> Upewnij się, że wszystkie zależności dla tego laboratorium zostały sprawdzone przed uruchomieniem Instalatora.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Używanie fragmentów kodu

W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu. Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, do których można uzyskać dostęp z poziomu Visual Studio 2013, aby uniknąć konieczności ręcznego dodawania go.

> [!NOTE]
> Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze **BEGIN** w ćwiczeniu, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych. Należy pamiętać, że fragmenty kodu dodawane podczas wykonywania nie są dostępne w tych rozwiązaniach i mogą nie działać do czasu ukończenia ćwiczenia. Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder **końcowy** zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu. Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Korzystanie z Entity Framework migracji](#Exercise1)
2. [Wdrażanie aplikacji sieci Web do przemieszczania](#Exercise2)
3. [Wykonywanie wycofywania wdrożenia w środowisku produkcyjnym](#Exercise3)
4. [Skalowanie przy użyciu usługi Azure Storage](#Exercise4)
5. [Używanie skalowania automatycznego dla Web Apps](#Exercise5) (opcjonalnie w przypadku wersji Visual Studio 2013 Ultimate)

Szacowany czas wykonywania tego laboratorium: **75 minut**

> [!NOTE]
> Po pierwszym uruchomieniu programu Visual Studio należy wybrać jedną z wstępnie zdefiniowanych kolekcji ustawień. Każda wstępnie zdefiniowana kolekcja jest zaprojektowana tak, aby była zgodna z konkretnym stylem deweloperskim i określa układy okien, zachowanie edytora, fragmenty kodu IntelliSense i opcje okna dialogowego. Procedury przedstawione w tym laboratorium opisują akcje niezbędne do wykonania danego zadania w programie Visual Studio, gdy jest używana **Ogólna kolekcja ustawień deweloperskich** . W przypadku wybrania innej kolekcji ustawień dla środowiska programistycznego mogą wystąpić różnice w czynnościach, które należy wziąć pod uwagę.

<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Ćwiczenie 1: korzystanie z Entity Framework migracji

Podczas tworzenia aplikacji model danych może ulec zmianie z upływem czasu. Te zmiany mogą wpływać na istniejący model bazy danych (Jeśli tworzysz nową wersję) i ważne jest, aby zachować aktualność bazy danych, aby zapobiec wystąpieniu błędów.

Aby uprościć śledzenie tych zmian w modelu, **migracje Code First platformy Entity Framework** automatycznie wykrywać zmiany porównujące model ze schematem bazy danych i generować określony kod w celu zaktualizowania bazy danych, tworząc nowe *wersje* bazy danych.

W tym ćwiczeniu pokazano, jak włączyć **migracje** dla aplikacji oraz jak łatwo wykrywać i generować zmiany w celu zaktualizowania baz danych.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Zadanie 1 — Włączanie migracji

W tym zadaniu przedstawiono kroki umożliwiające włączenie **migracje Code First platformy Entity Framework** do bazy danych **quizu pojedynek maniaków komputerowych** , zmiana modelu i zrozumienie sposobu odzwierciedlenia tych zmian w bazie danych.

1. Otwórz program Visual Studio i Otwórz plik rozwiązania **GeekQuiz. sln** z **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.
2. Skompiluj rozwiązanie, aby pobrać i zainstalować zależności pakietów **NuGet** . W tym celu kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Kompiluj rozwiązanie** lub naciśnij **klawisze CTRL + SHIFT + B**.
3. W menu **Narzędzia** w programie Visual Studio wybierz pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **konsola Menedżera pakietów**.
4. W **konsoli Menedżera pakietów**wprowadź następujące polecenie, a następnie naciśnij klawisz **Enter**. Zostanie utworzona początkowa migracja oparta na istniejącym modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Włączanie migracji](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Włączanie migracji")

    *Włączanie migracji*

    > [!NOTE]
    > To polecenie dodaje folder **migracji** do projektu quizu pojedynek maniaków komputerowych, który zawiera plik o nazwie **Configuration.cs**. Klasa **konfiguracji** umożliwia skonfigurowanie sposobu zachowania migracji w kontekście.
5. Po włączeniu migracji należy zaktualizować klasę **konfiguracji** , aby wypełnić bazę danych początkowymi danymi, które są wymagane przez **Quiz pojedynek maniaków komputerowych** . W obszarze **migracje**Zastąp plik **Configuration.cs** plikiem znajdującym się w folderze **Source\Assets** w tym laboratorium.

    > [!NOTE]
    > Ponieważ **migracje** spowodują wywołanie metody **inicjatora** przy każdej aktualizacji bazy danych, należy się upewnić, że rekordy nie są zduplikowane w bazie danych. Metoda **AddOrUpdate** pomaga zapobiegać duplikowaniu danych.
6. Aby dodać początkową migrację, wprowadź następujące polecenie, a następnie naciśnij klawisz **Enter**.

    > [!NOTE]
    > Upewnij się, że w wystąpieniu LocalDB nie ma bazy danych o nazwie &quot;GeekQuizProd&quot;.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Dodawanie migracji schematu bazowego](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Dodawanie migracji schematu bazowego")

    *Dodawanie migracji schematu bazowego*

    > [!NOTE]
    > Po zakończeniu migracji **dodatek** będzie szkieletem następnej migracji na podstawie zmian wprowadzonych w modelu od momentu utworzenia ostatniej migracji. W tym przypadku, ponieważ jest to pierwsza migracja projektu, spowoduje dodanie skryptów w celu utworzenia wszystkich tabel zdefiniowanych w klasie **TriviaContext** .
7. Wykonaj migrację, aby zaktualizować bazę danych, uruchamiając następujące polecenie. Dla tego polecenia należy określić flagę **verbose** , aby wyświetlić instrukcje SQL mające zastosowanie do docelowej bazy danych.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Tworzenie początkowej bazy danych](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Tworzenie początkowej bazy danych")

    *Tworzenie początkowej bazy danych*

    > [!NOTE]
    > **Aktualizacja — baza danych** będzie stosować wszystkie oczekujące migracje do bazy danych programu. W takim przypadku zostanie utworzona baza danych przy użyciu parametrów połączenia zdefiniowanych w pliku Web. config.
8. Przejdź do menu **Widok** i Otwórz **Eksplorator obiektów SQL Server**.

    ![Otwórz w Eksplorator obiektów SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Otwórz w Eksplorator obiektów SQL Server")

    *Otwórz w Eksplorator obiektów SQL Server*
9. W oknie **Eksplorator obiektów SQL Server** Połącz się z wystąpieniem LocalDB, klikając prawym przyciskiem myszy węzeł **SQL Server** i wybierając polecenie **Dodaj SQL Server...** .

    ![Dodawanie wystąpienia SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Dodawanie wystąpienia SQL Server")

    *Dodawanie wystąpienia SQL Server do Eksplorator obiektów SQL Server*
10. Ustaw **nazwę serwera** na *(LocalDB) \V11.0* i pozostaw **uwierzytelnianie systemu Windows** jako tryb uwierzytelniania. Kliknij pozycję **Połącz**, aby kontynuować.

    ![Nawiązywanie połączenia z usługą LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Nawiązywanie połączenia z usługą LocalDB")

    *Nawiązywanie połączenia z usługą LocalDB*
11. Otwórz bazę danych **GeekQuizProd** i rozwiń węzeł **tabele** . Jak widać, polecenie **Update-Database** wygenerowało wszystkie tabele zdefiniowane w klasie **TriviaContext** . Znajdź obiekt **dbo. TriviaQuestions** tabelę i Otwórz węzeł kolumny. W następnym zadaniu dodasz nową kolumnę do tej tabeli i zaktualizujesz bazę danych przy użyciu **migracji**.

    ![Kolumny pytań kwizy](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Kolumny pytań kwizy")

    *Kolumny pytań kwizy*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Zadanie 2 — aktualizowanie schematu bazy danych za pomocą migracji

W tym zadaniu będziesz używać **migracje Code First platformy Entity Framework** do wykrywania zmiany w modelu i generowania niezbędnego kodu w celu zaktualizowania bazy danych. Jednostkę **TriviaQuestions** należy zaktualizować, dodając do niej nową właściwość. Następnie uruchomisz polecenia, aby utworzyć nową migrację w celu uwzględnienia nowej kolumny w tabeli.

1. W **Eksplorator rozwiązań**kliknij dwukrotnie plik **TriviaQuestion.cs** znajdujący się wewnątrz folderu **models** .
2. Dodaj nową właściwość o nazwie **Hint**, jak pokazano w poniższym fragmencie kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. W **konsoli Menedżera pakietów**wprowadź następujące polecenie, a następnie naciśnij klawisz **Enter**. Zostanie utworzona nowa migracja odzwierciedlająca zmianę w modelu.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Dodawanie-migracja](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Dodawanie-migracja")

    *Dodawanie-migracja*

    > [!NOTE]
    > Plik migracji składa się z dwóch metod — w **górę** i **w dół**.
    >
    > - Metoda **up** zostanie użyta do określenia, jakie zmiany bieżąca wersja naszej aplikacji musi zastosować do bazy danych.
    > - W **dół** jest używany do odwrócenia zmian, które zostały dodane do metody **up** .
    >
    > Gdy migracja bazy danych aktualizuje bazę danych, zostanie uruchomiona wszystkie migracje w kolejności sygnatur czasowych, a tylko te, które nie były używane od ostatniej aktualizacji (\_tabela MigrationHistory śledzi zastosowanych migracji). Metoda **up** wszystkich migracji zostanie wywołana i wprowadzi zmiany w bazie danych. Jeśli zdecydujesz się powrócić do poprzedniej migracji, metoda w **dół** zostanie wywołana, aby ponownie wykonać zmiany w odwrotnej kolejności.
4. W **konsoli Menedżera pakietów**wprowadź następujące polecenie, a następnie naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. Dane wyjściowe polecenia **Update-Database** wygenerowały instrukcję **ALTER TABLE** SQL, aby dodać nową kolumnę do tabeli **TriviaQuestions** , jak pokazano na poniższej ilustracji.

    ![Wygenerowana instrukcja SQL Add Column](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Wygenerowana instrukcja SQL Add Column")

    *Wygenerowana instrukcja SQL Add Column*
6. W **Eksplorator obiektów SQL Server**Odśwież obiekt **dbo. TriviaQuestions** tabelę i sprawdź, czy jest wyświetlana nowa kolumna **podpowiedzi** .

    ![Wyświetlanie nowej kolumny podpowiedzi](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Wyświetlanie nowej kolumny podpowiedzi")

    *Wyświetlanie nowej kolumny podpowiedzi*
7. W edytorze **TriviaQuestion.cs** Dodaj ograniczenie **StringLength** do właściwości *wskazówki* , jak pokazano w poniższym fragmencie kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. W **konsoli Menedżera pakietów**wprowadź następujące polecenie, a następnie naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. W **konsoli Menedżera pakietów**wprowadź następujące polecenie, a następnie naciśnij klawisz **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. Dane wyjściowe polecenia **Update-Database** wygenerowały instrukcję **ALTER TABLE** SQL, aby zaktualizować typ kolumny *wskazówki* tabeli **TriviaQuestions** , jak pokazano na poniższej ilustracji.

    ![Wygenerowana instrukcja SQL ALTER COLUMN](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Wygenerowana instrukcja SQL ALTER COLUMN")

    *Wygenerowana instrukcja SQL ALTER COLUMN*
11. W **Eksplorator obiektów SQL Server**Odśwież obiekt **dbo. TriviaQuestions** tabelę i sprawdź, czy typ kolumny **wskazówki** to **nvarchar (150)** .

    ![Wyświetlanie nowego ograniczenia](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Wyświetlanie nowego ograniczenia")

    *Wyświetlanie nowego ograniczenia*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Ćwiczenie 2: wdrażanie aplikacji sieci Web do przemieszczania

**Web Apps w Azure App Service** umożliwia przeprowadzenie publikacji etapowej. Publikowanie etapowe tworzy miejsce na lokację przejściową dla każdej domyślnej witryny produkcyjnej i umożliwia zastępowanie tych gniazd bez określonego czasu. Jest to naprawdę przydatne do weryfikowania zmian przed udostępnieniem ich publicznie, przyrostowo integrowania zawartości lokacji i wycofywania, jeśli zmiany nie działają zgodnie z oczekiwaniami.

W tym ćwiczeniu aplikacja **quizu pojedynek maniaków komputerowych** zostanie wdrożona w środowisku przejściowym aplikacji sieci Web przy użyciu kontroli źródła git. W tym celu należy utworzyć aplikację sieci Web i udostępnić wymagane składniki w portalu zarządzania, skonfigurować repozytorium **git** i wypchnąć kod źródłowy aplikacji z komputera lokalnego do miejsca przejściowego. Możesz również zaktualizować produkcyjną bazę danych do **migracje Code First** utworzonej w poprzednim ćwiczeniu. Następnie wykonasz aplikację w tym środowisku testowym, aby sprawdzić jej działanie. Po upewnieniu się, że działa ona zgodnie z oczekiwaniami, będziesz wspierać aplikację w środowisku produkcyjnym.

> [!NOTE]
> Aby włączyć publikowanie etapowe, aplikacja sieci Web musi być w **trybie standardowym**. Należy pamiętać, że w przypadku zmiany aplikacji sieci Web na tryb standardowy zostaną naliczone dodatkowe opłaty. Aby uzyskać więcej informacji na temat cen, zobacz [Cennik usługi App Service](https://azure.microsoft.com/pricing/details/app-service/).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Zadanie 1 — Tworzenie aplikacji sieci Web w Azure App Service

To zadanie spowoduje utworzenie aplikacji sieci Web w **Azure App Service** z poziomu portalu zarządzania. Skonfigurujesz również **SQL Database** , aby zachować dane aplikacji, i skonfigurować lokalne repozytorium git dla kontroli źródła.

1. Przejdź do [portalu zarządzania platformy Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konto Microsoft skojarzonego z subskrypcją.

    ![Logowanie się do portalu zarządzania systemu Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Logowanie się do portalu zarządzania systemu Azure*
2. Kliknij przycisk **Nowy** na pasku poleceń w dolnej części strony.

    ![Tworzenie nowej aplikacji sieci Web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Tworzenie nowej aplikacji sieci Web")

    *Tworzenie nowej aplikacji sieci Web*
3. Kliknij pozycje **obliczenia**, **Witryna sieci Web** , a następnie pozycję **Tworzenie niestandardowe**.

    ![Tworzenie nowej aplikacji sieci Web przy użyciu funkcji tworzenia niestandardowego](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Tworzenie nowej aplikacji sieci Web przy użyciu funkcji tworzenia niestandardowego")

    *Tworzenie nowej aplikacji sieci Web przy użyciu funkcji tworzenia niestandardowego*
4. W oknie dialogowym **Nowy witryna sieci Web — Tworzenie niestandardowe** Podaj dostępny **adres URL** (np. *pojedynek maniaków komputerowych-Quiz*), wybierz lokalizację na liście rozwijanej **region** , a następnie wybierz pozycję **Utwórz nową bazę danych SQL** na liście rozwijanej **baza danych** . Na koniec zaznacz pole wyboru **Opublikuj z kontroli źródła** i kliknij przycisk **dalej**.

    ![Dostosowywanie nowej aplikacji sieci Web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Dostosowywanie nowej aplikacji sieci Web*
5. Określ następujące informacje dotyczące ustawień bazy danych:

   - W polu tekstowym **Nazwa** wprowadź nazwę bazy danych (np. *geekquiz\_DB*)
   - Z listy **rozwijanej** serwer wybierz pozycję **nowy serwer bazy danych SQL**. Alternatywnie można wybrać istniejący serwer.
   - W polach **Nazwa użytkownika** i **hasło bazy** danych wprowadź nazwę użytkownika i hasło administratora dla serwera bazy danych SQL. W przypadku wybrania już utworzonego serwera zostanie wyświetlony monit o podanie hasła.

     ![Określanie ustawień bazy danych](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Określanie ustawień bazy danych*
6. Kliknij przycisk **Dalej**, aby kontynuować.
7. Wybierz **lokalne repozytorium git** , które ma być używane przez kontrolę źródłową, a następnie kliknij przycisk **dalej**.

    > [!NOTE]
    > Może zostać wyświetlony monit o podanie poświadczeń wdrożenia (nazwy użytkownika i hasła).

    ![Tworzenie repozytorium git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Tworzenie repozytorium git*
8. Poczekaj na utworzenie nowej aplikacji sieci Web.

    > [!NOTE]
    > Domyślnie platforma Azure udostępnia domeny pod adresem *azurewebsites.NET* , ale zapewnia również możliwość ustawiania domen niestandardowych przy użyciu portalu zarządzania Azure. Można jednak zarządzać domenami niestandardowymi tylko w przypadku korzystania z określonych trybów Azure App Service.
    >
    > Azure App Service jest dostępna w wersjach bezpłatna, współdzielona, podstawowa, standardowa i Premium. W trybie bezpłatna i współdzielona wszystkie aplikacje sieci Web działają w środowisku wielodostępnym i mają przydziały użycia procesora CPU, pamięci i sieci. Maksymalna liczba bezpłatnych aplikacji może się różnić w zależności od planu. W trybie standardowym można wybrać aplikacje, które będą uruchamiane na dedykowanych maszynach wirtualnych odpowiadających standardowym zasobom obliczeniowym platformy Azure. Konfigurację trybu aplikacji sieci Web można znaleźć w menu **skalowanie** aplikacji sieci Web.
    >
    > ![Tryby Azure App Service](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Tryby Azure App Service")
    >
    > Jeśli używasz trybu **współużytkowanego** lub **standardowego** , będziesz mieć możliwość zarządzania domenami niestandardowymi dla aplikacji sieci Web, przechodząc do menu **konfigurowania** aplikacji i klikając pozycję **Zarządzaj domenami** w obszarze *nazwy domen*.
    >
    > ![Zarządzanie domenami](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Zarządzanie domenami")
    >
    > ![Zarządzanie domenami niestandardowymi](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Zarządzanie domenami niestandardowymi")
9. Po utworzeniu aplikacji sieci Web kliknij link w kolumnie **adres URL** , aby sprawdzić, czy nowa aplikacja sieci Web jest uruchomiona.

    ![Przeglądanie w nowej aplikacji sieci Web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Przeglądanie w nowej aplikacji sieci Web*

    ![uruchomiona aplikacja sieci Web](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *uruchomiona aplikacja sieci Web*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Zadanie 2 — Tworzenie SQL Database produkcyjnych

W tym zadaniu zostanie użyta **migracje Code First platformy Entity Framework** do utworzenia bazy danych dla wystąpienia **Azure SQL Database** utworzonego w poprzednim zadaniu.

1. W portal zarządzania przejdź do aplikacji sieci Web utworzonej w poprzednim zadaniu i przejdź do jej **pulpitu nawigacyjnego**.
2. Na stronie **pulpit nawigacyjny** kliknij link **Wyświetl parametry połączenia** w sekcji **szybki przegląd** .

    ![Wyświetlanie parametrów połączenia](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Wyświetlanie parametrów połączenia")

    *Wyświetlanie parametrów połączenia*
3. Skopiuj wartość **parametrów połączenia** i Zamknij okno dialogowe.

    ![Parametry połączenia w usłudze Azure portal zarządzania](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Parametry połączenia w usłudze Azure portal zarządzania")

    *Parametry połączenia w usłudze Azure portal zarządzania*
4. Kliknij pozycję **bazy danych SQL** , aby wyświetlić listę baz danych SQL na platformie Azure

    ![Menu SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "Menu SQL Database")

    *Menu SQL Database*
5. Znajdź bazę danych utworzoną w poprzednim zadaniu i kliknij serwer.

    ![Serwer SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "Serwer usługi SQL Database")

    *Serwer SQL Database*
6. Na stronie **Szybki Start** serwera kliknij pozycję **Konfiguruj**.

    ![Konfigurowanie menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Konfigurowanie menu")

    *Konfigurowanie menu*
7. W sekcji **dozwolone adresy IP** kliknij pozycję **Dodaj do linku dozwolone adresy IP** , aby umożliwić IP łączenie się z serwerem SQL Database.

    ![Dozwolone adresy IP](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Dozwolone adresy IP")

    *Dozwolone adresy IP*
8. Kliknij przycisk **Zapisz** w dolnej części strony, aby ukończyć ten krok.
9. Przełącz się z powrotem do programu Visual Studio.
10. W **konsoli Menedżera pakietów**wykonaj następujące polecenie zastępujące symbol zastępczy *[The-Connection-String]* z parametrami połączenia skopiowanymi z platformy Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Aktualizuj bazę danych dla systemu Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Aktualizuj bazę danych dla systemu Windows Azure SQL Database")

    *Aktualizowanie Azure SQL Database określania wartości docelowej bazy danych*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Zadanie 3 — wdrażanie quizu pojedynek maniaków komputerowych do przemieszczania za pomocą usługi git

W tym zadaniu zostanie włączone publikowanie etapowe w aplikacji sieci Web. Następnie będzie można opublikować aplikację quizu pojedynek maniaków komputerowych bezpośrednio z komputera lokalnego w środowisku tymczasowym aplikacji sieci Web za pomocą narzędzia Git.

1. Wróć do portalu i kliknij nazwę aplikacji sieci Web w kolumnie **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania aplikacjami sieci Web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Otwieranie stron zarządzania aplikacjami sieci Web*
2. Przejdź do strony **skalowanie** . W obszarze **Ogólne** wybierz pozycję Konfiguracja **standardowa** , a następnie kliknij pozycję **Zapisz** na pasku poleceń.

    > [!NOTE]
    > Aby uruchamiać wszystkie aplikacje sieci Web w bieżącym regionie i subskrypcji w trybie **standardowym** , należy pozostawić pole wyboru **Zaznacz wszystko** zaznaczone w konfiguracji **wybierz lokacje** . W przeciwnym razie wyczyść pole wyboru **Zaznacz wszystko** .

    ![Uaktualnianie aplikacji sieci Web do trybu standardowego](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Uaktualnianie aplikacji sieci Web do trybu standardowego")

    *Uaktualnianie aplikacji sieci Web do trybu standardowego*
3. Kliknij przycisk **tak** , aby potwierdzić zmiany.

    ![Potwierdzanie zmiany do trybu standardowego](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Kontynuowanie zmiany trybu aplikacji sieci Web")

    *Potwierdzanie zmiany do trybu standardowego*
4. Przejdź do strony **pulpit nawigacyjny** , a następnie kliknij pozycję **Włącz publikowanie etapowe** w sekcji **szybki przegląd** .

    ![Włączanie publikowania etapowego](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Włączanie publikowania etapowego")

    *Włączanie publikowania etapowego*
5. Kliknij przycisk **tak** , aby włączyć publikowanie etapowe.

    ![Potwierdzanie publikowania etapowego](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Kliknij przycisk tak, aby włączyć publikowanie etapowe")

    *Potwierdzanie publikowania etapowego*
6. Na liście aplikacji sieci Web rozwiń znacznik z lewej strony nazwy aplikacji sieci Web, aby wyświetlić miejsce lokacji tymczasowej. Ma nazwę aplikacji sieci Web, a następnie ***(przejściową)***. Kliknij lokację przemieszczania, aby przejść do strony zarządzania.

    ![Nawigowanie do tymczasowej aplikacji sieci Web](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Nawigowanie do tymczasowej aplikacji sieci Web")

    *Przechodzenie do aplikacji tymczasowej*
7. Zwróć uwagę, że strona zarządzania będzie wyglądać podobnie do strony zarządzania inną aplikacją sieci Web. Przejdź do strony **wdrożenia** i skopiuj wartość **adres URL usługi git** . Będzie on używany później w tym ćwiczeniu.

    ![Kopiowanie wartości adresu URL usługi git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Kopiowanie wartości adresu URL usługi git*
8. Otwórz nową konsolę usługi **git bash** i wykonaj następujące polecenia. Zaktualizuj symbol zastępczy *[moja aplikacja-ścieżka]* ścieżką do rozwiązania **GeekQuiz** znajdującego się w folderze **Source\Ex1-DeployingWebSiteToStaging\Begin** w tym laboratorium.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicjalizacja git i pierwsze zatwierdzenie](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicjalizacja git i pierwsze zatwierdzenie*
9. Uruchom następujące polecenie, aby wypchnąć aplikację sieci Web do zdalnego repozytorium **git** . Zamień symbol zastępczy na adres URL uzyskany w portalu zarządzania. Zostanie wyświetlony monit o hasło wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Wypychanie do systemu Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Wypychanie do platformy Azure*

    > [!NOTE]
    > Podczas wdrażania zawartości na hoście FTP lub repozytorium GIT aplikacji sieci Web, należy uwierzytelnić się przy użyciu **poświadczeń wdrożenia** utworzonych na podstawie **Szybki Start** lub stron zarządzania **pulpitu nawigacyjnego** aplikacji sieci Web. Jeśli poświadczenia wdrożenia nie są znane, możesz je łatwo zresetować przy użyciu portalu zarządzania. Otwórz stronę **pulpit nawigacyjny** aplikacji sieci Web i kliknij link **Zresetuj poświadczenia wdrożenia** . Podaj nowe hasło i kliknij przycisk **OK**. Poświadczenia wdrożenia są prawidłowe do użycia ze wszystkimi aplikacjami sieci Web skojarzonymi z Twoją subskrypcją.
10. Aby sprawdzić, czy aplikacja sieci Web została pomyślnie przekazana do platformy Azure, Wróć do portalu zarządzania i kliknij pozycję **witryny sieci Web**.
11. Znajdź aplikację sieci Web i rozwiń wpis, aby wyświetlić miejsce lokacji tymczasowej. Kliknij jego **nazwę** , aby przejść do strony zarządzania.
12. Kliknij pozycję **wdrożenia** , aby wyświetlić **historię wdrożenia**. Sprawdź, czy istnieje **aktywne wdrożenie** z *&quot;&quot;wstępnego zatwierdzania* .

    ![Aktywne wdrożenie](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Aktywne wdrożenie*
13. Na koniec kliknij przycisk **Przeglądaj** na pasku poleceń, aby przejść do aplikacji sieci Web.

    ![Przeglądaj aplikację sieci Web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Przeglądaj aplikację sieci Web*
14. Jeśli aplikacja zostanie pomyślnie wdrożona, zostanie wyświetlona strona logowania quizu pojedynek maniaków komputerowych.

    > [!NOTE]
    > Adres URL wdrożonej aplikacji zawiera nazwę aplikacji sieci Web, a następnie *przemieszczenie*.

    ![Aplikacja uruchomiona w środowisku przejściowym](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplikacja uruchomiona w środowisku przejściowym*
15. Jeśli chcesz eksplorować aplikację, kliknij pozycję **zarejestruj** , aby zarejestrować nowego użytkownika. Aby uzyskać szczegółowe informacje o koncie, wprowadź nazwę użytkownika, adres e-mail i hasło. Następnie aplikacja pokazuje pierwsze pytanie quizu. Odpowiedz na kilka pytań, aby upewnić się, że działa zgodnie z oczekiwaniami.

    ![Aplikacja gotowa do użycia](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplikacja gotowa do użycia*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Zadanie 4 — promowanie aplikacji sieci Web do środowiska produkcyjnego

Po zweryfikowaniu, że aplikacja sieci Web działa prawidłowo w środowisku przejściowym, możesz przystąpić do promowania jej do produkcji. W tym zadaniu zastąpią miejsce lokacji tymczasowej miejscem produkcyjnym.

1. Wróć do portalu zarządzania i wybierz miejsce lokacji tymczasowej. Kliknij pozycję **Zamień** na pasku poleceń.

    ![Zamień na produkcję](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Zamień na produkcję*
2. Kliknij przycisk **tak** w oknie dialogowym potwierdzenia, aby kontynuować operację zamiany. Platforma Azure natychmiast zamienia zawartość witryny produkcyjnej na zawartość lokacji tymczasowej.

    > [!NOTE]
    > Niektóre ustawienia z wersji przemieszczanej zostaną automatycznie skopiowane do wersji produkcyjnej (np. zastąpienia parametrów połączenia, mapowania programu obsługi itp.), ale inne ustawienia nie zmienią się (np. punktów końcowych usługi DNS, powiązań SSL itp.).

    ![Potwierdzanie operacji wymiany](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Potwierdzanie operacji wymiany*
3. Po zakończeniu wymiany wybierz miejsce produkcyjne i kliknij przycisk **Przeglądaj** na pasku poleceń, aby otworzyć lokację produkcyjną. Zwróć uwagę na adres URL na pasku adresu.

    > [!NOTE]
    > Może być konieczne odświeżenie przeglądarki w celu wyczyszczenia pamięci podręcznej. W programie Internet Explorer można to zrobić przez naciśnięcie **klawiszy Ctrl + R**.

    ![Aplikacja sieci Web działająca w środowisku produkcyjnym](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. W konsoli **GitBash** zaktualizuj zdalny adres URL dla lokalnego repozytorium git, aby określić miejsce docelowe w miejscu produkcyjnym. W tym celu uruchom następujące polecenie, zastępując symbole zastępcze nazwą użytkownika wdrożenia i nazwą aplikacji sieci Web.

    > [!NOTE]
    > W poniższych ćwiczeniach nastąpi wypychanie zmian w witrynie produkcyjnej, a nie tylko na potrzeby uproszczenia laboratorium. W rzeczywistym scenariuszu zaleca się zweryfikowanie zmian w środowisku przejściowym przed podwyższeniem poziomu do produkcji.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Ćwiczenie 3: wykonywanie wycofywania wdrożenia w środowisku produkcyjnym

Istnieją scenariusze, w których nie ma miejsca przejściowego do przeprowadzenia gorącą wymianę między etapami przejściowymi i produkcyjnymi, na przykład w przypadku pracy z trybem **bezpłatnym** lub **udostępnionym** . W tych scenariuszach należy przetestować aplikację w środowisku testowym — lokalnie lub w lokacji zdalnej — przed wdrożeniem do produkcji. Jednak jest możliwe, że problem nie został wykryty podczas fazy testowania może wystąpić w lokacji produkcyjnej. W takim przypadku należy mieć mechanizm, który umożliwia łatwe przełączenie na poprzednią i bardziej stabilną wersję aplikacji tak szybko, jak to możliwe.

W **Azure App Service**ciągłe wdrażanie z kontroli źródła pozwala to na dostęp do akcji **ponownego wdrażania** w portalu zarządzania. Platforma Azure śledzi wdrożenia skojarzone z zatwierdzeń wypychanych do repozytorium i udostępnia opcję ponownego wdrażania aplikacji przy użyciu dowolnych z poprzednich wdrożeń w dowolnym momencie.

W tym ćwiczeniu wykonasz zmianę w kodzie w aplikacji **quizu pojedynek maniaków komputerowych** , która celowo wprowadza *usterkę*. Aplikacja zostanie wdrożona w środowisku produkcyjnym w celu wyświetlenia błędu, a następnie będzie można skorzystać z funkcji ponownego wdrażania, aby powrócić do poprzedniego stanu.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Zadanie 1 — aktualizowanie aplikacji quizu pojedynek maniaków komputerowych

W tym zadaniu zostanie Refaktoryzacja niewielki fragment kodu klasy **TriviaController** w celu wyodrębnienia części logiki pobierającej wybraną opcję quizu z bazy danych do nowej metody.

1. Przejdź do wystąpienia programu Visual Studio z rozwiązaniem **GeekQuiz** z poprzedniego ćwiczenia.
2. W **Eksplorator rozwiązań**otwórz plik **TriviaController.cs** wewnątrz folderu **controllers** .
3. Znajdź metodę **StoreAsync** i wybierz kod wyróżniony na poniższej ilustracji.

    ![Wybieranie kodu](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Wybieranie kodu*
4. Kliknij prawym przyciskiem myszy wybrany kod, rozwiń menu **refaktoryzacji** i wybierz polecenie **Wyodrębnij metodę.** ...

    ![Wyodrębnianie kodu jako nowej metody](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Wybieranie metody Extract*
5. W oknie dialogowym **Wyodrębnij metodę** Nazwij nową metodę *MatchesOption* , a następnie kliknij przycisk **OK**.

    ![Określanie nazwy metody](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Określanie nazwy wyodrębnionej metody*
6. Wybrany kod jest następnie wyodrębniany do metody **MatchesOption** . Kod wynikowy jest przedstawiony w poniższym fragmencie kodu.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Zadanie 2 — ponowne wdrażanie aplikacji quizu pojedynek maniaków komputerowych

Teraz można wypchnąć zmiany wprowadzone w poprzednim zadaniu do repozytorium, które wyzwoli nowe wdrożenie w środowisku produkcyjnym. Następnie Troubleshot problem przy użyciu **narzędzi programistycznych F12** dostarczonych przez program Internet Explorer, a następnie wykonaj wycofanie do poprzedniego wdrożenia z portalu zarządzania Azure.

1. Otwórz nową konsolę **bash usługi git** , aby wdrożyć zaktualizowaną aplikację do Azure App Service.
2. Wykonaj następujące polecenia, aby wypchnąć zmiany do platformy Azure. Zaktualizuj symbol zastępczy *[The-Application-path]* ze ścieżką do rozwiązania **GeekQuiz** . Zostanie wyświetlony monit o hasło wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Wypychanie refaktoryzacji kodu do platformy Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Wypychanie refaktoryzacji kodu do platformy Azure*
3. Otwórz program Internet Explorer i przejdź do aplikacji sieci Web (np. `http://<your-web-site>.azurewebsites.net`). Zaloguj się przy użyciu wcześniej utworzonych poświadczeń.
4. Naciśnij klawisz **F12** , aby uruchomić narzędzia programistyczne, wybierz kartę **Sieć** , a następnie kliknij przycisk **Odtwórz** , aby rozpocząć nagrywanie.

    ![Rozpoczynanie rejestrowania w sieci](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Rozpoczynanie rejestrowania w sieci")

    *Rozpoczynanie rejestrowania w sieci*
5. Wybierz dowolną opcję quizu. Zobaczysz, że nic się nie dzieje.
6. W oknie **F12** wpis odpowiadający żądaniu post protokołu HTTP pokazuje wynik http **500** .

    ![Błąd HTTP 500](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *Błąd HTTP 500*
7. Wybierz kartę **konsola** . Błąd jest rejestrowany ze szczegółowymi informacjami o przyczynie.

    ![Zarejestrowany błąd](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Zarejestrowany błąd*
8. Znajdź część szczegółów błędu. Jasno ten błąd jest spowodowany przez refaktoryzację kodu, który został przekazany w poprzednich krokach.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Nie zamykaj przeglądarki.
10. W nowym wystąpieniu przeglądarki przejdź do [portalu zarządzania systemu Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konto Microsoft skojarzonego z subskrypcją.
11. Wybierz pozycję **websites** i kliknij aplikację sieci Web utworzoną w ćwiczeniu 2.
12. Przejdź do strony **wdrożenia** . Zwróć uwagę, że wszystkie wykonane zatwierdzenia są wymienione w historii wdrażania.

    ![Lista istniejących wdrożeń](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Lista istniejących wdrożeń*
13. Wybierz poprzednie zatwierdzenie i kliknij pozycję **Wdróż** ponownie na pasku poleceń.

    ![Ponowne wdrażanie poprzedniego zatwierdzenia](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Ponowne wdrażanie poprzedniego zatwierdzenia*
14. Po wyświetleniu monitu o potwierdzenie kliknij przycisk **tak**.

    ![Potwierdzenie ponownego wdrożenia](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Po zakończeniu wdrożenia przejdź z powrotem do wystąpienia przeglądarki za pomocą aplikacji sieci Web i naciśnij **klawisze CTRL + F5**.
16. Kliknij dowolną z opcji. Przerzucanie animacji zostanie teraz wykonane i zostanie wyświetlony wynik (*poprawna/niepoprawna*).
17. Obowiązkowe Przejdź do konsoli **git bash** i wykonaj następujące polecenia, aby przywrócić poprzednie zatwierdzenie.

    > [!NOTE]
    > Te polecenia tworzą nowe zatwierdzenie, które wycofa wszystkie zmiany w repozytorium git, które zostały wprowadzone w nieprawidłowym zatwierdzeniu. Platforma Azure ponownie wdroży aplikację przy użyciu nowego zatwierdzenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Ćwiczenie 4: skalowanie przy użyciu usługi Azure Storage

**Obiekty blob** to najprostszy sposób przechowywania dużych ilości danych tekstowych lub binarnych bez struktury, takich jak wideo, audio i obrazy. Przeniesienie zawartości statycznej aplikacji do magazynu ułatwia skalowanie aplikacji przez obsługę obrazów lub dokumentów bezpośrednio w przeglądarce.

W tym ćwiczeniu zawartość statyczna aplikacji zostanie przeniesiona do kontenera obiektów BLOB. Następnie skonfigurujesz aplikację w taki sposób, aby dodać **regułę ponownego zapisywania adresów URL ASP.NET** w **pliku Web. config** w celu przekierowania zawartości do kontenera obiektów BLOB.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Zadanie 1 — Tworzenie konta usługi Azure Storage

W tym zadaniu dowiesz się, jak utworzyć nowe konto magazynu przy użyciu portalu zarządzania.

1. Przejdź do [portalu zarządzania platformy Azure](https://manage.windowsazure.com) i zaloguj się przy użyciu konto Microsoft skojarzonego z subskrypcją.
2. Wybierz pozycję **Nowy | Data Services | Magazyn | Szybkie tworzenie** , aby rozpocząć tworzenie nowego konta magazynu. Wprowadź unikatową nazwę konta i wybierz **region** z listy. Kliknij pozycję **Utwórz konto magazynu** , aby kontynuować.

    ![Tworzenie nowego konta magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Tworzenie nowego konta magazynu")

    *Tworzenie nowego konta magazynu*
3. W sekcji **Magazyn** poczekaj, aż stan nowego konta magazynu zmieni się na *online* , aby kontynuować pracę z następującym krokiem.

    ![Utworzono konto magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Utworzono konto magazynu")

    *Utworzono konto magazynu*
4. Kliknij nazwę konta magazynu, a następnie kliknij link **pulpitu nawigacyjnego** w górnej części strony. Na stronie **pulpit nawigacyjny** znajdują się informacje o stanie konta i punktach końcowych usługi, które mogą być używane w aplikacjach.

    ![Wyświetlanie pulpitu nawigacyjnego konta magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Wyświetlanie pulpitu nawigacyjnego konta magazynu")

    *Wyświetlanie pulpitu nawigacyjnego konta magazynu*
5. Kliknij przycisk **Zarządzaj kluczami dostępu** na pasku nawigacyjnym.

    ![Przycisk Zarządzaj kluczami dostępu](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Przycisk Zarządzaj kluczami dostępu")

    *Przycisk Zarządzaj kluczami dostępu*
6. W oknie dialogowym **Zarządzanie kluczami dostępu** Skopiuj **nazwę konta magazynu** i **podstawowy klucz dostępu** , ponieważ będą one potrzebne w poniższym ćwiczeniu. Następnie zamknij okno dialogowe.

    ![Okno dialogowe Zarządzanie kluczem dostępu](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Okno dialogowe Zarządzanie kluczem dostępu")

    *Okno dialogowe Zarządzanie kluczem dostępu*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Zadanie 2 — przekazywanie elementu zawartości do usługi Azure Blob Storage

W tym zadaniu użyjesz okna Eksplorator serwera z programu Visual Studio, aby nawiązać połączenie z kontem magazynu. Następnie utworzysz kontener obiektów blob i przekażesz plik z logo quizu pojedynek maniaków komputerowych do kontenera.

1. Przejdź do wystąpienia programu Visual Studio z rozwiązaniem **GeekQuiz** z poprzedniego ćwiczenia.
2. Na pasku menu wybierz pozycję **Widok** , a następnie kliknij pozycję **Eksplorator serwera**.
3. W **Eksplorator serwera**kliknij prawym przyciskiem myszy węzeł **platformy Azure** i wybierz pozycję **Połącz z platformą Azure...** . Zaloguj się przy użyciu konto Microsoft skojarzonego z subskrypcją.

    ![Nawiązywanie połączenia z systemem Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Łączenie się z platformą Azure*
4. Rozwiń węzeł **Azure** , kliknij prawym przyciskiem myszy pozycję **Magazyn** , a następnie wybierz pozycję **Dołącz magazyn zewnętrzny..** ..
5. W oknie dialogowym **Dodawanie nowego konta magazynu** wprowadź **nazwę konta** i **klucz konta** uzyskany w poprzednim zadaniu, a następnie kliknij przycisk **OK**.

    ![Okno dialogowe Dodawanie nowego konta magazynu](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Okno dialogowe Dodawanie nowego konta magazynu*
6. Konto magazynu powinno pojawić się w węźle **Magazyn** . Rozwiń konto magazynu, kliknij prawym przyciskiem myszy pozycję **obiekty blob** i wybierz pozycję **Utwórz kontener obiektów BLOB..** ..

    ![Utwórz kontener obiektów BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Utwórz kontener obiektów BLOB")

    *Utwórz kontener obiektów BLOB*
7. W oknie dialogowym **Tworzenie kontenera obiektów BLOB** wprowadź nazwę kontenera obiektów blob, a następnie kliknij przycisk **OK**.

    ![Utwórz kontener obiektów BLOB — okno dialogowe](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Utwórz kontener obiektów BLOB — okno dialogowe")

    *Utwórz kontener obiektów BLOB — okno dialogowe*
8. Nowy kontener obiektów BLOB należy dodać do węzła **obiektów BLOB** . Zmień uprawnienia dostępu w kontenerze, aby utworzyć kontener jako publiczny. Aby to zrobić, kliknij prawym przyciskiem myszy kontener **obrazy** i wybierz polecenie **Właściwości**.

    ![właściwości kontenera obrazów](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "właściwości kontenera obrazów")

    *Właściwości kontenera obrazów*
9. W oknie **Właściwości** Ustaw **publiczny dostęp do odczytu** do **kontenera**.

    ![Zmiana właściwości publicznego dostępu do odczytu](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Zmiana właściwości publicznego dostępu do odczytu")

    *Zmiana właściwości publicznego dostępu do odczytu*
10. Po wyświetleniu monitu, jeśli na pewno chcesz zmienić właściwość dostępu publicznego, kliknij przycisk **tak**.

    ![Ostrzeżenie Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Ostrzeżenie Microsoft Visual Studio")

    *Ostrzeżenie Microsoft Visual Studio*
11. W **Eksplorator serwera**kliknij prawym przyciskiem myszy kontener obiektów BLOB **obrazów** i wybierz pozycję **Wyświetl kontener obiektów BLOB**.

    ![Wyświetlanie kontenera obiektów BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Wyświetlanie kontenera obiektów BLOB")

    *Wyświetlanie kontenera obiektów BLOB*
12. Kontener obrazów powinien zostać otwarty w nowym oknie, a legenda bez wpisów powinna być pokazywana. Kliknij ikonę **Przekaż** , aby przekazać plik do kontenera obiektów BLOB.

    ![Kontener obrazów bez wpisów](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Kontener obrazów bez wpisów")

    *Kontener obrazów bez wpisów*
13. W oknie dialogowym **przekazywanie obiektu BLOB** przejdź do folderu **Assets** w środowisku laboratoryjnym. Wybierz plik **logo-Big. png** , a następnie kliknij przycisk **Otwórz**.
14. Poczekaj, aż plik zostanie przekazany. Po zakończeniu przekazywania plik powinien znajdować się w kontenerze obrazy. Kliknij prawym przyciskiem myszy wpis pliku i wybierz polecenie **Kopiuj adres URL**.

    ![Kopiuj adres URL obiektu BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Kopiuj adres URL pliku BLOB")

    *Kopiuj adres URL obiektu BLOB*
15. Otwórz program Internet Explorer i wklej adres URL. W przeglądarce powinien zostać wyświetlony następujący obraz.

    ![Obraz logo-Big. png z systemu Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-Big. png — obraz z magazynu")

    *Obraz logo-Big. png z usługi Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Zadanie 3 — Aktualizowanie rozwiązania do korzystania z zawartości statycznej z usługi Azure Blob Storage

W tym zadaniu skonfigurujesz rozwiązanie **GeekQuiz** , aby korzystało z obrazu przekazanego do platformy Azure Blob Storage (zamiast obrazu znajdującego się w aplikacji sieci Web) przez dodanie reguły ponownego zapisu ASP.net URL w pliku **Web. config** .

1. W programie Visual Studio Otwórz plik **Web. config** w projekcie **GeekQuiz** i Znajdź element **&lt;system. WebServer&gt;** .
2. Dodaj następujący kod, aby dodać regułę ponownego zapisywania adresu URL, aktualizując symbol zastępczy przy użyciu nazwy konta magazynu.

    (Fragment kodu- *WebSitesInProduction-EX4-UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Ponowne zapisywanie adresów URL to proces przechwytywania przychodzącego żądania sieci Web i przekierowania żądania do innego zasobu. Reguły zapisywania adresów URL instruują aparat ponownego zapisywania, gdy żądanie musi zostać przekierowane, i gdzie należy je przekierowywać. Reguła ponownego zapisywania składa się z dwóch ciągów: wzorca, który ma być wyszukiwany w żądanym adresie URL (zazwyczaj przy użyciu wyrażeń regularnych), oraz ciągu, w którym ma zostać zamieniony wzorzec, jeśli znaleziono. Aby uzyskać więcej informacji, zobacz Ponowne [Zapisywanie adresów URL w ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Naciśnij **kombinację klawiszy Ctrl + S** , aby zapisać zmiany.
4. Otwórz nową konsolę **bash usługi git** , aby wdrożyć zaktualizowaną aplikację do Azure App Service.
5. Wykonaj następujące polecenia, aby wypchnąć zmiany do platformy Azure. Zaktualizuj symbol zastępczy *[The-Application-path]* ze ścieżką do rozwiązania **GeekQuiz** . Zostanie wyświetlony monit o hasło wdrożenia.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Wdrażanie aktualizacji na platformie Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Wdrażanie aktualizacji na platformie Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Zadanie 4 — weryfikacja

W tym zadaniu będziesz używać programu **Internet Explorer** do przeglądania aplikacji **quizu pojedynek maniaków komputerowych** i sprawdzenia, czy reguła ponownego zapisywania adresów URL dla obrazów działa i że nastąpi przekierowanie do obrazu hostowanego na **platformie Azure Blob Storage**.

1. Otwórz program Internet Explorer i przejdź do aplikacji sieci Web (np. `http://<your-web-site>.azurewebsites.net`). Zaloguj się przy użyciu wcześniej utworzonych poświadczeń.

    ![Wyświetlanie aplikacji sieci Web quizu pojedynek maniaków komputerowych z obrazem](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Wyświetlanie aplikacji sieci Web quizu pojedynek maniaków komputerowych z obrazem")

    *Wyświetlanie aplikacji sieci Web quizu pojedynek maniaków komputerowych z obrazem*
2. Naciśnij klawisz **F12** , aby uruchomić narzędzia programistyczne, wybierz kartę **Sieć** i Rozpocznij nagrywanie.

    ![Rozpoczynanie rejestrowania w sieci](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Rozpoczynanie rejestrowania w sieci")

    *Rozpoczynanie rejestrowania w sieci*
3. Naciśnij **kombinację klawiszy CTRL + F5** , aby odświeżyć stronę sieci Web.
4. Po zakończeniu ładowania strony powinno zostać wyświetlone żądanie HTTP dla adresu URL **/IMG/logo-Big.png** z wynikiem http **301** (redirect) i innym żądaniem dla `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` adres url z wynikiem http **200** .

    ![Weryfikowanie przekierowania adresu URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Wyświetlanie przekierowania w narzędziach deweloperskich")

    *Weryfikowanie przekierowania adresu URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Ćwiczenie 5. Używanie skalowania automatycznego dla Web Apps

> [!NOTE]
> To ćwiczenie jest opcjonalne, ponieważ wymaga obsługi testowania wydajności &amp; sieci Web, który jest dostępny tylko dla **Visual Studio 2013 Ultimate Edition**. Aby uzyskać więcej informacji na temat określonych funkcji Visual Studio 2013, należy porównać wersje w [tym miejscu](https://www.microsoft.com/visualstudio/eng/products/compare).

**Azure App Service Web Apps** udostępnia funkcję skalowania automatycznego dla aplikacji sieci Web działających w **trybie standardowym**. Funkcja automatycznego skalowania umożliwia platformie Azure automatyczne skalowanie liczby wystąpień aplikacji sieci Web w zależności od obciążenia. Gdy automatyczne skalowanie jest włączone, platforma Azure sprawdza procesor aplikacji sieci Web co pięć minut i dodaje wystąpienia w miarę potrzeb w danym momencie. Jeśli użycie procesora CPU jest niskie, platforma Azure usunie wystąpienia co dwie godziny, aby upewnić się, że wydajność aplikacji sieci Web nie została obniżona.

W tym ćwiczeniu przedstawiono kroki wymagane do skonfigurowania funkcji **automatycznego skalowania** dla aplikacji sieci Web **pojedynek maniaków komputerowych Quiz** . Tę funkcję należy zweryfikować, uruchamiając test obciążenia programu Visual Studio w celu wygenerowania wystarczającego obciążenia procesora w aplikacji w celu wyzwolenia uaktualnienia wystąpienia.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Zadanie 1 — Konfigurowanie automatycznego skalowania na podstawie metryki procesora

W tym zadaniu zostanie użyty Portal zarządzania Azure, aby włączyć funkcję skalowania automatycznego dla aplikacji sieci Web utworzonej w ćwiczeniu 2.

1. W [portalu zarządzania platformy Azure](https://manage.windowsazure.com/)wybierz pozycję **witryny sieci** Web, a następnie kliknij aplikację internetową utworzoną w ćwiczeniu 2.
2. Przejdź do strony **skalowanie** . W obszarze **pojemność** wybierz opcję **procesor CPU** dla konfiguracji **skalowania według metryk** .

    > [!NOTE]
    > Przy skalowaniu według procesora, platforma Azure dynamicznie dostosowuje liczbę wystąpień używanych przez aplikację w przypadku zmiany użycia procesora CPU.

    ![Wybieranie do skalowania według procesora CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Wybieranie metryki procesora CPU na potrzeby automatycznego skalowania")

    *Wybieranie do skalowania według procesora CPU*
3. Zmień **docelową konfigurację procesora** na **20**-**40** procent.

    > [!NOTE]
    > Ten zakres reprezentuje średnie użycie procesora dla aplikacji sieci Web. Platforma Azure doda lub usunie wystąpienia, aby zachować swoją aplikację internetową w tym zakresie. Minimalna i Maksymalna liczba wystąpień używanych do skalowania jest określona w konfiguracji **liczby wystąpień** . Platforma Azure nigdy nie będzie przekroczyć tego limitu.
    >
    > Domyślne **docelowe wartości procesora** są modyfikowane tylko w celach tego laboratorium. Konfigurując zakres procesorów przy użyciu małych wartości, zwiększasz prawdopodobieństwo wyzwalania automatycznego skalowania w przypadku umieszczenia w aplikacji umiarkowanego obciążenia.

    ![Zmiana docelowego procesora CPU na wartość z zakresu od 20 do 40%](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Zmiana docelowego procesora CPU na wartość z zakresu od 20 do 40%")

    *Zmiana docelowego procesora CPU na wartość z zakresu od 20 do 40%*
4. Kliknij przycisk **Zapisz** na pasku poleceń, aby zapisać zmiany.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Zadanie 2 — testowanie obciążenia za pomocą programu Visual Studio

Teraz po skonfigurowaniu **automatycznego skalowania** utworzysz **projekt testu wydajności i obciążenia sieci Web** w programie Visual Studio, aby wygenerować obciążenie procesora CPU w aplikacji sieci Web.

1. Otwórz **Visual Studio Ultimate 2013** i wybierz pozycję **plik | Nowy | Projekt...** , aby rozpocząć nowe rozwiązanie.

    ![Tworzenie nowego projektu](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Tworzenie nowego projektu")

    *Tworzenie nowego projektu*
2. W oknie dialogowym **Nowy projekt** wybierz **projekt test wydajności i obciążenia sieci Web** w obszarze **Wizualizacja C# | Karta test** . Upewnij się, że **.NET Framework 4,5** jest zaznaczone, nazwij *projekt WebAndLoadTestProject*, wybierz **lokalizację** i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu testu sieci Web i obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Tworzenie nowego projektu testu sieci Web i obciążenia")

    *Tworzenie nowego projektu testu sieci Web i obciążenia*
3. W **WebTest1. webtest** kliknij prawym przyciskiem myszy węzeł **WebTest1** i kliknij polecenie **Dodaj żądanie**.

    ![Dodawanie żądania do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Dodawanie żądania do WebTest1")

    *Dodawanie żądania do WebTest1*
4. W oknie **Właściwości** nowego węzła żądania zaktualizuj właściwość **adresu URL** , aby wskazywała adres URL aplikacji sieci web (np. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Zmiana właściwości adresu URL](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Zmiana właściwości adresu URL")

    *Zmiana właściwości adresu URL*
5. W oknie **WebTest1. webtest** kliknij prawym przyciskiem myszy pozycję **WebTest1** , a następnie kliknij pozycję **Dodaj pętlę...** .

    ![Dodawanie pętli do WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Dodawanie pętli do WebTest1")

    *Dodawanie pętli do WebTest1*
6. W oknie dialogowym **Dodaj regułę warunkową i elementy do pętli** wybierz regułę **pętli for** i zmodyfikuj następujące właściwości.

   1. **Wartość zakończenia:** 1000
   2. **Nazwa parametru kontekstu:** Iterator
   3. **Wartość przyrostu:** 1

      ![Wybieranie reguły pętli for i aktualizowanie właściwości](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Wybieranie reguły pętli for i aktualizowanie właściwości")

      *Wybieranie reguły pętli for i aktualizowanie właściwości*
7. W sekcji **elementy w pętli** wybierz utworzone wcześniej żądanie jako pierwszy i ostatni element dla pętli. Kliknij przycisk **OK** , aby kontynuować.

    ![Wybieranie pierwszych i ostatnich elementów dla pętli](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Wybieranie pierwszych i ostatnich elementów dla pętli")

    *Wybieranie pierwszych i ostatnich elementów dla pętli*
8. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **WebAndLoadTestProject** , rozwiń menu **Dodaj** i wybierz polecenie **test obciążenia.** ...

    ![Dodawanie testu obciążenia do projektu WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Dodawanie testu obciążenia do projektu WebAndLoadTestProject")

    *Dodawanie testu obciążenia do projektu WebAndLoadTestProject*
9. W oknie dialogowym **nowy Kreator testu obciążeniowego** kliknij przycisk **dalej**.

    ![Nowe Kreator testu obciążeniowego](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Nowe Kreator testu obciążeniowego")

    *Nowe Kreator testu obciążeniowego*
10. Na stronie **scenariusz** wybierz opcję nie **Używaj czasów reakcji** i kliknij przycisk **dalej**.

    ![Wybieranie, aby nie używać czasów reakcji](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Wybieranie, aby nie używać czasów reakcji")

    *Wybieranie, aby nie używać czasów reakcji*
11. Na stronie **wzorzec obciążenia** upewnij się, że opcja **ładowania stałego** jest zaznaczona. Zmień ustawienie **Liczba użytkowników** na **250** , a następnie kliknij przycisk **dalej**.

    ![Zmiana liczby użytkowników na 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Zmiana liczby użytkowników na 250")

    *Zmiana liczby użytkowników na 250*
12. Na stronie **model testu mieszanego** wybierz pozycję **na podstawie sekwencyjnej kolejności testów** i kliknij przycisk **dalej**.

    ![Wybieranie modelu testu mieszanego](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Wybieranie modelu testu mieszanego")

    *Wybieranie modelu testu mieszanego*
13. Na stronie **model testu mieszanego** kliknij przycisk **Dodaj...** , aby dodać test do kombinacji.

    ![Dodawanie testu do testu mieszanego](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Dodawanie testu do testu mieszanego")

    *Dodawanie testu do testu mieszanego*
14. W oknie dialogowym **Dodawanie testów** kliknij dwukrotnie pozycję **WebTest1** , aby dodać test do listy **Wybrane testy** . Kliknij przycisk **OK** , aby kontynuować.

    ![Dodawanie testu WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Dodawanie testu WebTest1")

    *Dodawanie testu WebTest1*
15. Na stronie **test mix** kliknij przycisk **dalej**.

    ![Ukończenie testu mieszanego](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Ukończenie testu mieszanego")

    *Ukończenie testu mieszanego*
16. Na stronie **mieszanie sieci** kliknij przycisk **dalej**.

    ![Kliknięcie przycisku Dalej na stronie mieszanie sieci](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Kliknięcie przycisku Dalej na stronie mieszanie sieci")

    *Kliknięcie przycisku Dalej na stronie mieszanie sieci*
17. Na stronie **mieszanie przeglądarki** wybierz opcję **Internet Explorer 10,0** jako typ przeglądarki, a następnie kliknij przycisk **dalej**.

    ![Wybieranie typu przeglądarki](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Wybieranie typu przeglądarki")

    *Wybieranie typu przeglądarki*
18. Na stronie **zbiory liczników** kliknij przycisk **dalej**.

    ![Kliknięcie przycisku Dalej na stronie zbiory liczników](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Kliknięcie przycisku Dalej na stronie zbiory liczników")

    *Kliknięcie przycisku Dalej na stronie zbiory liczników*
19. Na stronie **Ustawienia uruchomieniowe** Ustaw **czas trwania testu obciążenia** na **5 minut** , a następnie kliknij przycisk **Zakończ**.

    ![Ustawianie czasu trwania testu obciążenia na 5 minut](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Ustawianie czasu trwania testu obciążenia na 5 minut")

    *Ustawianie czasu trwania testu obciążenia na 5 minut*
20. W **Eksplorator rozwiązań**kliknij dwukrotnie plik **Local. Settings** , aby poznać ustawienia testu. Domyślnie program Visual Studio używa komputera lokalnego do uruchamiania testów.

    > [!NOTE]
    > Alternatywnie można skonfigurować projekt testowy do uruchamiania testów obciążenia w chmurze przy użyciu **Azure test Plans**. Azure Test Plans zapewnia usługę testowania obciążenia opartego na chmurze, która symuluje bardziej realistyczne obciążenie, unikając ograniczeń środowiska lokalnego, takich jak pojemność procesora CPU, dostępna pamięć i przepustowość sieci. Aby uzyskać więcej informacji na temat używania Azure Test Plans do uruchamiania testów obciążenia, zobacz [scenariusze testowania obciążenia](/azure/devops/test/load-test/overview?view=vsts).

    ![Ustawienia testu](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Zadanie 3 — weryfikacja skalowania automatycznego

Teraz wykonasz test obciążenia utworzony w poprzednim zadaniu i zobaczysz, jak Twoja aplikacja internetowa zachowuje się pod obciążeniem.

1. W **Eksplorator rozwiązań**kliknij dwukrotnie pozycję **LoadTest1. LoadTest** , aby otworzyć test obciążenia.

    ![Otwieranie LoadTest1. LoadTest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Otwieranie LoadTest1. LoadTest")

    *Otwieranie LoadTest1. LoadTest*
2. W oknie **LoadTest1. LoadTest** kliknij pierwszy przycisk w przyborniku, aby uruchomić test obciążenia.

    ![Uruchamianie testu obciążenia](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Uruchamianie testu obciążenia")

    *Uruchamianie testu obciążenia*
3. Poczekaj na zakończenie testu obciążenia.

    > [!NOTE]
    > Test obciążenia symuluje wielu użytkowników, którzy jednocześnie wysyłają żądania do aplikacji sieci Web. Gdy test jest uruchomiony, można monitorować dostępne liczniki w celu wykrywania wszelkich błędów, ostrzeżeń lub innych informacji dotyczących przebiegu testu obciążenia.

    ![Test obciążenia jest uruchomiony](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Oczekiwanie na zakończenie testu obciążenia")

    *Test obciążenia jest uruchomiony*
4. Po zakończeniu testu Wróć do portalu zarządzania i przejdź do strony **skalowanie** aplikacji sieci Web. W sekcji **pojemność** powinna zostać wyświetlona na grafie, że nowe wystąpienie zostało automatycznie wdrożone.

    ![Nowe wystąpienie automatycznie wdrożone](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nowe wystąpienie automatycznie wdrożone*

    > [!NOTE]
    > Aby zmiany pojawiły się na wykresie, może upłynąć kilka minut (okresowo naciśnij **kombinację klawiszy CTRL + F5** , aby odświeżyć stronę). Jeśli nie widzisz żadnych zmian, możesz spróbować wykonać następujące czynności:
    >
    > - Zwiększ czas trwania testu obciążenia (np. do **10 minut**)
    > - Zmniejsz maksymalne i minimalne wartości **docelowego zakresu procesora CPU** w konfiguracji automatycznego skalowania aplikacji sieci Web
    > - Uruchom test obciążenia w chmurze za pomocą **Azure test Plans**. Więcej informacji [znajdziesz tutaj](/azure/devops/test/load-test/index?view=vsts)

---

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym ćwiczeniu dowiesz się, jak skonfigurować i wdrożyć aplikację do produkcyjnych aplikacji sieci Web na platformie Azure. Rozpoczęto od wykrywania i aktualizowania baz danych przy użyciu **migracje Code First platformy Entity Framework**, a następnie kontynuowania przez wdrożenie nowych wersji witryny przy użyciu **narzędzia Git** i przeprowadzenie wycofywania do najnowszej stabilnej wersji lokacji. Ponadto wiesz już, jak skalować aplikację przy użyciu usługi Storage, aby przenieść zawartość statyczną do kontenera obiektów BLOB.

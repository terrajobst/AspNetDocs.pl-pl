---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework tworzenia szkieletów i migracji | Microsoft Docs
author: rick-anderson
description: Jeśli znasz metody kontrolera ASP.NET MVC 4 lub zakończył &quot;pomocnicy, formularze i sprawdzanie poprawności&quot; laboratorium, należy pamiętać o tym...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598904"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 — tworzenie szkieletu i migracje platformy Entity Framework

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

[Pobierz zestaw szkoleniowy dla sieci Web Camp](https://aka.ms/webcamps-training-kit)

Jeśli znasz metody kontrolera ASP.NET MVC 4 lub zakończono &quot;pomocników, formularzy i walidacji&quot; laboratorium, należy pamiętać, że wiele logiki umożliwia tworzenie, aktualizowanie, wyświetlanie i usuwanie wszystkich jednostek danych, które są powtarzane między aplikacjami. Aby nie wspominać, że jeśli model zawiera kilka klas do manipulowania, prawdopodobnie poświęcasz dużo czasu na pisanie metod POST i GET akcji dla każdej operacji jednostki, a także każdego z widoków.

W tym laboratorium dowiesz się, jak używać szkieletu ASP.NET MVC 4 do automatycznego generowania linii bazowej aplikacji CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie). Rozpoczynając od prostej klasy modelu i bez pisania pojedynczego wiersza kodu, utworzysz kontroler, który będzie zawierać wszystkie operacje CRUD, a także wszystkie niezbędne widoki. Po skompilowaniu i uruchomieniu prostego rozwiązania zostanie wygenerowana baza danych aplikacji wraz z logiką MVC i widokami służącymi do manipulowania danymi.

Ponadto dowiesz się, jak łatwo można używać migracji Entity Framework, aby wykonywać aktualizacje modelu w całej aplikacji. Entity Framework migracji umożliwią zmodyfikowanie bazy danych po zmianie modelu z prostymi krokami. Ze względu na to, że będziesz mieć możliwość bardziej wydajnego kompilowania i konserwowania aplikacji sieci Web, korzystając z najnowszych funkcji ASP.NET MVC 4.

> [!NOTE]
> Wszystkie przykładowe kod i fragmenty kodu są dołączone do zestawu szkoleniowego dla sieci Web Camp, dostępnego w [wersji Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzny dla tego laboratorium jest dostępny w witrynie [ASP.NET MVC 4 Entity Framework tworzenia szkieletów i migracji](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Używanie szkieletu ASP.NET dla operacji CRUD na kontrolerach.
- Zmień model bazy danych za pomocą migracji Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku B: Using fragmenty kodu](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

Następujące czynności sprawiają, że ćwiczenia te są wykonywane w tym ćwiczeniu:

1. [Używanie szkieletu ASP.NET MVC 4 z migracją Entity Framework](#Exercise1)

> [!NOTE]
> Do tego ćwiczenia dołączany jest folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenie.

Szacowany czas wykonywania tego laboratorium: **30 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Ćwiczenie 1: używanie szkieletu ASP.NET MVC 4 z migracją Entity Framework

Funkcja tworzenia szkieletów ASP.NET MVC umożliwia szybkie generowanie operacji CRUD w ustandaryzowany sposób, co pozwala na współdziałanie aplikacji z warstwą bazy danych.

W tym ćwiczeniu dowiesz się, jak utworzyć metody CRUD przy użyciu szkieletu ASP.NET MVC 4 z kodem. Następnie dowiesz się, jak zaktualizować model, stosując zmiany z bazy danych za pomocą migracji Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Zadanie 1 — Tworzenie nowego projektu ASP.NET MVC 4 przy użyciu szkieletu

1. Jeśli nie jest jeszcze otwarty, uruchom **program Visual Studio 2012**.
2. Wybierz **plik | Nowy projekt**. W oknie dialogowym Nowy projekt w obszarze **Wizualizacja C# | Sekcja sieci Web** , wybierz opcję **aplikacja sieci Web ASP.NET MVC 4**. Nadaj projektowi nazwę **MVC4andEFMigrations** i Ustaw lokalizację do folderu **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** w tym laboratorium. Ustaw **nazwę rozwiązania** na **Rozpocznij** i upewnij się, że jest zaznaczone pole wyboru **Utwórz katalog dla rozwiązania** . Kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Okno dialogowe Nowy projekt ASP.NET MVC 4")

    *Okno dialogowe Nowy projekt ASP.NET MVC 4*
3. W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz szablon **aplikacja internetowa** i upewnij się, że **Razor** jest wybrany **aparat widoku**. Kliknij przycisk **OK**, aby utworzyć projekt.

    ![Nowa aplikacja internetowa ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nowa aplikacja internetowa ASP.NET MVC 4")

    *Nowa aplikacja internetowa ASP.NET MVC 4*
4. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy pozycję **modele** i wybierz polecenie **Dodaj | Klasa** do utworzenia prostej osoby klasy (POCO). Nadaj nazwę **osobie** IT i kliknij przycisk **OK**.
5. Otwórz klasę Person i Wstaw następujące właściwości.

    (Fragment kodu — *ASP.NET i migracji Entity Framework MVC 4 — Ex1 właściwości osoby*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Kliknij pozycję **kompilacja | Skompiluj rozwiązanie** , aby zapisać zmiany i skompilować projekt.

    ![Kompilowanie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Kompilowanie aplikacji")

    *Kompilowanie aplikacji*
7. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz pozycję **Dodaj | Kontroler**.
8. Nadaj kontrolerowi nazwę *PersonController* i wypełnij **Opcje tworzenia szkieletu** , korzystając z następujących wartości.

   1. Z listy rozwijanej **szablon** wybierz **kontroler MVC z akcjami odczytu/zapisu i widokami, używając opcji Entity Framework** .
   2. Z listy rozwijanej **Klasa modelu** wybierz klasę **Person** .
   3. Na liście **Klasa kontekstu danych** wybierz opcję **&lt;nowy kontekst danych...&gt;** . Wybierz dowolną nazwę, a następnie kliknij przycisk **OK**.
   4. Upewnij się, że na liście rozwijanej **widoki** została wybrana wartość **Razor** .

      ![Dodawanie kontrolera osoby za pomocą szkieletu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Dodawanie kontrolera osoby za pomocą szkieletu")

      *Dodawanie kontrolera osoby za pomocą szkieletu*
9. Kliknij przycisk **Dodaj** , aby utworzyć nowy kontroler dla osoby z szkieletem. Teraz zostały wygenerowane akcje kontrolera oraz widoki.

    ![Po utworzeniu kontrolera osoby przy użyciu szkieletu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Po utworzeniu kontrolera osoby przy użyciu szkieletu")

    *Po utworzeniu kontrolera osoby przy użyciu szkieletu*
10. Otwórz klasę **PersonController** . Zauważ, że wszystkie metody akcji CRUD zostały wygenerowane automatycznie.

   ![Wewnątrz kontrolera osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Wewnątrz kontrolera osoby")

   *Wewnątrz kontrolera osoby*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Zadanie 2 — Uruchamianie aplikacji

W tym momencie baza danych nie została jeszcze utworzona. W tym zadaniu uruchomisz aplikację po raz pierwszy i przetestujesz operacje CRUD. Baza danych zostanie utworzona na bieżąco z Code First.

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.
2. W przeglądarce Dodaj **/Person** do adresu URL, aby otworzyć stronę osoby.

    ![Aplikacja pierwszego uruchomienia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Aplikacja pierwszego uruchomienia")

    *Aplikacja: pierwsze uruchomienie*
3. Zobaczysz teraz strony osoby i przetestujesz operacje CRUD.

    1. Kliknij przycisk **Utwórz nowy** , aby dodać nową osobę. Wprowadź imię i nazwisko, a następnie kliknij przycisk **Utwórz**.

        ![Dodawanie nowej osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Dodawanie nowej osoby")

        *Dodawanie nowej osoby*
    2. Na liście osób można usuwać, edytować lub dodawać elementy.

        ![Lista osób](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "Lista osób")

        *Lista osób*
    3. Kliknij przycisk **szczegóły** , aby otworzyć Szczegóły osoby.

        ![Szczegóły osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Szczegóły osoby")

        *Szczegóły osoby*
4. Zamknij przeglądarkę i wróć do programu Visual Studio. Zwróć uwagę, że utworzono całą CRUD dla podmiotu osoby w całej aplikacji — od modelu do widoków — bez konieczności pisania pojedynczego wiersza kodu.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Zadanie 3 — Aktualizowanie bazy danych za pomocą migracji Entity Framework

W tym zadaniu zostanie zaktualizowana baza danych przy użyciu Entity Framework migracji. Zobaczysz, jak łatwo można zmienić model i odzwierciedlić zmiany w bazach danych za pomocą funkcji migracji Entity Framework.

1. Otwórz konsolę Menedżera pakietów. Wybierz kolejno pozycje **narzędzia** > **menedżer pakietów NuGet** > **konsola Menedżera pakietów**.
2. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Włączanie migracji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Włączanie migracji")

    *Włączanie migracji*

    Polecenie Enable-Migration tworzy folder **migracji** , który zawiera skrypt umożliwiający zainicjowanie bazy danych.

    ![Folder migracji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Folder migracji")

    *Folder migracji*
3. Otwórz plik **Configuration.cs** w folderze migrations. Znajdź konstruktora klasy i zmień wartość **AutomaticMigrationsEnabled** na *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Otwórz klasę Person i Dodaj atrybut do nazwy środkowej osoby. Ten nowy atrybut zmienia model.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Wybierz **kompilację | Kompiluj rozwiązanie** w menu, aby skompilować aplikację.

    ![Kompilowanie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Kompilowanie aplikacji")

    *Kompilowanie aplikacji*
6. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    To polecenie spowoduje przeszukanie zmian w obiektach danych, a następnie doda odpowiednie polecenia, aby odpowiednio zmodyfikować bazę danych.

    ![Dodawanie nazwy środkowej](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Dodawanie nazwy środkowej")

    *Dodawanie nazwy środkowej*
7. Obowiązkowe Można uruchomić następujące polecenie, aby wygenerować skrypt SQL z aktualizacją różnicową. Pozwoli to na aktualizację bazy danych ręcznie (w tym przypadku nie jest to konieczne) lub zastosowanie zmian w innych bazach danych:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generowanie skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generowanie skryptu SQL")

    *Generowanie skryptu SQL*

    ![Aktualizacja skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Aktualizacja skryptu SQL")

    *Aktualizacja skryptu SQL*
8. W konsoli Menedżera pakietów wprowadź następujące polecenie, aby zaktualizować bazę danych:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aktualizowanie bazy danych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizowanie bazy danych")

    *Aktualizowanie bazy danych*

    Spowoduje to dodanie kolumny **MiddleName** w tabeli **osób** w celu dopasowania jej do bieżącej definicji klasy **Person** .
9. Gdy baza danych zostanie zaktualizowana, kliknij prawym przyciskiem myszy folder kontroler i wybierz polecenie **Dodaj | Kontroler** , aby ponownie dodać kontrolera osoby (wykonaj te same wartości). Spowoduje to zaktualizowanie istniejących metod i widoków dodając nowy atrybut.

    ![Dodawanie aktualizacji kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Dodawanie aktualizacji kontrolera")

    *Aktualizowanie kontrolera*
10. Kliknij pozycję **Add** (Dodaj). Następnie wybierz wartości **Zastąp PersonController.cs** i **Zastąp powiązane widoki** , a następnie kliknij przycisk **OK**.

   ![Dodawanie zastąpienia kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Aktualizowanie kontrolera*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 — uruchamianie aplikacji

1. Naciśnij klawisz **F5**, aby uruchomić aplikację.
2. Otwórz **/Person**. Zwróć uwagę, że dane zostały zachowane, podczas gdy została dodana kolumna nazwy środkowej.

    ![Dodano środkową nazwę](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Dodano środkową nazwę")

    *Dodano środkową nazwę*
3. Jeśli klikniesz pozycję **Edytuj**, będziesz mieć możliwość dodania nazwy Środkowej do bieżącej osoby.

    ![Wersja środkowa](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Wersja środkowa")

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym ćwiczeniu dowiesz się, jak tworzyć operacje CRUD z użyciem szkieletu ASP.NET MVC 4 przy użyciu dowolnej klasy modelu. Następnie wiesz już, jak wykonać aktualizację kompleksową w aplikacji — z bazy danych do widoków — za pomocą migracji Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do obszaru [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169) (Ustawienia — Integracje i usługi). Alternatywnie, jeśli zainstalowano już Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Kafelek VS Express for Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
2. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

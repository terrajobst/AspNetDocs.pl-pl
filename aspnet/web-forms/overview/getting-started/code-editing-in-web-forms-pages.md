---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edytowanie kodu ASP.NET formularzy sieci Web w Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 3473ad476fbbebc58e12586334b4600f57cf17ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632749"
---
# <a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Edytowanie kodu we wzorcu ASP.NET Web Forms w programie Visual Studio 2013

Autor [Erik Reitan](https://github.com/Erikre)

Wiele stron formularza sieci Web ASP.NET umożliwia pisanie kodu w Visual Basic, C#lub w innym języku. Edytor kodu w programie Visual Studio może pomóc szybko napisać kod, jednocześnie pomagając uniknąć błędów. Ponadto edytor zapewnia sposoby tworzenia kodu wielokrotnego użytku w celu zmniejszenia ilości pracy, którą należy wykonać.

W tym instruktażu przedstawiono różne funkcje edytora kodu programu Visual Studio.

W tym instruktażu dowiesz się, jak:

- Poprawiaj błędy kodowania wbudowanego.
- Refaktoryzacja i zmiana nazwy kodu.
- Zmień nazwę zmiennych i obiektów.
- Wstaw fragmenty kodu.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten Instruktaż, potrzebne są:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [Microsoft Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework jest instalowana automatycznie. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 i Microsoft Visual Studio Express 2013 dla sieci Web często są określane jako Visual Studio w całej tej serii samouczków.  
    >   
    > W przypadku korzystania z programu Visual Studio w tym przewodniku przyjęto założenie, że wybrano kolekcję **Web Development** dla ustawień przy pierwszym uruchomieniu programu Visual Studio. Aby uzyskać więcej informacji, zobacz [How to: SELECT Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Tworzenie projektu aplikacji sieci Web i strony

<a id="sectionToggle0"></a>

W tej części przewodnika utworzysz projekt aplikacji sieci Web i dodasz do niego nową stronę.

### <a name="to-create-a-web-application-project"></a>Aby utworzyć projekt aplikacji sieci Web

1. Otwórz Microsoft Visual Studio.
2. W menu **plik** wybierz pozycję **Nowy projekt**.  
    ![menu plik](code-editing-in-web-forms-pages/_static/image1.png)

    Zostanie wyświetlone okno dialogowe **Nowy projekt**.
3. Wybierz kolejno pozycje **Szablony** -&gt;  **C# Visual** -&gt; szablon **sieci Web** po lewej stronie.
4. Wybierz szablon **aplikacji sieci Web ASP.NET** w środkowej kolumnie.
5. Nazwij projekt ***BasicWebApp*** i kliknij przycisk **OK** .   
okno dialogowe ![nowego projektu](code-editing-in-web-forms-pages/_static/image2.png)
6. Następnie wybierz szablon **formularze sieci Web** i kliknij przycisk **OK** , aby utworzyć projekt.  
okno dialogowe ![nowego projektu ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Program Visual Studio tworzy nowy projekt, który zawiera wstępnie utworzoną funkcję opartą na szablonie formularzy sieci Web.

## <a name="creating-a-new-aspnet-web-forms-page"></a>Tworzenie nowej strony formularzy sieci Web ASP.NET

Podczas tworzenia nowej aplikacji formularzy sieci Web przy użyciu szablonu projektu **aplikacji sieci web ASP.NET** program Visual Studio dodaje stronę ASP.NET (stronę Web Forms) o nazwie *default. aspx*, a także kilka innych plików i folderów. Możesz użyć domyślnej strony *. aspx* jako strony głównej aplikacji sieci Web. Jednak w tym instruktażu utworzysz nową stronę i zaczniesz korzystać z niej.

### <a name="to-add-a-page-to-the-web-application"></a>Aby dodać stronę do aplikacji sieci Web

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy nazwę aplikacji sieci Web (w tym samouczku nazwa aplikacji to **BasicWebSite**), a następnie kliknij pozycję **Dodaj** -&gt; **nowy element**.   
Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Wybierz grupę szablonów **sieci Web** **Visual C#**  -&gt; po lewej stronie. Następnie wybierz pozycję **formularz sieci Web** z listy Środkowej i nadaj jej nazwę *FirstWebPage. aspx*.   
    ![okno dialogowe Dodawanie nowego elementu](code-editing-in-web-forms-pages/_static/image4.png)
3. Kliknij przycisk **Dodaj** , aby dodać stronę formularzy sieci Web do projektu.  
 Program Visual Studio tworzy nową stronę i otwiera ją.
4. Następnie ustaw tę nową stronę jako domyślną stronę startową. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy nową stronę o nazwie *FirstWebPage. aspx* i wybierz pozycję **Ustaw jako stronę startową**. Przy następnym uruchomieniu tej aplikacji w celu przetestowania postępu ta nowa strona zostanie automatycznie wyświetlona w przeglądarce.

## <a name="correcting-inline-coding-errors"></a>Poprawianie błędów kodowania wbudowanego

Edytor kodu w programie Visual Studio pomaga uniknąć błędów podczas pisania kodu, a jeśli wystąpi błąd, Edytor kodu pomoże Ci poprawić błąd. W tej części przewodnika napiszesz wiersz kodu, który ilustruje funkcje korekcji błędów w edytorze.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Aby poprawić proste błędy kodowania w programie Visual Studio

1. W widoku **projekt** kliknij dwukrotnie pustą stronę, aby utworzyć procedurę obsługi dla zdarzenia **ładowania** dla strony.   
   Używasz programu obsługi zdarzeń tylko jako miejsca do pisania kodu.
2. Wewnątrz procedury obsługi wpisz następujący wiersz, który zawiera błąd, i naciśnij klawisz **Enter**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Po naciśnięciu klawisza **Enter**Edytor kodu umieści zieloną i czerwoną podkreślenie (często wywołując &quot;falistej&quot; linie) w obszarze kodu, który ma problemy. Zielona podkreślenie wskazuje na ostrzeżenie. Czerwona podkreślenie wskazuje błąd, który należy naprawić. 

    Przytrzymaj wskaźnik myszy nad `myStr`, aby wyświetlić etykietkę narzędzia informującą o ostrzeżeniu. Ponadto przytrzymaj wskaźnik myszy nad czerwonym podkreśleniem, aby wyświetlić komunikat o błędzie.

    Na poniższej ilustracji przedstawiono kod z podkreśleniami.

    ![Tekst powitalny w widok Projekt](code-editing-in-web-forms-pages/_static/image5.png "Tekst powitalny w widok Projekt")  
   Błąd należy naprawić, dodając średnik `;` na końcu wiersza. Ostrzeżenie po prostu powiadamia, że zmienna `myStr` nie została jeszcze użyta.  

    > [!NOTE] 
    > 
    > Bieżące ustawienia formatowania kodu w programie Visual Studio można wyświetlić, wybierając pozycję **narzędzia** -**opcje** &gt; -&gt; **czcionki i kolory**.

## <a name="refactoring-and-renaming"></a>Refaktoryzacja i zmiana nazwy

Refaktoryzacja jest metodologią oprogramowania, która polega na restrukturyzacji kodu w celu ułatwienia jego zrozumienia i utrzymania, przy jednoczesnym zachowaniu jego funkcjonalności. Prostym przykładem może być napisanie kodu w programie obsługi zdarzeń w celu pobrania danych z bazy danych. Podczas tworzenia strony Odkryj, że musisz uzyskać dostęp do danych z kilku różnych programów obsługi. W związku z tym można Refaktoryzacja kodu strony przez utworzenie metody dostępu do danych na stronie i wstawianie wywołań do metody w programach obsługi.

Edytor kodu zawiera narzędzia ułatwiające wykonywanie różnych zadań refaktoryzacji. W tym instruktażu będziesz korzystać z dwóch technik refaktoryzacji: zmiana nazw zmiennych i wyodrębnianie metod. Inne opcje refaktoryzacji obejmują Hermetyzowanie pól, promowanie zmiennych lokalnych do parametrów metody i zarządzanie parametrami metod. Dostępność tych opcji refaktoryzacji zależy od lokalizacji w kodzie.

### <a name="refactoring-code"></a>Refaktoryzacja kodu

Typowym scenariuszem refaktoryzacji jest utworzenie (wyodrębnienie) metody z kodu, który znajduje się wewnątrz innego elementu członkowskiego, na przykład metody. Pozwala to zmniejszyć rozmiar oryginalnego elementu członkowskiego i sprawia, że wyodrębniony kod wielokrotnego użytku.

W tej części przewodnika napiszesz prosty kod, a następnie wyodrębnimy metodę z niej. Refaktoryzacja jest obsługiwana dla C#programu, dlatego utworzysz stronę używaną C# jako język programowania.

### <a name="to-extract-a-method-in-a-c-page"></a>Aby wyodrębnić metodę na C# stronie

1. Przejdź do widoku **projektu** .
2. W **przyborniku**, na karcie **Standardowy** przeciągnij kontrolkę [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) na stronę.
3. Kliknij dwukrotnie formant **przycisku** , aby utworzyć procedurę obsługi dla zdarzenia [kliknięcia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) , a następnie Dodaj następujący wyróżniony kod:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Kod tworzy obiekt **ArrayList** , używa pętli do załadowania do wartości, a następnie używa innej pętli do wyświetlania zawartości obiektu **ArrayList** .
4. Naciśnij **kombinację klawiszy CTRL + F5** , aby uruchomić stronę, a następnie kliknij **przycisk** , aby upewnić się, że zobaczysz następujące dane wyjściowe:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Wróć do edytora kodu, a następnie wybierz następujące wiersze w programie obsługi zdarzeń.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Kliknij prawym przyciskiem myszy zaznaczenie, kliknij pozycję **Refaktoryzacja**, a następnie wybierz polecenie **Wyodrębnij metodę**. 

    Zostanie wyświetlone okno dialogowe **Wyodrębnij metodę** .
7. W polu **Nazwa nowej metody** wpisz **DisplayArray**, a następnie kliknij przycisk **OK**. 

    Edytor kodu tworzy nową metodę o nazwie `DisplayArray`i umieszcza wywołanie nowej metody w obsłudze **kliknięcia** , w której pętla była pierwotnie.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Naciśnij **kombinację klawiszy CTRL + F5** , aby ponownie uruchomić stronę, a następnie kliknij **przycisk**.

    Strona działa tak samo jak wcześniej. Metoda `DisplayArray` może teraz wywołać z dowolnego miejsca w klasie Page.

## <a name="renaming-variables"></a>Zmiana nazw zmiennych

Gdy pracujesz z zmiennymi, a także obiektami, możesz chcieć zmienić ich nazwy, gdy już istnieją odwołania do kodu. Jednak zmiana nazw zmiennych i obiektów może spowodować przerwanie kodu, jeśli pominięto zmianę nazwy jednego z odwołań. W związku z tym można użyć refaktoryzacji do przeprowadzenia zmiany nazwy.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Aby użyć refaktoryzacji do zmiany nazwy zmiennej

1. W programie obsługi zdarzeń **kliknij** następujący wiersz:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Kliknij prawym przyciskiem myszy nazwę zmiennej `alist`, wybierz pozycję **Refaktoryzacja**, a następnie wybierz polecenie **Zmień nazwę**.

    Zostanie wyświetlone okno dialogowe **zmiana nazwy** .
3. W polu **Nowa nazwa** wpisz **ArrayList1** i upewnij się, że zaznaczone jest pole wyboru **zmiany odwołania w wersji zapoznawczej** . Następnie kliknij przycisk **OK**.

    Zostanie wyświetlone okno dialogowe **Podgląd zmian** i zostanie wyświetlone drzewo zawierające wszystkie odwołania do zmiennej, której nazwa jest zmieniana.
4. Kliknij przycisk **Zastosuj** , aby zamknąć okno dialogowe **Podgląd zmian** .

    Nazwy zmiennych odwołujących się w odniesieniu do wybranego wystąpienia są zmieniane. Należy jednak pamiętać, że nie zmieniono nazwy zmiennej `alist` w następującym wierszu.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Nie można zmienić nazwy zmiennej `alist` w tym wierszu, ponieważ nie reprezentuje ona takiej samej wartości jak zmienna, `alist` której nazwa została zmieniona. Zmienna `alist` w deklaracji `DisplayArray` jest zmienną lokalną dla tej metody. Ilustruje to, że używanie refaktoryzacji do zmiany nazwy zmiennych jest inne niż wykonywanie akcji znajdowania i zamieniania w edytorze; Refaktoryzacja zmienia nazwy zmiennych ze wiedzą semantyki zmiennej, z którą pracuje.

## <a name="inserting-snippets"></a>Wstawianie fragmentów kodu

Ponieważ istnieje wiele zadań związanych z kodowaniem, których deweloperzy formularzy sieci Web często potrzebują do wykonania, Edytor kodu udostępnia bibliotekę fragmentów kodu lub bloków przedpisanych kod. Możesz wstawić te fragmenty kodu do strony.

Każdy język, który jest używany w programie Visual Studio, ma niewielkie różnice w sposobie wstawiania fragmentów kodu. Aby uzyskać informacje na temat wstawiania wstawek, zobacz [Visual Basic fragmenty kodu IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx). Aby uzyskać informacje na temat wstawiania wstawek w C#wizualizacji, zobacz [fragmenty kodu wizualnego C# ](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Następne kroki

W tym instruktażu przedstawiono podstawowe funkcje edytora kodu programu Visual Studio 2010 na potrzeby poprawiania błędów w kodzie, refaktoryzacji kodu, zmieniania nazw zmiennych i wstawiania fragmentów kodu do kodu. Dodatkowe funkcje w edytorze umożliwiają szybkie i łatwe tworzenie aplikacji. Na przykład możesz chcieć:

- Dowiedz się więcej o funkcjach IntelliSense, takich jak modyfikowanie opcji IntelliSense, zarządzanie fragmentami kodu i wyszukiwanie fragmentów kodu w trybie online. Aby uzyskać więcej informacji, zobacz [Korzystanie z funkcji IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Dowiedz się, jak tworzyć własne fragmenty kodu. Aby uzyskać więcej informacji, zobacz [Tworzenie i używanie fragmentów kodu IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Dowiedz się więcej na temat funkcji specyficznych dla Visual Basic fragmentów kodu IntelliSense, takich jak Dostosowywanie wstawek i rozwiązywanie problemów. Aby uzyskać więcej informacji, zobacz [Visual Basic fragmenty kodu IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Dowiedz się więcej C#o funkcjach IntelliSense, takich jak Refaktoryzacja i fragmenty kodu. Aby uzyskać więcej informacji, [Zobacz C# Visual IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).

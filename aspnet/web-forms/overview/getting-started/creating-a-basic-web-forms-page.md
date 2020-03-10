---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Tworzenie strony formularzy sieci Web w warstwie Podstawowa ASP.NET 4,5 przy użyciu Visual Studio 2013
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 5d13a51128eecd92a82cfd06054448582a348e11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629760"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Tworzenie strony formularzy sieci Web w warstwie Podstawowa ASP.NET 4,5 przy użyciu Visual Studio 2013

Autor [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Ten przewodnik zawiera wprowadzenie do środowiska deweloperskiego sieci Web w [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) i w [Microsoft Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Ten przewodnik przeprowadzi Cię przez proces tworzenia prostej strony formularzy sieci Web ASP.NET i ilustruje podstawowe techniki tworzenia nowej strony, dodawania formantów i pisania kodu.

Zadania przedstawione w tym instruktażu obejmują:

- Tworzenie projektu aplikacji Web Forms w systemie plików.
- Zapoznaj się z programem Visual Studio.
- Tworzenie strony ASP.NET.
- Dodawanie kontrolek.
- Dodawanie programów obsługi zdarzeń.
- Uruchamianie i testowanie strony z programu Visual Studio.

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

W tej części przewodnika utworzysz projekt aplikacji sieci Web i dodasz do niego nową stronę. Dodasz również tekst HTML i uruchomisz stronę w przeglądarce.

### <a name="to-create-a-web-application-project"></a>Aby utworzyć projekt aplikacji sieci Web

1. Otwórz Microsoft Visual Studio.
2. W menu **plik** wybierz pozycję **Nowy projekt**.  
    ![menu plik](creating-a-basic-web-forms-page/_static/image1.png)

    Zostanie wyświetlone okno dialogowe **Nowy projekt**.
3. Wybierz kolejno pozycje **Szablony** -&gt;  **C# Visual** -&gt; szablon **sieci Web** po lewej stronie.
4. Wybierz szablon **aplikacji sieci Web ASP.NET** w środkowej kolumnie.
5. Nazwij projekt ***BasicWebApp*** i kliknij przycisk **OK** .   
okno dialogowe ![nowego projektu](creating-a-basic-web-forms-page/_static/image2.png)
6. Następnie wybierz szablon **formularze sieci Web** i kliknij przycisk **OK** , aby utworzyć projekt.  
okno dialogowe ![nowego projektu ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Program Visual Studio tworzy nowy projekt, który zawiera wstępnie utworzoną funkcję opartą na szablonie formularzy sieci Web. Zawiera ona nie tylko stronę *Home. aspx* , stronę *about. aspx* , stronę *Contact. aspx* , ale również zawiera funkcje członkostwa, które rejestrują użytkowników i zapisują swoje poświadczenia, dzięki czemu mogą logować się do witryny sieci Web. Gdy nowa strona zostanie utworzona, domyślnie program Visual Studio wyświetla stronę w widoku **źródła** , w której można zobaczyć elementy HTML strony. Na poniższej ilustracji przedstawiono elementy wyświetlane w widoku **źródła** , jeśli utworzono nową stronę sieci Web o nazwie *BasicWebApp. aspx*.  
    ](creating-a-basic-web-forms-page/_static/image4.png) widok źródła ![

### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Przewodnik po środowisku deweloperskim sieci Web programu Visual Studio

Przed kontynuowaniem modyfikowania strony warto zapoznać się ze środowiskiem programistycznym programu Visual Studio. Na poniższej ilustracji przedstawiono okna i narzędzia, które są dostępne w programie Visual Studio i Visual Studio Express dla sieci Web.

> [!NOTE] 
> 
> Ten diagram przedstawia domyślne lokalizacje okien i okien. Menu **Widok** umożliwia wyświetlenie dodatkowych okien oraz zmianę układu systemu Windows w celu dostosowania do swoich preferencji. Jeśli zostały już wprowadzone zmiany w rozmieszczeniu okna, wyświetlane informacje nie są zgodne z ilustracją.

 Środowisko programu Visual Studio

![Środowisko programu Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Zapoznaj się z projektant internetowy

Przejrzyj powyższą ilustrację i Dopasuj tekst do poniższej listy, która opisuje najczęściej używane okna i narzędzia. (Nie wszystkie okna i narzędzia, które są widoczne, są wymienione tutaj, tylko te oznaczone na poprzedniej ilustracji).

- Paski narzędzi. Podaj polecenia formatowania tekstu, wyszukiwania tekstu i tak dalej. Niektóre paski narzędzi są dostępne tylko wtedy, gdy Pracujesz w widoku **projektu** .
- Okno **Eksplorator rozwiązań** . Wyświetla pliki i foldery w aplikacji sieci Web.
- Okno dokumentu. Wyświetla dokumenty, nad którymi pracujesz w oknach z kartami. Możesz przełączać się między dokumentami, klikając pozycję karty.
- Okno **Właściwości** . Umożliwia zmianę ustawień strony, elementów HTML, formantów i innych obiektów.
- Wyświetl karty. Zaprezentowanie różnych widoków tego samego dokumentu. Widok **projektu** jest powierzchnią edycji niemal-WYSIWYG. Widok **źródła** jest edytorem HTML strony. Widok **podzielony** wyświetla widok **projektu** i widok **źródła** dokumentu. W dalszej części tego instruktażu będziesz korzystać z widoków **projekt** i **Źródło** . Jeśli wolisz otworzyć stronę sieci Web w widoku **projektu** , w menu **Narzędzia** kliknij polecenie **Opcje**, wybierz węzeł **Projektant HTML** i zmień opcję **Uruchom strony w** .
- **Przybornik**. Udostępnia kontrolki i elementy HTML, które można przeciągać na stronę. Elementy **przybornika** są pogrupowane według typowej funkcji.
- **Erwera Explorer**. Wyświetla połączenia bazy danych. Jeśli Eksplorator serwera nie jest widoczny, w menu Widok kliknij pozycję Eksplorator serwera.

### <a name="creating-a-new-aspnet-web-forms-page"></a>Tworzenie nowej strony formularzy sieci Web ASP.NET

Podczas tworzenia nowej aplikacji formularzy sieci Web przy użyciu szablonu projektu **aplikacji sieci web ASP.NET** program Visual Studio dodaje stronę ASP.NET (stronę Web Forms) o nazwie *default. aspx*, a także kilka innych plików i folderów. Możesz użyć domyślnej strony *. aspx* jako strony głównej aplikacji sieci Web. Jednak w tym instruktażu utworzysz nową stronę i zaczniesz korzystać z niej.

### <a name="to-add-a-page-to-the-web-application"></a>Aby dodać stronę do aplikacji sieci Web

1. Zamknij stronę *default. aspx* . Aby to zrobić, kliknij kartę, na której jest wyświetlana nazwa pliku, a następnie kliknij przycisk Zamknij.
2. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy nazwę aplikacji sieci Web (w tym samouczku nazwa aplikacji to **BasicWebSite**), a następnie kliknij pozycję **Dodaj** -&gt; **nowy element**.   
Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
3. Wybierz grupę szablonów **sieci Web** **Visual C#**  -&gt; po lewej stronie. Następnie wybierz pozycję **formularz sieci Web** z listy Środkowej i nadaj jej nazwę *FirstWebPage. aspx*.   
    ![okno dialogowe Dodawanie nowego elementu](creating-a-basic-web-forms-page/_static/image6.png)
4. Kliknij przycisk **Dodaj** , aby dodać stronę sieci Web do projektu.  
Program Visual Studio tworzy nową stronę i otwiera ją.

### <a name="adding-html-to-the-page"></a>Dodawanie kodu HTML do strony

W tej części przewodnika dodasz do strony jakiś tekst statyczny.

### <a name="to-add-text-to-the-page"></a>Aby dodać tekst do strony

1. W dolnej części okna dokumentu kliknij kartę **projekt** , aby przełączyć się do widoku **projektu** .

    Widok Projekt wyświetla bieżącą stronę w sposób podobny do tego. W tym momencie nie masz żadnych tekstu ani kontrolek na stronie, więc strona jest pusta, z wyjątkiem linii kreskowanej, która wskazuje prostokąt. Ten prostokąt reprezentuje element **DIV** na stronie.
2. Kliknij wewnątrz prostokąta, który jest obramowany linią kreskowaną.
3. Wpisz **Witamy w programie Visual Web Developer** i naciśnij dwa razy klawisz **Enter** .

    Na poniższej ilustracji przedstawiono tekst, który wpisano w widoku **projektu** .

    ![Tekst powitalny w widok Projekt](creating-a-basic-web-forms-page/_static/image7.png "Tekst powitalny w widok Projekt")
4. Przejdź do widoku **źródła** .

    KOD HTML można wyświetlić w widoku **źródła** , który został utworzony podczas wpisywania w widoku **projektu** .  
    ![stronę sieci Web z tekstem statycznym](creating-a-basic-web-forms-page/_static/image8.png)

### <a name="running-the-page"></a>Uruchamianie strony

Przed kontynuowaniem dodawania kontrolek do strony, możesz ją najpierw uruchomić.

### <a name="to-run-the-page"></a>Aby uruchomić stronę

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *FirstWebPage. aspx* i wybierz pozycję **Ustaw jako stronę startową**.
2. Naciśnij **klawisze CTRL + F5** , aby uruchomić stronę.

    Ta strona zostanie wyświetlona w przeglądarce. Mimo że utworzona strona ma rozszerzenie nazwy pliku *. aspx*, jest ono obecnie uruchamiane jak każda strona HTML.

    Aby wyświetlić stronę w przeglądarce, możesz również kliknąć prawym przyciskiem myszy stronę w **Eksplorator rozwiązań** i wybrać pozycję **Widok w przeglądarce**.
3. Zamknij przeglądarkę, aby zatrzymać aplikację sieci Web.

## <a name="adding-and-programming-controls"></a>Dodawanie i Programowanie formantów

<a id="sectionToggle1"></a>

Teraz dodasz kontrolki serwera do strony. Formanty serwera, takie jak przyciski, etykiety, pola tekstowe i inne znane formanty, zapewniają typowe możliwości przetwarzania formularzy dla stron formularzy sieci Web. Można jednak obsłużyć formanty z kodem, który jest uruchamiany na serwerze, a nie na kliencie programu.

Dodasz kontrolkę [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , kontrolkę [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) i kontrolkę [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) do strony i napiszesz kod, aby obsłużyć zdarzenie [kliknięcia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) dla kontrolki [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

### <a name="to-add-controls-to-the-page"></a>Aby dodać kontrolki do strony

1. Kliknij kartę **projekt** , aby przełączyć się do widoku **projektu** .
2. Umieść punkt wstawiania na końcu tekstu w **programie Visual Web Developer** i naciśnij klawisz **Enter** , aby zwolnić miejsce w polu **DIV** elementu.
3. W **przyborniku**rozwiń grupę **Standard** , jeśli nie została jeszcze rozwinięta.  
Należy pamiętać, że może być konieczne rozwinięcie okna **przybornika** po lewej stronie, aby je wyświetlić.
4. Przeciągnij kontrolkę [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) na stronę i upuść ją w środku pola **DIV** , które ma **Witamy w Visual Web Developer** w pierwszym wierszu.
5. Przeciągnij kontrolkę [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) na stronę i upuść ją po prawej stronie kontrolki [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) .
6. Przeciągnij kontrolkę [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) na stronę i upuść ją w osobnym wierszu poniżej kontrolki [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .
7. Umieść punkt wstawiania powyżej kontrolki [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) , a następnie wpisz **Wprowadź swoją nazwę:** .

    Ten statyczny tekst HTML jest podpisem dla kontrolki [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) . Na tej samej stronie można mieszać statyczne kontrolki HTML i serwery. Na poniższej ilustracji przedstawiono, jak trzy kontrolki pojawiają się w widoku **projektu** .

    ![Trzy kontrolki w widok Projekt](creating-a-basic-web-forms-page/_static/image9.png "Trzy kontrolki w widok Projekt")

### <a name="setting-control-properties"></a>Ustawianie właściwości kontrolki

Program Visual Studio oferuje różne sposoby ustawiania właściwości formantów na stronie. W tej części przewodnika ustawisz właściwości zarówno w widoku **projektu** , jak i w widoku **źródła** .

### <a name="to-set-control-properties"></a>Aby ustawić właściwości kontrolki

1. Najpierw Wyświetl okna **Właściwości** , wybierając z menu **Widok** polecenie&gt; **inne** -&gt; **Właściwości okna**. Możesz również wybrać **F4** , aby wyświetlić okno **Właściwości** .
2. Zaznacz kontrolkę [przycisk](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , a następnie w oknie **Właściwości** ustaw wartość **tekst** w polu **Nazwa wyświetlana**. Wprowadzony tekst jest wyświetlany na przycisku w projektancie, jak pokazano na poniższej ilustracji.

    ![Ustaw tekst przycisku](creating-a-basic-web-forms-page/_static/image10.png "Ustaw tekst przycisku")
3. Przejdź do widoku **źródła** .

    Widok **źródła** wyświetla kod HTML strony, w tym elementy utworzone przez program Visual Studio dla kontrolek serwera. Formanty są deklarowane przy użyciu składni podobnej do języka HTML, z tą różnicą, że Tagi korzystają z prefiksu **ASP:** i zawierają atrybut **runat =&quot;Server&quot;** .

    Właściwości kontrolki są deklarowane jako atrybuty. Na przykład po ustawieniu właściwości [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) dla kontrolki [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) w kroku 1 w rzeczywistości ustawisz atrybut **Text** w znaczniku kontrolki.

    > [!NOTE] 
    > 
    > Wszystkie kontrolki znajdują się wewnątrz elementu **form** , który ma także atrybut **runat =&quot;Server&quot;** . Wartość **runat =&quot;server&quot;** Attribute i **ASP:** prefix dla tagów kontroli oznacza kontrolki tak, aby były przetwarzane przez ASP.NET na serwerze podczas uruchamiania strony. Kod poza **&lt;formularzu runat =&quot;server&quot;&gt;** i **&lt;skryptu runat =&quot;Server&quot;&gt;** elementy są wysyłane bez zmian do przeglądarki, co jest dlatego, że kod ASP.NET musi znajdować się wewnątrz elementu, którego tag otwierającego zawiera atrybut **runat =&quot;Server&quot;** .
4. Następnie dodasz dodatkową właściwość do kontrolki [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Umieść punkt wstawiania bezpośrednio po stronie **ASP: etykieta** w tagu **&lt;asp: Label&gt;** , a następnie naciśnij klawisz **spacji**.

    Zostanie wyświetlona lista rozwijana, która wyświetla listę dostępnych właściwości, które można ustawić dla kontrolki [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Ta funkcja, nazywana technologią **IntelliSense**, pomaga w widoku **źródła** z składnią formantów serwera, elementów HTML i innych elementów na stronie. Na poniższej ilustracji przedstawiono listę rozwijaną **IntelliSense** dla kontrolki [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

    ![Atrybuty IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "Atrybuty IntelliSense")
5. Wybierz opcję **ForeColor** , a następnie wpisz znak równości.

    Technologia IntelliSense wyświetla listę kolorów.

    > [!NOTE] 
    > 
    > Listę rozwijaną **IntelliSense** można wyświetlić w dowolnym momencie, naciskając **klawisze Ctrl + J** podczas wyświetlania kodu.
6. Wybierz kolor tekstu kontrolki **[etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** . Upewnij się, że wybrano kolor, który jest wystarczająco ciemny, aby odczytać białe tło.

    Atrybut **ForeColor** jest uzupełniany wybranym kolorem, w tym cudzysłowem zamykającym.

### <a name="programming-the-button-control"></a>Programowanie kontrolki przycisku

W tym instruktażu napiszesz kod, który odczytuje nazwę, którą wprowadzi użytkownik w polu tekstowym, a następnie wyświetla nazwę w kontrolce [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

### <a name="add-a-default-button-event-handler"></a>Dodaj domyślną procedurę obsługi zdarzeń przycisku

1. Przejdź do widoku **projektu** .
2. Kliknij dwukrotnie formant [przycisku](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

    Domyślnie program Visual Studio przełącza się do pliku związanego z kodem i tworzy szkieletową obsługę zdarzeń dla domyślnego zdarzenia kontrolki [przycisku](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) . zdarzenie [kliknięcia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) . Plik związany z kodem oddziela znaczniki interfejsu użytkownika (na przykład HTML) z kodu serwera (na C#przykład).   
   Kursor jest umieszczony na dodanym kodzie dla tego programu obsługi zdarzeń.

    > [!NOTE] 
    > 
    > Dwukrotne kliknięcie kontrolki w widoku **projekt** jest tylko jednym z kilku sposobów tworzenia obsługi zdarzeń.
3. Wewnątrz **Button1\_kliknij pozycję** procedura obsługi zdarzeń, wpisz **Label1** , a następnie kropkę ( **.** ).

    Gdy wpiszesz kropkę po **identyfikatorze** etykiety (**Label1**), program Visual Studio wyświetli listę dostępnych elementów członkowskich dla kontrolki [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) , jak pokazano na poniższej ilustracji. Składową zwykle jest właściwość, metoda lub zdarzenie.

    ![Technologia IntelliSense w widoku kodu](creating-a-basic-web-forms-page/_static/image12.png "Technologia IntelliSense w widoku kodu")
4. Zakończ procedurę obsługi zdarzeń **kliknięcia** dla przycisku, aby była odczytywana, jak pokazano w poniższym przykładzie kodu.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Przełącz się z powrotem, aby wyświetlić widok **źródła** znaczników HTML, klikając prawym przyciskiem myszy *FirstWebPage. aspx* w **Eksplorator rozwiązań** i wybierając pozycję **Wyświetl znaczniki**.
6. Przewiń do elementu **&lt;ASP: Button&gt;** . Należy zauważyć, że element **&lt;ASP: Button&gt;** ma teraz atrybut **onkliknięciu =&quot;Button1\_kliknij&quot;** .

    Ten atrybut wiąże zdarzenie [kliknięcia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) przycisku z metodą obsługi zakodowaną w poprzednim kroku.

    Metody obsługi zdarzeń mogą mieć dowolną nazwę; wyświetlana nazwa jest domyślną nazwą utworzoną przez program Visual Studio. Ważnym punktem jest to, że nazwa używana dla atrybutu **onkliknięcia** w kodzie HTML musi być zgodna z nazwą metody zdefiniowanej w kodzie.

### <a name="running-the-page"></a>Uruchamianie strony

Teraz można testować kontrolki serwera na stronie.

### <a name="to-run-the-page"></a>Aby uruchomić stronę

1. Naciśnij **klawisze CTRL + F5** , aby uruchomić stronę w przeglądarce. Jeśli wystąpi błąd, ponownie sprawdź powyższe kroki.
2. Wprowadź nazwę w polu tekstowym, a następnie kliknij przycisk **Nazwa wyświetlana** .

    Wprowadzona nazwa zostanie wyświetlona w kontrolce [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Należy pamiętać, że po kliknięciu przycisku strona zostanie opublikowana na serwerze sieci Web. ASP.NET następnie ponownie tworzy stronę, uruchamia kod (w tym przypadku zostanie uruchomiony program obsługi zdarzeń [kliknięcia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) [przycisku](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ), a następnie wysyła nową stronę do przeglądarki. Jeśli oglądasz pasek stanu w przeglądarce, zobaczysz, że strona jest przejazdem do serwera sieci Web po każdym kliknięciu przycisku.
3. W przeglądarce Wyświetl źródło uruchomionej strony, klikając ją prawym przyciskiem myszy i wybierając pozycję **Wyświetl źródło**.

    Kod źródłowy strony zawiera kod HTML bez kodu serwera. W tym celu nie widzisz elementów **&lt;ASP:&gt;** , z którymi pracowano w widoku **źródła** . Gdy strona zostanie uruchomiona, ASP.NET przetwarza formanty serwera i renderuje elementy HTML na stronie, która wykonuje funkcje reprezentujące formant. Na przykład formant **&lt;ASP: Button&gt;** jest RENDEROWANY jako HTML **&lt;input type =&quot;Submit&quot;&gt;** .
4. Zamknij okno przeglądarki.

## <a name="working-with-additional-controls"></a>Praca z dodatkowymi kontrolkami

<a id="sectionToggle2"></a>

W tej części przewodnika będziesz korzystać z kontrolki [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) , która wyświetla daty w danym miesiącu. Formant [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) jest bardziej skomplikowaną kontrolką niż przycisk, pole tekstowe i etykietę, z którą pracujesz i przedstawiono kilka dalszych możliwości formantów serwera.

W tej sekcji dodasz kontrolkę [System. Web. UI. WebControls. Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) do strony i sformatujesz ją.

### <a name="to-add-a-calendar-control"></a>Aby dodać kontrolkę Calendar

1. W programie Visual Studio przejdź do widoku **projektu** .
2. W **standardowej** sekcji **przybornika**przeciągnij kontrolkę [Kalendarz](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) na stronę i upuść ją poniżej elementu **DIV** , który zawiera inne kontrolki.

    Zostanie wyświetlony panel tagów inteligentnych kalendarza. Panel wyświetla polecenia, które ułatwiają wykonywanie najbardziej typowych zadań dla wybranej kontrolki. Na poniższej ilustracji przedstawiono kontrolkę [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) , która jest renderowana w widoku **projektu** .

    ![Kontrolka Calendar w widok Projekt](creating-a-basic-web-forms-page/_static/image13.png "Kontrolka Calendar w widok Projekt")
3. W panelu tagów inteligentnych wybierz pozycję **Autoformatowanie**.

    Zostanie wyświetlone okno dialogowe **autoformat** , które pozwala wybrać schemat formatowania dla kalendarza. Na poniższej ilustracji przedstawiono okno dialogowe **Autoformatowanie** dla formantu [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    ![Okno dialogowe autoformat (kontrolka Calendar)](creating-a-basic-web-forms-page/_static/image14.png "Okno dialogowe autoformat (kontrolka Calendar)")
4. Z listy **Wybierz schemat** wybierz pozycję **prosty** , a następnie kliknij przycisk **OK**.
5. Przejdź do widoku **źródła** .

    Można zobaczyć **&lt;ASP: Calendar&gt;** elementu. Ten element jest znacznie dłuższy niż elementy dla utworzonych wcześniej formantów prostych. Zawiera również elementy podelementowe, takie jak **&lt;WeekEndDayStyle&gt;** , które reprezentują różne ustawienia formatowania. Na poniższej ilustracji przedstawiono kontrolkę [Kalendarz](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) w widoku **źródła** . (Dokładne znaczniki widoczne w widoku **źródła** mogą się nieco różnić od ilustracji).

    ![Kontrolka Calendar w widoku źródła](creating-a-basic-web-forms-page/_static/image15.png "Kontrolka Calendar w widoku źródła")

### <a name="programming-the-calendar-control"></a>Programowanie kontrolki Calendar

W tej sekcji zostanie zaprogramować formant [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) , aby wyświetlić aktualnie wybraną datę.

### <a name="to-program-the-calendar-control"></a>Aby programować formant Calendar

1. W widoku **projekt** kliknij dwukrotnie kontrolkę [Kalendarz](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    Zostanie utworzony nowy program obsługi zdarzeń, który zostanie wyświetlony w pliku związanym z kodem o nazwie *FirstWebPage.aspx.cs*.
2. Zakończ obsługę zdarzeń [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) z poniższym kodem.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Powyższy kod ustawia tekst kontrolki etykieta na wybraną datę kontrolki Calendar.

### <a name="running-the-page"></a>Uruchamianie strony

Teraz można testować kalendarz.

### <a name="to-run-the-page"></a>Aby uruchomić stronę

1. Naciśnij **klawisze CTRL + F5** , aby uruchomić stronę w przeglądarce.
2. Kliknij datę w kalendarzu.

    Wybrana data jest wyświetlana w kontrolce [etykieta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .
3. W przeglądarce Wyświetl kod źródłowy strony.

    Należy zauważyć, że formant [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) został renderowany na stronie jako **tabela**, z każdym dniem jako elementem **TD** .
4. Zamknij okno przeglądarki.

## <a name="next-steps"></a>Następne kroki

<a id="nextStepsToggle"></a>

W tym instruktażu przedstawiono podstawowe funkcje projektanta stron programu Visual Studio. Teraz, gdy zrozumiesz, jak utworzyć i edytować stronę formularzy sieci Web w programie Visual Studio, możesz chcieć poznać inne funkcje. Na przykład możesz chcieć wykonać następujące czynności:

- Dowiedz się więcej o formularzach sieci Web ASP.NET, postępując zgodnie z serią samouczków krok po kroku [wprowadzenie z formularzami sieci web ASP.NET 4,5 i Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Dowiedz się więcej na temat kaskadowych arkuszy stylów (CSS). Aby uzyskać szczegółowe informacje, zobacz [Praca z omówieniem CSS](https://msdn.microsoft.com/library/bb398931.aspx).

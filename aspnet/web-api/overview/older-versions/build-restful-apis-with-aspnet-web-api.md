---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Tworzenie interfejsów API RESTful za pomocą ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: 'Wskazówki dotyczące laboratorium: Użyj interfejsu API sieci Web w ASP.NET 4. x, aby utworzyć prosty interfejs API REST dla aplikacji menedżera kontaktów.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621815"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a>Tworzenie interfejsów API RESTful za pomocą interfejsu API sieci Web ASP.NET

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

> Wskazówki dotyczące laboratorium: Użyj interfejsu API sieci Web w ASP.NET 4. x, aby utworzyć prosty interfejs API REST dla aplikacji menedżera kontaktów. Zostanie również skompilowany klient, aby korzystał z interfejsu API.

W ostatnich latach stało się jasne, że protokół HTTP nie tylko służy do obsługi stron HTML. Jest to również zaawansowana platforma służąca do tworzenia interfejsów API sieci Web przy użyciu kilku czasowników (GET, POST itd.) oraz kilku prostych koncepcji, takich jak *identyfikatory URI* i *nagłówki*. Interfejs API sieci Web ASP.NET to zestaw składników, które upraszczają programowanie HTTP. Ponieważ jest zbudowany w oparciu o środowisko uruchomieniowe ASP.NET MVC, interfejs API sieci Web automatycznie obsługuje szczegóły transportu niskiego poziomu protokołu HTTP. W tym samym czasie interfejs API sieci Web naturalnie uwidacznia model programowania HTTP. W rzeczywistości jednym celem interfejsu API sieci Web jest *nieabstrakcyjna w rzeczywistości* wartość http. W efekcie interfejs API sieci Web jest elastyczny i łatwy do rozbudowania.  Styl architektury REST okazał się skutecznym sposobem korzystania z protokołu HTTP, chociaż nie jest to jedyne prawidłowe podejście do protokołu HTTP. Program Contact Manager uwidoczni RESTful do wyświetlania, dodawania i usuwania kontaktów między innymi. 

To laboratorium wymaga podstawowej znajomości protokołu HTTP, REST i zakłada, że masz podstawową praktyczną wiedzę na temat języków HTML, JavaScript i jQuery.
> 
> > [!NOTE]
> > Witryna sieci Web ASP.NET ma obszar dedykowany struktury internetowego interfejsu API ASP.NET w [https://asp.net/web-api](https://asp.net/web-api). Ta lokacja będzie w dalszym ciągu dostarczać najświeższe informacje, przykłady i wiadomości związane z interfejsem API sieci Web, dlatego należy sprawdzać je często, jeśli chcesz uzyskać więcej informacji na temat tworzenia niestandardowych interfejsów API sieci Web dostępnych dla praktycznie dowolnego urządzenia lub platformy programistycznej.
> > 
> > Interfejs API sieci Web ASP.NET, podobny do ASP.NET MVC 4, ma doskonałą elastyczność pod względem rozdzielania warstwy usług od kontrolerów, co pozwala na bardzo proste użycie kilku dostępnych platform iniekcji zależności. W witrynie MSDN znajduje się dobry przykład, który pokazuje, jak używać Ninject do iniekcji zależności w projekcie interfejsu API sieci Web ASP.NET, który można pobrać z tego [miejsca](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Implementowanie interfejsu API sieci Web RESTful
- Wywoływanie interfejsu API z klienta HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek B](#AppendixB) , aby uzyskać instrukcje dotyczące sposobu ich instalacji).

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatek A: używanie fragmentów kodu](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące czynności:

1. [Ćwiczenie 1. Tworzenie interfejsu API sieci Web tylko do odczytu](#Exercise1)
2. [Ćwiczenie 2: Tworzenie internetowego interfejsu API do odczytu/zapisu](#Exercise2)
3. [Ćwiczenie 3: korzystanie z internetowego interfejsu API z klienta HTML](#Exercise3)

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

Szacowany czas wykonywania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Ćwiczenie 1. Tworzenie interfejsu API sieci Web tylko do odczytu

W tym ćwiczeniu zostaną zaimplementowane metody pobierania tylko do odczytu dla Menedżera kontaktów.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Zadanie 1 — Tworzenie projektu interfejsu API

W tym zadaniu zostanie użyty nowy szablon projektu sieci Web ASP.NET do utworzenia aplikacji internetowej interfejsu API sieci Web.

1. Uruchom **program Visual Studio 2012 Express for Web**, w tym celu przejdź do **menu Start** , a następnie wpisz **vs Express for Web** następnie naciśnij klawisz **Enter**.
2. Z menu **plik** wybierz pozycję **Nowy projekt**. Wybierz **wizualizację C# | Typ projektu sieci Web** z widoku drzewa typ projektu, a następnie wybierz typ projektu **aplikacji sieci Web ASP.NET MVC 4** . Ustaw **nazwę** projektu na *ContactName* i **nazwę rozwiązania** , aby *rozpocząć*, a następnie kliknij przycisk **OK**.

    ![Tworzenie nowego projektu aplikacji sieci Web ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Tworzenie nowego projektu aplikacji sieci Web ASP.NET MVC 4,0")

    *Tworzenie nowego projektu aplikacji sieci Web ASP.NET MVC 4,0*
3. W oknie dialogowym ASP.NET MVC 4 typu projektu wybierz typ projektu **interfejsu API sieci Web** . Kliknij przycisk **OK**.

    ![Określanie typu projektu interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Określanie typu projektu interfejsu API sieci Web")

    *Określanie typu projektu interfejsu API sieci Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Zadanie 2 — Tworzenie kontrolerów interfejsu API Menedżera kontaktów

W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.

1. Usuń plik o nazwie **ValuesController.cs** w folderze **controllers** z projektu.
2. Kliknij prawym przyciskiem myszy folder **controllers** w projekcie i wybierz polecenie **Dodaj | Z menu** kontekstowego.

    ![Dodawanie nowego kontrolera do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "Dodawanie nowego kontrolera do projektu")

    *Dodawanie nowego kontrolera do projektu*
3. W wyświetlonym oknie dialogowym **Dodaj kontroler** wybierz pozycję **pusty kontroler interfejsu API** z menu szablon. Nadaj nazwę klasy kontrolera **ContactController**. Następnie kliknij przycisk **Dodaj.**

    ![Przy użyciu okna dialogowego Dodawanie kontrolera można utworzyć nowy kontroler internetowego interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image4.png "Przy użyciu okna dialogowego Dodawanie kontrolera można utworzyć nowy kontroler internetowego interfejsu API")

    *Przy użyciu okna dialogowego Dodawanie kontrolera można utworzyć nowy kontroler internetowego interfejsu API*
4. Dodaj następujący kod do **ContactController**.

    (Fragment kodu- *Web API Lab-Ex01-Get API Metoda*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Naciśnij klawisz **F5**, aby debugować aplikację. Powinna zostać wyświetlona domyślna Strona główna projektu interfejsu API sieci Web.

    ![Domyślna strona główna aplikacji internetowego interfejsu API ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "Domyślna strona główna aplikacji internetowego interfejsu API ASP.NET")

    *Domyślna strona główna aplikacji internetowego interfejsu API ASP.NET*
6. W oknie programu Internet Explorer naciśnij klawisz **F12** , aby otworzyć okno **Narzędzia deweloperskie** . Kliknij kartę **Sieć** , a następnie kliknij przycisk **Rozpocznij przechwytywanie** , aby rozpocząć przechwytywanie ruchu sieciowego do okna.

    ![Otwieranie karty sieć i Inicjowanie przechwytywania sieciowego](build-restful-apis-with-aspnet-web-api/_static/image6.png "Otwieranie karty sieć i Inicjowanie przechwytywania sieciowego")

    *Otwieranie karty sieć i Inicjowanie przechwytywania sieciowego*
7. Dołącz adres URL na pasku adresu przeglądarki z **/API/Contact** i naciśnij klawisz ENTER. Szczegóły przesyłania zostaną wyświetlone w oknie przechwytywanie sieci. Należy pamiętać, że typ MIME odpowiedzi to **Application/JSON**. Pokazuje, jak domyślny format danych wyjściowych to JSON.

    ![Wyświetlanie danych wyjściowych żądania internetowego interfejsu API w widoku sieci](build-restful-apis-with-aspnet-web-api/_static/image7.png "Wyświetlanie danych wyjściowych żądania internetowego interfejsu API w widoku sieci")

    *Wyświetlanie danych wyjściowych żądania internetowego interfejsu API w widoku sieci*

    > [!NOTE]
    > Domyślne zachowanie programu Internet Explorer 10 będzie miało na celu zaproszenie użytkownika o zapisanie lub otwarcie strumienia pochodzącego z wywołania interfejsu API sieci Web. Dane wyjściowe będą plikami tekstowymi zawierającymi wynik JSON wywołania adresu URL interfejsu API sieci Web. Nie Anuluj okna dialogowego, aby zobaczyć zawartość odpowiedzi za pomocą okna narzędzia deweloperzy.
8. Kliknij przycisk **Przejdź do widoku szczegółowego** , aby zobaczyć więcej szczegółów na temat odpowiedzi tego wywołania interfejsu API.

    ![Przełącz do widoku szczegółowego](build-restful-apis-with-aspnet-web-api/_static/image8.png "Przełącz do widoku szczegółów")

    *Przełącz do widoku szczegółowego*
9. Kliknij kartę **treść odpowiedzi** , aby wyświetlić rzeczywisty tekst odpowiedzi JSON.

    ![Wyświetlanie tekstu wyjściowego JSON w monitorze sieci](build-restful-apis-with-aspnet-web-api/_static/image9.png "Wyświetlanie tekstu wyjściowego JSON w monitorze sieci")

    *Wyświetlanie tekstu wyjściowego JSON w monitorze sieci*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Zadanie 3 — Tworzenie modeli kontaktów i rozszerzanie kontrolera kontaktów

W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.

1. Kliknij prawym przyciskiem myszy folder **modele** i wybierz polecenie **Dodaj | Klasa...** z menu kontekstowego.

    ![Dodawanie nowego modelu do aplikacji sieci Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Dodawanie nowego modelu do aplikacji sieci Web")

    *Dodawanie nowego modelu do aplikacji sieci Web*
2. W oknie dialogowym **Dodaj nowy element** Nazwij nowy plik **Contact.cs** a następnie kliknij przycisk **Dodaj.**

    ![Tworzenie nowego pliku klasy kontaktu](build-restful-apis-with-aspnet-web-api/_static/image11.png "Tworzenie nowego pliku klasy kontaktu")

    *Tworzenie nowego pliku klasy kontaktu*
3. Dodaj następujący wyróżniony kod do klasy **Contact** .

    (Fragment kodu- *Web API Lab-Ex01-Contact Class*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. W klasie **ContactController** wybierz **ciąg** wyrazu w definicji metody metody **Get** , a następnie wpisz słowo *Contact*. Po wpisaniu wyrazu w programie wskaźnik pojawi się na początku **kontaktu z**programem Word. Przytrzymaj wciśnięty klawisz **Ctrl** i naciśnij klawisz kropki (.) lub kliknij ikonę przy użyciu myszy, aby otworzyć okno dialogowe pomocy w edytorze kodu, aby automatycznie wypełnić dyrektywę **using** dla przestrzeni nazw models.

    ![Korzystanie z pomocy IntelliSense dla deklaracji przestrzeni nazw](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Korzystanie z pomocy IntelliSense dla deklaracji przestrzeni nazw*
5. Zmodyfikuj kod metody **Get** , aby zwracała tablicę wystąpień modelu kontaktu.

    (Fragment kodu — *Web API Lab-Ex01 — zwracanie listy kontaktów*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Naciśnij klawisz **F5** , aby debugować aplikację sieci Web w przeglądarce. Aby wyświetlić zmiany wprowadzone do danych wyjściowych odpowiedzi interfejsu API, wykonaj następujące czynności.

   1. Po otwarciu przeglądarki naciśnij klawisz **F12** , jeśli narzędzia deweloperskie nie są jeszcze otwarte.
   2. Kliknij kartę **Sieć** .
   3. Naciśnij przycisk **Rozpocznij przechwytywanie** .
   4. Dodaj sufiks adresu URL **/API/Contact** do adresu URL na pasku adresu i naciśnij klawisz **Enter** .
   5. Naciśnij przycisk **Przejdź do widoku szczegółowego** .
   6. Wybierz kartę **treść odpowiedzi** . Powinien zostać wyświetlony ciąg JSON reprezentujący serializowaną postać tablicy wystąpień kontaktów.

      ![Serializowane dane wyjściowe JSON złożonego wywołania metody interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image13.png "Serializowane dane wyjściowe JSON złożonego wywołania metody interfejsu API sieci Web")

      *Serializowane dane wyjściowe JSON złożonego wywołania metody interfejsu API sieci Web*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Zadanie 4 — wyodrębnianie funkcji do warstwy usług

To zadanie pokazuje, jak wyodrębnić funkcjonalność do warstwy usług, aby ułatwić deweloperom oddzielenie ich funkcjonalności usługi od warstwy kontrolera, co pozwala na wielokrotne wykorzystywanie usług, które faktycznie wykonują prace.

1. Utwórz nowy folder w katalogu głównym rozwiązania i nadaj mu nazwę **usługi**IT. W tym celu kliknij prawym przyciskiem myszy pozycję **ContactManager** Project, wybierz pozycję **Dodaj** | **Nowy folder**, nadaj nazwę *usługom*IT.

    ![Tworzenie folderu usług](build-restful-apis-with-aspnet-web-api/_static/image14.png "Tworzenie folderu usług")

    *Tworzenie folderu usług*
2. Kliknij prawym przyciskiem myszy folder **usługi** i wybierz polecenie **Dodaj | Klasa...** z menu kontekstowego.

    ![Dodawanie nowej klasy do folderu Services](build-restful-apis-with-aspnet-web-api/_static/image15.png "Dodawanie nowej klasy do folderu Services")

    *Dodawanie nowej klasy do folderu Services*
3. Gdy pojawi się okno dialogowe **Dodaj nowy element** , nazwij nową klasę **ContactRepository** i kliknij przycisk **Dodaj**.

    ![Tworzenie pliku klasy zawierającego kod dla warstwy usługi repozytorium kontaktów](build-restful-apis-with-aspnet-web-api/_static/image16.png "Tworzenie pliku klasy zawierającego kod dla warstwy usługi repozytorium kontaktów")

    *Tworzenie pliku klasy zawierającego kod dla warstwy usługi repozytorium kontaktów*
4. Dodaj dyrektywę using do pliku **ContactRepository.cs** , aby uwzględnić przestrzeń nazw models.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Dodaj następujący wyróżniony kod do pliku **ContactRepository.cs** , aby zaimplementować metodę GetAllContacts.

    (Fragment kodu — *Web API Lab-Ex01-contacting Repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Otwórz plik **ContactController.cs** , jeśli nie jest jeszcze otwarty.
7. Dodaj następującą instrukcję using do sekcji deklaracji przestrzeni nazw pliku.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Dodaj następujący wyróżniony kod do klasy **ContactController.cs** , aby dodać pole prywatne do reprezentowania wystąpienia repozytorium, dzięki czemu pozostałe elementy członkowskie klasy mogą korzystać z implementacji usługi.

    (Fragment kodu — *Web API Lab-Ex01-Contact Controller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Zmień metodę **Get** tak, aby korzystała z usługi repozytorium kontaktów.

    (Fragment kodu — *Web API Lab-Ex01 — zwraca listę kontaktów za pośrednictwem repozytorium*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Umieść punkt przerwania w definicji metody **Get** **ContactController**.

   ![Dodawanie punktów przerwania do kontrolera kontaktów](build-restful-apis-with-aspnet-web-api/_static/image17.png "Dodawanie punktów przerwania do kontrolera kontaktów")

   *Dodawanie punktów przerwania do kontrolera kontaktów*
11. Naciśnij klawisz **F5**, aby uruchomić aplikację.
12. Po otwarciu przeglądarki naciśnij klawisz **F12** , aby otworzyć narzędzia deweloperskie.
13. Kliknij kartę **Sieć** .
14. Kliknij przycisk **Rozpocznij przechwytywanie** .
15. Dołącz adres URL na pasku adresu przy użyciu sufiksu **/API/Contact** i naciśnij klawisz **Enter** , aby załadować kontroler interfejsu API.
16. Program Visual Studio 2012 powinien zostać przerwany po rozpoczęciu wykonywania metody **Get** .

   ![Przerywanie w ramach metody get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Przerywanie w ramach metody get")

   *Przerywanie w ramach metody get*
17. Naciśnij klawisz **F5**, aby kontynuować.
18. Wróć do programu Internet Explorer, jeśli nie jest jeszcze fokusem. Zanotuj okno Przechwytywanie sieci.

    ![Widok sieci w programie Internet Explorer przedstawiający wyniki wywołania interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Widok sieci w programie Internet Explorer przedstawiający wyniki wywołania interfejsu API sieci Web")

    *Widok sieci w programie Internet Explorer przedstawiający wyniki wywołania interfejsu API sieci Web*
19. Kliknij przycisk **Przejdź do widoku szczegółowego** .
20. Kliknij kartę **treść odpowiedzi** . Zanotuj dane wyjściowe JSON wywołania interfejsu API i sposób przedstawiający dwa kontakty pobrane przez warstwę usług.

    ![Wyświetlanie danych wyjściowych JSON z internetowego interfejsu API w oknie narzędzia deweloperskie](build-restful-apis-with-aspnet-web-api/_static/image20.png "Wyświetlanie danych wyjściowych JSON z internetowego interfejsu API w oknie narzędzia deweloperskie")

    *Wyświetlanie danych wyjściowych JSON z internetowego interfejsu API w oknie narzędzia deweloperskie*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Ćwiczenie 2: Tworzenie internetowego interfejsu API do odczytu/zapisu

W tym ćwiczeniu zostaną zaimplementowane metody POST i PUT dla Menedżera kontaktów, aby umożliwić korzystanie z funkcji edytowania danych.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Zadanie 1 — Otwieranie projektu interfejsu API sieci Web

W tym zadaniu zostanie przygotowana do udoskonalenia projektu internetowego interfejsu API utworzonego w ćwiczeniu 1, aby mógł akceptować dane wprowadzane przez użytkownika.

1. Uruchom **program Visual Studio 2012 Express for Web**, w tym celu przejdź do **menu Start** , a następnie wpisz **vs Express for Web** następnie naciśnij klawisz **Enter**.
2. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex02-ReadWriteWebAPI/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Otwórz plik **Services/ContactRepository. cs** .

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Zadanie 2 — Dodawanie funkcji trwałości danych do implementacji repozytorium kontaktów

W tym zadaniu zostanie zaakceptowana Klasa ContactRepository projektu internetowego interfejsu API utworzona w ćwiczeniu 1, dzięki czemu może ona utrzymywać i akceptować dane wejściowe użytkownika i nowe wystąpienia kontaktów.

1. Dodaj następującą stałą do klasy **ContactRepository** , aby reprezentować nazwę klucza elementu pamięci podręcznej serwera sieci Web w dalszej części tego ćwiczenia.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Dodaj konstruktora do **ContactRepository** zawierającego Poniższy kod.

    (Fragment kodu — *Web API Lab-Ex02-contacting Konstruktor repozytorium*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Zmodyfikuj kod metody **GetAllContacts** , jak pokazano poniżej.

    (Fragment kodu- *Web API Lab-Ex02-Get All Contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Ten przykład jest przeznaczony dla celów demonstracyjnych i będzie używać pamięci podręcznej serwera sieci Web jako nośnika magazynu, dzięki czemu wartości będą dostępne jednocześnie dla wielu klientów, a nie przy użyciu mechanizmu magazynu sesji lub okresu istnienia żądania. Jedna z nich może korzystać z Entity Framework, magazynu XML lub dowolnej innej odmiany zamiast pamięci podręcznej serwera sieci Web.
4. Zaimplementuj nową metodę o nazwie **SaveContact** z klasą **ContactRepository** , aby wykonać zadania zapisywania kontaktu. Metoda **SaveContact** powinna przyjmować pojedynczy parametr **kontaktu** i zwracać wartość logiczną wskazującą powodzenie lub niepowodzenie.

    (Fragment kodu — *Web API Lab-Ex02-implementująca metodę SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Ćwiczenie 3: korzystanie z internetowego interfejsu API z klienta HTML

W tym ćwiczeniu utworzysz klienta HTML, który wywoła internetowy interfejs API. Ten klient ułatwi wymianę danych z interfejsem API sieci Web przy użyciu języka JavaScript i wyświetla wyniki w przeglądarce internetowej przy użyciu znacznika HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Zadanie 1 — Modyfikowanie widoku indeksu w celu zapewnienia graficznego interfejsu użytkownika do wyświetlania kontaktów

W tym zadaniu zmodyfikujesz domyślny widok indeksu aplikacji sieci Web, który będzie obsługiwał wymaganie wyświetlania listy istniejących kontaktów w przeglądarce HTML.

1. Otwórz **program Visual Studio 2012 Express for Web** , jeśli nie jest jeszcze otwarty.
2. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex03-ConsumingWebAPI/BEGIN/** folder. W przeciwnym razie można nadal korzystać z rozwiązania **końcowego** uzyskanego przez zakończenie poprzedniego ćwiczenia.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Otwórz plik **index. cshtml** znajdujący się w folderze **widoki/główne** .
4. Zastąp kod HTML w elemencie DIV identyfikatorem **treści** , tak aby wyglądał jak poniższy kod.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Dodaj następujący kod JavaScript w dolnej części pliku, aby wykonać żądanie HTTP do internetowego interfejsu API.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Otwórz plik **ContactController.cs** , jeśli nie jest jeszcze otwarty.
7. Umieść punkt przerwania w metodzie **Get** klasy **ContactController** .

    ![Umieszczanie punktu przerwania w metodzie GET kontrolera interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Umieszczanie punktu przerwania w metodzie GET kontrolera interfejsu API")

    *Umieszczanie punktu przerwania w metodzie GET kontrolera interfejsu API*
8. Naciśnij klawisz **F5** , aby uruchomić projekt. Przeglądarka załaduje dokument HTML.

    > [!NOTE]
    > Upewnij się, że przeglądasz adres URL katalogu głównego aplikacji.
9. Po załadowaniu strony i wykonaniu kodu JavaScript zostanie osiągnięty punkt przerwania, a wykonywanie kodu zostanie wstrzymane na kontrolerze.

    ![Debugowanie do wywołań interfejsu API sieci Web przy użyciu VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugowanie do wywołań interfejsu API sieci Web przy użyciu VS Express for Web")

    *Debugowanie do wywołania interfejsu API sieci Web za pomocą programu Visual Studio 2012 Express for Web*
10. Usuń punkt przerwania i naciśnij klawisz **F5** lub przycisk **Kontynuuj** debugowania paska narzędzi, aby kontynuować ładowanie widoku w przeglądarce. Po zakończeniu wywołania interfejsu API sieci Web powinny zostać wyświetlone kontakty zwrócone z wywołania interfejsu API sieci Web jako elementy listy w przeglądarce.

    ![Wyniki wywołania interfejsu API wyświetlane w przeglądarce jako elementy listy](build-restful-apis-with-aspnet-web-api/_static/image23.png "Wyniki wywołania interfejsu API wyświetlane w przeglądarce jako elementy listy")

    *Wyniki wywołania interfejsu API wyświetlane w przeglądarce jako elementy listy*
11. Zatrzymaj debugowanie.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Zadanie 2 — Modyfikowanie widoku indeksu w celu zapewnienia graficznego interfejsu użytkownika do tworzenia kontaktów

W tym zadaniu będziesz w dalszym ciągu modyfikować widok indeksu aplikacji MVC. Do strony HTML zostanie dodany formularz, który będzie przechwytywać dane wejściowe użytkownika i wysyłany do internetowego interfejsu API w celu utworzenia nowego kontaktu, a nowa metoda kontrolera internetowego interfejsu API zostanie utworzona w celu zebrania daty z graficznego interfejsu użytkownika.

1. Otwórz plik **ContactController.cs** .
2. Dodaj nową metodę do klasy kontrolera o nazwie **post** , jak pokazano w poniższym kodzie.

    (Fragment kodu- *Web API Lab-Ex03-post-Metoda*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Otwórz plik **index. cshtml** w programie Visual Studio, jeśli nie jest jeszcze otwarty.
4. Dodaj poniższy kod HTML do pliku tuż po liście nieuporządkowanej, która została dodana w poprzednim zadaniu.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. W elemencie skryptu w dolnej części dokumentu Dodaj następujący wyróżniony kod, aby obsłużyć zdarzenia klikania przycisku, co spowoduje opublikowanie danych w interfejsie API sieci Web przy użyciu wywołania HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. W **ContactController.cs**Umieść punkt przerwania w metodzie **post** .
7. Naciśnij klawisz **F5** , aby uruchomić aplikację w przeglądarce.
8. Po załadowaniu strony w przeglądarce wpisz nową nazwę kontaktu i identyfikator, a następnie kliknij przycisk **Zapisz** .

    ![Dokument HTML klienta załadowany w przeglądarce](build-restful-apis-with-aspnet-web-api/_static/image24.png "Dokument HTML klienta załadowany w przeglądarce")

    *Dokument HTML klienta załadowany w przeglądarce*
9. Gdy okno debugera jest zerwane w metodzie **post** , zapoznaj się z właściwościami parametru **Contact** . Wartości powinny być zgodne z danymi wprowadzonymi w formularzu.

    ![Obiekt Contact wysyłany do internetowego interfejsu API z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "Obiekt Contact wysyłany do internetowego interfejsu API z klienta")

    *Obiekt Contact wysyłany do internetowego interfejsu API z klienta*
10. Przechodzenie przez metodę w debugerze do momentu utworzenia zmiennej **odpowiedzi** . Po inspekcji w oknie **zmiennych lokalnych** w debugerze zobaczysz, że wszystkie właściwości zostały ustawione.

   ![Odpowiedź następująca po utworzeniu w debugerze](build-restful-apis-with-aspnet-web-api/_static/image26.png "Odpowiedź następująca po utworzeniu w debugerze")

   *Odpowiedź następująca po utworzeniu w debugerze*
11. Jeśli naciśniesz klawisz **F5** lub klikniesz przycisk **Kontynuuj** w debugerze, żądanie zostanie ukończone. Po ponownym przełączeniu do przeglądarki nowy kontakt został dodany do listy kontaktów przechowywanych przez implementację **ContactRepository** .

   ![Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu](build-restful-apis-with-aspnet-web-api/_static/image27.png "Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu")

   *Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu*

> [!NOTE]
> Ponadto możesz wdrożyć tę aplikację na platformie Azure, postępując zgodnie z [dodatkiem C: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixC).

---

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium wprowadzono nową strukturę interfejsu API sieci Web ASP.NET oraz implementację interfejsów API sieci Web RESTful przy użyciu struktury. W tym miejscu można utworzyć nowe repozytorium, które ułatwia trwałość danych przy użyciu dowolnej liczby mechanizmów i drutu, a nie prostego, dostarczonego jako przykład w tym laboratorium. Interfejs API sieci Web obsługuje wiele dodatkowych funkcji, takich jak Włączanie komunikacji od klientów innych niż HTML pisanych w dowolnym języku, który obsługuje protokoły HTTP i JSON lub XML. Możliwe jest również hostowanie internetowego interfejsu API poza typową aplikacją sieci Web, a także możliwość tworzenia własnych formatów serializacji.

Witryna sieci Web ASP.NET ma obszar dedykowany struktury internetowego interfejsu API ASP.NET w [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api). Ta lokacja będzie w dalszym ciągu dostarczać najświeższe informacje, przykłady i wiadomości związane z interfejsem API sieci Web, dlatego należy sprawdzać je często, jeśli chcesz uzyskać więcej informacji na temat tworzenia niestandardowych interfejsów API sieci Web dostępnych dla praktycznie dowolnego urządzenia lub platformy programistycznej.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Dodatek A: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](build-restful-apis-with-aspnet-web-api/_static/image28.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

    ![Zacznij wpisywać nazwę fragmentu kodu](build-restful-apis-with-aspnet-web-api/_static/image29.png "Zacznij wpisywać nazwę fragmentu kodu")

    *Zacznij wpisywać nazwę fragmentu kodu*

    ![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](build-restful-apis-with-aspnet-web-api/_static/image30.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

    *Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

    ![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](build-restful-apis-with-aspnet-web-api/_static/image31.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

    *Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)

1. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.
2. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
3. Wybierz odpowiedni fragment kodu z listy, klikając go.

    ![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

    *Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

    ![Wybierz odpowiedni fragment kodu z listy, klikając go](build-restful-apis-with-aspnet-web-api/_static/image33.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

    *Wybierz odpowiedni fragment kodu z listy, klikając go*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Dodatek B: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli wcześniej zainstalowano Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Kafelek VS Express for Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek C: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web w witrynie Azure Portal i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy oferowanej przez platformę Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web w witrynie Azure Portal

1. Przejdź do [usługi Azure Portal zarządzania](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Dzięki platformie Azure możesz bezpłatnie hostować 10 ASP.NET witryn sieci Web, a następnie skalować je w miarę wzrostu ruchu. Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do systemu Windows Azure Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Zaloguj się do systemu Windows Azure Portal")

    *Zaloguj się do portalu*
2. Na pasku poleceń kliknij pozycję **Nowy** .

    ![Tworzenie nowej witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Tworzenie nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**. Następnie wybierz opcję **szybkie tworzenie** . Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Platforma Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią. Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web na platformie Azure spoza portalu. Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](build-restful-apis-with-aspnet-web-api/_static/image41.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")

    *Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*
4. Poczekaj na utworzenie nowej **witryny sieci Web** .
5. Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** . Sprawdź, czy nowa witryna sieci Web działa.

    ![Przeglądanie w nowej witrynie sieci Web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Przeglądanie w nowej witrynie sieci Web")

    *Przeglądanie w nowej witrynie sieci Web*

    ![Uruchomiona witryna sieci Web](build-restful-apis-with-aspnet-web-api/_static/image43.png "Uruchomiona witryna sieci Web")

    *Uruchomiona witryna sieci Web*
6. Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania witryną sieci Web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Otwieranie stron zarządzania witryną sieci Web")

    *Otwieranie stron zarządzania witryną sieci Web*
7. Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web na platformie Azure dla każdej z włączonych metod publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji. **Firma Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów na potrzeby publikowania aplikacji sieci Web na platformie Azure.

    ![Pobieranie profilu publikowania witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Pobieranie profilu publikowania witryny sieci Web")

    *Pobieranie profilu publikowania witryny sieci Web*
8. Pobierz plik profilu publikowania do znanej lokalizacji. W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web na platformie Azure z programu Visual Studio.

    ![Zapisywanie pliku profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image46.png "Zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer. Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.

1. Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database. Możesz wyświetlić SQL Database serwery z subskrypcji w portalu zarządzania Azure w obszarze **bazy danych SQL** | **serwery** | **pulpicie nawigacyjnym serwera**. Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń. Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach. Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.

    ![Pulpit nawigacyjny serwera SQL Database](build-restful-apis-with-aspnet-web-api/_static/image47.png "Pulpit nawigacyjny serwera SQL Database")

    *Pulpit nawigacyjny serwera SQL Database*
2. W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera. W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](build-restful-apis-with-aspnet-web-api/_static/image48.png).

    ![Dodawanie adresu IP klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Dodawanie adresu IP klienta*
3. Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.

    ![Potwierdź zmiany](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Potwierdź zmiany*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.

    ![Publikowanie aplikacji](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publikowanie aplikacji")

    *Publikowanie witryny sieci Web*
2. Zaimportuj profil publikowania zapisany w pierwszym zadaniu.

    ![Importowanie profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij pozycję **Weryfikuj połączenie**. Po zakończeniu walidacji kliknij przycisk **dalej**.

    > [!NOTE]
    > Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.

    ![Weryfikowanie połączenia](build-restful-apis-with-aspnet-web-api/_static/image53.png "Weryfikowanie połączenia")

    *Weryfikowanie połączenia*
4. Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).

    ![Konfiguracja narzędzia Web Deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Konfiguracja narzędzia Web Deploy")

    *Konfiguracja narzędzia Web Deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .
   - W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.
   - W polu **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nową nazwę bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia docelowego](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurowanie parametrów połączenia docelowego")

     *Konfigurowanie parametrów połączenia docelowego*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.

    ![Tworzenie bazy danych](build-restful-apis-with-aspnet-web-api/_static/image56.png "Tworzenie ciągu bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do nawiązywania połączenia z SQL Database w systemie Windows Azure, są wyświetlane w domyślnym polu tekstowym połączenie. Następnie kliknij przycisk **Next** (Dalej).

    ![Parametry połączenia wskazujące SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Parametry połączenia wskazujące SQL Database")

    *Parametry połączenia wskazujące SQL Database*
8. Na stronie **Podgląd** kliknij przycisk **Publikuj**.

    ![Publikowanie aplikacji sieci Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publikowanie aplikacji sieci Web")

    *Publikowanie aplikacji sieci Web*
9. Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.

    ![Aplikacja opublikowana w systemie Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Aplikacja opublikowana w systemie Windows Azure")

    *Aplikacja opublikowana na platformie Azure*

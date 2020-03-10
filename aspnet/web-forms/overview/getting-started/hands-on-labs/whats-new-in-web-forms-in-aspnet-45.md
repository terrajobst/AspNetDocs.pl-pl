---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Co nowego w formularzach sieci Web w programie ASP.NET 4,5 | Microsoft Docs
author: rick-anderson
description: Nowa wersja ASP.NET Web Forms zawiera kilka ulepszeń, które koncentrują się na ulepszaniu środowiska użytkownika podczas pracy z danymi. W poprzednich wersjach programu...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 301af8ed877b58624e419c04f605c41f27dbdd0c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525733"
---
# <a name="whats-new-in-web-forms-in-aspnet-45"></a>Co nowego we wzorcu Web Forms na platformie ASP.NET 4.5

przez [zespół Camp sieci Web](https://twitter.com/webcamps)

> Nowa wersja ASP.NET Web Forms zawiera kilka ulepszeń, które koncentrują się na ulepszaniu środowiska użytkownika podczas pracy z danymi.
> 
> W poprzednich wersjach formularzy sieci Web, gdy przy użyciu powiązania danych do emisji wartości elementu członkowskiego obiektu użyto wyrażeń powiązania danych bind () lub eval (). W nowej wersji ASP.NET można zadeklarować typ danych, z którymi formant ma być powiązany, przy użyciu nowej właściwości ItemType. Ustawienie tej właściwości umożliwi użycie zmiennej o jednoznacznie określonym typie w celu uzyskania pełnych korzyści związanych z programowaniem Visual Studio, takich jak IntelliSense, Nawigacja członków i sprawdzanie w czasie kompilacji.
> 
> Formanty powiązane z danymi umożliwiają teraz również określanie własnych metod niestandardowych do wybierania, aktualizowania, usuwania i wstawiania danych, uproszczenia interakcji między kontrolkami stron i logiką aplikacji. Ponadto możliwości powiązania modelu zostały dodane do ASP.NET, co oznacza, że można mapować dane ze strony bezpośrednio do parametrów typu metody.
> 
> Sprawdzanie poprawności danych wejściowych użytkownika powinno być również łatwiejsze przy użyciu najnowszej wersji formularzy sieci Web. Teraz można dodać adnotacje do klas modelu z atrybutami walidacji z przestrzeni nazw **System. ComponentModel. DataAnnotations** i zażądać, aby wszystkie formanty lokacji sprawdzali poprawność danych wejściowych użytkownika przy użyciu tych informacji. Sprawdzanie poprawności po stronie klienta w formularzach sieci Web jest teraz zintegrowane z platformą jQuery, dostarczając kod po stronie klienta i niedyskretne funkcje języka JavaScript.
> 
> W obszarze Walidacja żądania wprowadzono ulepszenia ułatwiające wybiórcze Wyłączanie sprawdzania poprawności żądań dla określonych części aplikacji lub odczytywanie unieważnionych danych żądania.
> 
> Wprowadzono kilka ulepszeń w kontrolkach serwera formularzy sieci Web, aby korzystać z nowych funkcji języka HTML5:
> 
> - Właściwość TextMode kontrolki TextBox została zaktualizowana w celu obsługi nowych typów danych wejściowych HTML5, takich jak wiadomości e-mail, DateTime i tak dalej.
> - Kontrolka FileUpload obsługuje teraz wiele operacji przekazywania plików z przeglądarek, które obsługują tę funkcję HTML5.
> - Kontrolki walidatora obsługują teraz sprawdzanie poprawności elementów wejściowych HTML5.
> - Nowe elementy HTML5, które mają atrybuty reprezentujące adres URL, obsługują teraz&quot;runat =&quot;Server. W związku z tym można używać konwencji ASP.NET w ścieżkach URL, takich jak operator ~, aby reprezentować katalog główny aplikacji (na przykład &lt;Video runat =&quot;Server&quot; src =&quot;~/myVideo.wmv&quot;&gt;&lt;/Video&gt;).
> - Kontrolka UpdatePanel została naprawiona w celu obsługi publikowania pól wejściowych HTML5.
> 
> W oficjalnym portalu ASP.NET znajdziesz więcej przykładów nowych funkcji w ASP.NET WebForms 4,5: [co nowego w ASP.NET 4,5 i Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym ćwiczeniu dowiesz się, jak:

- Używanie wyrażeń z powiązaniem danych o jednoznacznie określonym typie
- Korzystanie z nowych funkcji powiązania modelu w formularzach sieci Web
- Korzystanie z dostawców wartości na potrzeby mapowania danych strony do metod związanych z kodem
- Używanie adnotacji danych do sprawdzania poprawności danych wejściowych użytkownika
- Korzystaj z niezauważalnej weryfikacji po stronie klienta za pomocą technologii jQuery w formularzach sieci Web
- Zaimplementuj szczegółowe sprawdzanie poprawności żądania
- Implementuj asynchroniczne przetwarzanie stron w formularzach sieci Web

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć to laboratorium, musisz mieć następujące elementy:

- [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub nadrzędnych (Przeczytaj [dodatek A,](#AppendixA) Aby uzyskać instrukcje na temat sposobu jego instalacji).

<a id="Setup"></a>
### <a name="setup"></a>Konfigurowanie

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany w tym laboratorium, jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować plik fragmentów kodu, uruchom **.\Source\Setup\CodeSnippets.VSI** .

Jeśli nie znasz fragmentów kodu Visual Studio Code i chcesz dowiedzieć się, jak z nich korzystać, możesz odwołać się do dodatku z tego dokumentu &quot;[dodatku C: Using fragmenty kodu](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Symulacyjn

To laboratorium praktyczne obejmuje następujące ćwiczenia:

1. [Ćwiczenie 1: powiązanie modelu w formularzach sieci Web ASP.NET](#Exercise1)
2. [Ćwiczenie 2: sprawdzanie poprawności danych](#Exercise2)
3. [Ćwiczenie 3: asynchroniczne przetwarzanie stron w formularzach sieci Web ASP.NET](#Exercise3)

> [!NOTE]
> Każdemu ćwiczeniu towarzyszy folder **końcowy** zawierający rozwiązanie, które należy uzyskać po zakończeniu ćwiczenia. Tego rozwiązania można użyć jako przewodnika, jeśli potrzebujesz dodatkowej pomocy w pracy przez ćwiczenia.

Szacowany czas wykonywania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Ćwiczenie 1: powiązanie modelu w formularzach sieci Web ASP.NET

Nowa wersja ASP.NET Web Forms wprowadza szereg ulepszeń, które koncentrują się na ulepszaniu środowiska podczas pracy z danymi. W tym ćwiczeniu dowiesz się więcej na temat kontroli danych o jednoznacznie określonym typie i powiązania modelu.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Zadanie 1 — Używanie powiązań danych o jednoznacznie określonym typie

W tym zadaniu zostaną wykryte nowe powiązania o jednoznacznie określonym typie dostępne w ASP.NET 4,5.

1. Otwórz rozwiązanie **BEGIN** znajdujące się w lokalizacji **Source/Ex1-ModelBinding/BEGIN/** folder.

   1. Przed kontynuowaniem musisz pobrać brakujące pakiety NuGet. W tym celu kliknij menu **projekt** i wybierz polecenie **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz stronę **Customers. aspx** . Umieść nienumerowaną listę w formancie głównym i Dołącz do niej kontrolkę wzmacniak, aby wyświetlić listę poszczególnych klientów. Ustaw nazwę wzmacniak na **customersRepeater** , jak pokazano w poniższym kodzie.

    W poprzednich wersjach formularzy sieci Web przy użyciu powiązania danych do emisji wartości elementu członkowskiego obiektu, do którego jest powiązane dane, należy użyć wyrażenia powiązania danych oraz wywołania metody Eval, przekazując nazwę elementu członkowskiego jako ciąg.

    W czasie wykonywania te wywołania oceny będą używać odbicia względem aktualnie powiązanego obiektu, aby odczytać wartość elementu członkowskiego o podaną nazwę, a następnie wyświetlić wynik w kodzie HTML. Takie podejście ułatwia powiązanie danych z dowolnymi, niekształtnymi danymi.

    Niestety, stracisz wiele doskonałych funkcji środowiska programistycznego w programie Visual Studio, w tym IntelliSense dla nazw członków, obsługę nawigacji (na przykład przejdź do definicji) i sprawdzanie czasu kompilacji.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Otwórz plik **Customers.aspx.cs** .
4. Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Na **stronie\_Metoda ładowania** Dodaj kod, aby wypełnić wzmacniak listą klientów.

    (Fragment kodu- *Web Forms Lab-Ex01 — źródło danych powiązanych klientów*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Do tworzenia bazy danych i uzyskiwania dostępu do niej jest stosowane rozwiązanie EntityFramework razem z CodeFirst. W poniższym kodzie customersRepeater jest powiązany z zapytaniem z materiałami, które zwraca wszystkich klientów z bazy danych.
6. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie, a następnie przejdź do strony **klienci** , aby zobaczyć wzmacniak w akcji. Ponieważ rozwiązanie korzysta z usługi CodeFirst, baza danych zostanie utworzona i wypełniona w lokalnym wystąpieniu programu SQL Express podczas uruchamiania aplikacji.

    ![Wyświetlanie listy klientów z wzmacniaką](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Wyświetlanie listy klientów z wzmacniaką")

    *Wyświetlanie listy klientów z wzmacniaką*

    > [!NOTE]
    > W programie Visual Studio 2012 IIS Express jest domyślnym serwerem programu Web Development.
7. Zamknij przeglądarkę i wróć do programu Visual Studio.
8. Teraz Zastąp implementację, aby używać powiązań silnie wpisanych. Otwórz stronę **Customers. aspx** i Użyj nowego atrybutu **ItemType** w wzmacniake, aby ustawić typ **klienta** jako typ powiązania.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Właściwość ItemType umożliwia zadeklarować typ danych, z którymi formant ma być powiązany, i umożliwia użycie wiązania z jednoznacznie określonymi typami wewnątrz kontrolki powiązanej z danymi.
9. Zastąp zawartość ItemTemplate następującym kodem.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Jeden minusem z powyższymi metodami polega na tym, że wywołania metody Eval () i bind () są z opóźnieniem, co oznacza, że są przekazywane ciągi do reprezentowania nazw właściwości. Oznacza to, że nie otrzymujesz funkcji IntelliSense dla nazw składowych, obsługi nawigacji w kodzie (np. Przejdź do definicji) ani do sprawdzania dostępności w czasie kompilacji.

    Ustawienie właściwości ItemType powoduje generowanie dwóch nowych zmiennych wpisanych w zakresie wyrażeń powiązania danych: **Item** i **binditem**. Możesz użyć tych zmiennych silnie wpisanych w wyrażeniach powiązania danych i uzyskać pełne zalety środowiska programistycznego Visual Studio.

    &quot; **:** &quot; użyte w wyrażeniu automatycznie koduje dane wyjściowe, aby uniknąć problemów z zabezpieczeniami (na przykład ataki skryptów między lokacjami). Ta notacja była dostępna od wersji .NET 4 do zapisu odpowiedzi, ale teraz jest również dostępna w wyrażeniach powiązań danych.

    > [!NOTE]
    > Element członkowski elementu działa dla jednokierunkowego powiązania. Jeśli chcesz wykonać dwukierunkowe powiązanie, użyj elementu członkowskiego **binditem** .

    ![Obsługa technologii IntelliSense w przypadku powiązania o jednoznacznie określonym typie](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "Obsługa technologii IntelliSense w przypadku powiązania o jednoznacznie określonym typie")

    *Obsługa technologii IntelliSense w przypadku powiązania o jednoznacznie określonym typie*
10. Naciśnij klawisz **F5** , aby uruchomić rozwiązanie, a następnie przejdź do strony Klienci, aby upewnić się, że zmiany działają zgodnie z oczekiwaniami.

    ![Wyświetlanie szczegółów klienta](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Wyświetlanie szczegółów klienta")

    *Wyświetlanie szczegółów klienta*
11. Zamknij przeglądarkę i wróć do programu Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Zadanie 2 — wprowadzenie powiązania modelu w formularzach sieci Web

W poprzednich wersjach formularzy sieci Web ASP.NET, gdy chciałem wykonać dwukierunkowe powiązanie danych, pobierając i aktualizując dane, musisz użyć obiektu źródła danych. Może to być źródło danych obiektu, źródło danych SQL, źródło danych LINQ i tak dalej. Jeśli jednak scenariusz wymaga niestandardowego kodu do obsługi danych, konieczne jest użycie źródła danych obiektu i nastąpiło kilka wad. Na przykład konieczne jest uniknięcie typów złożonych i wymaganie obsługi wyjątków podczas wykonywania logiki walidacji.

W nowej wersji ASP.NET Web Forms formanty powiązane z danymi obsługują powiązania modelu. Oznacza to, że można określić metody Select, Update, INSERT i DELETE bezpośrednio w kontrolce powiązanej z danymi, aby wywoływać logikę z pliku związanego z kodem lub z innej klasy.

Aby dowiedzieć się więcej o tym, będziesz używać widoku GridView do wyświetlania kategorii produktów przy użyciu nowego atrybutu **SelectMethod** . Ten atrybut umożliwia określenie metody pobierania danych GridView.

1. Otwórz stronę **Products. aspx** i Dołącz widok **GridView**. Skonfiguruj widok GridView, jak pokazano poniżej, aby użyć powiązań z silną typem i włączyć sortowanie i stronicowanie.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Użyj nowego atrybutu **SelectMethod** , aby skonfigurować widok GridView do wywoływania metody **GetCategories** w celu wybrania danych.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Otwórz plik związany z kodem **Products.aspx.cs** i Dodaj następujące instrukcje using.

    (Fragment kodu — *Web Forms Lab-Ex01-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Dodaj prywatny element członkowski w klasie **Products** i przypisz nowe wystąpienie elementu **ProductsContext**. Ta właściwość będzie przechowywać kontekst danych Entity Framework, który umożliwia nawiązanie połączenia z bazą danych.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Utwórz metodę **GetCategories** , aby pobrać listę kategorii za pomocą LINQ. Zapytanie będzie zawierać właściwość **Products (produkty** ), aby wyświetlić ilość produktów dla każdej kategorii. Zwróć uwagę, że metoda zwraca nieprzetworzony obiekt IQueryable reprezentujący zapytanie, które ma zostać wykonane później na cyklu życia strony.

    (Fragment kodu — *Web Forms Lab-Ex01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > W poprzednich wersjach formularzy sieci Web ASP.NET Włączanie sortowania i stronicowania przy użyciu własnej logiki repozytorium w kontekście źródła danych obiektów, co jest wymagane do pisania własnego niestandardowego kodu i uzyskiwania wszystkich wymaganych parametrów. Teraz, ponieważ metody wiązania danych mogą zwracać interfejs IQueryable, a to oznacza, że zapytanie jest nadal wykonywane, ASP.NET może zadbać o modyfikację zapytania, aby dodać odpowiednie parametry sortowania i stronicowania.
6. Naciśnij klawisz **F5** , aby rozpocząć debugowanie witryny i przejdź do strony produkty. Powinien zostać wyświetlony widok GridView z kategoriami zwracanymi przez metodę GetCategories.

    ![Wypełnianie widoku GridView przy użyciu powiązania modelu](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Wypełnianie widoku GridView przy użyciu powiązania modelu")

    *Wypełnianie widoku GridView przy użyciu powiązania modelu*
7. Naciśnij klawisz **SHIFT**+**F5** Zatrzymaj debugowanie.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Zadanie 3 — dostawcy wartości w powiązaniu modelu

Powiązanie modelu nie tylko pozwala określić niestandardowe metody do pracy z danymi bezpośrednio w kontrolce powiązanej z danymi, ale również umożliwia mapowanie danych ze strony na parametry z tych metod. W parametrze metody można użyć atrybutów dostawcy wartości, aby określić źródło danych wartości. Na przykład:

- Kontrolki na stronie
- Wartości ciągu zapytania
- Wyświetlanie danych
- Stan sesji
- Cookie
- Dane formularza ogłoszonego
- Wyświetl stan
- Niestandardowi dostawcy wartości są również obsługiwani

Jeśli korzystasz z ASP.NET MVC 4, zobaczysz, że obsługa powiązań modelu jest podobna. W rzeczywistości te funkcje zostały wykonane z ASP.NET MVC i przeniesiono do zestawu **System. Web** , aby można było używać ich również w formularzach sieci Web.

W tym zadaniu zostanie zaktualizowany widok GridView, aby przefiltrować wyniki według ilości produktów dla każdej kategorii, pobierając parametr filtru z powiązaniem modelu.

1. Wróć do strony **Products. aspx** .
2. W górnej części widoku GridView Dodaj **etykietę** i **pole kombi** , aby wybrać liczbę produktów dla każdej kategorii, jak pokazano poniżej.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Dodaj **EmptyDataTemplate** do widoku GridView, aby wyświetlić komunikat, gdy nie ma kategorii z wybraną liczbą produktów.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Otwórz kod **Products.aspx.cs** i Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Zmodyfikuj metodę **GetCategories** , aby otrzymać argument **minProductsCount** integer i odfiltrować zwrócone wyniki. Aby to zrobić, Zastąp metodę poniższym kodem.

    (Fragment kodu — *Web Forms Lab-Ex01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Nowy atrybut **[Control]** w argumencie **minProductsCount** umożliwia ASP.NET znać jego wartość musi zostać wypełniona przy użyciu kontrolki na stronie. ASP.NET będzie szukać dowolnej kontrolki zgodnej z nazwą argumentu (minProductsCount) i wykonać niezbędne mapowanie i konwersję, aby wypełnić parametr wartością kontrolną.

    Alternatywnie, atrybut zawiera przeciążony Konstruktor, który umożliwia określenie formantu, z którego ma zostać pobrana wartość.

    > [!NOTE]
    > Jednym z celów funkcji powiązania danych jest zmniejszenie ilości kodu, który należy napisać w celu interakcji ze strony. Oprócz dostawcy wartości [Control] można używać innych dostawców powiązań modelu w parametrach metody. Niektóre z nich są wymienione w temacie Wprowadzenie do zadania.
6. Naciśnij klawisz **F5** , aby rozpocząć debugowanie witryny i przejdź do strony produkty. Wybierz z listy rozwijanej liczbę produktów i zwróć uwagę na sposób, w jaki widok GridView został zaktualizowany.

    ![Filtrowanie widoku GridView przy użyciu wartości listy rozwijanej](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtrowanie widoku GridView przy użyciu wartości listy rozwijanej")

    *Filtrowanie widoku GridView przy użyciu wartości listy rozwijanej*
7. Zatrzymaj debugowanie.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Zadanie 4 — Używanie powiązania modelu do filtrowania

W tym zadaniu dodasz drugi, podrzędny element GridView, aby wyświetlić produkty należące do wybranej kategorii.

1. Otwórz stronę **Products. aspx** i zaktualizuj kategorię GridView, aby automatycznie wygenerować przycisk Wybierz.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Dodaj drugi element **GridView** o nazwie **productsGrid** w dolnej części strony. Ustaw **ItemType** na **WebFormsLab. model. Product**, **DataKeyNames ustawionej** na **ProductID** i **SelectMethod** dla **getProducts**. Ustaw **AutoGenerateColumns** na **false** i Dodaj kolumny dla ProductID, ProductName, Description i CenaJednostkowa.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Otwórz plik związany z kodem **Products.aspx.cs** . Zaimplementuj metodę **getProducts** , aby otrzymać identyfikator kategorii z kategorii GridView i przesączyć produkty. Powiązanie modelu ustawi wartość parametru przy użyciu wybranego wiersza w **categoriesGrid**. Ponieważ nazwa argumentu i nazwa kontrolki nie są zgodne, należy określić nazwę kontrolki w dostawcy wartości formantu.

    (Fragment kodu — *Web Forms Lab-Ex01-getProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Takie podejście ułatwia testowanie jednostkowe tych metod. W kontekście testu jednostkowego, w którym formularze sieci Web nie są wykonywane, atrybut [Control] nie będzie wykonywał żadnej konkretnej akcji.
4. Otwórz stronę **Products. aspx** i Znajdź produkty GridView. Zaktualizuj widok GridView produktów, aby wyświetlić link do edytowania wybranego produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Otwórz kod strony **ProductDetails. aspx** i Zastąp metodę **SelectProduct** następującym kodem.

    (Fragment kodu- *Web Forms Lab-Ex01-SelectProduct Metoda*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Zwróć uwagę, że atrybut **[QueryString]** jest używany do wypełniania parametru Method z parametru ProductID w ciągu zapytania.
6. Naciśnij klawisz **F5** , aby rozpocząć debugowanie witryny i przejdź do strony produkty. Wybierz dowolną kategorię z kategorii GridView i zwróć uwagę na to, że produkty GridView zostały zaktualizowane.

    ![Wyświetlanie produktów z wybranej kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Wyświetlanie produktów z wybranej kategorii")

    *Wyświetlanie produktów z wybranej kategorii*
7. Kliknij link **Wyświetl** w produkcie, aby otworzyć stronę ProductDetails. aspx.

    Zwróć uwagę, że Strona pobiera produkt za pomocą metody SelectMethod przy użyciu parametru productId z ciągu zapytania.

    ![Wyświetlanie szczegółów produktu](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Wyświetlanie szczegółów produktu")

    *Wyświetlanie szczegółów produktu*

    > [!NOTE]
    > Możliwość wpisania opisu HTML zostanie zaimplementowana w następnym ćwiczeniu.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Zadanie 5 — Używanie powiązania modelu dla operacji aktualizacji

W poprzednim zadaniu użyto powiązania modelu głównie do wybierania danych. w tym zadaniu dowiesz się, jak używać powiązania modelu w operacjach aktualizacji.

Zobaczysz aktualizację kategorii GridView, aby umożliwić użytkownikom aktualizowanie kategorii.

1. Otwórz stronę **Products. aspx** i zaktualizuj kategorię GridView, aby automatycznie wygenerować przycisk Edytuj i użyć nowego atrybutu **UpdateMethod** , aby określić metodę **UpdateCategory** w celu zaktualizowania wybranego elementu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Atrybut DataKeyNames ustawionej w widoku GridView definiuje, które są elementami członkowskimi, które jednoznacznie identyfikują obiekt powiązany z modelem i w związku z tym, które są parametrami, metoda Update powinna mieć co najmniej wartość Receive.
2. Otwórz plik związany z kodem **Products.aspx.cs** i Zaimplementuj metodę **UpdateCategory** . Metoda powinna odebrać identyfikator kategorii w celu załadowania bieżącej kategorii, wypełnić wartości z widoku GridView, a następnie zaktualizować kategorię.

    (Fragment kodu — *Web Forms Lab-Ex01-UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Nowa metoda **TryUpdateModel** w klasie Page jest odpowiedzialna za wypełnianie obiektu modelu przy użyciu wartości z formantów na stronie. W takim przypadku zastępuje zaktualizowane wartości z bieżącego wiersza GridView edytowanego do obiektu **kategorii** .

    > [!NOTE]
    > Następne ćwiczenie wyjaśnia użycie ModelState. IsValid do sprawdzania poprawności danych wprowadzonych przez użytkownika podczas edytowania obiektu.
3. Uruchom witrynę i przejdź do strony produkty. Edytuj kategorię. Wpisz nową nazwę, a następnie kliknij przycisk **Aktualizuj** , aby zachować zmiany.

    ![Edytowanie kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Edytowanie kategorii")

    *Edytowanie kategorii*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Ćwiczenie 2: sprawdzanie poprawności danych

W tym ćwiczeniu dowiesz się więcej o nowych funkcjach sprawdzania poprawności danych w programie ASP.NET 4,5. W formularzach sieci Web znajdziesz nowe niezauważalne funkcje sprawdzania poprawności. Będziesz używać adnotacji danych w klasach modelu aplikacji do sprawdzania poprawności danych wejściowych użytkownika, a wreszcie dowiesz się, jak włączyć lub wyłączyć weryfikację żądań do poszczególnych kontrolek na stronie.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Zadanie 1 — niezauważalne sprawdzenie poprawności

Formularze zawierające złożone dane, w tym moduły walidacji, w celu wygenerowania zbyt dużej ilości kodu JavaScript na stronie, które mogą reprezentować około 60% kodu. Po włączeniu dyskretnej weryfikacji kod HTML będzie wyglądał w sposób estetyczny i tidier.

W tej sekcji zostanie włączona niezauważalna weryfikacja w programie ASP.NET, aby porównać kod HTML wygenerowany przez obie konfiguracje.

1. Otwórz **program Visual Studio 2012** i Otwórz rozwiązanie **BEGIN** znajdujące się w folderze **Source\Ex2-Validation\Begin** w tym laboratorium. Alternatywnie możesz kontynuować pracę na istniejącym rozwiązaniu w poprzednim ćwiczeniu.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu w Eksplorator rozwiązań kliknij projekt **WebFormsLab** **Zarządzanie pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Naciśnij klawisz **F5** , aby uruchomić aplikację sieci Web. Przejdź do strony klienci i kliknij link **Dodaj nowego klienta** .
3. Kliknij prawym przyciskiem myszy na stronie przeglądarki i wybierz opcję **Wyświetl źródło** , aby otworzyć kod HTML wygenerowany przez aplikację.

    ![Wyświetlanie kodu HTML strony](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Wyświetlanie kodu HTML strony")

    *Wyświetlanie kodu HTML strony*
4. Przewiń kod źródłowy strony i zwróć uwagę, że ASP.NET wprowadza kod JavaScript i moduły sprawdzania poprawności danych na stronie, aby wykonać walidacje i wyświetlić listę błędów.

    ![Walidacja kodu JavaScript na stronie CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Walidacja kodu JavaScript na stronie CustomerDetails")

    *Walidacja kodu JavaScript na stronie CustomerDetails*
5. Zamknij przeglądarkę i wróć do programu Visual Studio.
6. Teraz będzie można włączyć niezauważalną weryfikację. Otwórz **plik Web. config** i Znajdź klucz **Właściwość: UnobtrusiveValidationMode** w sekcji AppSettings **.** Ustaw wartość klucza na **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Możesz również ustawić tę właściwość na **stronie &quot;\_** zdarzenia&quot; obciążenia, jeśli chcesz włączyć niedyskretną weryfikację tylko dla niektórych stron.
7. Otwórz **CustomerDetails. aspx** i naciśnij klawisz **F5** , aby uruchomić aplikację sieci Web.
8. Naciśnij klawisz F12, aby otworzyć narzędzia deweloperskie dla programu IE. Po otwarciu narzędzi programistycznych wybierz kartę skrypt. Wybierz **CustomerDetails. aspx** z menu i zwróć uwagę na to, że skrypty wymagane do uruchomienia platformy jQuery na stronie zostały załadowane do przeglądarki z lokacji lokalnej.

    ![Ładowanie plików JavaScript w języku jQuery bezpośrednio z lokalnego serwera usług IIS](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Ładowanie plików JavaScript w języku jQuery bezpośrednio z lokalnego serwera usług IIS")

    *Ładowanie plików JavaScript w języku jQuery bezpośrednio z lokalnego serwera usług IIS*
9. Zamknij przeglądarkę, aby powrócić do programu Visual Studio. Otwórz ponownie plik **site. Master** i Znajdź element **ScriptManager**. Dodaj właściwość **EnableCdn** atrybutu o wartości **true**. Spowoduje to wymuszenie załadowania platformy jQuery z adresu URL online, a nie adresu URL witryny lokalnej.
10. Otwórz **CustomerDetails. aspx** w programie Visual Studio. Naciśnij klawisz F5, aby uruchomić lokację. Po otwarciu programu Internet Explorer naciśnij klawisz F12, aby otworzyć narzędzia deweloperskie. Wybierz kartę **skrypt** , a następnie zapoznaj się z listą rozwijaną. Zwróć uwagę, że pliki JavaScript w języku jQuery nie są już ładowane z lokacji lokalnej, ale nie z sieci CDN online jQuery.

    ![Ładowanie plików JavaScript języka jQuery z sieci CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Ładowanie plików JavaScript języka jQuery z sieci CDN")

    *Ładowanie plików JavaScript języka jQuery z sieci CDN*
11. Ponownie otwórz kod źródłowy strony HTML przy użyciu opcji Wyświetl źródło w przeglądarce. Należy zauważyć, że umożliwienie niezauważalnego sprawdzania poprawności ASP.NET zastąpiło wstrzykiwany kod JavaScript z atrybutami \*danych.

    ![Niedyskretny kod weryfikacyjny](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Niedyskretny kod weryfikacyjny")

    *Niedyskretny kod weryfikacyjny*

    > [!NOTE]
    > W tym przykładzie przedstawiono sposób, w jaki podsumowanie walidacji z adnotacjami danych było uproszczone tylko do kilku wierszy HTML i JavaScript. Wcześniej, bez niezauważalnego sprawdzania poprawności, im więcej kontrolek weryfikacyjnych, tym większa, tym większy kod weryfikacyjny języka JavaScript zostanie powiększony.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Zadanie 2 — Walidacja modelu przy użyciu adnotacji danych

ASP.NET 4,5 wprowadza weryfikację adnotacji danych dla formularzy sieci Web. Zamiast mieć kontrolę weryfikacji dla poszczególnych danych wejściowych, można teraz definiować ograniczenia w klasach modelu i używać ich we wszystkich aplikacjach sieci Web. W tej sekcji dowiesz się, jak używać adnotacji danych do sprawdzania poprawności nowego/edytowania formularza klienta.

1. Otwórz stronę **CustomerDetail. aspx** . Zwróć uwagę, że imię i nazwisko klienta w sekcjach **EditItemTemplate** i **InsertItemTemplate** są weryfikowane przy użyciu kontrolek RequiredFieldValidator. Każdy moduł walidacji jest skojarzony z określonym warunkiem, dlatego należy dołączyć dowolną liczbę modułów sprawdzania poprawności jako warunki do sprawdzenia.
2. Dodaj adnotacje danych, aby sprawdzić poprawność klasy modelu klienta. Otwórz klasę **Customer.cs** w folderze **model** i *dekorować* każdą właściwość przy użyciu atrybutów adnotacji danych.

    (Fragment kodu- *Web Forms Lab-Ex02-adnotacje danych*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4,5 rozszerza istniejącą kolekcję adnotacji danych. Oto niektóre adnotacje danych, których można użyć: [CreditCard], [Phone], [EmailAddress], [zakres], [Compare], [URL], [FileExtensions], [wymagane], [klucz], [RegularExpression].
    > 
    > Przykłady użycia:
    > 
    > [Klucz]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Można także definiować własne komunikaty o błędach w ramach każdego atrybutu.
3. Otwórz **CustomerDetails. aspx** i Usuń wszystkie RequiredFieldValidators dla pól First i last name w sekcjach in EditItemTemplate i InsertItemTemplate formantu FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Jedną z zalet używania adnotacji z danymi jest to, że logika walidacji nie jest duplikowana na stronach aplikacji. Definiujesz ją raz w modelu i używają jej na wszystkich stronach aplikacji, które manipulują danymi.
4. Otwórz za pomocą kodu **CustomerDetails. aspx** i Znajdź metodę SaveCustomer. Ta metoda jest wywoływana podczas wstawiania nowego klienta i odbiera parametr klienta z wartości kontrolki FormView. Gdy występuje mapowanie między kontrolkami strony a obiektem parametru, ASP.NET będzie wykonywać walidację modelu względem wszystkich atrybutów adnotacji danych i wypełnić słownik ModelState błędami, które wystąpiły.

    ModelState. IsValid zwróci wartość true tylko wtedy, gdy wszystkie pola w modelu są prawidłowe po przeprowadzeniu walidacji.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Dodaj kontrolkę **podsumowania walidacji** na końcu strony CustomerDetails, aby wyświetlić listę błędów modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** to nowa właściwość kontrolki podsumowania walidacji, która po ustawieniu na **wartość true**kontrolka będzie wyświetlać Błędy ze słownika ModelState. Te błędy pochodzą z walidacji adnotacji danych.
6. Naciśnij klawisz **F5** , aby uruchomić aplikację sieci Web. Wypełnij formularz, podając błędne wartości, a następnie kliknij przycisk **Zapisz** , aby wykonać walidację. Zwróć uwagę na podsumowanie błędu u dołu.

    ![Walidacja przy użyciu adnotacji danych](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Walidacja przy użyciu adnotacji danych")

    *Walidacja przy użyciu adnotacji danych*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Zadanie 3 — Obsługa niestandardowych błędów bazy danych za pomocą ModelState

W poprzedniej wersji formularzy sieci Web obsługa błędów bazy danych, takich jak zbyt długi ciąg lub naruszenie klucza unikatowego, może polegać na wyrzucaniu wyjątków w kodzie repozytorium, a następnie obsłudze wyjątków w kodzie w celu wyświetlenia błędu. Aby coś było stosunkowo proste, wymagane jest doskonałe ilości kodu.

W formularzach sieci Web 4,5 obiekt ModelState może służyć do wyświetlania błędów na stronie z modelu lub bazy danych w spójny sposób.

W tym zadaniu dodasz kod do prawidłowego obsłużenia wyjątków bazy danych i zostanie wyświetlony odpowiedni komunikat dla użytkownika przy użyciu obiektu ModelState.

1. Gdy aplikacja jest nadal uruchomiona, spróbuj zaktualizować nazwę kategorii, używając zduplikowanej wartości.

    ![Aktualizowanie kategorii o zduplikowanej nazwie](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Aktualizowanie kategorii o zduplikowanej nazwie")

    *Aktualizowanie kategorii o zduplikowanej nazwie*

    Należy zauważyć, że wyjątek jest zgłaszany z powodu &quot;unikatowego ograniczenia&quot; kolumny **CategoryName** .

    ![Wyjątek zduplikowanych nazw kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Wyjątek zduplikowanych nazw kategorii")

    *Wyjątek zduplikowanych nazw kategorii*
2. Zatrzymaj debugowanie. W pliku związanym z kodem **Products.aspx.cs** zaktualizuj metodę **UpdateCategory** , aby obsługiwała wyjątki zgłoszone przez bazę danych. Metody SaveChanges () Metoda wywołuje i dodaje błąd do obiektu **ModelState** .

    Nowa metoda **TryUpdateModel** aktualizuje obiekt kategorii pobrany z bazy danych przy użyciu danych formularza dostarczonych przez użytkownika.

    (Fragment kodu- *Web Forms Lab-Ex02-UpdateCategory obsługi błędów*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > W idealnym przypadku trzeba zidentyfikować przyczynę dbupdateexception i sprawdzić, czy przyczyną problemu głównego jest naruszenie ograniczenia UNIQUE Key.
3. Otwórz **produkt Products. aspx** i Dodaj formant **podsumowania walidacji** poniżej kategorii GridView, aby wyświetlić listę błędów modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Uruchom witrynę i przejdź do strony produkty. Spróbuj zaktualizować nazwę kategorii, używając zduplikowanej wartości.

    Zwróć uwagę, że wyjątek został obsłużony, a komunikat o błędzie pojawia się w kontrolce **podsumowania walidacji** .

    ![Błąd zduplikowanej kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Błąd zduplikowanej kategorii")

    *Błąd zduplikowanej kategorii*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Zadanie 4 — Walidacja żądania w programie ASP.NET Web Forms 4,5

Funkcja walidacji żądania w programie ASP.NET zapewnia określony poziom ochrony domyślnej przed atakami na skrypty między lokacjami (XSS). W poprzednich wersjach programu ASP.NET sprawdzanie poprawności żądania zostało włączone domyślnie i można ją wyłączyć tylko dla całej strony. Za pomocą nowej wersji formularzy sieci Web ASP.NET można teraz wyłączyć weryfikację żądania dla jednej kontrolki, przeprowadzić weryfikację żądań z opóźnieniem lub uzyskać dostęp do niezweryfikowanych danych żądania (należy zachować ostrożność, jeśli to zrobisz).

1. Naciśnij **kombinację klawiszy CTRL + F5** , aby uruchomić lokację bez debugowania i przejdź do strony produkty. Wybierz kategorię, a następnie kliknij link **Edytuj** na dowolnym z produktów.
2. Wpisz opis zawierający potencjalnie niebezpieczną zawartość, na przykład, w tym Tagi HTML. Zwróć uwagę na wyjątek zgłoszony z powodu weryfikacji żądania.

    ![Edytowanie produktu z potencjalnie niebezpieczną zawartością](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Edytowanie produktu z potencjalnie niebezpieczną zawartością")

    *Edytowanie produktu z potencjalnie niebezpieczną zawartością*

    ![Zgłoszono wyjątek z powodu weryfikacji żądania](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Zgłoszono wyjątek z powodu weryfikacji żądania")

    *Zgłoszono wyjątek z powodu weryfikacji żądania*
3. Zamknij stronę i w programie Visual Studio naciśnij klawisze **Shift + F5** , aby zatrzymać debugowanie.
4. Otwórz stronę **ProductDetails. aspx** i Znajdź pole tekstowe **Opis** .
5. Dodaj nową właściwość **ValidateRequestMode** do pola tekstowego i ustaw jej wartość na **Disabled**.

    Nowy atrybut **ValidateRequestMode** pozwala na szczegółowe wyłączenie sprawdzania poprawności żądania w każdej kontrolce. Jest to przydatne, gdy chcesz użyć danych wejściowych, które mogą otrzymać kod HTML, ale chcesz zachować weryfikację w pozostałej części strony.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Naciśnij klawisz **F5** , aby uruchomić aplikację sieci Web. Otwórz ponownie stronę Edytuj produkt i uzupełnij Opis produktu, w tym Tagi HTML. Zauważ, że teraz możesz dodać zawartość HTML do opisu.

    ![Weryfikacja żądania została wyłączona dla opisu produktu](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Weryfikacja żądania została wyłączona dla opisu produktu")

    *Weryfikacja żądania została wyłączona dla opisu produktu*

    > [!NOTE]
    > W aplikacji produkcyjnej należy oczyszczać kod HTML wprowadzony przez użytkownika, aby upewnić się, że są wprowadzane tylko bezpieczne Tagi HTML (na przykład nie ma &lt;tagów&gt; skryptów). W tym celu można użyć [biblioteki ochrony sieci Web firmy Microsoft](https://www.nuget.org/packages/AntiXSS).
7. Edytuj produkt ponownie. Wpisz kod HTML w polu Nazwa, a następnie kliknij przycisk **Zapisz**. Należy zauważyć, że sprawdzanie poprawności żądania jest wyłączone tylko dla pola Opis, a pozostałe pola nadal są weryfikowane pod kątem potencjalnie niebezpiecznej zawartości.

    ![Sprawdzanie poprawności żądania zostało włączone w pozostałej części pól](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Sprawdzanie poprawności żądania zostało włączone w pozostałej części pól")

    *Sprawdzanie poprawności żądania zostało włączone w pozostałej części pól*

    ASP.NET Web Forms 4,5 zawiera nowy tryb walidacji żądania w celu przeprowadzenia walidacji żądania opóźnieniem. W trybie walidacji żądania ustawionej na **4,5**, jeśli element kodu uzyskuje dostęp do *żądania. formularz [&quot;Key&quot;]* , walidacja żądania ASP.NET 4.5 będzie wyzwalać tylko weryfikację żądania dla tego konkretnego elementu w kolekcji formularzy.

    Ponadto ASP.NET 4,5 zawiera teraz podstawowe procedury kodowania z biblioteki Microsoft Anti-XSS Library v 4.0. Procedury kodowania antyxss są implementowane przez nowy typ *AntiXssEncoder* znaleziony w nowej przestrzeni nazw **System. Web. Security. AntiXSS** . Przy użyciu parametru **kodertype** skonfigurowanego do korzystania z *AntiXssEncoder*, wszystkie kodowanie danych wyjściowych w programie ASP.NET automatycznie używa nowych procedur kodowania.
8. Weryfikacja żądania ASP.NET 4,5 również obsługuje niezweryfikowany dostęp do danych żądania. ASP.NET 4,5 dodaje nową właściwość kolekcji do obiektu **HttpRequest** o nazwie **unvalidated**. Gdy przejdziesz do **żądania HttpRequest** , masz dostęp do wszystkich wspólnych fragmentów danych żądania, w tym formularzy, QueryString, plików cookie, adresów URL itd.

    ![Żądanie. niezweryfikowany obiekt](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Żądanie. niezweryfikowany obiekt")

    *Żądanie. niezweryfikowany obiekt*

    > [!NOTE]
    > **Użyj właściwości HttpRequest. unvalidated z przestrogą!** Upewnij się, że masz ostrożną weryfikację niestandardową danych żądania, aby upewnić się, że niebezpieczne teksty nie są wyskakujące i renderowane z powrotem do niepodejrzanych klientów.

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Ćwiczenie 3: asynchroniczne przetwarzanie stron w formularzach sieci Web ASP.NET

W tym ćwiczeniu nastąpi wprowadzenie do nowych funkcji asynchronicznych przetwarzania stron w formularzach sieci Web ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Zadanie 1 — Aktualizowanie strony szczegółów produktu w celu przekazywania i wyświetlania obrazów

W tym zadaniu zostanie zaktualizowana Strona szczegóły produktu, aby umożliwić użytkownikowi określenie adresu URL obrazu dla produktu i wyświetlenie go w widoku tylko do odczytu. Zostanie utworzona kopia lokalna określonego obrazu przez pobranie go synchronicznie. W następnym zadaniu zostanie zaktualizowana Ta implementacja, aby działała asynchronicznie.

1. Otwórz **program Visual Studio 2012** i Załaduj rozwiązanie **BEGIN** znajdujące się w **Source\Ex3-Async\Begin** z folderu tego laboratorium. Alternatywnie możesz kontynuować pracę na istniejącym rozwiązaniu z poprzednich ćwiczeń.

   1. Jeśli otwarto **udostępnione rozwiązanie** , należy pobrać brakujące pakiety NuGet przed kontynuowaniem. W tym celu w Eksplorator rozwiązań kliknij projekt **WebFormsLab** i wybierz pozycję **Zarządzaj pakietami NuGet**.
   2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przywróć** , aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając pozycję **kompiluj** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet korzystania z NuGet jest to, że nie trzeba dostarczać wszystkich bibliotek w projekcie, zmniejszając rozmiar projektu. Przy użyciu narzędzi NuGet PowerShell, określając wersje pakietu w pliku Packages. config, będzie można pobrać wszystkie wymagane biblioteki przy pierwszym uruchomieniu projektu. To dlatego, że trzeba będzie wykonać te kroki po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz źródło strony **ProductDetails. aspx** i Dodaj pole w ItemTemplate FormView, aby wyświetlić obraz produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Dodaj pole, aby określić adres URL obrazu w EditTemplate FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Otwórz plik związany z kodem **ProductDetails.aspx.cs** i Dodaj następujące dyrektywy przestrzeni nazw.

    (Fragment kodu — *Web Forms Lab-Ex03-Namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Utwórz metodę **UpdateProductImage** , aby przechowywać obrazy zdalne w folderze Local **images** i zaktualizować jednostkę produktu przy użyciu wartości Nowa lokalizacja obrazu.

    (Fragment kodu — *Web Forms Lab-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Zaktualizuj metodę **UpdateProduct** , aby wywołać metodę **UpdateProductImage** .

    (Fragment kodu- *Web Forms Lab-Ex03-UpdateProductImage Call*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Uruchom aplikację i spróbuj przekazać obraz dla produktu. Na przykład można użyć następującego adresu URL obrazu z programu Office clipart: [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Ustawianie obrazu dla produktu](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Ustawianie obrazu dla produktu")

    *Ustawianie obrazu dla produktu*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Zadanie 2 — Dodawanie przetwarzania asynchronicznego do strony szczegółów produktu

W tym zadaniu zostanie zaktualizowana Strona szczegóły produktu, aby działała asynchronicznie. Zostanie zwiększona długotrwałe zadanie — proces pobierania obrazu — za pomocą asynchronicznego przetwarzania stron ASP.NET 4,5.

Metody asynchroniczne w aplikacjach sieci Web mogą służyć do optymalizowania sposobu używania pul wątków ASP.NET. W ASP.NET istnieje ograniczona liczba wątków w puli wątków na potrzeby uczestniczenia w żądaniach, więc gdy wszystkie wątki są zajęte, ASP.NET zaczyna odrzucać nowe żądania, wysyła komunikaty o błędach aplikacji i sprawia, że witryna jest niedostępna.

Czasochłonne operacje w witrynie sieci Web są doskonałymi kandydatami do programowania asynchronicznego, ponieważ zajmują przydzielony wątek przez długi czas. Obejmuje to długotrwałe żądania, strony z wieloma różnymi elementami i stronami, które wymagają operacji offline, takich jak wykonywanie zapytań do bazy danych lub uzyskiwanie dostępu do zewnętrznego serwera sieci Web. Ta korzyść polega na tym, że jeśli używasz metod asynchronicznych dla tych operacji, podczas przetwarzania strony wątek jest zwalniany i zwracany do puli wątków i może służyć do wzięcia udziału w nowym żądaniu strony. Oznacza to, że strona rozpocznie przetwarzanie w jednym wątku z puli wątków i może zakończyć przetwarzanie w innym, po zakończeniu przetwarzania asynchronicznego.

1. Otwórz stronę **ProductDetails. aspx** . Dodaj atrybut **Async** w elemencie **Page** i ustaw dla niego **wartość true**. Ten atrybut nakazuje ASP.NET zaimplementowanie interfejsu IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Dodaj etykietę w dolnej części strony, aby wyświetlić szczegóły wątków, na których działa strona.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Otwórz **ProductDetails.aspx.cs** i Dodaj następujące dyrektywy przestrzeni nazw.

    (Fragment kodu — *Web Forms Lab-Ex03-przestrzenie nazw 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Zmodyfikuj metodę **UpdateProductImage** , aby pobrać obraz z zadania asynchronicznego. Metoda **DownloadFile** **WebClient** jest zastępowana metodą **DownloadFileTaskAsync** i zawiera słowo kluczowe **await** .

    (Fragment kodu — *Web Forms Lab-Ex03-UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask rejestruje nowe zadanie asynchroniczne strony, które ma zostać wykonane w innym wątku. Otrzymuje wyrażenie lambda z zadaniem (t), które ma zostać wykonane. Słowo kluczowe **await** w metodzie **DownloadFileTaskAsync** konwertuje resztę metody do wywołania zwrotnego, które jest wywoływane asynchronicznie po zakończeniu metody **DownloadFileTaskAsync** . ASP.NET wznowi wykonywanie metody przez automatyczne zachowanie wszystkich oryginalnych wartości żądania HTTP. Nowy asynchroniczny model programowania w programie .NET 4,5 pozwala pisać kod asynchroniczny, który wygląda bardzo podobnie do kodu synchronicznego i pozwala kompilatorowi obsłużyć komplikacje funkcji wywołania zwrotnego lub kodu kontynuacji.
    > [!NOTE]
    > RegisterAsyncTask i PageAsyncTask były już dostępne od czasu .NET 2,0. Słowo kluczowe await jest nowe z modelu programowania asynchronicznego .NET 4,5 i może być używane razem z nowymi metodami TaskAsync z obiektu WebClient programu .NET.
5. Dodaj kod, aby wyświetlić wątki, na których uruchomiono i zakończono wykonywanie kodu. W tym celu należy zaktualizować metodę **UpdateProductImage** przy użyciu następującego kodu.

    (Fragment kodu — *Web Forms Lab-Ex03-show Threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Otwórz plik **Web. config** witryny sieci Web. Dodaj następującą zmienną element appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Naciśnij klawisz **F5** , aby uruchomić aplikację i przekazać obraz dla produktu. Zwróć uwagę na identyfikator wątków, w których kod został uruchomiony i zakończony może być inny. Wynika to z faktu, że zadania asynchroniczne są uruchamiane w osobnym wątku z puli wątków ASP.NET. Po zakończeniu zadania ASP.NET umieszcza zadanie z powrotem w kolejce i przypisuje dowolny z dostępnych wątków.

    ![Asynchroniczne pobieranie obrazu](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Asynchroniczne pobieranie obrazu")

    *Asynchroniczne pobieranie obrazu*

> [!NOTE]
> Ponadto możesz wdrożyć tę aplikację na platformie Azure w [dodatku B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy](#AppendixB).

---

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym ćwiczeniu ćwiczenia zostały opisane następujące koncepcje:

- Używanie wyrażeń z powiązaniem danych o jednoznacznie określonym typie
- Korzystanie z nowych funkcji powiązania modelu w formularzach sieci Web
- Korzystanie z dostawców wartości na potrzeby mapowania danych strony do metod związanych z kodem
- Używanie adnotacji danych do sprawdzania poprawności danych wejściowych użytkownika
- Korzystaj z niezauważalnej weryfikacji po stronie klienta za pomocą technologii jQuery w formularzach sieci Web
- Zaimplementuj szczegółowe sprawdzanie poprawności żądania
- Implementuj asynchroniczne przetwarzanie stron w formularzach sieci Web

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie Visual Studio Express 2012 dla sieci Web

Można zainstalować **Microsoft Visual Studio Express 2012 dla sieci Web** lub innej &quot;Express&quot; wersji za pomocą **[Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przeprowadzą Cię przez kroki wymagane do zainstalowania *programu Visual Studio Express 2012 for Web* przy użyciu *Instalator platformy Microsoft Web*.

1. Przejdź do [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli wcześniej zainstalowano Instalatora platformy sieci Web, można go otworzyć i wyszukać produkt &quot;<em>Visual Studio Express 2012 for Web przy użyciu zestawu Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** , nastąpi przekierowanie do pobrania i zainstalowania go w pierwszej kolejności.
3. Po otwarciu **Instalatora platformy sieci Web** kliknij przycisk **Zainstaluj** , aby rozpocząć instalację.

    ![Zainstaluj Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Zainstaluj Visual Studio Express")

    *Zainstaluj Visual Studio Express*
4. Przeczytaj wszystkie licencje i postanowienia produktów, a następnie kliknij przycisk **Akceptuję** , aby kontynuować.

    ![Akceptowanie postanowień licencyjnych](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Akceptowanie postanowień licencyjnych*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja zakończona](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalacja zakończona*
7. Kliknij przycisk **Zakończ** , aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć Visual Studio Express dla sieci Web, przejdź do ekranu **startowego** i zacznij pisać &quot;**vs Express**&quot;, a następnie kliknij kafelek **vs Express for Web** .

    ![Kafelek VS Express for Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Kafelek VS Express for Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

W tym dodatku pokazano, jak utworzyć nową witrynę sieci Web w witrynie Azure Portal i opublikować aplikację uzyskaną w wyniku działania laboratorium, korzystając z funkcji publikowania Web Deploy oferowanej przez platformę Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web w witrynie Azure Portal

1. Przejdź do [usługi Azure Portal zarządzania](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Dzięki platformie Azure możesz bezpłatnie hostować 10 ASP.NET witryn sieci Web, a następnie skalować je w miarę wzrostu ruchu. Możesz utworzyć konto w [tym miejscu](https://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do systemu Windows Azure Portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Zaloguj się do systemu Windows Azure Portal")

    *Zaloguj się do portalu*
2. Na pasku poleceń kliknij pozycję **Nowy** .

    ![Tworzenie nowej witryny sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Tworzenie nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij pozycję **obliczeniowe** | **witrynie sieci Web**. Następnie wybierz opcję **szybkie tworzenie** . Podaj dostępny adres URL dla nowej witryny sieci Web i kliknij pozycję **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Platforma Azure jest hostem aplikacji sieci Web działającej w chmurze, którą można kontrolować i zarządzać nią. Opcja szybkie tworzenie umożliwia wdrożenie ukończonej aplikacji sieci Web na platformie Azure spoza portalu. Nie obejmuje to kroków związanych z konfigurowaniem bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia")

    *Tworzenie nowej witryny sieci Web przy użyciu funkcji szybkiego tworzenia*
4. Poczekaj na utworzenie nowej **witryny sieci Web** .
5. Po utworzeniu witryny sieci Web kliknij link pod kolumną **adres URL** . Sprawdź, czy nowa witryna sieci Web działa.

    ![Przeglądanie w nowej witrynie sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Przeglądanie w nowej witrynie sieci Web")

    *Przeglądanie w nowej witrynie sieci Web*

    ![Uruchomiona witryna sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Uruchomiona witryna sieci Web")

    *Uruchomiona witryna sieci Web*
6. Wróć do portalu i kliknij nazwę witryny sieci Web pod kolumną **Nazwa** , aby wyświetlić strony zarządzania.

    ![Otwieranie stron zarządzania witryną sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Otwieranie stron zarządzania witryną sieci Web")

    *Otwieranie stron zarządzania witryną sieci Web*
7. Na stronie **pulpit nawigacyjny** w sekcji **szybki przegląd** kliknij link **Pobierz profil publikowania** .

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do opublikowania aplikacji sieci Web na platformie Azure dla każdej z włączonych metod publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i ciągi bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego punktu końcowego, dla którego jest włączona metoda publikacji. **Firma Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** obsługują odczytywanie profilów publikowania w celu zautomatyzowania konfiguracji tych programów na potrzeby publikowania aplikacji sieci Web na platformie Azure.

    ![Pobieranie profilu publikowania witryny sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Pobieranie profilu publikowania witryny sieci Web")

    *Pobieranie profilu publikowania witryny sieci Web*
8. Pobierz plik profilu publikowania do znanej lokalizacji. W tym ćwiczeniu zobaczysz, jak używać tego pliku do publikowania aplikacji sieci Web na platformie Azure z programu Visual Studio.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z baz danych SQL Server, należy utworzyć SQL Database serwer. Jeśli chcesz wdrożyć prostą aplikację, która nie używa SQL Server można pominąć to zadanie.

1. Do przechowywania bazy danych aplikacji potrzebny jest serwer SQL Database. Możesz wyświetlić SQL Database serwery z subskrypcji w portalu zarządzania Azure w obszarze **bazy danych SQL** | **serwery** | **pulpicie nawigacyjnym serwera**. Jeśli nie masz utworzonego serwera, możesz go utworzyć przy użyciu przycisku **Dodaj** na pasku poleceń. Zanotuj **nazwę serwera i adres URL, nazwę logowania administratora i hasło**, ponieważ zostaną one użyte w następnych zadaniach. Nie Twórz jeszcze bazy danych, ponieważ zostanie ona utworzona na późniejszym etapie.

    ![Pulpit nawigacyjny serwera SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Pulpit nawigacyjny serwera SQL Database")

    *Pulpit nawigacyjny serwera SQL Database*
2. W następnym zadaniu zostanie przetestowane połączenie z bazą danych z programu Visual Studio. z tego powodu należy uwzględnić lokalny adres IP na liście **dozwolonych adresów IP**serwera. W tym celu kliknij pozycję **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go w polach tekstowych **początkowy adres IP** i **końcowy adres IP** , a następnie kliknij przycisk ![Dodaj-Client-IP-Address-OK-Button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png).

    ![Dodawanie adresu IP klienta](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Dodawanie adresu IP klienta*
3. Po dodaniu **adresu IP klienta** do listy dozwolone adresy IP kliknij pozycję **Zapisz** , aby potwierdzić zmiany.

    ![Potwierdź zmiany](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — publikowanie aplikacji ASP.NET MVC 4 przy użyciu Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt witryny sieci Web i wybierz polecenie **Publikuj**.

    ![Publikowanie aplikacji](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Publikowanie aplikacji")

    *Publikowanie witryny sieci Web*
2. Zaimportuj profil publikowania zapisany w pierwszym zadaniu.

    ![Importowanie profilu publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij pozycję **Weryfikuj połączenie**. Po zakończeniu walidacji kliknij przycisk **dalej**.

    > [!NOTE]
    > Walidacja jest ukończona po wyświetleniu zielonego znacznika wyboru obok przycisku Weryfikuj połączenie.

    ![Weryfikowanie połączenia](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Weryfikowanie połączenia")

    *Weryfikowanie połączenia*
4. Na stronie **Ustawienia** w sekcji **bazy danych** kliknij przycisk obok pola tekstowego połączenie z bazą danych (np. **DefaultConnection**).

    ![Konfiguracja narzędzia Web Deploy](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Konfiguracja narzędzia Web Deploy")

    *Konfiguracja narzędzia Web Deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W polu **Nazwa serwera** wpisz adres URL serwera SQL Database przy użyciu prefiksu *TCP:* .
   - W polu **Nazwa użytkownika** wpisz nazwę logowania administratora serwera.
   - W polu **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nową nazwę bazy danych.

     ![Konfigurowanie parametrów połączenia docelowego](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Konfigurowanie parametrów połączenia docelowego")

     *Konfigurowanie parametrów połączenia docelowego*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu o utworzenie bazy danych kliknij przycisk **tak**.

    ![Tworzenie bazy danych](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Tworzenie ciągu bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia się z SQL Database na platformie Azure, są wyświetlane w domyślnym polu tekstowym połączenie. Następnie kliknij przycisk **Next** (Dalej).

    ![Parametry połączenia wskazujące SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Parametry połączenia wskazujące SQL Database")

    *Parametry połączenia wskazujące SQL Database*
8. Na stronie **Podgląd** kliknij przycisk **Publikuj**.

    ![Publikowanie aplikacji sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Publikowanie aplikacji sieci Web")

    *Publikowanie aplikacji sieci Web*
9. Po zakończeniu procesu publikowania w domyślnej przeglądarce zostanie otwarta opublikowana witryna sieci Web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: używanie fragmentów kodu

Za pomocą fragmentów kodu masz cały kod, którego potrzebujesz na wyręką. Dokument laboratoryjny powiedzie się dokładnie wtedy, gdy można z nich korzystać, jak pokazano na poniższej ilustracji.

![Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio")

*Wstawianie kodu do projektu za pomocą fragmentów kodu programu Visual Studio*

***Aby dodać fragment kodu przy użyciu klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragmentu kodu (bez spacji lub łączników).
3. Obejrzyj jako IntelliSense wyświetla pasujące nazwy fragmentów kodu.
4. Wybierz poprawny fragment kodu (lub Kontynuuj wpisywanie, dopóki nie zostanie zaznaczona cała nazwa fragmentu kodu).
5. Naciśnij dwukrotnie klawisz Tab, aby wstawić fragment kodu w lokalizacji kursora.

![Zacznij wpisywać nazwę fragmentu kodu](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Zacznij wpisywać nazwę fragmentu kodu")

*Zacznij wpisywać nazwę fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu")

*Naciśnij klawisz Tab, aby zaznaczyć wyróżniony fragment kodu*

![Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta")

*Naciskaj klawisz Tab ponownie i wstawka zostanie rozwinięta*

***Aby dodać fragment kodu przy użyciu myszy (C#, Visual Basic i XML)*** jedno. Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment** kodu, po którym następuje **Moje fragmenty kodów**.
2. Wybierz odpowiedni fragment kodu z listy, klikając go.

![Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment")

*Kliknij prawym przyciskiem myszy miejsce, w którym chcesz wstawić fragment kodu, a następnie wybierz Wstaw fragment*

![Wybierz odpowiedni fragment kodu z listy, klikając go](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Wybierz odpowiedni fragment kodu z listy, klikając go")

*Wybierz odpowiedni fragment kodu z listy, klikając go*

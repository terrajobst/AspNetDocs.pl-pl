---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Wprowadzenie | Microsoft Docs
author: Rick-Anderson
description: Element WebMatrix nie jest już zalecany jako zintegrowane środowisko programistyczne dla stron sieci Web ASP.NET. Użyj programu Visual Studio lub Visual Studio Code. Niniejszy przewodnik a...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: bb863f8605e6f8faca3b285607b63a3e88e83012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547104"
---
# <a name="getting-started"></a>Wprowadzenie

Autor [FitzMacken](https://github.com/tfitzmac)

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > Element WebMatrix nie jest już zalecany jako zintegrowane środowisko programistyczne dla stron sieci Web ASP.NET. Użyj [programu Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) lub [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Te wskazówki i aplikacje udostępniają przegląd ASP.NET stron sieci Web (wersja 2 lub nowsza) oraz składnia Razor — uproszczonej platformy do tworzenia dynamicznych witryn sieci Web. Wprowadzono tu również program WebMatrix, narzędzie do tworzenia stron i witryn.
> 
> **Poziom**: nowy do ASP.NET stron sieci Web.  
> **Założono umiejętności**: HTML, podstawowe kaskadowe arkusze stylów (CSS).
> 
> Co znajdziesz w pierwszym samouczku zestawu:
> 
> - Co to jest technologia ASP.NET Web Pages i czego służy.
> - Co to jest WebMatrix.
> - Jak zainstalować wszystko.
> - Jak utworzyć witrynę sieci Web za pomocą programu WebMatrix.
>   
> 
> Omówione funkcje/technologie:
> 
> - Instalator platformy Microsoft Web.
> - WebMatrix.
> - *. cshtml* — strony
>   
> 
> Jan Pope oryginalnie zapisał ten samouczek. Tomasz FitzMacken zaktualizował go dla programu Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - WebMatrix 3

## <a name="what-should-you-know"></a>Co należy wiedzieć?

Zakładamy, że znasz już:

- **Kod HTML**. Szczegółowa wiedza nie jest wymagana. Nie wyjaśnimy kodu HTML, ale nie będziemy również używać żadnej złożonej. Udostępnimy linki do samouczków HTML, w których sądzimy, że są one przydatne.
- **Kaskadowe arkusze stylów (CSS)** . Analogicznie jak w przypadku języka HTML.
- **Podstawowe pomysły dotyczące bazy danych**. Jeśli korzystasz z arkusza kalkulacyjnego do danych i posortowano i przefiltrowany dane, jest to poziom doświadczenia, którego ogólnie przyjęto w tym zestawie samouczków.

Zakładamy również, że interesują Cię podstawowe programowanie. ASP.NET strony sieci Web używają języka programowania o C#nazwie. Nie trzeba mieć żadnych teł w programowaniu. Jeśli kiedykolwiek wcześniej Zapisano kod JavaScript na stronie sieci Web, masz dużą liczbę teł.

Pamiętaj, że jeśli wiesz już, jak programować, możesz się dowiedzieć, że ten samouczek został ustawiony początkowo wolniej, gdy nastąpi szybkie przeprowadzenie nowych programistów. Mimo że będziemy korzystać z kilku pierwszych samouczków, ale będzie to mniej podstawowe programowanie objaśniające, a elementy będą przenoszone w szybszym klipie.

## <a name="what-do-you-need"></a>Czego potrzebujesz?

Oto, co jest potrzebne:

- Komputer z systemem Windows 8, Windows 7, Windows Server 2008 lub Windows Server 2012.
- Aktywne połączenie internetowe.
- Uprawnienia administratora (wymagane w procesie instalacji).

## <a name="what-is-aspnet-web-pages"></a>Co to są ASP.NET Web Pages?

ASP.NET Web Pages to struktura, za pomocą której można tworzyć dynamiczne strony sieci Web. Prosta strona sieci Web HTML jest statyczna. jego zawartość jest określana na podstawie stałego znacznika HTML znajdującego się na stronie. Strony dynamiczne, takie jak te utworzone za pomocą stron sieci Web ASP.NET, umożliwiają tworzenie zawartości strony na bieżąco przy użyciu kodu.

Strony dynamiczne umożliwiają wykonywanie wszystkich zadań. Użytkownik może poproszeni o dane wejściowe przy użyciu formularza, a następnie zmienić zawartość wyświetlaną na stronie lub wygląd strony. Możesz uzyskać informacje od użytkownika, zapisać je w bazie danych, a następnie wyświetlić je później. Możesz wysyłać wiadomości e-mail z witryny programu. Można korzystać z innych usług w sieci Web (na przykład usługi mapowania) i generować strony, które integrują informacje z tych źródeł.

## <a name="what-is-webmatrix"></a>Co to jest WebMatrix?

WebMatrix to narzędzie, które integruje edytor stron sieci Web, narzędzie bazy danych, serwer sieci Web do testowania stron oraz funkcje publikowania witryny sieci Web w Internecie. Program WebMatrix jest bezpłatny i można go łatwo instalować i łatwo używać. (Działa również w przypadku tylko zwykłych stron HTML, a także dla innych technologii, takich jak PHP).

Nie *trzeba* w rzeczywistości używać programu WebMatrix do pracy ze stronami sieci Web ASP.NET. Możesz tworzyć strony przy użyciu edytora tekstu, na przykład, i strony testowe przy użyciu serwera sieci Web, do którego masz dostęp. Jednak program WebMatrix sprawia, że jest to bardzo proste, dzięki czemu samouczki będą korzystać z programu WebMatrix w systemie.

## <a name="about-these-tutorials"></a>Informacje o tych samouczkach

Ten zestaw samouczków zawiera wprowadzenie do korzystania z ASP.NET stron sieci Web. W tym zestawie samouczków wprowadzającej znajduje się łącznie 9 samouczków. Jest to część zestawu samouczków, która umożliwia ASP.NET stron sieci Web od początku do tworzenia rzeczywistych, profesjonalnych witryn sieci Web.

W pierwszym samouczku ustawiono skoncentrowanie się na przedstawianiu podstaw pracy ze stronami sieci Web ASP.NET. Gdy skończysz, możesz pracować z dodatkowymi zestawami samouczków, które są wybierane w tym samym czasie i które eksplorują strony sieci Web bardziej szczegółowo.

Zachęcamy do prostych wyjaśnień. I zawsze, gdy pokażemy coś, w tym samouczku zawsze wybieramy sposób, w jaki myślimy. Kolejne zestawy samouczków przechodzą do większej głębokości i pokazują bardziej wydajne lub bardziej elastyczne podejścia (również bardziej zabawne). Jednak te samouczki wymagają najpierw zrozumienia podstawowych informacji.

Właśnie uruchomiony zestaw samouczka obejmuje następujące funkcje stron sieci Web ASP.NET:

- Wprowadzenie i pobranie wszystkiego. (To jest w samouczku, który jest odczytywany).
- Podstawowe informacje na temat programowania stron sieci Web ASP.NET.
- Tworzenie bazy danych.
- Tworzenie i przetwarzanie formularza danych wejściowych użytkownika.
- Dodawanie, aktualizowanie i usuwanie danych w bazie danych.

## <a name="what-will-you-create"></a>Co utworzysz?

Ten samouczek jest ustawiany i kolejne są nanoszone wokół witryny sieci Web, w której można wyświetlić listę filmów. Będziesz mieć możliwość wprowadzania filmów, edytowania ich i wyświetlania listy. Poniżej przedstawiono kilka stron, które zostaną utworzone w tym zestawie samouczków. Pierwszy z nich zawiera stronę wyświetlania filmu, którą utworzysz:

![Aplikacja zakończył Movie pokazująca listę filmów](getting-started/_static/image1.png)

A oto strona, która umożliwia dodawanie nowych informacji o filmie do witryny:

![Gotowa aplikacja filmowa przedstawiająca stronę Dodawanie filmu](getting-started/_static/image2.png)

Kolejny samouczek ustawia kompilację w tym zestawie i dodaje więcej funkcji, takich jak przekazywanie obrazów, zezwalanie na logowanie, wysyłanie wiadomości e-mail i integrowanie z multimediami społecznościowymi.

## <a name="see-this-app-running-on-azure"></a>Wyświetlanie aplikacji działającej na platformie Azure

Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo? Pełną wersję aplikacji możesz wdrożyć na koncie platformy Azure — wystarczy kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure. Jeśli nie masz jeszcze konta, możesz:

- [Bezpłatnie Otwórz konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — Uzyskaj kredyty, których możesz użyć do wypróbowania płatnych usług platformy Azure, a nawet po ich użyciu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywuj korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) — subskrypcja MSDN daje środki na korzystanie z płatnych usług platformy Azure.

## <a name="installing-everything"></a>Instalowanie wszystkiego

Wszystko można zainstalować za pomocą Instalatora platformy sieci Web firmy Microsoft. W efekcie należy zainstalować Instalatora, a następnie użyć go do zainstalowania wszystkiego innego.

Aby korzystać ze stron sieci Web, musi być zainstalowany co najmniej system Windows XP z dodatkiem SP3 lub Windows Server 2008 lub nowszy.

Na [stronie strony sieci Web](../../../index.md) w witrynie ASP.NET kliknij pozycję **Zainstaluj**.

![ASP.NET witryna sieci Web przedstawiająca przycisk &quot;Zainstaluj&quot; WebMatrix](getting-started/_static/image3.png)

Przed zainstalowaniem programu WebMatrix zostanie wyświetlony monit o zaakceptowanie postanowień licencyjnych i zasad zachowania poufności informacji.

![Zaakceptuj termin, aby rozpocząć instalację](getting-started/_static/image4.png)

Kliknij przycisk **Uruchom** , aby rozpocząć instalację. (Jeśli chcesz zapisać Instalatora, kliknij przycisk **Zapisz** , a następnie uruchom Instalatora z folderu, w którym został zapisany).

![](getting-started/_static/image5.png)

Zostanie wyświetlony Instalator platformy sieci Web gotowy do zainstalowania programu WebMatrix. Kliknij polecenie **Zainstaluj**.

![](getting-started/_static/image6.png)

Proces instalacji zawiera informacje o tym, co należy zainstalować na komputerze, i uruchomić proces. W zależności od tego, co dokładnie trzeba zainstalować, proces może potrwać od kilku chwil do kilku minut. Wybierz **pozycję Akceptuję** , aby zaakceptować postanowienia licencyjne.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Po zakończeniu proces instalacji może automatycznie uruchomić program WebMatrix. Jeśli nie, w systemie Windows z menu **Start** , uruchom **program Microsoft WebMatrix**.

Po pierwszym uruchomieniu programu WebMatrix masz możliwość zalogowania się do Microsoft Azure przy użyciu konto Microsoft. Logując się, otrzymasz 10 bezpłatnych aplikacji sieci Web za pomocą platformy Azure. Te bezpłatne aplikacje sieci Web zapewniają wygodny sposób testowania aplikacji. Jeśli nie masz jeszcze konta platformy Azure, ale masz subskrypcję MSDN, możesz [aktywować korzyści z subskrypcji MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). W przeciwnym razie możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Nie musisz już się zalogować, aby kontynuować pracę z tym samouczkiem. Jeśli nie zalogujesz się teraz, nadal będziesz mieć możliwość zalogowania się później. Ostatni [temat](publishing.md) w tej serii samouczków zawiera opis sposobu wdrażania witryny sieci Web na platformie Azure. w związku z tym należy zalogować się, aby ukończyć ten temat.

W tym momencie Zaloguj się przy użyciu konto Microsoft lub wybierz pozycję **nie teraz** w prawym dolnym rogu.

![Zaloguj się](getting-started/_static/image7.png)

Aby rozpocząć, Utwórz pustą witrynę sieci Web i Dodaj stronę. W późniejszym samouczku w tym zestawie będziesz odtwarzać przy użyciu jednego z wbudowanych szablonów witryny sieci Web.

W oknie Start kliknij pozycję **Nowy**.

![Ekran startowy WebMatrix](getting-started/_static/image8.png)

Szablony to wstępnie skompilowane pliki i strony dla różnych typów witryn sieci Web. Aby wyświetlić wszystkie szablony, które są dostępne domyślnie, wybierz opcję Galeria szablonów.

![Wybieranie galerii szablonów](getting-started/_static/image9.png)

W oknie **Szybki Start** wybierz **pustą lokację** z grupy **ASP.NET** i nadaj nowej witrynie "WebPagesMovies".

![Okno Szybki start WebMatrix z wybranym pustym szablonem witryny](getting-started/_static/image10.png)

Kliknij przycisk **Dalej**.

Po zalogowaniu się do konto Microsoft będziesz mieć możliwość utworzenia witryny na platformie Azure. Na podstawie nazwy witryny Sugerowana jest domyślna nazwa **WebPagesMovies.azurewebsites.NET** ; jednak wykrzyknik wskazuje, że ta nazwa nie jest dostępna w systemie Windows Azure. Dla uproszczenia wybierz pozycję **Pomiń** , aby pominąć tworzenie witryny sieci Web na platformie Azure. W dalszej części tej serii będziemy publikować witrynę na platformie Azure.

![Utwórz witrynę platformy Azure](getting-started/_static/image11.png)

W programie WebMatrix zostanie utworzona i otwarta lokacja:

![Nowa witryna WebPagesMovies otwarta w programie WebMatrix](getting-started/_static/image12.png)

W górnej części znajduje się pasek narzędzi Szybki dostęp i wstążka. W lewym dolnym rogu zobaczysz selektor obszaru roboczego, w którym można przełączać się między zadaniami (**lokacja**, **pliki**, **bazy danych**, **raporty**). Po prawej stronie znajduje się okienko zawartość dla edytora i raportów. I w dolnej części czasu okresowo zobaczysz pasek powiadomień dla komunikatów.

Dowiesz się więcej na temat WebMatrix i jej funkcji w ramach tych samouczków.

## <a name="creating-a-web-page"></a>Tworzenie strony sieci Web

Aby zapoznać się ze stronami sieci Web WebMatrix i ASP.NET, utworzysz prostą stronę.

W selektorze obszaru roboczego wybierz obszar roboczy **pliki** . Ten obszar roboczy umożliwia pracy z plikami i folderami. W okienku po lewej stronie zostanie wyświetlona struktura plików witryny. Wstążka zmienia się w celu wyświetlenia zadań związanych z plikami.

![Obszar roboczy plików w programie WebMatrix](getting-started/_static/image13.png)

Na wstążce kliknij strzałkę w obszarze **Nowy** , a następnie kliknij pozycję **nowy plik**.

![Za pomocą polecenia &quot;New&quot; na Wstążce, aby utworzyć nowy plik](getting-started/_static/image14.png)

W programie WebMatrix zostanie wyświetlona lista typów plików. Wybierz pozycję **cshtml**, a w polu **Nazwa** wpisz "HelloWorld". Strona CSHTML jest stroną stron sieci Web ASP.NET.

![Tworzenie nowej strony CSHTML o nazwie HelloWorld. cshtml](getting-started/_static/image15.png)

Kliknij przycisk **OK**.

Program WebMatrix utworzy stronę i otworzy ją w edytorze.

![Nowa strona HelloWorld w edytorze WebMatrix](getting-started/_static/image16.png)

Jak widać, Strona zawiera głównie zwykłe znaczniki HTML, z wyjątkiem bloku znajdującego się u góry, który wygląda następująco:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

W ten sposób można dodać kod, ponieważ wkrótce zobaczysz.

Zauważ, że różne części strony &mdash; nazw elementów, atrybutów i tekstu oraz blok u góry — wszystkie kolory. Jest to tzw. *wyróżnianie składni*i ułatwia zachowanie wszystkiego. Jest to jedna z funkcji, które ułatwiają współpracę ze stronami sieci Web w programie WebMatrix.

Dodaj zawartość dla `<head>` i `<body>` elementów, takich jak w poniższym przykładzie. (Jeśli chcesz, możesz po prostu skopiować następujący blok i zastąpić całą istniejącą stronę tym kodem).

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Na pasku narzędzi Szybki dostęp lub w menu **plik** kliknij polecenie **Zapisz**.

![Przycisk Zapisz na pasku narzędzi Szybki dostęp w programie WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testowanie strony

W obszarze roboczym **pliki** kliknij prawym przyciskiem myszy stronę *HelloWorld. cshtml* , a następnie kliknij polecenie **Uruchom w przeglądarce**.

![Uruchamianie strony przy użyciu przycisku Uruchom na Wstążce WebMatrix](getting-started/_static/image18.png)

W programie WebMatrix jest uruchamiany wbudowany serwer sieci Web (IIS Express), którego można użyć do testowania stron na komputerze. (Bez IIS Express w programie WebMatrix przed przetestowaniem należy opublikować stronę na serwerze sieci Web). Ta strona zostanie wyświetlona w domyślnej przeglądarce.

![&quot;Hello world&quot; stronie działającej w przeglądarce](getting-started/_static/image19.png)

Należy zauważyć, że podczas testowania strony w programie WebMatrix adres URL w przeglądarce jest podobny do `http://localhost:33651/HelloWorld.cshtml.` nazwa *localhost* odnosi się do serwera lokalnego, co oznacza, że strona jest obsługiwana przez serwer sieci Web, który znajduje się na własnym komputerze. Jak wspomniano, WebMatrix zawiera program serwera sieci Web o nazwie IIS Express, który jest uruchamiany podczas uruchamiania strony.

Liczba po *hoście lokalnym* (na przykład *localhost: 33651*) odnosi się do *numeru portu* na komputerze. Jest to numer "kanału", którego IIS Express używa dla tej konkretnej witryny sieci Web. Numer portu jest wybierany losowo z zakresu od 1024 do 65536 podczas tworzenia lokacji i różni się dla każdej utworzonej lokacji. (Podczas testowania własnej lokacji numer portu niemal nie będzie liczbą inną niż 33561). Korzystając z innego portu dla każdej witryny sieci Web, IIS Express może obsłużyć proste, z których witryn.

Później, po opublikowaniu witryny na publicznym serwerze sieci Web, w adresie URL nie jest już wyświetlany numer *localhost* . W tym momencie zobaczysz bardziej typowy adres URL, taki jak `http://myhostingsite/mywebsite/HelloWorld.cshtml` lub niezależnie od strony. Dowiesz się więcej na temat publikowania witryny w dalszej części tej serii samouczków.

## <a name="adding-some-server-side-code"></a>Dodawanie kodu po stronie serwera

Zamknij przeglądarkę i wróć do strony w programie WebMatrix.

Dodaj wiersz do bloku kodu, tak aby wyglądał następująco:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Jest to niewielki kod kodu Razor. Prawdopodobnie jest jasne, że pobiera bieżącą datę i godzinę oraz umieszcza tę wartość w *zmiennej* o nazwie `currentDateTime`. Więcej informacji na temat składnia Razor można znaleźć w następnym samouczku.

W treści strony, po elemencie `<p>Hello World!</p>` Dodaj następujące elementy:

[!code-html[Main](getting-started/samples/sample4.html)]

Ten kod pobiera wartość umieszczoną w zmiennej `currentDateTime` u góry i wstawia ją do znacznika strony. Znak `@` oznacza kod stron sieci Web ASP.NET na stronie.

Uruchom ponownie stronę (WebMatrix zapisuje zmiany przed uruchomieniem strony). Tym razem zobaczysz datę i godzinę na stronie.

![&quot;Hello world&quot; strony działającej w przeglądarce z wygenerowanym dynamicznie ekranem czasu](getting-started/_static/image20.png)

Poczekaj chwilę, a następnie Odśwież stronę w przeglądarce. Wyświetlanie daty i godziny jest aktualizowane.

W przeglądarce Sprawdź Źródło strony. Wygląda ona podobnie do następującej:

[!code-html[Main](getting-started/samples/sample5.html)]

Zauważ, że blok `@{ }` w górnej części nie znajduje się w tym miejscu. Zauważ również, że wyświetlanie daty i godziny wyświetla rzeczywisty ciąg znaków (`1/18/2012 2:49:50 PM` lub niezależnie), a nie `@currentDateTime` jak na stronie *. cshtml* . Co się stało z tym, że po uruchomieniu strony ASP.NET przetworzył cały kod (bardzo mało w tym przypadku), który został oznaczony `@`. Kod generuje dane wyjściowe, a dane wyjściowe zostały wstawione na stronie.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Oto co to jest ASP.NET stron sieci Web

Po przeczytaniu, że ASP.NET strony sieci Web tworzą dynamiczną zawartość internetową, zobaczysz, co widzisz tutaj. Właśnie utworzona strona zawiera ten sam znacznik HTML, który był wcześniej widoczny. Może również zawierać kod, który może wykonywać różne zadania. W tym przykładzie było to proste zadanie pobierania bieżącej daty i godziny. Jak widać, możesz przeplatać kod przy użyciu kodu HTML, aby utworzyć dane wyjściowe na stronie. Gdy ktoś zażąda strony *. cshtml* w przeglądarce, ASP.NET przetworzy Tę stronę, gdy nadal znajduje się ona w ręce serwera sieci Web. ASP.NET wstawia dane wyjściowe kodu (jeśli istnieją) do strony jako HTML. Po zakończeniu przetwarzania kodu ASP.NET wysyła wyniki do przeglądarki. Wszystkie kiedykolwiek pobierane przeglądarki są w formacie HTML. Oto Diagram:

![Koncepcyjny przepływ, w jaki sposób ASP.NET generuje HTML dynamicznie](getting-started/_static/image21.png)

Pomysł jest prosty, ale istnieje wiele interesujących zadań, które może wykonywać kod, i istnieje wiele interesujących sposobów, w których można dynamicznie dodać zawartość HTML do strony. I ASP.NET *. cshtml* strony, podobnie jak każda strona HTML, może również zawierać kod, który działa w przeglądarce (kod JavaScript i jQuery). Wszystkie te elementy zostaną omówione w tym zestawie samouczków i w kolejnych.

## <a name="coming-up-next"></a>Przyszłe przejście

W następnym samouczku w tej serii przedstawiono ASP.NET stron sieci Web, które ułatwiają programowanie.

## <a name="additional-resources"></a>Dodatkowe materiały

[Utwórz witrynę sieci web ASP.NET od podstaw](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Jest to samouczek, który służy głównie do korzystania z WebMatrix (nie ASP.NET stron sieci Web). Zawiera nieco więcej szczegółów na temat niektórych dodatkowych funkcji programu WebMatrix, które nie zostały omówione w tym zestawie samouczków.

> [!div class="step-by-step"]
> [Dalej](intro-to-web-pages-programming.md)

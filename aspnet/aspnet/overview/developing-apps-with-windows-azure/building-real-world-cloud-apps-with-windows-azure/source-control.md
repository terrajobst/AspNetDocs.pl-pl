---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Kontrola źródła (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: a6f445e46d41b646cf6c25af2e65bc73e831d5ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583708"
---
# <a name="source-control-building-real-world-cloud-apps-with-azure"></a>Kontrola źródła (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Kontrola źródła jest istotna dla wszystkich projektów programowania w chmurze, a nie tylko środowisk zespołowych. Nie będzie można edytować kodu źródłowego, nawet dokumentu programu Word bez funkcji Undo i automatycznych kopii zapasowych, a kontrola źródła udostępnia te funkcje na poziomie projektu, gdzie mogą zaoszczędzić jeszcze więcej czasu, gdy coś się nie stało. Dzięki usługom kontroli źródła w chmurze nie trzeba już martwić się o skomplikowaną konfigurację i można użyć Azure Repos kontroli źródła bezpłatnie dla maksymalnie 5 użytkowników.

W pierwszej części tego rozdziału objaśniono trzy najważniejsze wskazówki, które należy wziąć pod uwagę:

- [Traktuj skrypty automatyzacji jako kod źródłowy](#scripts) i wersje ich razem z kodem aplikacji.
- [Nigdy nie zaznaczaj wpisów tajnych](#secrets) (danych poufnych, takich jak poświadczenia) w repozytorium kodu źródłowego.
- [Skonfiguruj gałęzie źródłowe](#devops) , aby włączyć przepływ pracy DevOps.

W pozostałej części rozdziału przedstawiono przykładowe implementacje tych wzorców w programie Visual Studio, na platformie Azure i Azure Repos:

- [Dodawanie skryptów do kontroli źródła w programie Visual Studio](#vsscripts)
- [Przechowywanie poufnych danych na platformie Azure](#appsettings)
- [Korzystanie z usługi Git w programie Visual Studio i Azure Repos](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Traktuj skrypty automatyzacji jako kod źródłowy

Podczas pracy nad projektem w chmurze często zmienia się elementy i chcesz szybko reagować na problemy zgłoszone przez klientów. Szybkie reagowanie polega na użyciu skryptów automatyzacji, jak wyjaśniono w rozdziale [Automatyzowanie wszystkiego](automate-everything.md) . Wszystkie skrypty, które są używane do tworzenia środowiska, wdrażania na nim, skalowania go itp., muszą być zsynchronizowane z kodem źródłowym aplikacji.

Aby zachować synchronizację skryptów z kodem, należy je zapisać w systemie kontroli źródła. Jeśli kiedykolwiek zajdzie potrzeba wycofania zmian lub dokonania szybkiej naprawy kodu produkcyjnego, który jest inny niż kod programistyczny, nie trzeba tracić czasu na próbę śledzenia ustawień, które uległy zmianie lub których członkowie zespołu mają kopie potrzebnej wersji. Masz pewność, że potrzebne skrypty są zsynchronizowane z bazą kodu, w której są potrzebne, i masz pewność, że wszyscy członkowie zespołu pracują z tymi samymi skryptami. Następnie, niezależnie od tego, czy musisz zautomatyzować testowanie i wdrażanie gorącej poprawki w środowisku produkcyjnym, czy nowej funkcji, masz odpowiedni skrypt dla kodu, który należy zaktualizować.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Nie sprawdzaj wpisów tajnych

Repozytorium kodu źródłowego jest zazwyczaj dostępne dla zbyt wielu osób, aby było odpowiednie bezpieczne miejsce na poufne dane, takie jak hasła. Jeśli skrypty są zależne od wpisów tajnych, takich jak hasła, Sparametryzuj te ustawienia, aby nie zapisywali ich w kodzie źródłowym i przechowywać wpisy tajne w innym miejscu.

Na przykład platforma Azure umożliwia pobieranie plików zawierających ustawienia publikowania w celu zautomatyzowania tworzenia profilów publikacji. Te pliki obejmują nazwy użytkowników i hasła, które są autoryzowane do zarządzania usługami platformy Azure. Jeśli ta metoda zostanie użyta do utworzenia profilów publikowania i w przypadku zaewidencjonowania tych plików w kontroli źródła, każda osoba mająca dostęp do repozytorium będzie widziała te nazwy użytkowników i hasła. Hasło można bezpiecznie zapisać w samym profilu publikacji, ponieważ jest ono zaszyfrowane i znajduje się w pliku *. pubxml. User* , który domyślnie nie jest uwzględniony w kontroli źródła.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Gałęzie źródłowe struktury do ułatwienia przepływu pracy DevOps

Zaimplementowanie gałęzi w repozytorium ma wpływ na możliwość tworzenia nowych funkcji i rozwiązywania problemów w środowisku produkcyjnym. Oto wzorzec, z którego korzystają wiele zespołów o średniej wielkości:

![Źródłowa struktura gałęzi](source-control/_static/image1.png)

Gałąź główna zawsze jest zgodna z kodem, który znajduje się w środowisku produkcyjnym. Gałęzie pod wzorcem odpowiadają różnym etapom cyklu życia deweloperskiego. Gałąź programistyczna to miejsce wdrożenia nowych funkcji. W przypadku małego zespołu może być tylko główny i programistyczny, ale często zalecamy, aby ludzie mieli rozgałęzienie przejściowe między programowaniem a serwerem głównym. Można użyć przemieszczania na potrzeby końcowego testowania integracji przed przeniesieniem aktualizacji do środowiska produkcyjnego.

W przypadku dużych zespołów może istnieć osobne rozgałęzienia dla każdej nowej funkcji; w przypadku mniejszych zespołów może się zdarzyć, że wszyscy znajdą się w gałęzi programowanie.

Jeśli masz gałąź dla każdej funkcji, podczas gdy funkcja A jest gotowa, należy scalić kod źródłowy zmiany w gałęzi deweloperskiej i w dół w innych gałęziach funkcji. Ten proces scalania kodu źródłowego może być czasochłonny, a aby uniknąć tego, że nadal pracują z innymi funkcjami, niektóre zespoły implementują alternatywną nazwę *[przełączników funkcji](http://en.wikipedia.org/wiki/Feature_toggle)* (nazywanych również *flagami funkcji*). Oznacza to, że cały kod dla wszystkich funkcji znajduje się w tej samej gałęzi, ale każdą funkcję można włączać lub wyłączać za pomocą przełączników w kodzie. Załóżmy na przykład, że funkcja A to nowe pole do naprawienia zadań aplikacji IT, a funkcja B dodaje funkcję buforowania. Kod dla obu funkcji może znajdować się w gałęzi deweloperskiej, ale aplikacja wyświetli nowe pole tylko wtedy, gdy zmienna jest ustawiona na wartość true i będzie używać buforowania tylko wtedy, gdy zmienna ma ustawioną wartość true. Jeśli funkcja A nie jest gotowa do awansowania, ale funkcja B jest gotowa, można podwyższyć poziom całego kodu do środowiska produkcyjnego za pomocą funkcji przełącznika i przełączyć funkcję B. Następnie można zakończyć funkcję A i podnieść ją później, bez scalania kodu źródłowego.

Niezależnie od tego, czy używasz gałęzi, czy przełączników dla funkcji, struktura rozgałęzienia, taka jak ta, umożliwia przepływ kodu od projektowania do produkcji w sposób Agile i powtarzalny.

Ta struktura umożliwia również szybkie reagowanie na Opinie klientów. Jeśli musisz wprowadzić szybką poprawkę do środowiska produkcyjnego, możesz to zrobić efektywnie w sposób Agile. Można utworzyć gałąź poza główną lub przejściową, a gdy jest ona gotowa do scalenia z gałęziami tworzenia i działania.

![Rozgałęzienie poprawek](source-control/_static/image2.png)

Bez struktury rozgałęziania, takiej jak w przypadku rozdzielenia gałęzi produkcyjnych i programistycznych, problem produkcyjny może pomieścić się w sytuacji, w której można promować nowy kod funkcji wraz z poprawkami produkcyjnymi. Nowy kod funkcji może nie być w pełni przetestowany i gotowy do produkcji. może być konieczne wykonanie wielu zadań związanych z tworzeniem kopii zapasowych, które nie są gotowe. Lub może zajść konieczność opóźnienia poprawki w celu przetestowania zmian i przygotowania ich do wdrożenia.

Następnie zobaczysz przykłady sposobów implementacji tych trzech wzorców w programie Visual Studio, na platformie Azure i Azure Repos. Są to przykłady, a nie szczegółowe instrukcje krok po kroku do wykonania. Szczegółowe instrukcje, które zawierają wszystkie wymagane kontekstu, można znaleźć w sekcji [Resources (zasoby](#resources) ) na końcu rozdziału.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Dodawanie skryptów do kontroli źródła w programie Visual Studio

Skrypty można dodawać do kontroli źródła w programie Visual Studio, dołączając je do folderu rozwiązania programu Visual Studio (przy założeniu, że projekt jest w kontroli źródła). Oto jeden ze sposobów, aby to zrobić.

Utwórz folder dla skryptów w folderze rozwiązania (ten sam folder, w którym znajduje się plik *. sln* ).

![Folder automatyzacji](source-control/_static/image3.png)

Skopiuj pliki skryptów do folderu.

![Zawartość folderu automatyzacji](source-control/_static/image4.png)

W programie Visual Studio Dodaj do projektu folder rozwiązania.

![Wybór menu nowego folderu rozwiązania](source-control/_static/image5.png)

I Dodaj pliki skryptów do folderu rozwiązania.

![Wybór menu Dodaj istniejący element](source-control/_static/image6.png)

![Okno dialogowe Dodawanie istniejącego elementu](source-control/_static/image7.png)

Pliki skryptów są teraz zawarte w projekcie i kontrola źródła śledzi zmiany wersji wraz z odpowiednimi zmianami kodu źródłowego.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Przechowywanie poufnych danych na platformie Azure

Jeśli aplikacja jest uruchamiana w witrynie sieci Web systemu Azure, jeden ze sposobów na uniknięcie przechowywania poświadczeń w kontroli źródła polega na tym, że przechowuje je na platformie Azure.

Na przykład poprawka IT jest przechowywana w pliku Web. config dwa parametry połączenia, które będą miały hasła w środowisku produkcyjnym i klucz zapewniający dostęp do konta usługi Azure Storage.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Jeśli wprowadzisz rzeczywiste wartości produkcyjne dla tych ustawień w pliku *Web. config* lub umieścisz je w pliku *Web. release. config* , aby skonfigurować transformację pliku Web. config w celu wstawienia ich podczas wdrażania, zostaną one zapisane w repozytorium źródłowym. W przypadku wprowadzenia parametrów połączenia bazy danych do profilu publikowania produkcyjnego hasło będzie w pliku *. pubxml* . (Można wykluczyć plik *pubxml* z kontroli źródła, ale utracisz korzyści wynikające z udostępniania wszystkich innych ustawień wdrożenia).

Platforma Azure oferuje alternatywę dla sekcji **AppSettings** i parametrów połączenia w pliku *Web. config* . Poniżej znajduje się odpowiednia część karty **Konfiguracja** witryny sieci Web w portalu zarządzania systemu Azure:

![appSettings i connectionStrings w portalu](source-control/_static/image8.png)

Podczas wdrażania projektu w tej witrynie sieci Web i uruchamiania aplikacji, wszelkie wartości przechowywane na platformie Azure zastępują wszelkie wartości w pliku Web. config.

Te wartości można ustawić na platformie Azure przy użyciu portalu zarządzania lub skryptów. Skrypt automatyzacji tworzenia środowiska, który został wyświetlony w rozdziale [Automatyzacja wszystkiego](automate-everything.md) , tworzy Azure SQL Database, pobiera parametry połączenia magazynu i SQL Database i przechowuje te wpisy tajne w ustawieniach witryny sieci Web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Zauważ, że skrypty są sparametryzowane, aby wartości rzeczywiste nie były utrwalane w repozytorium źródłowym.

Po uruchomieniu lokalnie w środowisku deweloperskim aplikacja odczytuje lokalny plik Web. config, a parametry połączenia wskazują na bazę danych LocalDB SQL Server w folderze *dane\_aplikacji* projektu sieci Web. Po uruchomieniu aplikacji na platformie Azure, gdy aplikacja próbuje odczytać te wartości z pliku Web. config, jego zawartość jest powracana i jest zastosowana dla witryny sieci Web, a nie w rzeczywistości w pliku Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Korzystanie z narzędzia Git w programie Visual Studio i platformie Azure DevOps

W celu zaimplementowania struktury rozgałęziania DevOps przedstawionej wcześniej można użyć dowolnego środowiska kontroli źródła. W przypadku zespołów rozproszonych [system kontroli wersji rozproszonej](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) może najlepiej współpracować z systemem w przypadku innych zespołów [scentralizowany system](http://en.wikipedia.org/wiki/Revision_control) może usprawnić działanie.

[Git](http://git-scm.com/) to popularny system kontroli wersji rozproszonej. W przypadku korzystania z narzędzia Git na potrzeby kontroli źródła masz kompletną kopię repozytorium ze wszystkimi swoimi historiami na komputerze lokalnym. Wiele osób preferuje, aby łatwiej kontynuować pracę, gdy nie masz połączenia z siecią — możesz kontynuować zatwierdzanie i wycofywanie, tworzenie i przełączanie gałęzi itd. Nawet jeśli masz połączenie z siecią, łatwiej i szybciej twórz gałęzie i przełączaj gałęzie, gdy wszystko jest lokalne. Możesz również wykonywać lokalne zatwierdzenia i wycofywania bez wpływu na innych deweloperów. Można też wykonać zadania wsadowe przed wysłaniem ich do serwera.

[Azure Repos](/azure/devops/repos/index?view=vsts) oferuje zarówno usługę [git](/azure/devops/repos/git/?view=vsts) , jak i [Kontrola wersji serwera Team Foundation](/azure/devops/repos/tfvc/index?view=vsts) (TFVC; scentralizowana kontrola źródła). Zacznij korzystać z usługi Azure DevOps [tutaj](https://app.vsaex.visualstudio.com/signup).

Program Visual Studio 2017 zawiera wbudowaną, pierwszą w swojej klasie [obsługę usługi git](https://msdn.microsoft.com/library/hh850437.aspx). Oto krótki pokaz, jak to działa.

Mając otwarty projekt w programie Visual Studio, kliknij prawym przyciskiem myszy rozwiązanie w **Eksplorator rozwiązań**, a następnie wybierz polecenie **Dodaj rozwiązanie do kontroli źródła**.

![Dodaj rozwiązanie do kontroli źródła](source-control/_static/image9.png)

Program Visual Studio pyta, czy chcesz używać TFVC (scentralizowany system kontroli wersji) czy usługi git.

![Wybierz kontrolę źródła](source-control/_static/image10.png)

Po wybraniu usługi git i kliknięciu przycisku **OK**program Visual Studio tworzy nowe lokalne repozytorium Git w folderze rozwiązania. Nowe repozytorium nie ma jeszcze żadnych plików; musisz dodać je do repozytorium, wykonując zatwierdzenie git. Kliknij prawym przyciskiem myszy rozwiązanie w **Eksplorator rozwiązań**, a następnie kliknij pozycję **Zatwierdź**.

![Zleca](source-control/_static/image11.png)

Program Visual Studio automatycznie etapuje wszystkie pliki projektu dla zatwierdzenia i wyświetla je w **Team Explorer** w okienku **uwzględnione zmiany** . (Jeśli niektóre z nich nie zostały uwzględnione w zatwierdzeniu, można je zaznaczyć, kliknąć prawym przyciskiem myszy, a następnie kliknąć pozycję **Wyklucz**).

![Team Explorer](source-control/_static/image12.png)

Wprowadź komentarz dotyczący zatwierdzania, a następnie kliknij przycisk **Zatwierdź**, a program Visual Studio wykonuje zatwierdzenie i wyświetla identyfikator zatwierdzenia.

![Team Explorer zmiany](source-control/_static/image13.png)

Teraz jeśli zmienisz jakiś kod, tak aby różnił się od tego, co znajduje się w repozytorium, możesz łatwo wyświetlić te różnice. Kliknij prawym przyciskiem myszy plik, który został zmieniony, a następnie wybierz pozycję **Porównaj z niezmodyfikowanym**, a następnie Wyświetl widok z porównaniem, który pokazuje Niezatwierdzone zmiany.

![Porównaj z niezmodyfikowanym](source-control/_static/image14.png)

![Różnice pokazujące zmiany](source-control/_static/image15.png)

Możesz łatwo zobaczyć, jakie zmiany są wprowadzane, i zapoznaj się z nimi.

Załóżmy, że trzeba utworzyć gałąź — można to zrobić w programie Visual Studio. W **Team Explorer**kliknij pozycję **nowe rozgałęzienie**.

![Team Explorer nowe rozgałęzienie](source-control/_static/image16.png)

Wprowadź nazwę gałęzi, kliknij pozycję **Utwórz gałąź**i w przypadku wybrania **gałęzi wyewidencjonowania**program Visual Studio automatycznie wyewidencjonuje nową gałąź.

![Team Explorer nowe rozgałęzienie](source-control/_static/image17.png)

Teraz można wprowadzać zmiany w plikach i zaewidencjonować je do tej gałęzi. Można łatwo przełączać się między gałęziami i program Visual Studio automatycznie synchronizuje pliki z niezależną gałęzią, która została sprawdzona. W tym przykładzie tytuł strony sieci Web w *\_Layout. cshtml* został zmieniony na "gorąca poprawka 1" w gałęzi gałąź poprawka1.

![Gałąź gałąź poprawka1](source-control/_static/image18.png)

Jeśli przełączysz się z powrotem do gałęzi głównej, zawartość pliku *\_Layout. cshtml* automatycznie powraca do wartości znajdujących się w gałęzi głównej.

![Gałąź główna](source-control/_static/image19.png)

Ten prosty przykład sposobu, w jaki można szybko utworzyć gałąź i odwrócić i przejść między gałęziami. Ta funkcja umożliwia przepływ pracy wysoce Agile przy użyciu struktury gałęzi i skryptów automatyzacji przedstawionych w rozdziale [Automatyzowanie wszystkiego](automate-everything.md) . Na przykład możesz pracować w gałęzi deweloperskiej, utworzyć rozgałęzienie gorącej poprawki z klasy głównej, przełączyć się do nowej gałęzi, wprowadzić zmiany i zatwierdzić je, a następnie przełączyć się z powrotem do gałęzi deweloperskiej i kontynuować działanie.

Informacje o tym, co było widoczne w tym miejscu, to sposób pracy z lokalnym repozytorium Git w programie Visual Studio. W środowisku zespołu zwykle są również wypychane zmiany do wspólnego repozytorium. Narzędzia programu Visual Studio pozwalają również wskazać zdalne repozytorium git. Możesz użyć GitHub.com do tego celu lub użyć usług [git i Azure Repos](/azure/devops/repos/git/overview?view=vsts) zintegrowanych ze wszystkimi innymi funkcjami DevOps platformy Azure, takimi jak elementy robocze i śledzenie błędów.

Nie jest to jedyny sposób, w jaki można wdrożyć strategię rozgałęziania Agile. Ten sam przepływ pracy Agile można włączyć przy użyciu scentralizowanego repozytorium kontroli źródła.

## <a name="summary"></a>Podsumowanie

Zmierz sukces systemu kontroli źródła w zależności od tego, jak szybko możesz wprowadzić zmianę i uzyskać ją na żywo w bezpieczny i przewidywalny sposób. Jeśli okaże się, że Obawialiśmy się zmienić, ponieważ trzeba wykonać dzień lub dwa testy ręczne, możesz zadawać sobie informacje o tym, co należy zrobić, aby wykonać tę zmianę w ciągu kilku minut lub najgorszenia czasu. Jedną z strategii wykonywania tej czynności jest zaimplementowanie ciągłej integracji i ciągłego dostarczania, które zajmiemy się w [następnym rozdziale](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Resources

Aby uzyskać więcej informacji na temat strategii rozgałęziania, zobacz następujące zasoby:

- [Tworzenie potoku wydania z Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Dokumentacja dotycząca wzorców i praktyk firmy Microsoft. Zobacz rozdział 6, aby zapoznać się z omówieniem strategii rozgałęzień. W przypadku korzystania z funkcji użytkownicy mogą przełączać gałęzie funkcji, a jeśli są używane gałęzie funkcji, są one niekrótsze (w godzinach lub w dniach).
- [Przewodnik kontroli wersji](https://aka.ms/vsarsolutions). Przewodnik po rozgałęzieniu strategii przez zakresy ALM. Zobacz rozgałęzianie strategii. PDF na karcie pliki do pobrania.
- [Programowanie oprogramowania z przełącznikami funkcji](https://msdn.microsoft.com/magazine/dn683796.aspx). Artykuł magazynu MSDN.
- [Przełącznik funkcji](http://martinfowler.com/bliki/FeatureToggle.html). Wprowadzenie do funkcji przełączania/flagi funkcji w blogu Fowlera.
- [Funkcja przełącza gałęzie funkcji vs](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Inny wpis w blogu dotyczący przełączników funkcji, Dylan Smith.

Aby uzyskać więcej informacji o sposobie obsługi poufnych informacji, które nie powinny być przechowywane w repozytoriach kontroli źródła, zobacz następujące zasoby:

- [Najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych w ASP.NET i Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Witryny sieci Web systemu Azure: sposób działania ciągów aplikacji i parametrów połączenia](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Wyjaśnia funkcję platformy Azure, która zastępuje `appSettings` i `connectionStrings` dane w pliku *Web. config* .
- [Niestandardowe ustawienia konfiguracji i aplikacji w witrynach sieci Web systemu Azure — z Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Aby uzyskać informacje o innych metodach utrzymywania poufnych informacji z kontroli źródła, zobacz [ASP.NET MVC: utrzymywanie ustawień prywatnych poza kontrolą źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Poprzednie](automate-everything.md)
> [dalej](continuous-integration-and-continuous-delivery.md)

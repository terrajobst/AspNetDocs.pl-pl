---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Wdrażanie witryny przy użyciu programu Visual Studio (VB) | Microsoft Docs
author: rick-anderson
description: Program Visual Studio zawiera narzędzia do wdrażania witryny sieci Web. Dowiedz się więcej na temat tych narzędzi w tym samouczku.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c71e36a8a434947882cc767cd2f903ff6e8d422
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582522"
---
# <a name="deploying-your-site-using-visual-studio-vb"></a>Wdrażanie witryny przy użyciu programu Visual Studio (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Program Visual Studio zawiera narzędzia do wdrażania witryny sieci Web. Dowiedz się więcej na temat tych narzędzi w tym samouczku.

## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku pokazano, jak wdrożyć prostą aplikację sieci Web ASP.NET dla dostawcy hosta sieci Web. W tym samouczku pokazano, jak używać klienta FTP, takiego jak FileZilla, aby przenieść niezbędne pliki ze środowiska programistycznego do środowiska produkcyjnego. Program Visual Studio oferuje również wbudowane narzędzia ułatwiające wdrażanie dostawcy hosta sieci Web. W tym samouczku przedstawiono dwa z tych narzędzi: Narzędzie do kopiowania witryn sieci Web, w którym można przenieść pliki do i ze zdalnego serwera sieci Web przy użyciu protokołu FTP lub rozszerzenia FrontPage Server Extensions; i narzędzia do publikowania, które kopiuje całą witrynę sieci Web do określonej lokalizacji.

> [!NOTE]
> Inne narzędzia związane z wdrożeniem oferowane przez program Visual Studio obejmują [projekty konfiguracji sieci Web](https://msdn.microsoft.com/library/wx3b589t.aspx) i dodatek [projektów wdrażania w sieci Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) . Projekty Instalatora sieci Web pakiet zawartości witryny internetowej i informacje o konfiguracji do jednego pliku MSI. Ta opcja jest najbardziej przydatna w przypadku witryn sieci Web, które są wdrożone w intranecie lub w firmach, które sprzedajeją wstępnie spakowaną aplikację internetową, którą klienci instalują na własnych serwerach sieci Web. Dodatek projektów wdrażania w sieci Web jest dodatkiem programu Visual Studio, który ułatwia określenie różnic między kompilacjami w środowiskach deweloperskich i środowiskami produkcyjnymi. Projekty konfiguracji sieci Web nie są omawiane w tej serii samouczków. Projekty wdrażania w sieci Web są sumowane według wspólnych różnic konfiguracji w samouczku dotyczącym [*tworzenia i produkcji*](common-configuration-differences-between-development-and-production-vb.md) .

## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Wdrażanie witryny przy użyciu narzędzia do kopiowania witryny sieci Web

Narzędzie do kopiowania witryn sieci Web programu Visual Studio jest podobne do autonomicznego klienta FTP. W Nutshell narzędzie kopiowania witryny sieci Web umożliwia nawiązywanie połączenia ze zdalną witryną sieci Web za pomocą protokołu FTP lub rozszerzenia FrontPage Server Extensions. Podobnie jak w przypadku interfejsu użytkownika FileZilla, interfejs użytkownika kopiowania witryny sieci Web składa się z dwóch okienek: w okienku po prawej stronie znajdują się pliki lokalne, które są wyświetlane na serwerze docelowym.

> [!NOTE]
> Narzędzie kopiowania witryny sieci Web jest dostępne tylko dla projektów witryny sieci Web. Program Visual Studio oferuje to narzędzie podczas pracy z projektem aplikacji sieci Web.

Przyjrzyjmy się narzędziu Kopiuj witrynę sieci Web, aby opublikować aplikację przegląd książki w środowisku produkcyjnym. Ponieważ narzędzie do kopiowania witryn sieci Web działa tylko z projektami, które używają modelu projektu witryny sieci Web, można je sprawdzać tylko za pomocą tego narzędzia w projekcie BookReviewsWSP. Otwórz ten projekt.

Uruchom projekt narzędzia kopiowania witryny sieci Web, klikając ikonę Kopiuj witrynę sieci Web w Eksplorator rozwiązań (ikona jest zakreślona na rysunku 1). Alternatywnie możesz wybrać opcję Kopiuj witrynę sieci Web z menu witryna sieci Web. Jedno z metod uruchamia interfejs użytkownika kopiowania witryny sieci Web pokazany na rysunku 1. tylko lewe okienko na rysunku 1 jest wypełniane, ponieważ nastąpiło jeszcze połączenie z serwerem zdalnym.

[![interfejs użytkownika narzędzia kopiowania witryny sieci Web jest podzielony na dwa okienka](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Rysunek 1**. interfejs użytkownika narzędzia kopiowania witryny sieci Web jest podzielony na dwa okienka ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image3.png))

W celu wdrożenia naszej witryny musimy najpierw połączyć się z dostawcą hosta sieci Web. Kliknij przycisk Połącz w górnej części interfejsu użytkownika kopiowania witryny sieci Web. Zostanie wyświetlone okno dialogowe Otwórz witrynę sieci Web pokazane na rysunku 2.

Możesz połączyć się z docelową witryną sieci Web, wybierając jedną z czterech opcji z lewej strony:

- **System plików** — wybierz tę opcję, aby wdrożyć lokację do folderu lub udziału sieciowego dostępnego na komputerze.
- **Lokalne usługi IIS** — Użyj tej opcji, aby wdrożyć lokację na serwerze sieci Web usług IIS zainstalowanym na komputerze.
- **Witryna FTP** — łączenie ze zdalną witryną sieci Web przy użyciu protokołu FTP.
- **Lokacja zdalna** — łączenie z zdalną witryną sieci Web przy użyciu rozszerzenia FrontPage Server Extensions.

Większość dostawców hosta sieci Web obsługuje protokół FTP, ale mniej oferuje obsługę rozszerzenia serwera FrontPage. Z tego powodu została wybrana opcja witryna FTP, a następnie wprowadzono informacje o połączeniu, jak pokazano na rysunku 2.

[![określić docelową witrynę sieci Web](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Rysunek 2**. Określanie docelowej witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image6.png))

Po nawiązaniu połączenia narzędzie do kopiowania witryn sieci Web ładuje pliki w zdalnej lokacji w okienku po prawej stronie i wskazuje stan każdego pliku: nowy, usunięty, zmieniony lub niezmieniony. Plik można skopiować z lokacji lokalnej do zdalnej lub na odwrót.

Dodajmy nową stronę do projektu BookReviewsWSP, a następnie wdrażamy ją tak, aby można było zobaczyć narzędzie do kopiowania witryn sieci Web w działaniu. Utwórz nową stronę ASP.NET w programie Visual Studio w katalogu głównym o nazwie `Privacy.aspx`. Aby strona była używana na stronie głównej `Site.master` i Dodaj do niej zasady zachowania poufności informacji. Rysunek 3 przedstawia program Visual Studio po utworzeniu tej strony.

[![dodać nową stronę o nazwie &lt;Code&gt;privacy. aspx&lt;/Code&gt; do folderu głównego witryny sieci Web](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Rysunek 3**. Dodaj nową stronę o nazwie `Privacy.aspx` do folderu głównego witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image9.png))

Następnie wróć do interfejsu użytkownika kopiowania witryny sieci Web. Jak pokazano na rysunku 4, okienko po lewej stronie zawiera teraz nowe pliki — `Policy.aspx` i `Policy.aspx.vb`. Co więcej, te pliki są oznaczone ikoną strzałki i stanem New, wskazując, że istnieją w lokacji lokalnej, ale nie w witrynie zdalnej.

[![narzędzie do kopiowania witryny sieci Web zawiera nowy kod &lt;&gt;privacy. aspx&lt;/Code&gt; stronę w lewym okienku](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Rysunek 4**. Narzędzie Kopiuj witrynę sieci Web zawiera nową stronę `Privacy.aspx` w lewym okienku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image12.png))

Aby wdrożyć nowe pliki, wybierz je, a następnie kliknij ikonę strzałki, aby przenieść je do lokacji zdalnej. Po zakończeniu transferu `Policy.aspx` pliki `Policy.aspx.vb` istnieją zarówno w lokacjach lokalnych, jak i zdalnych o stanie niezmieniony.

Wraz z listą nowych plików narzędzie kopiowania witryny sieci Web wyróżnia wszystkie pliki, które różnią się między lokacjami lokalnymi i zdalnymi. Aby wyświetlić tę akcję, Wróć do strony `Privacy.aspx` i Dodaj kilka wyrazów do zasad ochrony prywatności. Zapisz stronę, a następnie wróć do narzędzia kopiowania witryny sieci Web. Jak pokazano na rysunku 5, Strona `Privacy.aspx` w lewym okienku ma stan zmieniony, co oznacza, że nie jest zsynchronizowana z lokacją zdalną.

[![narzędzie kopiowania witryny sieci Web wskazuje, że kod &lt;&gt;privacy. aspx&lt;/Code&gt; Strona została zmieniona](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Rysunek 5**. narzędzie kopiowania witryny sieci Web wskazuje, że strona `Privacy.aspx` została zmieniona ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image15.png))

Narzędzie kopiowania witryny sieci Web wskazuje również, czy plik został usunięty od czasu ostatniej operacji kopiowania. Usuń `Privacy.aspx` z projektu lokalnego i Odśwież narzędzie kopiowania witryny sieci Web. Pliki `Privacy.aspx` i `Privacy.aspx.vb` pozostają wymienione w lewym okienku, ale mają stan usunięte, wskazując, że zostały usunięte od czasu ostatniej operacji kopiowania.

## <a name="publishing-a-web-application"></a>Publikowanie aplikacji sieci Web

Innym sposobem wdrożenia aplikacji sieci Web w programie Visual Studio jest użycie opcji Publikuj, która jest dostępna za pośrednictwem menu Kompilacja. Opcja Publikuj jawnie kompiluje aplikację, a następnie kopiuje wszystkie pliki niezbędne do określonej lokacji zdalnej. Tak jak wkrótce zobaczysz opcję publikacji Blunt niż narzędzie do kopiowania witryn sieci Web. Narzędzie kopiowania witryny sieci Web umożliwia badanie plików w lokacjach lokalnych i zdalnych, a także pozwala na przekazanie lub pobranie poszczególnych plików w razie potrzeby. opcja Publikuj wdraża całą aplikację sieci Web.

Oprócz kopiowania wszystkich wymaganych plików do określonej lokacji zdalnej, opcja Publikuj również jawnie kompiluje aplikację. Uwzględniając, że projekty aplikacji sieci Web muszą być jawnie skompilowane, powinny być niezależne od tego, że opcja Publikuj jest dostępna dla projektów aplikacji sieci Web. Może to być bit zaskakujące, że opcja publikowania jest również dostępna dla projektów witryny sieci Web. Zgodnie z opisem w temacie [*określanie plików wymagających wdrożenia*](determining-what-files-need-to-be-deployed-vb.md) samouczka, projekty witryn sieci Web mogą być jawnie kompilowane przez proces nazywany *prekompilowaniem*. Ten samouczek koncentruje się na używaniu opcji Publikuj z projektami aplikacji sieci Web; w przyszłości zapoznaj się z kompilacją wstępną, a następnie powrócimy do skorzystania z opcji Publikuj z projektami witryny sieci Web.

> [!NOTE]
> Gdy opcja Publikuj jest dostępna w programie Visual Studio dla projektów witryny sieci Web i projektów aplikacji sieci Web, Visual Web Developer oferuje tylko opcję Publikuj dla projektów aplikacji sieci Web.

Przyjrzyjmy się wdrażaniu aplikacji do przeglądu książek przy użyciu opcji Publikuj. Zacznij od otworzenia BookReviewsWAP (projekt aplikacji sieci Web) w programie Visual Studio. Z menu Publikuj wybierz projekt Build BookReviewsWAP. Spowoduje to wyświetlenie okna dialogowego wyświetlającego informacje o lokalizacji docelowej, między innymi opcjami konfiguracji (zobacz rysunek 6). Podobnie jak w przypadku narzędzia kopiowania witryny sieci Web można wprowadzić lokalizację wskazującą folder lokalny, lokalną witrynę sieci Web w usługach IIS, zdalną witrynę sieci Web obsługującą rozszerzenia FrontPage Server Extensions lub adres serwera FTP. Możesz zdecydować, czy pliki na zdalnym serwerze sieci Web mają zostać zastąpione przez wdrożone pliki, czy usunąć całą zawartość w zdalnej witrynie przed opublikowaniem. Możesz również określić, czy kopiować:

- Tylko pliki w projekcie potrzebne do uruchomienia aplikacji, co pomija niepotrzebny kod źródłowy i pliki związane z projektem.
- Wszystkie pliki projektu, w tym pliki kodu źródłowego i pliki projektu programu Visual Studio, takie jak plik rozwiązania.
- Wszystkie pliki w źródłowym folderze projektu, które kopiuje wszystkie pliki w źródłowym folderze projektu, niezależnie od tego, czy są one uwzględnione w projekcie.

Istnieje również możliwość przekazania zawartości folderu `App_Data`.

[![określić docelową witrynę sieci Web](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Ilustracja 6**. Określanie docelowej witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image18.png))

W przypadku aplikacji do przeglądu książki lokacja zdalna zawiera pliki wdrożone podczas kopiowania projektu BookReviewsWSP za pośrednictwem narzędzia kopiowania witryny sieci Web. W związku z tym teraz będzie można rozpocząć publikowanie, usuwając całą istniejącą zawartość. Należy również skopiować wymagane pliki zamiast zasłaniać środowisko produkcyjne z niepotrzebnym kodem źródłowym i plikami projektu. Po określeniu tych opcji kliknij przycisk Publikuj. W ciągu następnych kilku sekund program Visual Studio wdroży niezbędne pliki w lokacji docelowej, wyświetlając postęp w oknie danych wyjściowych.

Rysunek 7 przedstawia pliki w witrynie FTP po zakończeniu operacji publikowania. Należy pamiętać, że przekazano tylko strony znaczników i wymagane pliki obsługi klienta i serwera.

[![tylko te pliki, które są konieczne, zostały opublikowane w środowisku produkcyjnym](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Rysunek 7**. opublikowanie tylko wymaganych plików w środowisku produkcyjnym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-visual-studio-vb/_static/image21.png))

Opcja Publikuj jest mniej złożonychm narzędziem niż narzędzie kopiowania witryny sieci Web. Narzędzie kopiowania witryny sieci Web umożliwia inspekcję plików w lokacjach lokalnych i zdalnych oraz wyświetlanie ich różnic, ale opcja Publikuj nie zapewnia takiego interfejsu. Ponadto narzędzie do kopiowania witryn sieci Web umożliwia wprowadzanie jednostronnych zmian, przekazywanie lub usuwanie pojedynczych plików. Opcja Publikuj nie zezwala na taką szczegółową kontrolę; Zamiast tego publikuje *całą* aplikację. Takie zachowanie ma swoje zalety i wady. Po stronie znaku plus wiadomo, że w przypadku korzystania z opcji Publikuj nie można przekazać ważnego pliku. Należy jednak wziąć pod uwagę to, co się stanie w przypadku niewielkiej zmiany w bardzo dużej witrynie sieci Web — przy użyciu opcji Publikuj nie można zaktualizować tej strony lub dwóch modyfikacji, ale należy zaczekać, aż program Visual Studio wdroży całą lokację.

Niektóre pliki, których zawartość różnią się między środowiskami produkcyjnymi i programistycznymi, nie są rzadko spotykane. Przykładem klucza jest plik konfiguracyjny aplikacji `Web.config`. Ponieważ opcja Publikuj nie kopiuje plików aplikacji sieci Web, zastępuje dostosowane pliki konfiguracji środowiska produkcyjnego do wersji w środowisku programistycznym. Kolejny samouczek omawia ten temat w dalszej części i oferuje porady dotyczące wdrażania aplikacji sieci Web, gdy takie różnice istnieją.

## <a name="summary"></a>Podsumowanie

Wdrożenie witryny sieci Web polega na skopiowaniu niezbędnych plików ze środowiska programistycznego do środowiska produkcyjnego. W poprzednim samouczku pokazano, jak transferować pliki przy użyciu klienta FTP, takiego jak FileZilla. Ten samouczek zbadał dwa narzędzia wdrażania w programie Visual Studio: narzędzie kopiowania witryny sieci Web i opcja Publikuj. Narzędzie kopiowania witryny sieci Web przypomina klienta FTP, który ma dwuokienkowy interfejs listy plików na komputerze lokalnym i określony komputer zdalny, który ułatwia przekazywanie lub pobieranie plików między tymi dwoma komputerami. Opcja Publikuj jest bardziej bluntm narzędziem, które jawnie kompiluje projekt, a następnie wdraża całą aplikację do określonego miejsca docelowego.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Kopiowanie witryny sieci Web za pomocą narzędzia kopiowania witryny sieci Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Jak: wdrażanie witryny sieci Web za pomocą narzędzia kopiowania witryny sieci Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (wideo)
- [Instrukcje: publikowanie projektów aplikacji sieci Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Instrukcje: publikowanie witryn sieci Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Projekty instalacji i wdrażania w programie Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-your-site-using-an-ftp-client-vb.md)
> [dalej](common-configuration-differences-between-development-and-production-vb.md)

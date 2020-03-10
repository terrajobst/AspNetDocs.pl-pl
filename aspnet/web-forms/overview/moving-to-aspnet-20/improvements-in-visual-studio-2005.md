---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Ulepszenia w programie Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Program Visual Studio 2005 udostępnia deweloperom aplikacji sieci Web dużą listę ulepszeń i ulepszeń projektów sieci Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 64215d556ded0850537a13856fe69b094116ebca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575580"
---
# <a name="improvements-in-visual-studio-2005"></a>Ulepszenia w programie Visual Studio 2005

przez [firmę Microsoft](https://github.com/microsoft)

> Program Visual Studio 2005 udostępnia deweloperom aplikacji sieci Web dużą listę ulepszeń i ulepszeń projektów sieci Web.

Program Visual Studio 2005 udostępnia deweloperom aplikacji sieci Web dużą listę ulepszeń i ulepszeń projektów sieci Web. Tak jak w przypadku programu Visual Studio .NET 2002 i 2003, istniały wiele skarg w sposobie obsługi projektów sieci Web. Program Visual Studio 2005 dodaje znaczną liczbę nowych funkcji w celu rozwiązania tych skarg. Dla tych, którzy preferują sposób, w jaki program Visual Studio .NET 2003 obsłużył Kompilowanie aplikacji sieci Web, zobacz [projekty aplikacji sieci Web](https://go.microsoft.com/fwlink/?LinkId=57870).

W tym module dobrze omówiono ulepszenia tworzenia projektu sieci Web, zarządzania nim i programowania. W późniejszym module dobrze omówiono ulepszenia tworzenia projektów sieci Web i ich wdrażania.

## <a name="frontpage-server-extensions"></a>rozszerzenia FrontPage Server Extensions

Aby można było tworzyć lub kompilować projekty sieci Web, wymagane są rozszerzenia FrontPage Server Extensions programu Visual Studio .NET 2002 i 2003. Deweloperzy mieli wybór między dwoma różnymi trybami dostępu (rozszerzenia FrontPage Server Extensions lub tryb dostępu do pliku), zarówno używany rozszerzenia FrontPage Server Extensions do wykonywania zadań, takich jak Ustawianie katalogu głównego aplikacji w usługach IIS itd.

Program Visual Studio 2005 usuwa zależność od rozszerzenia FrontPage Server Extensions dla projektów lokalnych. Program Visual Studio 2005 teraz uzyskuje dostęp do metabazy usług IIS bezpośrednio, zamiast używać rozszerzenia FrontPage Server Extensions. Program Visual Studio 2005 dodaje również obsługę protokołu FTP, który umożliwia zdalny dostęp do projektu bez konieczności rozszerzenia FrontPage Server Extensions.

Dla tych deweloperów, którzy chcą korzystać z rozszerzenia FrontPage Server Extensions w swoich projektach, opcja jest nadal dostępna. Jednak na podstawie silnych opinii społeczności deweloperów ASP.NET nie jest to wymagane.

> [!NOTE]
> Rozszerzenia FrontPage Server Extensions nadal są wymagane do zdalnego tworzenia projektu, otwierania itp.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Program Visual Studio 2005 jest dostarczany z nowym serwerem sieci Web o nazwie ASP.NET Development Server. (Ten serwer sieci Web był wcześniej znany jako Cassini).

Serwer ASP.NET Development ma kilka zalet.

- Jest teraz możliwe, aby nie tylko Administratorzy mogli opracowywać i debugować na serwerze sieci Web.
- Serwer programistyczny ASP.NET dynamicznie mapuje katalogi wirtualne do dowolnej lokalizacji w systemie plików, co umożliwia elastyczne lokalizacje projektów.
- Użytkownicy w systemie Windows XP Professional, którzy już używają usług IIS, będą teraz mogli tworzyć nowe aplikacje sieci Web, które nie wpłyną na strukturę plików ani folderów ich domyślnej witryny sieci Web w usługach IIS.

W celu skorzystania z programu ASP.NET Development Server nie jest wymagana żadna specjalna konfiguracja. Gdy projekt sieci Web, który jest hostowany w systemie plików jest debugowany lub przeglądany, program Visual Studio 2005 automatycznie uruchomi wystąpienie serwera ASP.NET Development na losowo wybranym porcie do obsługi żądania.

Więcej informacji zostanie pokrytych na serwerze deweloperskim ASP.NET w dalszej części tego modułu.

## <a name="improved-file-management"></a>Ulepszone zarządzanie plikami

W programie Visual Studio 2002 i 2003, plik projektu (. vbproj dla VB.NET i. csproj dla C#) przechowuje informacje na temat wszystkich plików w aplikacji sieci Web. Ekran Eksplorator rozwiązań jest oparty na informacjach o pliku w pliku projektu. W związku z tym Eksplorator rozwiązań często wyświetla niedokładne informacje w przypadkach, w których użyto edytorów zewnętrznych. Programy Visual Studio 2002 i 2003 często zastępują zmiany plików lub nie wyświetlają najnowszej wersji plików.

Program Visual Studio 2005 zawiera plik projektu. Zamiast tego odczytuje informacje o pliku i folderze bezpośrednio z dysku, co umożliwia dokładne wyświetlanie plików w projekcie. Ponieważ folder References w programie Visual Studio 2002 i 2003 nie reprezentuje rzeczywistego folderu w aplikacji sieci Web, program Visual Studio 2005 również usuwa folder odwołań z Eksplorator rozwiązań. Aby uzyskać dostęp do odwołań dla projektu w programie Visual Studio 2005, należy użyć stron właściwości dla projektu.

## <a name="creating-web-projects"></a>Tworzenie projektów sieci Web

Deweloperzy sieci Web mogą korzystać z wielu nowych opcji tworzenia projektu w programie Visual Studio 2005. Witryny sieci Web można teraz tworzyć w dowolnym miejscu w systemie plików, a następnie można je debugować lub przeglądać przy użyciu nowego serwera ASP.NET Development. Deweloperzy mogą również tworzyć nowe witryny sieci Web przy użyciu protokołu FTP.

Kliknij tutaj, aby wyświetlić przewodnik wideo dotyczący tworzenia projektów sieci Web w programie Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Otwórz wideo pełnoekranowe](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Projekty systemu plików

Jak widać w instruktażu wideo, można utworzyć witryny sieci Web w systemie plików na komputerze lokalnym lub w lokalizacji zdalnej za pośrednictwem udziału plików. Witryny sieci Web, które są tworzone w systemie plików, są przeglądane i debugowane przy użyciu serwera ASP.NET Development.

> [!NOTE]
> Serwer programistyczny ASP.NET może spowodować pomyłkę dla klientów. Jeśli projekt sieci Web jest tworzony w systemie plików w strukturze katalogów IISs (tj. c:/Inetpub/wwwroot), witryna sieci Web będzie nadal przeszukiwana za pośrednictwem serwera deweloperskiego ASP.NET w przypadku uruchamiania z poziomu programu Visual Studio 2005. W związku z tym nie ma zastosowania żadna konfiguracja usług IIS (tj. metody uwierzytelniania).

Domyślny projekt sieci Web usuwa również wiele nakładów pracy, w tym tylko domyślną stronę aspx, plik default.cs i folder aplikacji/_Data. Plik Web. config i foldery specjalne (tj. app/_code) są dodawane, gdy są one zbędne. Projekt sieci Web zawiera tylko te pliki i foldery, które są potrzebne.

### <a name="http-projects"></a>Projekty HTTP

Projekty HTTP mogą być projektami utworzonymi w lokalnej witrynie sieci Web usług IIS lub w zdalnej witrynie sieci Web. Domyślna lokalizacja projektu to `http://localhost`. Po kliknięciu przycisku Przeglądaj dostępne są dwie opcje HTTP: lokalne usługi IIS i lokacja zdalna. Główną różnicą między tymi dwiema opcjami jest metoda, w której informacje o witrynie sieci Web są wyświetlane w oknie dialogowym Wybieranie lokalizacji oraz w jaki sposób pliki są kopiowane na serwer sieci Web.

Opcja lokalnych usług IIS odczytuje informacje o lokacji z metabazy na komputerze lokalnym, a pliki są kopiowane przy użyciu systemu plików. Opcja lokacja zdalna używa rozszerzenia FrontPage Server Extensions, a informacje o lokacji i pliki są kopiowane przy użyciu wywołań RPC i rozszerzenia FrontPage Server Extensions.

> [!NOTE]
> Plik vs # # #/_tmp. htm i Get/_aspx/_ver. aspx nie są już używane do określania informacji o wersji.

Domyślną opcją HTTP jest lokalna usługa IIS. Ta opcja odczytuje metabazę usług IIS, aby określić, które lokacje są dostępne, oraz lokalizację, w której ma zostać utworzona zawartość. Możesz wybrać inny folder lub katalog wirtualny, wybierając go w widoku drzewa. Możesz również utworzyć nowy katalog wirtualny, oznaczyć foldery jako aplikacje, a także usunąć istniejące katalogi wirtualne z tego okna dialogowego.

![Okno dialogowe Wybieranie lokalizacji](improvements-in-visual-studio-2005/_static/image1.gif)

**Rysunek 1**. okno dialogowe Wybieranie lokalizacji

W przeciwieństwie do starszych wersji programu Visual Studio, jeśli jest zaznaczone pole wyboru **użyj SSL** i certyfikat SSL nie jest zgodny z PRZEGLĄDANYM adresem URL, zostanie wyświetlone okno dialogowe alertu zabezpieczeń z pytaniem, czy chcesz kontynuować. Jeśli certyfikat nie był zgodny z programem Visual Studio .NET 2003, tworzenie projektu nie powiedzie się.

![Alert zabezpieczeń dotyczący certyfikatu SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Rysunek 2**. alert zabezpieczeń dotyczący certyfikatu SSL

### <a name="note-on-host-headers"></a>Uwaga dotycząca nagłówków hosta

Jeśli tworzysz aplikację sieci Web w lokacji powiązanej z określonym adresem IP, musisz się upewnić, że jest skonfigurowany nagłówek hosta. W przeciwnym razie program Visual Studio utworzy witrynę pod adresem `http://localhost`, ale adres IP nie zostanie prawidłowo rozpoznany, gdy lokacja zostanie przeszukana lub debugowana z poziomu środowiska IDE.

Jeśli wybierzesz opcję lokacja zdalna, okno dialogowe zmieni się, aby umożliwić wprowadzanie docelowego adresu URL dla nowej witryny sieci Web. Ten adres URL musi znajdować się na serwerze, na którym jest włączony rozszerzenia FrontPage Server Extensions. Jeśli chcesz współpracować z lokalnym serwerem sieci Web przy użyciu rozszerzenia FrontPage Server Extensions, możesz użyć opcji lokacja zdalna i określić lokalny adres URL.

![Tworzenie witryny sieci Web na serwerze zdalnym](improvements-in-visual-studio-2005/_static/image1.jpg)

**Rysunek 3**. Tworzenie witryny sieci Web na serwerze zdalnym

W przypadku tworzenia aplikacji w zdalnej lokacji za pośrednictwem protokołu SSL, jeśli certyfikat SSL nie jest zgodny, okno dialogowe potwierdzenia jest nieco inne niż okno dialogowe wyświetlane podczas korzystania z opcji lokalnych usług IIS.

![Alert zabezpieczeń lokacji zdalnej](improvements-in-visual-studio-2005/_static/image3.gif)

**Rysunek 4**. alert zabezpieczeń lokacji zdalnej

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

W programie Visual Studio 2005 wprowadzono opcję tworzenia witryn sieci Web za pośrednictwem protokołu FTP. W przypadku użycia tej opcji IDE tworzy pliki lokalnie w folderze Temp użytkowników, a następnie używa protokołu FTP do przenoszenia plików do lokalizacji FTP.

> [!NOTE]
> Lokalizacja folderu temp to c:/Documents and Settings/&lt;User&gt;local settings/temp/VWDWebCache/&lt;Server&gt;/_&lt;nazwa aplikacji&gt;

W przypadku korzystania z opcji FTP zostanie wyświetlone okno dialogowe Wybieranie lokalizacji. W tym oknie dialogowym wprowadź wymagane informacje dotyczące połączenia FTP, jak pokazano poniżej.

![Okno dialogowe Wybieranie lokalizacji dla protokołu FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Rysunek 5**. okno dialogowe Wybieranie lokalizacji dla usługi FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Lab: Konfigurowanie witryny FTP i tworzenie projektu

Poniższe kroki umożliwiają skonfigurowanie witryny FTP w taki sposób, aby użytkownik miał lokalizację, do której można przekazać tylko za pośrednictwem protokołu FTP.

### <a name="install-the-ftp-service"></a>Instalowanie usługi FTP

1. Otwórz aplet Dodaj Usuń programy, wybierz pozycję Dodaj/Usuń składniki systemu Windows.
2. Wybierz pozycję Internet Information Services (serwer aplikacji w systemie Windows 2003), a następnie kliknij pozycję **szczegóły**.
3. Sprawdź **usługę protokół transferu plików (FTP)** , a następnie kliknij przycisk **OK**.
4. Kliknij przycisk **dalej** , aby zainstalować usługę FTP.

### <a name="create-a-new-folder-for-content"></a>Utwórz nowy folder dla zawartości

1. W Eksploratorze Windows utwórz nowy folder o nazwie **Użytkownik1** w języku c:/Inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Skonfiguruj foldery i uprawnienia w folderach.

1. Otwórz przystawkę Internet Information Services z narzędzi administracyjnych. W węźle nazwa komputera będzie teraz dostępny folder witryny FTP.
2. Rozwiń węzeł **witryny FTP**.
3. Kliknij prawym przyciskiem myszy **domyślną witrynę FTP**, wybierz pozycję **Nowy**, a następnie **katalog wirtualny**, a następnie kliknij przycisk **dalej**.
4. W polu Nazwa katalogu wirtualnego wpisz **Użytkownik1** , a następnie kliknij przycisk **dalej**.
5. Wprowadź ścieżkę **c:/Inetpub/wwwroot/Użytkownik1** , a następnie kliknij przycisk **dalej**.
6. Kliknij przycisk **dalej** , a następnie **Zakończ** , aby zakończyć pracę kreatora.
7. Kliknij prawym przyciskiem myszy katalog wirtualny **Użytkownik1** w obszarze Domyślna witryna FTP i wybierz pozycję **Właściwości**.
8. Zaznacz pole wyboru **Zapisz** , a następnie kliknij przycisk **OK** , aby zamknąć okno dialogowe.
9. Kliknij prawym przyciskiem myszy pozycję **Domyślna witryna FTP** i wybierz pozycję **Właściwości**.
10. Na karcie **konta zabezpieczeń** Usuń zaznaczenie pola wyboru **Zezwalaj na połączenia anonimowe**.
11. Kliknij przycisk **tak** w oknie dialogowym z pytaniem, czy chcesz kontynuować.
12. Kliknij przycisk **OK** , aby zamknąć okno dialogowe.
13. Rozwiń **domyślną witrynę sieci Web** w węźle **witryny sieci Web** .
14. Kliknij prawym przyciskiem myszy katalog **Użytkownik1** i wybierz polecenie **Właściwości**
15. W sekcji **Ustawienia aplikacji** kliknij przycisk **Utwórz** , aby oznaczyć folder jako aplikację.
16. Kliknij przycisk **OK** , aby zamknąć okno dialogowe.
17. Zamknij przystawkę Internet Information Services.

### <a name="create-web-project"></a>Utwórz projekt sieci Web

1. Otwórz program Visual Studio 2005.
2. Z menu **plik** wybierz pozycję **Nowa witryna sieci Web**.
3. Na liście rozwijanej **Lokalizacja** wybierz pozycję **FTP**.
4. Kliknij przycisk **Przeglądaj**.
5. W polu tekstowym **serwer** wprowadź wartość **localhost** .
6. W polu tekstowym katalog wprowadź wartość **Użytkownik1** .
7. Kliknij przycisk **Otwórz**. Lokalizacja FTP zostanie wprowadzona do okna dialogowego Nowa witryna sieci Web.
8. Kliknij przycisk **OK**.
9. Usuń zaznaczenie opcji **Logowanie anonimowe** w oknie dialogowym logowania FTP, wprowadź poświadczenia i kliknij przycisk **OK**.
10. Jaki jest adres URL projektu? (Adres URL projektu zostanie wyświetlony w Eksplorator rozwiązań).
11. Z menu **kompilacja** wybierz opcję **Kompiluj witrynę sieci Web** lub **Kompiluj rozwiązanie**.
12. Kliknij prawym przyciskiem myszy wartość default. aspx w Eksplorator rozwiązań i wybierz pozycję **Widok w przeglądarce**.
13. W oknie dialogowym wymagana witryna sieci Web wprowadź adres URL `http://localhost/user1` i kliknij przycisk **OK**.

> [!NOTE]
> Jeśli wystąpi błąd wskazujący, że nie można załadować typu/_Default, upewnij się, że korzystasz z ASP.NET 2,0 w witrynie sieci Web, a nie w starszej wersji. Można to zrobić na karcie ASP.NET w Internet Information Services.

## <a name="opening-web-projects"></a>Otwieranie projektów sieci Web

Otwieranie projektów sieci Web jest podobne do tworzenia projektów. Poniższe sekcje odwołują się do obszarów, aby zachować efekt w trakcie pracy w środowisku IDE. Obejmuje on również pracę z projektami sieci Web przy użyciu protokołów HTTP i FTP.

Aby otworzyć projekt sieci Web, wybierz pozycję Otwórz witrynę sieci Web z menu plik. Zostanie wyświetlony monit o podanie tego samego okna dialogowego Wybieranie lokalizacji, które pozostało wcześniej i że dostępne są te same cztery opcje: system plików, lokalne usługi IIS, FTP i lokacja zdalna.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>System plików

Jak wskazano wcześniej w tym module, program Visual Studio nie używa już pliku projektu. W związku z tym, jeśli zdecydujesz się otworzyć witrynę sieci Web z systemu plików, możesz wybrać dowolny z folderów, nawet jeśli wybrany folder nie został początkowo utworzony jako projekt sieci Web w programie Visual Studio. Na przykład możesz otworzyć folder Moje dokumenty jako witrynę sieci Web, a program Visual Studio będzie Happily go otworzyć i wyświetlić pliki, jak pokazano poniżej.

![Moje dokumenty otwarte jako witryna sieci Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Ilustracja 6**. *Moje dokumenty* otwarte jako witryna sieci Web

Ponieważ program Visual Studio tworzy tylko w razie potrzeby dodatkowe pliki i foldery, do otwartej lokalizacji nie są dodawane żadne dodatkowe pliki ani foldery. Efektem ubocznym tej architektury jest uniemożliwianie zagnieżdżania witryn sieci Web w systemie plików. Rozważmy na przykład następującą strukturę katalogów.

Projekt sieci Web w witrynie C:/Website

Inny projekt sieci Web w C:/webwebsite/zagnieżdżony

Po otwarciu witryny sieci Web w witrynie c:/Website folder zagnieżdżony będzie wyświetlany jako podfolder tej aplikacji.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Podczas otwierania witryn sieci Web za pośrednictwem protokołu HTTP ustawienia są odczytywane z metabazy usług IIS (lokalnych usług IIS) lub przy użyciu rozszerzenia FrontPage Server Extensions (lokacja zdalna). Jeśli istnieją zagnieżdżone aplikacje sieci Web, są one wyświetlane z ikoną identyfikującą je jako aplikację. Jeśli wiesz już, jak pracować z aplikacjami sieci Web w programie FrontPage, zachowanie w programie Visual Studio 2005 jest podobne.

Mimo że program Visual Studio wyświetli ikonę dla aplikacji, które są zagnieżdżone pod aplikacją, która jest aktualnie otwarta w środowisku IDE, nie pozwoli na ich rozszerzenie, aby zobaczyć ich zawartość. Można jednak kliknąć je dwukrotnie, aby je otworzyć. Po wykonaniu tych czynności zostanie wyświetlone okno dialogowe z monitem o otwarcie aplikacji sieci Web (i zastąpienie aktualnie otwartego rozwiązania) lub dodanie aplikacji sieci Web do bieżącego rozwiązania.

![Dwukrotne kliknięcie ikony aplikacji zagnieżdżonej przedstawia to okno dialogowe](improvements-in-visual-studio-2005/_static/image4.jpg)

**Rysunek 7**. dwukrotne kliknięcie ikony zagnieżdżonej aplikacji spowoduje wyświetlenie tego okna dialogowego

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Witryna FTP

Po otwarciu witryny za pośrednictwem protokołu FTP wszystkie pliki są kopiowane lokalnie do folderu tymczasowego. Pełna ścieżka do lokalnej lokalizacji magazynu zostanie wyświetlona w okienku właściwości projektu i utworzona przy użyciu następującego formatu.

C:/Documents and Settings/&lt;User&gt;local settings/temp/VWDWebCache/&lt;Server&gt;/_&lt;nazwa aplikacji&gt;

W przypadku korzystania z protokołu FTP program Visual Studio będzie musiał określić podstawowy adres URL projektu, aby można było go przeglądać jak pokazano poniżej. Jeśli nie określisz podstawowego adresu URL, program Visual Studio wyświetli monit o podanie go przy pierwszej próbie przeglądania strony w witrynie sieci Web.

![Określanie podstawowego adresu URL dla witryn FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Ilustracja 8**. Określanie podstawowego adresu URL dla witryn FTP

## <a name="improvements-in-compilation"></a>Ulepszenia kompilacji

Praca z aplikacjami sieci Web w programie Visual Studio 2005 jest znacznie szybsza niż w poprzednich wersjach. Jest to spowodowane brakiem niewielkiej części zmian w architekturze kompilacji.

W programie Visual Studio 2002 i 2003 aplikacje sieci Web zostały skompilowane do jednego zestawu podstawowego znajdującego się w folderze/bin. W programie Visual Studio 2005 dodano folder aplikacji/_Code. Klasy i inny kod inny niż interfejs użytkownika są dodawane do folderu App/_Code. Gdy program Visual Studio kompiluje projekt, wszystkie pliki w folderze aplikacji/_Code są kompilowane w jednym pliku aplikacji/_Code. dll. Wynik tej zmiany polega na tym, że kolejne kompilacje są znacznie szybsze niż w poprzednich wersjach.

> [!NOTE]
> Narzędzia wiersza polecenia programu MSBuild można także użyć do kompilowania aplikacji sieci Web ASP.NET. To narzędzie zostanie omówione w module 9.

Innym ulepszeniem kompilacji jest opcja nowej strony kompilacji w menu Kompilacja. Ta funkcja umożliwia deweloperowi odbudowa tylko bieżącej strony (wraz z kursami i zależnościami), aby umożliwić szybsze Kompilowanie zmian. Ponieważ C# nie oferuje kompilacji w tle do celów aktualizowania technologii IntelliSense itp., korzyści te bardzo z tej funkcji, ponieważ umożliwi to szybkie aktualizowanie technologii IntelliSense przez po prostu przebudowywanie pojedynczej strony.

Właściwości kompilacji dla projektu umożliwiają skonfigurowanie typu kompilacji, która występuje przed wykonaniem strony startowej. Deweloperzy mogą zdecydować się na kompilację bieżącej strony, aby program Visual Studio mógł szybciej rozpocząć debugowanie aplikacji po zmianie kodu.

![Akcja rozpoczęcia strony kompilacji](improvements-in-visual-studio-2005/_static/image6.jpg)

**Ilustracja 9**. akcja rozpoczęcia strony kompilacji

Innym doskonałym ulepszeniem programu Visual Studio i architekturą ASP.NET jest w obszarze Edytuj i Kontynuuj. W programie Visual Studio 2005 deweloperzy mogą rozpocząć debugowanie projektu i wprowadzać zmiany kodu w projekcie bez odłączania debugera. W rzeczywistości można dosłownie rozpocząć debugowanie projektu, dodać nową klasę, dodać kod do tej klasy, dodać kod do strony, który tworzy nowe wystąpienie tej klasy i wykonać metodę klasy, bez odłączania debugera. Wykonywanie nowego kodu jest bardzo proste, jak w przypadku odświeżania przeglądarki!

Kliknij tutaj, aby zobaczyć przewodnik wideo dotyczący funkcji Edytuj i Kontynuuj w programie Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Otwórz wideo pełnoekranowe](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

Niezawodna funkcja Edytuj i Kontynuuj w ASP.NET 2,0 i Visual Studio 2005 jest spowodowana zmianą architektury dla aplikacji ASP.NET. W ASP.NET 1. x aplikacje utworzone w programie Visual Studio 2002/2003 zostały skompilowane do zestawu podstawowego, który został zapisany w folderze/bin. Wszystkie klasy, strony itp. dla aplikacji zostały skompilowane do tej jednej biblioteki DLL. Następnie w czasie wykonywania ASP.NET kompiluje wszystkie kontrolki, znaczniki i kod ASP.NET na stronach i skopiują te biblioteki DLL do folderu tymczasowego ASP.NET.

W programie Visual Studio 2005 przy użyciu programu ASP.NET 2,0 dwa konspekty dla modeli kompilacji (jeden dla programu Visual Studio i jeden dla ASP.NET w środowisku uruchomieniowym) zostały scalone w jeden wspólny model kompilacji. Oznacza to, że wszystkie problemy z kompilacją są teraz przechwytywane na etapie opracowywania, a nie w czasie wykonywania. Umożliwia także projektantom i obsługę technologii IntelliSense dla funkcji, takich jak kontrolki użytkownika i strony główne.

Kliknij tutaj, aby wyświetlić przewodnik wideo dotyczący obsługi projektanta dla kontrolek użytkownika.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Otwórz wideo pełnoekranowe](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Gdy kontrolka użytkownika zostanie usunięta ze strony, dyrektywa @Register pozostaje w znaczniku i powinna zostać usunięta ręcznie, aby uniknąć błędów parsera, Jeśli kontrolka użytkownika zostanie usunięta z witryny sieci Web.

Innym ulepszeniem modelu kompilacji programu Visual Studio jest funkcja publikowania witryny sieci Web. Ponieważ funkcja publikowania kompiluje wstępnie witrynę sieci Web, deweloperzy mogą korzystać z dodanej wydajności niezwiązanej z kompilowaniem wszystkiego na żądanie. Ponadto wstępnie kompiluje wszystkie kod źródłowy w folderze App/_Code do biblioteki DLL, aby nie trzeba było wdrażać kodu źródłowego.

![Okno dialogowe publikowania witryny sieci Web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Ilustracja 10**. okno dialogowe publikowanie witryny sieci Web

> [!NOTE]
> Narzędzia ASPNET/_compile. exe można również użyć do wstępnego skompilowania aplikacji sieci Web ASP.NET. To narzędzie zostanie omówione w module 9.

Po opublikowaniu witryny sieci Web wstępnie skompilowane pliki są przechowywane w folderze Temporary ASP.NET Files, jak pokazano poniżej. Pliki z rozszerzeniem *skompilowanego* pliku są plikami XML, które definiują zależności dla określonych bibliotek DLL. Każdy formant WebForm lub użytkownik jest kompilowany do losowych bibliotek DLL, które zaczynają się od *aplikacji/_sieci Web/_* .

Jeśli pole wyboru *Zezwalaj na dostęp do tej wstępnie skompilowanej witryny* będzie zaznaczone, Adiustacja wewnątrz formularzy WebForms i kontrolek użytkownika nie zostaną wstępnie skompilowane do biblioteki DLL, co umożliwi wprowadzanie zmian po wdrożeniu. Jeśli wolisz zablokować adiustację, tak aby zmiany wdrożonej zawartości nie były dozwolone, usuń zaznaczenie tego pola.

Pole wyboru *Zastosuj stałe nazewnictwo i zestawy pojedynczej strony* pozwala wyłączyć kompilację wsadową, tak aby każda Strona była skompilowana do zestawu o stałym rozmiarze. Pozostawienie tego pola niezaznaczone umożliwia korzystanie z kompilacji wsadowej.

Pole wyboru *Włącz silne nazewnictwo dla wstępnie skompilowanych zestawów* pozwala na silną nazwę wstępnie skompilowanych zestawów.

> [!NOTE]
> W ASP.NET 1. x zestawy o silnej nazwie musiały zostać zainstalowane w globalnej pamięci podręcznej zestawów (GAC). W programie ASP.NET 2,0 nie trzeba instalować zestawów o silnych nazwach w pamięci podręcznej GAC.

![Wstępnie skompilowane pliki aplikacji ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Rysunek 11**: aplikacje ASP.NET wstępnie skompilowane pliki

> [!NOTE]
> W powyższych aplikacjach nie ma pliku Web. config. W takim przypadku plik *PrecompiledApp. config* został wywołany po zakończeniu procesu publikowania witryny sieci Web.

## <a name="improvements-in-deployment"></a>Ulepszenia wdrożenia

Podobnie jak w przypadku programów Visual Studio 2002 i 2003, Visual Studio 2005 oferuje funkcję kopiowania projektu. Jednak funkcja została zawołowina w programie Visual Studio 2005 i jest teraz nazywana kopią witryny sieci Web.

Okno dialogowe kopiowanie witryny sieci Web jest podzielone na lewą ramkę i prawą ramkę. Lewa ramka jest nazywana źródłową witryną sieci Web, a odpowiednia ramka jest nazywana zdalną witryną sieci Web. Jedną z elementów, które mogą mylić niektórych deweloperów, jest to, że witryna wyświetlana w prawej ramce nie musi być zdalną lokacją. Może to być witryna w lokalnym systemie plików lub w lokalnym wystąpieniu usług IIS. Ponadto lokacja wyświetlana w lewej ramce nie musi być źródłową witryną sieci Web, ponieważ okno dialogowe pozwala na publikowanie ze zdalnej witryny sieci Web *do* źródłowej witryny sieci Web.

Jeśli kopiujesz projekt do zdalnej witryny sieci Web, ta lokacja musi mieć zainstalowaną rozszerzenia FrontPage Server Extensions. Jeśli tak nie jest, należy nawiązać połączenie przy użyciu protokołu FTP. Z drugiej strony, jeśli kopiujesz projekt do lokalnego wystąpienia usług IIS, rozszerzenia FrontPage Server Extensions nie są wymagane.

> [!NOTE]
> Jeśli spróbujesz utworzyć nową witrynę sieci Web w lokalnym wystąpieniu usług IIS, a rozszerzenia serwera programu FrontPage 2002 są zainstalowane, zostanie wyświetlony komunikat o błędzie z informacją, że tworzenie witryn sieci Web nie jest obsługiwane na serwerze programu SharePoint. W takim przypadku można zainstalować rozszerzenia serwera programu FrontPage 2000 lub usunąć rozszerzenia FrontPage Server Extensions.

Kliknij tutaj, aby zapoznać się z przewodnikiem wideo dotyczącym funkcji kopiowania witryny sieci Web.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Otwórz wideo pełnoekranowe](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Ulepszenia debugowania

W programie Visual Studio 2005 istnieją cztery kluczowe Ulepszenia debugowania.

- Debugowanie lokalne jako niebędące administratorem jest możliwe z poziomu pola.
- Atrybut Debug dla elementu compilation jest teraz domyślnie fałszywy.
- Instalacja i Konfiguracja debugowania zdalnego jest łatwiejsza niż wcześniej.
- Teraz można debugować witrynę sieci Web otwartą za pośrednictwem lokalizacji FTP.

## <a name="debugging-as-a-non-administrator"></a>Debugowanie jako nie będącej administratorem

Dodanie serwera deweloperskiego ASP.NET pozwala innym firmom na łatwe debugowanie aplikacji ASP.NET od razu. Gdy aplikacja ASP.NET uruchomiona w lokalnym systemie plików jest debugowana, program Visual Studio uruchamia serwer deweloperski ASP.NET w kontekście zalogowanego użytkownika. Następnie użytkownik może debugować tę aplikację bez żadnej dodatkowej konfiguracji.

## <a name="debug-is-false-by-default"></a>Debugowanie domyślnie ma wartość false

W ASP.NET 1. x atrybut *Debug* w elemencie *compilation* w pliku Web. config został domyślnie ustawiony na *wartość true* . Zawsze zaleca się, aby deweloperzy ustawili ten atrybut na *Fałsz* przed wdrożeniem aplikacji w środowisku produkcyjnym, ale ponieważ większość deweloperów nie zrozumie w pełni skutków pozostawienia atrybutu Debug ustawionego na wartość true.

Największym problemem związanym z atrybutem Debug ustawionym na true jest wyłączenie z modelu kompilacji wsadowej ASP. sieci. W związku z tym każda strona jest kompilowana w oddzielnym pliku DLL. Jeśli aplikacja sieci Web składa się z tysięcy stron (nieemitowanych przez żadne środki), oznacza to, że ta aplikacja utworzy kilka tysięcy niewielkich bibliotek DLL. Chociaż te biblioteki dll mają niewielki rozmiar, nie są ładowane do żadnej określonej lokalizacji w pamięci. W związku z tym powodują fragmentację pamięci systemowej i mogą współtworzyć wystąpienia OutOfMemoryException.

W ASP.NET 2,0 atrybut debugowania jest domyślnie ustawiony na FAŁSZ. Jak już widzisz, gdy deweloper debuguje aplikację ASP.NET w programie Visual Studio 2005, zostanie wyświetlony monit o dodanie pliku Web. config z włączonym debugowaniem. W związku z tym te same wady są takie same, jak w ASP.NET 1. x, ale teraz deweloper jest jasno ostrzegany, że atrybutu należy zresetować do wartości false przed przeniesieniem aplikacji do środowiska produkcyjnego.

## <a name="remote-debugging-setup-and-configuration"></a>Instalacja i Konfiguracja debugowania zdalnego

W programie Visual Studio 2002/2003 zdalne debugowanie opiera się na Menedżerze debugowania maszyn (MDM. exe) i procesie vs7jit. exe. Z tego powodu Rozwiązywanie problemów z debugowaniem zdalnym była często czarnym polem dla klientów i często nie jest to znacznie lepsze dla PSS.

Program Visual Studio 2005 usuwa zależność od procesów MDM. exe i vs7jit. exe. Zamiast tego używa teraz usługi zdalnego monitora debugowania (msvsmon. exe).

Wymagania dotyczące debugowania w programie Visual Studio 2005 są bardzo proste. Przed debugowaniem należy uruchomić msvsmon. exe na serwerze zdalnym. Możesz zainstalować zdalny Monitor debugowania z dysku CD programu Visual Studio lub po prostu uruchomić program msvsmon. exe z udziału bez instalowania wszystkich elementów na serwerze sieci Web.

Po uruchomieniu programu msvsmon. exe jest możliwe, że będzie on skargą o porty, które są blokowane na potrzeby debugowania zdalnego. Na szczęście można łatwo odblokować porty bezpośrednio w oknie dialogowym ostrzeżenia, jak pokazano poniżej.

![Powiadomienie, że Zapora systemu Windows blokuje debugowanie zdalne](improvements-in-visual-studio-2005/_static/image9.jpg)

**Ilustracja 12**. powiadomienie, że Zapora systemu Windows blokuje debugowanie zdalne

Po odblokowaniu portów niezbędnych do debugowania zobaczysz Monitor zdalnego debugowania jak pokazano poniżej. W tym interfejsie można monitorować połączenia i łatwo zmieniać uprawnienia debugowania.

![Monitor zdalnego debugowania](improvements-in-visual-studio-2005/_static/image10.jpg)

**Ilustracja 13**. Monitor zdalnego debugowania

Istnieje również możliwość zdalnego debugowania aplikacji sieci Web otwartej za pośrednictwem protokołu FTP. Kroki są takie same jak te, które zostały wcześniej omówione. Należy jednak określić podstawowy adres URL do przeglądania projektu FTP, jak opisano wcześniej w tym module.

## <a name="lab-2"></a>Lab 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Zdalne debugowanie za pomocą programu Visual Studio 2005

To laboratorium przeprowadzi Cię przez zdalne debugowanie w programie Visual Studio 2005.

Kliknij tutaj, aby zapoznać się z przewodnikiem wideo dotyczącym tego laboratorium.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Otwórz wideo pełnoekranowe](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

To laboratorium wymaga posiadania dwóch maszyn, na których działa program Visual Studio 2005 i inne uruchomione usługi IIS w wersji 5 lub nowszej.

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web na serwerze zdalnym.

> [!NOTE]
> Witrynę sieci Web można utworzyć w zdalnym wystąpieniu usług IIS lub za pośrednictwem protokołu FTP.

1. Na zdalnym serwerze sieci Web zlokalizuj msvsmon. exe na komputerze deweloperskim przy użyciu ścieżki UNC i wykonaj tę operację.  
 Domyślną lokalizacją programu msvsmon. exe jest//Server/c $/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Jeśli zostanie wyświetlony monit o Odblokowanie portów do zdalnego debugowania, zrób to.
3. Na maszynie deweloperskiej Otwórz kod w polu Default. aspx i ustaw punkt przerwania w metodzie Page/_Load.
4. Rozpocznij debugowanie z komputera deweloperskiego.

Punkt przerwania powinien zostać osiągnięty zgodnie z oczekiwaniami.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Jak już omawiamy, program Visual Studio 2005 jest dostarczany z serwerem sieci Web o nazwie ASP.NET Development Server. (Serwer programistyczny ASP.NET jest czasami określany jako Cassini). Ten serwer sieci Web jest wygodnym sposobem przeglądania i debugowania aplikacji sieci Web działających w systemie plików.

Serwer ASP.NET Development to serwer sieci Web z ograniczeniami. Nie zezwala na połączenia zdalne, ale nie zezwala na żadne żądania od innego użytkownika niż użytkownik, który uruchomił serwer sieci Web. Nie ma również możliwości obsługi stron ASP. Obsługiwane są tylko zasoby ASP.NET i zasoby HTML (w tym obrazy, pliki CSS itp.).

Serwer ASP.NET Development można uruchomić za pomocą wiersza polecenia, uruchamiając plik WebDev. WebServer. exe znajdujący się w c:/Windows/Microsoft. NET/Framework/v 2.0./ */* / */* /*. Następujące okno dialogowe wyświetla dostępne parametry.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Rysunek 14**

> [!NOTE]
> Serwer programistyczny ASP.NET nie jest obsługiwany, gdy jest uruchamiany jawnie za pomocą wiersza polecenia.

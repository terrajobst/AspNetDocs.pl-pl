---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Konfigurowanie serwera sieci Web na potrzeby publikowania Web Deploy (Agent zdalny) | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób konfigurowania serwera sieci Web Internet Information Services (IIS) do obsługi publikowania w sieci Web i wdrażania przy użyciu wdrożenia sieci Web usług IIS...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: ce0d246afdfb65c2ea15a287064511e7d1d58622
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589048"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Konfigurowanie serwera internetowego dla usługi publikowania Web Deploy (agent zdalny)

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania serwera sieci Web Internet Information Services (IIS) do obsługi publikowania w sieci Web i wdrażania przy użyciu usługi zdalnego agenta usług IIS Web Deployment Tool (Web Deploy).
> 
> Pracując z Web Deploy 2,0 lub nowszym, istnieją trzy główne podejścia, których można użyć do uzyskania aplikacji lub witryn na serwerze sieci Web. Można:
> 
> - Użyj *usługi zdalnego agenta Web Deploy*. Takie podejście wymaga mniejszej konfiguracji serwera sieci Web, ale musisz podać poświadczenia administratora serwera lokalnego w celu wdrożenia wszystkich elementów na serwerze.
> - Użyj *procedury obsługi Web Deploy*. To podejście jest znacznie bardziej złożone i wymaga większego nakładu pracy w celu skonfigurowania serwera sieci Web. Jednak podczas korzystania z tego podejścia można skonfigurować usługi IIS, aby umożliwić użytkownikom niebędącym administratorami wdrażanie. Procedura obsługi Web Deploy jest dostępna tylko w usługach IIS w wersji 7 lub nowszej.
> - Użyj *wdrożenia w trybie offline*. Takie podejście wymaga najmniejszej konfiguracji serwera sieci Web, ale administrator serwera musi ręcznie skopiować pakiet sieci Web na serwer i zaimportować go za pomocą Menedżera usług IIS.
> 
> Aby uzyskać więcej informacji o najważniejszych funkcjach, zaletach i wadach tych metod, zobacz [Wybieranie odpowiedniego podejścia do wdrażania w sieci Web](choosing-the-right-approach-to-web-deployment.md).

## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>Czy Web Deploy zdalny agent ma odpowiednie podejście?

Tak, jeśli użytkownik, który będzie wdrażał zawartość, może podać poświadczenia administratora na serwerze docelowym. Takie podejście jest często pożądane w następujących typach scenariuszy:

- Środowiska deweloperskie lub testowe, w przypadku których deweloper ma pełną kontrolę nad docelowym serwerem sieci Web i serwerem baz danych.
- Mniejsze organizacje, w których pojedynczy użytkownik lub niewielka grupa użytkowników ma kontrolę nad całym cyklem życia aplikacji.

W wielu większych organizacjach i szczególnie w przypadku środowisk przejściowych i produkcyjnych często nie jest to realistyczne, aby nadać użytkownikom uprawnienia administratora na serwerach sieci Web. W przypadku hostowanych serwerów sieci Web jest to szczególnie mało prawdopodobne. Ponadto, jeśli planujesz Automatyzowanie wdrożenia z poziomu serwera kompilacji, możesz nie chcieć używać poświadczeń administratora w procesie wdrażania. W tych scenariuszach skonfigurowanie serwerów sieci Web do obsługi wdrożenia przy użyciu programu [obsługi Web Deploy](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) może zapewnić bardziej zadowalający wybór.

## <a name="task-overview"></a>Przegląd zadania

W tym temacie opisano sposób konfigurowania serwera sieci Web Internet Information Services (IIS) 7,5 do akceptowania i wdrażania pakietów internetowych z komputera zdalnego przy użyciu metody zdalnego agenta Web Deploy. Należy:

- Zainstaluj usługi IIS 7,5 i zalecaną konfigurację usług IIS 7.
- Zainstaluj Web Deploy 2,1 lub nowszy.
- Utwórz witrynę sieci Web usług IIS do hostowania wdrożonej zawartości.
- Upewnij się, że usługa Deployment Agent sieci Web jest uruchomiona.

Aby hostować przykładowe rozwiązanie w specjalny sposób, należy również wykonać następujące:

- Zainstaluj .NET Framework 4,0.
- Zainstaluj ASP.NET MVC 3.

W tym temacie przedstawiono sposób wykonywania każdej z tych procedur. W zadaniach i przewodnikach w tym temacie przyjęto założenie, że rozpoczyna się od czystej kompilacji serwera z systemem Windows Server 2008 R2. Przed kontynuowaniem upewnij się, że:

- System Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są zainstalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.

> [!NOTE]
> Aby uzyskać więcej informacji na temat dołączania komputerów do domeny, zobacz [dołączanie komputerów do domeny i logowanie](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [Konfigurowanie statycznego adresu IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Usługa zdalnego agenta jest obsługiwana przez usługi IIS w wersji 6 lub nowszej i nie wymaga przyłączenia do domeny. Jednak kroki opisane w tym samouczku zostały opracowane i przetestowane w usługach IIS 7,5, a procedury dla innych wersji mogą się różnić.

## <a name="install-products-and-components"></a>Zainstaluj produkty i składniki

Ta sekcja przeprowadzi Cię przez proces instalowania wymaganych produktów i składników na serwerze sieci Web. Przed rozpoczęciem zaleca się uruchomienie Windows Update, aby upewnić się, że serwer jest całkowicie aktualny.

W takim przypadku należy zainstalować następujące elementy:

- **Zalecana konfiguracja usług IIS 7**. Umożliwia to roli **serwer sieci Web (IIS)** na serwerze sieci Web i instaluje zestaw modułów i składników usług IIS, które są potrzebne w celu hostowania aplikacji ASP.NET.
- **.NET Framework 4,0**. Jest to wymagane do uruchamiania aplikacji, które zostały skompilowane w tej wersji .NET Framework.
- **Web Deployment Tool 2,1 lub nowszym**. Spowoduje to zainstalowanie Web Deploy (i jego podstawowego pliku wykonywalnego, MSDeploy. exe) na serwerze. W ramach tego procesu instaluje i uruchamia usługę Deployment Agent sieci Web. Ta usługa umożliwia wdrażanie pakietów internetowych z komputera zdalnego.
- **ASP.NET MVC 3**. Spowoduje to zainstalowanie zestawów potrzebnych do uruchamiania aplikacji MVC 3.

> [!NOTE]
> W tym przewodniku opisano sposób korzystania z Instalatora platformy sieci Web w celu zainstalowania i skonfigurowania wymaganych składników. Mimo że nie jest konieczne używanie Instalatora platformy sieci Web, upraszcza proces instalacji przez automatyczne wykrywanie zależności i zapewnia, że zawsze otrzymujesz najnowsze wersje produktu. Aby uzyskać więcej informacji, zobacz [Instalator platformy Microsoft Web 3,0](https://go.microsoft.com/?linkid=9805118).

**Aby zainstalować wymagane produkty i składniki**

1. Pobierz i zainstaluj [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).
2. Po zakończeniu instalacji Instalator platformy sieci Web zostanie uruchomiony automatycznie.

    > [!NOTE]
    > Teraz można uruchomić Instalatora platformy sieci Web w dowolnym momencie z menu **Start** . W tym celu w menu **Start** kliknij pozycję **Wszystkie programy**, a następnie kliknij pozycję **Instalator platformy Microsoft Web**.
3. W górnej części okna **Instalatora platformy sieci Web 3,0** kliknij pozycję **produkty**.
4. Po lewej stronie okna w okienku nawigacji kliknij pozycję **struktury**.
5. W wierszu **Microsoft .NET Framework 4** , jeśli .NET Framework nie jest jeszcze zainstalowany, kliknij przycisk **Dodaj**.

    > [!NOTE]
    > Być może zainstalowano już .NET Framework 4,0 do Windows Update. Jeśli produkt lub składnik jest już zainstalowany, Instalator platformy sieci Web wskaże ten element, zastępując przycisk **Dodaj** **zainstalowanym**tekstem.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. W wierszu **ASP.NET MVC 3 (Visual Studio 2010)** kliknij przycisk **Dodaj**.
7. W okienku nawigacji kliknij pozycję **serwer**.
8. W wierszu **zalecana konfiguracja usług IIS 7** kliknij przycisk **Dodaj**.
9. W wierszu **narzędzia Web Deployment 2,1** kliknij pozycję **Dodaj**.
10. Kliknij przycisk **Instaluj**. W Instalatorze platformy sieci Web zostanie wyświetlona lista&#x2014;produktów wraz ze wszystkimi skojarzonymi zależnościami&#x2014;, które zostaną zainstalowane, i wyświetli monit o zaakceptowanie postanowień licencyjnych.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Zapoznaj się z postanowieniami licencyjnymi, a jeśli wyrażasz zgodę na warunki, kliknij przycisk **Akceptuję**.
12. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie zamknij okno **Instalatora platformy sieci Web 3,0** .

Jeśli zainstalowano .NET Framework 4,0 przed zainstalowaniem usług IIS, należy uruchomić [Narzędzie rejestracji ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) w celu zarejestrowania najnowszej wersji ASP.NET za pomocą usług IIS. Jeśli tego nie zrobisz, program IIS będzie obsługiwał zawartość statyczną (np. pliki HTML) bez żadnych problemów, ale zwróci **błąd HTTP 404,0 — nie można go znaleźć** podczas próby przeglądania zawartości ASP.NET. Za pomocą tej procedury można upewnić się, że ASP.NET 4,0 jest zarejestrowany.

**Aby zarejestrować ASP.NET 4,0 w usługach IIS**

1. Kliknij przycisk **Start**, a następnie wpisz **wiersz polecenia**.
2. W wynikach wyszukiwania kliknij prawym przyciskiem myszy pozycję **wiersz polecenia**, a następnie kliknij polecenie **Uruchom jako administrator**.
3. W oknie wiersza polecenia przejdź do katalogu **%windir%\Microsoft.NET\Framework\v4.0.30319** .
4. Wpisz następujące polecenie, a następnie naciśnij klawisz ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Jeśli zamierzasz hostować 64-bitowe aplikacje sieci Web w dowolnym momencie, należy również zarejestrować 64-bitową wersję ASP.NET z usługami IIS. Aby to zrobić, w oknie wiersza polecenia przejdź do katalogu **%windir%\Microsoft.NET\Framework64\v4.0.30319** .
6. Wpisz następujące polecenie, a następnie naciśnij klawisz ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

Dobrym sposobem jest ponowne użycie Windows Update w tym momencie do pobrania i zainstalowania wszelkich dostępnych aktualizacji dla nowych produktów i składników, które zostały zainstalowane.

## <a name="configure-the-iis-website"></a>Skonfiguruj witrynę sieci Web usług IIS

Aby można było wdrożyć zawartość sieci Web na serwerze, należy utworzyć i skonfigurować witrynę sieci Web usług IIS do hostowania zawartości. Web Deploy można wdrażać pakiety sieci Web tylko w istniejącej witrynie sieci Web usług IIS; nie można utworzyć witryny sieci Web. Na wysokim poziomie należy wykonać następujące zadania:

- Utwórz folder w systemie plików, aby hostować zawartość.
- Utwórz witrynę sieci Web usług IIS, która będzie obsługiwała zawartość, a następnie skojarz ją z folderem lokalnym.
- Przyznaj uprawnienia do odczytu tożsamości puli aplikacji w folderze lokalnym.

Mimo że nie zatrzymasz wdrożenia zawartości w domyślnej witrynie sieci Web w usługach IIS, to podejście nie jest zalecane w przypadku wszystkich scenariuszy testowych i demonstracyjnych. Aby symulować środowisko produkcyjne, należy utworzyć nową witrynę sieci Web usług IIS z ustawieniami specyficznymi dla wymagań aplikacji.

**Aby utworzyć i skonfigurować witrynę sieci Web usług IIS**

1. W lokalnym systemie plików Utwórz folder do przechowywania zawartości (na przykład **C:\DemoSite**).
2. W menu **Start** wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij pozycję **Menedżer Internet Information Services (IIS)** .
3. W Menedżerze usług IIS w okienku **połączenia** rozwiń węzeł serwera (na przykład **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Kliknij prawym przyciskiem myszy węzeł **Lokacje** , a następnie kliknij polecenie **Dodaj witrynę sieci Web**.
5. W polu **Nazwa lokacji** wpisz nazwę witryny sieci Web usług IIS (na przykład **DemoSite**).
6. W polu **Ścieżka fizyczna** wpisz (lub przejdź do) ścieżkę do folderu lokalnego (na przykład **C:\DemoSite**).
7. W polu **port** wpisz numer portu, na którym ma być hostowana witryna sieci Web (na przykład **85**).

    > [!NOTE]
    > Standardowe numery portów są 80 dla protokołów HTTP i 443 dla protokołu HTTPS. Jeśli jednak ta witryna sieci Web jest hostowana na porcie 80, należy zatrzymać domyślną witrynę sieci Web przed uzyskaniem dostępu do witryny.
8. Pozostaw pole **Nazwa hosta** puste, chyba że chcesz skonfigurować rekord systemu nazw domen (DNS) dla witryny sieci Web, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > W środowisku produkcyjnym prawdopodobnie chcesz hostować witrynę sieci Web na porcie 80 i skonfigurować nagłówek hosta wraz ze zgodnymi rekordami DNS. Aby uzyskać więcej informacji na temat konfigurowania nagłówków hosta w usługach IIS 7, zobacz [Konfigurowanie nagłówka hosta dla witryny sieci Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Aby uzyskać więcej informacji na temat roli serwera DNS w systemie Windows Server 2008 R2, zobacz [Omówienie serwera DNS](https://technet.microsoft.com/library/cc770392.aspx) i [serwera DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. W okienku **Akcje** w obszarze **Edytuj witrynę**kliknij pozycję **powiązania**.
10. W oknie dialogowym **powiązania witryny** kliknij przycisk **Dodaj**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. W oknie dialogowym **Dodawanie powiązania witryny** Ustaw **adres IP** i **port** zgodne z istniejącą konfiguracją lokacji.
12. W polu **Nazwa hosta** wpisz nazwę serwera sieci Web (na przykład **TESTWEB1**), a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > Pierwsze powiązanie witryny umożliwia dostęp do lokacji lokalnie przy użyciu adresu IP i portu lub `http://localhost:85`. Drugie powiązanie witryny pozwala uzyskać dostęp do lokacji z innych komputerów w domenie przy użyciu nazwy komputera (na przykład http://testweb1:85).
13. W oknie dialogowym **powiązania witryny** kliknij przycisk **Zamknij**.
14. W okienku **połączenia** kliknij pozycję **Pule aplikacji**.
15. W okienku **Pule aplikacji** kliknij prawym przyciskiem myszy nazwę puli aplikacji, a następnie kliknij pozycję **Ustawienia podstawowe**. Domyślnie nazwa puli aplikacji będzie zgodna z nazwą witryny sieci Web (na przykład **DemoSite**).
16. Na liście **wersja .NET Framework** wybierz pozycję **.NET Framework v 4.0.30319**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > Przykładowe rozwiązanie wymaga .NET Framework 4,0. Nie jest to wymagane dla Web Deploy ogólnie.

Aby witryna sieci Web mogła udostępniać zawartość, tożsamość puli aplikacji musi mieć uprawnienia do odczytu w lokalnym folderze, w którym jest przechowywana zawartość. W usługach IIS 7,5 pule aplikacji są domyślnie uruchamiane z unikatową tożsamością puli aplikacji (w przeciwieństwie do poprzednich wersji usług IIS, gdzie pule aplikacji zwykle są uruchamiane przy użyciu konta usługi sieciowej). Tożsamość puli aplikacji nie jest kontem użytkownika rzeczywistego i nie jest wyświetlana na żadnej liście użytkowników lub grup&#x2014;zamiast tego jest tworzona dynamicznie po uruchomieniu puli aplikacji. Każda tożsamość puli aplikacji jest dodawana do lokalnej grupy zabezpieczeń **programu IIS\_IUSRS** jako element ukryty.

Aby udzielić uprawnień do tożsamości puli aplikacji w pliku lub folderze, dostępne są dwie opcje:

- Bezpośrednio przypisuj uprawnienia do tożsamości puli aplikacji, używając formatu <strong>IIS puli aplikacji\</strong ><em>[Nazwa puli aplikacji]</em>(na przykład <strong>IIS AppPool\DemoSite</strong>).
- Przypisz uprawnienia do grupy **IUSRS\_usług IIS** .

Najbardziej typowym podejściem jest przypisanie uprawnień do lokalnej grupy **IUSRS usług\_IIS** , ponieważ takie podejście umożliwia zmianę pul aplikacji bez konieczności ponownego konfigurowania uprawnień systemu plików. Kolejna procedura korzysta z tego podejścia opartego na grupach.

> [!NOTE]
> Aby uzyskać więcej informacji o tożsamościach puli aplikacji w usługach IIS 7,5, zobacz [tożsamość puli aplikacji](https://go.microsoft.com/?linkid=9805123).

**Aby skonfigurować uprawnienia do folderu dla witryny sieci Web usług IIS**

1. W Eksploratorze Windows przejdź do lokalizacji folderu lokalnego.
2. Kliknij prawym przyciskiem myszy folder, a następnie kliknij polecenie **Właściwości**.
3. Na karcie **zabezpieczenia** kliknij przycisk **Edytuj**, a następnie kliknij przycisk **Dodaj**.
4. Kliknij pozycję **lokalizacje**. W oknie dialogowym **lokalizacje** wybierz serwer lokalny, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. W oknie dialogowym **Wybieranie użytkowników lub grup** wpisz **IIS\_IUSRS**, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.
6. W oknie dialogowym <strong>uprawnienia dla</strong><em>[nazwa folderu]</em>Zwróć uwagę na to, że nowa grupa ma przypisanych domyślnie uprawnienia <strong>Odczyt &amp; wykonywanie</strong>, <strong>Wyświetlanie zawartości folderu</strong>i <strong>Odczyt</strong> . Pozostaw ten niezmieniony i kliknij przycisk <strong>OK</strong>.
7. Kliknij przycisk <strong>OK</strong> , aby zamknąć okno dialogowe<strong>Właściwości</strong> <em>[nazwa folderu]</em>.

Jako końcowe zadanie przed podjęciem próby wdrożenia jakichkolwiek pakietów sieci Web na serwerze należy upewnić się, że usługa Deployment Agent sieci Web jest uruchomiona. Podczas wdrażania pakietu z komputera zdalnego usługa Deployment Agent sieci Web jest odpowiedzialna za wyodrębnianie i Instalowanie zawartości pakietu. Usługa jest uruchamiana domyślnie podczas instalowania narzędzia do wdrażania w sieci Web i uruchamiania w ramach tożsamości usługi sieciowej.

Możesz sprawdzić, czy usługa jest uruchomiona na wiele różnych sposobów, przy użyciu różnych narzędzi wiersza polecenia lub poleceń cmdlet programu Windows PowerShell. Ta procedura opisuje proste podejście oparte na interfejsie użytkownika.

**Aby sprawdzić, czy usługa Deployment Agent sieci Web jest uruchomiona**

1. W menu **Start** wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij pozycję **usługi**.
2. Znajdź wiersz **usługi Deployment Agent sieci Web** i sprawdź, czy **stan** jest ustawiony na wartość **uruchomione**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Jeśli usługa nie jest już uruchomiona, kliknij przycisk **Uruchom**.

## <a name="configure-firewall-exceptions"></a>Konfigurowanie wyjątków zapory

Domyślnie usługa zdalnego agenta nasłuchuje na porcie TCP 80, pod tym adresem URL:

<http://servername.com/MSDEPLOYAGENTSERVICE>

W większości przypadków nie trzeba konfigurować żadnych dodatkowych reguł zapory dla usługi zdalnego agenta, ponieważ serwer sieci Web zwykle nasłuchuje żądań HTTP na porcie 80. Jeśli dostosowano instalację do nasłuchiwania na niestandardowym porcie, należy skonfigurować wyjątki zapory zgodnie z wymaganiami.

## <a name="conclusion"></a>Wniosek

Na tym etapie serwer sieci Web jest gotowy do akceptowania i zainstalowania pakietów internetowych z komputera zdalnego. Przed podjęciem próby wdrożenia aplikacji sieci Web na serwerze warto sprawdzić następujące kluczowe punkty:

- Czy zarejestrowano ASP.NET 4,0 z usługami IIS?
- Czy tożsamość puli aplikacji ma dostęp do odczytu do folderu źródłowego witryny sieci Web?
- Czy usługa Deployment Agent sieci Web jest uruchomiona?

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące konfigurowania niestandardowych plików programu Microsoft Build Engine (MSBuild) do wdrażania pakietów sieci Web w usłudze zdalnego agenta, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-production-environment-for-web-deployment.md)
> [dalej](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)

---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Konfigurowanie serwera sieci Web na potrzeby publikowania Web Deploy (obsługa Web Deploy) | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób konfigurowania serwera sieci Web Internet Information Services (IIS) do obsługi publikowania w sieci Web i wdrażania przy użyciu programu IIS Web Deploy Han...
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: baaebd32f08d3c6b861572c5c5a16ec0fb70aaf0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589041"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Konfigurowanie serwera internetowego dla usługi publikowania Web Deploy (procedura obsługi narzędzia Web Deploy)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania serwera sieci Web Internet Information Services (IIS) do obsługi publikowania w sieci Web i wdrażania przy użyciu programu obsługi Web Deploy IIS.
> 
> Pracując z Web Deploy 2,0 lub nowszym, istnieją trzy główne podejścia, których można użyć do uzyskania aplikacji lub witryn na serwerze sieci Web. Można:
> 
> - Użyj *usługi zdalnego agenta Web Deploy*. Takie podejście wymaga mniejszej konfiguracji serwera sieci Web, ale musisz podać poświadczenia administratora serwera lokalnego w celu wdrożenia wszystkich elementów na serwerze.
> - Użyj *procedury obsługi Web Deploy*. To podejście jest znacznie bardziej złożone i wymaga większego nakładu pracy w celu skonfigurowania serwera sieci Web. Jednak podczas korzystania z tego podejścia można skonfigurować usługi IIS, aby umożliwić użytkownikom niebędącym administratorami wdrażanie. Procedura obsługi Web Deploy jest dostępna tylko w usługach IIS w wersji 7 lub nowszej.
> - Użyj *wdrożenia w trybie offline*. Takie podejście wymaga najmniejszej konfiguracji serwera sieci Web, ale administrator serwera musi ręcznie skopiować pakiet sieci Web na serwer i zaimportować go za pomocą Menedżera usług IIS.
> 
> Aby uzyskać więcej informacji o najważniejszych funkcjach, zaletach i wadach tych metod, zobacz [Wybieranie odpowiedniego podejścia do wdrażania w sieci Web](choosing-the-right-approach-to-web-deployment.md).

Tak, jeśli chcesz zezwolić użytkownikom niebędącym administratorami na wdrażanie zawartości w określonych witrynach sieci Web usług IIS. Takie podejście jest często pożądane w następujących typach scenariuszy:

- Środowiska przejściowe lub produkcyjne, w przypadku których osoba lub konto usługi wyzwalające wdrożenie zdalne prawdopodobnie nie będzie miało dostępu do poświadczeń administratora serwera.
- Środowiska hostowane, w których użytkownicy zdalni mają mieć możliwość aktualizowania swoich witryn sieci Web bez podawania im pełnej kontroli nad swoimi serwerami (lub dostępem do witryn internetowych innych firm).

W scenariuszach programistycznych lub testowych lub w mniejszych organizacjach wdrażanie zawartości przy użyciu poświadczeń administratora serwera jest często mniej contentious. W tych scenariuszach skonfigurowanie serwerów sieci Web do obsługi wdrażania przy użyciu [usługi zdalnego agenta Web Deploy](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) oferuje bardziej proste podejście.

## <a name="task-overview"></a>Przegląd zadania

Aby skonfigurować serwer sieci Web do akceptowania i wdrażania pakietów internetowych z komputera zdalnego przy użyciu metody obsługi Web Deploy, należy wykonać następujące działania:

- Utwórz lub wybierz konto użytkownika domeny ("użytkownik niebędący administratorem"), którego poświadczenia będą używane do wykonywania wdrożeń.
- Zainstaluj usługi IIS 7,5, w tym usługę zarządzania siecią Web i moduł uwierzytelniania podstawowego.
- Zainstaluj Web Deploy 2,1 lub nowszy.
- Skonfiguruj usługę zarządzania siecią Web tak, aby zezwalała na połączenia zdalne, i uruchom usługę.
- Utwórz witrynę sieci Web usług IIS do hostowania wdrożonej zawartości.
- Przyznaj użytkownikom innym niż administrator uprawnienia administratora w Menedżerze usług IIS.
- Upewnij się, że reguły delegowania usługi zarządzania siecią Web zezwalają usłudze na dodawanie i zmienianie zawartości witryny sieci Web przy użyciu konta użytkownika niebędącego administratorem.
- Skonfiguruj zapory, aby zezwolić na połączenia przychodzące na porcie 8172.

Aby hostować przykładowe rozwiązanie ContactManager, należy również wykonać następujące:

- Zainstaluj .NET Framework 4,0.
- Zainstaluj ASP.NET MVC 3.

W tym temacie przedstawiono sposób wykonywania każdej z tych procedur. Zadania i instruktaże w tym temacie zakładają, że rozpoczyna się od czystej kompilacji serwera z systemem Windows Server 2016. Przed kontynuowaniem upewnij się, że:

- Windows Server 2016
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.

> [!NOTE]
> Aby uzyskać więcej informacji na temat dołączania komputerów do domeny, zobacz [dołączanie komputerów do domeny i logowanie](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [Konfigurowanie statycznego adresu IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Zainstaluj produkty i składniki

Ta sekcja przeprowadzi Cię przez proces instalowania wymaganych produktów i składników na serwerze sieci Web. Przed rozpoczęciem zaleca się uruchomienie Windows Update, aby upewnić się, że serwer jest całkowicie aktualny.

W takim przypadku należy zainstalować następujące elementy:

- **Zalecana konfiguracja usług IIS 7**. Umożliwia to roli **serwer sieci Web (IIS)** na serwerze sieci Web i instaluje zestaw modułów i składników usług IIS, które są potrzebne w celu hostowania aplikacji ASP.NET.
- **IIS: Usługa zarządzania**. Spowoduje to zainstalowanie usługi zarządzania siecią Web (WMSvc) w usługach IIS. Ta usługa umożliwia zdalne zarządzanie witrynami sieci Web usług IIS i uwidacznia punkt końcowy programu obsługi Web Deploy na klientach.
- **Usługi IIS: uwierzytelnianie podstawowe**. Spowoduje to zainstalowanie modułu uwierzytelniania podstawowego usług IIS. Dzięki temu usługa zarządzania siecią Web (WMSvc) będzie uwierzytelniać wprowadzone poświadczenia.
- **Web Deployment Tool 2,1 lub nowszym**. Spowoduje to zainstalowanie Web Deploy (i jego podstawowego pliku wykonywalnego, MSDeploy. exe) na serwerze. W ramach tego procesu program instaluje procedurę obsługi Web Deploy i integruje ją z usługą zarządzania siecią Web.
- **.NET Framework 4,0**. Jest to wymagane do uruchamiania aplikacji, które zostały skompilowane w tej wersji .NET Framework.
- **ASP.NET MVC 3**. Spowoduje to zainstalowanie zestawów potrzebnych do uruchamiania aplikacji MVC 3.

> [!NOTE]
> W tym przewodniku opisano sposób korzystania z Instalatora platformy sieci Web w celu zainstalowania i skonfigurowania różnych składników. Mimo że nie jest konieczne używanie Instalatora platformy sieci Web, upraszcza proces instalacji przez automatyczne wykrywanie zależności i zapewnia, że zawsze otrzymujesz najnowsze wersje produktu. Aby uzyskać więcej informacji, zobacz [Instalator platformy Microsoft Web](https://go.microsoft.com/?linkid=9805118).

**Aby zainstalować wymagane produkty i składniki**

1. Pobierz i zainstaluj [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).
2. Po zakończeniu instalacji Instalator platformy sieci Web zostanie uruchomiony automatycznie.

    > [!NOTE]
    > Teraz można uruchomić Instalatora platformy sieci Web w dowolnym momencie z menu **Start** . W tym celu w menu **Start** kliknij pozycję **Wszystkie programy**, a następnie kliknij pozycję **Instalator platformy Microsoft Web**.
3. W górnej części okna **Instalatora platformy sieci Web** kliknij pozycję **produkty**.
4. Po lewej stronie okna w okienku nawigacji kliknij pozycję **struktury**.
5. W wierszu **Microsoft .NET Framework 4** , jeśli .NET Framework nie jest jeszcze zainstalowany, kliknij przycisk **Dodaj**.

    > [!NOTE]
    > Być może zainstalowano już .NET Framework 4,0 do Windows Update. Jeśli produkt lub składnik jest już zainstalowany, Instalator platformy sieci Web wskaże ten element, zastępując przycisk **Dodaj** **zainstalowanym**tekstem.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. W wierszu **ASP.NET MVC 3 (Visual Studio 2010)** kliknij przycisk **Dodaj**.
7. W okienku nawigacji kliknij pozycję **serwer**.
8. W wierszu **zalecana konfiguracja usług IIS 7** kliknij przycisk **Dodaj**.
9. W wierszu **narzędzia Web Deployment 2,1** kliknij pozycję **Dodaj**.
10. W programie **IIS: uwierzytelnianie podstawowe** , kliknij przycisk **Dodaj**.
11. W wierszu **usługi IIS: Management** kliknij pozycję **Dodaj**.
12. Kliknij przycisk **Instaluj**. W Instalatorze platformy sieci Web zostanie wyświetlona lista&#x2014;produktów wraz ze wszystkimi skojarzonymi zależnościami&#x2014;, które zostaną zainstalowane, i wyświetli monit o zaakceptowanie postanowień licencyjnych.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Zapoznaj się z postanowieniami licencyjnymi, a jeśli wyrażasz zgodę na warunki, kliknij przycisk **Akceptuję**.
14. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie zamknij okno **Instalatora platformy sieci Web** .

Jeśli zainstalowano .NET Framework 4,0 przed zainstalowaniem usług IIS, należy uruchomić [Narzędzie rejestracji ASP.NET IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) w celu zarejestrowania najnowszej wersji ASP.NET za pomocą usług IIS. Jeśli tego nie zrobisz, program IIS będzie obsługiwał zawartość statyczną (np. pliki HTML) bez żadnych problemów, ale zwróci **błąd HTTP 404,0 — nie można go znaleźć** podczas próby przeglądania zawartości ASP.NET. Możesz użyć następnej procedury, aby upewnić się, że ASP.NET 4,0 jest zarejestrowany.

**Aby zarejestrować ASP.NET 4,0 w usługach IIS**

1. Kliknij przycisk **Start**, a następnie wpisz **wiersz polecenia**.
2. W wynikach wyszukiwania kliknij prawym przyciskiem myszy pozycję **wiersz polecenia**, a następnie kliknij polecenie **Uruchom jako administrator**.
3. W oknie wiersza polecenia przejdź do katalogu **%windir%\Microsoft.NET\Framework\v4.0.30319** .
4. Wpisz następujące polecenie, a następnie naciśnij klawisz ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Jeśli zamierzasz hostować 64-bitowe aplikacje sieci Web w dowolnym momencie, należy również zarejestrować 64-bitową wersję ASP.NET z usługami IIS. Aby to zrobić, w oknie wiersza polecenia przejdź do katalogu **%windir%\Microsoft.NET\Framework64\v4.0.30319** .
6. Wpisz następujące polecenie, a następnie naciśnij klawisz ENTER:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Dobrym sposobem jest ponowne użycie Windows Update w tym momencie do pobrania i zainstalowania wszelkich dostępnych aktualizacji dla nowych produktów i składników, które zostały zainstalowane.

## <a name="configure-the-web-management-service"></a>Konfigurowanie usługi zarządzania siecią Web

Teraz, gdy zainstalowano wszystko, czego potrzebujesz, następnym krokiem jest skonfigurowanie usługi zarządzania siecią Web w usługach IIS. Na wysokim poziomie należy wykonać następujące zadania:

- Włącz uwierzytelnianie podstawowe na poziomie serwera.
- Skonfiguruj usługę zarządzania siecią Web tak, aby akceptowała połączenia zdalne.
- Uruchom usługę zarządzania siecią Web.
- Sprawdź, czy istnieją wymagane reguły delegowania usługi zarządzania siecią Web.

**Aby skonfigurować usługę zarządzania siecią Web**

1. W menu **Start** wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij pozycję **Menedżer Internet Information Services (IIS)** .
2. W Menedżerze usług IIS w okienku **połączenia** kliknij węzeł serwera (na przykład **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. W środkowym okienku w obszarze **IIS**kliknij dwukrotnie pozycję **uwierzytelnianie**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. Kliknij prawym przyciskiem myszy pozycję **uwierzytelnianie podstawowe**, a następnie kliknij pozycję **Włącz**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. W okienku **połączenia** kliknij ponownie węzeł serwera, aby powrócić do ustawień najwyższego poziomu.
6. W środkowym okienku w obszarze **Zarządzanie**kliknij dwukrotnie pozycję **Usługa zarządzania**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. W środkowym okienku wybierz opcję **Włącz połączenia zdalne**.

    > [!NOTE]
    > Jeśli usługa zarządzania siecią Web jest już uruchomiona, należy ją najpierw zatrzymać.
8. W okienku **Akcje** kliknij pozycję **Uruchom** , aby uruchomić usługę zarządzania siecią Web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Jeśli zostanie wyświetlony monit o zapisanie ustawień, kliknij przycisk **tak**.

    > [!NOTE]
    > Możesz również skonfigurować automatyczne uruchamianie usługi. W tym celu Otwórz konsolę usługi, kliknij prawym przyciskiem myszy pozycję **Usługa zarządzania siecią Web**, a następnie kliknij polecenie **Właściwości**. Z listy rozwijanej **Typ uruchomienia** wybierz pozycję **Automatyczne**, a następnie kliknij przycisk **OK**.
10. W okienku **połączenia** kliknij ponownie węzeł serwera, aby powrócić do ustawień najwyższego poziomu.
11. W środkowym okienku w obszarze **Zarządzanie**kliknij dwukrotnie pozycję **delegowanie usługi zarządzania**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Sprawdź, czy środkowe okienko zawiera zestaw reguł.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Te reguły umożliwiają autoryzowanym użytkownikom usługi zarządzania sieci Web korzystanie z różnych dostawców Web Deploy. Aby na przykład wdrożyć aplikacje sieci Web i zawartość w usługach IIS za pomocą programu obsługi Web Deploy, musi istnieć reguła delegowania umożliwiająca wszystkim użytkownikom usługi zarządzania siecią Web użycie dostawców **contentPath** i **iisapp** (ostatnią regułę, którą można zobaczyć na zrzucie ekranu).

    Jeśli zainstalowano produkty i składniki w kolejności opisanej w tym temacie, Najnowsza wersja Web Deploy powinna automatycznie dodać wszystkie wymagane reguły delegowania do usługi zarządzania siecią Web. Jeśli na stronie delegowania usługi zarządzania nie są wyświetlane żadne reguły, należy utworzyć je samodzielnie. Aby uzyskać instrukcje, jak to zrobić, zobacz [Konfigurowanie programu obsługi wdrażania w sieci Web](https://go.microsoft.com/?linkid=9805124).
13. W okienku **połączenia** kliknij ponownie węzeł serwera, aby powrócić do ustawień najwyższego poziomu.

## <a name="create-and-configure-an-iis-website"></a>Tworzenie i Konfigurowanie witryny sieci Web usług IIS

Aby można było wdrożyć zawartość sieci Web na serwerze, należy utworzyć i skonfigurować witrynę sieci Web usług IIS do hostowania zawartości. Web Deploy można wdrażać pakiety sieci Web tylko w istniejącej witrynie sieci Web usług IIS; nie można utworzyć witryny sieci Web. Należy również wykonać nieco dodatkową konfigurację, aby zezwolić na zdalne wdrażanie zawartości przez konto niebędące administratorami. Na wysokim poziomie należy wykonać następujące zadania:

- Utwórz folder w systemie plików, aby hostować zawartość.
- Utwórz witrynę sieci Web usług IIS, która będzie obsługiwała zawartość, a następnie skojarz ją z folderem lokalnym.
- Przyznaj uprawnienia do odczytu tożsamości puli aplikacji w folderze lokalnym.
- Przyznaj wymagane uprawnienia usług IIS do konta domeny, które będzie wdrażać aplikację sieci Web.

Mimo że nie zatrzymasz wdrożenia zawartości w domyślnej witrynie sieci Web w usługach IIS, to podejście nie jest zalecane w przypadku wszystkich scenariuszy testowych i demonstracyjnych. Aby symulować środowisko produkcyjne, należy utworzyć nową witrynę sieci Web usług IIS z ustawieniami specyficznymi dla wymagań aplikacji.

**Aby utworzyć witrynę sieci Web usług IIS**

1. W lokalnym systemie plików Utwórz folder do przechowywania zawartości (na przykład **C:\DemoSite**).
2. W menu **Start** wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij pozycję **Menedżer Internet Information Services (IIS)** .
3. W Menedżerze usług IIS w okienku **połączenia** rozwiń węzeł serwera (na przykład **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Kliknij prawym przyciskiem myszy węzeł **Lokacje** , a następnie kliknij polecenie **Dodaj witrynę sieci Web**.
5. W polu **Nazwa lokacji** wpisz nazwę witryny sieci Web usług IIS (na przykład **DemoSite**).
6. W polu **Ścieżka fizyczna** wpisz (lub przejdź do) ścieżkę do folderu lokalnego (na przykład **C:\DemoSite**).
7. W polu **port** wpisz numer portu, na którym ma być hostowana witryna sieci Web (na przykład **85**).

    > [!NOTE]
    > Standardowe numery portów są 80 dla protokołów HTTP i 443 dla protokołu HTTPS. Jeśli jednak ta witryna sieci Web jest hostowana na porcie 80, należy zatrzymać domyślną witrynę sieci Web przed uzyskaniem dostępu do witryny.
8. Pozostaw pole **Nazwa hosta** puste, chyba że chcesz skonfigurować rekord systemu nazw domen (DNS) dla witryny sieci Web, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > W środowisku produkcyjnym prawdopodobnie chcesz hostować witrynę sieci Web na porcie 80 i skonfigurować nagłówek hosta wraz ze zgodnymi rekordami DNS. Aby uzyskać więcej informacji na temat konfigurowania nagłówków hosta w usługach IIS 7, zobacz [Konfigurowanie nagłówka hosta dla witryny sieci Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Aby uzyskać więcej informacji na temat roli serwera DNS w systemie Windows Server, zobacz [Omówienie serwera DNS](https://technet.microsoft.com/library/cc770392.aspx) i [serwera DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. W okienku **Akcje** w obszarze **Edytuj witrynę**kliknij pozycję **powiązania**.
10. W oknie dialogowym **powiązania witryny** kliknij przycisk **Dodaj**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. W oknie dialogowym **Dodawanie powiązania witryny** Ustaw **adres IP** i **port** zgodne z istniejącą konfiguracją lokacji.
12. W polu **Nazwa hosta** wpisz nazwę serwera sieci Web (na przykład **STAGEWEB1**), a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > Pierwsze powiązanie witryny umożliwia dostęp do lokacji lokalnie przy użyciu adresu IP i portu lub `http://localhost:85`. Drugie powiązanie witryny pozwala uzyskać dostęp do lokacji z innych komputerów w domenie przy użyciu nazwy komputera (na przykład http://stageweb1:85).
13. W oknie dialogowym **powiązania witryny** kliknij przycisk **Zamknij**.
14. W okienku **połączenia** kliknij pozycję **Pule aplikacji**.
15. W okienku **Pule aplikacji** kliknij prawym przyciskiem myszy nazwę puli aplikacji, a następnie kliknij pozycję **Ustawienia podstawowe**. Domyślnie nazwa puli aplikacji będzie zgodna z nazwą witryny sieci Web (na przykład **DemoSite**).
16. Na liście **wersja środowiska CLR platformy .NET** wybierz pozycję **.NET CLR v 4.0.30319**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

    > [!NOTE]
    > Przykładowe rozwiązanie wymaga .NET Framework 4,0. Nie jest to wymagane dla Web Deploy ogólnie.

Aby witryna sieci Web mogła udostępniać zawartość, tożsamość puli aplikacji musi mieć uprawnienia do odczytu w lokalnym folderze, w którym jest przechowywana zawartość. W usługach IIS 7,5 pule aplikacji są domyślnie uruchamiane z unikatową tożsamością puli aplikacji (w przeciwieństwie do poprzednich wersji usług IIS, gdzie pule aplikacji zwykle są uruchamiane przy użyciu konta usługi sieciowej). Tożsamość puli aplikacji nie jest kontem użytkownika rzeczywistego i nie jest wyświetlana na żadnej liście użytkowników lub grup&#x2014;zamiast tego jest tworzona dynamicznie po uruchomieniu puli aplikacji. Każda tożsamość puli aplikacji jest dodawana do lokalnej grupy zabezpieczeń **programu IIS\_IUSRS** jako element ukryty.

Aby udzielić uprawnień do tożsamości puli aplikacji w pliku lub folderze, dostępne są dwie opcje:

- Bezpośrednio przypisuj uprawnienia do tożsamości puli aplikacji, używając formatu <strong>IIS puli aplikacji\</strong ><em>[Nazwa puli aplikacji]</em>(na przykład <strong>IIS AppPool\DemoSite</strong>).
- Przypisz uprawnienia do grupy **IUSRS\_usług IIS** .

Najbardziej typowym podejściem jest przypisanie uprawnień do lokalnej grupy **IUSRS usługi\_IIS** , ponieważ takie podejście umożliwia zmianę pul aplikacji bez konieczności ponownego konfigurowania uprawnień systemu plików. Kolejna procedura korzysta z tego podejścia opartego na grupach.

> [!NOTE]
> Aby uzyskać więcej informacji o tożsamościach puli aplikacji w usługach IIS 7,5, zobacz [tożsamość puli aplikacji](https://go.microsoft.com/?linkid=9805123).

**Aby skonfigurować uprawnienia do folderu dla witryny sieci Web usług IIS**

1. W Eksploratorze Windows przejdź do lokalizacji folderu lokalnego.
2. Kliknij prawym przyciskiem myszy folder, a następnie kliknij polecenie **Właściwości**.
3. Na karcie **zabezpieczenia** kliknij przycisk **Edytuj**, a następnie kliknij przycisk **Dodaj**.
4. Kliknij pozycję **lokalizacje**. W oknie dialogowym **lokalizacje** wybierz serwer lokalny, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. W oknie dialogowym **Wybieranie użytkowników lub grup** wpisz **IIS\_IUSRS**, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.
6. W oknie dialogowym <strong>uprawnienia dla</strong><em>[nazwa folderu]</em> Zwróć uwagę na to, że nowa grupa ma przypisanych domyślnie uprawnienia <strong>Odczyt &amp; wykonywanie</strong>, <strong>Wyświetlanie zawartości folderu</strong>i <strong>Odczyt</strong> . Pozostaw ten niezmieniony i kliknij przycisk <strong>OK</strong>.
7. Kliknij przycisk <strong>OK</strong> , aby zamknąć okno dialogowe<strong>Właściwości</strong> <em>[nazwa folderu]</em>.

W końcowym zadaniu należy przyznać odpowiednie uprawnienia użytkownikowi niebędącemu administratorem, którego poświadczenia będą używane do wdrożenia zawartości. Ten użytkownik wymaga uprawnień do zdalnego wdrożenia zawartości w witrynie sieci Web.

**Aby skonfigurować uprawnienia witryny sieci Web usług IIS dla użytkownika domeny, który nie jest administratorem**

1. W Menedżerze usług IIS w okienku **połączenia** kliknij prawym przyciskiem myszy węzeł witryny sieci Web (na przykład **DemoSite**), wskaż polecenie **Wdróż**, a następnie kliknij pozycję **Konfiguruj publikowanie Web Deploy**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. W oknie dialogowym **Konfigurowanie publikowania Web Deploy** po prawej stronie listy **Wybierz użytkownika, aby nadać uprawnienia do publikowania** kliknij przycisk wielokropka.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. W oknie dialogowym **Zezwalanie użytkownikowi** wpisz nazwę domeny i użytkownika konta, którego chcesz użyć do wdrożenia zawartości, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. W oknie dialogowym **Konfigurowanie publikowania Web Deploy** kliknij przycisk **Setup**(Konfiguracja).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Ta operacja wykonuje dwie kluczowe funkcje w jednym kroku. Po pierwsze przyznaje użytkownikowi uprawnienia do zdalnego modyfikowania witryny internetowej za pomocą usługi zarządzania siecią Web, zgodnie z regułami delegowania, które zostały sprawdzone w poprzedniej sekcji. Drugi umożliwia użytkownikowi pełną kontrolę nad folderem źródłowym witryny sieci Web, co pozwala użytkownikowi dodawać, modyfikować i ustawiać uprawnienia do zawartości witryny sieci Web.
5. W oknie dialogowym **Konfigurowanie publikowania Web Deploy** kliknij przycisk **Zamknij**.

## <a name="configure-firewall-exceptions"></a>Konfigurowanie wyjątków zapory

Domyślnie usługa zarządzania siecią Web usług IIS nasłuchuje na porcie TCP 8172. Jeśli Zapora systemu Windows jest włączona na serwerze sieci Web, należy utworzyć nową regułę ruchu przychodzącego zezwalającą na ruch TCP na porcie 8172 (domyślnie cały ruch wychodzący jest dozwolony w zaporze systemu Windows). Jeśli używasz zapory innej firmy, musisz utworzyć reguły zezwalające na ruch.

| Kierunek | Z portu | Do portu | Typ portu |
| --- | --- | --- | --- |
| Dotycząc | Dowolne | 8172 | TCP |
| Wyjściowy | 8172 | Dowolne | TCP |

Aby uzyskać więcej informacji na temat konfigurowania reguł w zaporze systemu Windows, zobacz [Konfigurowanie reguł zapory](https://technet.microsoft.com/library/dd448559(WS.10).aspx). W przypadku zapór innych firm zapoznaj się z dokumentacją produktu.

## <a name="conclusion"></a>Wniosek

Serwer sieci Web powinien być teraz gotowy do akceptowania zdalnych wdrożeń do programu obsługi Web Deploy za pomocą usługi zarządzania siecią Web. Przed podjęciem próby wdrożenia aplikacji sieci Web na serwerze warto sprawdzić następujące kluczowe punkty:

- Czy na poziomie serwera w usługach IIS włączono uwierzytelnianie podstawowe?
- Czy włączono połączenia zdalne z usługą zarządzania siecią Web?
- Czy uruchomiono usługę zarządzania siecią Web?
- Czy istnieją reguły delegowania usług zarządzania?
- Czy tożsamość puli aplikacji ma dostęp do odczytu do folderu źródłowego witryny sieci Web?
- Czy konto użytkownika niebędącego administratorem ma uprawnienia na poziomie witryny w usługach IIS?
- Czy zapora zezwala na połączenia przychodzące do serwera na porcie TCP 8172?

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące konfigurowania niestandardowych plików programu Microsoft Build Engine (MSBuild) do wdrażania pakietów sieci Web w obsłudze Web Deploy, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [dalej](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)

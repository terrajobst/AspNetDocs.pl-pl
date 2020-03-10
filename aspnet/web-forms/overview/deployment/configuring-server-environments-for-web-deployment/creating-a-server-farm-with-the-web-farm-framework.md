---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Tworzenie farmy serwerów za pomocą struktury farmy sieci Web | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób używania platformy Web Farm Framework (WFF) 2,0 do tworzenia i konfigurowania farmy serwerów sieci Web na podstawie kolekcji serwerów.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 204996514bed336e60ab77f184a923f04e7e2bba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637250"
---
# <a name="creating-a-server-farm-with-the-web-farm-framework"></a>Tworzenie farmy serwerów za pomocą rozwiązania Web Farm Framework

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób używania platformy Web Farm Framework (WFF) 2,0 do tworzenia i konfigurowania farmy serwerów sieci Web na podstawie kolekcji serwerów.

WFF umożliwia synchronizowanie produktów i składników platformy sieci Web, aplikacji sieci Web, witryn internetowych i ustawień konfiguracji na wielu serwerach sieci Web o zrównoważonym obciążeniu. W scenariuszach, w których potrzebny jest więcej niż jeden serwer sieci Web, na przykład środowisko przejściowe i produkcyjne, może to znacznie uprościć proces wdrażania i konfiguracji. Aplikację sieci Web można wdrożyć na jednym serwerze&#x2014;, a *serwer*&#x2014;podstawowy i WFF będą automatycznie replikować tę aplikację sieci Web na wszystkich pozostałych serwerach sieci Web w farmie serwerów.

## <a name="understanding-the-web-farm-framework"></a>Omówienie struktury kolektywu serwerów sieci Web

Możesz użyć WFF 2,0 do aprowizacji, zarządzania i wdrażania zawartości w grupie serwerów sieci Web. Wdrożenie WFF składa się z trzech kluczowych ról serwera:

- *Serwer kontrolera*. Ten serwer jest używany do tworzenia i konfigurowania farm serwerów WFF. Serwer kontrolera zarządza synchronizacją składników platformy sieci Web, ustawień konfiguracji i aplikacji między serwerami sieci Web w farmie serwerów. Zainstalujesz WFF 2,0 na serwerze kontrolera, a serwer kontrolera zainstaluje agenta WFF na każdym serwerze w farmie serwerów. Serwer kontrolera nie jest koncepcyjnie należący do żadnej farmy serwerów WFF, a jeden serwer kontrolera może zarządzać wieloma farmami serwerów. W tym scenariuszu używany jest jeden serwer kontrolera WFF do tworzenia i zarządzania tymczasową farmą serwerów oraz farmą serwerów produkcyjnych.
- *Serwer podstawowy*. Każda farma serwerów WFF zawiera jeden serwer podstawowy. Gdy instalujesz składniki platformy sieci Web lub wdrażasz aplikacje na serwerze podstawowym, WFF zsynchronizuje zmiany z innymi serwerami w farmie serwerów.
- *Serwer pomocniczy*. Każda farma serwerów WFF zawiera jeden lub więcej serwerów pomocniczych. Wszelkie zmiany wprowadzane na serwerze podstawowym są replikowane na każdy serwer pomocniczy w farmie serwerów.

Pokazuje, w jaki sposób te role serwera odnoszą się do środowisk firmy Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

W tym scenariuszu środowisko przejściowe i środowisko produkcyjne są skonfigurowane jako farmy serwerów WFF. Jeden serwer kontrolera WFF zarządza obydwoma farmami. W ramach każdej farmy serwerów wszelkie zmiany na serwerze podstawowym są replikowane do każdego serwera pomocniczego.

Przed rozpoczęciem konfigurowania środowisk przejściowych i produkcyjnych zalecamy zapoznanie się z tymi artykułami w celu zapoznania się z kluczowymi założeniami WFF 2,0:

- [Omówienie struktury farmy sieci Web 2,0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Konfigurowanie farmy serwerów z architekturą sieci Web farmy 2,0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Wymagania systemowe i platformy dla struktury farmy sieci Web 2,0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Przegląd zadania

Aby wykonać zadania i instruktaże w tym temacie, potrzebne są co najmniej trzy serwery&#x2014;z jednym kontrolerem WFF, jeden podstawowy serwer sieci Web farmy serwerów oraz co najmniej jeden dodatkowy serwer sieci Web farmy serwerów. W każdej chwili można dodać więcej serwerów pomocniczych do farmy serwerów WFF. Na wysokim poziomie, aby utworzyć i skonfigurować farmę serwerów WFF w środowisku przejściowym lub produkcyjnym, należy:

- Utwórz serwer kontrolera, instalując Internet Information Services (IIS) 7,5 i WFF 2,0.
- Przygotuj podstawowe i pomocnicze serwery przez utworzenie wspólnego konta administratora i skonfigurowanie wyjątków zapory.
- Skonfiguruj farmę serwerów za pomocą Menedżera usług IIS na serwerze kontrolera.
- Skonfiguruj Równoważenie obciążenia przy użyciu funkcji Routing żądań aplikacji (ARR) usług IIS lub alternatywnej technologii równoważenia obciążenia.

W zadaniach i przewodnikach w tym temacie przyjęto założenie, że rozpoczyna się od czystego kompilowania serwerów z systemem Windows Server 2008 R2. Przed rozpoczęciem dla każdego serwera upewnij się, że:

- System Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są zainstalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.

> [!NOTE]
> Aby uzyskać więcej informacji na temat dołączania komputerów do domeny, zobacz [dołączanie komputerów do domeny i logowanie](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [Konfigurowanie statycznego adresu IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="create-the-wff-controller-server"></a>Tworzenie serwera kontrolera WFF

Aby utworzyć serwer kontrolera WFF, należy zainstalować zarówno usługi IIS 7, jak i nowsze oraz WFF 2,0 lub nowsze. W obszarze okładek WFF używa narzędzia do wdrażania w sieci Web usług IIS (Web Deploy) 2. x, aby zsynchronizować serwery w farmie. Jeśli zainstalujesz program WFF za pomocą Instalatora platformy sieci Web, Instalator automatycznie pobierze i zainstaluje Web Deploy.

**Aby utworzyć serwer kontrolera WFF**

1. Pobierz i zainstaluj [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9739157).
2. W górnej części okna **Instalatora platformy sieci Web 3,0** kliknij pozycję **produkty**.
3. Po lewej stronie okna w okienku nawigacji kliknij pozycję **serwer**.
4. W wierszu **zalecana konfiguracja usług IIS 7** kliknij przycisk **Dodaj**.
5. W programie <strong>Web Farm Framework 2.</strong> wiersz <em>x</em> , kliknij przycisk <strong>Dodaj</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Kliknij polecenie **Zainstaluj**. Należy zauważyć, że Instalator platformy sieci Web dodał narzędzie Web Deployment wraz z różnymi innymi zależnościami do listy instalacji.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Zapoznaj się z postanowieniami licencyjnymi, a jeśli wyrażasz zgodę na warunki, kliknij przycisk **Akceptuję**.
8. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie zamknij okno **Instalatora platformy sieci Web 3,0** .

## <a name="configure-the-primary-and-secondary-servers"></a>Konfigurowanie serwerów podstawowych i pomocniczych

Przed utworzeniem farmy serwerów WFF należy wykonać kilka zadań przygotowawczych na serwerach sieci Web, które tworzą farmę:

- Dodaj wyjątki zapory, aby umożliwić komunikację między **sieciami podstawowymi**, **administracją zdalną**i funkcją **udostępniania plików i drukarek** na serwerze kontrolera WFF.
- Utwórz konto domeny (na przykład **FABRIKAM\stagingfarm**) w Active Directory i Dodaj je do lokalnej grupy administratorów na każdym serwerze. To konto będzie używane jako konto administratora farmy serwerów podczas tworzenia farmy serwerów.

Aby uzyskać więcej informacji na temat sposobu konfigurowania tych wyjątków zapory w zaporze systemu Windows, zobacz [wymagania systemowe i platformy dla platformy farmy sieci Web 2,0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805128). W przypadku innych systemów zapory zapoznaj się z dokumentacją produktu.

Przy użyciu następnej procedury można dodać konto domeny do lokalnej grupy administratorów w systemie Windows Server 2008 R2. Tę procedurę należy wykonać na każdym serwerze, który chcesz dodać do farmy&#x2014;serwerów innymi słowy, dodać to samo konto domeny do lokalnej grupy administratorów na serwerze podstawowym i na każdym serwerze pomocniczym.

**Aby dodać konto domeny do lokalnej grupy administratorów**

1. W menu **Start** wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij przycisk **Menedżer serwera**.
2. W oknie **Menedżer serwera** w okienku Widok drzewa rozwiń węzeł **Konfiguracja**, rozwiń węzeł **lokalni użytkownicy i grupy**, a następnie kliknij pozycję **grupy**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. W okienku **grupy** kliknij dwukrotnie pozycję **administratorzy**.
4. W oknie dialogowym **Właściwości administratorów** kliknij przycisk **Dodaj**.
5. W oknie dialogowym **Wybieranie użytkowników, komputerów, kont usług lub grup** wpisz (lub Przeglądaj) konto domeny (na przykład **FABRIKAM\stagingfarm**), a następnie kliknij przycisk **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. W oknie dialogowym **Właściwości administratorów** kliknij przycisk **OK**.

Serwery są teraz gotowe do dodania do farmy serwerów. W przypadku serwera podstawowego można skonfigurować serwer tak, aby spełniał wymagania aplikacji przed lub po utworzeniu farmy&#x2014;serwerów w obu przypadkach, WFF zsynchronizuje serwery, wdrażając te same produkty, składniki lub konfigurację na serwerach pomocniczych. W tym samouczku przyjęto założenie, że serwer podstawowy zostanie skonfigurowany po zakończeniu tworzenia farmy serwerów.

## <a name="create-the-wff-server-farm"></a>Tworzenie farmy serwerów WFF

W tym momencie wszystkie serwery są gotowe do dodania do farmy serwerów WFF:

- Na serwerze kontrolera zainstalowano WFF.
- Wyjątki zapory zostały skonfigurowane na podstawowym i pomocniczym serwerze sieci Web.
- Konto domeny zostało dodane do lokalnej grupy administratorów na serwerze podstawowym i pomocniczym.

Następnym krokiem jest utworzenie farmy serwerów w WFF. Można to zrobić za pomocą Menedżera usług IIS na serwerze kontrolera WFF.

**Aby utworzyć farmę serwerów WFF**

1. Na serwerze kontrolera WFF, w menu **Start** wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij polecenie **Menedżer Internet Information Services (IIS)** .
2. W okienku **połączenia** rozwiń węzeł serwer lokalny, kliknij prawym przyciskiem myszy pozycję **farmy serwerów**, a następnie kliknij polecenie **Utwórz farmę serwerów**.
3. W oknie dialogowym **tworzenie farmy serwerów** wpisz zrozumiałą nazwę farmy serwerów (na przykład **Farma przejściowa**), a następnie wybierz pozycję **Udostępnij farmę serwerów**.
4. Wpisz nazwę użytkownika i hasło konta domeny, które zostało dodane do lokalnej grupy administratorów na każdym serwerze.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Kliknij przycisk **Dalej**.
6. Na stronie **Dodawanie serwerów** wpisz w pełni kwalifikowaną nazwę domeny (FQDN) serwera podstawowego, wybierz opcję **serwer podstawowy**, a następnie kliknij przycisk **Dodaj**.
7. W tym momencie WFF podejmie próbę skontaktowania się z serwerem podstawowym przy użyciu podanych poświadczeń. W przypadku pomyślnego nawiązania połączenia serwer podstawowy zostanie dodany do tabeli na stronie **Dodawanie serwerów** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Być może zauważono, że **serwer jest dostępny do równoważenia obciążenia** jest domyślnie wybrany. WFF używa modułu usług IIS ARR do implementowania równoważenia obciążenia, a tym samym dystrybuuje żądania na serwerach sieci Web w farmie serwerów. W większości scenariuszy tylko **serwer jest dostępny dla opcji równoważenia obciążenia** , jeśli zamiast tego zechcesz użyć rozwiązania do równoważenia obciążenia innej firmy.
8. Na stronie **Dodawanie serwerów** wpisz nazwę FQDN pierwszego serwera pomocniczego, a następnie kliknij przycisk **Dodaj**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Powtórz krok 7 dla wszystkich dodatkowych serwerów pomocniczych w farmie, a następnie kliknij przycisk **Zakończ**.

Farma serwerów WFF jest teraz uruchomiona. Wszystkie produkty lub składniki platformy sieci Web instalowane na serwerze podstawowym oraz wszystkie aplikacje sieci Web lub zawartość wdrażane na serwerze podstawowym zostaną automatycznie wdrożone na wszystkich serwerach pomocniczych.

WFF to szeroki i skomplikowany temat i można dowiedzieć się więcej na jego temat na [Microsoft Web Farm Framework 2,0 dla witryny sieci Web usług IIS 7](https://go.microsoft.com/?linkid=9805129) . W tym czasie istnieją jednak dwa obszary funkcji, z którymi należy się zapoznać:

- *Inicjowanie obsługi aplikacji* to proces, który replikuje zawartość z serwera podstawowego, takiego jak aplikacje sieci Web i ustawienia konfiguracji, na wszystkich serwerach pomocniczych w farmie serwerów. Na przykład w przypadku wdrożenia przykładowego rozwiązania Contact Manager na podstawowym serwerze tymczasowym proces aprowizacji aplikacji WFF będzie wdrażać to rozwiązanie na wszystkich dodatkowych serwerach przejściowych. Domyślnie proces aprowizacji aplikacji jest uruchamiany co 30 sekund.
- *Inicjowanie obsługi platformy* to proces, który synchronizuje produkty i składniki platformy sieci Web z serwera podstawowego do wszystkich serwerów pomocniczych w farmie serwerów. Jeśli na przykład zainstalujesz ASP.NET MVC 3 na podstawowym serwerze tymczasowym, proces aprowizacji platformy użyje Instalatora platformy sieci Web, aby zainstalować ASP.NET MVC 3 na wszystkich dodatkowych serwerach przemieszczania. Domyślnie proces aprowizacji platformy jest uruchamiany co pięć minut.

W Menedżerze usług IIS na serwerze kontrolera WFF można zarządzać podstawowymi ustawieniami aprowizacji aplikacji i platformy.

**Eksplorowanie ustawień aprowizacji aplikacji i platformy**

1. W Menedżerze usług IIS w okienku **połączenia** wybierz farmę serwerów.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. W okienku **farma serwerów** kliknij dwukrotnie pozycję **Inicjowanie obsługi aplikacji**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Jak widać, farma serwerów jest obecnie skonfigurowana do synchronizacji zawartości sieci Web i ustawień konfiguracji między serwerem podstawowym a serwerami pomocniczymi co 30 sekund.
4. Kliknij przycisk **Wstecz**, a następnie kliknij dwukrotnie pozycję **Inicjowanie obsługi platformy**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Jak widać, farma serwerów jest obecnie skonfigurowana do synchronizowania produktów i składników platformy sieci Web między serwerem podstawowym a serwerami pomocniczymi co pięć minut.
6. Kliknij przycisk **Wstecz**.
7. Aby wymusić natychmiastowe synchronizowanie produktów platformy sieci Web w farmie serwerów, w okienku **Akcje** kliknij pozycję **Zainicjuj obsługę platformy**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Inicjowanie obsługi platformy może zająć trochę czasu. Proces Instalatora jest uruchamiany w tle na serwerach pomocniczych w farmie serwerów.
8. Po dodaniu wystarczającej ilości czasu na zakończenie procesu inicjowania obsługi administracyjnej można sprawdzić, czy produkty i składniki dodane do serwera podstawowego zostały teraz zreplikowane na serwerach pomocniczych. Na przykład możesz zalogować się na serwerze pomocniczym i użyć okna **Menedżer serwera** , aby sprawdzić, czy rola serwera sieci Web została zainstalowana.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Możesz również sprawdzić listę zainstalowanych programów, aby sprawdzić, czy dodano różne składniki platformy sieci Web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurowanie równoważenia obciążenia

Podczas tworzenia kolektywu serwerów sieci Web należy skonfigurować pewną formę równoważenia obciążenia, aby dystrybuować żądania HTTP między serwerami sieci Web. Może to być system Windows Server 2008 Równoważenie obciążenia sieciowego usług IIS, ARR lub sprzętowe rozwiązanie do równoważenia obciążenia oparte na oprogramowaniu.

WFF zaprojektowano w celu ścisłej integracji z usługami IIS o GENOTYPie. Aby skorzystać z tej integracji, należy zainstalować moduł ARR na serwerze kontrolera WFF. Następnie należy skierować cały ruch internetowy do serwera kontrolera, zazwyczaj przez skonfigurowanie rekordów systemu nazw domen (DNS). Serwer kontrolera następnie dystrybuuje żądania przychodzące między serwerami w farmie, w oparciu o dostępność serwera i różne inne kryteria.

> [!NOTE]
> Nie musisz używać ARR z WFF; można skonfigurować WFF do pracy z rozwiązaniami do równoważenia obciążenia innych firm. Aby uzyskać więcej informacji, zobacz [Omówienie struktury farmy sieci Web 2,0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805126).

Równoważenie obciążenia przy użyciu ARR to złożone zagadnienie, z których większość wykracza poza zakres tego samouczka. Można jednak użyć następnej procedury, aby zainstalować moduł ARR i zacząć korzystać z równoważenia obciążenia.

**Aby skonfigurować równoważenie obciążenia na serwerze kontrolera WFF**

1. Na serwerze kontrolera WFF Uruchom Instalatora platformy sieci Web.
2. W górnej części okna **Instalatora platformy sieci Web 3,0** kliknij pozycję **produkty**.
3. Po lewej stronie okna w okienku nawigacji kliknij pozycję **serwer**.
4. W wierszu **Routing żądań aplikacji 2,5** kliknij przycisk **Dodaj**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Kliknij przycisk **Zainstaluj**, a następnie postępuj zgodnie z instrukcjami w oknie **instalacji platformy sieci Web** .
6. Po zakończeniu instalacji uruchom Menedżera usług IIS, a następnie w okienku **połączenia** kliknij węzeł farmy serwerów. Zwróć uwagę, że kilka nowych ikon zostało dodanych do okienka **farmy serwerów** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. W okienku **Farma serwerów** kliknij dwukrotnie opcję **Równoważenie obciążenia**.
8. W okienku **równoważenia obciążenia** wybierz algorytm równoważenia obciążenia (na przykład **co najmniej bieżące żądanie**).

    > [!NOTE]
    > Aby uzyskać więcej informacji o algorytmach równoważenia obciążenia i innych ustawieniach konfiguracji, zobacz [moduł routingu żądań aplikacji](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. W okienku **Akcje** kliknij pozycję **Zastosuj**.

Skonfigurowano podstawowe Równoważenie obciążenia dla serwerów w farmie serwerów. Jeśli przekierujesz cały ruch farmy sieci Web do serwera kontrolera, żądania będą dystrybuowane między serwerami w farmie zgodnie z dostępnością i wybranym algorytmem równoważenia obciążenia.

Aby uzyskać więcej informacji na temat konfigurowania równoważenia obciążenia z ARR, zobacz [moduł routingu żądań aplikacji](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitorowanie farmy serwerów

Kondycję farmy serwerów można monitorować w dowolnym momencie za pomocą Menedżera usług IIS na serwerze kontrolera. W okienku **połączenia** rozwiń farmę serwerów, a następnie kliknij pozycję **serwery**. W środkowym okienku zostaną wyświetlone podsumowanie każdego serwera w farmie wraz z dziennikiem śledzenia ostatnich działań.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Podsumowanie

Farma serwerów WFF powinna być teraz uruchomiona. Serwer podstawowy można skonfigurować do obsługi dowolnie&#x2014;wybranego podejścia do wdrożenia, aby uzyskać szczegółowe informacje&#x2014;, a konfiguracja zostanie zreplikowana na każdym serwerze pomocniczym w farmie serwerów.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej wskazówek na temat wszystkich aspektów konfigurowania i korzystania z programu WFF, zobacz witrynę sieci Web [Microsoft Web Farm Framework 2,0 dla usług IIS 7](https://go.microsoft.com/?linkid=9805129) .

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-database-server-for-web-deploy-publishing.md)
> [dalej](configuring-deployment-properties-for-a-target-environment.md)

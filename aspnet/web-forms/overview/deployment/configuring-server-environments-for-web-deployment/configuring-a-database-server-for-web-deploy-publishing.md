---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Konfigurowanie serwera bazy danych na potrzeby publikowania Web Deploy | Microsoft Docs
author: jrjlee
description: W tym temacie opisano sposób konfigurowania serwera bazy danych SQL Server 2008 R2 do obsługi wdrażania i publikowania w sieci Web. Zadania opisane w tym temacie to...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: ade3c1ba1c470092f512436f39b8831458408c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634618"
---
# <a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurowanie serwera bazy danych dla usługi publikowania Web Deploy

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania serwera bazy danych SQL Server 2008 R2 do obsługi wdrażania i publikowania w sieci Web.
> 
> Zadania opisane w tym temacie są wspólne dla każdego scenariusza&#x2014;wdrażania, bez względu na to, czy serwery sieci Web są skonfigurowane do korzystania z usługi zdalnego agenta wdrażania usług IIS (Web Deploy), programu obsługi Web Deploy lub wdrożenia w trybie offline lub aplikacji uruchomionej na jednym serwerze sieci Web lub w farmie serwerów. Sposób wdrażania bazy danych może ulec zmianie zgodnie z wymaganiami dotyczącymi zabezpieczeń i innymi kwestiami. Można na przykład wdrożyć bazę danych z przykładowymi danymi lub bez nich i można wdrożyć mapowania roli użytkownika lub skonfigurować je ręcznie po wdrożeniu. Jednak sposób konfigurowania serwera bazy danych pozostaje taki sam.

Nie trzeba instalować żadnych dodatkowych produktów ani narzędzi, aby skonfigurować serwer bazy danych do obsługi wdrażania w sieci Web. Przy założeniu, że serwer bazy danych i serwer sieci Web działają na różnych komputerach, wystarczy wykonać następujące czynności:

- Zezwól SQL Server na komunikację przy użyciu protokołu TCP/IP.
- Zezwalaj na ruch SQL Server przez zapory.
- Nadaj kontu komputera serwera sieci Web nazwę logowania SQL Server.
- Zamapuj logowanie do konta komputera na wszystkie wymagane role bazy danych.
- Nadaj kontu, które będą uruchamiać wdrożenie, SQL Server uprawnienia użytkownika i twórcę bazy danych.
- Aby można było obsługiwać powtarzające się wdrożenia, należy zamapować logowanie do konta wdrożenia na rolę bazy danych **właściciela\_DB** .

W tym temacie przedstawiono sposób wykonywania każdej z tych procedur. W zadaniach i przewodnikach w tym temacie przyjęto założenie, że rozpoczynasz pracę z domyślnym wystąpieniem SQL Server 2008 R2 uruchomionym w systemie Windows Server 2008 R2. Przed kontynuowaniem upewnij się, że:

- System Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są zainstalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.
- SQL Server 2008 R2 z dodatkiem Service Pack 1 oraz wszystkie dostępne aktualizacje.

Wystąpienie SQL Server musi zawierać tylko rolę **usługi aparatu bazy danych** , która jest włączana automatycznie podczas instalacji SQL Server. Jednak w celu ułatwienia konfiguracji i konserwacji zaleca się dołączenie **narzędzi do zarządzania — podstawowe** i narzędzia do **zarządzania — kompletne** role serwera.

> [!NOTE]
> Aby uzyskać więcej informacji na temat dołączania komputerów do domeny, zobacz [dołączanie komputerów do domeny i logowanie](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [Konfigurowanie statycznego adresu IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Aby uzyskać więcej informacji na temat instalowania SQL Server, zobacz [instalowanie SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).

## <a name="enable-remote-access-to-sql-server"></a>Włączanie dostępu zdalnego do SQL Server

SQL Server używa protokołu TCP/IP do komunikowania się z komputerami zdalnymi. Jeśli serwer bazy danych i serwer sieci Web znajdują się na różnych komputerach, należy:

- Skonfiguruj ustawienia sieci SQL Server, aby umożliwić komunikację za pośrednictwem protokołu TCP/IP.
- Skonfiguruj wszelkie zapory sprzętowe lub programowe w taki sposób, aby zezwalały na ruch TCP (oraz w niektórych przypadkach ruch UDP) na portach używanych przez wystąpienie SQL Server.

Aby umożliwić SQL Server komunikację za pośrednictwem protokołu TCP/IP, należy użyć SQL Server Configuration Manager do zmiany konfiguracji sieci dla wystąpienia SQL Server.

**Aby włączyć komunikację SQL Server przy użyciu protokołu TCP/IP**

1. W menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft SQL Server 2008 R2**, kliknij pozycję **Narzędzia konfiguracji**, a następnie kliknij pozycję **SQL Server Configuration Manager**.
2. W okienku widoku drzewa rozwiń węzeł **Konfiguracja sieci SQL Server**, a następnie kliknij pozycję **Protokoły dla MSSQLSERVER**.

   > [!NOTE]
   > Jeśli zainstalowano wiele wystąpień SQL Server, zobaczysz <strong>Protokoły dla</strong>elementu<em>[nazwa wystąpienia]</em> dla każdego wystąpienia. Należy skonfigurować ustawienia sieciowe na podstawie wystąpienia.
3. W okienku szczegółów kliknij prawym przyciskiem myszy wiersz **TCP/IP** , a następnie kliknij pozycję **Włącz**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. W oknie dialogowym **ostrzeżenia** kliknij przycisk **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Aby nowa konfiguracja sieci zaczęła obowiązywać, należy ponownie uruchomić usługę MSSQLSERVER. Można to zrobić w wierszu polecenia, z poziomu konsoli usługi lub z SQL Server Management Studio. W tej procedurze zostanie użyta SQL Server Management Studio.
6. Zamknij Menedżera konfiguracji programu SQL Server.
7. W menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft SQL Server 2008 R2**, a następnie kliknij pozycję **SQL Server Management Studio**.
8. W oknie dialogowym **łączenie z serwerem** w polu **Nazwa serwera** wpisz nazwę serwera bazy danych, a następnie kliknij przycisk **Połącz**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. W okienku **Eksplorator obiektów** kliknij prawym przyciskiem myszy węzeł serwera nadrzędnego (na przykład **TESTDB1**), a następnie kliknij pozycję **Uruchom ponownie**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. W oknie dialogowym **Management Studio Microsoft SQL Server** kliknij przycisk **tak**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Po ponownym uruchomieniu usługi Zamknij SQL Server Management Studio.

Aby zezwolić na ruch SQL Server przez zaporę, najpierw musisz wiedzieć, które porty są używane przez wystąpienie SQL Server. Zależą od tego, jak wystąpienie SQL Server zostało utworzone i skonfigurowane:

- *Domyślne wystąpienie* SQL Server nasłuchuje żądań (i odpowiada na nie) na porcie TCP 1433.
- *Nazwane wystąpienie* SQL Server nasłuchuje żądań (i reaguje na nie) na dynamicznie przypisanym porcie TCP.
- Jeśli usługa SQL Server Browser jest włączona, klienci mogą wysyłać zapytania do usługi na porcie UDP 1434, aby dowiedzieć się, który port TCP ma być używany dla określonego wystąpienia SQL Server. Ta usługa jest jednak często wyłączona ze względów bezpieczeństwa.

Przy założeniu, że używasz domyślnego wystąpienia SQL Server, musisz skonfigurować zaporę tak, aby zezwalała na ruch.

| Kierunek | Z portu | Do portu | Typ portu |
| --- | --- | --- | --- |
| Przychodzący | Dowolne | 1433 | TCP |
| Wychodzący | 1433 | Dowolne | TCP |

> [!NOTE]
> Technicznie komputery klienckie będą używać losowo przypisanego portu TCP między 1024 i 5000, aby komunikować się z SQL Server i można odpowiednio ograniczyć reguły zapory. Aby uzyskać więcej informacji na SQL Server portów i zapór, zobacz [numery portów TCP/IP wymagane do komunikacji z usługą SQL przez zaporę](https://go.microsoft.com/?linkid=9805125) i [instrukcje: Konfigurowanie serwera do nasłuchiwania na konkretnym porcie TCP (SQL Server Configuration Manager)](https://msdn.microsoft.com/library/ms177440.aspx).

W większości środowisk systemu Windows Server prawdopodobnie trzeba będzie skonfigurować zaporę systemu Windows na serwerze bazy danych. Domyślnie Zapora systemu Windows zezwala na cały ruch wychodzący, chyba że reguła zabrania go. Aby umożliwić serwerowi sieci Web dostęp do bazy danych, należy skonfigurować regułę ruchu przychodzącego, która zezwala na ruch TCP na numer portu, którego używa wystąpienie SQL Server. Jeśli używasz domyślnego wystąpienia SQL Server, możesz użyć następnej procedury, aby skonfigurować tę regułę.

**Aby skonfigurować zaporę systemu Windows w celu zezwalania na komunikację z domyślnym wystąpieniem SQL Server**

1. Na serwerze bazy danych w menu **Start** wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij pozycję **Zapora systemu Windows z zabezpieczeniami zaawansowanymi**.
2. W okienku widoku drzewa kliknij pozycję **reguły ruchu przychodzącego**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. W okienku **Akcje** w obszarze **reguły ruchu przychodzącego**kliknij pozycję **Nowa reguła**.
4. W Kreatorze nowej reguły ruchu przychodzącego na stronie **Typ reguły** wybierz pozycję **port**, a następnie kliknij przycisk **dalej**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Na stronie **Protokół i porty** upewnij się, że wybrano opcję **TCP** i w polu **określone porty lokalne** wpisz **1433**, a następnie kliknij przycisk **dalej**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Na stronie **Akcja** pozostaw zaznaczone pole wyboru **Zezwalaj na połączenie** , a następnie kliknij przycisk **dalej**.
7. Na stronie **profil** pozostaw wybraną **domenę** , wyczyść pola wyboru **prywatne** i **publiczne** , a następnie kliknij przycisk **dalej**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Na stronie **Nazwa** nadaj regule odpowiednio nazwę opisową (na przykład **SQL Server domyślne wystąpienie — dostęp do sieci**), a następnie kliknij przycisk **Zakończ**.

Aby uzyskać więcej informacji na temat konfigurowania zapory systemu Windows dla SQL Server, szczególnie jeśli chcesz komunikować się z SQL Server za pośrednictwem portów niestandardowych lub dynamicznych, zobacz [How to: Configure a Zapora systemu Windows na potrzeby dostępu do aparatu bazy danych](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurowanie logowań i uprawnień bazy danych

Podczas wdrażania aplikacji sieci Web do Internet Information Services (IIS) aplikacja jest uruchamiana przy użyciu tożsamości puli aplikacji. W środowisku domeny tożsamości puli aplikacji używają konta komputera serwera, na którym są uruchamiane w celu uzyskania dostępu do zasobów sieciowych. Konta komputerów przyjmują postać <em>[Domain Name]</em><strong>\</strong ><em>[nazwa komputera]</em> <strong>$</strong> &#x2014;na przykład <strong>FABRIKAM\TESTWEB1 $</strong>. Aby umożliwić aplikacji sieci Web dostęp do bazy danych w sieci, należy:

- Dodaj nazwę logowania dla konta komputera serwera sieci Web do wystąpienia SQL Server.
- Zamapuj logowanie do konta komputera na wszystkie wymagane role bazy danych (zazwyczaj **db\_DataReader** i **DB\_datawriteer**).

Jeśli aplikacja sieci Web jest uruchomiona w farmie serwerów, a nie na pojedynczym serwerze, należy powtórzyć te procedury dla każdego serwera sieci Web w farmie serwerów.

> [!NOTE]
> Aby uzyskać więcej informacji na temat tożsamości puli aplikacji i uzyskiwania dostępu do zasobów sieciowych, zobacz [tożsamość puli aplikacji](https://go.microsoft.com/?linkid=9805123).

Te zadania można podejścia na różne sposoby. Aby utworzyć identyfikator logowania, można wykonać jedną z:

- Ręcznie Utwórz identyfikator logowania na serwerze bazy danych przy użyciu języka Transact-SQL lub SQL Server Management Studio.
- Użyj projektu serwera SQL Server 2008 w programie Visual Studio, aby utworzyć i wdrożyć nazwę logowania.

Logowanie SQL Server jest obiektem na poziomie serwera, a nie obiektem na poziomie bazy danych, więc nie zależy od bazy danych, którą chcesz wdrożyć. W związku z tym można utworzyć identyfikator logowania w dowolnym momencie, a najprostszym podejściem jest często ręczne utworzenie nazwy logowania na serwerze bazy danych przed rozpoczęciem wdrażania baz danych. Przy użyciu następnej procedury można utworzyć identyfikator logowania w SQL Server Management Studio.

**Aby utworzyć identyfikator logowania SQL Server dla konta komputera serwera sieci Web**

1. Na serwerze bazy danych w menu **Start** wskaż polecenie **Wszystkie programy**, kliknij pozycję **Microsoft SQL Server 2008 R2**, a następnie kliknij pozycję **SQL Server Management Studio**.
2. W oknie dialogowym **łączenie z serwerem** w polu **Nazwa serwera** wpisz nazwę serwera bazy danych, a następnie kliknij przycisk **Połącz**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. W okienku **Eksplorator obiektów** kliknij prawym przyciskiem myszy pozycję **zabezpieczenia**, wskaż polecenie **Nowy**, a następnie kliknij pozycję **Zaloguj**.
4. W oknie dialogowym **Logowanie — nowy** w polu **Nazwa logowania** wpisz nazwę konta komputera serwera sieci Web (na przykład **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Kliknij przycisk **OK**.

Na tym etapie serwer bazy danych jest gotowy do publikowania Web Deploy. Jednak wdrożone rozwiązania nie będą działały, dopóki nie zostanie zmapowane logowanie do konta komputera w wymaganych rolach bazy danych. Mapowanie logowania do ról bazy danych wymaga dużej liczby myśli, ponieważ nie można mapować ról do momentu, gdy baza danych zostanie wdrożona. Aby zmapować logowanie do konta komputera do wymaganych ról bazy danych, można wykonać jedną z następujących czynności:

- Ręcznie Przypisz role bazy danych do nazwy logowania po wdrożeniu bazy danych po raz pierwszy.
- Użyj skryptu powdrożeniowego, aby przypisać role bazy danych do nazwy logowania.

Aby uzyskać więcej informacji na temat automatyzowania tworzenia identyfikatorów logowania i mapowania ról bazy danych, zobacz [wdrażanie członkostw ról bazy danych w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Można też użyć następnej procedury, aby ręcznie zmapować logowanie do konta komputera do wymaganej roli bazy danych. Należy pamiętać, że nie można wykonać tej procedury dopiero *po* wdrożeniu bazy danych.

**Aby zamapować role bazy danych na logowanie do konta komputera serwera sieci Web**

1. Otwórz SQL Server Management Studio jak wcześniej.
2. W okienku **Eksplorator obiektów** rozwiń węzeł **zabezpieczenia** , rozwiń węzeł **logowania** , a następnie kliknij dwukrotnie pozycję Logowanie do konta komputera (na przykład **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. W oknie dialogowym **Właściwości logowania** kliknij pozycję **Mapowanie użytkownika**.
4. W tabeli **Użytkownicy zamapowane na tę nazwę logowania** wybierz nazwę bazy danych (na przykład **ContactManager**).
5. Na liście **członkostwo w roli bazy danych dla:** *[nazwa bazy danych]* wybierz wymagane uprawnienia. W przypadku przykładowego rozwiązania Contact Manager należy wybrać role **bazy danych\_DataReader** i **baza danych\_** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Kliknij przycisk **OK**.

Podczas ręcznego mapowania ról bazy danych jest często większa niż odpowiednia dla środowisk testowych, ale jest mniej pożądane dla wdrożeń zautomatyzowanych lub jednym kliknięciem w środowisku przejściowym lub produkcyjnym. Więcej informacji na temat automatyzowania tego rodzaju zadania za pomocą skryptów po wdrożeniu można znaleźć w temacie [wdrażanie członkostw roli bazy danych w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Aby uzyskać więcej informacji na temat projektów serwera i projektów bazy danych, zobacz [Visual Studio 2010 SQL Server projects Database](https://msdn.microsoft.com/library/ff678491.aspx).

## <a name="configure-permissions-for-the-deployment-account"></a>Konfigurowanie uprawnień dla konta wdrożenia

Jeśli konto, którego będziesz używać do uruchamiania wdrożenia, nie jest administratorem SQL Server, należy również utworzyć identyfikator logowania dla tego konta. Aby można było utworzyć bazę danych, konto musi być członkiem roli serwera **dbcreator** lub mieć równoważne uprawnienia.

> [!NOTE]
> W przypadku używania Web Deploy lub VSDBCMD do wdrożenia bazy danych można użyć poświadczeń systemu Windows lub poświadczeń SQL Server (Jeśli wystąpienie SQL Server jest skonfigurowane do obsługi uwierzytelniania w trybie mieszanym). W następnej procedurze przyjęto założenie, że użytkownik chce używać poświadczeń systemu Windows, ale nie ma żadnego zatrzymywania podania nazwy użytkownika SQL Server i hasła w parametrach połączenia podczas konfigurowania wdrożenia.

**Aby skonfigurować uprawnienia dla konta wdrożenia**

1. Otwórz SQL Server Management Studio jak wcześniej.
2. W okienku **Eksplorator obiektów** kliknij prawym przyciskiem myszy pozycję **zabezpieczenia**, wskaż polecenie **Nowy**, a następnie kliknij pozycję **Zaloguj**.
3. W oknie dialogowym **login — nowe** w polu **Nazwa logowania** wpisz nazwę konta wdrożenia (na przykład **FABRIKAM\matt**).
4. W okienku **Wybierz stronę** kliknij pozycję **role serwera**.
5. Wybierz pozycję **dbcreator**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Aby obsłużyć kolejne wdrożenia, należy również dodać konto wdrażania do roli **właściciela bazy danych\_** w bazie danych programu po pierwszym wdrożeniu. Dzieje się tak, ponieważ w przypadku kolejnych wdrożeń modyfikujących schemat istniejącej bazy danych zamiast tworzenia nowej bazy danych. Zgodnie z opisem w poprzedniej sekcji nie można dodać użytkownika do roli bazy danych, dopóki baza danych nie zostanie utworzona z oczywistych powodów.

**Aby zmapować logowanie do konta wdrożenia do roli bazy danych właściciela\_**

1. Otwórz SQL Server Management Studio jak wcześniej.
2. W oknie **Eksplorator obiektów** rozwiń węzeł **zabezpieczenia** , rozwiń węzeł **logowania** , a następnie kliknij dwukrotnie pozycję Logowanie do konta komputera (na przykład **FABRIKAM\matt**).
3. W oknie dialogowym **Właściwości logowania** kliknij pozycję **Mapowanie użytkownika**.
4. W tabeli **Użytkownicy zamapowane na tę nazwę logowania** wybierz nazwę bazy danych (na przykład **ContactManager**).
5. Z listy **członkostwo w roli bazy danych dla:** *[nazwa bazy danych]* wybierz rolę **właściciela\_bazy danych** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Kliknij przycisk **OK**.

## <a name="conclusion"></a>Podsumowanie

Serwer bazy danych powinien teraz być gotowy do akceptowania zdalnych wdrożeń baz danych i zezwalania zdalnym serwerom sieci Web usług IIS na dostęp do baz danych. Przed podjęciem próby wdrożenia baz danych i korzystania z nich warto sprawdzić następujące kluczowe punkty:

- Czy skonfigurowano SQL Server do akceptowania zdalnych połączeń TCP/IP?
- Czy skonfigurowano zapory zezwalające na ruch SQL Server?
- Czy utworzono logowanie do konta komputera dla każdego serwera sieci Web, który będzie miał dostęp SQL Server?
- Czy wdrożenie bazy danych obejmuje skrypt służący do tworzenia mapowań roli użytkownika, czy też należy je utworzyć ręcznie po wdrożeniu bazy danych po raz pierwszy?
- Czy utworzono identyfikator logowania dla konta wdrożenia i został on dodany do roli serwera **dbcreator** ?

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące wdrażania projektów bazy danych, zobacz [wdrażanie projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać wskazówki dotyczące tworzenia członkostw roli bazy danych, uruchamiając skrypt powdrożeniowy, zobacz [wdrażanie członkostw roli bazy danych w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Aby uzyskać informacje na temat sposobu zaspokajania unikatowych wyzwań związanych z wdrażaniem, które są przeznaczone dla baz danych, zobacz [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [dalej](creating-a-server-farm-with-the-web-farm-framework.md)

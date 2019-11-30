---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie w środowisku produkcyjnym — 7 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: db838633accdedd7c0693b126a007e254ca681e4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74627225"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: wdrażanie w środowisku produkcyjnym — 7 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku skonfigurujesz konto z dostawcą hostingu i wdrażasz aplikację sieci Web ASP.NET w środowisku produkcyjnym przy użyciu funkcji publikowania po jednym kliknięciu programu Visual Studio.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Wybieranie dostawcy hostingu

W przypadku aplikacji firmy Contoso University i tej serii samouczków potrzebny jest dostawca obsługujący ASP.NET 4 i Web Deploy. Określona firma hostingu została wybrana, aby samouczki mogły zilustrować pełne środowisko wdrażania w działającej witrynie sieci Web. Każda firma hostingowa udostępnia różne funkcje, a środowisko wdrażania na serwerach różni się nieco. Jednak proces opisany w tym samouczku jest typowy dla całego procesu. Dostawca hostingu używany w tym samouczku, Cytanium.com, jest jednym z dostępnych, a jego użycie w tym samouczku nie stanowi adnotacji ani rekomendacji.

Gdy wszystko będzie gotowe do wybrania własnego dostawcy hostingu, można porównać funkcje i ceny w [galerii dostawców](https://www.microsoft.com/web/hosting) w witrynie Microsoft.com/Web.

## <a name="creating-an-account"></a>Tworzenie konta

Utwórz konto na wybranym dostawcy. Jeśli obsługa pełnej SQL Serverj bazy danych jest dodaną dodatkową, nie trzeba jej wybierać dla tego samouczka, ale będzie ona potrzebna [SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) w dalszej części tej serii.

W przypadku tych samouczków nie trzeba rejestrować nowej nazwy domeny. Można testować, aby zweryfikować pomyślne wdrożenie przy użyciu tymczasowego adresu URL przypisanego do witryny przez dostawcę.

Po utworzeniu konta zazwyczaj otrzymujesz powitalną wiadomość e-mail, która zawiera wszystkie informacje potrzebne do wdrożenia lokacji i zarządzania nią. Informacje wysyłane przez dostawcę usług hostingowych będą wyglądać podobnie jak w tym miejscu. Cytanium powitalną wiadomość e-mail, która jest wysyłana do nowych właścicieli konta, zawiera następujące informacje:

- Adres URL witryny panelu sterowania dostawcy, w którym można zarządzać ustawieniami witryny. Podane identyfikator i hasło są zawarte w tej części powitalnej wiadomości e-mail, aby ułatwić odwołanie. (Obie zostały zmienione na wartość demonstracyjną dla tej ilustracji).

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Domyślna wersja .NET Framework i informacje o sposobie jego zmiany. Wiele witryn hostingu domyślnie to 2,0, które współdziałają z aplikacjami ASP.NET, które są przeznaczone dla .NET Framework 2,0, 3,0 lub 3,5. Jednakże firma Contoso University to aplikacja .NET Framework 4, dlatego należy zmienić to ustawienie. (Dla aplikacji ASP.NET 4,5 Użyj ustawienia programu .NET 4,0).

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Tymczasowy adres URL, za pomocą którego można uzyskać dostęp do witryny sieci Web. Po utworzeniu tego konta jako nazwę istniejącej domeny wprowadzono wartość "contosouniversity.com". W związku z tym tymczasowy adres URL jest `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informacje na temat sposobu konfigurowania baz danych i parametrów połączenia, które są potrzebne w celu uzyskania dostępu do nich:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informacje o narzędziach i ustawieniach wdrażania witryny. (Wiadomość e-mail z Cytanium zawiera również informacje o programie WebMatrix, które zostały pominięte w tym miejscu).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Ustawianie wersji .NET Framework

Powitalna wiadomość e-mail Cytanium zawiera link do instrukcji dotyczących zmiany wersji .NET Framework. Te instrukcje wyjaśniają, że można to zrobić za pomocą panelu sterowania Cytanium. Inni dostawcy mają witryny panelu sterowania, które wyglądają inaczej lub mogą być w inny sposób poinstruować.

Przejdź do adresu URL panelu sterowania. Po zalogowaniu się przy użyciu nazwy użytkownika i hasła zobaczysz Panel sterowania.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

W polu **miejsca hostingu** przytrzymaj wskaźnik myszy nad ikoną sieci Web i wybierz pozycję **witryny sieci Web** z menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

W polu **witryny sieci Web** kliknij pozycję **contosouniversity.com** (nazwa witryny, która została użyta podczas tworzenia konta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

W oknie **Właściwości witryny sieci Web** wybierz kartę **rozszerzenia** .

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Zmień ASP.NET z **potoku zintegrowanego 2,0** na **4,0 (potok zintegrowany)** , a następnie kliknij przycisk **Aktualizuj**.

## <a name="publishing-to-the-hosting-provider"></a>Publikowanie w dostawcy hostingu

Powitalna wiadomość e-mail od dostawcy hostingu obejmuje wszystkie ustawienia, które są potrzebne w celu opublikowania projektu, i można wprowadzić te informacje ręcznie w profilu publikacji. Ale będziesz używać łatwiejszej i mniejszej metody podatności na błędy, aby skonfigurować wdrożenie dla dostawcy: Pobierz plik *. publishsettings* i zaimportuj go do profilu publikacji.

W przeglądarce przejdź do panelu sterowania Cytanium i wybierz pozycję **Sieć Web** , a następnie wybierz pozycję **witryny sieci Web.**

![Panel sterowania — Wybieranie witryn sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Wybierz witrynę sieci Web **contosouniversity.com** .

![Panel sterowania wybierania contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Wybierz kartę **Publikowanie w sieci Web** .

![Karta publikowanie w sieci Web w panelu sterowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Utwórz poświadczenia, które mają być używane na potrzeby publikowania w sieci Web, wprowadzając nazwę użytkownika i hasło. Możesz wprowadzić te same poświadczenia, których używasz do logowania się w panelu sterowania. Następnie kliknij pozycję **Włącz**.

![Panel sterowania — Tworzenie poświadczeń publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Kliknij pozycję **Pobierz profil publikowania dla tej witryny sieci Web**.

![Panel sterowania Pobieranie profilu publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Gdy zostanie wyświetlony monit o otwarcie lub zapisanie pliku, Zapisz go.

![Zapisz plik profilu publikowania](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

W **Eksplorator rozwiązań** w programie Visual Studio kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz polecenie **Publikuj**. Zostanie otwarte okno dialogowe **Publikowanie sieci Web** na karcie **Podgląd** z wybranym profilem **testowym** , ponieważ jest to ostatni użyty profil.

Wybierz kartę **profil** , a następnie kliknij pozycję **Importuj**.

![Przycisk importowania Kreatora publikacji sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

W oknie dialogowym **Importowanie ustawień publikowania** wybierz pobrany plik *. publishsettings* , a następnie kliknij przycisk **Otwórz**. Kreator przechodzi do karty połączenie ze wszystkimi wypełnionymi polami.

![Karta połączenia Kreatora publikacji sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Plik. publishsettings umieszcza planowany stały adres URL witryny w polu docelowy adres URL, ale jeśli nie zakupiono jeszcze tej domeny, Zastąp wartość tymczasowym adresem URL. W tym przykładzie adres URL jest  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* Jedynym celem tego pola jest określenie, który adres URL zostanie otwarty w przeglądarce po pomyślnym zakończeniu wdrażania. Jeśli pozostawisz to pole puste, jedyną konsekwencją jest to, że przeglądarka nie zostanie uruchomiona automatycznie po wdrożeniu.

Kliknij pozycję **Sprawdź poprawność połączenia** , aby sprawdzić, czy ustawienia są poprawne i czy można nawiązać połączenie z serwerem. Jak pokazano wcześniej, zielony znacznik wyboru weryfikuje, czy połączenie zostało nawiązane pomyślnie.

Po kliknięciu przycisku Weryfikuj połączenie może zostać wyświetlone okno dialogowe **błąd certyfikatu** . Jeśli tak jest, sprawdź, czy nazwa serwera jest oczekiwana. Jeśli tak jest, wybierz pozycję **Zapisz ten certyfikat dla przyszłych sesji programu Visual Studio** , a następnie kliknij przycisk **Akceptuj**. (Ten błąd oznacza, że dostawca hostingu zdecydował się uniknąć wydatków zakupu certyfikatu SSL dla adresu URL, który jest wdrażany. Jeśli wolisz nawiązać bezpieczne połączenie przy użyciu ważnego certyfikatu, skontaktuj się z dostawcą hostingu.

![Błąd certyfikatu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Kliknij przycisk **Dalej**.

W sekcji **bazy danych** na karcie **Ustawienia** wprowadź te same wartości, które zostały wprowadzone dla profilu publikowania testowego. Parametry połączenia, które są potrzebne, znajdują się na listach rozwijanych.

- W polu Parametry połączenia dla **SchoolContext** wybierz pozycję `Data Source=|DataDirectory|School-Prod.sdf`
- W obszarze **SchoolContext**wybierz pozycję **Zastosuj migracje Code First**.
- W polu Parametry połączenia dla **DefaultConnection**wybierz pozycję `Data Source=|DataDirectory|aspnet-Prod.sdf`
- W obszarze **DefaultConnection**pozostaw wyczyszczone polecenie **Aktualizuj bazę danych** .

![Karta Ustawienia Kreatora publikacji sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Kliknij przycisk **Dalej**.

Na karcie **Podgląd** kliknij pozycję **Rozpocznij podgląd** , aby wyświetlić listę plików, które zostaną skopiowane. Zostanie wyświetlona ta sama lista, która została wcześniej zamieszczona podczas wdrażania programu w usługach IIS na komputerze lokalnym.

Przed opublikowaniem Zmień nazwę profilu, tak aby plik transformacji Web. Production. config został zastosowany. Wybierz kartę **profil** , a następnie kliknij pozycję **Zarządzaj profilami**.

![Kreator publikowania sieci Web — zarządzanie profilami](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

W oknie dialogowym **Edytowanie profilów publikowania w sieci Web** wybierz profil produkcyjny, kliknij przycisk **Zmień**nazwę, a następnie zmień wartość w polu Nazwa profilu na produkcja. Następnie kliknij przycisk **Zamknij**.

![Okno dialogowe Edytowanie profilów publikowania w sieci Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Kliknij przycisk **Publikuj**.

Aplikacja zostanie opublikowana w dostawcy hostingu. Wyniki są wyświetlane w oknie **dane wyjściowe** .

![Okno danych wyjściowych po wdrożeniu](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Przeglądarka automatycznie otworzy adres URL wprowadzony w polu **docelowy adres URL** na karcie **połączenie** kreatora **publikacji w sieci Web** . Po uruchomieniu witryny w programie Visual Studio zostanie wyświetlona ta sama Strona główna, z wyjątkiem tego, że na pasku tytułu nie ma wskaźnika środowiska "(test)" lub "(dev)". Wskazuje to, że transformacja środowiska *Web. config* jest działała poprawnie.

> [!NOTE]
> Jeśli nadal widzisz "(test)" w nagłówku, Usuń folder *obj* z projektu ContosoUniversity i Wdróż ponownie. W wersji wstępnej oprogramowania wcześniej zastosowany plik transformacji (Web. test. config) mógł zostać zastosowany ponownie, mimo że jest używany profil produkcyjny.

[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Przed uruchomieniem strony, która powoduje dostęp do bazy danych, upewnij się, że program ELMAH będzie mógł rejestrować wszystkie błędy, które wystąpiły.

## <a name="setting-folder-permissions-for-elmah"></a>Ustawianie uprawnień do folderu dla programu ELMAH

Pamiętaj, że w tej serii należy upewnić się, że aplikacja ma uprawnienia do zapisu w tym folderze w aplikacji, w której program ELMAH przechowuje pliki dzienników błędów. Po wdrożeniu w usługach IIS lokalnie na komputerze, należy ręcznie ustawić te uprawnienia. W tej sekcji zobaczysz, jak ustawić uprawnienia w Cytanium. (Niektórzy dostawcy hostingu mogą nie pozwalać na to zrobić; mogą zaoferować jeden lub więcej wstępnie zdefiniowanych folderów z uprawnieniami do zapisu. W takim przypadku należy zmodyfikować aplikację tak, aby korzystała z określonych folderów.

Uprawnienia do folderów można ustawić w panelu sterowania Cytanium. Przejdź do adresu URL panelu sterowania i wybierz pozycję **Menedżer plików**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

W oknie **Menedżer plików** wybierz pozycję **contosouniversity.com** , a następnie katalogu **wwwroot** , aby wyświetlić folder główny aplikacji. Kliknij ikonę kłódki obok pozycji **ELMAH**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

W oknie uprawnienia do **folderu**/**plików** zaznacz pola wyboru **Odczyt** i **zapis** dla **contosouniversity.com** i kliknij pozycję **Ustaw uprawnienia**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Upewnij się, że obiekt ELMAH ma dostęp do zapisu do folderu *ELMAH* , powodując błąd, a następnie wyświetlając raport o błędach ELMAH. Zażądaj nieprawidłowego adresu URL, takiego jak *Studentsxxx. aspx*. Tak jak wcześniej zobaczysz stronę *GenericErrorPage. aspx* . Kliknij link **Wyloguj** , a następnie uruchom polecenie *ELMAH. axd*. Najpierw otrzymujesz stronę **logowania** , która sprawdza, czy pomyślnie dodaliśmy transformację programu *Web. config* . Po zalogowaniu zostanie wyświetlony Raport z wyświetlonym błędem.

[![ELMAH. axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Testowanie w środowisku produkcyjnym

Uruchom stronę **uczniów** . Aplikacja próbuje uzyskać dostęp do bazy danych szkoły po raz pierwszy, która wyzwala Migracje Code First, aby utworzyć bazę danych. Gdy strona jest wyświetlana po opóźnieniu momentu, pokazuje, że nie ma studentów.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Uruchom stronę **instruktorów** , aby sprawdzić, czy dane inicjatora pomyślnie wstawiono dane instruktora do bazy danych.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Podobnie jak w środowisku testowym, należy sprawdzić, czy aktualizacje bazy danych działają w środowisku produkcyjnym, ale zazwyczaj nie należy wprowadzać danych testowych do produkcyjnej bazy danych. Na potrzeby tego samouczka będziesz używać tej samej metody, która została przetestowana. Jednak w rzeczywistej aplikacji warto znaleźć metodę, która sprawdza, czy aktualizacje bazy danych zostały pomyślnie zakończone, bez wprowadzania danych testowych do produkcyjnej bazy danych. W niektórych aplikacjach warto dodać coś, a następnie go usunąć.

Dodaj studenta, a następnie Wyświetl dane wprowadzone na stronie **uczniów** , aby sprawdzić, czy możesz zaktualizować dane w bazie danych.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Sprawdź, czy reguły autoryzacji działają prawidłowo, wybierając pozycję **Aktualizuj kredyty** z menu **kursy** . Zostanie wyświetlona strona **logowania** . Wprowadź poświadczenia konta administratora, kliknij przycisk **Zaloguj**, a zostanie wyświetlona strona **kredyty aktualizacji** .

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Jeśli logowanie powiedzie się, zostanie wyświetlona strona **kredyty dotyczące aktualizacji** . Oznacza to, że baza danych członkostwa ASP.NET (z jednym kontem administratora) została pomyślnie wdrożona.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Pomyślnie wdrożono i przetestowano witrynę i jest ona dostępna publicznie za pośrednictwem Internetu.

## <a name="creating-a-more-reliable-test-environment"></a>Tworzenie bardziej niezawodnego środowiska testowego

Jak wyjaśniono w samouczku [wdrażanie do środowiska testowego](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) , najbardziej niezawodnym środowiskiem testowym będzie drugie konto u dostawcy hostingu, które jest takie samo jak konto produkcyjne. Jest to droższe niż używanie lokalnych usług IIS jako środowiska testowego, ponieważ konieczne jest zarejestrowanie się w celu utworzenia drugiego konta hostingu. Jeśli jednak uniemożliwiają one błędy lub przestoje w środowisku produkcyjnym, można zdecydować, że koszt jest cenny.

Większość procesu tworzenia i wdrażania na koncie testowym jest podobna do wdrożonych w środowisku produkcyjnym:

- Utwórz plik transformacji pliku *Web. config* .
- Utwórz konto u dostawcy hostingu.
- Utwórz nowy profil publikowania i Wdróż go na koncie testowym.

### <a name="preventing-public-access-to-the-test-site"></a>Uniemożliwianie publicznego dostępu do witryny testowej

Ważnym zagadnieniem dotyczącym konta testowego jest fakt, że będzie on aktywny w Internecie, ale nie chcesz, aby był on używany. Aby zachować prywatną lokację, można użyć co najmniej jednej z następujących metod:

- Skontaktuj się z dostawcą hostingu, aby ustawić reguły zapory zezwalające na dostęp do witryny testowej tylko z adresów IP używanych do testowania.
- Pomniejsz adres URL, tak aby nie był podobny do adresu URL witryny publicznej.
- Użyj pliku *robots. txt* , aby upewnić się, że aparaty wyszukiwania nie przeszukają witryny testowej i nie będą przeszukiwane w wynikach wyszukiwania.

Pierwszy z tych metod jest najbardziej bezpieczny, ale procedura dla programu jest specyficzna dla każdego dostawcy hostingu i nie zostanie uwzględniona w tym samouczku. Jeśli ustawisz dostawcę hostingu w taki sposób, aby zezwalał na dostęp do adresu URL konta testowego tylko adres IP, teoretycznie nie musisz martwić się o aparaty wyszukiwania przeszukiwane. Jednak nawet w takim przypadku wdrożenie pliku *robots. txt* jest dobrym pomysłem jako kopii zapasowej w przypadku, gdy reguła zapory jest kiedykolwiek wyłączona.

Plik *robots. txt* znajduje się w folderze projektu i powinien zawierać następujący tekst:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

Wiersz `User-agent` informuje aparaty wyszukiwania, że reguły w pliku mają zastosowanie do wszystkich przeszukiwań sieci Web aparatu wyszukiwania (robotów), a wiersz `Disallow` określa, że nie należy przeszukiwać stron w witrynie.

Prawdopodobnie aparaty wyszukiwania wykazują witrynę produkcyjną, dlatego należy wykluczyć ten plik z wdrożenia produkcyjnego. Aby to zrobić, zobacz sekcję czy **można wykluczyć określone pliki lub foldery z wdrożenia?** w artykule [ASP.NET wdrażanie projektu aplikacji sieci Web — często zadawane pytania](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Upewnij się, że określisz tylko wykluczenie dla profilu publikowania produkcji.

Tworzenie drugiego konta hostingu jest podejściem do pracy ze środowiskiem testowym, które nie jest wymagane, ale może być cennym dodatkowym kosztem. W poniższych samouczkach będziesz nadal korzystać z usług IIS jako środowiska testowego.

W następnym samouczku należy zaktualizować kod aplikacji i wdrożyć zmiany w środowisku testowym i produkcyjnym.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)

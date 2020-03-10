---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie w środowisku produkcyjnym | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: ddc3d15f0436c4c3a24491cf0377111768da67df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632784"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie w środowisku produkcyjnym

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku skonfigurujesz konto Microsoft Azure, tworzysz środowiska przejściowe i produkcyjne oraz wdrażasz aplikację sieci Web ASP.NET w środowiskach tymczasowych i produkcyjnych przy użyciu funkcji publikowania po jednym kliknięciu programu Visual Studio.

Jeśli wolisz, możesz wdrożyć program w ramach dostawcy hostingu innej firmy. Większość procedur opisanych w tym samouczku jest taka sama dla dostawcy hostingu lub platformy Azure, z tą różnicą, że każdy dostawca ma własny interfejs użytkownika do zarządzania kontami i witrynami sieci Web. Dostawcę hostingu można znaleźć w [galerii dostawców](https://www.microsoft.com/web/hosting) w witrynie sieci Web Microsoft.com.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu strony rozwiązywania problemów w tej serii samouczków.

## <a name="get-a-microsoft-azure-account"></a>Pobierz konto Microsoft Azure

Jeśli nie masz jeszcze konta platformy Azure, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Tworzenie środowiska przejściowego

> [!NOTE]
> Ponieważ ten samouczek został zapisany, Azure App Service dodano nową funkcję w celu zautomatyzowania wielu procesów tworzenia środowisk przejściowych i produkcyjnych. Zobacz [Konfigurowanie środowisk przejściowych dla aplikacji sieci Web w Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).

Jak wyjaśniono w [samouczku wdrażanie do środowiska testowego](deploying-to-iis.md), najbardziej niezawodnym środowiskiem testowym jest witryna sieci Web dostawcy hostingu, która jest taka sama jak produkcyjna witryna sieci Web. W przypadku wielu dostawców hostingu należy rozważyć korzyści wynikające z ponoszenia znaczących kosztów, ale na platformie Azure można utworzyć dodatkową bezpłatną aplikację sieci Web jako aplikację przemieszczania. Potrzebna jest również baza danych, a dodatkowe koszty związane z tym kosztem dla produkcyjnej bazy danych będą mieć wartość Brak lub minimum. Na platformie Azure płacisz za ilość używanego magazynu bazy danych, a nie dla każdej bazy danych, a ilość dodatkowego magazynu, który będzie używany w ramach przemieszczania, będzie minimalny.

Jak wyjaśniono w [samouczku wdrażanie do środowiska testowego](deploying-to-iis.md), w fazie przygotowywania i produkcji można wdrożyć dwie bazy danych w jednej bazie danych. Jeśli chcesz je oddzielić, proces będzie taki sam, z wyjątkiem sytuacji, gdy utworzysz dodatkową bazę danych dla każdego środowiska i wybierasz prawidłowy ciąg docelowy dla każdej bazy danych podczas tworzenia profilu publikowania.

W tej części samouczka utworzysz aplikację sieci Web i bazę danych do użycia w środowisku przejściowym, a wdrożenie zostanie wdrożone w celu przygotowania i przetestowania w środowisku produkcyjnym.

> [!NOTE]
> Poniższe kroki pokazują, jak utworzyć aplikację sieci Web w Azure App Service przy użyciu portalu zarządzania Azure. W najnowszej wersji zestawu Azure SDK można to zrobić również bez opuszczania programu Visual Studio, używając Eksplorator serwera. W Visual Studio 2013 można również utworzyć aplikację sieci Web bezpośrednio z okna dialogowego publikowanie. Aby uzyskać więcej informacji, zobacz [Tworzenie aplikacji sieci web ASP.NET w Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)

1. Na [platformie Azure Portal zarządzania](https://manage.windowsazure.com/)kliknij pozycję **websites**, a następnie kliknij pozycję **New (nowy**).
2. Kliknij pozycję **Witryna sieci Web**, a następnie kliknij pozycję **Tworzenie niestandardowe**.

    **Nowa witryna sieci Web — Kreator tworzenia niestandardowego** zostanie otwarty. **Niestandardowy Kreator tworzenia** umożliwia tworzenie witryny sieci Web i bazy danych w tym samym czasie.
3. W kroku **Tworzenie witryny sieci Web** wprowadź ciąg w polu **adresu URL** , który będzie używany jako unikatowy adres URL środowiska przejściowego aplikacji. Na przykład wprowadź ContosoUniversity-staging123 (w tym liczby losowe na końcu, aby była unikatowa w przypadku, gdy podejmowana jest ContosoUniversity — przemieszczenie).

    Pełny adres URL będzie zawierać dane wprowadzane tutaj oraz sufiks wyświetlany obok pola tekstowego.
4. Z listy rozwijanej **region** wybierz region znajdujący się najbliżej Ciebie.

    To ustawienie określa centrum danych, w którym będzie działać aplikacja sieci Web.
5. Z listy rozwijanej **baza danych** wybierz pozycję **Utwórz nową bazę danych SQL**.
6. W polu **nazwa parametrów połączenia bazy danych** pozostaw wartość domyślną *DefaultConnection*.
7. Kliknij strzałkę wskazującą w prawo u dołu pola.

    Na poniższej ilustracji przedstawiono okno dialogowe **Tworzenie witryny sieci Web** z przykładowymi wartościami. Wprowadzony adres URL i region będą się różnić.

    ![Utwórz krok witryny sieci Web](deploying-to-production/_static/image1.png)

    Kreator przechodzi do kroku **Określ ustawienia bazy danych** .
8. W polu **Nazwa** wprowadź *ContosoUniversity* oraz liczbę losową, aby określić ją jako unikatową, na przykład *ContosoUniversity123*.
9. W polu **serwer** wybierz pozycję **nowy serwer SQL Database**.
10. Wprowadź nazwę i hasło administratora.

    Nie wprowadzasz tutaj istniejącej nazwy i hasła. Podajesz nową nazwę i hasło, które definiujesz teraz do użycia w przyszłości podczas uzyskiwania dostępu do bazy danych.
11. W polu **region** wybierz ten sam region, który został wybrany dla aplikacji sieci Web.

    Utrzymywanie serwera sieci Web i serwera bazy danych w tym samym regionie zapewnia najlepszą wydajność i minimalizuje wydatki.
12. Kliknij znacznik wyboru u dołu pola, aby wskazać, że wszystko zostało zakończone.

    Na poniższej ilustracji przedstawiono okno dialogowe **Określanie ustawień bazy danych** z przykładowymi wartościami. Wprowadzone wartości mogą być różne.

    ![Krok ustawień bazy danych nowej witryny sieci Web — Kreator tworzenia z bazą danych](deploying-to-production/_static/image2.png)

    Portal zarządzania powróci do strony witryny sieci Web, a w kolumnie **stan** zostanie wyświetlona aplikacja sieci Web, która jest tworzona. Po czasie (zazwyczaj krótszym niż minutę) kolumna **stan** pokazuje, że aplikacja sieci Web została pomyślnie utworzona. Na pasku nawigacyjnym po lewej stronie liczba aplikacji sieci Web znajdujących się w Twoim koncie pojawia się obok ikony **witryn sieci** Web, a obok ikony **baz danych SQL** jest wyświetlana liczba baz danych.

    ![Strona witryny sieci Web portal zarządzania, utworzona witryna sieci Web](deploying-to-production/_static/image3.png)

    Nazwa aplikacji sieci Web różni się od przykładowej aplikacji na ilustracji.

## <a name="deploy-the-application-to-staging"></a>Wdrażanie aplikacji do przemieszczania

Po utworzeniu aplikacji sieci Web i bazy danych dla środowiska przejściowego można wdrożyć projekt.

> [!NOTE]
> Te instrukcje pokazują, jak utworzyć profil publikowania, pobierając plik *. publishsettings* , który działa nie tylko dla platformy Azure, ale również dla dostawców hostingu innych firm. Najnowsza wersja zestawu Azure SDK umożliwia także bezpośrednie nawiązywanie połączenia z platformą Azure z programu Visual Studio i wybieranie z listy aplikacji sieci Web, które znajdują się na koncie platformy Azure. W Visual Studio 2013 możesz zalogować się do platformy Azure w oknie dialogowym **publikowania w sieci Web** lub w oknie **Eksplorator serwera** . Aby uzyskać więcej informacji, zobacz [Tworzenie aplikacji sieci web ASP.NET w Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

### <a name="download-the-publishsettings-file"></a>Pobierz plik. publishsettings

1. Kliknij nazwę aplikacji sieci Web, która została właśnie utworzona.

    ![Kliknij witrynę, aby przejść do pulpitu nawigacyjnego](deploying-to-production/_static/image4.png)
2. W obszarze **szybkie przeglądy** na karcie **pulpit nawigacyjny** kliknij pozycję **Pobierz profil publikowania**.

    ![Pobierz link do profilu publikowania](deploying-to-production/_static/image5.png)

    Ten krok umożliwia pobranie pliku zawierającego wszystkie ustawienia, które są potrzebne w celu wdrożenia aplikacji w aplikacji sieci Web. Ten plik zostanie zaimportowany do programu Visual Studio, aby nie trzeba było wprowadzać tych informacji ręcznie.
3. Zapisz plik *. publishsettings* w folderze, do którego można uzyskać dostęp z programu Visual Studio.

    ![Zapisywanie pliku. publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Zabezpieczenia — plik *. publishsettings* zawiera Twoje poświadczenia (niekodowane), które są używane do administrowania subskrypcjami i usługami platformy Azure. Najlepszym rozwiązaniem w zakresie zabezpieczeń dla tego pliku jest przechowywanie go tymczasowo poza katalogami źródłowymi (na przykład w folderze Libraries\Documents), a następnie usunięcie go po zakończeniu importu. Złośliwy użytkownik, który uzyskuje dostęp do pliku *. publishsettings* , może edytować, tworzyć i usuwać usługi platformy Azure.

### <a name="create-a-publish-profile"></a>Tworzenie profilu publikowania

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt ContosoUniversity w **Eksplorator rozwiązań** i wybierz polecenie **Publikuj** z menu kontekstowego.

    Zostanie otwarty Kreator **publikowania w sieci Web** .
2. Kliknij kartę **profil** .
3. Kliknij przycisk **Importuj**.
4. Przejdź do pliku *publishsettings* , który został pobrany wcześniej, a następnie kliknij przycisk **Otwórz**.

    ![Importowanie ustawień publikowania — okno dialogowe](deploying-to-production/_static/image7.png)
5. Na karcie **połączenie** kliknij pozycję **Sprawdź poprawność połączenia** , aby upewnić się, że ustawienia są poprawne.

    Po sprawdzeniu poprawności połączenia zostanie wyświetlony zielony znacznik wyboru obok przycisku **Weryfikuj połączenie** .

    W przypadku niektórych dostawców hostingu po kliknięciu przycisku **Weryfikuj połączenie**może zostać wyświetlone okno dialogowe **błąd certyfikatu** . Jeśli tak jest, sprawdź, czy nazwa serwera jest oczekiwana. Jeśli nazwa serwera jest poprawna, wybierz pozycję **Zapisz ten certyfikat dla przyszłych sesji programu Visual Studio** , a następnie kliknij przycisk **Akceptuj**. (Ten błąd oznacza, że dostawca hostingu zdecydował się uniknąć wydatków zakupu certyfikatu SSL dla adresu URL, który jest wdrażany. Jeśli wolisz nawiązać bezpieczne połączenie przy użyciu ważnego certyfikatu, skontaktuj się z dostawcą hostingu.
6. Kliknij przycisk **Dalej**.

    ![ikona pomyślnego połączenia i przycisk Dalej na karcie połączenie](deploying-to-production/_static/image8.png)
7. Na karcie **Ustawienia** rozwiń węzeł **Opcje publikowania plików**, a następnie wybierz opcję **wyklucz pliki z folderu\_danych aplikacji**.

    Informacje o innych opcjach w obszarze **Opcje publikowania plików**można znaleźć w samouczku [wdrażanie do usług IIS](deploying-to-iis.md) . Zrzut ekranu pokazujący wynik tego kroku oraz następujące kroki konfiguracji bazy danych znajdują się na końcu kroków konfiguracji bazy danych.
8. W obszarze **DefaultConnection** w sekcji **Databases (bazy danych** ) Skonfiguruj wdrożenie bazy danych dla bazy danych członkostwa.
9. 1. Wybierz pozycję **Aktualizuj bazę danych**.

        Pole **ciągu połączenia zdalnego** bezpośrednio poniżej **DefaultConnection** jest wypełnione parametrami połączenia z pliku publishsettings. Parametry połączenia zawierają poświadczenia SQL Server, które są przechowywane w postaci zwykłego tekstu w pliku *. pubxml* . Jeśli wolisz nie przechowywać ich trwale, możesz je usunąć z profilu publikowania po wdrożeniu bazy danych i przechowanie ich na platformie Azure. Aby uzyskać więcej informacji, zobacz [jak zachować bezpieczeństwo parametrów połączenia z bazą danych ASP.NET podczas wdrażania na platformie Azure z poziomu źródła](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) w blogu Scotta Hanselman.
      2. Kliknij pozycję **Konfiguruj aktualizacje bazy danych**.
      3. W oknie dialogowym **Konfiguruj aktualizacje bazy danych** kliknij przycisk **Dodaj skrypt SQL**.
      4. W polu **Dodaj skrypt SQL** przejdź do skryptu *ASPNET-Data-prod. SQL* , który został zapisany wcześniej w folderze rozwiązania, a następnie kliknij przycisk **Otwórz**.
      5. Zamknij okno dialogowe **Konfigurowanie aktualizacji bazy danych** .
10. W obszarze **SchoolContext** w sekcji **bazy danych** wybierz pozycję **Wykonaj migracje Code First (uruchamiaj przy uruchamianiu aplikacji)** .

    Program Visual Studio Wyświetla **migracje Code First wykonywania** zamiast **aktualizacji bazy danych** dla klas `DbContext`. Jeśli chcesz użyć dostawcy dbDacFx zamiast migracji do wdrożenia bazy danych, do której uzyskuje się dostęp przy użyciu klasy `DbContext`, zobacz [Jak mogę wdrożyć bazę danych Code First bez migracji?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) w przypadku wdrażania w sieci Web często zadawane pytania dotyczące programu Visual Studio i ASP.NET w witrynie MSDN.

    Karta **Ustawienia** wygląda teraz tak, jak w poniższym przykładzie:

    ![Karta Ustawienia dla przemieszczania](deploying-to-production/_static/image9.png)
11. Wykonaj następujące kroki, aby zapisać profil i zmienić jego nazwę na *przejściowy*:

    1. Kliknij kartę **profil** , a następnie kliknij pozycję **Zarządzaj profilami**.
    2. Import utworzył dwa nowe profile, jeden dla usługi FTP i jeden dla Web Deploy. Profil Web Deploy został skonfigurowany: Zmień nazwę tego profilu na *tymczasowy*.

        ![Zmień nazwę profilu na tymczasowy](deploying-to-production/_static/image10.png)
    3. Zamknij okno dialogowe **Edytowanie profilów publikowania w sieci Web** .
    4. Zamknij kreatora **publikacji w sieci Web** .

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Skonfiguruj transformację profilu publikowania dla wskaźnika środowiska

> [!NOTE]
> W tej sekcji pokazano, jak skonfigurować transformację pliku Web. config dla wskaźnika środowiska. Ponieważ wskaźnik znajduje się w `<appSettings>` elementu, istnieje inna alternatywa dla określania transformacji podczas wdrażania do Azure App Service. Aby uzyskać więcej informacji, zobacz [Określanie ustawień sieci Web. config na platformie Azure](web-config-transformations.md#watransforms).

1. W **Eksplorator rozwiązań**rozwiń węzeł **Właściwości**, a następnie rozwiń węzeł **PublishProfiles**.
2. Kliknij prawym przyciskiem myszy pozycję *tymczasowy. pubxml*, a następnie kliknij pozycję **Dodaj przekształcenie konfiguracji**.

    ![Dodaj transformację konfiguracji dla przemieszczania](deploying-to-production/_static/image11.png)

    Program Visual Studio tworzy plik transformacji *Web. Stag. config* i otwiera go.
3. W pliku transformacji *Web. stagd. config* Wstaw następujący kod bezpośrednio po tagu otwierającym `configuration`.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    W przypadku korzystania z profilu publikowania przemieszczania, to przekształcenie ustawia wskaźnik środowiska na "prod". W wdrożonej aplikacji sieci Web nie zobaczysz żadnego sufiksu, takiego jak "(dev)" lub "(test)" po nagłówku "Contoso University" H1.
4. Kliknij prawym przyciskiem myszy plik *Web. stagd. config* , a następnie kliknij pozycję **Podgląd transformacji** , aby upewnić się, że zakodowana transformacja generuje oczekiwane zmiany.

    W oknie **Podgląd pliku Web. config** zostanie wyświetlona informacja o wyniku zastosowania zarówno transformacji *Web. release. config* , jak i *pliku Web. stagd. config* .

### <a name="prevent-public-use-of-the-test-app"></a>Zapobiegaj publicznym użyciu aplikacji testowej

Ważnym zagadnieniem dotyczącym aplikacji przemieszczania jest to, że będzie ona aktywna w Internecie, ale nie chcesz, aby była ona używana publicznie. Aby zminimalizować prawdopodobieństwo, że użytkownicy znajdą i korzystają z niej, możesz użyć jednej z następujących metod:

- Ustawianie reguł zapory zezwalających na dostęp do tymczasowej aplikacji tylko z adresów IP używanych do testowania przemieszczania.
- Użyj zasłoniętego adresu URL, który mógłby być niemożliwy do odgadnięcia.
- Utwórz plik *robots. txt* , aby upewnić się, że aparaty wyszukiwania nie przeszukają aplikacji testowej i łączy się z nią w wynikach wyszukiwania.

Pierwszy z tych metod jest najbardziej skuteczny, ale nie jest omawiany w tym samouczku, ponieważ wymagało wdrożenia usługi w chmurze platformy Azure, a nie Azure App Service. Aby uzyskać więcej informacji na temat ograniczeń dotyczących Cloud Services i adresów IP na platformie Azure, zobacz [Opcje hostingu obliczeń udostępniane przez platformę Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) i [Blokuj dostęp do roli sieci Web przy użyciu określonych adresów IP](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). W przypadku wdrażania w ramach dostawcy hostingu innej firmy skontaktuj się z dostawcą, aby dowiedzieć się, jak zaimplementować ograniczenia adresów IP.

W tym samouczku utworzysz plik *robots. txt* .

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity i kliknij pozycję **Dodaj nowy element**.
2. Utwórz nowy **plik tekstowy** o nazwie *robots. txt*i umieść w nim następujący tekst:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    Wiersz `User-agent` informuje aparaty wyszukiwania, że reguły w pliku mają zastosowanie do wszystkich przeszukiwań sieci Web aparatu wyszukiwania (robotów), a wiersz `Disallow` określa, że nie należy przeszukiwać stron w witrynie.

    Aby aparaty wyszukiwania mogli wykazać aplikację produkcyjną, należy wykluczyć ten plik z wdrożenia produkcyjnego. W tym celu skonfigurujesz ustawienie w profilu publikowania produkcji podczas jego tworzenia.

### <a name="deploy-to-staging"></a>Wdrażanie w środowisku przejściowym

1. Otwórz kreatora **publikacji w sieci Web** , klikając prawym przyciskiem myszy projekt Contoso University i klikając pozycję **Publikuj**.
2. Upewnij się, że wybrano profil **przemieszczania** .
3. Kliknij przycisk **Opublikuj**.

    W oknie **danych wyjściowych** znajdują się informacje o wykonywaniu akcji wdrażania i raporty o pomyślnym zakończeniu wdrożenia. Domyślna przeglądarka automatycznie otwiera adres URL wdrożonej aplikacji sieci Web.

## <a name="test-in-the-staging-environment"></a>Testowanie w środowisku przejściowym

Zwróć uwagę, że wskaźnik środowiska jest nieobecny (nie ma "(test)" lub "(dev)" po nagłówku H1, który pokazuje, że transformacja *pliku Web. config* dla wskaźnika środowiska zakończyła się pomyślnie.

![Przemieszczanie strony głównej](deploying-to-production/_static/image12.png)

Uruchom stronę **uczniów** , aby sprawdzić, czy wdrożona baza danych nie ma uczniów.

Uruchom stronę **instruktorów** , aby sprawdzić, czy Code First wypełniania bazy danych za pomocą instruktora:

Wybierz pozycję **Dodaj uczniów** z menu **uczniów** , Dodaj ucznia, a następnie Wyświetl nowych uczniów na stronie **uczniów** , aby sprawdzić, czy można pomyślnie zapisywać dane w bazie danych.

Na stronie **kursy** kliknij pozycję **Aktualizuj kredyty**. Strona **kredyty aktualizacji** wymaga uprawnień administratora, więc zostanie wyświetlona strona **logowania** . Wprowadź utworzone wcześniej poświadczenia konta administratora ("admin" i "prodpwd"). Zostanie wyświetlona strona **kredyty aktualizacji** , która sprawdza, czy konto administratora utworzone w poprzednim samouczku zostało prawidłowo wdrożone w środowisku testowym.

Zażądaj nieprawidłowego adresu URL, aby spowodować błąd, który będzie śledzony przez program ELMAH, a następnie Zażądaj raportu o błędach ELMAH. W przypadku wdrażania w ramach dostawcy hostingu innej firmy prawdopodobnie okaże się, że raport jest pusty z tego samego powodu, gdyby był pusty w poprzednim samouczku. Musisz użyć narzędzi do zarządzania kontami dostawcy hostingu, aby skonfigurować uprawnienia do folderu, aby umożliwić programowi ELMAH zapis w folderze dziennika.

Utworzona aplikacja jest teraz uruchomiona w chmurze w aplikacji sieci Web, która jest taka sama jak ta, która będzie używana w środowisku produkcyjnym. Ponieważ wszystko działa prawidłowo, następnym krokiem jest wdrożenie w środowisku produkcyjnym.

## <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

Proces tworzenia produkcyjnej aplikacji sieci Web i wdrażania jej w środowisku produkcyjnym jest taki sam jak w przypadku przemieszczania, z tą różnicą, że należy wykluczyć *plik robots. txt* z wdrożenia. Aby edytować plik profilu publikacji.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Utwórz środowisko produkcyjne i profil publikowania produkcji

1. Utwórz produkcyjną aplikację sieci Web i bazę danych na platformie Azure, postępując zgodnie z tą samą procedurą, która była używana do przemieszczania.

    Podczas tworzenia bazy danych można ją umieścić na tym samym serwerze, który został utworzony wcześniej, lub utworzyć nowy serwer.
2. Pobierz plik *. publishsettings* .
3. Utwórz profil publikowania przez zaimportowanie pliku Product *. publishsettings* , postępując zgodnie z tą samą procedurą, która była używana do przemieszczania.

    Nie zapomnij skonfigurować skryptu wdrażania danych w obszarze **DefaultConnection** w sekcji **bazy danych** na karcie **Ustawienia** .
4. Zmień nazwę profilu publikowania na *produkcyjny*.
5. Skonfiguruj transformację profilu publikowania dla wskaźnika środowiska, wykonując tę samą procedurę, która była używana do przemieszczania...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edytuj plik. pubxml, aby wykluczyć pliki robots. txt

Pliki profilu publikowania mają nazwę &lt;ProfileName&gt; *. pubxml* i znajdują się w folderze *PublishProfiles* . Folder *PublishProfiles* znajduje się w folderze *Właściwości* w projekcie aplikacji C# sieci Web, w folderze *mój projekt* w projekcie aplikacji sieci web w języku vb lub w folderze *dane\_aplikacji* w projekcie aplikacji sieci Web. Każdy plik *. pubxml* zawiera ustawienia, które dotyczą jednego profilu publikowania. Wartości wprowadzone w Kreatorze publikowania w sieci Web są przechowywane w tych plikach i można je edytować w celu utworzenia lub zmiany ustawień, które nie są dostępne w interfejsie użytkownika programu Visual Studio.

Domyślnie pliki *. pubxml* są uwzględniane w projekcie podczas tworzenia profilu publikowania, ale można je wykluczyć z projektu, a program Visual Studio będzie nadal z nich korzystać. Program Visual Studio szuka plików *. pubxml* w folderze *PublishProfiles* , niezależnie od tego, czy są one dołączone do projektu.

Dla każdego pliku *. pubxml* istnieje plik *. pubxml. User* . Plik *. pubxml. User* zawiera zaszyfrowane hasło w przypadku wybrania opcji **Zapisz hasło** i domyślnie jest wykluczony z projektu.

Plik *. pubxml* zawiera ustawienia, które odnoszą się do określonego profilu publikacji. Aby skonfigurować ustawienia, które mają zastosowanie do wszystkich profilów, można utworzyć plik *. WPP. targets* . Proces kompilacji importuje te pliki do pliku projektu *. csproj* lub *. vbproj* , dlatego większość ustawień, które można skonfigurować w pliku projektu, można skonfigurować w tych plikach. Aby uzyskać więcej informacji na temat plików *. pubxml* i *. WPP. targets* , zobacz [How to: Edit Settings Deployment (pliki. pubxml) i plik. WPP. targets w projektach internetowych programu Visual Studio](https://msdn.microsoft.com/library/ff398069.aspx).

1. W **Eksplorator rozwiązań**rozwiń węzeł **Właściwości** i rozwiń węzeł **PublishProfiles**.
2. Kliknij prawym przyciskiem myszy pozycję *Product. pubxml* , a następnie kliknij pozycję **Otwórz**.

    ![Otwórz plik. pubxml](deploying-to-production/_static/image13.png)
3. Kliknij prawym przyciskiem myszy pozycję *Product. pubxml* , a następnie kliknij pozycję **Otwórz**.
4. Dodaj następujące wiersze bezpośrednio przed zamykającym `PropertyGroup` elementu:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Plik. pubxml wygląda teraz podobnie do poniższego przykładu:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Aby uzyskać więcej informacji na temat wykluczania plików i folderów, zobacz [czy można wykluczać określone pliki lub foldery z wdrożenia?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) w artykule **wdrażanie w sieci Web — często zadawane pytania dotyczące programu Visual Studio i ASP.NET** w witrynie MSDN.

### <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

1. Otwórz kreatora **publikacji w sieci Web** , upewnij się, że wybrano opcję profil publikowania **produkcyjnego** , a następnie kliknij przycisk **Uruchom podgląd** na karcie **Podgląd** , aby sprawdzić, czy plik *robots. txt* nie zostanie skopiowany do aplikacji produkcyjnej.

    ![Wersja zapoznawcza plików do opublikowania w środowisku produkcyjnym](deploying-to-production/_static/image14.png)

    Przejrzyj listę plików, które zostaną skopiowane. Zobaczysz, że wszystkie pliki *. cs* , w tym pliki *. aspx.cs*, *. aspx.Designer.cs*, *Master.cs*i *Master.Designer.cs* , są pomijane. Wszystkie te kody zostały skompilowane do plików *ContosoUniversity. dll* i *ContosoUniversity. pdb* , które znajdują się w folderze *bin* . Ponieważ tylko plik *. dll* jest wymagany do uruchomienia aplikacji i określono wcześniej, że należy wdrożyć tylko pliki potrzebne do uruchomienia aplikacji, żadne pliki *. cs* nie zostały skopiowane do środowiska docelowego. Folder *obj* i pliki *ContosoUniversity. csproj* i *. csproj. User* są pomijane z tego samego powodu.

    Kliknij pozycję **Publikuj** , aby wdrożyć w środowisku produkcyjnym.
2. Przetestuj w środowisku produkcyjnym, wykonując tę samą procedurę, która została użyta do przemieszczania.

    Wszystko jest identyczne z przemieszczeniem poza adresem URL i brakiem pliku *robots. txt* .

## <a name="summary"></a>Podsumowanie

Pomyślnie wdrożono i przetestowasz aplikację internetową, która jest dostępna publicznie przez Internet.

![Strona główna — produkcja](deploying-to-production/_static/image15.png)

W następnym samouczku należy zaktualizować kod aplikacji i wdrożyć zmiany w środowiskach testowych, przejściowych i produkcyjnych.

> [!NOTE]
> Gdy aplikacja jest używana w środowisku produkcyjnym, należy wykonać wdrożenie planu odzyskiwania. Oznacza to, że należy okresowo tworzyć kopie zapasowe baz danych z aplikacji produkcyjnej w bezpiecznej lokalizacji magazynu i należy zachować kilka generacji takich kopii zapasowych. Podczas aktualizowania bazy danych należy wykonać kopię zapasową od razu przed zmianą. Następnie, jeśli wystąpi błąd i nie wykryjesz go, dopóki nie zostanie wdrożony w środowisku produkcyjnym, nadal będzie można odzyskać bazę danych do stanu, w którym była przed uszkodzeniem. Aby uzyskać więcej informacji, zobacz [Tworzenie kopii zapasowej i przywracanie bazy danych SQL na platformie Azure](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> W tym samouczku SQL Server Edition, do którego wdrażasz, jest Azure SQL Database. Chociaż proces wdrażania jest podobny do innych wersji SQL Server, prawdziwa aplikacja produkcyjna może wymagać specjalnego kodu dla Azure SQL Database w niektórych scenariuszach. Aby uzyskać więcej informacji, zobacz [Praca z Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) i [wybór między SQL Server i Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Poprzednie](setting-folder-permissions.md)
> [dalej](deploying-a-code-update.md)

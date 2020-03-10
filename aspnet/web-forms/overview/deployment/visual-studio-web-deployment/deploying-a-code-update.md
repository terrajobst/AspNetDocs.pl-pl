---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji kodu | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567404"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie aktualizacji kodu

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

Po wdrożeniu wstępnym można nadal utrzymywać i opracowywać swoją witrynę sieci Web oraz przed długim wdrożeniem aktualizacji. Ten samouczek przeprowadzi Cię przez proces wdrażania aktualizacji w kodzie aplikacji. Aktualizacja zaimplementowana i wdrażana w tym samouczku nie obejmuje zmiany bazy danych; w następnym samouczku znajdziesz informacje o sposobie wdrażania zmian w bazie danych.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).

## <a name="make-a-code-change"></a>Wprowadzanie zmiany w kodzie

Jako prosty przykład aktualizacji aplikacji, dodasz do strony **instruktorów** listę kursów według wybranego instruktora.

Jeśli uruchomisz stronę **instruktorów** , zobaczysz, że w siatce znajdują **się linki,** ale nie wykonują żadnych innych czynności niż sprawia, że tło wierszy jest szare.

![Strona instruktorów z zaznaczeniem](deploying-a-code-update/_static/image1.png)

Teraz dodasz kod, który jest uruchamiany, gdy zostanie kliknięty link **Wybierz** i zostanie wyświetlona lista szkoleń szkoleniowych według wybranego instruktora.

1. W oknie *instruktors. aspx*Dodaj następujące znaczniki bezpośrednio po kontrolce `Label` **ErrorMessageLabel** :

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Uruchom stronę i wybierz instruktora. Zostanie wyświetlona lista szkoleń szkoleniowych według tego instruktora.

    ![Strona instruktorów z nauczaniem kursów](deploying-a-code-update/_static/image2.png)
3. Zamknij okno przeglądarki.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Wdrażanie aktualizacji kodu w środowisku testowym

Aby można było używać profilów publikowania do wdrożenia do testowania, przemieszczania i produkcji, należy zmienić opcje publikowania bazy danych. W przypadku bazy danych członkostwa nie trzeba już uruchamiać skryptów grantu i wdrażania danych.

1. Otwórz kreatora **publikacji w sieci Web** , klikając prawym przyciskiem myszy projekt ContosoUniversity i klikając polecenie **Publikuj**.
2. Kliknij profil **testu** na liście rozwijanej **profil** .
3. Kliknij kartę **Ustawienia**.
4. W obszarze **DefaultConnection** w sekcji **bazy danych** wyczyść pole wyboru **Aktualizuj bazę danych** .
5. Kliknij kartę **profil** , a następnie kliknij pozycję Profil **przemieszczania** na liście rozwijanej **profil** .
6. Gdy zostanie wyświetlony monit o zapisanie zmian wprowadzonych w profilu **testu** , kliknij przycisk **tak**.
7. Wprowadź tę samą zmianę w profilu przemieszczania.
8. Powtórz ten proces, aby wprowadzić tę samą zmianę w profilu produkcyjnym.
9. Zamknij kreatora **publikacji w sieci Web** .

Wdrażanie w środowisku testowym jest teraz prostą kwestią ponownego uruchamiania jednego kliknięcia. Aby ten proces był szybszy, możesz skorzystać z paska narzędzi **Publikowanie w sieci Web** .

1. W menu **Widok** wybierz **paski narzędzi** , a następnie wybierz pozycję **Sieć Web jeden kliknij pozycję Publikuj**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. W **Eksplorator rozwiązań**wybierz projekt ContosoUniversity.
3. na pasku narzędzi **Publikowanie w sieci Web** , wybierz profil publikacji **test** , a następnie kliknij pozycję **Publikuj sieć Web** (ikona z strzałkami wskazującymi w lewo i w prawo).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Program Visual Studio wdraża zaktualizowaną aplikację, a przeglądarka automatycznie otwiera stronę główną.
5. Uruchom stronę Instruktorzy i wybierz instruktora, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

Zwykle można także przeprowadzić testy regresji (to oznacza pozostałą część lokacji, aby upewnić się, że Nowa zmiana nie przerywa żadnej istniejącej funkcjonalności). Jednak w tym samouczku pominiesz ten krok i zaczniesz wdrożyć aktualizację do środowiska przejściowego i produkcyjnego.

Podczas ponownego wdrażania Web Deploy automatycznie określa, które pliki uległy zmianie, i kopiuje tylko zmienione pliki na serwer. Domyślnie Web Deploy używa ostatnich zmienionych dat dla plików, aby określić, które z nich uległy zmianie. Niektóre systemy kontroli źródła zmieniają daty plików nawet wtedy, gdy nie zmienisz zawartości pliku. W takim przypadku można skonfigurować Web Deploy do używania sum kontrolnych plików, aby określić, które pliki uległy zmianie. Aby uzyskać więcej informacji, zobacz [dlaczego wszystkie moje pliki są ponownie wdrażane, mimo że nie zostały one zmienione?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) w ramach wdrożenia ASP.NET często zadawane pytania.

## <a name="take-the-application-offline-during-deployment"></a>Przejmowanie aplikacji w tryb offline podczas wdrażania

Wdrażana zmiana to prosta zmiana jednej strony. Czasami jednak wdrażane są większe zmiany lub wdrażane są zarówno zmiany kodu, jak i bazy danych, a lokacja może zachowywać się nieprawidłowo, jeśli użytkownik zażąda strony przed ukończeniem wdrożenia. Aby uniemożliwić użytkownikom dostęp do witryny w trakcie wdrażania, można użyć *aplikacji\_pliku offline. htm* . Po umieszczeniu pliku o nazwie *app\_offline. htm* w folderze głównym aplikacji usługi IIS automatycznie wyświetlają ten plik zamiast uruchamiania aplikacji. Aby zapobiec uzyskiwaniu dostępu podczas wdrażania, należy umieścić *aplikację\_w trybie offline. htm* w folderze głównym, uruchomić proces wdrażania, a następnie usunąć *aplikację\_offline. htm* po pomyślnym wdrożeniu.

Web Deploy można skonfigurować do automatycznego umieszczania aplikacji domyślnej *\_pliku w trybie offline. htm* na serwerze podczas uruchamiania wdrażania i usuwania go po zakończeniu. Aby to zrobić, należy dodać następujący element XML do pliku profilu publikacji (. pubxml):

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

W tym samouczku przedstawiono sposób tworzenia i używania aplikacji niestandardowej\_pliku *offline. htm* .

Korzystanie z *aplikacji\_w trybie offline. htm* w lokacji przemieszczania nie jest wymagane, ponieważ nie masz użytkowników uzyskujących dostęp do lokacji tymczasowej. Jest jednak dobrym sposobem na przetestowanie wszystkich sposobów planowania wdrożenia w środowisku produkcyjnym.

### <a name="create-app_offlinehtm"></a>Utwórz aplikację\_w trybie offline. htm

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **nowy element**.
2. Utwórz **stronę HTML** o nazwie *App\_offline. htm* (Usuń ostateczną "l" w rozszerzeniu *HTML* , które domyślnie tworzy program Visual Studio).
3. Zastąp znaczniki szablonu następującym znacznikiem:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Zapisz i zamknij plik.

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a>Kopiuj aplikację\_offline. htm do folderu głównego witryny sieci Web

Do kopiowania plików do witryny sieci Web można użyć dowolnego narzędzia FTP. [FileZilla](http://filezilla-project.org/) to popularne narzędzie FTP, które jest wyświetlane na zrzutach ekranu.

Aby korzystać z narzędzia FTP, potrzebne są trzy rzeczy: adres URL FTP, nazwa użytkownika i hasło.

Adres URL jest pokazywany na stronie pulpitu nawigacyjnego witryny sieci Web w usłudze Azure portal zarządzania, a nazwa użytkownika i hasło dla usługi FTP można znaleźć w pliku *publishsettings* , który został wcześniej pobrany. Poniższe kroki pokazują, jak uzyskać te wartości.

1. W portal zarządzania Azure kliknij kartę **witryny sieci Web** , a następnie kliknij tymczasową witrynę sieci Web.
2. Na stronie **pulpit nawigacyjny** przewiń w dół, aby znaleźć nazwę hosta FTP w sekcji **szybki przegląd** .

    ![Nazwa hosta FTP](deploying-a-code-update/_static/image5.png)
3. Otwórz plik przemieszczania *publishsettings* w Notatniku lub innym edytorze tekstu.
4. Znajdź element `publishProfile` dla profilu FTP.
5. Skopiuj wartości `userName` i `userPWD`.

    ![Poświadczenia FTP](deploying-a-code-update/_static/image6.png)
6. Otwórz narzędzie FTP i zaloguj się do adresu URL FTP.
7. Kopiuj *aplikację\_offline. htm* z folderu rozwiązanie do folderu */site/wwwroot* w lokacji tymczasowej.

    ![Kopiuj app_offline](deploying-a-code-update/_static/image7.png)
8. Przejdź do adresu URL witryny tymczasowej. Zobaczysz, że *aplikacja\_w trybie offline. htm* jest teraz wyświetlana zamiast strony głównej.

    ![app_offline. htm w oknie przeglądarki](deploying-a-code-update/_static/image8.png)

Teraz można przystąpić do wdrażania do wdrożenia przejściowego.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Wdrażanie aktualizacji kodu do przemieszczania i produkcji

1. W **sieci Web kliknij przycisk Publikuj** na pasku narzędzi, wybierz profil publikowania **przejściowego** , a następnie kliknij pozycję **Publikuj sieć Web**.

    Program Visual Studio wdraża zaktualizowaną aplikację i otwiera przeglądarkę na stronie głównej witryny. Zostanie wyświetlony plik *\_w trybie offline. htm* . Przed przeprowadzeniem testów w celu zweryfikowania pomyślnego wdrożenia należy usunąć *aplikację\_pliku offline. htm* .
2. Wróć do narzędzia FTP i Usuń **aplikację\_w trybie offline. htm** z lokacji tymczasowej.
3. W przeglądarce Otwórz stronę instruktorzy w lokacji tymczasowej, a następnie wybierz instruktora, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.
4. Postępuj zgodnie z tą samą procedurą dla środowiska produkcyjnego, jak w przypadku przygotowania.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Przeglądanie zmian i wdrażanie określonych plików

Program Visual Studio 2012 daje również możliwość wdrażania pojedynczych plików. Dla wybranego pliku można wyświetlić różnice między lokalną wersją i wdrożoną wersją, wdrożyć plik w środowisku docelowym lub skopiować plik ze środowiska docelowego do projektu lokalnego. W tej części samouczka pokazano, jak korzystać z tych funkcji.

### <a name="make-a-change-to-deploy"></a>Wprowadź zmianę do wdrożenia

1. Otwórz *zawartość/site. css*i Znajdź blok dla tagu `body`.
2. Zmień wartość `background-color` z `#fff` na `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Wyświetl zmianę w oknie publikowania wersji zapoznawczej

W przypadku opublikowania projektu za pomocą kreatora **publikacji w sieci Web** można zobaczyć, jakie zmiany mają być publikowane przez dwukrotne kliknięcie pliku w oknie **podglądu** .

1. Kliknij prawym przyciskiem myszy projekt ContosoUniversity, a następnie kliknij pozycję **Publikuj**.
2. Z listy rozwijanej **profil** wybierz profil publikacji **testowej** .
3. Kliknij pozycję **Podgląd**, a następnie kliknij pozycję **Uruchom podgląd**.
4. W okienku **podglądu** kliknij dwukrotnie pozycję **site. css**.

    ![Kliknij dwukrotnie pozycję site. css.](deploying-a-code-update/_static/image9.png)

    Okno dialogowe **Podgląd zmian** zawiera Podgląd zmian, które zostaną wdrożone.

    ![Podgląd zmian w pliku site. CSS](deploying-a-code-update/_static/image10.png)

    W przypadku dwukrotnego kliknięcia pliku *Web. config* okno dialogowe **Podgląd zmian** pokazuje efekt transformacji konfiguracji kompilacji i przekształceń profilu publikowania. W tym momencie nie wykonano żadnych czynności, które mogłyby spowodować zmianę pliku *Web. config* na serwerze, aby zobaczyć, że nie wprowadzono żadnych zmian. Jednak okno **Podgląd zmian** nieprawidłowo pokazuje dwie zmiany. Wygląda na to, że dwa elementy XML zostaną usunięte. Te elementy są dodawane przez proces publikowania po wybraniu opcji **wykonaj migracje Code First przy uruchamianiu aplikacji** dla klasy kontekstu Code First. Porównanie jest wykonywane przed dodaniem tych elementów przez proces publikowania, dlatego wygląda na to, że są usuwane, chociaż nie zostaną usunięte. Ten błąd zostanie rozwiązany w przyszłej wersji.
5. Kliknij przycisk **Zamknij**.
6. Kliknij przycisk **Opublikuj**.
7. Gdy przeglądarka zostanie otwarta na stronie głównej witryny testowej, naciśnij kombinację klawiszy CTRL + F5, aby wypróbować twarde odświeżanie, aby zobaczyć efekt zmiany w CSS.

    ![Efekt zmiany CSS](deploying-a-code-update/_static/image11.png)
8. Zamknij okno przeglądarki.

### <a name="publish-specific-files-from-solution-explorer"></a>Publikowanie określonych plików z Eksplorator rozwiązań

Załóżmy, że nie podoba Ci się niebieskie tło i chcesz przywrócić oryginalny kolor. W tej sekcji zostaną przywrócone pierwotne ustawienia przez opublikowanie określonego pliku bezpośrednio z **Eksplorator rozwiązań**.

1. Otwórz *zawartość/site. css* i przywróć ustawienie `background-color`, aby `#fff`.
2. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy plik *Content/site. css* .

    Menu kontekstowe zawiera trzy opcje publikacji.

    ![Opcje publikowania z Eksplorator rozwiązań](deploying-a-code-update/_static/image12.png)
3. Kliknij pozycję **Podgląd zmian w pliku site. css**.

    Zostanie otwarte okno pokazujące różnice między plikiem lokalnym a jego wersją w środowisku docelowym.

    ![Diff-Content/site. CSS](deploying-a-code-update/_static/image13.png)
4. W **Eksplorator rozwiązań**ponownie kliknij prawym przyciskiem myszy **witrynę site. css** , a następnie kliknij pozycję **Publikuj witrynę. css**.

    Okno **działanie publikowania w sieci Web** pokazuje, że plik został opublikowany.

    ![Okno działania publikowania w sieci Web](deploying-a-code-update/_static/image14.png)
5. Otwórz przeglądarkę, aby uzyskać adres URL `http://localhost/contosouniversity`, a następnie naciśnij klawisze CTRL + F5, aby wypróbować twarde odświeżanie, aby zobaczyć efekt zmiany w CSS.

    ![Strona główna z normalnym arkuszem stylów CSS](deploying-a-code-update/_static/image15.png)
6. Zamknij okno przeglądarki.

## <a name="summary"></a>Podsumowanie

Zaobserwowano kilka sposobów wdrażania aktualizacji aplikacji, która nie obejmuje zmian w bazie danych, i przedstawiono sposób wyświetlania podglądu zmian w celu sprawdzenia, czy aktualizacje są aktualne, czego oczekujesz. Strona instruktorzy ma teraz sekcję **szkolenia kursów** .

![Strona instruktorów z nauczaniem kursów](deploying-a-code-update/_static/image16.png)

W następnym samouczku pokazano, jak wdrożyć zmianę bazy danych: należy dodać pole DataUrodzenia do bazy danych i do strony instruktorzy.

> [!div class="step-by-step"]
> [Poprzednie](deploying-to-production.md)
> [dalej](deploying-a-database-update.md)

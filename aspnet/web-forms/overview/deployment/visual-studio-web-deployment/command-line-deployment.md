---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrożenie wiersza polecenia | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630922"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrożenie wiersza polecenia

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak wywoływać potok publikacji w sieci Web w programie Visual Studio z poziomu wiersza polecenia. Jest to przydatne w scenariuszach, w których [proces wdrażania](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) ma być zautomatyzowany, zamiast ręcznie w programie Visual Studio, zazwyczaj przy użyciu [systemu kontroli wersji kodu źródłowego](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Wprowadź zmianę do wdrożenia

Obecnie Strona informacje zawiera kod szablonu.

![Informacje o stronie z kodem szablonu](command-line-deployment/_static/image1.png)

Spowoduje to zamianę na kod, który wyświetla podsumowanie rejestracji ucznia.

Otwórz stronę *about. aspx* , Usuń wszystkie znaczniki wewnątrz elementu `MainContent` `Content` i Wstaw następujące znaczniki w miejscu:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Uruchom projekt i wybierz stronę **informacje** .

![Informacje o stronie](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Wdróż do testowania przy użyciu wiersza polecenia

Nie zostanie wdrożona inna zmiana bazy danych, dlatego należy wyłączyć wdrażanie bazy danych dbDacFx dla bazy danych ASPNET-ContosoUniversity. Otwórz kreatora **publikacji w sieci Web** i w każdym z trzech profilów publikowania wyczyść pole wyboru **Aktualizuj bazę danych** na karcie **Ustawienia** .

Na stronie startowej systemu Windows 8 Wyszukaj **wiersz polecenia dla deweloperów VS2012**.

Kliknij prawym przyciskiem myszy ikonę **wiersz polecenia dla deweloperów dla VS2012** , a następnie kliknij pozycję **Uruchom jako administrator**.

Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżką do pliku rozwiązania:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

Program MSBuild kompiluje rozwiązanie i wdraża je w środowisku testowym.

![Dane wyjściowe wiersza polecenia](command-line-deployment/_static/image3.png)

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie kliknij stronę **informacje** , aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.

Jeśli nie utworzono uczniów w teście, zobaczysz pustą stronę w nagłówku **statystyki treści ucznia** . Przejdź do strony **uczniów** , kliknij pozycję **Dodaj uczniów**i Dodaj wybranych uczniów, a następnie wróć do strony **informacje** , aby wyświetlić statystyki uczniów.

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opcje wiersza polecenia klucza

Wprowadzone polecenie przekazało ścieżkę pliku rozwiązania i dwie właściwości do programu MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Wdrażanie rozwiązania i wdrażanie pojedynczych projektów

Określenie pliku rozwiązania powoduje skompilowanie wszystkich projektów w rozwiązaniu. Jeśli masz wiele projektów sieci Web w rozwiązaniu, stosuje się następujące zachowanie MSBuild:

- Właściwości określone w wierszu polecenia są przesyłane do każdego projektu. W związku z tym każdy projekt sieci Web musi mieć profil publikacji o określonej nazwie. Jeśli określisz `/p:PublishProfile=Test`, każdy projekt sieci Web musi mieć profil publikacji o nazwie *test*.
- Możesz pomyślnie opublikować jeden projekt, gdy inny nie będzie jeszcze kompilować. Aby uzyskać więcej informacji, zobacz StackOverflow wątku programu [MSBuild kończy się niepowodzeniem z dwoma pakietami](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Jeśli określisz indywidualny projekt zamiast rozwiązania, musisz dodać parametr, który określa wersję programu Visual Studio. Jeśli używasz programu Visual Studio 2012, wiersz polecenia będzie wyglądać podobnie do poniższego przykładu:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Numer wersji dla programu Visual Studio 2010 to 10,0. Aby uzyskać więcej informacji, zobacz blog programu [Visual Studio — zgodność i VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) na blogu Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Określanie profilu publikowania

Możesz określić profil publikacji według nazwy lub pełną ścieżkę do pliku *. pubxml* , jak pokazano w następującym przykładzie:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Metody publikowania w sieci Web obsługiwane w przypadku publikowania w wierszu polecenia

Trzy metody publikowania są obsługiwane w przypadku publikowania w wierszu polecenia:

- `MSDeploy` — Publikowanie przy użyciu Web Deploy.
- `Package` — Publikuj przez utworzenie pakietu Web Deploy. Należy zainstalować pakiet niezależnie od polecenia MSBuild, które go tworzy.
- `FileSystem` — Publikuj przez kopiowanie plików do określonego folderu.

### <a name="specifying-the-build-configuration-and-platform"></a>Określanie konfiguracji i platformy kompilacji

Konfiguracja kompilacji i platforma musi być ustawiona w programie Visual Studio lub w wierszu polecenia. Profile publikowania obejmują właściwości o nazwie `LastUsedBuildConfiguration` i `LastUsedPlatform`, ale nie można ustawić tych właściwości w celu określenia sposobu kompilowania projektu. Aby uzyskać więcej informacji, zobacz [MSBuild: jak ustawić właściwość konfiguracji](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) na blogu Sayed Hashimi.

## <a name="deploy-to-staging"></a>Wdrażanie w środowisku przejściowym

Aby wdrożyć na platformie Azure, musisz dodać hasło do wiersza polecenia. Jeśli hasło zostało zapisane w profilu publikowania w programie Visual Studio, zostało zapisane w postaci zaszyfrowanej w pliku *. pubxml. User* . Ten plik nie jest używany przez program MSBuild podczas wdrażania wiersza polecenia, więc musisz przekazać hasło w parametrze wiersza polecenia.

1. Skopiuj wymagane hasło z pliku *publishsettings* , który został wcześniej pobrany dla tymczasowej witryny sieci Web. Hasło jest wartością atrybutu `userPWD` dla elementu Web Deploy `publishProfile`.

    ![Web Deploy hasło](command-line-deployment/_static/image5.png)
2. Na stronie startowej systemu Windows 8 Wyszukaj pozycję **wiersz polecenia dla deweloperów for VS2012**, a następnie kliknij ikonę, aby otworzyć wiersz polecenia. (Nie musisz otwierać go jako administrator tego czasu, ponieważ nie są wdrażane w usługach IIS na komputerze lokalnym).
3. Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżką do pliku rozwiązania i hasłem hasła:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Należy zauważyć, że ten wiersz polecenia zawiera dodatkowy parametr: `/p:AllowUntrustedCertificate=true`. W trakcie pisania tego samouczka Właściwość `AllowUntrustedCertificate` musi być ustawiona podczas publikowania na platformie Azure z poziomu wiersza polecenia. Gdy poprawka dla tej usterki zostanie wydana, ten parametr nie jest potrzebny.
4. Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij stronę **informacje** , aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.

    Jak widać wcześniej w środowisku testowym, może być konieczne utworzenie niektórych uczniów, aby wyświetlić statystyki na stronie **informacje** .

## <a name="deploy-to-production"></a>Wdrażanie w środowisku produkcyjnym

Proces wdrażania w środowisku produkcyjnym jest podobny do procesu przemieszczania.

1. Skopiuj wymagane hasło z pliku *publishsettings* , który został pobrany wcześniej dla produkcyjnej witryny sieci Web.
2. Otwórz **wiersz polecenia dla deweloperów dla VS2012**.
3. Wprowadź następujące polecenie w wierszu polecenia, zastępując ścieżkę do pliku rozwiązania ścieżką do pliku rozwiązania i hasłem hasła:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    W przypadku rzeczywistej lokacji produkcyjnej, jeśli wprowadzono również zmiany w bazie danych, należy zwykle skopiować *aplikację\_pliku offline. htm* do lokacji przed wdrożeniem i usunąć ją po pomyślnym wdrożeniu.
4. Otwórz przeglądarkę i przejdź do adresu URL witryny tymczasowej, a następnie kliknij stronę **informacje** , aby sprawdzić, czy wdrożenie zakończyło się pomyślnie.

## <a name="summary"></a>Podsumowanie

Aktualizacja aplikacji została wdrożona przy użyciu wiersza polecenia.

![Informacje o stronie w środowisku testowym](command-line-deployment/_static/image6.png)

W następnym samouczku zobaczysz przykład sposobu rozszerania potoku publikowania w sieci Web. W przykładzie pokazano, jak wdrożyć pliki, które nie są uwzględnione w projekcie.

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-database-update.md)
> [dalej](deploying-extra-files.md)

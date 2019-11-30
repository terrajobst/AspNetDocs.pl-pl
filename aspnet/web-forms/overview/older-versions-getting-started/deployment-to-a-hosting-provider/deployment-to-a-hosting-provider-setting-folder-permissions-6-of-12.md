---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Ustawianie uprawnień do folderów — 6 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 85a77a196cf3458bbb2e6308838a846936cd070b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633507"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: Ustawianie uprawnień do folderów — 6 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku ustawisz uprawnienia folderu dla folderu *ELMAH* w wdrożonej witrynie sieci Web, aby aplikacja mogła tworzyć pliki dziennika w tym folderze.

Podczas testowania aplikacji sieci Web w programie Visual Studio przy użyciu programu Visual Studio Development Server (Cassini) aplikacja jest uruchamiana w ramach Twojej tożsamości. Najprawdopodobniej jesteś administratorem na swoim komputerze deweloperskim i masz pełne uprawnienia do dowolnych plików w dowolnym folderze. Jednak gdy aplikacja działa w ramach usług IIS, jest uruchamiana w ramach tożsamości zdefiniowanej dla puli aplikacji, do której jest przypisana lokacja. Jest to zazwyczaj zdefiniowane przez system konto, które ma ograniczone uprawnienia. Domyślnie ma uprawnienia do odczytu i wykonywania w plikach i folderach aplikacji sieci Web, ale nie ma dostępu do zapisu.

Jest to problem, jeśli aplikacja tworzy lub aktualizuje pliki, które są typowymi potrzebami w aplikacjach sieci Web. W aplikacji uniwersytetów firmy Contoso program ELMAH tworzy pliki XML w folderze *ELMAH* w celu zapisania szczegółowych informacji o błędach. Nawet jeśli nie używasz czegoś takiego jak ELMAH, witryna może umożliwić użytkownikom przekazywanie plików lub wykonywanie innych zadań, które zapisują dane do folderu w witrynie.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testowanie rejestrowania błędów i raportowania

Aby zobaczyć, jak aplikacja nie działa prawidłowo w usługach IIS (chociaż została przetestowana w programie Visual Studio), można spowodować błąd, który normalnie będzie rejestrowany przez program ELMAH, a następnie otworzyć dziennik błędów programu ELMAH, aby wyświetlić szczegóły. Jeśli ELMAH nie mógł utworzyć pliku XML i zapisać szczegóły błędu, zobaczysz pusty raport o błędach.

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie Zażądaj nieprawidłowego adresu URL, takiego jak *Studentsxxx. aspx*. Zostanie wyświetlona strona błędu wygenerowana przez system zamiast strony *GenericErrorPage. aspx* , ponieważ ustawienie `customErrors` w pliku Web. config ma wartość "RemoteOnly", a program IIS jest uruchomiony lokalnie:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Teraz uruchom *ELMAH. axd* , aby zobaczyć raport o błędach. Zostanie wyświetlona pusta strona dziennika błędów, ponieważ program ELMAH nie mógł utworzyć pliku XML w folderze *ELMAH* :

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Ustawianie uprawnienia do zapisu w folderze ELMAH

Uprawnienia do folderów można ustawiać ręcznie lub w ramach procesu wdrażania. Automatyczne wymaganie złożonego kodu MSBuild jest wymagane, ponieważ należy to zrobić tylko przy pierwszym wdrożeniu. w tym samouczku pokazano, jak to zrobić ręcznie. (Aby uzyskać informacje na temat sposobu wykonywania tej części procesu wdrażania, zobacz [Ustawianie uprawnień do folderu w blogu publikowanie w sieci Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na Sayed Hashimi).

W **Eksploratorze Windows**przejdź do *C:\inetpub\wwwroot\ContosoUniversity*. Kliknij prawym przyciskiem myszy folder *ELMAH* , wybierz polecenie **Właściwości**, a następnie wybierz kartę **zabezpieczenia** .

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Jeśli na liście **nazw grup lub użytkowników** nie widzisz opcji **Domyślna** , prawdopodobnie użyto innej metody niż określona w tym samouczku, aby skonfigurować usługi IIS i ASP.NET 4 na komputerze. W takim przypadku należy dowiedzieć się, jaka tożsamość jest używana przez pulę aplikacji przypisaną do aplikacji firmy Contoso University, i przyznać uprawnienia do zapisu dla tej tożsamości. Zobacz linki dotyczące tożsamości pul aplikacji na końcu tego samouczka.)

Kliknij przycisk **Edytuj**. W oknie dialogowym **uprawnienia dla programu ELMAH** wybierz pozycję **Domyślna pula aplikacji**, a następnie zaznacz pole wyboru **zapis** w kolumnie **Zezwalaj** .

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Kliknij przycisk **OK** w obu oknach dialogowych.

## <a name="retesting-error-logging-and-reporting"></a>Testowanie rejestrowania błędów i raportowania

Przetestuj, powodując błąd ponownie w ten sam sposób (Zażądaj nieprawidłowego adresu URL) i Uruchom stronę **dziennika błędów** . Tym razem błąd pojawia się na stronie.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Musisz również mieć uprawnienie do zapisu w folderze *danych\_aplikacji* , ponieważ masz SQL Server Compact pliki bazy danych w tym folderze i chcesz mieć możliwość aktualizowania danych w tych bazach. W takim przypadku nie trzeba wykonywać żadnych dodatkowych czynności, ponieważ proces wdrażania automatycznie ustawia uprawnienie do zapisu w folderze *danych\_aplikacji* .

Wykonano teraz wszystkie zadania niezbędne do poprawnego działania programu Contoso University w usługach IIS na komputerze lokalnym. W następnym samouczku udostępnisz tę witrynę publicznie, wdrażając ją u dostawcy hostingu.

## <a name="more-information"></a>Więcej informacji

W tym przykładzie powód, dla którego nie można było zapisać plików dziennika. Śledzenia usług IIS można użyć w przypadkach, gdy przyczyna problemu nie jest tak oczywista; Zobacz [Rozwiązywanie problemów z nieudanymi żądaniami przy użyciu śledzenia w usługach IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) w witrynie IIS.NET.

Aby uzyskać więcej informacji na temat przyznawania uprawnień do tożsamości puli aplikacji, zobacz [tożsamość puli aplikacji](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) i [bezpieczna zawartość w usługach IIS za pomocą list ACL systemu plików](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) w witrynie IIS.NET.

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [dalej](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)

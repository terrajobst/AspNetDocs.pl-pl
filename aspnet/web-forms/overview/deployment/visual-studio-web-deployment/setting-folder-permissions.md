---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: Ustawianie uprawnień do folderów | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 410525bb2e3f6e5a0be6d7d6b33fb3a40509041a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576063"
---
# <a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: Ustawianie uprawnień do folderów

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku ustawisz uprawnienia folderu dla folderu *ELMAH* w wdrożonej witrynie sieci Web, aby aplikacja mogła tworzyć pliki dziennika w tym folderze.

W przypadku testowania aplikacji sieci Web w programie Visual Studio przy użyciu programu Visual Studio Development Server (Cassini) lub IIS Express aplikacja działa w ramach Twojej tożsamości. Najprawdopodobniej jesteś administratorem na swoim komputerze deweloperskim i masz pełne uprawnienia do dowolnych plików w dowolnym folderze. Jednak gdy aplikacja działa w ramach usług IIS, jest uruchamiana w ramach tożsamości zdefiniowanej dla puli aplikacji, do której jest przypisana lokacja. Jest to zazwyczaj zdefiniowane przez system konto, które ma ograniczone uprawnienia. Domyślnie ma uprawnienia do odczytu i wykonywania w plikach i folderach aplikacji sieci Web, ale nie ma dostępu do zapisu.

Jest to problem, jeśli aplikacja tworzy lub aktualizuje pliki, które są typowymi potrzebami w aplikacjach sieci Web. W aplikacji uniwersytetów firmy Contoso program ELMAH tworzy pliki XML w folderze *ELMAH* w celu zapisania szczegółowych informacji o błędach. Nawet jeśli nie używasz czegoś takiego jak ELMAH, witryna może umożliwić użytkownikom przekazywanie plików lub wykonywanie innych zadań, które zapisują dane do folderu w witrynie.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Rejestrowanie i raportowanie błędów testów

Aby zobaczyć, jak aplikacja nie działa prawidłowo w usługach IIS (chociaż została przetestowana w programie Visual Studio), można spowodować błąd, który normalnie będzie rejestrowany przez program ELMAH, a następnie otworzyć dziennik błędów programu ELMAH, aby wyświetlić szczegóły. Jeśli ELMAH nie mógł utworzyć pliku XML i zapisać szczegóły błędu, zobaczysz pusty raport o błędach.

Otwórz przeglądarkę i przejdź do `http://localhost/ContosoUniversity`, a następnie Zażądaj nieprawidłowego adresu URL, takiego jak *Studentsxxx. aspx*. Zostanie wyświetlona strona błędu wygenerowana przez system zamiast strony *GenericErrorPage. aspx* , ponieważ ustawienie `customErrors` w pliku Web. config ma wartość "RemoteOnly", a program IIS jest uruchomiony lokalnie:

![Strona błędu HTTP 404](setting-folder-permissions/_static/image1.png)

Teraz uruchom *ELMAH. axd* , aby zobaczyć raport o błędach. Po zalogowaniu się przy użyciu poświadczeń konta administratora (&quot;administrator&quot; i &quot;devpwd&quot;) zostanie wyświetlona pusta strona dziennika błędów, ponieważ program ELMAH nie mógł utworzyć pliku XML w folderze *ELMAH* :

![Pusty dziennik błędów](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Ustaw uprawnienie do zapisu w folderze ELMAH

Uprawnienia do folderów można ustawiać ręcznie lub w ramach procesu wdrażania. Automatyczne wymaganie złożonego kodu MSBuild jest wymagane, a ponieważ trzeba to zrobić przy pierwszym wdrożeniu, należy wykonać następujące czynności, aby zrobić to ręcznie. (Aby uzyskać informacje na temat sposobu wykonywania tej części procesu wdrażania, zobacz [Ustawianie uprawnień do folderu w blogu publikowanie w sieci Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na Sayed Hashimi).

1. W **Eksploratorze plików**przejdź do *C:\inetpub\wwwroot\ContosoUniversity*. Kliknij prawym przyciskiem myszy folder *ELMAH* , wybierz polecenie **Właściwości**, a następnie wybierz kartę **zabezpieczenia** .
2. Kliknij pozycję **Edytuj**.
3. W oknie dialogowym **uprawnienia dla programu ELMAH** wybierz pozycję **Domyślna pula aplikacji**, a następnie zaznacz pole wyboru **zapis** w kolumnie **Zezwalaj** .

    ![Uprawnienia dla folderu ELMAH](setting-folder-permissions/_static/image3.png)

    (Jeśli na liście **nazw grup lub użytkowników** nie widzisz opcji **Domyślna** , prawdopodobnie użyto innej metody niż określona w tym samouczku, aby skonfigurować usługi IIS i ASP.NET 4 na komputerze. W takim przypadku należy dowiedzieć się, jaka tożsamość jest używana przez pulę aplikacji przypisaną do aplikacji firmy Contoso University, i przyznać uprawnienia do zapisu dla tej tożsamości. Zobacz linki dotyczące tożsamości pul aplikacji na końcu tego samouczka.) Kliknij przycisk **OK** w obu oknach dialogowych.

## <a name="retest-error-logging-and-reporting"></a>Testowanie rejestrowania błędów i raportowania

Przetestuj, powodując błąd ponownie w ten sam sposób (Zażądaj nieprawidłowego adresu URL) i Uruchom stronę **dziennika błędów** . Tym razem błąd pojawia się na stronie.

![Strona dziennika błędów ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Podsumowanie

Wykonano teraz wszystkie zadania niezbędne do poprawnego działania programu Contoso University w usługach IIS na komputerze lokalnym. W następnym samouczku udostępnisz tę witrynę publicznie, wdrażając ją na platformie Azure.

## <a name="more-information"></a>Więcej informacji

W tym przykładzie powód, dla którego nie można było zapisać plików dziennika. Śledzenia usług IIS można użyć w przypadkach, gdy przyczyna problemu nie jest tak oczywista; Zobacz [Rozwiązywanie problemów z nieudanymi żądaniami przy użyciu śledzenia w usługach IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) w witrynie IIS.NET.

Aby uzyskać więcej informacji na temat przyznawania uprawnień do tożsamości puli aplikacji, zobacz [tożsamość puli aplikacji](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) i [bezpieczna zawartość w usługach IIS za pomocą list ACL systemu plików](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) w witrynie IIS.NET.

> [!div class="step-by-step"]
> [Poprzednie](deploying-to-iis.md)
> [dalej](deploying-to-production.md)

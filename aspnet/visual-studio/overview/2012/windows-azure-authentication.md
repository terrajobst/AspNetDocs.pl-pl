---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Uwierzytelnianie systemu Windows Azure | Microsoft Docs
author: Rick-Anderson
description: Narzędzia Microsoft ASP.NET dla systemu Windows Azure Active Directory ułatwiają uwierzytelnianie aplikacji sieci Web hostowanych w witrynach sieci Web systemu Windows Azure...
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 41c4e6d02c965c10aa35b882964f4f04d9b8c44b
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075154"
---
# <a name="windows-azure-authentication"></a>Uwierzytelnianie platformy Microsoft Azure

Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Narzędzia Microsoft ASP.NET dla systemu Windows Azure Active Directory ułatwiają uwierzytelnianie aplikacji sieci Web hostowanych w [witrynach sieci Web systemu Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Za pomocą uwierzytelniania platformy Microsoft Azure można uwierzytelniać użytkowników pakietu Office 365 w organizacji, konta firmowe synchronizowane z lokalnymi Active Directoryami lub użytkownikami utworzonymi w niestandardowej domenie Azure Active Directory systemu Windows. Włączenie uwierzytelniania systemu Windows Azure umożliwia skonfigurowanie aplikacji do uwierzytelniania użytkowników przy użyciu jednej dzierżawy [systemu windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .
>
> Narzędzie uwierzytelniania systemu Windows Azure ASP.NET nie jest obsługiwane w przypadku ról sieci Web w usłudze w chmurze, ale planujemy to zrobić w przyszłej wersji. Platforma [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) jest obsługiwana w rolach sieci Web systemu Windows Azure.
>
> Aby uzyskać szczegółowe informacje na temat konfigurowania synchronizacji między lokalnym Active Directory i dzierżawą Azure Active Directory systemu Windows, zobacz [używanie AD FS 2,0 do implementowania logowania jednokrotnego i zarządzania nim](https://technet.microsoft.com/library/jj205462.aspx).
>
> Usługa Windows Azure Active Directory jest obecnie dostępna jako [bezpłatna wersja zapoznawcza](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="requirements"></a>Wymagania:

- Visual Studio 2012 lub [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Rozszerzenia Web Tools dla rozszerzeń Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) lub [web tools dla Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools for windows Azure Active Directory — Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) lub [Microsoft ASP.net tools for windows Azure Active Directory — Visual Studio Express 2012 dla sieci Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Tworzenie aplikacji sieci Web ASP.NET za pomocą programu Visual Studio 2012

Możesz utworzyć dowolną aplikację sieci Web za pomocą programu Visual Studio 2012, w tym samouczku zostanie użyty szablon intranetu ASP.NET MVC.

1. Utwórz nową aplikację intranetową ASP.NET MVC 4 i Zaakceptuj wszystkie ustawienia domyślne. (Musi to być w **tra** sieci, a nie w projekcie w sieci **Ter** ).
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Włącz uwierzytelnianie systemu Azure Window (gdy jesteś administratorem globalnym cechą)

Jeśli nie masz istniejącej dzierżawy systemu Windows Azure Active Directory (na przykład za pomocą istniejącego konta Office 365), możesz utworzyć nową dzierżawę, rejestrując się w celu utworzenia [nowego konta systemu Windows Azure Active Directory](https://g.microsoftonline.com/0AX00en/5).

1. W menu Projekt wybierz opcję **Włącz uwierzytelnianie systemu Windows Azure**:

   ![](windows-azure-authentication/_static/image2.png)

2. Wprowadź domenę dla dzierżawy Azure Active Directory systemu Windows (na przykład contoso.onmicrosoft.com), a następnie kliknij pozycję **Włącz**:

![](windows-azure-authentication/_static/image3.png)

3. W oknie dialogowym Uwierzytelnianie sieci Web Zaloguj się jako Administrator dzierżawy systemu Windows Azure Active Directory:

   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Włącz platformę Azure w systemie Windows przez administratora cechą

Jeśli nie masz uprawnień administratora globalnego dla dzierżawy systemu Windows Azure Active Directory, możesz usunąć zaznaczenie pola wyboru w celu aprowizacji aplikacji.

![](windows-azure-authentication/_static/image6.png)

W oknie dialogowym zostanie wyświetlona **domena**, **Identyfikator podmiotu zabezpieczeń aplikacji** i **adres URL odpowiedzi** , które są wymagane do aprowizacji aplikacji za pomocą Azure Active Directory cechą. Musisz przekazać te informacje osobie, która ma wystarczające uprawnienia do aprowizacji aplikacji. Zobacz[jak zaimplementować Logowanie jednokrotne za pomocą aplikacji Windows Azure Active Directory-ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) , aby uzyskać szczegółowe informacje dotyczące sposobu użycia polecenia cmdlet do ręcznego tworzenia jednostki usługi.
Po pomyślnym udostępnieniu aplikacji można kliknąć pozycję **Kontynuuj, aby zaktualizować plik Web. config przy użyciu wybranych ustawień**. Jeśli chcesz kontynuować tworzenie aplikacji podczas oczekiwania na zainicjowanie obsługi, możesz kliknąć przycisk **Zamknij, aby zapamiętać ustawienia w pliku projektu**. Przy następnym wywołaniu opcji Włącz uwierzytelnianie systemu Windows Azure i usuń zaznaczenie pola wyboru aprowizacji, zobaczysz te same ustawienia, a następnie możesz kliknąć przycisk **Kontynuuj**, a następnie kliknąć przycisk **Zastosuj te ustawienia w pliku Web. config**.

1. Zaczekaj, aż aplikacja jest skonfigurowana do uwierzytelniania w systemie Windows Azure i została zainicjowana z systemem Windows Azure Active Directory.
2. Po włączeniu uwierzytelniania systemu Windows Azure dla aplikacji kliknij przycisk **Zamknij:**

    ![](windows-azure-authentication/_static/image7.png)
3. Naciśnij klawisz F5, aby uruchomić aplikację. Należy automatycznie przekierować do strony logowania. Użyj poświadczeń użytkownika cechą Directory, aby zalogować się do aplikacji...

    ![](windows-azure-authentication/_static/image1.jpg)
4. Ponieważ Twoja aplikacja używa obecnie certyfikatu testowego z podpisem własnym, otrzymasz ostrzeżenie z przeglądarki, że certyfikat nie został wystawiony przez zaufany urząd certyfikacji.

    To ostrzeżenie można bezpiecznie zignorować podczas lokalnego tworzenia, klikając pozycję **Kontynuuj w tej witrynie sieci Web:**

    ![](windows-azure-authentication/_static/image8.png)
5. Pomyślnie zalogowano się do aplikacji przy użyciu uwierzytelniania systemu Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Włączenie uwierzytelniania systemu Windows Azure wprowadza następujące zmiany w aplikacji:

- Do projektu zostanie dodana Klasa[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))(Anti-krzyżowe żądanie fałszerstwa) ( *aplikacja\_Start\AntiXsrfConfig.cs* ).
- Pakiety NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` są dodawane do projektu.
- Ustawienia usługi Windows Identity Foundation w aplikacji zostaną skonfigurowane tak, aby akceptowały tokeny zabezpieczające z dzierżawy Azure Active Directory systemu Windows. Kliknij poniższy obraz, aby wyświetlić rozwinięty widok zmian wprowadzonych w pliku *Web. config* .

     ![](windows-azure-authentication/_static/image9.png)
- Zostanie zainicjowana nazwa główna usługi dla aplikacji w dzierżawie systemu Windows Azure Active Directory.
- Protokół HTTPS jest włączony.

## <a name="deploy-the-application-to-windows-azure"></a>Wdrażanie aplikacji w systemie Windows Azure

Aby uzyskać pełne instrukcje, zobacz [wdrażanie aplikacji sieci web ASP.NET w witrynie sieci Web systemu Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Aby opublikować aplikację przy użyciu uwierzytelniania systemu Windows Azure w witrynie sieci Web platformy Azure:

1. Kliknij prawym przyciskiem myszy aplikację i wybierz polecenie **Publikuj:**

    ![](windows-azure-authentication/_static/image3.jpg)
2. W oknie dialogowym Publikowanie w sieci Web Pobierz i zaimportuj profil publikowania dla witryny sieci Web systemu Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. Na karcie **połączenie** jest wyświetlany **docelowy adres URL** (publiczny adres URL dla aplikacji). Kliknij pozycję **Weryfikuj połączenie** , aby przetestować połączenie:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Jeśli opublikowano w tej witrynie sieci Web platformy Azure, należy wcześniej rozważyć sprawdzenie ustawienia **Usuń dodatkowe pliki w miejscu docelowym** , aby upewnić się, że aplikacja jest w sposób przejrzysty. Zwróć uwagę na to pole wyboru **Włącz uwierzytelnianie systemu Windows Azure** jest zaznaczone.

    ![](windows-azure-authentication/_static/image10.png)
5. Opcjonalnie: na karcie **Podgląd** kliknij pozycję **Rozpocznij podgląd** , aby zobaczyć wdrożone pliki.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Kliknij przycisk **Publikuj.**

    Zostanie wyświetlony monit o włączenie uwierzytelniania systemu Windows Azure dla hosta docelowego. Kliknij pozycję **Włącz** , aby kontynuować:

    ![](windows-azure-authentication/_static/image11.png)
7. Wprowadź poświadczenia administratora dla dzierżawy usługi Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Po pomyślnym opublikowaniu aplikacji Przeglądarka zostanie otwarta w opublikowanej witrynie sieci Web.

    > [!NOTE]
    > Po włączeniu uwierzytelniania systemu Windows Azure dla hosta docelowego może upłynąć do 5 minut (zwykle musi to Azure Active Directory być mniejsze). Po pierwszym uruchomieniu aplikacji w przypadku otrzymania błędu ACS50001: nie znaleziono jednostki uzależnionej o nazwie "[Realm]", odczekaj kilka minut i spróbuj ponownie uruchomić aplikację.
9. Po wyświetleniu monitu zaloguj się jako użytkownik w katalogu:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Pomyślnie zalogowano się do aplikacji hostowanej na platformie Azure przy użyciu uwierzytelniania platformy Windows Azure.

     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Znane problemy

#### <a name="role-based-authorization-fails-when-using-windows-azure-authentication"></a>Autoryzacja oparta na rolach kończy się niepowodzeniem w przypadku korzystania z uwierzytelniania systemu Windows Azure

Uwierzytelnianie systemu Windows Azure nie udostępnia obecnie wymaganego żądania roli, aby można było wykonać autoryzację opartą na rolach. Rola uwierzytelnionego użytkownika musi zostać pobrana ręcznie z Azure Active Directory systemu Windows.

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-sts"></a>Przechodzenie do aplikacji z uwierzytelnianiem w systemie Windows Azure skutkuje błędem "ACS20016 domena zalogowanego użytkownika (live.com) nie jest zgodna z żadną dopuszczalną domeną tej usługi STS"

Jeśli użytkownik jest już zalogowany na koncie Microsoft (na przykład hotmail.com, live.com, outlook.com) i próbujesz uzyskać dostęp do aplikacji, która włączyła uwierzytelnianie w systemie Windows Azure, możesz otrzymać 400 odpowiedź na błąd, ponieważ domena konta Microsoft nie jest rozpoznawany przez Azure Active Directory systemu Windows. Aby zalogować się do aplikacji, najpierw Wyloguj się z konta Microsoft.

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificate"></a>Logowanie do aplikacji z włączonym uwierzytelnianiem systemu Windows Azure i X509CertificateValidationMode inne niż None powoduje błędy walidacji certyfikatu dla certyfikatu accounts.accesscontrol.windows.net

Walidacja certyfikatu nie jest wymagana i powinna pozostać wyłączona. Odcisk palca certyfikatu wystawcy jest weryfikowany przez elementu WSFederationAuthenticationModule.

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-sts"></a>Podczas próby włączenia uwierzytelniania systemu Windows Azure w oknie dialogowym Uwierzytelnianie sieci Web zostanie wyświetlony komunikat o błędzie "ACS20016: domena zalogowanego użytkownika (contoso.onmicrosoft.com) nie jest zgodna z żadną dopuszczalną domeną tej usługi STS".

Ten błąd może pojawić się po pomyślnym zalogowaniu się przy użyciu innego konta Azure Active Directory systemu Windows z tego samego procesu programu Visual Studio. Wyloguj się z określonego konta lub Uruchom ponownie program Visual Studio. Jeśli wcześniej zalogowano się i wybrano opcję "nie wylogowuj mnie", może być konieczne wyczyszczenie plików cookie w przeglądarce.

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message"></a>ACS20012: żądanie nie jest prawidłowym komunikatem protokołu WS-Federation

Taka sytuacja może wystąpić, jeśli użytkownik zalogował się już przy użyciu innego identyfikatora firmy Microsoft do jednej z usług platformy Azure. Używaj prywatnego okna przeglądarki, takiego jak InPrivate w programie IE lub incognito w przeglądarce Chrome, lub wyczyść wszystkie pliki cookie.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Microsoft ASP.NET Tools for Windows Azure Active Directory — Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) — Vittorio Bertocci
- [Funkcje systemu Windows Azure: tożsamość](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Azure Active Directory systemu Windows](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: Programowanie aplikacji dla organizacji](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: Programowanie aplikacji dla wielu organizacji](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Jak zaimplementować Logowanie jednokrotne w systemie Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Logowanie jednokrotne z systemem Windows Azure Active Directory: głębokie szczegółowe](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) — Vittorio Bertocci
- [Zaimplementowanie logowania jednokrotnego i zarządzanie nim za pomocą AD FS 2,0](https://technet.microsoft.com/library/jj205462.aspx)

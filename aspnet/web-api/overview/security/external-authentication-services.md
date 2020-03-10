---
uid: web-api/overview/security/external-authentication-services
title: Zewnętrzne usługi uwierzytelniania z interfejsem API sieciC#Web ASP.NET () | Microsoft Docs
author: rmcmurray
description: Opisuje korzystanie z usług uwierzytelniania zewnętrznego w interfejsie API sieci Web ASP.NET.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555476"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a>Zewnętrzne usługi uwierzytelniania z interfejsem API sieciC#Web ASP.NET ()

Programy Visual Studio 2017 i ASP.NET 4.7.2 rozszerzają opcje zabezpieczeń dla [aplikacji jednostronicowych](../../../single-page-application/index.md) (Spa) i usług [internetowego interfejsu API](../../index.md) w celu integracji z zewnętrznymi usługami uwierzytelniania, które obejmują kilka usług uwierzytelniania OAuth/OpenID Connect i mediów społecznościowych: konta Microsoft, Twitter, Facebook i Google.  

### <a name="in-this-walkthrough"></a>W tym instruktażu

- [Korzystanie z usług uwierzytelniania zewnętrznego](#USING)
- [Tworzenie przykładowej aplikacji sieci Web](#SAMPLE)
- [Włączanie uwierzytelniania w usłudze Facebook](#FACEBOOK)
- [Włączanie uwierzytelniania Google](#GOOGLE)
- [Włączanie uwierzytelniania firmy Microsoft](#MICROSOFT)
- [Włączanie uwierzytelniania w usłudze Twitter](#TWITTER)
- [Dodatkowe informacje](#MOREINFO)

    - [Łączenie usług uwierzytelniania zewnętrznego](#COMBINE)
    - [Konfigurowanie IIS Express do używania w pełni kwalifikowanej nazwy domeny](#FQDN)
    - [Jak uzyskać ustawienia aplikacji na potrzeby uwierzytelniania firmy Microsoft](#OBTAIN)
    - [Opcjonalne: wyłącz rejestrację lokalną](#DISABLE)

### <a name="prerequisites"></a>Wymagania wstępne

Aby postępować zgodnie z przykładami w tym instruktażu, należy dysponować następującymi informacjami:

- Visual Studio 2017
- Konto dewelopera z identyfikatorem aplikacji i kluczem tajnym dla jednej z następujących usług uwierzytelniania mediów społecznościowych:

  - Konta Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
  - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
  - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))
  - Google ([https://developers.google.com/](https://developers.google.com))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Korzystanie z usług uwierzytelniania zewnętrznego

Liczebność zewnętrznych usług uwierzytelniania, które są obecnie dostępne dla deweloperów sieci Web, ułatwia skrócenie czasu projektowania podczas tworzenia nowych aplikacji sieci Web. Użytkownicy sieci Web mają zwykle kilka istniejących kont dla popularnych usług sieci Web i witryn internetowych mediów społecznościowych, dlatego gdy aplikacja sieci Web implementuje usługi uwierzytelniania z zewnętrznej usługi sieci Web lub witryny sieci Web mediów społecznościowych, zapisuje czas projektowania, który wykorzystano Tworzenie implementacji uwierzytelniania. Korzystanie z zewnętrznej usługi uwierzytelniania powoduje, że użytkownicy końcowi nie muszą tworzyć innego konta dla aplikacji sieci Web, a także zapamiętać inną nazwę użytkownika i hasło.

W przeszłości deweloperzy mieli dwie opcje: Utwórz własną implementację uwierzytelniania lub Dowiedz się, jak zintegrować zewnętrzną usługę uwierzytelniania z aplikacjami. Na poniższym diagramie przedstawiono prosty przepływ żądania dla agenta użytkownika (przeglądarki sieci Web), który żąda informacji z aplikacji sieci Web skonfigurowanej do korzystania z zewnętrznej usługi uwierzytelniania:

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

Na powyższym diagramie agent użytkownika (lub przeglądarka sieci Web w tym przykładzie) wysyła żądanie do aplikacji sieci Web, która przekierowuje przeglądarkę internetową do zewnętrznej usługi uwierzytelniania. Agent użytkownika wysyła swoje poświadczenia do zewnętrznej usługi uwierzytelniania, a w przypadku pomyślnego uwierzytelnienia agenta użytkownika zewnętrzna usługa uwierzytelniania przekierowuje agenta użytkownika do oryginalnej aplikacji sieci Web przy użyciu jakiejś postaci tokenu, która Agent użytkownika wyśle do aplikacji sieci Web. Aplikacja sieci Web użyje tokenu, aby sprawdzić, czy agent użytkownika został pomyślnie uwierzytelniony przez zewnętrzną usługę uwierzytelniania, a aplikacja sieci Web może użyć tokenu, aby zebrać więcej informacji na temat agenta użytkownika. Po zakończeniu przetwarzania informacji o agencie aplikacji aplikacja sieci Web zwróci odpowiednią odpowiedź do agenta użytkownika na podstawie jego ustawień autoryzacji.

W drugim przykładzie agent użytkownika negocjuje z aplikacją sieci Web i zewnętrznym serwerem autoryzacji, a aplikacja sieci Web wykonuje dodatkową komunikację z zewnętrznym serwerem autoryzacji w celu pobrania dodatkowych informacji o użytkowniku Odczynnik

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

Programy Visual Studio 2017 i ASP.NET 4.7.2 zapewniają integrację z zewnętrznymi usługami uwierzytelniania dla deweloperów, zapewniając wbudowaną integrację dla następujących usług uwierzytelniania:

- Facebook
- Google
- Konta Microsoft (konta usługi Windows Live ID)
- Twitter

W przykładach w tym instruktażu pokazano, jak skonfigurować poszczególne obsługiwane usługi uwierzytelniania zewnętrznego przy użyciu nowego szablonu aplikacji sieci Web ASP.NET, który jest dostarczany z programem Visual Studio 2017.

> [!NOTE]
> W razie potrzeby może być konieczne dodanie nazwy FQDN do ustawień usługi uwierzytelniania zewnętrznego. To wymaganie jest oparte na ograniczeniach zabezpieczeń niektórych zewnętrznych usług uwierzytelniania, które wymagają, aby nazwa FQDN w ustawieniach aplikacji była zgodna z nazwą FQDN używaną przez klientów. (Kroki dla tego elementu różnią się znacznie dla każdej usługi uwierzytelniania zewnętrznego. należy zapoznać się z dokumentacją dla każdej usługi uwierzytelniania zewnętrznego, aby sprawdzić, czy jest to wymagane i jak skonfigurować te ustawienia.) Jeśli musisz skonfigurować IIS Express do korzystania z nazwy FQDN do testowania tego środowiska, zobacz sekcję [konfigurowanie IIS Express do używania w pełni kwalifikowanej nazwy domeny](#FQDN) w dalszej części tego przewodnika.

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a>Tworzenie przykładowej aplikacji sieci Web

Poniższe kroki przeprowadzą Cię przez proces tworzenia przykładowej aplikacji przy użyciu szablonu aplikacji sieci Web ASP.NET. Ta przykładowa aplikacja zostanie użyta w dalszej części tego przewodnika.

Uruchom program Visual Studio 2017 i wybierz pozycję **Nowy projekt** na stronie początkowej. Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

Gdy zostanie wyświetlone okno dialogowe **Nowy projekt** , wybierz pozycję **zainstalowane** i rozwiń **element C#Wizualizacja** . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web ASP.NET (.NET Framework)** . Wprowadź nazwę projektu i kliknij przycisk **OK**.

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

Po wyświetleniu **nowego projektu ASP.NET** wybierz szablon **aplikacja jednostronicowa** i kliknij pozycję **Utwórz projekt**.

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

Poczekaj, aż program Visual Studio 2017 utworzy projekt.

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

Po zakończeniu tworzenia projektu przez program Visual Studio 2017 Otwórz plik *Startup.auth.cs* , który znajduje się w folderze **Start\_** .

Podczas pierwszego tworzenia projektu żadna z usług uwierzytelniania zewnętrznego nie jest włączona w pliku *Startup.auth.cs* ; Poniżej przedstawiono sposób, w jaki Twój kod może wyglądać, z sekcjami wyróżnionymi w celu włączenia zewnętrznej usługi uwierzytelniania oraz wszelkich odpowiednich ustawień w celu używania kont Microsoft, Twitter, Facebook lub Google w przypadku aplikacji ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Po naciśnięciu klawisza F5 w celu skompilowania i debugowania aplikacji sieci Web zostanie wyświetlony ekran logowania, w którym zobaczysz, że żadne usługi uwierzytelniania zewnętrznego nie zostały zdefiniowane.

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

W poniższych sekcjach opisano sposób włączania poszczególnych usług uwierzytelniania zewnętrznego, które są dostarczane z ASP.NET w programie Visual Studio 2017.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Włączanie uwierzytelniania w usłudze Facebook

Przy użyciu uwierzytelniania w serwisie Facebook wymagane jest utworzenie konta dewelopera w serwisie Facebook, a projekt będzie wymagał identyfikatora aplikacji i klucza tajnego z serwisu Facebook w celu zapełnienia działania. Aby uzyskać informacje na temat tworzenia konta dewelopera w serwisie Facebook i uzyskiwania identyfikatora aplikacji i klucza tajnego, zobacz [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Po uzyskaniu identyfikatora aplikacji i klucza tajnego wykonaj następujące kroki, aby włączyć uwierzytelnianie w serwisie Facebook dla aplikacji sieci Web:

1. Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .

2. Znajdź sekcję uwierzytelnianie w serwisie Facebook kodu:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj identyfikator aplikacji i klucz tajny. Po dodaniu tych parametrów możesz ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce sieci Web zobaczysz, że w serwisie Facebook została zdefiniowana usługa uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. Po kliknięciu przycisku **Facebook** przeglądarka zostanie przekierowana na stronę logowania w serwisie Facebook:

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. Po wprowadzeniu poświadczeń usługi Facebook i kliknięciu przycisku **Zaloguj**, przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z kontem w usłudze Facebook:

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **rejestracji** aplikacja sieci Web wyświetli domyślną **stronę główną** dla Twojego konta w serwisie Facebook:

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Włączanie uwierzytelniania Google

Korzystając z uwierzytelniania Google, należy utworzyć konto dla deweloperów firmy Google, a projekt będzie wymagał działania identyfikatora aplikacji i klucza tajnego z usługi Google. Aby uzyskać informacje na temat tworzenia konta usługi Google Developer i uzyskiwania identyfikatora aplikacji i klucza tajnego, zobacz [https://developers.google.com](https://developers.google.com).

Aby włączyć uwierzytelnianie Google dla aplikacji sieci Web, wykonaj następujące czynności:

1. Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .

2. Znajdź sekcję uwierzytelnianie Google w kodzie:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj identyfikator aplikacji i klucz tajny. Po dodaniu tych parametrów możesz ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce sieci Web zobaczysz, że firma Google została zdefiniowana jako usługa uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. Po kliknięciu przycisku **Google** przeglądarka zostanie przekierowana na stronę logowania Google:

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. Po wprowadzeniu poświadczeń Google i kliknięciu przycisku **Zaloguj konto**Google wyświetli monit o zweryfikowanie, czy aplikacja sieci Web ma uprawnienia dostępu do konta Google:

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. Po kliknięciu przycisku **Akceptuj**przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z kontem Google:

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **rejestracji** aplikacja sieci Web wyświetli domyślną **stronę główną** konta Google:

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Włączanie uwierzytelniania firmy Microsoft

Uwierzytelnianie firmy Microsoft wymaga utworzenia konta dewelopera i wymaga do działania identyfikatora klienta i klucza tajnego klienta. Aby uzyskać informacje na temat tworzenia konta dewelopera firmy Microsoft i uzyskiwania identyfikatora klienta i klucza tajnego klienta, zobacz [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).

Po uzyskaniu klucza klienta i wpisu tajnego klienta wykonaj następujące kroki, aby włączyć uwierzytelnianie firmy Microsoft dla aplikacji sieci Web:

1. Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .

2. Znajdź sekcję uwierzytelniania firmy Microsoft dotyczącą kodu:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj identyfikator klienta i klucz tajny klienta. Po dodaniu tych parametrów możesz ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce internetowej zobaczysz, że firma Microsoft została zdefiniowana jako zewnętrzna usługa uwierzytelniania:

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. Po kliknięciu przycisku **Microsoft** przeglądarka zostanie przekierowana do strony logowania firmy Microsoft:

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. Po wprowadzeniu poświadczeń firmy Microsoft i kliknięciu przycisku **Zaloguj**zostanie wyświetlony monit o zweryfikowanie, czy aplikacja sieci Web ma uprawnienia dostępu do konto Microsoft:

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. Po kliknięciu przycisku **tak**przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z konto Microsoft:

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **Utwórz konto** aplikacja sieci Web wyświetli domyślną **stronę główną** dla konto Microsoft:

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Włączanie uwierzytelniania w usłudze Twitter

Uwierzytelnianie w usłudze Twitter wymaga utworzenia konta dewelopera, które wymaga klucza klienta i wpisu tajnego klienta w celu działania programu. Aby uzyskać informacje na temat tworzenia konta dewelopera usługi Twitter i uzyskiwania klucza klienta i wpisu tajnego klienta, zobacz [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Po uzyskaniu klucza klienta i wpisu tajnego klienta wykonaj następujące kroki, aby włączyć uwierzytelnianie w usłudze Twitter dla aplikacji sieci Web:

1. Gdy projekt jest otwarty w programie Visual Studio 2017, Otwórz plik *Startup.auth.cs* .

2. Znajdź sekcję uwierzytelnianie usługi Twitter w temacie Code:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Usuń znaki &quot;//&quot;, aby usunąć komentarz z wyróżnionych wierszy kodu, a następnie Dodaj klucz klienta i wpis tajny klienta. Po dodaniu tych parametrów możesz ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Po naciśnięciu klawisza F5 w celu otwarcia aplikacji sieci Web w przeglądarce internetowej zobaczysz, że usługa Twitter została zdefiniowana jako zewnętrzny element usługi uwierzytelniania:

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. Po kliknięciu przycisku **Twitter** przeglądarka zostanie przekierowana na stronę logowania do usługi Twitter:

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. Po wprowadzeniu poświadczeń usługi Twitter i kliknięciu przycisku **Autoryzuj aplikację**przeglądarka sieci Web zostanie przekierowana z powrotem do aplikacji sieci Web, co spowoduje wyświetlenie monitu o podanie **nazwy użytkownika** , która ma zostać skojarzona z kontem w usłudze Twitter:

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. Po wprowadzeniu nazwy użytkownika i kliknięciu przycisku **rejestracji** aplikacja sieci Web wyświetli domyślną **stronę główną** konta usługi Twitter:

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać dodatkowe informacje na temat tworzenia aplikacji korzystających z protokołu OAuth i OpenID Connect, zobacz następujące adresy URL:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Łączenie usług uwierzytelniania zewnętrznego

Aby zapewnić większą elastyczność, można w tym samym czasie definiować wiele usług uwierzytelniania zewnętrznego — pozwala to użytkownikom aplikacji sieci Web na korzystanie z konta z dowolnej z włączonych usług uwierzytelniania zewnętrznego:

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a>Konfigurowanie IIS Express do używania w pełni kwalifikowanej nazwy domeny

Niektórzy dostawcy uwierzytelniania zewnętrznego nie obsługują testowania aplikacji przy użyciu adresu HTTP, takiego jak `http://localhost:port/`. Aby obejść ten problem, można dodać statyczne, w pełni kwalifikowane Mapowanie nazw domen (FQDN) do pliku HOSTs i skonfigurować opcje projektu w programie Visual Studio 2017, aby użyć nazwy FQDN do testowania/debugowania. Aby to zrobić, wykonaj następujące kroki:

- Dodawanie statycznej nazwy FQDN mapowania pliku HOSTs:

  1. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień w systemie Windows.
  2. Wpisz następujące polecenie:

      <kbd>Notatnik%WinDir%\system32\drivers\etc\hosts</kbd>
  3. Dodaj wpis podobny do następującego do pliku HOSTs:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Zapisz i zamknij plik HOSTs.

- Skonfiguruj projekt programu Visual Studio tak, aby używał nazwy FQDN:

  1. Gdy projekt jest otwarty w programie Visual Studio 2017, kliknij menu **projekt** , a następnie wybierz właściwości projektu. Na przykład możesz wybrać **Właściwości WebApplication1**.
  2. Wybierz kartę **Sieć Web** .
  3. Wprowadź nazwę FQDN dla <strong>adresu URL projektu</strong>. Można na przykład wprowadzić <kbd><http://www.wingtiptoys.com></kbd> , jeśli było to mapowanie nazwy FQDN dodane do pliku Hosts.

- Skonfiguruj IIS Express do używania nazwy FQDN dla aplikacji:

    1. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień w systemie Windows.
    2. Wpisz następujące polecenie, aby przejść do folderu IIS Express:

        <kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Wpisz następujące polecenie, aby dodać nazwę FQDN do aplikacji:

        <kbd>Appcmd. exe set config-Section: System. applicationHost/sites/+&quot;[name = "WebApplication1"]. powiązania. [Protocol = "http", bindingInformation = "*: 80: www. wingtiptoys. com"]&quot;/commit: AppHost</kbd>

  Gdzie **WebApplication1** jest nazwą projektu, a **bindingInformation** zawiera numer portu i nazwę FQDN, których chcesz użyć do testowania.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Jak uzyskać ustawienia aplikacji na potrzeby uwierzytelniania firmy Microsoft

Łączenie aplikacji z usługą Windows Live na potrzeby uwierzytelniania firmy Microsoft jest procesem prostym. Jeśli aplikacja nie została jeszcze połączona z usługą Windows Live, możesz wykonać następujące czynności:

1. Po wyświetleniu monitu przejdź do [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) i wprowadź nazwę konto Microsoft i hasło, a następnie kliknij przycisk **Zaloguj się**:

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. Wybierz pozycję **Dodaj aplikację** i wprowadź nazwę aplikacji po wyświetleniu monitu, a następnie kliknij pozycję **Utwórz**:

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. Wybierz swoją aplikację w polu **Nazwa** , a zostanie wyświetlona strona właściwości aplikacji.

4. Wprowadź domenę przekierowania dla aplikacji. Skopiuj **Identyfikator aplikacji** i w obszarze wpisy **tajne aplikacji**wybierz pozycję **Generuj hasło**. Skopiuj wyświetlone hasło. Identyfikator aplikacji i hasło to identyfikator klienta i klucz tajny klienta. Wybierz przycisk **OK** , a następnie **Zapisz**.

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Opcjonalne: wyłącz rejestrację lokalną

Bieżąca Funkcja rejestracji lokalnej ASP.NET nie uniemożliwia zautomatyzowanym programom (botów) tworzenia kont członków. na przykład za pomocą technologii zapobiegania bot i sprawdzania poprawności, takiej jak [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). W związku z tym należy usunąć formularz logowania lokalnego i link rejestracji na stronie logowania. Aby to zrobić, Otwórz stronę *\_login. cshtml* w projekcie, a następnie Skomentuj wiersze dla lokalnego panelu logowania i linku rejestracji. Strona wyników powinna wyglądać podobnie do następującego przykładu kodu:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Po wyłączeniu lokalnego panelu logowania i usunięciu linku rejestracji na stronie logowania będą wyświetlane tylko zewnętrzni dostawcy uwierzytelniania:

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)

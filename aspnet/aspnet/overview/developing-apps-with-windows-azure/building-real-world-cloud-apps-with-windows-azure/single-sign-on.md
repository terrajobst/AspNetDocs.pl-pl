---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Logowanie jednokrotne (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 7e32f444dc38132296cffd45ac658f5abf51f314
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585280"
---
# <a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Logowanie jednokrotne (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Istnieje wiele problemów z zabezpieczeniami, które należy wziąć pod uwagę podczas tworzenia aplikacji w chmurze, ale w przypadku tej serii będziemy skupić się na jednym: rejestracji jednokrotnej. Pytania często pytają o to: "jestem głównie tworzeniem aplikacji dla pracowników mojej firmy". Jak hostować aplikacje w chmurze i nadal umożliwiać korzystanie z tego samego modelu zabezpieczeń, który widzą Moi pracownicy w środowisku lokalnym, gdy używają aplikacji hostowanych w zaporze? Jednym ze sposobów włączania tego scenariusza jest nazywa się Azure Active Directory (Azure AD). Usługa Azure AD umożliwia korzystanie z aplikacji biznesowych dla przedsiębiorstw (LOB) dostępnych w Internecie i umożliwia udostępnianie tych aplikacji również partnerom biznesowym.

## <a name="introduction-to-azure-ad"></a>Wprowadzenie do usługi Azure AD

[Usługa Azure AD](https://docs.microsoft.com/azure/active-directory/) zapewnia [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) w chmurze. Najważniejsze funkcje obejmują:

- Integruje się z lokalnym Active Directory.
- Umożliwia logowanie jednokrotne przy użyciu aplikacji.
- Obsługuje ona otwarte standardy, takie jak [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-karmione](http://en.wikipedia.org/wiki/WS-Federation)i [OAuth 2,0](http://oauth.net/2/).
- Obsługuje [interfejs API REST](https://msdn.microsoft.com/library/hh974476.aspx)programu Enterprise Graph.

Załóżmy, że masz lokalne środowisko Active Directory systemu Windows Server, za pomocą którego pracownicy mogą logować się do aplikacji intranetowych:

![](single-sign-on/_static/image1.png)

Usługa Azure AD umożliwia tworzenie katalogu w chmurze. Jest to bezpłatna funkcja, która jest łatwa do skonfigurowania.

Może być całkowicie niezależna od lokalnego Active Directory; Możesz umieścić dowolną osobę w niej i uwierzytelniać ją w aplikacjach internetowych.

![Usługa Active Directory systemu Windows Azure](single-sign-on/_static/image2.png)

Możesz też zintegrować ją z lokalną usługą AD.

![Integracja usług AD i WAAD](single-sign-on/_static/image3.png)

Teraz wszyscy pracownicy, którzy mogą uwierzytelniać się lokalnie, mogą także uwierzytelniać się za pośrednictwem Internetu — bez konieczności otwierania zapory ani wdrażania nowych serwerów w centrum danych. Możesz nadal korzystać ze wszystkich istniejących środowisk Active Directory, które znasz i których używasz, aby zapewnić możliwość logowania jednokrotnego aplikacji wewnętrznych.

Po nawiązaniu tego połączenia między usługą AD i usługą Azure AD możesz także włączyć aplikacje sieci Web i urządzenia przenośne, aby uwierzytelniać pracowników w chmurze, a także włączyć aplikacje innych firm, takie jak Office 365, SalesForce.com lub Google Apps, aby akceptować poświadczenia pracowników. Jeśli korzystasz z pakietu Office 365, masz już skonfigurowaną usługę Azure AD, ponieważ pakiet Office 365 używa usługi Azure AD do uwierzytelniania i autoryzacji.

![aplikacje innych firm](single-sign-on/_static/image4.png)

Estetyki tego podejścia polega na tym, że w miarę dodawania lub usuwania użytkownika przez organizację lub zmiany hasła użytkownik korzysta z tego samego procesu, który jest używany dzisiaj w środowisku lokalnym. Wszystkie lokalne zmiany usługi AD są automatycznie propagowane do środowiska chmury.

Jeśli firma korzysta z usługi lub przeniesiesz ją do pakietu Office 365, dobrą wiadomośćą jest to, że usługa Azure AD jest automatycznie skonfigurowana, ponieważ pakiet Office 365 używa usługi Azure AD do uwierzytelniania. Dzięki temu można łatwo korzystać z własnych aplikacji w ramach tego samego uwierzytelniania, które są używane przez pakiet Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Konfigurowanie dzierżawy usługi Azure AD

Katalog usługi Azure AD jest nazywany [dzierżawą](https://technet.microsoft.com/library/jj573650.aspx)usługi Azure AD, a Konfigurowanie dzierżawy jest bardzo proste. Pokażemy, jak to robimy w portal zarządzania platformy Azure, aby zilustrować koncepcje, ale podobnie jak w przypadku innych funkcji portalu, można również użyć skryptu lub interfejsu API zarządzania.

W portalu zarządzania kliknij kartę Active Directory.

![WAAD w portalu](single-sign-on/_static/image5.png)

Automatycznie masz jedną dzierżawę usługi Azure AD dla Twojego konta platformy Azure. możesz kliknąć przycisk **Dodaj** w dolnej części strony, aby utworzyć dodatkowe katalogi. Można na przykład utworzyć jeden dla środowiska testowego i jeden do produkcji. Uważnie zapoznaj się z nazwą nowego katalogu. Jeśli używasz nazwy dla katalogu, a następnie używasz swojej nazwy dla jednego z użytkowników, co może być mylące.

![Dodaj katalog](single-sign-on/_static/image6.png)

Portal ma pełną obsługę tworzenia i usuwania użytkowników w ramach tego środowiska oraz zarządzania nimi. Na przykład, aby dodać użytkownika, przejdź do karty **Użytkownicy** i kliknij przycisk **Dodaj użytkownika** .

![Przycisk Dodaj użytkownika](single-sign-on/_static/image7.png)

![Okno dialogowe Dodawanie użytkownika](single-sign-on/_static/image8.png)

Możesz utworzyć nowego użytkownika, który istnieje tylko w tym katalogu. Możesz też zarejestrować konto Microsoft jako użytkownik w tym katalogu lub zarejestrować się lub użytkownika z innego katalogu usługi Azure AD jako użytkownik w tym katalogu. (W rzeczywistym katalogu domena domyślna byłaby ContosoTest.onmicrosoft.com. Możesz również użyć własnej domeny, na przykład contoso.com.)

![Typy użytkowników](single-sign-on/_static/image9.png)

![Okno dialogowe Dodawanie użytkownika](single-sign-on/_static/image10.png)

Użytkownika można przypisać do roli.

![Profil użytkownika](single-sign-on/_static/image11.png)

A konto jest tworzone z hasłem tymczasowym.

![Hasło tymczasowe](single-sign-on/_static/image12.png)

Użytkownicy tworzący ten sposób mogą natychmiast zalogować się do aplikacji sieci Web przy użyciu tego katalogu w chmurze.

Co jest doskonałe dla logowania jednokrotnego w przedsiębiorstwie, ale jest karta **integracji katalogu** :

![Karta integracja katalogów](single-sign-on/_static/image13.png)

Jeśli włączysz integrację katalogów i [pobierzesz narzędzie](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), możesz zsynchronizować ten katalog w chmurze z istniejącym lokalnym Active Directory, który jest już używany w organizacji. Następnie wszyscy użytkownicy przechowywani w katalogu będą wyświetlani w tym katalogu w chmurze. Aplikacje w chmurze mogą teraz uwierzytelniać wszystkich pracowników przy użyciu istniejących poświadczeń Active Directory. Wszystkie te elementy są bezpłatne — zarówno narzędzia synchronizacji, jak i usługi Azure AD.

Narzędzie to Kreator, który jest łatwy w użyciu, ponieważ można go zobaczyć na podstawie zrzutów ekranu. Nie są to instrukcje kompletne — tylko przykład pokazujący podstawowy proces. Aby uzyskać bardziej szczegółowe informacje o tym, jak to zrobić, zobacz linki w sekcji [Resources (zasoby](#resources) ) na końcu rozdziału.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image14.png)

Kliknij przycisk **dalej**, a następnie wprowadź poświadczenia Azure Active Directory.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image15.png)

Kliknij przycisk **dalej**, a następnie wprowadź swoje lokalne poświadczenia usługi AD.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image16.png)

Kliknij przycisk **dalej**, a następnie wskaż, czy chcesz przechowywać skrót haseł usługi AD w chmurze.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image17.png)

Skrót hasła, który można przechowywać w chmurze, to jednokierunkowa wartość skrótu; rzeczywiste hasła nigdy nie są przechowywane w usłudze Azure AD. Jeśli zdecydujesz się na przechowywanie skrótów w chmurze, musisz użyć [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (AD FS). Istnieją również [inne czynniki, które należy wziąć pod uwagę podczas wybierania, czy należy używać usług AD FS](https://technet.microsoft.com/library/jj573653.aspx). Opcja ADFS wymaga kilku dodatkowych kroków konfiguracyjnych.

Jeśli zdecydujesz się na przechowywanie skrótów w chmurze, to znaczy, że narzędzie rozpocznie synchronizowanie katalogów po kliknięciu **przycisku Dalej**.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image18.png)

I w ciągu kilku minut wszystko gotowe.

![Kreator konfiguracji narzędzia synchronizacji WAAD](single-sign-on/_static/image19.png)

Wystarczy uruchomić ten tylko na jednym kontrolerze domeny w organizacji, w systemie Windows 2003 lub nowszym. I nie ma konieczności ponownego uruchamiania. Gdy wszystko będzie gotowe, wszyscy użytkownicy znajdują się w chmurze i można wykonać logowanie jednokrotne z dowolnej aplikacji sieci Web lub mobilnej przy użyciu protokołu SAML, OAuth lub WS-karmione.

Czasami zostanie wyświetlony monit o jego bezpieczeństwo — czy firma Microsoft używa go do własnych poufnych danych firmowych? Odpowiedź to tak. Na przykład jeśli przejdziesz do wewnętrznej witryny programu Microsoft SharePoint w [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), zostanie wyświetlony monit o zalogowanie się.

![Logowanie do pakietu Office 365](single-sign-on/_static/image20.png)

Firma Microsoft włączyła usługi AD FS, więc po wprowadzeniu identyfikatora firmy Microsoft nastąpi przekierowanie do strony logowania usług ADFS.

![Logowanie w usługach ADFS](single-sign-on/_static/image21.png)

Po wprowadzeniu poświadczeń przechowywanych na wewnętrznym koncie usługi Microsoft AD masz dostęp do tej aplikacji wewnętrznej.

![Witryna programu MS SharePoint](single-sign-on/_static/image22.png)

Korzystamy głównie z serwera logowania do usługi AD, ponieważ usługa ADFS została skonfigurowana przed udostępnieniem usługi Azure AD, ale proces logowania odbywa się za pośrednictwem katalogu usługi Azure AD w chmurze. Udostępniamy nasze ważne dokumenty, kontrolę źródła, pliki zarządzania wydajnością, raporty sprzedaży i nie tylko, w chmurze i korzystają z tego samego rozwiązania, aby je zabezpieczyć.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Tworzenie aplikacji ASP.NET używającej usługi Azure AD do logowania jednokrotnego

Program Visual Studio ułatwia tworzenie aplikacji, która używa usługi Azure AD do logowania jednokrotnego, co pozwala zobaczyć kilka zrzutów ekranu.

Podczas tworzenia nowej aplikacji ASP.NET, MVC lub Web Forms, domyślną metodą uwierzytelniania jest ASP.NET Identity. Aby zmienić ten element na usługę Azure AD, kliknij przycisk **Zmień uwierzytelnianie** .

![Zmień uwierzytelnianie](single-sign-on/_static/image23.png)

Wybierz pozycję konta organizacyjne, wprowadź nazwę domeny, a następnie wybierz pozycję Logowanie jednokrotne.

![Okno dialogowe Konfigurowanie uwierzytelniania](single-sign-on/_static/image24.png)

Możesz również udzielić aplikacji uprawnienia Odczyt lub odczyt/zapis dla danych katalogu. Jeśli to zrobisz, można użyć [interfejsu API REST usługi Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) do wyszukiwania numeru telefonu użytkowników, sprawdzić, czy znajdują się w biurze, podczas ostatniego logowania itd.

To wszystko, co trzeba zrobić — program Visual Studio pyta o poświadczenia administratora dzierżawy usługi Azure AD, a następnie konfiguruje zarówno projekt, jak i dzierżawę usługi Azure AD dla nowej aplikacji.

Po uruchomieniu projektu zostanie wyświetlona strona logowania i będzie można zalogować się przy użyciu poświadczeń użytkownika w katalogu usługi Azure AD.

![Logowanie do konta organizacji](single-sign-on/_static/image25.png)

![Zalogowano](single-sign-on/_static/image26.png)

Po wdrożeniu aplikacji na platformie Azure należy zaznaczyć pole wyboru **Włącz uwierzytelnianie organizacyjne** , a po ponownym uruchomieniu program Visual Studio zajmie się całą konfiguracją.

![Publikowanie w sieci Web](single-sign-on/_static/image27.png)

Te zrzuty ekranu pochodzą z pełnego samouczka krok po kroku, w którym pokazano, jak utworzyć aplikację, która używa uwierzytelniania usługi Azure AD: [Tworzenie aplikacji ASP.NET za pomocą Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Podsumowanie

W tym rozdziale zawarto te Azure Active Directory, Visual Studio i ASP.NET, ułatwiają konfigurowanie logowania jednokrotnego w aplikacjach internetowych dla użytkowników w organizacji. Użytkownicy mogą logować się w aplikacjach internetowych przy użyciu tych samych poświadczeń, których używają do logowania się przy użyciu Active Directory w sieci wewnętrznej.

[Następny rozdział](data-storage-options.md) analizuje opcje magazynu danych dostępne dla aplikacji w chmurze.

<a id="resources"></a>
## <a name="resources"></a>Resources

Więcej informacji można znaleźć w następujących zasobach:

- [Dokumentacja Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Strona portalu dokumentacji usługi Azure AD w witrynie windowsazure.com. Samouczki krok po kroku znajdują się w sekcji **programowanie** .
- [Multi-Factor Authentication platformy Azure](https://docs.microsoft.com/azure/multi-factor-authentication/). Strona portalu z dokumentacją dotyczącą uwierzytelniania wieloskładnikowego na platformie Azure.
- [Opcje uwierzytelniania konta organizacyjnego](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Wyjaśnienie opcji uwierzytelniania usługi Azure AD w oknie dialogowym Visual Studio 2013 nowego projektu.
- [Wzorce i praktyki firmy Microsoft — wzorzec tożsamości federacyjnej](https://msdn.microsoft.com/library/dn589790.aspx).
- [Porady: Zainstaluj narzędzie do synchronizacji Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Mapa zawartości Active Directory Federation Services 2,0](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Linki do dokumentacji dotyczącej usług AD FS 2,0.
- [Autoryzacja oparta na rolach i listy ACL w aplikacji usługi AD systemu Windows Azure](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Przykładowa aplikacja.
- [Azure Active Directory interfejs API programu Graph blog](https://blogs.msdn.com/b/aadgraphteam/).
- [Access Control w BYOD i integracji katalogu w hybrydowej infrastrukturze tożsamości](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 wideo sesji przez Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Poprzednie](web-development-best-practices.md)
> [dalej](data-storage-options.md)

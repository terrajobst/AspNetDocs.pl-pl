---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Opracowywanie aplikacji ASP.NET przy użyciu Azure Active Directory-ASP.NET 4. x
author: Rick-Anderson
description: Microsoft ASP.NET narzędzia dla Azure Active Directory ułatwiają uwierzytelnianie aplikacji sieci Web hostowanych na platformie Azure. Możesz użyć usługi Azure wstępnego...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 28425ea8d1312dfc6e14df9677396f2cbcf6f16d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583854"
---
# <a name="developing-aspnet-apps-with-azure-active-directory"></a>Tworzenie aplikacji ASP.NET z wykorzystaniem usługi Azure Active Directory

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

Microsoft ASP.NET narzędzia dla Azure Active Directory upraszczają Włączanie uwierzytelniania dla aplikacji sieci Web hostowanych na [platformie Azure](https://www.windowsazure.com/home/features/web-sites/). Za pomocą uwierzytelniania platformy Azure można uwierzytelniać użytkowników pakietu Office 365 w organizacji, konta firmowe synchronizowane z lokalnymi Active Directory lub użytkownikami utworzonymi w niestandardowej domenie Azure Active Directory. Włączenie uwierzytelniania systemu Windows Azure umożliwia skonfigurowanie aplikacji do uwierzytelniania użytkowników przy użyciu jednej dzierżawy [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .

W tym samouczku pokazano, jak utworzyć aplikację ASP.NET, która jest skonfigurowana do logowania za pomocą usługi [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Dowiesz się również, jak wywoływać interfejs API programu Graph, aby uzyskać informacje o aktualnie zalogowanym użytkowniku oraz o sposobie wdrażania aplikacji na platformie Azure.

## <a name="prerequisites"></a>Wymagania wstępne

1. [Visual Studio Express 2013 dla sieci Web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express) lub [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) — wymagana jest aktualizacja Update 3 lub nowsza.
3. Konto platformy Azure. [Kliknij tutaj](https://azure.microsoft.com/pricing/free-trial/) , aby uzyskać bezpłatną wersję próbną, jeśli jeszcze nie masz konta.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Dodawanie administratora globalnego do Active Directory

1. Zaloguj się do [Portal zarządzania platformy Azure](https://manage.windowsazure.com/).
2. Wszystkie konta platformy Azure zawierają **katalog domyślny** — kliknij go, a następnie kliknij kartę **Users (Użytkownicy** ) w górnej części strony (zobacz poniższy obraz).
3. Kliknij pozycję Dodaj użytkownika.
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Utwórz nowego użytkownika z rolą **administratora globalnego** . Kliknij pozycję **Użytkownicy** w górnym menu, a następnie kliknij przycisk **Dodaj użytkownika** na pasku poleceń.
5. W oknie dialogowym **Dodawanie użytkownika** wprowadź nazwę nowego użytkownika, a następnie kliknij strzałkę w prawo.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Wprowadź nazwę użytkownika i Ustaw rolę na **administratora globalnego**. Administratorzy globalni wymagają alternatywnego adresu e-mail na potrzeby odzyskiwania hasła. Po zakończeniu kliknij strzałkę w prawo.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na następnej stronie okna dialogowego kliknij pozycję **Utwórz**. Dla nowego użytkownika zostanie utworzone hasło tymczasowe, które zostanie wyświetlone w oknie dialogowym.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   Zapisz hasło, musisz zmienić hasło po pierwszym zalogowaniu się. Na poniższej ilustracji przedstawiono nowe konto administratora. Musisz użyć Azure Active Directory, aby zalogować się do aplikacji, a nie konto Microsoft również na tej stronie.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Tworzenie aplikacji ASP.NET

Poniższe kroki używają [Visual Studio Express 2013 dla sieci Web](https://www.microsoft.com/download/details.aspx?id=40747)i wymagają [aktualizacji Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. W programie Visual Studio kliknij polecenie **plik** , a następnie pozycję **Nowy projekt**. W oknie dialogowym **Nowy projekt** wybierz projekt Visual C# Web Project z menu po lewej stronie, a następnie kliknij przycisk **OK**. Można również usunąć zaznaczenie pola wyboru **dodaj Application Insights do projektu** , jeśli nie chcesz, aby funkcja aplikacji była jeszcze niedostępna.
2. W oknie dialogowym **Nowy projekt ASP.NET** wybierz pozycję **MVC**, a następnie kliknij pozycję **Zmień uwierzytelnianie**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. W oknie dialogowym **Zmienianie uwierzytelniania** wybierz pozycję **konta organizacyjne**. Przy użyciu tych opcji można automatycznie zarejestrować aplikację w usłudze Azure AD, a także automatycznie skonfigurować aplikację do integracji z usługą Azure AD. Nie trzeba używać okna dialogowego **Zmienianie uwierzytelniania** w celu zarejestrowania i skonfigurowania aplikacji, ale jest to znacznie prostsze. Jeśli na przykład używasz programu Visual Studio 2012, możesz nadal ręcznie zarejestrować aplikację w usłudze Azure portal zarządzania i zaktualizować jej konfigurację, aby zintegrować ją z usługą Azure AD.
   Z menu rozwijanego wybierz kolejno pozycje **organizacja** i logowanie jednokrotne **, Odczytaj dane katalogu**. Wprowadź domenę dla katalogu usługi Azure AD, na przykład (w poniższych obrazach) *aricka0yahoo.onmicrosoft.com*, a następnie kliknij przycisk **OK**. Możesz uzyskać nazwę domeny z karty domeny dla katalogu domyślnego w witrynie Azure Portal (Zobacz następny obraz w dół).

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   Na poniższej ilustracji przedstawiono nazwę domeny z Azure Portal.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > Opcjonalnie możesz skonfigurować identyfikator URI aplikacji, który zostanie zarejestrowany w usłudze Azure AD, klikając pozycję **więcej opcji**. Identyfikator URI aplikacji jest unikatowym identyfikatorem dla aplikacji, która jest zarejestrowana w usłudze Azure AD i używana przez aplikację do identyfikacji w przypadku komunikacji z usługą Azure AD. Aby uzyskać więcej informacji o IDENTYFIKATORze URI aplikacji i innych właściwościach zarejestrowanych aplikacji, zobacz [ten temat](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Klikając pole wyboru poniżej pola Identyfikator URI aplikacji, możesz również zastąpić istniejącą rejestrację w usłudze Azure AD, która korzysta z tego samego identyfikatora URI aplikacji.
4. Po kliknięciu przycisku **OK**zostanie wyświetlone okno dialogowe logowania i będzie konieczne zalogowanie się przy użyciu konta administratora globalnego (nie konto Microsoft skojarzonego z subskrypcją). Jeśli nowe konto administratora zostało wcześniej utworzone, należy zmienić hasło, a następnie ponownie zalogować się przy użyciu nowego hasła.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Po pomyślnym uwierzytelnieniu w oknie dialogowym **Nowy projekt ASP.NET** zostanie wyświetlona opcja uwierzytelniania (**organizacja** ) i katalog, w którym zostanie zarejestrowana nowa aplikacja (*aricka0yahoo.onmicrosoft.com* na poniższej ilustracji). Poniżej tych informacji zaznacz pole wyboru etykieta **hosta w chmurze**. Jeśli to pole wyboru jest zaznaczone, projekt zostanie zainicjowany jako aplikacja internetowa platformy Azure i będzie można go później łatwo opublikować. Kliknij przycisk **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Zostanie wyświetlone okno dialogowe **Konfigurowanie witryny sieci Web platformy Azure** z automatycznie wygenerowaną nazwą i regionem witryny. Zwróć uwagę na to, że konto, na którym się zalogowano, znajduje się w oknie dialogowym. Aby upewnić się, że to konto jest dołączone do subskrypcji platformy Azure, zwykle jest to konto Microsoft.

    > [!NOTE]
    > Ten projekt wymaga bazy danych. Musisz wybrać jedną z istniejących baz danych lub utworzyć nową. Baza danych jest wymagana, ponieważ projekt używa już lokalnego pliku bazy danych do przechowywania niewielkiej ilości danych konfiguracji uwierzytelniania. Podczas wdrażania aplikacji w witrynie sieci Web platformy Azure ta baza danych nie jest spakowana we wdrożeniu, dlatego należy wybrać jedną z dostępnych w chmurze. Kliknij przycisk **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Projekt zostanie utworzony, a opcje uwierzytelniania i opcje aplikacji sieci Web zostaną automatycznie skonfigurowane przy użyciu projektu. Po zakończeniu tego procesu Uruchom projekt lokalnie, naciskając klawisz **^ F5**. Konieczne będzie zalogowanie się przy użyciu konta organizacyjnego. Podaj nazwę użytkownika i hasło dla utworzonego wcześniej konta, a następnie kliknij przycisk **Zaloguj**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Po pomyślnym zalogowaniu się w witrynie ASP.NET zostanie wyświetlona nazwa użytkownika w prawym górnym rogu strony.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   Jeśli wystąpi błąd: wartość nie może mieć wartości null ani być pusta. Nazwa parametru: linkText ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   Zapoznaj się z sekcją [debugowanie](#dbg) na końcu tego samouczka.

## <a name="basics-of-the-graph-api"></a>Podstawy interfejs API programu Graph

[Interfejs API programu Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) jest interfejs programistyczny używany do wykonywania CRUD i innych operacji na obiektach w katalogu usługi Azure AD. W przypadku wybrania opcji konta organizacyjnego do uwierzytelniania podczas tworzenia nowego projektu w Visual Studio 2013 aplikacja zostanie już skonfigurowana do wywoływania interfejs API programu Graph. W tej sekcji krótko opisano, jak działa interfejs API programu Graph.

1. W uruchomionej aplikacji kliknij nazwę zalogowanego użytkownika w prawym górnym rogu strony. Spowoduje to przejście do strony profilu użytkownika, która jest akcją na kontrolerze głównym. Należy zauważyć, że tabela zawiera informacje o użytkowniku utworzone wcześniej. Te informacje są przechowywane w katalogu, a interfejs API programu Graph jest wywoływana, aby pobrać te informacje podczas ładowania strony.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Wróć do programu Visual Studio i rozwiń folder **controllers** , a następnie otwórz plik **HomeController.cs** . Zobaczysz akcję **USERPROFILE ()** , która zawiera kod umożliwiający pobranie tokenu, a następnie wywołaj interfejs API programu Graph. Ten kod jest zduplikowany poniżej:

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Aby wywołać interfejs API programu Graph, musisz najpierw pobrać token. Po pobraniu tokenu jego wartość ciągu musi być dołączona do nagłówka autoryzacji dla wszystkich kolejnych żądań do interfejs API programu Graph. Większość powyższych kodów obsługuje szczegóły uwierzytelniania w usłudze Azure AD w celu uzyskania tokenu, przy użyciu tokenu do wywołania interfejs API programu Graph, a następnie przekształcenia odpowiedzi w taki sposób, aby mogła być wyświetlana w widoku.

    Najbardziej istotną częścią dyskusji jest następujący wyróżniony wiersz: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Ten wiersz reprezentuje nazwę użytkownika, który został przeszeregowany z odpowiedzi JSON i jest prezentowany w widoku.

    Możesz wywoływać interfejs API programu Graph przy użyciu HttpClient i obsłużyć dane pierwotne, ale łatwiejszym sposobem jest użycie [biblioteki klienta programu Graph, która jest dostępna za pośrednictwem narzędzia NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Biblioteka klienta obsługuje nieprzetworzone żądania HTTP i transformację zwracanych danych, a znacznie ułatwia pracę z interfejs API programu Graph w środowisku .NET. Zobacz przykłady kodu powiązanego interfejs API programu Graph w witrynie [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure

Poniższe kroki pokazują, jak wdrożyć aplikację na platformie Azure. We wcześniejszych krokach został połączony nowy projekt z aplikacją sieci Web na platformie Azure, dzięki czemu można go opublikować w zaledwie kilku krokach.

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Publikuj**. Zostanie wyświetlone okno dialogowe **Publikowanie w sieci Web** z każdym skonfigurowanym już ustawieniem. Kliknij przycisk **dalej** , aby przejść do strony **ustawień** . Może zostać wyświetlony monit o uwierzytelnienie; Upewnij się, że uwierzytelniasz się przy użyciu konta subskrypcji platformy Azure (zwykle konto Microsoft), a nie konta organizacji utworzonego wcześniej.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Zaznacz opcję **Włącz uwierzytelnianie organizacyjne** . W polu **domena** wprowadź domenę dla katalogu. Z listy rozwijanej **poziom dostępu** wybierz pozycję Logowanie jednokrotne **, Odczytaj dane katalogu**. Zobaczysz, że poprzednia baza danych została już wypełniona w sekcji **bazy danych** . Kliknij przycisk **Opublikuj**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Program Visual Studio rozpocznie wdrażanie witryny sieci Web, a następnie zostanie wyświetlone nowe okno przeglądarki. Może zostać wyświetlony monit o ponowne uwierzytelnienie w katalogu. Po uwierzytelnieniu nastąpi przekierowanie do nowo opublikowanej witryny sieci Web na platformie Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debugowanie aplikacji

Jeśli zostanie wyświetlony następujący błąd: wartość nie może mieć wartości null ani być pusta. Nazwa parametru: linkText

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)

Zastąp kod w pliku *Views\Shared\\_LoginPartial. cshtml* następującym:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Po uruchomieniu aplikacji, Jeśli zalogowany użytkownik pokazuje "użytkownika o wartości null", Wyloguj się i zaloguj się ponownie przy użyciu utworzonego wcześniej konta Active Directory.

Doskonałym samouczkiem, które należy wykonać, jest Rick Rainey [szczegółowe: Azure Websites i uwierzytelnianie organizacyjne przy użyciu usługi Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Więcej informacji

- [Głębokie szczegółowe: usługi Azure Websites i uwierzytelnianie organizacyjne przy użyciu usługi Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Omówienie usługi Azure AD interfejs API programu Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scenariusze uwierzytelniania w usłudze Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Przykłady kodu usługi Azure AD w witrynie GitHub](https://github.com/AzureADSamples)

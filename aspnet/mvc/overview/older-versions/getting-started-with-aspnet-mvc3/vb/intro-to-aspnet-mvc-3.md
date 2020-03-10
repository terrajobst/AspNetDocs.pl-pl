---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Wprowadzenie do ASP.NET MVC 3 (VB) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 24f7de303ef7f5a457bd509ecc6bd0e3be7e3d9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599898"
---
# <a name="intro-to-aspnet-mvc-3-vb"></a>Wprowadzenie do wzorca ASP.NET MVC 3 (VB)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępny do tego tematu. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przejdź do [ C# wersji](../cs/intro-to-aspnet-mvc-3.md) tego samouczka.

Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:

- [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)

Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Projekt programu Visual Web Developer z kodem źródłowym VB jest dostępny do tego tematu. [Pobierz wersję VB tutaj](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Jeśli wolisz CSharp, przejdź do [wersji CSharp](../cs/intro-to-aspnet-mvc-3.md) tego samouczka.

## <a name="what-youll-build"></a>Co będziesz kompilować

Zaimplementujmy prostą aplikację z listą filmów, która obsługuje tworzenie, edytowanie i wyświetlanie list filmów z bazy danych. Poniżej znajdują się dwa zrzuty ekranu aplikacji, która zostanie utworzona. Zawiera stronę wyświetlającą listę filmów z bazy danych:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów oraz wyświetlanie szczegółowych informacji o poszczególnych użytkownikach. Wszystkie scenariusze związane z wprowadzaniem danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są poprawne.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Posiadane umiejętności

Oto, co uzyskasz:

- Jak utworzyć nowy projekt ASP.NET MVC
- Jak utworzyć nową bazę danych przy użyciu kodu Entity Framework — najpierw
- Jak utworzyć kontrolery i widoki ASP.NET MVC
- Jak pobierać i wyświetlać dane
- Jak edytować dane i włączać sprawdzanie poprawności danych

## <a name="getting-started"></a>Wprowadzenie

Zacznij od uruchomienia programu Visual Web Developer 2010 Express ("pliku VWD" for Short) i wybierz pozycję **Nowy projekt** na stronie **startowej** .

Visual Web Developer to IDE lub zintegrowane środowisko programistyczne. Podobnie jak w przypadku używania programu Microsoft Word do pisania dokumentów, do tworzenia aplikacji będziesz używać środowiska IDE. W programie Visual Web Developer znajduje się pasek narzędzi wzdłuż górnej krawędzi, w którym są dostępne różne opcje. Istnieje również menu, które zapewnia inny sposób wykonywania zadań w środowisku IDE. (Na przykład zamiast wybierania **nowego projektu** ze strony **początkowej** można użyć menu i wybrać polecenie **plik** &gt; **Nowy projekt**).

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Możesz tworzyć aplikacje, korzystając z wybranych opcji Visual Basic lub wizualizacji C# jako języka programowania. Na potrzeby tego samouczka wybierz pozycję Visual Basic po lewej stronie, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC 3**. Nadaj projektowi nazwę "MvcMovie", a następnie kliknij przycisk **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

W oknie dialogowym **Nowy projekt ASP.NET MVC 3** wybierz pozycję **aplikacja internetowa**. Pozostaw **Razor** jako domyślny aparat widoku.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Kliknij przycisk **OK**. Visual Web Developer użył szablonu domyślnego dla projektu ASP.NET MVC, który właśnie został utworzony, więc masz działającą aplikację, która teraz nie wykonuje żadnych działań. To jest prosta "Hello world!" projekt i jest dobrym miejscem do uruchamiania aplikacji.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Z menu **Debuguj** wybierz polecenie **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Zwróć uwagę, że skrót klawiaturowy do rozpoczęcia debugowania jest F5.

F5 powoduje, że Visual Web Developer uruchamia programistyczny serwer sieci Web i uruchamia aplikację sieci Web. PLIKU VWD następnie uruchamia przeglądarkę i otwiera stronę główną aplikacji. Zwróć uwagę, że na pasku adresu przeglądarki widnieją `localhost` a nie `example.com`. Jest to spowodowane tym, że `localhost` zawsze wskazuje na własny komputer lokalny, w tym przypadku uruchamia właśnie utworzoną aplikację. Gdy pliku VWD uruchamia projekt sieci Web, do projektu jest używany port losowy. Na poniższym obrazie losowy numer portu to 43246. Prawdopodobnie Twój projekt będzie używał innego numeru portu.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Z pola ten szablon domyślny udostępnia dwie strony do odwiedzania oraz podstawową stronę logowania. Zmieńmy sposób działania tej aplikacji i Dowiedz się więcej na temat ASP.NET MVC w procesie. Zamknij przeglądarkę i Zmieńmy kod.

> [!div class="step-by-step"]
> [Dalej](adding-a-controller.md)

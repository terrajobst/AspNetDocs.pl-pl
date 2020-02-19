---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Wprowadzenie do ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Zaktualizowana wersja, jeśli ten samouczek jest dostępny w tym miejscu przy użyciu Visual Studio 2013. W nowym samouczku jest używany ASP.NET MVC 5, który zapewnia wiele ulepszeń w porównaniu do t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51709a9c6ddb39b8fcd1cd94cd08d530a595825a
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455545"
---
# <a name="intro-to-aspnet-mvc-4"></a>Wprowadzenie do wzorca ASP.NET MVC 4

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Zaktualizowana wersja, jeśli ten samouczek jest dostępny w [tym miejscu](../../getting-started/introduction/getting-started.md) przy użyciu [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). W nowym samouczku jest używany ASP.NET MVC 5, który zapewnia wiele ulepszeń w tym samouczku.
>
> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC 4 przy użyciu programu Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) lub Visual Web Developer 2010 Express Service Pack 1. Program Visual Studio 2012 jest zalecany, nie trzeba instalować niczego w celu ukończenia tego samouczka. W przypadku korzystania z programu Visual Studio 2010 należy zainstalować poniższe składniki. Wszystkie z nich można zainstalować, klikając następujące linki:
>
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalator WPI dla ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, zainstaluj [Instalatora WPI dla ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) i: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
>
> Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu. [Pobierz wersję C# programu](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> Samouczek dotyczący uruchamiania aplikacji w programie Visual Studio. Aplikację można również udostępnić za pośrednictwem Internetu, wdrażając ją u dostawcy hostingu. Firma Microsoft oferuje bezpłatny hosting w sieci Web dla maksymalnie 10 witryn sieci Web w ramach [bezpłatnego konta wersji próbnej platformy Microsoft Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Informacje o sposobie wdrażania projektu sieci Web programu Visual Studio w witrynie sieci Web systemu Windows Azure znajdują się w temacie [Tworzenie i wdrażanie witryny sieci web ASP.NET oraz SQL Database przy użyciu programu Visual Studio](https://docs.microsoft.com/dotnet/azure/). Ten samouczek pokazuje również, jak używać migracje Code First platformy Entity Framework do wdrażania bazy danych SQL Server w systemie Windows Azure SQL Database (dawniej SQL Azure).
>
> Ten samouczek został zapisany przez Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).

## <a name="what-youll-build"></a>Co będziesz kompilować

> [!NOTE]
> Zaktualizowana wersja, jeśli ten samouczek jest dostępny w [tym miejscu](../../getting-started/introduction/getting-started.md) przy użyciu [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). W nowym samouczku jest używany ASP.NET MVC 5, który zapewnia wiele ulepszeń w tym samouczku.

Zaimplementujmy prostą aplikację z listą filmów, która obsługuje tworzenie, edytowanie, wyszukiwanie i wyświetlanie list filmów z bazy danych. Poniżej znajdują się dwa zrzuty ekranu aplikacji, która zostanie utworzona. Zawiera stronę wyświetlającą listę filmów z bazy danych:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów oraz wyświetlanie szczegółowych informacji o poszczególnych użytkownikach. Wszystkie scenariusze związane z wprowadzaniem danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są poprawne.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Wprowadzenie

Zacznij od uruchomienia Visual Studio Express 2012 lub Visual Web Developer 2010 Express. Większość zrzutów ekranu w tej serii korzysta z Visual Studio Express 2012, ale można wykonać ten samouczek z programem Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 lub Visual Web Developer 2010 Express. Na stronie **startowej** wybierz pozycję **Nowy projekt** .

Program Visual Studio to IDE lub zintegrowane środowisko programistyczne. Podobnie jak w przypadku używania programu Microsoft Word do pisania dokumentów, do tworzenia aplikacji będziesz używać środowiska IDE. W programie Visual Studio znajduje się pasek narzędzi wzdłuż górnej krawędzi, w którym są dostępne różne opcje. Istnieje również menu, które zapewnia inny sposób wykonywania zadań w środowisku IDE. (Na przykład zamiast wybierania **nowego projektu** ze strony **początkowej** można użyć menu i wybrać polecenie **plik** &gt; **Nowy projekt**).

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Aplikacje można tworzyć przy użyciu Visual Basic lub wizualizacji C# jako języka programowania. Wybierz pozycję C# Wizualizacja po lewej stronie, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC 4**. Nazwij projekt &quot;MvcMovie&quot; a następnie kliknij przycisk **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **aplikacja internetowa**. Pozostaw **Razor** jako domyślny aparat widoku.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Kliknij przycisk **OK**. Program Visual Studio użył szablonu domyślnego dla projektu ASP.NET MVC, który właśnie został utworzony, więc masz działającą aplikację, która teraz nie wykonuje żadnych działań. To jest prosta &quot;Hello world!&quot; projekt i jest dobrym miejscem do uruchamiania aplikacji.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Z menu **Debuguj** wybierz polecenie **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Zwróć uwagę, że skrót klawiaturowy do rozpoczęcia debugowania jest F5.

F5 powoduje uruchomienie programu Visual Studio IIS Express i uruchomienie aplikacji sieci Web. Program Visual Studio uruchomi przeglądarkę i otworzy stronę główną aplikacji. Zwróć uwagę, że na pasku adresu przeglądarki widnieją `localhost` a nie `example.com`. Jest to spowodowane tym, że `localhost` zawsze wskazuje na własny komputer lokalny, w tym przypadku uruchamia właśnie utworzoną aplikację. Gdy program Visual Studio uruchamia projekt sieci Web, dla serwera sieci Web jest używany port losowy. Na poniższej ilustracji numer portu to 41788. Po uruchomieniu aplikacji prawdopodobnie zobaczysz inny numer portu.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Po prawej stronie pola ten szablon domyślny udostępnia Strona główna, kontakt i informacje o stronach. Zapewnia również obsługę rejestrowania i logowania oraz linki do serwisów Facebook i Twitter. Następnym krokiem jest zmiana sposobu działania tej aplikacji i uzyskanie nieco informacji o ASP.NET MVC. Zamknij przeglądarkę i Zmieńmy kod.

> [!div class="step-by-step"]
> [Dalej](adding-a-controller.md)

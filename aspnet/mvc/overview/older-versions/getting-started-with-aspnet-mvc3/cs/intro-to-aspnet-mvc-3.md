---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Wprowadzenie do ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: e71275c93558c0b6ca087a145786e8c846b69721
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457541"
---
# <a name="intro-to-aspnet-mvc-3-c"></a>Wprowadzenie do wzorca ASP.NET MVC 3 (C#)

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Zaktualizowana wersja tego samouczka jest dostępna w [tym miejscu](../../../getting-started/introduction/getting-started.md) , w którym są używane ASP.NET MVC 5 i Visual Studio 2013. Jest to bezpieczniejsze i łatwiejsze w obserwowanie i zademonstrowanie większej liczby funkcji.
> 
> 
> Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1, który jest bezpłatną wersją Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wstępnie wymagane składniki, korzystając z następujących linków:
> 
> - [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast programu Visual Web Developer 2010, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt programu Visual Web Developer z C# kodem źródłowym jest dostępny do załączenia do tego tematu. [Pobierz wersję C# programu](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przejdź do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.

## <a name="what-youll-build"></a>Co będziesz kompilować

Zaimplementujmy prostą aplikację z listą filmów, która obsługuje tworzenie, edytowanie i wyświetlanie list filmów z bazy danych. Poniżej znajdują się dwa zrzuty ekranu aplikacji, która zostanie utworzona. Zawiera stronę wyświetlającą listę filmów z bazy danych:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów oraz wyświetlanie szczegółowych informacji o poszczególnych użytkownikach. Wszystkie scenariusze związane z wprowadzaniem danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są poprawne.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Posiadane umiejętności

Oto, co uzyskasz:

- Jak utworzyć nowy projekt ASP.NET MVC.
- Jak utworzyć kontrolery i widoki ASP.NET MVC.
- Jak utworzyć nową bazę danych przy użyciu Entity Framework Code First model.
- Jak pobierać i wyświetlać dane.
- Jak edytować dane i włączać sprawdzanie poprawności danych.

## <a name="getting-started"></a>Wprowadzenie

Zacznij od uruchomienia programu Visual Web Developer 2010 Express ("Visual Web Developer" for Short) i wybierz pozycję **Nowy projekt** na stronie **początkowej** .

Visual Web Developer to IDE lub zintegrowane środowisko programistyczne. Podobnie jak w przypadku używania programu Microsoft Word do pisania dokumentów, do tworzenia aplikacji będziesz używać środowiska IDE. W programie Visual Web Developer znajduje się pasek narzędzi wzdłuż górnej krawędzi, w którym są dostępne różne opcje. Istnieje również menu, które zapewnia inny sposób wykonywania zadań w środowisku IDE. (Na przykład zamiast wybierania **nowego projektu** ze strony **początkowej** można użyć menu i wybrać polecenie **plik** &gt; **Nowy projekt**).

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Aplikacje można tworzyć przy użyciu Visual Basic lub wizualizacji C# jako języka programowania. Wybierz pozycję C# Wizualizacja po lewej stronie, a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC 3**. Nadaj projektowi nazwę "MvcMovie", a następnie kliknij przycisk **OK**. (Jeśli wolisz Visual Basic, przejdź do [Visual Basic wersji](../vb/intro-to-aspnet-mvc-3.md) tego samouczka).

![](intro-to-aspnet-mvc-3/_static/image5.png)

W oknie dialogowym **Nowy projekt ASP.NET MVC 3** wybierz pozycję **aplikacja internetowa**. Zaznacz opcję **Użyj znacznika HTML5** i pozostaw **Razor** jako domyślny aparat widoku.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Kliknij przycisk **OK**. Visual Web Developer użył szablonu domyślnego dla projektu ASP.NET MVC, który właśnie został utworzony, więc masz działającą aplikację, która teraz nie wykonuje żadnych działań. To jest prosta "Hello world!" projekt i jest dobrym miejscem do uruchamiania aplikacji.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Z menu **Debuguj** wybierz polecenie **Rozpocznij debugowanie**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Zwróć uwagę, że skrót klawiaturowy do rozpoczęcia debugowania jest F5.

F5 powoduje, że Visual Web Developer uruchamia programistyczny serwer sieci Web i uruchamia aplikację sieci Web. Visual Web Developer następnie uruchamia przeglądarkę i otwiera stronę główną aplikacji. Zwróć uwagę, że na pasku adresu przeglądarki widnieją `localhost` a nie `example.com`. Jest to spowodowane tym, że `localhost` zawsze wskazuje na własny komputer lokalny, w tym przypadku uruchamia właśnie utworzoną aplikację. Gdy Visual Web Developer uruchamia projekt sieci Web, dla serwera sieci Web jest używany port losowy. Na poniższym obrazie losowy numer portu to 43246. Po uruchomieniu aplikacji prawdopodobnie zobaczysz inny numer portu.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Z prawej strony pola ten szablon domyślny udostępnia dwie strony do odwiedzania oraz podstawową stronę logowania. Następnym krokiem jest zmiana sposobu działania tej aplikacji i uzyskanie nieco informacji o ASP.NET MVC w procesie. Zamknij przeglądarkę i Zmieńmy kod.

> [!div class="step-by-step"]
> [Dalej](adding-a-controller.md)

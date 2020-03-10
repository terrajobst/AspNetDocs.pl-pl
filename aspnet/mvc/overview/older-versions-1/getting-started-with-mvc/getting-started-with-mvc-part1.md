---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Wprowadzenie do ASP.NET MVC | Microsoft Docs
author: shanselman
description: Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utwórz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581586"
---
# <a name="intro-to-aspnet-mvc"></a>Wprowadzenie do wzorca ASP.NET MVC

przez [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Zaktualizowana wersja, jeśli ten samouczek jest dostępny w [tym miejscu](../../getting-started/introduction/getting-started.md) przy użyciu [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). W nowym samouczku jest używany ASP.NET MVC 5, który zapewnia wiele ulepszeń w tym samouczku.
>
>
> Jest to samouczek początkującego, który wprowadza podstawy ASP.NET MVC. Utworzysz prostą aplikację sieci Web, która odczytuje i zapisuje dane w bazie danych. Odwiedź [centrum learning ASP.NET MVC](../../../index.md) , aby znaleźć inne samouczki i przykłady MVC ASP.NET.

Utwórzmy pierwszą aplikację sieci Web ASP.NET MVC przy użyciu [programu Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Udostępnimy małą listę filmów, która pozwoli nam tworzyć i wyświetlać filmy.

## <a name="what-youll-build"></a>Co będziesz kompilować

Poniżej przedstawiono dwa zrzuty ekranu aplikacji, która zostanie utworzona. Będziesz mieć prostą tabelę filmów z różnymi kolumnami.

[Lista filmów ![— Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

A będziesz mieć formularz tworzenia, aby umożliwić Dodawanie filmów do listy.

[![utworzyć filmu — Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Posiadane umiejętności

Ten samouczek zawiera informacje na temat tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Visual Studio. W dokumencie omówiono następujące zagadnienia :

- Jak utworzyć nowy projekt ASP.NET MVC
- Jak utworzyć nową bazę danych za pomocą SQL Server
- Jak utworzyć kontrolery i widoki ASP.NET MVC
- Jak pobierać i wyświetlać dane
- Jak edytować dane i włączać sprawdzanie poprawności danych
- Jak zaktualizować schemat bazy danych

## <a name="get-started"></a>Rozpoczęcie pracy

Zacznij od uruchomienia programu Visual Web Developer 2010 Express (wywołam teraz "pliku VWD") i wybierz pozycję Nowy projekt na ekranie startowym.

Visual Web Developer to IDE lub zintegrowane środowisko deweloperskie. Podobnie jak w przypadku używania programu Microsoft Word do pisania dokumentów, do tworzenia aplikacji będziesz używać środowiska IDE. Na początku znajduje się pasek narzędzi pokazujący różne dostępne opcje, a także menu, które można także użyć do wybrania pliku | Nowy projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Aplikacje można tworzyć przy użyciu Visual Basic lub wizualizacji C#. Na razie zaznacz opcję Visual C# po lewej stronie, a następnie wybierz pozycję "ASP.NET MVC 2 Web Application". Nadaj projektowi nazwę "filmy" i kliknij przycisk OK.

[![nowy projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Po prawej stronie jest Eksplorator rozwiązań pokazujący wszystkie pliki i foldery w aplikacji. Duże okno w środku polega na tym, że edytujesz swój kod i spędzasz najwięcej czasu. Program Visual Studio użył szablonu domyślnego dla projektu ASP.NET MVC, który właśnie został utworzony, więc masz działającą aplikację, która teraz nie wykonuje żadnych działań. To jest prosta "Hello world! projekt i jest dobrym miejscem do rozpoczęcia pracy z naszą aplikacją.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Wybierz przycisk "Odtwórz" na pasku narzędzi.

![Rozpocznij debugowanie](getting-started-with-mvc-part1/_static/image11.png)

Jest to zielona strzałka wskazująca prawo do skompilowania programu i uruchomienia aplikacji w przeglądarce sieci Web.

*Uwaga: możesz zamiast tego nacisnąć klawisz F5 na klawiaturze lub wybrać polecenie Debuguj-&gt;Rozpocznij debugowanie z menu "Debugowanie".*

Spowoduje to, że Visual Web Developer rozpocznie Programowanie na serwerze sieci Web i uruchamianie naszej aplikacji sieci Web (nie ma konieczności konfigurowania ani ręcznego wykonywania czynności). Następnie zostanie uruchomiona przeglądarka i zostanie skonfigurowana w celu przeglądania strony głównej aplikacji. Zwróć uwagę, że na pasku adresu przeglądarki jest wyświetlany napis "localhost", a nie coś, takiego jak example.com. Jest to spowodowane tym, że hosty localhost zawsze wskazuje na własny komputer lokalny — w tym przypadku jest uruchomiona utworzona przez nas aplikacja.

[Strona główna ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Z pola ten szablon domyślny udostępnia dwie strony do odwiedzania oraz podstawową stronę logowania. Zmieńmy sposób działania tej aplikacji i Dowiedz się więcej na temat ASP.NET MVC w procesie. Zamknij przeglądarkę i umożliwia zmianę kodu.

> [!div class="step-by-step"]
> [Dalej](getting-started-with-mvc-part2.md)

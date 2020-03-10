---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Część 1: plik-> Nowy projekt | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 1 obejmuje przegląd i plik/nowy projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636130"
---
# <a name="part-1-file--new-project"></a>Część 1. Plik -> Nowy projekt

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 1 obejmuje przegląd i plik/nowy projekt.

## <a id="_Toc260221666"></a>Podsumowanie

Ten samouczek stanowi wprowadzenie do ASP.NET formularzy sieci Web. Zaczniemy powoli, więc środowisko programistyczne dla początkujących użytkowników jest w stanie dobra.

Aplikacja, którą będziemy kompilować, to prosty magazyn online.

![](tailspin-spyworks-part-1/_static/image1.jpg)

Goście mogą przeglądać produkty według kategorii:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Mogą oni wyświetlać jeden produkt i dodawać go do swojego koszyka:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Mogą przeglądać ich koszyk, usuwając wszystkie elementy, które nie są już potrzebne:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Przeprowadzenie wyewidencjonowania spowoduje wyświetlenie monitu o

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Po określeniu kolejności zobaczysz prosty ekran potwierdzenia:

![](tailspin-spyworks-part-1/_static/image7.jpg)

Zaczniemy od utworzenia nowego projektu ASP.NET WebForms w programie Visual Studio 2010 i nastąpi przyrostowe dodanie funkcji w celu utworzenia kompletnej działającej aplikacji. W ten sposób będziemy obejmować dostęp do bazy danych, widoki list i siatki, strony aktualizacji danych, sprawdzanie poprawności danych przy użyciu stron wzorcowych na potrzeby spójnego układu strony, technologii AJAX, sprawdzania poprawności, członkostwa użytkowników i innych.

Możesz wykonać instrukcje krok po kroku lub pobrać ukończoną aplikację z [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Możesz użyć programu Visual Studio 2010 lub bezpłatnego programu Visual Web Developer 2010 z [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Aby skompilować aplikację, można użyć dowolnej SQL Server lub bezpłatnego SQL Server Express do hostowania bazy danych programu.

## <a id="_Toc260221667"></a>Plik/nowy projekt

Zaczniemy od wybrania nowego projektu z menu plik w programie Visual Studio. Spowoduje to wyświetlenie okna dialogowego Nowy projekt.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Wybierzemy grupę szablonów wizualizacji C# /sieci Web po lewej stronie, a następnie wybierz szablon "aplikacja sieci Web ASP.NET" w środkowej kolumnie. Nazwij projekt TailspinSpyworks i naciśnij przycisk OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Spowoduje to utworzenie naszego projektu. Spójrzmy na foldery dołączone do naszej aplikacji w Eksplorator rozwiązań po prawej stronie.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Puste rozwiązanie nie jest całkowicie puste — dodaje podstawową strukturę folderów:

![](tailspin-spyworks-part-1/_static/image1.png)

Należy zwrócić uwagę na konwencje zaimplementowane przez domyślny szablon projektu ASP.NET 4.

- Folder "Account" implementuje podstawowy interfejs użytkownika dla środowiska ASP. Podsystem członkostwa w sieci.
- Folder "Scripts" służy jako repozytorium dla plików JavaScript po stronie klienta, a podstawowe pliki jQuery. js są udostępniane domyślnie.
- Folder "Style" służy do organizowania wizualizacji witryn sieci Web (arkuszy stylów CSS)

Po naciśnięciu klawisza F5 w celu uruchomienia aplikacji i renderowania domyślnej strony. aspx zostanie wyświetlona następująca strona.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Nasze pierwsze ulepszenie aplikacji będzie zastępować plik style. CSS z domyślnego szablonu WebForms z klasami CSS i skojarzonymi z nimi plikami obrazu, które będą asthetics wizualizację dla naszej aplikacji Tailspin Spyworks.

Po wykonaniu tej operacji domyślna strona. aspx jest renderowana w następujący sposób.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Zwróć uwagę na linki do obrazów w prawym górnym rogu strony i elementy menu, które zostały dodane do strony wzorcowej. Tylko łącza "Zaloguj się" i "konto" wskazują strony istniejące (generowane przez szablon domyślny) oraz pozostałe strony, które zostaną wdrożone podczas kompilowania aplikacji.

Zamierzamy również przenieść stronę wzorcową do katalogu stylów. Mimo że jest to tylko preferencja, może to trochę potrwać, jeśli chcemy, aby nasza aplikacja była "w przyszłości".

Po wykonaniu tej czynności będziemy musieli zmienić odwołania do stron wzorcowych we wszystkich plikach aspx wygenerowanych przez domyślne strony formularzy sieci Web ASP.NET.

> [!div class="step-by-step"]
> [Dalej](tailspin-spyworks-part-2.md)

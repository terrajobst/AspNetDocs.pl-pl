---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Wprowadzenie do samouczka NerdDinner | Microsoft Docs
author: shanselman
description: Najlepszym sposobem uczenia się nowego środowiska jest skompilowanie czegoś. Ten samouczek przeprowadzi Cię przez proces tworzenia małego, ale kompletnej aplikacji przy użyciu usługi ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580578"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Wprowadzenie do samouczka NerdDinner

przez [Scott Hanselman](https://github.com/shanselman)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Najlepszym sposobem uczenia się nowego środowiska jest skompilowanie czegoś. W tym samouczku przedstawiono sposób tworzenia małego, ale kompletnej aplikacji używającej ASP.NET MVC 1 i wprowadzono niektóre podstawowe pojęcia.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-tutorial"></a>Samouczek NerdDinner

Najlepszym sposobem uczenia się nowego środowiska jest skompilowanie czegoś. W tym samouczku przedstawiono sposób tworzenia małego, ale kompletnej aplikacji używającej ASP.NET MVC i wprowadzono niektóre podstawowe pojęcia.

Aplikacja, którą zamierzamy skompilować, nosi nazwę "NerdDinner". NerdDinner zapewnia łatwy sposób, aby użytkownicy mogli znajdować i organizować obiady w trybie online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner umożliwia zarejestrowanym użytkownikom tworzenie, edytowanie i usuwanie obiadów. Wymusza spójny zestaw weryfikacji i reguł firmy w aplikacji:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Osoby odwiedzające mogą używać mapy opartej na technologii AJAX, aby wyszukiwać nadchodzące uroczyste obiady w ich sąsiedztwie:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Kliknięcie obiadu spowoduje przejście do strony szczegółów, na której można dowiedzieć się więcej na jej temat:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Jeśli interesują Cię obiad, mogą oni zalogować się lub zarejestrować w witrynie:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Następnie mogą kliknąć link RSVP oparty na technologii AJAX, aby wziąć udział w zdarzeniu:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementowanie NerdDinner

Zamierzamy zacząć korzystać z naszej aplikacji NerdDinner przy użyciu polecenia plik&gt;nowy projekt w programie Visual Studio, aby utworzyć nowy projekt ASP.NET MVC. Następnie będziemy stopniowo dodawać funkcje i funkcje. W ten sposób będziemy:

1. [Jak utworzyć nowy projekt ASP.NET MVC](create-a-new-aspnet-mvc-project.md)
2. [Jak utworzyć bazę danych](create-a-database.md)
3. [Jak skompilować model przy użyciu walidacji reguł firmy](build-a-model-with-business-rule-validations.md)
4. [Jak używać kontrolerów i widoków do implementowania interfejsu użytkownika listy/szczegółów](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Jak zapewnić obsługę wpisów w formularzu danych CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie)](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Jak używać ViewData i implementować klasy ViewModel](use-viewdata-and-implement-viewmodel-classes.md)
7. [Jak ponowne używać interfejsu użytkownika za pomocą stron wzorcowych i częściowych](re-use-ui-using-master-pages-and-partials.md)
8. [Jak zaimplementować wydajne stronicowanie danych](implement-efficient-data-paging.md)
9. [Jak zabezpieczyć aplikacje przy użyciu uwierzytelniania i autoryzacji](secure-applications-using-authentication-and-authorization.md)
10. [Jak używać technologii AJAX do dostarczania aktualizacji dynamicznych](use-ajax-to-deliver-dynamic-updates.md)
11. [Jak zaimplementować scenariusze mapowania przy użyciu technologii AJAX](use-ajax-to-implement-mapping-scenarios.md)
12. [Jak włączyć automatyczne testowanie jednostek](enable-automated-unit-testing.md)

Możesz utworzyć własną kopię NerdDinner od podstaw, wypełniając każdy krok w tym rozdziale. Możesz też pobrać kompletną wersję kodu źródłowego tutaj: [NerdDinner w witrynie GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Opcjonalnie możesz również [pobrać bezpłatną wersję pliku PDF z tego samouczka](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) , jeśli chcesz przeczytać samouczek w trybie offline.

Do skompilowania aplikacji możesz użyć programu Visual Studio 2008 lub bezpłatnej wersji Visual Web Developer 2008 Express. Możesz użyć dowolnej SQL Server lub bezpłatnej SQL Server Express bazy danych.

Możesz zainstalować ASP.NET MVC, Visual Web Developer 2008 Express i SQL Server Express (wszystko bezpłatne) przy użyciu wersji 2 [Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Zacznijmy teraz...

Teraz, w jaki sposób NerdDinner się, przyjrzyjmy nasze rękawy i napisać kod.

Rozpocznie się tworzenie aplikacji NerdDinner przy użyciu pliku&gt;nowego projektu w programie Visual Studio.

> [!div class="step-by-step"]
> [Dalej](create-a-new-aspnet-mvc-project.md)

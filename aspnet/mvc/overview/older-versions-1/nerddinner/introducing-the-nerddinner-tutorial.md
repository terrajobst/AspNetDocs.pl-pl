---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Wprowadzenie do samouczka NerdDinner | Dokumentacja firmy Microsoft
author: shanselman
description: Najlepszym sposobem, aby dowiedzieć się nowej struktury jest Zbuduj coś z nim. Ten samouczek zawiera szczegółowe instrukcje dotyczące tworzenia aplikacji mały, ale pełny, za pomocą ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: ebd49295ea165ba4ef1a25398cff7dddcfa54f11
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59392199"
---
# <a name="introducing-the-nerddinner-tutorial"></a>Wprowadzenie do samouczka NerdDinner

przez [Scotta Hanselmana](https://github.com/shanselman)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Najlepszym sposobem, aby dowiedzieć się nowej struktury jest Zbuduj coś z nim. W tym samouczku przedstawiono sposób tworzenia małych, ale wykonania aplikacji za pomocą platformy ASP.NET MVC 1 oraz przedstawiono niektóre z podstawowych pojęciach związanych z nim.
> 
> Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.


## <a name="nerddinner-tutorial"></a>Samouczka NerdDinner

Najlepszym sposobem, aby dowiedzieć się nowej struktury jest Zbuduj coś z nim. W tym samouczku przedstawiono sposób tworzenia małych, ale wykonania aplikacji za pomocą platformy ASP.NET MVC i wprowadza niektóre z podstawowych pojęciach związanych z nim.

Aplikację, którą zamierzamy kompilacji jest nazywany "NerdDinner". NerdDinner zapewnia łatwy sposób dla osób, Znajdź i organizowania kolacji w trybie online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner umożliwia tworzenie, edytowanie i usuwanie kolacji dla zarejestrowanych użytkowników. Wymusza on spójny zestaw reguł biznesowych i weryfikacji w aplikacji:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Osoby odwiedzające opartych na technologii AJAX tablica służy do wyszukiwania nadchodzących kolacji odbywają się w pobliżu je:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Klikając obiad powoduje wyświetlenie strony szczegółów gdzie oni dowiedzieć się więcej na ten temat:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Jeśli są zainteresowani udziałowi obiad mogą zalogować się lub zarejestrować się w lokacji:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Są następnie kliknąć link RSVP opartych na technologii AJAX w taki sposób, aby weź udział w wydarzeniu:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>Implementowanie NerdDinner

Zamierzamy zacząć naszej aplikacji NerdDinner przy użyciu pliku -&gt;polecenie Nowy projekt w programie Visual Studio do tworzenia nowego projektu ASP.NET MVC. Następnie przyrostowe dodamy funkcje i. Po drodze omówimy:

1. [Tworzenie nowego projektu MVC ASP.NET](create-a-new-aspnet-mvc-project.md)
2. [Jak utworzyć bazę danych](create-a-database.md)
3. [Sposób tworzenia modelu z weryfikacją reguł biznesowych](build-a-model-with-business-rule-validations.md)
4. [Jak zaimplementować lista/szczegóły interfejsu użytkownika za pomocą widoków i kontrolerów](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [Jak zapewnić CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) obsługi formularzach danych](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [Jak używać ViewData i Implementowanie klas ViewModel](use-viewdata-and-implement-viewmodel-classes.md)
7. [Jak ponownie używać interfejsu użytkownika za pomocą stron wzorcowych i częściowych](re-use-ui-using-master-pages-and-partials.md)
8. [Jak Implementowanie wydajnego stronicowania danych](implement-efficient-data-paging.md)
9. [Jak zabezpieczyć aplikacje przy użyciu uwierzytelniania i autoryzacji](secure-applications-using-authentication-and-authorization.md)
10. [Jak korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych](use-ajax-to-deliver-dynamic-updates.md)
11. [Sposób użycia interfejsu AJAX w celu implementacji scenariuszy mapowania](use-ajax-to-implement-mapping-scenarios.md)
12. [Jak włączyć automatyczne testy jednostkowe](enable-automated-unit-testing.md)

Możesz tworzyć własną kopię NerdDinner od podstaw, wykonując każdego kroku będziemy instrukcje przedstawione w tym rozdziale. Alternatywnie możesz pobrać pełną wersję kodu źródłowego w tym miejscu: [NerdDinner w serwisie GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Możesz również opcjonalnie również [Pobierz bezpłatną wersję PDF po ukończeniu tego samouczka](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) aby przeczytaj samouczek w trybie offline.

Aby skompilować aplikację, można użyć programu Visual Studio 2008 lub bezpłatnego programu Visual Web Developer 2008 Express. Dla bazy danych, można użyć programu SQL Server lub bezpłatnego programu SQL Server Express.

ASP.NET MVC, Visual Web Developer 2008 Express i programu SQL Server Express (wszystkie wersja bezpłatna) przy użyciu programu w wersji 2 można zainstalować [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Teraz Rozpocznijmy od...

Teraz, gdy Omówiliśmy już jest NerdDinner, umożliwia rzutowanie naszych programistyczne firmy Microsoft i pisanie kodu.

Firma Microsoft rozpocznie się za pomocą pliku -&gt;nowy projekt w programie Visual Studio do tworzenia aplikacji NerdDinner.

> [!div class="step-by-step"]
> [Next](create-a-new-aspnet-mvc-project.md)

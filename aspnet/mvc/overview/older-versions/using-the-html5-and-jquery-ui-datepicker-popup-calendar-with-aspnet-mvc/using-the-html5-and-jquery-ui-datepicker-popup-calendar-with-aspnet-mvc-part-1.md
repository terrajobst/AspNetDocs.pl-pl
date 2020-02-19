---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 1 | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i menu podręcznego interfejsu użytkownika jQuery DatePicker w ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c1c2380f24c72f6aabaaacaf975e95288a384ff1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457625"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 1

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i okienka podręcznego interfejsu użytkownika jQuery DatePicker w aplikacji sieci Web ASP.NET MVC.

W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i [okienka podręcznego interfejsu użytkownika jQuery DatePicker](http://plugins.jquery.com/project/datepicker) w aplikacji sieci Web ASP.NET MVC. W tym samouczku można użyć programu Microsoft Visual Web Developer 2010 Express z dodatkiem Service Pack 1 (&quot;Visual Web Developer&quot;), który jest bezpłatną wersją Microsoft Visual Studio lub jeśli masz już tę usługę, możesz użyć programu Visual Studio 2010 z dodatkiem SP1.

Przed rozpoczęciem upewnij się, że zainstalowano wymagania wstępne wymienione poniżej. Wszystkie z nich można zainstalować, klikając następujące łącze: [Instalator platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie możesz zainstalować wymagane oprogramowanie, korzystając z następujących linków:

- [Wymagania wstępne programu Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Aktualizacja narzędzi ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(obsługa środowiska uruchomieniowego + narzędzia)

Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer, Zainstaluj wymagania wstępne, klikając następujące łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

W tym samouczku przyjęto założenie, że zakończono [wprowadzenie za pomocą samouczka MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub że znasz już programowanie ASP.NET MVC. Ten samouczek rozpoczyna się od ukończonego projektu z [wprowadzenie za pomocą samouczka MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) .

W tym samouczku przedstawiono C#kod w. Jednak [projekt startowy](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) i ukończony projekt są również dostępne w Visual Basic.

Projekt programu Visual Studio z C# kodem źródłowym i Visual Basic jest dostępny do załączenia do tego tematu: [pobieranie](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Co będziesz kompilować

Będziesz dodawać szablony (w specjalny sposób, edytować i wyświetlać szablony) do prostej aplikacji do wyświetlania filmów, która została utworzona w [wprowadzenie za pomocą samouczka MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) . Zostanie również dodany kalendarz podręczny [DatePicker interfejsu użytkownika platformy jQuery](http://jqueryui.com/demos/datepicker/) , aby uprościć proces wprowadzania dat. Poniższy zrzut ekranu przedstawia zmodyfikowaną aplikację z wyświetlonym kalendarzem podręczny DatePicker interfejsu użytkownika jQuery.

![Zakończono jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Posiadane umiejętności

Oto, co uzyskasz:

- Jak używać atrybutów z przestrzeni nazw [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) do sterowania formatem danych wyświetlanych i w trybie edycji.
- Tworzenie szablonów (edytowanie i wyświetlanie szablonów) w celu kontrolowania formatowania danych.
- Jak dodać [interfejs użytkownika jQuery DatePicker](http://jqueryui.com/demos/datepicker/) jako sposób wprowadzania pól daty.

### <a name="getting-started"></a>Wprowadzenie

Jeśli nie masz jeszcze aplikacji z listą filmów z początkowego projektu, pobierz ją: 

* [Pobierz](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).
* W Eksploratorze Windows kliknij prawym przyciskiem myszy plik *MvcMovie. zip* i wybierz polecenie **Właściwości**. 
* W oknie dialogowym **Właściwości MvcMovie. zip** wybierz opcję **Odblokuj**. (Odblokowywanie zapobiega ostrzeżeniu zabezpieczeń, które występuje podczas próby użycia pliku *zip* pobranego z sieci Web).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Kliknij prawym przyciskiem myszy plik *MvcMovie. zip* i wybierz polecenie **Wyodrębnij wszystko** , aby rozpakować plik. W programie Visual Web Developer lub Visual Studio 2010 Otwórz plik *MvcMovieCS\_jednostek przepływności. sln* .

W **Eksplorator rozwiązań**kliknij dwukrotnie plik *Views\Shared\\_Layout. cshtml* , aby go otworzyć. Zmień nagłówek `H1` z **aplikacji filmowej MVC** na **film jQuery**. Naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację, a następnie kliknij kartę **Narzędzia główne** , która spowoduje przejście do `Index` metody kontrolera filmu. Aby wypróbować aplikację, wybierz link **Edytuj** i link **szczegóły** dla jednego z filmów. Zwróć uwagę, że w widokach indeks, Edytuj i szczegóły Data wydania i cena są dobrze sformatowane:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

Formatowanie daty i ceny jest wynikiem użycia atrybutu [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) we właściwościach klasy `Movie`.

Otwórz plik *Movie.cs* i Dodaj komentarz do `DisplayFormat` atrybutu we właściwościach `ReleaseDate` i `Price`. Klasa wynikowa `Movie` wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Ponownie naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie wybierz kartę **Narzędzia główne** , aby wyświetlić listę filmów. Tym razem Data wydania pokazuje datę i godzinę, a w polu cena nie jest już wyświetlany symbol waluty. Zmiana w klasie `Movie` spowodowała cofnięcie wcześniej sformatowanego formatowania, ale w tej chwili nastąpi rozwiązanie tego problemu.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Użycie atrybutu DataType DataAnnotations w celu określenia typu danych

Zastąp atrybut `DisplayFormat` z komentarzem dla właściwości `ReleaseDate` z atrybutem [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , używając wyliczenia `Date`. Zastąp atrybut `DisplayFormat` właściwości `Price` atrybutem [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) ponownie, tym razem używając wyliczenia `Currency`. Oto, jak wygląda ukończony kod:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Uruchom aplikację. Teraz Data wydania i właściwości ceny są poprawnie sformatowane (to jest przy użyciu odpowiednich formatów daty i waluty). Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera metadane typu dla wbudowanych szablonów ASP.NET MVC, tak aby pola były renderowane w poprawnym formacie. Korzystanie z atrybutu `DataType` jest preferowane przy użyciu atrybutu `DisplayFormat`, który pierwotnie był w kodzie, ponieważ atrybut `DataType` sprawia, że model i bardziej elastyczny jest przejrzysty dla celów, takich jak w przypadku funkcji wielojęzycznych.

W następnej sekcji zobaczysz, jak utworzyć niestandardowe szablony do wyświetlania pól daty.

> [!div class="step-by-step"]
> [Dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)

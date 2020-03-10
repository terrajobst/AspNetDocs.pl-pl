---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Część 3: menu Układ i Kategoria | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 3 dotyczy dodawania układu i menu kategorii.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639119"
---
# <a name="part-3-layout-and-category-menu"></a>Część 3. Układ i menu kategorii

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 3 dotyczy dodawania układu i menu kategorii.

## <a id="_Toc260221669"></a>Dodawanie niektórych układów i menu kategorii

Na naszej stronie wzorcowej witryny dodamy blok DIV dla kolumny po lewej stronie, która będzie zawierać menu kategorii produktów.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Należy zauważyć, że odpowiednie wyrównanie i inne formatowanie będzie zapewnione przez klasę CSS dodaną do naszego pliku style. css.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Menu Kategoria produktu zostanie dynamicznie utworzone w środowisku uruchomieniowym przez wykonanie zapytania dotyczącego bazy danych commerce dla istniejących kategorii produktów i utworzenie elementów menu i odpowiednich linków.

Aby to osiągnąć, użyjemy dwóch z ASP. Zaawansowane kontrolki danych sieci. Kontrolka "Źródło danych jednostki" i kontrolka "ListView".

![](tailspin-spyworks-part-3/_static/image1.jpg)

Przełączmy na "Widok projektu" i użyj pomocników, aby skonfigurować nasze kontrolki.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Skonfigurujmy Właściwość Identyfikator obiektu EntityDataSource do menu EDS\_kategorii\_i kliknij pozycję "Konfiguruj źródło danych".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Wybierz połączenie CommerceEntities, które zostało utworzone dla nas po utworzeniu modelu źródła danych jednostki dla bazy danych firmy Commerce i kliknij przycisk "dalej".

![](tailspin-spyworks-part-3/_static/image4.jpg)

Wybierz nazwę zestawu jednostek "Kategorie" i pozostaw pozostałe opcje jako domyślne. Kliknij przycisk "Zakończ".

Teraz Skonfigurujmy Właściwość ID instancji formantu ListView, która została umieszczona na naszej stronie w celu udostępnienia elementu ListView\_ProductsMenu i aktywowania pomocnika.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Mimo że możemy użyć opcji kontroli do formatowania wyświetlania i formatowania elementów danych, nasze polecenie tworzenia menu będzie wymagało tylko prostego znacznika, więc w widoku źródła wprowadzimy kod.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Zanotuj instrukcję "eval": &lt;% # eval ("CategoryName")%&gt;

Składnia ASP.NET &lt;% #%&gt; jest konwencją skróconą, która instruuje środowisko uruchomieniowe, aby wykonać dowolne, które jest zawarte w i wyprowadza wyniki "w wierszu".

Instrukcja eval ("CategoryName") instruuje, że dla bieżącego wpisu w powiązanej kolekcji elementów danych Pobierz wartość elementów modelu jednostki "CategoryName". Jest to zwięzła składnia dla bardzo zaawansowanej funkcji.

Umożliwia uruchomienie aplikacji teraz.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Należy zauważyć, że nasze menu Kategoria produktu jest teraz wyświetlane, a po umieszczeniu wskaźnika myszy nad jednym z elementów menu Kategoria zobaczysz, że na stronie nie ma jeszcze zaimplementowanej wartości Nazwa ProductsList. aspx i że został skompilowany argument ciągu zapytania dynamicznego zawierający  Identyfikator kategorii.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-2.md)
> [dalej](tailspin-spyworks-part-4.md)

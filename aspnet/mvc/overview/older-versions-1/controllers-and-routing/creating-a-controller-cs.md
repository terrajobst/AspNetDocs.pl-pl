---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Tworzenie kontrolera (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak dodać kontrolera do aplikacji ASP.NET MVC.'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e3d0bae7f07410637c2b06c500d94a02c821f5c
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65123617"
---
# <a name="creating-a-controller-c"></a>Tworzenie kontrolera (C#)

przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> W tym samouczku Walther Autor: Stephen pokazuje, jak dodać kontrolera do aplikacji ASP.NET MVC.

Celem tego samouczka jest wyjaśniają, jak utworzyć nowe platformy ASP.NET MVC kontrolerów. Dowiesz się, jak utworzyć kontrolerów, zarówno za pomocą opcji menu Visual Studio Dodaj kontroler, jak i przez utworzenie pliku klasy ręcznie.

### <a name="using-the-add-controller-menu-option"></a>Za pomocą Dodaj opcję Menu kontrolera

Najprostszym sposobem utworzenia nowego kontrolera jest kliknij prawym przyciskiem myszy folder kontrolerów, w oknie Eksploratora rozwiązań w usłudze Visual Studio i wybierz **Dodaj, kontroler** opcji menu (patrz rysunek 1). Wybranie tej opcji menu otwiera **Dodaj kontroler** okna dialogowego (patrz rysunek 2).

[![Okno dialogowe Nowy projekt](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Rysunek 01**: Dodawanie nowego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image2.png))

[![Okno dialogowe Nowy projekt](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Rysunek 02**: Okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image4.png))

Należy zauważyć, że pierwsza część nazwy kontrolera jest wyróżniony na **Dodaj kontroler** okna dialogowego. Każda nazwa kontrolera musi kończyć się sufiksem *kontrolera*. Na przykład można utworzyć kontroler o nazwie *ProductController* , ale nie kontroler o nazwie *produktu*.

Jeśli utworzysz kontroler, na których brakuje *kontrolera* sufiks, a następnie nie będzie można wywołać z kontrolerem. Nie ma tych — mam już zmarnowany niezliczone godziny swojego życia po wprowadzeniu tego błędu.

**Wyświetlanie listy 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Należy zawsze tworzyć kontrolerów w folderze kontrolerów. W przeciwnym razie będzie naruszenie Konwencji platformy ASP.NET MVC i inni deweloperzy mają coraz trudniejszy czasu, informacje o aplikacji.

### <a name="scaffolding-action-methods"></a>Metody akcji tworzenia szkieletów

Podczas tworzenia kontrolera, masz możliwość automatycznego wygenerowania metod akcji tworzenia, aktualizowania lub szczegóły (zobacz rysunek 3). Jeśli wybierzesz tę opcję, generowany jest klasy kontrolera w ofercie 2.

[![Automatyczne tworzenie metod akcji](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Rysunek 03**: Automatyczne tworzenie metod akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image6.png))

**Wyświetlanie listy 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Te wygenerowane metody są metodami klasy zastępczej. Należy dodać rzeczywiste Logic Apps do tworzenia, aktualizowania i wyświetlania szczegółów dla klienta, samodzielnie. Jednak metod klasy zastępczej zapewnia dobre rozwiązanie punkt początkowy.

### <a name="creating-a-controller-class"></a>Tworzenie klasy kontrolera

Kontroler ASP.NET MVC jest po prostu klasą. Jeśli wolisz, możesz zignorować wygodne szkieletu kontrolera programu Visual Studio i ręcznie utworzyć klasę kontrolera. Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy folder kontrolery i wybierz opcję menu **Dodaj, nowy element** i wybierz **klasy** szablonu (zobacz rysunek 4).
2. Nadaj nowej klasie PersonController.cs, a następnie kliknij przycisk **Dodaj** przycisku.
3. Zmodyfikuj plik wynikowy klasy, tak, aby klasa dziedziczy z klasy bazowej System.Web.Mvc.Controller (patrz lista 3).

[![Tworzenie nowej klasy](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Rysunek 04**: Tworzenie nowej klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-cs/_static/image8.png))

**Wyświetlanie listy 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Kontroler w ofercie 3 udostępnia jedną akcję o nazwie indeks(), która zwraca ciąg "Hello World!". Można wywołać tej akcji kontrolera, uruchamiając aplikację i żąda adresu URL, jak pokazano poniżej:

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET Development Server używa numeru portu losowego (na przykład 40071). Wprowadzenie adresu URL do wywołania z kontrolerem, należy podać numer portu w prawo. Należy określić numer portu, ustawiając kursor myszy na ikonie dla ASP.NET Development Server, w obszarze powiadomień (prawym dolnym rogu ekranu), Windows.
> 
> [!div class="step-by-step"]
> [Poprzednie](adding-dynamic-content-to-a-cached-page-cs.md)
> [dalej](creating-an-action-cs.md)

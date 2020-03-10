---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Tworzenie kontrolera (VB) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther pokazuje, jak można dodać kontroler do aplikacji ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601459"
---
# <a name="creating-a-controller-vb"></a>Tworzenie kontrolera (VB)

Autor [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther pokazuje, jak można dodać kontroler do aplikacji ASP.NET MVC.

Celem tego samouczka jest wyjaśnienie, jak można tworzyć nowe kontrolery MVC ASP.NET. Dowiesz się, jak utworzyć kontrolery przy użyciu opcji menu Dodaj kontroler programu Visual Studio i ręcznie tworząc plik klasy.

### <a name="using-the-add-controller-menu-option"></a>Korzystanie z opcji menu Dodaj kontroler

Najprostszym sposobem utworzenia nowego kontrolera jest kliknięcie prawym przyciskiem myszy folderu controllers w oknie Eksplorator rozwiązań programu Visual Studio i wybranie opcji **Dodaj, kontroler** (patrz rysunek 1). Wybranie tej opcji menu spowoduje otwarcie okna dialogowego **Dodawanie kontrolera** (patrz rysunek 2).

[![okno dialogowe Nowy projekt](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Ilustracja 01**. Dodawanie nowego kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image2.png))

[![okno dialogowe Nowy projekt](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Ilustracja 02**. okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](creating-a-controller-vb/_static/image4.png))

Zwróć uwagę, że pierwsza część nazwy kontrolera jest wyróżniona w oknie dialogowym **Dodaj kontroler** . Każda nazwa kontrolera musi kończyć się *kontrolerem*sufiksu. Na przykład można utworzyć kontroler o nazwie *ProductController* , ale nie kontroler o nazwie *Product*.

Jeśli utworzysz kontroler, na którym brakuje sufiksu *kontrolera* , nie będzie możliwe wywołanie kontrolera. Nie wykonuj tej czynności — po wprowadzeniu tego błędu niezliczone kilka godzin użytkowania.

**Lista 1 — Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Należy zawsze tworzyć kontrolery w folderze controllers. W przeciwnym razie nastąpi naruszenie Konwencji ASP.NET MVC, a inni deweloperzy będą mieli trudniejszy czas na zrozumienie aplikacji.

### <a name="scaffolding-action-methods"></a>Metody akcji tworzenia szkieletu

Podczas tworzenia kontrolera można automatycznie generować metody akcji Create, Update i Details (zobacz rysunek 3). Jeśli wybierzesz tę opcję, zostanie wygenerowana Klasa kontrolera na liście 2.

[![tworzenia metod akcji automatycznie](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Ilustracja 03**: automatyczne tworzenie metod akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image6.png))

**Lista 2 — Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Te wygenerowane metody są metodami zastępczymi. Należy dodać rzeczywistą logikę do tworzenia, aktualizowania i pokazywania szczegółowych informacji dla klienta. Jednak metody zastępcze zapewniają całkiem punkt wyjścia.

### <a name="creating-a-controller-class"></a>Tworzenie klasy kontrolera

Kontroler ASP.NET MVC jest tylko klasą. Jeśli wolisz, możesz zignorować wygodne tworzenie szkieletu kontrolera programu Visual Studio i ręcznie utworzyć klasę kontrolera. Wykonaj następujące kroki:

1. Kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz opcję menu **Dodaj, nowy element** i wybierz szablon **klasy** (patrz rysunek 4).
2. Nazwij nową klasę PersonController. vb i kliknij przycisk **Dodaj** .
3. Zmodyfikuj plik klasy powstającej w taki sposób, aby Klasa dziedziczy z klasy podstawowa system. Web. MVC. Controller (patrz lista 3).

[![tworzenia nowej klasy](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Ilustracja 04**: Tworzenie nowej klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-controller-vb/_static/image8.png))

**Lista 3 — Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Kontroler z listy 3 uwidacznia jedną akcję o nazwie index (), która zwraca ciąg "Hello world!". Możesz wywołać tę akcję kontrolera, uruchamiając aplikację i żądając adresu URL, takiego jak:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Serwer ASP.NET Development używa losowego numeru portu (na przykład 40071). Po wprowadzeniu adresu URL w celu wywołania kontrolera należy podać odpowiedni numer portu. Możesz określić numer portu, aktywując wskaźnik myszy nad ikoną serwera deweloperskiego ASP.NET w obszarze powiadomień systemu Windows (w prawym dolnym rogu ekranu).
> 
> [!div class="step-by-step"]
> [Poprzednie](adding-dynamic-content-to-a-cached-page-vb.md)
> [dalej](creating-an-action-vb.md)

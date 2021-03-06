---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Tworzenie tras niestandardowych (VB) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak dodać niestandardowe trasy do aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak zmodyfikować domyślną tabelę tras w pliku Global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 22b44e9e575c9d404881a23ee735bb0c8b7109e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601298"
---
# <a name="creating-custom-routes-vb"></a>Tworzenie tras niestandardowych (VB)

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać niestandardowe trasy do aplikacji ASP.NET MVC. W tym samouczku dowiesz się, jak zmodyfikować domyślną tabelę tras w pliku Global. asax.

W tym samouczku dowiesz się, jak dodać trasę niestandardową do aplikacji ASP.NET MVC. Dowiesz się, jak zmodyfikować domyślną tabelę tras w pliku Global. asax przy użyciu trasy niestandardowej.

W aplikacjach ASP.NET MVC domyślna tabela tras będzie działać prawidłowo. Można jednak stwierdzić, że masz specjalne potrzeby routingu. W takim przypadku można utworzyć trasę niestandardową.

Załóżmy na przykład, że tworzysz aplikację w blogu. Możesz chcieć obsługiwać przychodzące żądania, które wyglądają następująco:

/Archive/12-25-2009

Gdy użytkownik wprowadzi to żądanie, chce zwrócić wpis w blogu odpowiadający dacie 12/25/2009. Aby można było obsłużyć ten typ żądania, należy utworzyć trasę niestandardową.

Plik Global. asax znajdujący się na liście 1 zawiera nową trasę niestandardową o nazwie blog, która obsługuje żądania, które wyglądają jak*Data entry*/Archive/.

**Lista 1-Global. asax (z trasą niestandardową)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Kolejność tras dodawanych do tabeli tras jest ważna. Nasza nowa niestandardowa trasa blogu jest dodawana przed istniejącą domyślną trasą. Jeśli zamówienie zostało cofnięte, trasa domyślna zawsze zostanie wywołana zamiast trasy niestandardowej.

Niestandardowa trasa blogu dopasowuje wszystkie żądania, które zaczynają się od/Archive/. Tak więc pasuje do wszystkich następujących adresów URL:

- /Archive/12-25-2009

- /Archive/10-6-2004

- /Archive/apple

Trasa niestandardowa mapuje żądanie przychodzące do kontrolera o nazwie archiwalne i wywołuje akcję wejścia (). Gdy wywoływana jest metoda entry (), Data wejścia jest przenoszona jako parametr o nazwie entryDate.

Możesz użyć trasy niestandardowej blogu z kontrolerem na liście 2.

**Lista 2 — ArchiveController. vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Zwróć uwagę, że metoda entry () w liście 2 akceptuje parametr typu DateTime. Struktura MVC jest wystarczająco inteligentna, aby przekonwertować datę wejścia z adresu URL na wartość typu DateTime. Jeśli nie można przekonwertować parametru daty wpisu z adresu URL na DateTime, zostanie zgłoszony błąd (patrz rysunek 1).

**Rysunek 1. Błąd konwersji parametru**

[![okno dialogowe Nowy projekt](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Ilustracja 01**. błąd podczas konwertowania parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-routes-vb/_static/image2.png))

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było zademonstrowanie sposobu tworzenia trasy niestandardowej. Dowiesz się, jak dodać trasę niestandardową do tabeli tras w pliku Global. asax, która reprezentuje wpisy w blogu. Omawiamy sposób mapowania żądań wpisów blogu do kontrolera o nazwie ArchiveController i akcji kontrolera o nazwie entry ().

> [!div class="step-by-step"]
> [Poprzednie](asp-net-mvc-controller-overview-vb.md)
> [dalej](creating-a-route-constraint-vb.md)

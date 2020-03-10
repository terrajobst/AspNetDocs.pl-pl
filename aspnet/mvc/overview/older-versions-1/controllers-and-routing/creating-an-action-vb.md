---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Tworzenie akcji (VB) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metody jako akcji.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: b1b53bea899deecef203551b23c087944e3990ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582006"
---
# <a name="creating-an-action-vb"></a>Tworzenie akcji (VB)

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metody jako akcji.

Celem tego samouczka jest wyjaśnienie, jak można utworzyć nową akcję kontrolera. Dowiesz się więcej na temat wymagań metody akcji. Dowiesz się również, jak zapobiec ujawnieniu metody jako akcji.

## <a name="adding-an-action-to-a-controller"></a>Dodawanie akcji do kontrolera

Dodaj nową akcję do kontrolera, dodając nową metodę do kontrolera. Na przykład kontroler z listą 1 zawiera akcję o nazwie index () i akcję o nazwie SayHello (). Obie metody są udostępniane jako akcje.

**Lista 1 — Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Aby można było uwidocznić w programie Universe jako czynność, metoda musi spełniać pewne wymagania:

- Metoda musi być publiczna.
- Metoda nie może być metodą statyczną.
- Metoda nie może być metodą rozszerzenia.
- Metoda nie może być konstruktora, metody pobierającej ani metody ustawiającej.
- Metoda nie może mieć otwartych typów ogólnych.
- Metoda nie jest metodą klasy bazowej kontrolera.
- Metoda nie może zawierać parametrów **ref** ani **out** .

Zwróć uwagę, że nie ma żadnych ograniczeń dla zwracanego typu akcji kontrolera. Akcja kontrolera może zwracać ciąg, typ DateTime, wystąpienie klasy losowej lub typ void. Platforma ASP.NET MVC przekonwertuje wszystkie typy zwracane, które nie są wynikiem akcji na ciąg i renderuje ciąg do przeglądarki.

Po dodaniu dowolnej metody, która nie narusza tych wymagań dla kontrolera, metoda jest uwidaczniana jako akcja kontrolera. Tutaj należy zachować ostrożność. Akcja kontrolera może być wywoływana przez dowolną osobę, która jest połączona z Internetem. Nie można na przykład utworzyć akcji kontrolera DeleteMyWebsite ().

## <a name="preventing-a-public-method-from-being-invoked"></a>Uniemożliwianie wywołania metody publicznej

Jeśli musisz utworzyć metodę publiczną w klasie kontrolera i nie chcesz ujawniać metody jako akcji kontrolera, możesz uniemożliwić wywoływanie metody przy użyciu atrybutu&gt; &lt;nie akcja. Na przykład kontroler na liście 2 zawiera metodę publiczną o nazwie CompanySecrets (), która jest uzupełniona atrybutem&gt; &lt;nie akcja.

**Lista 2 — Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Jeśli spróbujesz wywołać akcję kontrolera CompanySecrets (), wpisując/Work/CompanySecrets na pasku adresu przeglądarki, zostanie wyświetlony komunikat o błędzie na rysunku 1.

[![wywoływania metody niefunkcjonalnej](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Ilustracja 01**: wywoływanie metody niefunkcjonalnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Poprzednie](creating-a-controller-vb.md)
> [dalej](aspnet-mvc-controllers-overview-cs.md)

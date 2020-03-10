---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Jak: Tworzenie niestandardowego pomocnika HTML dla aplikacji MVC? | Microsoft Docs'
author: rick-anderson
description: W tym filmie wideo Krzysztof pikseli pokazuje, jak utworzyć niestandardowy HtmlHelper, który nie jest dostępny w standardowym zestawie w aplikacji MVC. Najpierw przykładowy zastoso MVC...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 60953243d3038667e4f729b1394e68f0c9d7c178
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559046"
---
# <a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a>Jak: Tworzenie niestandardowego pomocnika HTML dla aplikacji MVC?

[Krzysztof pikseli](https://twitter.com/chrispels)

W tym filmie wideo Krzysztof pikseli pokazuje, jak utworzyć niestandardowy HtmlHelper, który nie jest dostępny w standardowym zestawie w aplikacji MVC. Najpierw zostanie utworzona Przykładowa aplikacja MVC z kontrolerem demonstracyjnym i widok służący do testowania niestandardowego HtmlHelper. Następnie moduł jest tworzony przy użyciu funkcji publicznej, która jest metodą rozszerzenia, która reprezentuje implementację niestandardowego HtmlHelper. Pomocnik niestandardowy służy do tworzenia tagów `<img>` na stronie i otrzymywania kilku parametrów przychodzących, takich jak identyfikator, adres URL i tekst alternatywny dla tagu obrazu. Następnie logika jest dodawana do funkcji w celu zwrócenia znacznika ukończony `<img>` z określonymi informacjami. Następnie niestandardowy HtmlHelper jest używany na stronie demonstracyjnej do wyświetlania obrazu. Na koniec niestandardowy HtmlHelper jest rozwinięty, aby obejmował wiele zastąpień konstruktora, które zapewniają elastyczność umożliwiającą łatwiejsze tworzenie różnych tagów `<img>`.

[&#9654;Obejrzyj wideo (18 minut)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> [Poprzednie](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [dalej](how-do-i-work-with-model-binders-in-an-mvc-application.md)

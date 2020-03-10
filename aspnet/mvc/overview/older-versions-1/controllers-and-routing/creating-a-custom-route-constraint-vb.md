---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Tworzenie niestandardowego ograniczenia trasy (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstruje, jak można utworzyć niestandardowe ograniczenie trasy. Implementujemy proste ograniczenie niestandardowe, które uniemożliwia dopasowanie trasy w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2330708cf4a28180ce8a05f4696bf7a7a32092d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601410"
---
# <a name="creating-a-custom-route-constraint-vb"></a>Tworzenie niestandardowego ograniczenia trasy (VB)

Autor [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther demonstruje, jak można utworzyć niestandardowe ograniczenie trasy. Implementujemy proste ograniczenie niestandardowe, które uniemożliwia dopasowanie trasy w przypadku żądania przeglądarki z komputera zdalnego.

Celem tego samouczka jest zademonstrowanie sposobu tworzenia niestandardowego ograniczenia trasy. Ograniczenie trasy niestandardowej umożliwia uniemożliwienie dopasowania trasy, chyba że jest dopasowywany warunek niestandardowy.

W tym samouczku utworzysz ograniczenie trasy hosta lokalnego. Ograniczenie trasy hosta lokalnego odpowiada tylko na żądania wysyłane z komputera lokalnego. Żądania zdalne przesyłane przez Internet nie są zgodne.

Implementowanie niestandardowego ograniczenia trasy przez implementację interfejsu IRouteConstraint. Jest to bardzo prosty interfejs, który opisuje jedną metodę:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Metoda zwraca wartość logiczną. Jeśli zwracasz wartość false, trasa skojarzona z ograniczeniem nie będzie zgodna z żądaniem przeglądarki.

Ograniczenie localhost jest zawarte w liście 1.

**Lista 1 — LocalhostConstraint. vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Ograniczenie dotyczące listy 1 wykorzystuje Właściwość IsLocal uwidocznioną przez klasę HttpRequest. Ta właściwość zwraca wartość PRAWDA, jeśli adres IP żądania to 127.0.0.1 lub gdy adres IP żądania jest taki sam, jak w przypadku adresu IP serwera.

Używasz ograniczenia niestandardowego w ramach trasy zdefiniowanej w pliku Global. asax. Plik Global. asax na liście 2 używa ograniczenia localhost, aby uniemożliwić wszystkim użytkownikom żądanie strony administratora, chyba że zgłasza żądanie z serwera lokalnego. Na przykład żądanie/Admin/DeleteAll zakończy się niepowodzeniem, gdy zostanie wykonane z serwera zdalnego.

**Lista 2 — Global. asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Ograniczenie localhost jest używane w definicji trasy administratora. Ta trasa nie będzie zgodna z żądaniem przeglądarki zdalnej. Należy jednak pamiętać, że inne trasy zdefiniowane w Global. asax mogą być zgodne z tym samym żądaniem. Ważne jest, aby zrozumieć, że ograniczenie uniemożliwia określonej trasie dopasowanie żądania i nie wszystkie trasy zdefiniowane w pliku Global. asax.

Zwróć uwagę, że trasa domyślna została oznaczona jako komentarz z pliku Global. asax na liście 2. W przypadku uwzględnienia trasy domyślnej trasy domyślnej byłyby zgodne z żądaniami kontrolera administratora. W takim przypadku użytkownicy zdalni nadal mogą wywoływać akcje kontrolera administratora, mimo że ich żądania nie są zgodne z trasą administracyjną.

> [!div class="step-by-step"]
> [Wstecz](creating-a-route-constraint-vb.md)

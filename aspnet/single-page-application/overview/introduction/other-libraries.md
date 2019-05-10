---
uid: single-page-application/overview/introduction/other-libraries
title: Znasz biblioteki inne niż Knockout? | Microsoft Docs
author: madskristensen
description: Znasz biblioteki inne niż Knockout?
ms.author: riande
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 64a4ad1fb411f7291a5cba634afdf4d2fdb16d55
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65116083"
---
# <a name="know-a-library-other-than-knockout"></a>Znasz biblioteki inne niż Knockout?

przez [: Mads Kristensen](https://github.com/madskristensen)

[Szablonu jednej strony aplikacji (SPA)](knockoutjs-template.md) to świetny sposób, aby rozpocząć pisanie aplikacji jednostronicowej. Szablon używa [KnockoutJS](http://knockoutjs.com/) powiązać elementów DOM danych aplikacji.

Ale separowania na ostro nie tylko biblioteki JavaScript do tworzenia rozbudowanych aplikacji klienckich aplikacji. Inne biblioteki wyzwaniom podobne na różne sposoby. Jedna biblioteka preferować za pośrednictwem innego, więc wprowadziliśmy kilka szablonów utworzonych przez społeczność dostępne do pobrania. Każdy z tych szablonów używa kombinacji różnych bibliotek klienckich dla języka JavaScript.

Aby zainstalować szablon utworzonych przez społeczność, odwiedź jedną z szablonu stron wymienione poniżej, a następnie kliknij przycisk pobierania. Szablony są dostarczane jako plików VSIX.

## <a name="backbonejs"></a>BackboneJS

[Szablon SPA backbone.js](../templates/backbonejs-template.md). Ten szablon zawiera szkielet początkowy do tworzenia [Backbone.js](http://backbonejs.org/) aplikacji na platformie ASP.NET MVC. Gotowych zapewnia funkcje logowania użytkowników w warstwie podstawowa, w tym, resetowanie hasła tworzenia nowych kont i logowania użytkownika i potwierdzenie przez użytkownika za pomocą szablonów podstawowy adres e-mail.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) to biblioteka typu open source do zarządzania danymi sformatowanego w klienta JavaScript. Szybka i bezproblemowa obsługuje zapytania, buforowania, śledzenia zmian, weryfikacji i nie tylko. Dwa szablony funkcji Breeze:

- [Breeze/Knockout](../templates/breezeknockout-template.md) szablonu rozszerza szablonu Knockout SPA, pokazujący, jak łatwo można tworzenie aplikacji jednej strony z szybka i bezproblemowa dla zarządzania danymi i KnockoutJS dla powiązania danych.
- [Breeze/Angular](../templates/breezeangular-template.md) rozszerzają szablonu Knockout SPA, szybka i bezproblemowa, ale przy użyciu szablonu [AngularJS](http://angularjs.org) biblioteki dla wiązania danych, wstrzykiwanie zależności i zarządzanie ekranu.

Ponadto [szablon Hot Towel SPA](../templates/hottowel-template.md) używa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Szablon EmberJS SPA](../templates/emberjs-template.md). Ten szablon używa [użyciu](http://emberjs.com/), zaawansowane biblioteki MVC JavaScript, która rozwiązuje szeroką gamę wyzwania do tworzenia zaawansowanych aplikacji klienckich.

Szablon użyciu SPA jest ponownego wdrażania szablonu Knockout SPA, korzystając z szablonów EmberJS i Handlebars.

## <a name="hot-towel"></a>Hot Towel

[Hot Towel SPA szablonu](../templates/hottowel-template.md). Ten szablon łączy w kilku bibliotek JavaScript, w tym szybka i bezproblemowa, Knockout i RequireJS oraz Twitter Bootstrap.

W porównaniu z innych szablonów wymienione w tym miejscu, szablon Hot Towel zapewnia bardziej szczegółowy aplikacji, z której można tworzyć własne. Istnieją więcej koncepcji, które należy zwrócić uwagę, ale po zrozumieniu ich ten szablon może być po prostu czego szukasz. Jeśli chcesz skompilować SPA, ale nie podjęcie decyzji rozpocząć, należy użyć Hot Towel, a w ciągu kilku sekund będziesz mieć SPA i wszystkie narzędzia potrzebne do tworzenia na nim.

## <a name="feature-table"></a>Tabela funkcji

Poniżej przedstawiono funkcji oferowanych przez każdy szablon SPA:

|                        | ASP.NET SPA | Sieci szkieletowej | BREEZE/Angular | Szybka i bezproblemowa/KO |  Użyciu platformy ember   | Hot Towel |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Przykładowe ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Szablon kompletnego      |             | &#10003; |                |           |          | &#10003;  |
| Nawigacji i Historia |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Biblioteki       |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Użyciu platformy ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

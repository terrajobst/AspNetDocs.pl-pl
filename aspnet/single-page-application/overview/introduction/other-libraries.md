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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578548"
---
# <a name="know-a-library-other-than-knockout"></a>Znasz biblioteki inne niż Knockout?

Autor [produktywność Madsa Kristensena](https://github.com/madskristensen)

[Szablon aplikacji jednostronicowej (Spa)](knockoutjs-template.md) to doskonały sposób na rozpoczęcie pisania aplikacji jednostronicowych. Szablon używa [KnockoutJS](http://knockoutjs.com/) do wiązania danych aplikacji z elementami modelu DOM.

Jednak odcinanie nie jest jedyną biblioteką języka JavaScript do tworzenia rozbudowanych aplikacji klienckich. Inne biblioteki rozwiązują podobne wyzwania na różne sposoby. Możesz preferować jedną bibliotekę w innej, więc wprowadziliśmy kilka szablonów utworzonych przez społeczność do pobrania. Każdy z tych szablonów używa innej kombinacji bibliotek JavaScript klienta.

Aby zainstalować szablon utworzony przez społeczność, odwiedź jedną z wymienionych poniżej stron szablonów, a następnie kliknij przycisk Pobierz. Szablony są udostępniane jako pliki VSIX.

## <a name="backbonejs"></a>BackboneJS

[Szablon spa. js](../templates/backbonejs-template.md). Ten szablon zawiera początkowy szkielet tworzenia aplikacji sieci [szkieletowej](http://backbonejs.org/) w języku ASP.NET MVC. Poza tym pole zapewnia podstawowe funkcje logowania użytkowników, w tym rejestrowanie użytkowników, logowanie, resetowanie haseł i potwierdzenie użytkownika przy użyciu podstawowych szablonów wiadomości e-mail.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) to biblioteka typu open source służąca do zarządzania rozbudowanymi danymi w kliencie JavaScript. Breeze obsługuje zapytania, buforowanie, śledzenie zmian, sprawdzanie poprawności i inne. Breeze funkcji dwóch szablonów:

- Szablon [Breeze/odcinania](../templates/breezeknockout-template.md) rozszerza szablon Spa odcinania, pokazując jak łatwo można skompilować aplikację jednostronicową z Breeze do zarządzania danymi i KnockoutJS na potrzeby tworzenia powiązań danych.
- Szablon [Breeze/kątowy](../templates/breezeangular-template.md) rozszerza również szablon odcinania spa z Breeze, ale używa biblioteki [AngularJS](http://angularjs.org) do tworzenia powiązań danych, iniekcji zależności i zarządzania ekranem.

Ponadto [szablon Hot ręczników Spa](../templates/hottowel-template.md) używa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Szablon Spa EmberJS](../templates/emberjs-template.md). Ten szablon używa [wpływ](http://emberjs.com/), zaawansowanej biblioteki języka JavaScript w języku MVC, która rozwiązuje szeroką gamę wyzwań związanych z tworzeniem rozbudowanych aplikacji klienckich.

Szablon SPA wpływ jest ponowną implementacją szablonu odcinania SPA przy użyciu EmberJS i kierownicy tworzenia szablonów.

## <a name="hot-towel"></a>Gorąca ręczników

[Szablon Hot ręczników Spa](../templates/hottowel-template.md). Ten szablon zawiera kilka bibliotek JavaScript, w tym Breeze, odcinania, RequireJS i Twitter Bootstrap.

W porównaniu z innymi szablonami wymienionymi w tym miejscu, szablon gorąca ręczników zapewnia bardziej kompletną aplikację, z której można utworzyć własne. Istnieje więcej koncepcji, które należy wziąć pod uwagę, ale po ich zrozumieniu ten szablon może być właśnie wyszukiwany. Jeśli chcesz skompilować SPA, ale nie możesz zdecydować, gdzie zacząć, użyj gorącej ręczników i w ciągu kilku sekund, będziesz mieć SPA i wszystkie narzędzia potrzebne do jego skompilowania.

## <a name="feature-table"></a>Tabela funkcji

Poniżej przedstawiono funkcje udostępniane przez każdy szablon SPA:

|                        | ASP.NET SPA | Szybkości | Breeze/kątowy | Breeze/KO |  Ember   | Gorąca ręczników |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Przykład do zrobienia       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Szablon systemu operacyjnego      |             | &#10003; |                |           |          | &#10003;  |
| Nawigacja i historia |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Biblioteki       |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Separowanie        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

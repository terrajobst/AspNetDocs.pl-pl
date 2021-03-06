---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Omówienie routingu ASP.NET MVC (C#) | Microsoft Docs
author: StephenWalther
description: W tym samouczku Stephen Walther pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e1155ca676e7a25b5bfc63e251c6387a010eb34
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544143"
---
# <a name="aspnet-mvc-routing-overview-c"></a>Omówienie routingu we wzorcu ASP.NET MVC (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.

W tym samouczku wprowadzasz do ważnej funkcji każdej aplikacji ASP.NET MVC o nazwie *ASP.NET routing*. Moduł routingu ASP.NET jest odpowiedzialny za mapowanie przychodzących żądań przeglądarki do określonych akcji kontrolera MVC. Na końcu tego samouczka dowiesz się, jak standardowa tabela tras mapuje żądania na akcje kontrolera.

## <a name="using-the-default-route-table"></a>Korzystanie z tabeli tras domyślnych

Podczas tworzenia nowej aplikacji ASP.NET MVC aplikacja jest już skonfigurowana do używania routingu ASP.NET. Routing ASP.NET jest skonfigurowany w dwóch miejscach.

Najpierw ASP.NET routing jest włączony w pliku konfiguracji sieci Web aplikacji (plik Web. config). W pliku konfiguracyjnym znajdują się cztery sekcje dotyczące routingu: sekcja system. Web. httpModules, sekcja system. Web. httpHandlers, sekcja system. WebServer. module oraz sekcja system. WebServer. programy obsługi. Należy zachować ostrożność, aby nie usuwać tych sekcji, ponieważ nie będą już działać.

Drugi i co ważniejsze, tabela tras jest tworzona w pliku Global. asax aplikacji. Plik Global. asax to specjalny plik, który zawiera programy obsługi zdarzeń dla zdarzeń cyklu życia aplikacji ASP.NET. Tabela tras jest tworzona podczas zdarzenia uruchamiania aplikacji.

Plik z listą 1 zawiera domyślny plik Global. asax dla aplikacji ASP.NET MVC.

**Lista 1 — Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Po pierwszym uruchomieniu aplikacji MVC wywoływana jest metoda\_Start () aplikacji. Ta metoda z kolei wywołuje metodę RegisterRoutes (). Metoda RegisterRoutes () tworzy tabelę tras.

Domyślna tabela tras zawiera pojedynczą trasę (nazwa domyślna). Trasa domyślna mapuje pierwszy segment adresu URL na nazwę kontrolera, drugi segment adresu URL do akcji kontrolera i trzeci segment do parametru o nazwie **ID**.

Załóżmy, że na pasku adresu przeglądarki sieci Web wprowadzisz następujący adres URL:

/Home/Index/3

Trasa domyślna mapuje ten adres URL na następujące parametry:

- Kontroler = Strona główna

- Akcja = indeks

- id = 3

Gdy żądanie adresu URL/Home/Index/3, zostanie wykonany następujący kod:

HomeController.Index(3)

Domyślna trasa obejmuje wartości domyślne dla wszystkich trzech parametrów. Jeśli nie podasz kontrolera, parametr Controller domyślnie przyjmuje wartość **Home**. Jeśli nie podasz akcji, parametr Action domyślnie przyjmuje wartość **index**. Na koniec, jeśli nie podasz identyfikatora, parametr identyfikatora zostanie domyślnie pustym ciągiem.

Przyjrzyjmy się kilku przykładom, jak trasa domyślna mapuje adresy URL na akcje kontrolera. Załóżmy, że na pasku adresu przeglądarki wprowadzisz następujący adres URL:

/Home

Ze względu na domyślne wartości domyślnych parametrów trasy wprowadzenie tego adresu URL spowoduje wywołanie metody index () klasy HomeController na liście 2.

**Lista 2 — HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Na liście 2 Klasa HomeController zawiera metodę o nazwie index (), która akceptuje pojedynczy parametr o nazwie ID. /Home URL powoduje wywołanie metody index () z pustym ciągiem jako wartość parametru ID.

Ze względu na sposób, w jaki Struktura MVC wywołuje akcje kontrolera, adres URL/Home również jest zgodny z metodą index () klasy HomeController na liście 3.

**Lista 3-HomeController.cs (Akcja indeksu bez parametru)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Metoda index () na liście 3 nie akceptuje żadnych parametrów. /Home URL spowoduje wywołanie tej metody index (). /Home/Index/3 adresu URL wywołuje również tę metodę (identyfikator jest ignorowany).

/Home adresu URL jest również zgodna z metodą index () klasy HomeController na liście 4.

**Lista 4-HomeController.cs (Akcja indeksu z parametrem dopuszczającym wartość null)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

W przypadku listy 4 Metoda index () ma jeden parametr Integer. Ponieważ parametr jest parametrem wartości null (może mieć wartość null), można wywołać indeks () bez zgłaszania błędu.

Na koniec wywoływanie metody index () w liście 5 z adresem URL/Home powoduje wyjątek, ponieważ parametr ID *nie jest* parametrem dopuszczającym wartość null. Jeśli spróbujesz wywołać metodę index (), pojawi się komunikat o błędzie wyświetlany na rysunku 1.

**Lista 5-HomeController.cs (Akcja indeksu z parametrem identyfikatora)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]

[![wywoływania akcji kontrolera, która oczekuje wartości parametru](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Ilustracja 01**: Wywoływanie akcji kontrolera, która oczekuje wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-routing-overview-cs/_static/image2.png))

Adres URL/Home/Index/3, z drugiej strony, działa prawidłowo z akcją kontrolera indeksu w temacie 5. Żądanie/Home/Index/3 powoduje wywołanie metody index () z parametrem ID o wartości 3.

## <a name="summary"></a>Podsumowanie

Celem tego samouczka było dostarczenie krótkiego wprowadzenia do routingu ASP.NET. Zbadamy domyślną tabelę tras uzyskaną z nową aplikacją ASP.NET MVC. Wiesz już, jak trasa domyślna mapuje adresy URL na akcje kontrolera.

> [!div class="step-by-step"]
> [Dalej](understanding-action-filters-cs.md)

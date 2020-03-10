---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalowanie pomocnika w witrynie ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik to składnik wielokrotnego użytku, który zawiera kod i adiustację do...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638587"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalowanie pomocnika w witrynie ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). *Pomocnik* to składnik wielokrotnego użytku, który zawiera kod i znaczniki umożliwiające wykonanie zadania, które może być żmudnym lub złożone.
> 
> Zawartość:
> 
> - Jak zainstalować pomocnika w witrynie sieci Web utworzonej przy użyciu programu WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - WebMatrix 3

## <a name="overview-of-helpers"></a>Przegląd pomocników

Niektóre zadania, które często mają być wykonywane na stronach sieci Web, wymagają dużej ilości kodu lub wymagają dodatkowej wiedzy. Przykłady obejmują Wyświetlanie wykresu dla danych; Umieszczanie przycisku "Obserwuj" w usłudze Twitter na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie lub zmienianie rozmiarów obrazów; Korzystanie z systemu PayPal dla witryny. Aby ułatwić Ci wykonywanie tego rodzaju rzeczy, ASP.NET strony sieci Web umożliwiają korzystanie z *pomocników*. Pomocnicy są składnikami instalowanymi dla lokacji i, które umożliwiają wykonywanie typowych zadań przy użyciu tylko linii lub dwóch kodów Razor.

Strony sieci Web ASP.NET zawierają kilka pomocników wbudowanych w. Jednak wiele pomocników jest dostępnych w pakietach (dodatków) udostępnianych za pomocą Menedżera pakietów NuGet. Pakiet NuGet umożliwia wybranie pakietu do zainstalowania, a następnie uwzględnia wszystkie szczegóły instalacji.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalowanie pomocnika w programie WebMatrix 3

1. W programie WebMatrix 3 Kliknij przycisk **NuGet** .

    ![Okno dialogowe Galeria NuGet w programie WebMatrix](installing-helpers/_static/image1.png)
2. Spowoduje to uruchomienie Menedżera pakietów NuGet i wyświetlenie dostępnych pakietów. W polu wyszukiwania wprowadź słowo kluczowe pomocnika, który chcesz zainstalować.

    ![Okno dialogowe Galeria NuGet w programie WebMatrix](installing-helpers/_static/image2.png)
3. Wybierz pakiet, a następnie kliknij przycisk **Instaluj**. Kliknij przycisk **tak** po wyświetleniu monitu, jeśli chcesz zainstalować pakiet i wskazać, że akceptujesz warunki.

     Jeśli po raz pierwszy zainstalowano pomocnika, program NuGet utworzy foldery w witrynie sieci Web dla kodu, który tworzy pomocnika.
4. Aby odinstalować pomocnika, kliknij przycisk **Galeria** , kliknij kartę **zainstalowane** i wybierz pakiet, który chcesz odinstalować.

## <a name="installing-the-twitter-helper"></a>Instalowanie pomocnika usługi Twitter

Najnowsza wersja interfejsu API usługi Twitter nie jest zgodna z pomocnikiem usługi Twitter instalowanym za pomocą narzędzia NuGet. Zamiast tego zapoznaj się z tematem [pomocnik usługi Twitter z WebMatrix](twitter-helper.md) , aby uzyskać informacje na temat sposobu konfigurowania pomocnika usługi Twitter w projekcie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Wprowadzenie do stron sieci Web ASP.NET 2 — Podstawy programowania](../getting-started/introducing-razor-syntax-c.md)

[Pomocnik usługi Twitter w programie WebMatrix](twitter-helper.md)

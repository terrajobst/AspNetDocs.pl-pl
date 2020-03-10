---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Samouczek: Dostosowywanie widoku dla Database First EF z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na zmianie automatycznie generowanych widoków w celu ulepszenia prezentacji.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583595"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: Dostosowywanie widoku dla Database First EF z aplikacją ASP.NET MVC

Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych. Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.

Ten samouczek koncentruje się na zmianie automatycznie generowanych widoków w celu ulepszenia prezentacji.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodawanie kursów do strony szczegółów ucznia
> * Potwierdź, że kursy są dodawane do strony

## <a name="prerequisites"></a>Wymagania wstępne

* [Zmiana bazy danych](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Dodawanie kursów do szczegółów ucznia

Wygenerowany kod stanowi dobry punkt wyjścia dla aplikacji, ale nie musi zapewniać wszystkich funkcji potrzebnych w aplikacji. Można dostosować kod w celu spełnienia określonych wymagań aplikacji. Obecnie aplikacja nie wyświetla zarejestrowanych kursów dla wybranego ucznia. W tej sekcji dodasz zarejestrowane kursy dla każdego ucznia do widoku **szczegółów** dla ucznia.

Otwórz **widoki** > **uczniów** > *szczegóły. cshtml*. Poniżej ostatniego znacznika &lt;/DL&gt;, ale przed zamykającym tagiem &lt;/DIV&gt; Dodaj następujący kod.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla wybranego ucznia. Metoda **Display** renderuje HTML dla obiektu (ModelItem), który reprezentuje wyrażenie. Aby upewnić się, że wartość jest poprawnie sformatowana w oparciu o typ i szablon tego typu, należy użyć metody wyświetlania (zamiast osadzania wartości właściwości w kodzie). W tym przykładzie każde wyrażenie zwraca pojedynczą właściwość z bieżącego rekordu w pętli, a wartości są typami pierwotnymi, które są renderowane jako tekst.

## <a name="confirm-courses-are-added"></a>Dodano kursy potwierdzające

Uruchom rozwiązanie. Kliknij **listę studentów** i wybierz **szczegóły** dla jednego z uczniów. W widoku zostaną wyświetlone zarejestrowane kursy.

![student z rejestracją](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodano kursy do strony szczegółów ucznia
> * Potwierdzenie, że kursy są dodawane do strony

Przejdź do następnego samouczka, aby dowiedzieć się, jak dodać adnotacje do danych, aby określić wymagania dotyczące weryfikacji i formatowanie wyświetlania.
> [!div class="nextstepaction"]
> [Ulepszanie walidacji danych](enhancing-data-validation.md)

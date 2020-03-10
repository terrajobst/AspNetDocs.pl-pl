---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Samouczek: zmiana bazy danych dla Database First EF z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na wprowadzaniu aktualizacji struktury bazy danych i propagowaniu zmian w całej aplikacji sieci Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616264"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: zmiana bazy danych dla Database First EF z aplikacją ASP.NET MVC

Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych. Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.

Ten samouczek koncentruje się na wprowadzaniu aktualizacji struktury bazy danych i propagowaniu zmian w całej aplikacji sieci Web.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodawanie kolumny
> * Dodawanie właściwości do widoków

## <a name="prerequisites"></a>Wymagania wstępne

* [Generowanie widoków](generating-views.md)

## <a name="add-a-column"></a>Dodawanie kolumny

W przypadku aktualizowania struktury tabeli w bazie danych należy upewnić się, że zmiana jest propagowana do modelu danych, widoków i kontrolera.

W tym samouczku dodasz nową kolumnę do tabeli Students, aby zarejestrować nazwę środkową ucznia. Aby dodać tę kolumnę, Otwórz projekt bazy danych i Otwórz plik student. SQL. Za pomocą projektanta lub kodu T-SQL Dodaj kolumnę o nazwie **MiddleName** , która jest nvarchar (50) i dopuszcza wartości null.

Wdróż tę zmianę w lokalnej bazie danych, uruchamiając projekt bazy danych (lub F5). Nowe pole zostanie dodane do tabeli. Jeśli nie widzisz go w Eksplorator obiektów SQL Server, kliknij przycisk Odśwież w okienku.

![Pokaż nową kolumnę](changing-the-database/_static/image2.png)

Nowa kolumna istnieje w tabeli bazy danych, ale nie istnieje obecnie w klasie modelu danych. Musisz zaktualizować model, aby dołączyć nową kolumnę. W folderze **modele** Otwórz plik **ContosoModel. edmx** , aby wyświetlić Diagram modelu. Zwróć uwagę, że model ucznia nie zawiera właściwości MiddleName. Kliknij prawym przyciskiem myszy w dowolnym miejscu na powierzchni projektowej, a następnie wybierz polecenie **Aktualizuj model z bazy danych**.

W Kreatorze aktualizacji wybierz kartę **Odśwież** , a następnie wybierz pozycję **tabele** > **dbo** > **student**. Kliknij przycisk **Finish** (Zakończ).

Po zakończeniu procesu aktualizacji diagram bazy danych zawiera nową właściwość **MiddleName** . Zapisz plik **ContosoModel. edmx** . Musisz zapisać ten plik, aby uzyskać nową właściwość do propagowania do klasy **student.cs** . Baza danych i model zostały zaktualizowane.

Skompiluj rozwiązanie.

## <a name="add-the-property-to-the-views"></a>Dodawanie właściwości do widoków

Niestety, widoki nadal nie zawierają nowej właściwości. Aby zaktualizować widoki, które są dostępne, możesz ponownie wygenerować widoki przez ponowne dodanie szkieletu dla klasy uczniów lub można ręcznie dodać nową właściwość do istniejących widoków. W tym samouczku dodasz szkielet ponownie, ponieważ nie wprowadzono żadnych niestandardowych zmian w widokach generowanych automatycznie. Możesz rozważyć ręczne dodanie właściwości po wprowadzeniu zmian w widokach i nie chcesz utracić tych zmian.

Aby upewnić się, że widoki są odtwarzane, Usuń folder **uczniów** w obszarze **widoki**i Usuń **StudentsController**. Następnie kliknij prawym przyciskiem myszy folder **controllers** , a następnie Dodaj szkielet dla modelu **ucznia** . Ponownie Nazwij kontroler **StudentsController**. Wybierz pozycję **Dodaj**.

Ponownie skompiluj rozwiązanie. Widoki zawierają teraz Właściwość MiddleName.

![Pokaż drugie imię](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodano kolumnę
> * Dodano właściwość do widoków

Przejdź do następnego samouczka, aby dowiedzieć się, jak dostosować widok na potrzeby wyświetlania szczegółowych informacji o rekordzie ucznia.
> [!div class="nextstepaction"]
> [Dostosowywanie widoku](customizing-a-view.md)
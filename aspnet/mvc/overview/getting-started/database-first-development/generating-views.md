---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Samouczek: Generowanie widoków dla Database First EF z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na używaniu szkieletu ASP.NET w celu wygenerowania kontrolerów i widoków.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616208"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: Generowanie widoków dla Database First EF z aplikacją ASP.NET MVC

Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych. Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.

Ten samouczek koncentruje się na używaniu szkieletu ASP.NET w celu wygenerowania kontrolerów i widoków.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodawanie szkieletu
> * Dodawanie linków do nowych widoków
> * Wyświetl widoki uczniów
> * Wyświetl widoki rejestracji

## <a name="prerequisite"></a>Wymagania wstępne

* [Tworzenie aplikacji sieci Web i modeli danych](creating-the-web-application.md)

## <a name="add-scaffold"></a>Dodawanie szkieletu

Możesz przystąpić do generowania kodu, który będzie dostarczać standardowe operacje na danych dla klas modelu. Kod można dodać poprzez dodanie elementu szkieletu. Istnieje wiele opcji dla typu szkieletu, który można dodać; w tym samouczku szkielet obejmuje kontroler i widoki zgodne z modelami uczniów i rejestracji utworzonymi w poprzedniej sekcji.

Aby zachować spójność w projekcie, należy dodać nowy kontroler do folderu istniejące **Kontrolery** . Kliknij prawym przyciskiem myszy folder **controllers** , a następnie wybierz pozycję **Dodaj** > **nowy element szkieletowy**.

Wybierz **kontroler MVC 5 z widokami, używając opcji Entity Framework** . Ta opcja spowoduje wygenerowanie kontrolera i widoków do aktualizowania, usuwania, tworzenia i wyświetlania danych w modelu.

![Dodaj kontroler MVC](generating-views/_static/image2.png)

Wybierz pozycję **student (ContosoSite. models)** dla klasy model i wybierz pozycję **ContosoUniversityDataEntities (ContosoSite. models)** dla klasy kontekstu. Zachowaj nazwę kontrolera jako **StudentsController**.

Kliknij pozycję **Add** (Dodaj).

Jeśli wystąpi błąd, może to być spowodowane tym, że projekt nie został skompilowany w poprzedniej sekcji. Jeśli tak, spróbuj skompilować projekt, a następnie ponownie Dodaj element szkieletowy.

Po zakończeniu procesu generowania kodu zobaczysz nowy kontroler i widoki w **kontrolerach** i **widokach** projektu, > foldery **uczniów** .

Wykonaj ponownie te same kroki, ale Dodaj szkielet dla klasy **rejestracji** . Po zakończeniu masz plik **EnrollmentsController.cs** i folder w obszarze **widoki** o nazwie **rejestracje** za pomocą widoków Utwórz, Usuń, szczegóły, Edytuj i Indeksuj.

## <a name="add-links-to-new-views"></a>Dodawanie linków do nowych widoków

Aby ułatwić przechodzenie do nowych widoków, można dodać kilka hiperlinków do widoków indeksów dla studentów i rejestracji. Otwórz plik w **widokach** > **Home** > *index. cshtml*, który jest stroną główną Twojej witryny. Dodaj następujący kod poniżej Jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Dla metody ActionLink pierwszy parametr jest tekstem, który ma być wyświetlany w łączu. Drugi parametr jest akcją, a trzeci parametr jest nazwą kontrolera. Na przykład pierwszy link wskazuje akcję indeks w StudentsController. Rzeczywiste hiperłącze jest zbudowane z tych wartości. Pierwszy link ostatecznie przybierze użytkownikom plik **index. cshtml** w folderze **widoki/uczniowie** .

## <a name="display-student-views"></a>Wyświetl widoki uczniów

Upewnij się, że kod dodany do projektu prawidłowo wyświetla listę studentów i umożliwia użytkownikom edytowanie, tworzenie i usuwanie rekordów uczniów w bazie danych.

Kliknij prawym przyciskiem myszy **widoki** > **Home** > *index. cshtml* plik, a następnie wybierz pozycję **Wyświetl w przeglądarce**. Na stronie głównej aplikacji wybierz pozycję **Lista uczniów**.

![](generating-views/_static/image6.png)

Na stronie **indeks** należy zwrócić uwagę na listę studentów i linki do zmodyfikowania tych danych. Wybierz pozycję **Utwórz nowe** łącze i podaj wartości dla nowego ucznia. Kliknij przycisk **Utwórz**i zwróć uwagę na to, że nowy student zostanie dodany do listy.

Wróć na stronę **indeks** , wybierz link **Edytuj** i Zmień niektóre wartości dla ucznia. Kliknij przycisk **Zapisz**i zwróć uwagę, że rekord ucznia został zmieniony.

Na koniec wybierz łącze **Usuń** i Potwierdź, że chcesz usunąć rekord, klikając przycisk **Usuń** .

Bez pisania kodu, dodano widoki, które wykonują Typowe operacje na danych w tabeli uczniów.

Być może zauważono, że etykieta tekstowa dla pola jest oparta na właściwości bazy danych (takiej jak **LastName**), która nie musi być wyświetlana na stronie sieci Web. Na przykład możesz preferować etykietę jako **nazwisko**. Ten problem z wyświetlaniem zostanie naprawiony w dalszej części tego samouczka.

## <a name="display-enrollment-views"></a>Wyświetl widoki rejestracji

Baza danych zawiera relację jeden do wielu między tabelami uczniów i rejestracji oraz relacją jeden-do-wielu między tabelami kurs i rejestracja. Widoki na potrzeby rejestracji poprawnie obsługują te relacje. Przejdź do strony głównej witryny i wybierz łącze **Lista rejestracji** , a następnie **Utwórz nowy** link.

Widok wyświetla formularz służący do tworzenia nowego rekordu rejestracji. W szczególności należy zauważyć, że formularz zawiera listę rozwijaną **CourseID** i listę rozwijaną **StudentID** . Oba są wypełniane wartościami z powiązanych tabel.

Ponadto sprawdzanie poprawności podanych wartości jest automatycznie stosowane na podstawie typu danych pola. **Klasa** wymaga liczby, więc zostanie wyświetlony komunikat o błędzie, jeśli spróbujesz podać niezgodną wartość: *Klasa pola musi być liczbą.*

Sprawdzono, że automatycznie generowane widoki umożliwiają użytkownikom współpracują z danymi w bazie danych. W następnym samouczku w tej serii będziesz aktualizować bazę danych i wprowadzać odpowiednie zmiany w aplikacji sieci Web.

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodano szkielet
> * Dodano linki do nowych widoków
> * Wyświetlone widoki uczniów
> * Wyświetlone widoki rejestracji

Przejdź do następnego samouczka, aby dowiedzieć się, jak zmienić bazę danych.
> [!div class="nextstepaction"]
> [Zmiana bazy danych](changing-the-database.md)
---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Samouczek: ulepszanie sprawdzania poprawności danych dla EF Database First z aplikacją ASP.NET MVC'
description: Ten samouczek koncentruje się na dodawaniu adnotacji do danych do modelu danych, aby określić wymagania dotyczące weryfikacji i formatowanie wyświetlania.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616278"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Samouczek: ulepszanie sprawdzania poprawności danych dla EF Database First z aplikacją ASP.NET MVC

Korzystając z szkieletów MVC, Entity Framework i ASP.NET, można utworzyć aplikację sieci Web, która udostępnia interfejs istniejącej bazy danych. W tej serii samouczków pokazano, jak automatycznie generować kod, który umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i usuwanie danych znajdujących się w tabeli bazy danych. Wygenerowany kod odpowiada kolumnom w tabeli bazy danych.

Ten samouczek koncentruje się na dodawaniu adnotacji do danych do modelu danych, aby określić wymagania dotyczące weryfikacji i formatowanie wyświetlania. Ulepszono je na podstawie opinii użytkowników w sekcji komentarzy.

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodawanie adnotacji do danych
> * Dodaj klasy metadanych

## <a name="prerequisites"></a>Wymagania wstępne

* [Dostosowywanie widoku](customizing-a-view.md)

## <a name="add-data-annotations"></a>Dodawanie adnotacji do danych

Zgodnie z wcześniejszym tematem niektóre reguły sprawdzania poprawności danych są automatycznie stosowane do danych wejściowych użytkownika. Na przykład można podać tylko liczbę dla właściwości klasy. Aby określić więcej reguł walidacji danych, można dodać adnotacje do danych klasy modelu. Adnotacje są stosowane w całej aplikacji sieci Web dla określonej właściwości. Można również zastosować atrybuty formatowania, które zmieniają sposób wyświetlania właściwości. takie jak, zmiana wartości używanej w etykietach tekstowych.

W tym samouczku dodasz adnotacje danych w celu ograniczenia długości wartości podanych dla właściwości FirstName, LastName i MiddleName. W bazie danych te wartości są ograniczone do 50 znaków; Niemniej jednak w aplikacji sieci Web limit znaków nie jest wymuszany. Jeśli użytkownik poda więcej niż 50 znaków dla jednej z tych wartości, strona ulegnie awarii podczas próby zapisania wartości w bazie danych. Należy również ograniczyć zakres do wartości z zakresu od 0 do 4.

Wybierz pozycję **modele** > **ContosoModel. edmx** > **ContosoModel.tt** i Otwórz plik *student.cs* . Dodaj następujący wyróżniony kod do klasy.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Otwórz *Enrollment.cs* i Dodaj następujący wyróżniony kod.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Skompiluj rozwiązanie.

Kliknij pozycję **Lista uczniów** i wybierz pozycję **Edytuj**. Jeśli spróbujesz wprowadzić więcej niż 50 znaków, zostanie wyświetlony komunikat o błędzie.

![Pokaż komunikat o błędzie](enhancing-data-validation/_static/image1.png)

Wróć do strony głównej. Kliknij pozycję **Lista rejestracji** i wybierz pozycję **Edytuj**. Próba dostarczenia klasy powyżej 4. Zostanie wyświetlony następujący błąd: *Klasa pola musi zawierać się w przedziale od 0 do 4.*

## <a name="add-metadata-classes"></a>Dodaj klasy metadanych

Dodawanie atrybutów walidacji bezpośrednio do klasy modelu działa, gdy nie oczekuje się, że baza danych nie zostanie zmieniona; Jeśli jednak baza danych ulegnie zmianie i trzeba będzie ponownie wygenerować klasę modelu, utracisz wszystkie atrybuty zastosowane do klasy modelu. Takie podejście może być bardzo wydajne i podatne na utratę ważnych reguł sprawdzania poprawności.

Aby uniknąć tego problemu, można dodać klasę metadanych, która zawiera atrybuty. Po skojarzeniu klasy modelu z klasą metadanych te atrybuty są stosowane do modelu. W tym podejściu klasy modelu można ponownie wygenerować bez utraty wszystkich atrybutów, które zostały zastosowane do klasy metadanych.

W folderze **modele** Dodaj klasę o nazwie *Metadata.cs*.

Zastąp kod w *Metadata.cs* następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Te klasy metadanych zawierają wszystkie atrybuty walidacji, które zostały wcześniej zastosowane do klas modelu. Atrybut **Display** służy do zmiany wartości używanej w etykietach tekstowych.

Teraz należy skojarzyć klasy modelu z klasami metadanych.

W folderze **modele** Dodaj klasę o nazwie *PartialClasses.cs*.

Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Zauważ, że każda klasa jest oznaczona jako Klasa `partial`, a każda z nich jest zgodna z nazwą i przestrzenią nazw jako klasą, która jest generowana automatycznie. Stosując atrybut metadanych do klasy częściowej, upewnij się, że atrybuty walidacji danych zostaną zastosowane do klasy generowanej automatycznie. Te atrybuty nie zostaną utracone w przypadku ponownego wygenerowania klas modelu, ponieważ atrybut metadanych jest stosowany w klasach częściowych, które nie są ponownie generowane.

Aby ponownie wygenerować klasy generowane automatycznie, Otwórz plik *ContosoModel. edmx* . Ponownie kliknij prawym przyciskiem myszy powierzchnię projektu i wybierz polecenie **Aktualizuj model z bazy danych**. Mimo że baza danych nie została zmieniona, proces ten spowoduje ponowne wygenerowanie klas. Na karcie **Odśwież** wybierz pozycję **tabele** i **Zakończ**.

Zapisz plik *ContosoModel. edmx* , aby zastosować zmiany.

Otwórz plik *student.cs* lub plik *Enrollment.cs* i Zauważ, że zastosowane wcześniej atrybuty sprawdzania poprawności danych nie są już w pliku. Jednak Uruchom aplikację i zwróć uwagę, że reguły walidacji są nadal stosowane podczas wprowadzania danych.

## <a name="conclusion"></a>Podsumowanie

W tej serii przedstawiono prosty przykład generowania kodu z istniejącej bazy danych, który umożliwia użytkownikom edytowanie, aktualizowanie, tworzenie i usuwanie danych. Do tworzenia projektu używane są ASP.NET MVC 5, Entity Framework i ASP.NET. 

Przykładowy przykład tworzenia Code First można znaleźć [w temacie Wprowadzenie with ASP.NET MVC 5](../introduction/getting-started.md). 

Aby zapoznać się z bardziej zaawansowanym przykładem, zobacz [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Należy zauważyć, że interfejs API DbContext używany do pracy z danymi w Database First jest taki sam jak interfejs API służący do pracy z danymi w Code First. Nawet jeśli zamierzasz używać Database First, możesz dowiedzieć się, jak obsługiwać bardziej złożone scenariusze, takie jak odczytywanie i aktualizowanie powiązanych danych, obsługa konfliktów współbieżności i tak dalej z samouczka Code First. Jedyną różnicą jest to, jak tworzona jest baza danych, Klasa kontekstowa i klasy jednostek.

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać pełną listę adnotacji dotyczących walidacji danych, które można zastosować do właściwości i klas, zobacz [System. ComponentModel. DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Dodano adnotacje danych
> * Dodano klasy metadanych

Aby dowiedzieć się, jak wdrożyć aplikację internetową i bazę danych SQL na Azure App Service, zobacz ten samouczek:
> [!div class="nextstepaction"]
> [Wdrażanie aplikacji .NET do Azure App Service](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)

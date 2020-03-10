---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 5 | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework. Przykładowa aplikacja to...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 75328e67abb4295b619cac5423a9eb970942fff7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528001"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 5

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data-continued"></a>Praca z powiązanymi danymi, kontynuowanie

W poprzednim samouczku rozpoczęto korzystanie z kontrolki `EntityDataSource` do pracy z powiązanymi danymi. Wyświetlono wiele poziomów hierarchii i edytowane dane we właściwościach nawigacji. W tym samouczku będziesz nadal korzystać z pokrewnych danych przez dodawanie i usuwanie relacji oraz Dodawanie nowej jednostki, która ma relację z istniejącą jednostką.

Utworzysz stronę, która dodaje kursy, które są przypisane do działów. Działy już istnieją, a podczas tworzenia nowego kursu, w tym samym czasie, należy ustanowić relację między działem IT a istniejącym działem.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Utworzysz również stronę, która współpracuje z relacją wiele-do-wielu przez przypisanie instruktora do kursu (dodanie relacji między dwoma wybranymi jednostkami) lub usunięcie instruktora z kursu (usunięcie relacji między dwiema jednostkami Wybierz pozycję). W bazie danych Dodawanie relacji między instruktorem a kursem powoduje dodanie nowego wiersza do tabeli skojarzeń `CourseInstructor`; usunięcie relacji polega na usunięciu wiersza z tabeli skojarzeń `CourseInstructor`. Należy jednak wykonać tę czynność w Entity Framework przez ustawienie właściwości nawigacji bez jawnego odwoływania się do tabeli `CourseInstructor`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Dodawanie jednostki z relacją do istniejącej jednostki

Utwórz nową stronę sieci Web o nazwie *CoursesAdd. aspx* , która korzysta ze strony wzorcowej *site. Master* , i Dodaj następujące znaczniki do formantu `Content` o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Ten znacznik tworzy formant `EntityDataSource`, który wybiera kursy, które umożliwiają wstawianie i określa procedurę obsługi dla zdarzenia `Inserting`. Użyjesz programu obsługi, aby zaktualizować właściwość nawigacji `Department` po utworzeniu nowej jednostki `Course`.

Znacznik tworzy również formant `DetailsView`, który służy do dodawania nowych jednostek `Course`. Znacznik używa pól powiązanych do `Course` właściwości jednostki. Musisz wprowadzić wartość `CourseID`, ponieważ nie jest to pole identyfikatora generowane przez system. Zamiast tego jest to numer kursu, który należy określić ręcznie podczas tworzenia kursu.

Użyj pola szablon dla właściwości nawigacji `Department`, ponieważ właściwości nawigacji nie mogą być używane z kontrolkami `BoundField`. Pole szablon zawiera listę rozwijaną, aby wybrać dział. Lista rozwijana jest powiązana z jednostką `Departments`ową ustawioną przy użyciu `Eval` zamiast `Bind`, ponieważ nie można bezpośrednio powiązać właściwości nawigacji w celu ich aktualizacji. Należy określić procedurę obsługi dla zdarzenia `Init` formantu `DropDownList`, aby można było przechowywać odwołanie do formantu do użycia przez kod, który aktualizuje `DepartmentID` klucz obcy.

W *CoursesAdd.aspx.cs* tuż po deklaracji częściowej klasy Dodaj pole klasy, aby pomieścić odwołanie do kontrolki `DepartmentsDropDownList`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Dodaj procedurę obsługi dla zdarzenia `Init` kontrolki `DepartmentsDropDownList`, aby można było przechowywać odwołanie do kontrolki. Pozwala to na uzyskanie wartości wprowadzonej przez użytkownika i użycie jej do zaktualizowania wartości `DepartmentID` jednostki `Course`.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Dodaj program obsługi dla zdarzenia `Inserting` kontrolki `DetailsView`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Gdy użytkownik kliknie `Insert`, zdarzenie `Inserting` zostanie wywołane przed wstawieniem nowego rekordu. Kod w programie obsługi pobiera `DepartmentID` z kontrolki `DropDownList` i używa jej do ustawiania wartości, która będzie używana dla właściwości `DepartmentID` jednostki `Course`.

Entity Framework zajmie się dodaniem tego kursu do właściwości nawigacji `Courses` skojarzonej jednostki `Department`. Dodaje również dział do właściwości nawigacji `Department` jednostki `Course`.

Uruchom stronę.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Wprowadź identyfikator, tytuł, liczbę kredytów i wybierz dział, a następnie kliknij przycisk **Wstaw**.

Uruchom stronę *kursy. aspx* , a następnie wybierz ten sam dział, aby wyświetlić nowy kurs.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Praca z relacjami wiele-do-wielu

Relacja między zestawem jednostek `Courses` i zestawem jednostek `People` jest relacją "wiele do wielu". Jednostka `Course` ma właściwość nawigacji o nazwie `People`, która może zawierać zero, jedną lub więcej powiązanych jednostek `Person` (reprezentujących instruktorów przypisanych do uczenia tego kursu). A jednostka `Person` ma właściwość nawigacji o nazwie `Courses`, która może zawierać zero, jedną lub więcej powiązanych jednostek `Course` (reprezentującą kursy, do których jest przypisany instruktor). Jeden instruktor może uczyć się wielu kursów, a jeden kurs może być nauczany przez wielu instruktorów. W tej części przewodnika dodasz i usuniesz relacje między `Person` i `Course` jednostek przez zaktualizowanie właściwości nawigacji powiązanych jednostek.

Utwórz nową stronę sieci Web o nazwie *InstructorsCourses. aspx* , która korzysta ze strony wzorcowej *site. Master* , i Dodaj następujące znaczniki do formantu `Content` o nazwie `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Ten znacznik tworzy formant `EntityDataSource`, który pobiera nazwę i `PersonID` jednostek `Person` dla instruktorów. Formant `DropDrownList` jest powiązany z kontrolką `EntityDataSource`. Formant `DropDownList` określa procedurę obsługi dla zdarzenia `DataBound`. Ta procedura obsługi służy do powiązania dwóch list rozwijanych, które wyświetlają kursy.

Znacznik tworzy również następujące grupy kontrolek, które służą do przypisywania kursu do wybranego instruktora:

- Kontrolka `DropDownList` do wybierania kursu do przypisania. Ten formant zostanie wypełniony kursami, które nie są obecnie przypisane do wybranego instruktora.
- Formant `Button`, aby zainicjować przypisanie.
- Formant `Label`, aby wyświetlić komunikat o błędzie, jeśli przypisanie nie powiedzie się.

Na koniec znacznik tworzy również grupę formantów do użycia w celu usunięcia kursu z wybranego instruktora.

W *InstructorsCourses.aspx.cs*Dodaj instrukcję using:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Dodaj metodę zapełniania dwóch list rozwijanych, które wyświetlają kursy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Ten kod pobiera wszystkie kursy z zestawu jednostek `Courses` i pobiera kursy z właściwości nawigacji `Courses` jednostki `Person` dla wybranego instruktora. Następnie decyduje o tym, które kursy są przypisane do tego instruktora i odpowiednio wypełnia listę rozwijaną.

Dodaj program obsługi dla zdarzenia `Click` `Assign` przycisku:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Ten kod pobiera jednostkę `Person` dla wybranego instruktora, pobiera jednostkę `Course` dla wybranego kursu i dodaje wybrany kurs do właściwości nawigacji `Courses` jednostki `Person` instruktora. Następnie zapisuje zmiany w bazie danych i ponownie wypełnia listy rozwijane, dzięki czemu wyniki mogą być widoczne natychmiast.

Dodaj program obsługi dla zdarzenia `Click` `Remove` przycisku:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Ten kod pobiera jednostkę `Person` dla wybranego instruktora, pobiera jednostkę `Course` dla wybranego kursu i usuwa wybrany kurs z `Courses` właściwości nawigacji jednostki `Person`. Następnie zapisuje zmiany w bazie danych i ponownie wypełnia listy rozwijane, dzięki czemu wyniki mogą być widoczne natychmiast.

Dodaj kod do metody `Page_Load`, która sprawdza, czy komunikaty o błędach nie są widoczne, gdy nie ma błędów do raportowania i Dodaj programy obsługi dla zdarzeń `DataBound` i `SelectedIndexChanged` na liście rozwijanej instruktorzy w celu wypełnienia list rozwijanych kursy:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Uruchom stronę.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Wybierz instruktora. Lista rozwijana <strong>Przypisz kurs</strong> zawiera kursy, których instruktor nie uczy się, a lista rozwijana <strong>Usuń kurs</strong> zawiera kursy, do których ten instruktor jest już przypisany. W sekcji <strong>przypisywanie kursu</strong> Wybierz kurs, a następnie kliknij przycisk <strong>Przypisz</strong>. Kurs zostanie przeniesiony na listę rozwijaną <strong>Usuń kurs</strong> . Wybierz kurs w sekcji <strong>usuwanie kursu</strong> i kliknij przycisk <strong>Usuń</strong><em>.</em> Kurs zostanie przeniesiony na listę rozwijaną <strong>Przypisz kurs</strong> .

Zaobserwowano już kilka sposobów pracy z powiązanymi danymi. W poniższym samouczku dowiesz się, jak używać dziedziczenia w modelu danych, aby zwiększyć łatwość utrzymania aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [dalej](the-entity-framework-and-aspnet-getting-started-part-6.md)

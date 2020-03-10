---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 8 | Microsoft Docs
author: tdykstra
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework. Przykładowa aplikacja to...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: ff6a808dd501df8056c0fb0784a8e42e094d5db7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585912"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Wprowadzenie z Entity Framework 4,0 Database First i ASP.NET 4 Web Forms — część 8

Autor [Dykstra](https://github.com/tdykstra)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET Web Forms przy użyciu Entity Framework 4,0 i programu Visual Studio 2010. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Używanie funkcji danych dynamicznych do formatowania i walidacji danych

W poprzednim samouczku zaimplementowano procedury składowane. W tym samouczku przedstawiono sposób, w jaki funkcje danych dynamicznych mogą zapewnić następujące korzyści:

- Pola są automatycznie formatowane pod kątem wyświetlania na podstawie ich typu danych.
- Pola są automatycznie sprawdzane na podstawie ich typu danych.
- Możesz dodać metadane do modelu danych, aby dostosować formatowanie i zachowanie walidacji. Gdy to zrobisz, możesz dodać reguły formatowania i walidacji tylko w jednym miejscu i są one automatycznie stosowane wszędzie tam, gdzie uzyskujesz dostęp do pól przy użyciu dynamicznych kontrolek danych.

Aby zobaczyć, jak to działa, zmienisz kontrolki służące do wyświetlania i edytowania pól na istniejącej stronie *uczniów. aspx* , a następnie dodasz metadane dotyczące formatowania i walidacji do pól Nazwa i Data typu jednostki `Student`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Korzystanie z kontrolek DynamicField i DynamicControl

Otwórz stronę *Students. aspx* i w kontrolce `StudentsGridView` Zamień **nazwę** i **datę rejestracji** `TemplateField` elementy na następujące znaczniki:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

W tym znaczniku są stosowane kontrolki `DynamicControl` zamiast formantów `TextBox` i `Label` w polu szablonu nazwa ucznia, a w przypadku daty rejestracji jest stosowana kontrolka `DynamicField`. Nie określono ciągów formatu.

Dodaj kontrolkę `ValidationSummary` po kontrolce `StudentsGridView`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

W kontrolce `SearchGridView` Zastąp znaczniki dla kolumny **Nazwa** i **Data rejestracji** , jak w kontrolce `StudentsGridView`, z wyjątkiem pominięcie elementu `EditItemTemplate`. Element `Columns` kontrolki `SearchGridView` zawiera teraz następujące znaczniki:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Otwórz *Students.aspx.cs* i Dodaj następującą instrukcję `using`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Dodaj procedurę obsługi dla zdarzenia `Init` strony:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Ten kod określa, że dane dynamiczne będą udostępniać formatowanie i sprawdzanie poprawności w tych kontrolkach powiązanych z danymi dla pól jednostki `Student`. Jeśli zostanie wyświetlony komunikat o błędzie podobny do poniższego przykładu podczas uruchamiania strony, zazwyczaj oznacza to, że zapomniano wywołać metodę `EnableDynamicData` w `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Uruchom stronę.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

W kolumnie **Data rejestracji** zostanie wyświetlony czas wraz z datą, ponieważ typ właściwości to `DateTime`. Naprawisz to później.

Na razie należy zauważyć, że dane dynamiczne automatycznie zapewniają podstawowe sprawdzanie poprawności danych. Na przykład kliknij pozycję **Edytuj**, wyczyść pole Date, kliknij przycisk **Aktualizuj**i zobaczysz, że dane dynamiczne automatycznie czynią to pole wymagane, ponieważ wartość nie dopuszcza wartości null w modelu danych. Na stronie zostanie wyświetlona gwiazdka po polu i komunikat o błędzie w kontrolce `ValidationSummary`:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Możesz pominąć formant `ValidationSummary`, ponieważ można także trzymać wskaźnik myszy nad gwiazdką, aby wyświetlić komunikat o błędzie:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dane dynamiczne będą również sprawdzać poprawność danych wprowadzonych w polu **Data rejestracji** :

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Jak widać, jest to ogólny komunikat o błędzie. W następnej sekcji zobaczysz, jak dostosować komunikaty, a także reguły sprawdzania poprawności i formatowania.

## <a name="adding-metadata-to-the-data-model"></a>Dodawanie metadanych do modelu danych

Zazwyczaj chcesz dostosować funkcje udostępniane przez dane dynamiczne. Można na przykład zmienić sposób wyświetlania danych i zawartość komunikatów o błędach. Zwykle dostosowuje się również reguły sprawdzania poprawności danych w celu zapewnienia większej funkcjonalności, niż dane dynamiczne są automatycznie oparte na typach danych. W tym celu należy utworzyć klasy częściowe, które odpowiadają typom jednostek.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **ContosoUniversity** , wybierz polecenie **Dodaj odwołanie**i Dodaj odwołanie do `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

W folderze *dal* Utwórz nowy plik klasy, nadaj mu nazwę *student.cs*i Zastąp kod szablonu w następującym kodzie.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Ten kod tworzy klasę częściową dla jednostki `Student`. Atrybut `MetadataType` stosowany do tej klasy częściowej identyfikuje klasę, która jest używana do określania metadanych. Klasa metadanych może mieć dowolną nazwę, ale przy użyciu nazwy jednostki i "Metadata" jest powszechną procedurą.

Atrybuty stosowane do właściwości w klasie metadanych określają formatowanie, walidację, reguły i komunikaty o błędach. Podane tutaj atrybuty będą miały następujące wyniki:

- `EnrollmentDate` będzie wyświetlana jako Data (bez godziny).
- Pola nazw nie mogą zawierać więcej niż 25 znaków i został podany niestandardowy komunikat o błędzie.
- Wymagane są oba pola nazw, a podano niestandardowy komunikat o błędzie.

Ponownie uruchom stronę *uczniów. aspx* i zobaczysz, że daty są teraz wyświetlane bez godzin:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Edytuj wiersz i spróbuj wyczyścić wartości w polach Nazwa. Gwiazdki wskazujące błędy pola są wyświetlane zaraz po opuszczeniu pola, przed kliknięciem przycisku **Aktualizuj**. Po kliknięciu przycisku **Aktualizuj**na stronie zostanie wyświetlony tekst komunikatu o błędzie określony przez użytkownika.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Spróbuj wprowadzić nazwy dłuższe niż 25 znaków, kliknij przycisk **Aktualizuj**, a na stronie zostanie wyświetlony tekst komunikatu o błędzie określony przez użytkownika.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Teraz, po skonfigurowaniu tych reguł formatowania i walidacji w metadanych modelu danych, reguły zostaną automatycznie zastosowane na każdej stronie, która wyświetla lub dopuszcza zmiany w tych polach, tak długo, jak w przypadku formantów `DynamicControl` lub `DynamicField`. Zmniejsza to ilość nadmiarowego kodu, który trzeba napisać, co ułatwia programowanie i testowanie oraz zapewnia spójność formatowania i walidacji danych w całej aplikacji.

## <a name="more-information"></a>Więcej informacji

Ten zbiór samouczków znajduje się na Wprowadzenie z Entity Framework. Aby uzyskać więcej zasobów ułatwiających nauczenie się, jak korzystać z Entity Framework, przejdź do [pierwszego samouczka w następnej Entity Framework serii samouczków](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) lub odwiedź następujące witryny:

- [Entity Framework często zadawane pytania](http://www.ef-faq.org/introduction.html)
- [Blog zespołu Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework w bibliotece MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework w centrum deweloperów danych MSDN](https://msdn.microsoft.com/data/ef.aspx)
- [Omówienie kontrolki serwera sieci Web dla obiektu EntityDataSource w bibliotece MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Dokumentacja interfejsu API formantu EntityDataSource w bibliotece MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework forów w witrynie MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Wstecz](the-entity-framework-and-aspnet-getting-started-part-7.md)

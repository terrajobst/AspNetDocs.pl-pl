---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 3 | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i menu podręcznego interfejsu użytkownika jQuery DatePicker w ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457898"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 3

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i okienka podręcznego interfejsu użytkownika jQuery DatePicker w aplikacji sieci Web ASP.NET MVC.

## <a name="working-with-complex-types"></a>Praca z typami złożonymi

W tej sekcji utworzysz klasę adresów i dowiesz się, jak utworzyć szablon w celu wyświetlenia go.

W folderze *modele* Utwórz nowy plik klasy o nazwie *Person.cs* , gdzie umieścisz dwa typy: klasy `Person` i klasy `Address`. Klasa `Person` będzie zawierać właściwość, która jest wpisana jako `Address`. Typ `Address` jest typu złożonego, co oznacza, że nie jest to jeden z wbudowanych typów, takich jak `int`, `string`lub `double`. Zamiast tego ma kilka właściwości. Kod dla nowych klas wygląda następująco:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

Na kontrolerze `Movie` Dodaj następującą akcję `PersonDetail`, aby wyświetlić wystąpienie osoby:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Następnie Dodaj następujący kod do kontrolera `Movie`, aby wypełnić model `Person` niektórymi przykładowymi danymi:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Otwórz plik *Views\Movies\PersonDetail.cshtml* i Dodaj następujący znacznik dla widoku `PersonDetail`.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, a następnie przejdź do *filmów/PersonDetail*.

Widok `PersonDetail` nie zawiera `Address` typu złożonego, co można zobaczyć na tym zrzucie ekranu. (Żaden adres nie jest wyświetlany).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

Dane modelu `Address` nie są wyświetlane, ponieważ jest typem złożonym. Aby wyświetlić informacje o adresie, ponownie otwórz plik *Views\Movies\PersonDetail.cshtml* i Dodaj następujący znacznik.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

Kompletny znacznik dla `PersonDetail` teraz będzie wyglądać następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Uruchom aplikację ponownie i Wyświetl widok `PersonDetail`. Informacje o adresie są teraz wyświetlane:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Tworzenie szablonu dla typu złożonego

W tej sekcji utworzysz szablon, który będzie używany do renderowania `Address` typu złożonego. Podczas tworzenia szablonu dla typu `Address`, ASP.NET MVC może automatycznie używać go do formatowania modelu adresów w dowolnym miejscu w aplikacji. Dzięki temu można kontrolować renderowanie typu `Address` z tylko jednego miejsca w aplikacji.

W folderze *Views\Shared\DisplayTemplates* Utwórz silnie nazwany widok częściowy o nazwie **Address**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Kliknij przycisk **Dodaj**, a następnie otwórz nowy plik *Views\Shared\DisplayTemplates\Address.cshtml* . Nowy widok zawiera następujące wygenerowane znaczniki:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Uruchom aplikację i Wyświetl widok `PersonDetail`. Tym razem szablon `Address`, który właśnie został utworzony, jest używany do wyświetlania `Address` typu złożonego, tak aby ekran wyglądał następująco:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Podsumowanie: sposoby określania formatu i szablonu wyświetlania modelu

Widzisz, że możesz określić format lub szablon właściwości modelu, wykonując następujące podejścia:

- Stosowanie atrybutu `DisplayFormat` do właściwości w modelu. Na przykład poniższy kod powoduje wyświetlenie daty bez czasu:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Stosowanie atrybutu [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) do właściwości w modelu i Określanie typu danych. Na przykład poniższy kod powoduje wyświetlenie daty bez czasu.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Jeśli aplikacja zawiera szablon *Date. cshtml* w folderze *Views\Shared\DisplayTemplates* lub folderze *Views\Movies\DisplayTemplates* , ten szablon zostanie użyty do renderowania właściwości `DateTime`. W przeciwnym razie wbudowany system ASP.NET tworzenia szablonów wyświetla właściwość jako datę.
- Tworzenie szablonu wyświetlania w folderze *Views\Shared\DisplayTemplates* lub folderze *Views\Movies\DisplayTemplates* , którego nazwa pasuje do typu danych, który chcesz sformatować. Załóżmy na przykład, że *Views\Shared\DisplayTemplates\DateTime.cshtml* był używany do renderowania właściwości `DateTime` w modelu, bez dodawania atrybutu do modelu i bez dodawania znaczników do widoków.
- Przy użyciu atrybutu [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) w modelu, aby określić szablon do wyświetlania właściwości model.
- Jawne dodanie nazwy szablonu wyświetlania do wywołania [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) w widoku.

Używane podejście zależy od tego, co należy zrobić w aplikacji. Te podejścia nie są rzadko spotykane, aby uzyskać dokładnie wymagany rodzaj formatowania.

W następnej sekcji nastąpi przełączenie na bit i przejście przez dostosowanie sposobu wyświetlania danych w celu dostosowania sposobu wprowadzania. Przełączymy się z DatePicker jQuery do widoków edycji w aplikacji, aby zapewnić sprawny sposób określania dat.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)

---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Korzystanie z okienka podręcznego DatePicker interfejsu użytkownika HTML5 i jQuery z ASP.NET MVC — część 2 | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i menu podręcznego interfejsu użytkownika jQuery DatePicker w ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 325cc90eb6e717c47863eda6253e0d48d796386b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455896"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 2

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i okienka podręcznego interfejsu użytkownika jQuery DatePicker w aplikacji sieci Web ASP.NET MVC.

## <a name="adding-an-automatic-datetime-template"></a>Dodawanie automatycznego szablonu DateTime

W pierwszej części tego samouczka dowiesz się, jak dodać atrybuty do modelu, aby jawnie określić formatowanie oraz jak można jawnie określić szablon, który jest używany do renderowania modelu. Na przykład atrybut [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) w poniższym kodzie jawnie określa formatowanie właściwości `ReleaseDate`.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

W poniższym przykładzie atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , przy użyciu wyliczenia `Date`, określa, że szablon daty powinien być używany do renderowania modelu. Jeśli w projekcie nie ma szablonu daty, używany jest wbudowany szablon daty.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Jednak ASP. MVC można wykonać dopasowanie typu przy użyciu konwencji-over-Configuration, szukając szablonu zgodnego z nazwą typu. Dzięki temu można utworzyć szablon, który automatycznie formatuje dane bez używania jakichkolwiek atrybutów lub kodu. W tej części samouczka utworzysz szablon, który jest automatycznie stosowany do właściwości modelu typu [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Nie trzeba używać atrybutu ani innej konfiguracji, aby określić, że szablon ma być używany do renderowania wszystkich właściwości modelu typu [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Dowiesz się również, jak dostosować wyświetlanie poszczególnych właściwości, a nawet pojedynczych pól.

Aby rozpocząć, usuńmy istniejące informacje o formatowaniu i wyświetlaj pełne daty w aplikacji.

Otwórz plik *Movie.cs* i skomentuj atrybut `DataType` właściwości `ReleaseDate`:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację.

Zauważ, że właściwość `ReleaseDate` wyświetla teraz zarówno datę, jak i godzinę, ponieważ jest to wartość domyślna, jeśli nie podano informacji o formatowaniu.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Dodawanie stylów CSS do testowania nowych szablonów

Przed utworzeniem szablonu do formatowania dat należy dodać kilka reguł stylów CSS, które można zastosować do nowych szablonów. Dzięki temu można sprawdzić, czy renderowane strony używa nowego szablonu.

Otwórz plik *Content\Site.cs*s i Dodaj następujące reguły CSS w dolnej części pliku:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Dodawanie szablonów wyświetlania DateTime

Teraz można utworzyć nowy szablon. W folderze *Views\Movies* Utwórz folder *DisplayTemplates* .

W folderze *Views\Shared* Utwórz folder *DisplayTemplates* i folder *EditorTemplates* .

Szablony wyświetlania w folderze *Views\Shared\DisplayTemplates* będą używane przez wszystkie kontrolery. Szablony wyświetlania w folderze *Views\Movie\DisplayTemplates* będą używane tylko przez kontroler `Movie`. (Jeśli szablon o tej samej nazwie jest wyświetlany w obu folderach, szablon w folderze *Views\Movie\DisplayTemplates* — to znaczy, szablon bardziej szczegółowy — ma pierwszeństwo w przypadku widoków zwracanych przez kontroler `Movie`).

W **Eksplorator rozwiązań**rozwiń folder *widoki* , rozwiń folder *udostępniony* , a następnie kliknij prawym przyciskiem myszy folder *Views\Shared\DisplayTemplates* .

Kliknij przycisk **Dodaj** , a następnie kliknij przycisk **Wyświetl**. Zostanie wyświetlone okno dialogowe **Dodawanie widoku** .

W polu **Nazwa widoku** wpisz `DateTime`. (Ta nazwa musi być używana w celu dopasowania do nazwy typu).

Zaznacz pole wyboru **Utwórz jako widok częściowy** . Upewnij się, że pola **Używaj układu lub strony wzorcowej** i **Utwórz widok z silną typem** nie są zaznaczone.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Kliknij pozycję **Add** (Dodaj). W *Views\Shared\DisplayTemplates*jest tworzony szablon *DateTime. cshtml* .

Na poniższej ilustracji przedstawiono folder *widoki* w **Eksplorator rozwiązań** po utworzeniu `DateTime` szablonów wyświetlania i edytora.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Otwórz plik *Views\Shared\DisplayTemplates\DateTime.cshtml* i Dodaj następujący znacznik, który używa metody [String. format](https://msdn.microsoft.com/library/system.string.format.aspx) , aby sformatować właściwość jako datę bez czasu. (Format `{0:d}` określa format daty krótkiej).

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Powtórz ten krok, aby utworzyć szablon `DateTime` w folderze *Views\Movie\DisplayTemplates* . Użyj poniższego kodu w pliku *Views\Movie\DisplayTemplates\DateTime.cshtml* .

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

Klasa `loud-1` CSS powoduje, że data jest wyświetlana pogrubioną czerwoną czcionką. Klasa `loud-1` CSS została dodana jako środek tymczasowy, dzięki czemu można łatwo zobaczyć, kiedy ten konkretny szablon jest używany.

Utworzone przez Ciebie elementy i dostosowane szablony, które będą używane przez ASP.NET do wyświetlania dat. Bardziej ogólny szablon (w folderze *Views\Shared\DisplayTemplates* ) wyświetla prostą datę krótką. Szablon przeznaczony dla kontrolera `Movie` (w folderze *Views\Movies\DisplayTemplates* ) zawiera krótką datę, która jest również formatowana jako czerwony tekst.

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację. Przeglądarka renderuje widok indeksu dla aplikacji.

Właściwość `ReleaseDate` teraz wyświetla datę w pogrubieniu czerwoną czcionką bez czasu. Dzięki temu można potwierdzić, że pomocnik szablonu `DateTime` w folderze *Views\Movies\DisplayTemplates* jest wybierany przez pomocnika `DateTime` szablon w folderze udostępnionym (*Views\Shared\DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Teraz zmień nazwę pliku *Views\Movies\DisplayTemplates\DateTime.cshtml* na *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację.

Tym razem Właściwość `ReleaseDate` wyświetla datę bez czasu i bez pogrubionej czerwonej czcionki. Pokazuje to, że szablon, który ma nazwę typu danych (w tym przypadku `DateTime`) jest automatycznie używany do wyświetlania wszystkich właściwości modelu tego typu. Po zmianie nazwy pliku *DateTime. cshtml* na *LoudDateTime. cshtml*ASP.NET nie znaleziono szablonu w folderze *Views\Movies\DisplayTemplates* , dlatego użyto szablonu *DateTime. cshtml* z folderu * Views\Movies\Shared\*.

(W dopasowaniu do szablonu jest rozróżniana wielkość liter, dlatego można utworzyć nazwę pliku szablonu z dowolną wielkością liter. Na przykład wartości *DateTime. cshtml, DateTime. cshtml*i *DateTime. cshtml* byłyby zgodne z typem `DateTime`.)

Aby przejrzeć: w tym momencie pole `ReleaseDate` jest wyświetlane przy użyciu szablonu *Views\Movies\DisplayTemplates\DateTime.cshtml* , który wyświetla dane przy użyciu formatu daty krótkiej, ale w przeciwnym razie nie dodaje żadnych specjalnych formatów.

### <a name="using-uihint-to-specify-a-display-template"></a>Określanie szablonu wyświetlania przy użyciu UIHint

Jeśli aplikacja sieci Web ma wiele pól `DateTime` i domyślnie chcesz wyświetlić wszystkie lub większość z nich w formacie daty i godziny, szablon *DateTime. cshtml* jest dobrym rozwiązaniem. Ale co zrobić, jeśli masz kilka dat, w których chcesz wyświetlić pełną datę i godzinę? Nie ma sprawy. Można utworzyć dodatkowy szablon i użyć atrybutu [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) , aby określić formatowanie dla pełnej daty i godziny. Następnie można wybiórczo zastosować ten szablon. Można użyć atrybutu [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) na poziomie modelu lub określić szablon w widoku. W tej sekcji zobaczysz, jak używać atrybutu `UIHint`, aby selektywnie zmienić formatowanie niektórych wystąpień pól daty i godziny.

Otwórz plik *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* i Zastąp istniejący kod następującym kodem:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Powoduje to wyświetlenie pełnej daty i godziny i dodanie klasy CSS, która sprawia, że tekst jest zielony i duży.

Otwórz plik *Movie.cs* i Dodaj atrybut [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) do właściwości `ReleaseDate`, jak pokazano w następującym przykładzie:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Oznacza to, że ASP.NET MVC, że po wyświetleniu właściwości `ReleaseDate` (w specjalnym, a nie tylko w przypadku obiektu `DateTime`) powinien użyć szablonu *LoudDateTime. cshtml* .

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację.

Zauważ, że właściwość `ReleaseDate` teraz wyświetla datę i godzinę w dużej zielonej czcionce.

Powróć do atrybutu `UIHint` w pliku *Movie.cs* i Skomentuj go, aby nie był używany szablon *LoudDateTime. cshtml* . Uruchom ponownie aplikację. Data wydania nie jest wyświetlana jako duża i zielona. Sprawdza, czy szablon *Views\Shared\DisplayTemplates\DateTime.cshtml* jest używany w widokach indeksu i szczegółów.

Jak wspomniano wcześniej, można również zastosować szablon w widoku, który umożliwia zastosowanie szablonu do poszczególnych wystąpień pewnych danych. Otwórz widok *Views\Movies\Details.cshtml* . Dodaj `"LoudDateTime"` jako drugi parametr wywołania [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) dla pola `ReleaseDate`. Ukończony kod wygląda następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Określa, że szablon `LoudDateTime` powinien być używany do wyświetlania właściwości modelu niezależnie od tego, jakie atrybuty są stosowane do modelu.

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację.

Sprawdź, czy strona indeks filmu używa szablonu *Views\Shared\DisplayTemplates\DateTime.cshtml* (czerwony znak pogrubiony), a strona *Movie\Details* używa szablonu *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* (duża i zielona).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

W następnej sekcji utworzysz szablon dla typu złożonego.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)

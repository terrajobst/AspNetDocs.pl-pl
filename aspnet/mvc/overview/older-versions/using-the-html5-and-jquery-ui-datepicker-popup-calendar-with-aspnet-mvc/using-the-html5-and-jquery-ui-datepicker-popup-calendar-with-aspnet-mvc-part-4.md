---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Korzystanie z okienka podręcznego DatePicker interfejsu użytkownika HTML5 i jQuery z ASP.NET MVC — część 4 | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i menu podręcznego interfejsu użytkownika jQuery DatePicker w ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 583e782641efea9a9517edb31f7718b28203d756
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538949"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Korzystanie z menu podręcznego interfejsu użytkownika HTML5 i jQuery DatePicker z ASP.NET MVC — część 4

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym samouczku przedstawiono podstawowe informacje dotyczące pracy z szablonami edytora, szablonów wyświetlania i okienka podręcznego interfejsu użytkownika jQuery DatePicker w aplikacji sieci Web ASP.NET MVC.

### <a name="adding-a-template-for-editing-dates"></a>Dodawanie szablonu do edytowania dat

W tej sekcji utworzysz szablon służący do edytowania dat, które zostaną zastosowane, gdy ASP.NET MVC wyświetla interfejs użytkownika do edycji właściwości modelu, które są oznaczone wyliczeniem **daty** atrybutu [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) . Szablon będzie renderować tylko datę; godzina nie zostanie wyświetlona. W szablonie zostanie użyty kalendarz podręczny [DatePicker interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) , aby umożliwić edycję dat.

Aby rozpocząć, Otwórz plik *Movie.cs* i Dodaj atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) z wyliczeniem **daty** do właściwości `ReleaseDate`, jak pokazano w poniższym kodzie:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Ten kod powoduje wyświetlenie pola `ReleaseDate` bez czasu w obu szablonach wyświetlania i Edytowanie szablonów. Jeśli aplikacja zawiera szablon *Date. cshtml* w folderze *Views\Shared\EditorTemplates* lub w folderze *Views\Movies\EditorTemplates* , ten szablon będzie używany do renderowania dowolnej właściwości `DateTime` podczas edytowania. W przeciwnym razie wbudowany system ASP.NET tworzenia szablonów będzie wyświetlał właściwość jako datę.

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację. Wybierz łącze Edytuj, aby sprawdzić, czy w polu wejściowym daty wydania jest wyświetlana tylko Data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

W **Eksplorator rozwiązań**rozwiń folder *widoki* , rozwiń folder *udostępniony* , a następnie kliknij prawym przyciskiem myszy folder *Views\Shared\EditorTemplates* .

Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **Wyświetl**. Zostanie wyświetlone okno dialogowe **Dodawanie widoku** .

W polu **Nazwa widoku** wpisz &quot;Data&quot;.

Zaznacz pole wyboru **Utwórz jako widok częściowy** . Upewnij się, że pola **Używaj układu lub strony wzorcowej** i **Utwórz widok z silną typem** nie są zaznaczone.

Kliknij pozycję **Add** (Dodaj). Utworzono szablon *Views\Shared\EditorTemplates\Date.cshtml* .

Dodaj następujący kod do szablonu *Views\Shared\EditorTemplates\Date.cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

Pierwszy wiersz deklaruje model jako typ `DateTime`. Chociaż nie musisz deklarować typu modelu w szablonach edycji i wyświetlania, najlepszym rozwiązaniem jest to, aby w celu uzyskania sprawdzenia, czy model jest przenoszona do widoku. (Kolejną zaletą jest to, że uzyskasz funkcję IntelliSense dla modelu w widoku w programie Visual Studio). Jeśli typ modelu nie jest zadeklarowany, ASP.NET MVC uważa, że jest typem [dynamicznym](https://msdn.microsoft.com/library/dd264741.aspx) i nie jest sprawdzanie typu w czasie kompilacji. Jeśli model zostanie zadeklarowany jako typ `DateTime`, zostanie jednoznacznie wpisany.

Drugi wiersz jest po prostu literałem HTML, który wyświetla &quot;przy użyciu szablonu daty&quot; przed polem daty. Ten wiersz będzie używany tymczasowo do sprawdzenia, czy ten szablon daty jest używany.

Następnym wierszem jest pomocnik [HTML. TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) , który renderuje `input` pole, które jest polem tekstowym. Trzeci parametr pomocnika używa typu anonimowego, aby ustawić klasę dla pola tekstowego na `datefield` i typ do `date`. (Ponieważ `class` jest zastrzeżona w C#, należy użyć znaku `@` do ucieczki atrybutu `class` w C# analizatorze).

Typ `date` jest typem wejściowym HTML5, który umożliwia przeglądarkom obsługującym język HTML5 renderowanie kontrolki kalendarza HTML5. Później dodasz kod JavaScript, aby podłączyć jQuery DatePicker do elementu `Html.TextBox` przy użyciu klasy `datefield`.

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację. Możesz sprawdzić, czy właściwość `ReleaseDate` w widoku edycji używa szablonu edycji, ponieważ szablon wyświetla &quot;użyciu szablonu daty,&quot; tuż przed polem wprowadzania tekstu `ReleaseDate`, jak pokazano na poniższej ilustracji:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Sprawdź Źródło strony w przeglądarce. (Na przykład kliknij prawym przyciskiem myszy stronę i wybierz polecenie **Wyświetl źródło**). W poniższym przykładzie przedstawiono niektóre znaczniki dla strony, ilustrując atrybuty `class` i `type` w renderowanym kodzie HTML.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Wróć do szablonu *Views\Shared\EditorTemplates\Date.cshtml* i Usuń &quot;przy użyciu szablonu daty&quot; znaczników. Teraz ukończony szablon wygląda następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Dodawanie Datepickerego okienka podręcznego interfejsu użytkownika jQuery przy użyciu narzędzia NuGet

W tej sekcji dodasz kalendarz podręczny [DatePicker interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) do szablonu daty i edycji. Biblioteka [interfejsu użytkownika jQuery](http://jqueryui.com/) zapewnia obsługę animacji, zaawansowanych efektów i dostosowywalnych elementów widget. Jest ona zbudowana na bazie biblioteki jQuery JavaScript. Kalendarz podręczny DatePicker umożliwia łatwe i naturalne wprowadzanie dat przy użyciu kalendarza zamiast wprowadzać ciąg. W kalendarzu podręcznym są również ograniczane użytkowników do dat prawnych — zwykłe wprowadzanie tekstu dla daty umożliwi Ci wprowadzanie takich elementów jak `2/33/1999` (luty 33rd, 1999), ale kalendarz podręczny [DatePicker interfejsu użytkownika jQuery](http://jqueryui.com/demos/datepicker/) nie zezwoli na to.

Najpierw należy zainstalować biblioteki interfejsu użytkownika jQuery. W tym celu należy użyć programu NuGet, który jest menedżerem pakietów dołączonym do wersji SP1 programu Visual Studio 2010 i Visual Web Developer.

W programie Visual Web Developer z menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet** , a następnie wybierz pozycję **Zarządzaj pakietami NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Uwaga: Jeśli menu **Narzędzia** nie wyświetla polecenia **Menedżera pakietów NuGet** , należy zainstalować pakiet NuGet, postępując zgodnie z instrukcjami na stronie Instalowanie narzędzia [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) w witrynie sieci Web NuGet.   
  
Jeśli używasz programu Visual Studio zamiast programu Visual Web Developer, w menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet** , a następnie wybierz pozycję **Dodaj odwołanie do pakietu biblioteki**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

W oknie dialogowym **MVCMovie — zarządzanie pakietami NuGet** kliknij kartę **online** po lewej stronie, a następnie wprowadź &quot;jQuery. UI&quot; w polu wyszukiwania. Wybierz pozycję j **zapytania element widgets UI: DatePicker**, a następnie wybierz przycisk **Instaluj** .

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

Pakiet NuGet dodaje te wersje debugowania oraz zminimalizowanego wersje interfejsu użytkownika systemu jQuery i selektor daty interfejsu użytkownika jQuery do projektu:

- *jQuery. UI. Core. js*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. js*
- *jQuery. UI. DatePicker. min. js*

Uwaga: wersje debugowania (pliki bez rozszerzenia *. min. js* ) są przydatne do debugowania, ale w witrynie produkcyjnej dołączane są tylko wersje zminimalizowanego.

Aby faktycznie używać selektora dat jQuery, należy utworzyć skrypt jQuery, który będzie podłączać widżet Calendar do szablonu edycji. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *skrypty* i wybierz polecenie **Dodaj**, a następnie **nowy element**, a następnie **plik JScript**. Nazwij plik *DatePickerReady. js*.

Dodaj następujący kod do pliku *DatePickerReady. js* :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Jeśli nie znasz już platformy jQuery, poniżej przedstawiono krótkie wyjaśnienie tego, co robi: pierwszy wiersz to funkcja&quot; &quot;platformy jQuery, która jest wywoływana, gdy wszystkie elementy DOM na stronie zostały załadowane. Drugi wiersz wybiera wszystkie elementy DOM, które mają nazwę klasy `datefield`, a następnie wywołuje funkcję `datepicker` dla każdego z nich. (Należy pamiętać, że Klasa `datefield` została dodana do szablonu *Views\Shared\EditorTemplates\Date.cshtml* wcześniej w samouczku).

Następnie otwórz plik *Views\Shared\\_Layout. cshtml* . Należy dodać odwołania do następujących plików, które są wymagane, aby można było użyć selektora dat:

- *Content/motywy/Base/jQuery. UI. Core. CSS*
- *Content/motywy/Base/jQuery. UI. DatePicker. CSS*
- *Zawartość/motywy/Base/jQuery. UI. Theme. CSS*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. min. js*
- *DatePickerReady. js*

W poniższym przykładzie przedstawiono rzeczywisty kod, który należy dodać u dołu elementu `head` w pliku *Views\Shared\\_Layout. cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Pełna sekcja `head` jest pokazana tutaj:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

Metoda [pomocnika zawartości adresu URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) Konwertuje ścieżkę zasobu na ścieżkę bezwzględną. Należy użyć `@URL.Content`, aby prawidłowo odwoływać się do tych zasobów, gdy aplikacja jest uruchomiona w usługach IIS.

Naciśnij klawisze CTRL+F5, aby uruchomić aplikację. Wybierz łącze Edytuj, a następnie umieść punkt wstawiania w polu **ReleaseDate** . Zostanie wyświetlony kalendarz podręczny interfejsu użytkownika jQuery.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Podobnie jak większość formantów jQuery, DatePicker umożliwia szerokie dostosowanie go. Aby uzyskać więcej informacji, zobacz [Visual Customization: Projektowanie interfejsu użytkownika jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) w lokacji [interfejsu użytkownika jQuery](http://learn.jquery.com/jquery-ui/getting-started/) .

### <a name="supporting-the-html5-date-input-control"></a>Obsługa kontroli danych wejściowych daty HTML5

Ponieważ więcej przeglądarek obsługuje język HTML5, należy użyć natywnego wejścia HTML5, takiego jak `date` elementu wejściowego, i nie używać kalendarza interfejsu użytkownika jQuery. Możesz dodać logikę do aplikacji, aby automatycznie używać formantów HTML5, jeśli przeglądarka je obsługuje. Aby to zrobić, Zastąp zawartość pliku *DatePickerReady. js* następującym:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

Pierwszy wiersz tego skryptu używa programu modernizacji do sprawdzenia, czy jest obsługiwane wprowadzanie dat HTML5. Jeśli nie jest to obsługiwane, selektor daty interfejsu użytkownika jQuery jest podłączany. ([Modernizacja](http://www.modernizr.com/docs/) to biblioteka języka JavaScript typu open source, która wykrywa Dostępność natywnych implementacji języków HTML5 i CSS3. Modernizacja jest dołączana do nowych projektów ASP.NET MVC tworzonych przez użytkownika.

Po wprowadzeniu tej zmiany możesz ją przetestować przy użyciu przeglądarki obsługującej HTML5, takiej jak Opera 11. Uruchom aplikację przy użyciu przeglądarki zgodnej z HTML5 i Edytuj wpis filmu. Zamiast okienka podręcznego interfejsu użytkownika jQuery jest używany formant daty HTML5:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Ze względu na to, że nowe wersje przeglądarek wdrażają program HTML5 przyrostowo, dobrym rozwiązaniem jest dodanie kodu do witryny sieci Web, który obsługuje szeroką gamę obsługi HTML5. Na przykład poniżej przedstawiono bardziej niezawodny skrypt *DatePickerReady. js* , który umożliwia obsługę przeglądarek, które tylko częściowo obsługują kontrolę daty HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Ten skrypt wybiera elementy HTML5 `input` typu `date`, które nie obsługują w pełni kontroli daty HTML5. Dla tych elementów przechwytuje kalendarz podręczny interfejsu użytkownika jQuery, a następnie zmienia atrybut `type` z `date` na `text`. Zmiana `type` atrybutu z `date` na `text`powoduje, że obsługa częściowej daty HTML5 zostanie wyeliminowana. Jeszcze bardziej niezawodny skrypt *DatePickerReady. js* można znaleźć pod adresem [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Dodawanie do szablonów dat dopuszczających wartości null

Jeśli używasz jednego z istniejących szablonów dat i przekażesz datę null, wystąpi błąd czasu wykonywania. Aby szablony dat były bardziej niezawodne, należy zmienić je na obsługę wartości null. W celu zapewnienia obsługi dat dopuszczających wartości null Zmień kod w *Views\Shared\DisplayTemplates\DateTime.cshtml* na następujący:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

Kod zwraca pusty ciąg, jeśli model ma **wartość null**.

Zmień kod w pliku *Views\Shared\EditorTemplates\Date.cshtml* na następujący:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Gdy ten kod jest uruchamiany, jeśli model nie ma wartości null, zostanie użyta wartość `DateTime` modelu. Jeśli model ma wartość null, zamiast niej zostanie użyta bieżąca data.

### <a name="wrapup"></a>Wrapup

Ten samouczek obejmuje podstawy pomocników z szablonem ASP.NET i pokazuje, jak używać okna podręcznego interfejsu użytkownika jQuery DatePicker w aplikacji ASP.NET MVC. Aby uzyskać więcej informacji, wypróbuj następujące zasoby:

- Aby uzyskać informacje dotyczące lokalizacji, zobacz blog Rajeesh [JQueryUI DatePicker in ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Aby uzyskać informacje o interfejsie użytkownika jQuery, zobacz [interfejs użytkownika jQuery](http://docs.jquery.com/UI).
- Aby uzyskać informacje o sposobie lokalizowania formantu DatePicker, zobacz [UI/DatePicker/lokalizacja](http://docs.jquery.com/UI/Datepicker/Localization).
- Aby uzyskać więcej informacji na temat szablonów ASP.NET MVC, zapoznaj się z serią blogów Brada Wilson na temat [ASP.NET szablonów MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Mimo że seria została zapisywana dla ASP.NET MVC 2, materiał nadal stosuje się do bieżącej wersji ASP.NET MVC.

> [!div class="step-by-step"]
> [Wstecz](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)

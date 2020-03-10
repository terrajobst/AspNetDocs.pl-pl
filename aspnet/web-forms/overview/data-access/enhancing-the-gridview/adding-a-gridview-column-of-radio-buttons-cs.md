---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Dodawanie jednokolumnowych przycisków radiowych (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku pokazano, jak dodać kolumnę przycisków radiowych do kontrolki GridView, aby zapewnić użytkownikowi bardziej intuicyjny sposób zaznaczania pojedynczego wiersza...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: b59cc64b14c6414e6558fdb8a281644db8386701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78591099"
---
# <a name="adding-a-gridview-column-of-radio-buttons-c"></a>Dodawanie kolumny przycisków radiowych do kontrolki GridView (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) lub [Pobierz plik PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> W tym samouczku pokazano, jak dodać kolumnę przycisków radiowych do kontrolki GridView, aby zapewnić użytkownikowi bardziej intuicyjny sposób zaznaczania pojedynczego wiersza GridView.

## <a name="introduction"></a>Wprowadzenie

Kontrolka GridView oferuje doskonałą liczbę wbudowanych funkcji. Zawiera kilka różnych pól do wyświetlania tekstu, obrazów, hiperłączy i przycisków. Obsługuje ona szablony do dalszej dostosowywania. Za pomocą kilku kliknięć myszy można wykonać widok GridView, w którym każdy wiersz może być wybierany za pośrednictwem przycisku lub aby umożliwić edytowanie lub usuwanie funkcji. Pomimo mnóstwo podanych funkcji, często będą sytuacje, w których konieczne będzie dodanie dodatkowych, nieobsługiwanych funkcji. W tym samouczku i następnych dwóch przeanalizuje sposób udoskonalenia funkcji GridView s w celu uwzględnienia dodatkowych funkcji.

Ten samouczek i kolejny fokus dotyczący ulepszania procesu wyboru wiersza. Zgodnie z opisem w części [wzorzec/szczegóły przy użyciu możliwego do wyboru wzorca GridView z kontrolką detailviewą szczegółów](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), możemy dodać CommandField do widoku GridView zawierającego przycisk Wybierz. Po kliknięciu polecenie ogłaszania zwrotnego i właściwość GridView `SelectedIndex` są aktualizowane do indeksu wiersza, którego przycisk wyboru został kliknięty. W obszarze [wzorzec/szczegóły przy użyciu wybranego wzorca GridView z szczegółowym kontrolką detailview](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczka dowiesz się, jak używać tej funkcji do wyświetlania szczegółowych informacji o wybranym wierszu GridView.

Chociaż przycisk Wybierz działa w wielu sytuacjach, może nie działać również dla innych. Zamiast korzystać z przycisku, dwa inne elementy interfejsu użytkownika są często używane do zaznaczania: przycisk radiowy i pole wyboru. Możemy rozszerzyć widok GridView, aby zamiast przycisku SELECT, każdy wiersz zawiera przycisk radiowy lub pole wyboru. W scenariuszach, w których użytkownik może wybrać tylko jeden z rekordów GridView, przycisk radiowy może być preferowany przez przycisk Wybierz. W sytuacjach, w których użytkownik może wybrać wiele rekordów, takich jak w aplikacji poczty e-mail opartej na sieci Web, w której użytkownik może chcieć wybrać wiele komunikatów do usunięcia pola wyboru oferuje funkcje, które nie są dostępne za pomocą przycisku SELECT lub przycisku radiowego interfejsy użytkownika.

Ten samouczek przedstawia sposób dodawania kolumny przycisków radiowych do widoku GridView. Samouczek dotyczący postępu eksplorowania przy użyciu pól wyboru.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Krok 1. Tworzenie rozszerzających się stron sieci Web

Zanim zaczniemy rozszerzać widok GridView w celu uwzględnienia kolumny przycisków radiowych, poczekaj chwilę na utworzenie stron ASP.NET w naszym projekcie witryny sieci Web, które będziemy potrzebować w tym samouczku i następnych dwóch. Zacznij od dodania nowego folderu o nazwie `EnhancedGridView`. Następnie Dodaj następujące strony ASP.NET do tego folderu, aby upewnić się, że każda strona jest skojarzona z `Site.master` stroną wzorcową:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`

![Dodaj strony ASP.NET dla samouczków związanych z kontrolki SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Rysunek 1**. dodawanie stron ASP.NET dla samouczków związanych z kontrolki SqlDataSource

Podobnie jak w przypadku innych folderów, `Default.aspx` w folderze `EnhancedGridView` wyświetli samouczki w sekcji. Odwołaj się do tego, że formant użytkownika `SectionLevelTutorialListing.ascx` udostępnia tę funkcję. W związku z tym należy dodać tę kontrolkę użytkownika do `Default.aspx`, przeciągając ją z Eksplorator rozwiązań na stronę widok Projekt strony.

[![dodać kontrolkę użytkownika SectionLevelTutorialListing. ascx do default. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Rysunek 2**. Dodawanie kontrolki użytkownika `SectionLevelTutorialListing.ascx` do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))

Na koniec Dodaj te cztery strony jako wpisy do pliku `Web.sitemap`. W każdym przypadku Dodaj następujące znaczniki po użyciu kontrolki kontrolki SqlDataSource `<siteMapNode>`:

[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Po aktualizacji `Web.sitemap`zapoznaj się z witryną internetową samouczków za pomocą przeglądarki. Menu po lewej stronie zawiera teraz elementy do edycji, wstawiania i usuwania samouczków.

![Mapa witryny zawiera teraz wpisy rozszerzające samouczki GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Rysunek 3**. Mapa witryny zawiera teraz wpisy rozszerzające samouczki GridView

## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Krok 2. Wyświetlanie dostawców w widoku GridView

W ramach tego samouczka utworzysz widok GridView, który wyświetla listę dostawców z USA, z każdym wierszem GridView zawierającym przycisk radiowy. Po wybraniu dostawcy za pośrednictwem przycisku radiowego użytkownik może wyświetlić produkty dostawcy, klikając przycisk. Chociaż to zadanie może być trudne, istnieje wiele subtleties, które sprawiają, że jest to szczególnie trudne. Przed odstąpieniem do tych subtleties należy najpierw uzyskać widok GridView zawierający listę dostawców.

Aby rozpocząć, Otwórz stronę `RadioButtonField.aspx` w folderze `EnhancedGridView`, przeciągając Widok GridView z przybornika na projektanta. Ustaw `ID` GridView s na `Suppliers` i, z jego tagu inteligentnego, wybierz opcję utworzenia nowego źródła danych. W celu utworzenia elementu ObjectDataSource o nazwie `SuppliersDataSource`, który pobiera dane z obiektu `SuppliersBLL`.

[![utworzyć nowego elementu ObjectDataSource o nazwie SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Rysunek 4**. Utwórz nowy element ObjectDataSource o nazwie `SuppliersDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))

[![skonfigurować element ObjectDataSource do używania klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Rysunek 5**. Konfigurowanie elementu ObjectDataSource do używania klasy `SuppliersBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))

Ponieważ chcemy wyświetlić tylko tych dostawców w USA, wybierz metodę `GetSuppliersByCountry(country)` z listy rozwijanej na karcie Wybierz.

[![skonfigurować element ObjectDataSource do używania klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Ilustracja 6**. Konfigurowanie elementu ObjectDataSource do używania klasy `SuppliersBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))

Na karcie Aktualizacja wybierz opcję (brak), a następnie kliknij przycisk Dalej.

[![skonfigurować element ObjectDataSource do używania klasy SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Rysunek 7**. Konfigurowanie elementu ObjectDataSource do używania klasy `SuppliersBLL` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))

Ponieważ metoda `GetSuppliersByCountry(country)` akceptuje parametr, Kreator konfiguracji źródła danych poprosi nas o źródło tego parametru. Aby określić twardą wartość zakodowaną (USA, w tym przykładzie), pozostaw listę rozwijaną Źródło parametru jako None i wprowadź wartość domyślną w polu tekstowym. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

[![używać USA jako wartości domyślnej parametru Country](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Ilustracja 8**. Użyj USA jako wartości domyślnej parametru `country` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))

Po zakończeniu działania kreatora widok GridView będzie zawierał BoundField dla każdego z pól danych dostawcy. Usuń wszystkie oprócz `CompanyName`, `City`i `Country` BoundFields, a następnie zmień nazwę właściwości `CompanyName` BoundFields `HeaderText` na wartość dostawca. Po wykonaniu tej czynności składnia deklaracyjne GridView i ObjectDataSource powinna wyglądać podobnie do poniższego.

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Na potrzeby tego samouczka zezwól użytkownikom na wyświetlanie wybranych produktów dostawcy na tej samej stronie, na której znajduje się lista dostawców, lub na innej stronie. Aby to umożliwić, Dodaj dwa kontrolki sieci Web Button do strony. Chcę ustawić `ID` s tych dwóch przycisków do `ListProducts` i `SendToProducts`, z pomysłem, że po kliknięciu `ListProducts` zostanie kliknięte ogłaszanie zwrotne i wybrane produkty dostawcy zostaną wyświetlone na tej samej stronie, ale gdy zostanie kliknięty `SendToProducts`, użytkownik zostanie whisky do innej strony, która wyświetla listę produktów.

Rysunek 9 przedstawia `Suppliers` GridView i dwa kontrolki sieci Web przycisków, gdy są wyświetlane za pomocą przeglądarki.

[![dostawców z USA mają swoją nazwę, miasto i informacje o kraju](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Rysunek 9**. dostawcy z USA mają swoją nazwę, miasto i informacje o kraju ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))

## <a name="step-3-adding-a-column-of-radio-buttons"></a>Krok 3. Dodawanie kolumny przycisków radiowych

W tym momencie `Suppliers` GridView ma trzy BoundFields wyświetlające nazwę firmy, miasto i kraj każdego dostawcy w USA. Nadal brakuje kolumny przycisków radiowych. Niestety, widok GridView nie obejmuje wbudowanej RadioButtonField. w przeciwnym razie można po prostu dodać go do siatki i wykonać. Zamiast tego możemy dodać TemplateField i skonfigurować jej `ItemTemplate`, aby renderować przycisk radiowy, co spowoduje przycisk radiowy dla każdego wiersza GridView.

Początkowo możemy założyć, że żądany interfejs użytkownika może zostać zaimplementowany przez dodanie kontrolki sieci Web RadioButton do `ItemTemplate` TemplateField. W związku z tym w rzeczywistości dodasz pojedynczy przycisk radiowy do każdego wiersza GridView, przyciski radiowe nie mogą być zgrupowane i dlatego nie wykluczają się wzajemnie. Oznacza to, że użytkownik końcowy może wybrać wiele przycisków radiowych jednocześnie z widoku GridView.

Mimo że użycie TemplateField formantów sieci Web RadioButton nie oferuje potrzebnych funkcji, poczekaj na wdrożenie tego podejścia, ponieważ wartościowa, aby poznać dlaczego te przyciski radiowe nie są zgrupowane. Zacznij od dodania TemplateField do widoku GridView dostawcy, a następnie pola z lewej strony. Następnie w tagu inteligentnym GridView kliknij link Edytuj szablony i przeciągnij formant sieci Web RadioButton z przybornika do TemplateField `ItemTemplate` (Zobacz Rysunek 10). Ustaw właściwość `ID` RadioButton na `RowSelector` i Właściwość `GroupName` na `SuppliersGroup`.

[![dodać kontrolkę sieci Web RadioButton do ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Ilustracja 10**. Dodawanie kontrolki sieci Web RadioButton do `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))

Po wprowadzeniu tych dodatków za pomocą projektanta, znacznik GridView powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

Właściwość RadioButton s [`GroupName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) jest używana do grupowania serii przycisków radiowych. Wszystkie kontrolki RadioButton o tej samej wartości `GroupName` są uważane za zgrupowane; jednocześnie można wybrać tylko jeden przycisk radiowy z grupy. Właściwość `GroupName` określa wartość dla `name` atrybutu renderowanego przycisku radiowego. Przeglądarka analizuje przyciski radiowe `name` atrybuty w celu określenia grup przycisków radiowych.

Po dodaniu kontrolki sieci Web RadioButton do `ItemTemplate`odwiedź tę stronę za pomocą przeglądarki, a następnie kliknij przycisk radiowy w wierszach siatki. Zwróć uwagę, jak przyciski radiowe nie są zgrupowane, dzięki czemu można wybrać wszystkie wiersze, jak pokazano na rysunku 11.

[![przyciski radiowe GridView s nie są zgrupowane](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Ilustracja 11**. przyciski radiowe "GridView" nie są zgrupowane ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))

Przyczyną, że przyciski radiowe nie są zgrupowane, są takie same, jak renderowane `name` atrybuty są różne, mimo że ma to samo ustawienie właściwości `GroupName`. Aby wyświetlić te różnice, należy wykonać widok/źródło z przeglądarki i sprawdzić adiustację przycisków radiowych:

[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Zwróć uwagę, jak atrybuty `name` i `id` nie są dokładnymi wartościami określonymi w okno Właściwości, ale są poprzedzone liczbą innych wartości `ID`. Dodatkowe wartości `ID` dodane na początku renderowanych `id` i `name` atrybuty są `ID` s przycisków radiowych, które są elementami nadrzędnymi, `GridViewRow` s `ID` s, `ID`GridView s, Content Control-`ID`i formularzy sieci Web s `ID`. Te `ID` s są dodawane, aby każdy renderowany formant sieci Web w widoku GridView miał unikatowe wartości `id` i `name`.

Każda renderowana kontrolka wymaga innego `name` i `id`, ponieważ jest to sposób, w jaki przeglądarka jednoznacznie identyfikuje poszczególne kontrolki po stronie klienta i identyfikujący serwer sieci Web, jakie akcje lub zmiany pojawiły się w odniesieniu do ogłaszania zwrotnego. Załóżmy na przykład, że chcemy uruchomić jakiś kod po stronie serwera za każdym razem, gdy stan zaznaczono element RadioButton. Możemy to osiągnąć przez ustawienie właściwości `AutoPostBack` RadioButton, aby `true` i utworzyć procedurę obsługi zdarzeń dla zdarzenia `CheckChanged`. Jednakże jeśli renderowane `name` i `id` wartości dla wszystkich przycisków radiowych były takie same, na stronie ogłaszania zwrotnego nie udało nam się określić, który został kliknięty.

W krótkim przypadku nie można utworzyć kolumny przycisków radiowych w widoku GridView przy użyciu kontrolki sieci Web RadioButton. Zamiast tego należy użyć technik niestabilnego, aby upewnić się, że odpowiednie oznakowanie jest wstrzykiwane do każdego wiersza GridView.

> [!NOTE]
> Podobnie jak w przypadku kontrolki sieci Web RadioButton, kontrolka HTML przycisku radiowego, po dodaniu do szablonu, będzie zawierać unikatowy atrybut `name`, co sprawia, że przyciski radiowe w siatce są niezgrupowane. Jeśli nie znasz formantów HTML, możesz zignorować tę uwagę, ponieważ kontrolki HTML rzadko są używane, szczególnie w ASP.NET 2,0. Ale jeśli chcesz dowiedzieć się więcej, zobacz sekcję [K. Scott](http://odetocode.com/blogs/scott/default.aspx) odnoszący się do wpisów [w blogu i kontrolek HTML](http://www.odetocode.com/Articles/348.aspx).

## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Używanie kontrolki Literal do dodawania znaczników przycisków radiowych

Aby poprawnie zgrupować wszystkie przyciski radiowe w widoku GridView, musimy ręcznie wstrzyknąć znaczniki przycisków radiowych do `ItemTemplate`. Każdy przycisk radiowy wymaga tego samego atrybutu `name`, ale powinien mieć unikatowy atrybut `id` (w przypadku, gdy chcemy uzyskać dostęp do przycisku radiowego za pośrednictwem skryptu po stronie klienta). Gdy użytkownik wybierze przycisk radiowy i opublikuje stronę, przeglądarka wyśle do tyłu wartość zaznaczonego przycisku radiowego `value` atrybutu. W związku z tym każdy przycisk radiowy będzie potrzebować unikatowego atrybutu `value`. Na koniec należy pamiętać, aby dodać atrybut `checked` do jednego przycisku radiowego, który jest zaznaczony, w przeciwnym razie po dokonaniu wyboru i przeniesieniu zwrotów przyciski radiowe powracają do stanu domyślnego (wszystkie niezaznaczone).

Istnieją dwie metody, które można podjąć w celu dodania znaczników niskiego poziomu do szablonu. Jednym z nich jest przeprowadzenie kombinacji znaczników i wywołań metod formatowania zdefiniowanych w klasie powiązanej z kodem. Ta technika została najpierw omówiona w samouczku [Using w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) . W naszym przypadku może wyglądać następująco:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Tutaj `GetUniqueRadioButton` i `GetRadioButtonValue` byłyby metodami zdefiniowanymi w klasie związanej z kodem, która zwróciła odpowiednie `id` i `value` wartości atrybutów dla każdego przycisku radiowego. To podejście dobrze sprawdza się w przypadku przypisywania atrybutów `id` i `value`, ale jest krótkie, gdy trzeba określić wartość atrybutu `checked`, ponieważ składnia DataBinding jest wykonywana tylko wtedy, gdy dane są najpierw powiązane z elementem GridView. W związku z tym, jeśli w widoku GridView jest włączony stan widoku, metody formatowania będą wyzwalane tylko wtedy, gdy strona zostanie załadowana po raz pierwszy (lub gdy GridView zostanie jawnie powiązana ze źródłem danych), a w związku z tym funkcja, która ustawia atrybut `checked` nie będzie wywoływana w przypadku ogłaszania zwrotnego. Jest to raczej delikatny problem i bit wykraczający poza zakres tego artykułu, dlatego pozostawi to. Jednak zachęcam do wypróbowania korzystania z powyższego podejścia i przechodzenia do punktu, w którym będziesz mieć zablokowany. Chociaż takie ćwiczenie nie powiedzie się w stosunku do wersji roboczej, pomoże to lepiej zrozumieć widok GridView i cały cykl życia powiązań danych.

Innym podejściem do dodawania niestandardowych znaczników niskiego poziomu w szablonie i podejście, które będziemy używać na potrzeby tego samouczka, jest dodanie [kontrolki literału](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) do szablonu. Następnie w `RowCreated` GridView lub `RowDataBound` obsługi zdarzeń można uzyskać dostęp do kontrolki literału, a jej Właściwość `Text` ustawiona na wartość Emituj.

Zacznij od usunięcia elementu RadioButton z `ItemTemplate`TemplateField, zastępując go kontrolką Literal. Ustaw `ID` kontrolki literału na `RadioButtonMarkup`.

[![dodać kontrolkę literału do ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Ilustracja 12**. Dodawanie kontrolki Literal do `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))

Następnie Utwórz procedurę obsługi zdarzeń dla zdarzenia `RowCreated` GridView. Zdarzenie `RowCreated` wyzwalane raz dla każdego dodanego wiersza, niezależnie od tego, czy dane są ponownie powiązane z elementem GridView. Oznacza to, że nawet w przypadku ogłaszania zwrotnego po ponownym załadowaniu danych z stanu widoku, zdarzenie `RowCreated` nadal zostanie wyzwolone i jest to powód, z którego korzystamy, zamiast `RowDataBound` (które jest wyzwalane tylko wtedy, gdy dane są jawnie powiązane z formantem sieci Web danych).

W tym obsłudze zdarzeń chcemy kontynuować tylko w przypadku, gdy zajmiemy się wierszem danych. Dla każdego wiersza danych chcemy programowo odwoływać się do `RadioButtonMarkup` kontrolki literał i ustawić jej Właściwość `Text` na wartość Emituj. Jak pokazano na poniższym kodzie, emitowane adiustacje tworzą przycisk radiowy, którego atrybut `name` jest ustawiony na `SuppliersGroup`, którego atrybut `id` jest ustawiony na `RowSelectorX`, gdzie *X* jest indeksem wiersza GridView i którego atrybut `value` jest ustawiony na indeks wiersza GridView.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Gdy zostanie wybrany wiersz GridView i następuje ogłaszanie zwrotne, chcemy `SupplierID` wybranego dostawcy. W związku z tym, jeden może być uważany, że wartość każdego przycisku radiowego powinna być rzeczywistym `SupplierID` (a nie indeksem wiersza GridView). Chociaż może to potrwać w pewnych okolicznościach, będzie to ryzykowne dla bezpieczeństwa, które pozwala odejść i przetworzyć `SupplierID`. W naszym widoku GridView można na przykład uzyskać listę tylko tych dostawców w USA. Jeśli jednak `SupplierID` jest przekazywany bezpośrednio z przycisku radiowego, co uniemożliwia użytkownikowi mischievousemu manipulowanie `SupplierID` wartości wysyłanej na odświeżenie? Używając indeksu wierszy jako `value`, a następnie pobierając `SupplierID` na stronie ogłaszania zwrotnego z kolekcji `DataKeys`, możemy upewnić się, że użytkownik używa tylko jednej z wartości `SupplierID` skojarzonych z jednym z wierszy GridView.

Po dodaniu tego kodu obsługi zdarzeń Poświęć minutę na przetestowanie strony w przeglądarce. Najpierw należy zauważyć, że w danej chwili można wybrać tylko jeden przycisk radiowy w siatce. Jednak po wybraniu przycisku radiowego i kliknięciu jednego z przycisków następuje ogłaszanie zwrotne, a wszystkie przyciski radiowe odwracają do stanu początkowego (to znaczy, że po odświeżeniu nie jest już zaznaczony przycisk radiowy). Aby rozwiązać ten problem, należy rozszerzyć program obsługi zdarzeń `RowCreated`, tak aby sprawdził wybrany indeks przycisku radiowego wysłany z ogłaszania zwrotnego i dodaje atrybut `checked="checked"` do wyemitowanego znacznika dopasowania indeksu wierszy.

Gdy następuje ogłaszanie zwrotne, przeglądarka wysyła `name` i `value` wybranego przycisku radiowego. Wartość można programowo pobrać przy użyciu `Request.Form["name"]`. [Właściwość`Request.Form`](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) zapewnia [`NameValueCollection`](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) reprezentującą zmienne formularza. Zmienne formularza to nazwy i wartości pól formularza na stronie sieci Web i są wysyłane z powrotem przez przeglądarkę sieci Web za każdym razem, gdy ogłasza zwrotne. Ponieważ renderowany `name` atrybutu przycisków radiowych w widoku GridView jest `SuppliersGroup`, po ponownym opublikowaniu strony sieci Web przeglądarka wyśle `SuppliersGroup=valueOfSelectedRadioButton` z powrotem do serwera sieci Web (wraz z innymi polami formularza). Dostęp do tych informacji można uzyskać z właściwości `Request.Form` przy użyciu: `Request.Form["SuppliersGroup"]`.

Ponieważ będziemy musieli określić wybrany indeks przycisku radiowego nie tylko w obsłudze zdarzeń `RowCreated`, ale w `Click` obsługi zdarzeń dla kontrolek sieci Web Button, Zezwól, aby dodać właściwość `SuppliersSelectedIndex` do klasy związanej z kodem, która zwraca `-1`, jeśli nie wybrano przycisku radiowego i wybranego indeksu, jeśli wybrano jeden z przycisków radiowych.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Po dodaniu tej właściwości wiemy, że dodajemy `checked="checked"` znaczników w programie obsługi zdarzeń `RowCreated`, gdy `SuppliersSelectedIndex` równa się `e.Row.RowIndex`. Zaktualizuj procedurę obsługi zdarzeń w celu uwzględnienia tej logiki:

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Po tej zmianie wybrany przycisk radiowy pozostaje wybrany po odświeżeniu. Teraz, gdy mamy możliwość określenia, jaki przycisk radiowy jest wybrany, możemy zmienić zachowanie tak, aby podczas pierwszej wizyty strony zostało zaznaczone przycisk radiowy pierwszy wiersz GridView (a nie nie ma żadnych przycisków radiowych domyślnie zaznaczonych jako bieżąca zachowanie). Aby domyślnie zaznaczyć przycisk radiowy, po prostu zmień instrukcję `if (SuppliersSelectedIndex == e.Row.RowIndex)` na następującą: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

W tym momencie dodaliśmy do widoku GridView kolumnę zgrupowanych przycisków radiowych, która umożliwia wybranie jednego wiersza GridView i zapamiętanie ich na stronach ogłaszania zwrotnego. W następnych krokach są wyświetlane produkty dostarczone przez wybranego dostawcę. W kroku 4 dowiesz się, jak przekierować użytkownika do innej strony, wysyłając ją na wybrane `SupplierID`. W kroku 5 zobaczymy, jak wyświetlić wybrane produkty dostawcy w widoku GridView na tej samej stronie.

> [!NOTE]
> Zamiast korzystać z TemplateField (fokus ten ma długość kroku 3), możemy utworzyć niestandardową klasę `DataControlField`, która renderuje odpowiedni interfejs użytkownika i jego funkcjonalność. [Klasa`DataControlField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) jest klasą bazową, z której pochodzą BoundField, CheckBoxField, TemplateField i inne wbudowane pola GridView i DetailsView. Utworzenie niestandardowej klasy `DataControlField` oznacza, że kolumna przycisków radiowych może zostać dodana bezpośrednio przy użyciu składni deklaracyjnej, a także ułatwić replikację funkcji na innych stronach sieci Web i innych aplikacjach sieci Web.

Jeśli kiedykolwiek utworzysz niestandardowe, skompilowane kontrolki w ASP.NET, wiadomo, że wymaga to odpowiedniej ilości żmudne zadania i przeprowadzi z nim, aby hostować przypadki subtleties i brzegowe, które muszą być starannie obsługiwane. W związku z tym będziemy forgo implementację kolumny przycisków radiowych jako niestandardowej klasy `DataControlField` teraz i naklejać z opcją TemplateField. Prawdopodobnie będziemy mogli poznać tworzenie, używanie i wdrażanie niestandardowych klas `DataControlField` w przyszłym samouczku!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Krok 4. Wyświetlanie wybranych produktów dostawcy na osobnej stronie

Gdy użytkownik wybierze wiersz GridView, musimy pokazać wybrane produkty dostawcy. W pewnych okolicznościach możemy chcieć wyświetlić te produkty na osobnej stronie, w innych, możemy woli tego zrobić na tej samej stronie. Pozwól, aby najpierw przeanalizować sposób wyświetlania produktów na osobnej stronie; w kroku 5 dowiesz się, jak dodać GridView do `RadioButtonField.aspx`, aby wyświetlić wybrane produkty dostawcy.

Obecnie istnieją dwa kontrolki sieci Web przycisków na stronie `ListProducts` i `SendToProducts`. Po kliknięciu przycisku `SendToProducts` chcemy wysłać użytkownika do `~/Filtering/ProductsForSupplierDetails.aspx`. Ta strona została utworzona w samouczku [wzorzec/szczegóły dla dwóch stron](../masterdetail/master-detail-filtering-across-two-pages-cs.md) i zawiera produkty dla dostawcy, którego `SupplierID` jest przenoszona za pośrednictwem pola QueryString o nazwie `SupplierID`.

Aby zapewnić tę funkcję, Utwórz procedurę obsługi zdarzeń dla zdarzenia `Click` `SendToProducts` przycisku. W kroku 3 dodaliśmy Właściwość `SuppliersSelectedIndex`, która zwraca indeks wiersza, którego przycisk radiowy jest zaznaczony. Odpowiednie `SupplierID` można pobrać z kolekcji GridView s `DataKeys`, a użytkownik może zostać wysłany do `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` przy użyciu `Response.Redirect("url")`.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Ten kod działa dobrze, o ile jeden z przycisków radiowych został wybrany z widoku GridView. Jeśli początkowo w widoku GridView nie zaznaczono żadnych przycisków opcji, a użytkownik kliknie przycisk `SendToProducts`, `SuppliersSelectedIndex` zostanie `-1`, co spowoduje zgłoszenie wyjątku, ponieważ `-1` poza zakresem indeksu kolekcji `DataKeys`. Nie dotyczy to jednak sytuacji, w której podjęto decyzję o zaktualizowaniu programu obsługi zdarzeń `RowCreated`, zgodnie z opisem w kroku 3, tak aby pierwszy przycisk radiowy w elemencie GridView był początkowo zaznaczony.

Aby pomieścić `SuppliersSelectedIndex` wartość `-1`, Dodaj kontrolkę sieci Web etykieta do strony powyżej widoku GridView. Ustaw jej Właściwość `ID` na `ChooseSupplierMsg`, jej Właściwość `CssClass` na `Warning`, jej `EnableViewState` i `Visible` właściwości do `false`, a jej Właściwość `Text` wybierz dostawcę z siatki. Klasa CSS `Warning` wyświetla tekst w kolorze czerwonym, kursywą, pogrubienie, dużą czcionkę i jest zdefiniowany w `Styles.css`. Ustawiając `EnableViewState` i `Visible` właściwości na `false`, etykieta nie jest renderowana, z wyjątkiem tego, że tylko te zwrotne, w których właściwość Control s `Visible` jest programowo ustawiona na `true`.

[![dodać kontrolkę sieci Web etykieta powyżej widoku GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Ilustracja 13**. Dodawanie kontrolki sieci Web etykiety powyżej widoku GridView ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))

Następnie należy rozszerzyć procedurę obsługi zdarzeń `Click`, aby wyświetlić etykietę `ChooseSupplierMsg`, jeśli `SuppliersSelectedIndex` jest mniejsza od zera, i przekierować użytkownika do `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` w inny sposób.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Odwiedź stronę w przeglądarce i kliknij przycisk `SendToProducts` przed wybraniem dostawcy z widoku GridView. Jak pokazano na rysunku 14, zostanie wyświetlona etykieta `ChooseSupplierMsg`. Następnie wybierz dostawcę i kliknij przycisk `SendToProducts`. Spowoduje to wypróbowanie strony zawierającej listę produktów dostarczonych przez wybranego dostawcę. Rysunek 15 pokazuje stronę `ProductsForSupplierDetails.aspx`, gdy wybrano dostawcę Bigfoot Breweries.

[![etykieta ChooseSupplierMsg jest wyświetlana, jeśli nie wybrano dostawcy](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Ilustracja 14**: etykieta `ChooseSupplierMsg` jest wyświetlana, jeśli nie wybrano żadnego dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))

[![wybrane produkty dostawcy są wyświetlane w ProductsForSupplierDetails. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Ilustracja 15**. wybrane produkty dostawcy są wyświetlane w `ProductsForSupplierDetails.aspx` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))

## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Krok 5. Wyświetlanie wybranych produktów dostawcy na tej samej stronie

W kroku 4 pokazano, jak wysłać użytkownika na inną stronę sieci Web, aby wyświetlić wybrane produkty dostawcy. Alternatywnie wybrane produkty dostawcy mogą być wyświetlane na tej samej stronie. Aby to zilustrować, dodamy kolejny widok GridView do `RadioButtonField.aspx`, aby wyświetlić wybrane produkty dostawcy.

Ponieważ chcemy, aby tylko ten element GridView był wyświetlany po wybraniu dostawcy, Dodaj kontrolkę sieci Web panelu poniżej `Suppliers` GridView, ustawiając jej `ID` na `ProductsBySupplierPanel` i jej Właściwość `Visible` na `false`. W panelu Dodaj produkty tekstowe dla wybranego dostawcy, a następnie element GridView o nazwie `ProductsBySupplier`. W tagu inteligentnym GridView wybierz, aby powiązać go z nowym elementem ObjectDataSource o nazwie `ProductsBySupplierDataSource`.

[![Powiąż element GridView ProductsBySupplier z nowym elementem ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Rysunek 16**. powiązanie `ProductsBySupplier` GridView z nowym elementem ObjectDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))

Następnie skonfiguruj element ObjectDataSource, aby używał klasy `ProductsBLL`. Ponieważ chcemy tylko pobrać te produkty dostarczone przez wybranego dostawcę, należy określić, że element ObjectDataSource powinien wywołać metodę `GetProductsBySupplierID(supplierID)`, aby pobrać swoje dane. Wybierz pozycję (brak) z listy rozwijanej na kartach UPDATE, INSERT i DELETE.

[![skonfigurować element ObjectDataSource do używania metody GetProductsBySupplierID (IDDostawcy)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Ilustracja 17**. Konfigurowanie elementu ObjectDataSource do używania metody `GetProductsBySupplierID(supplierID)` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))

[![ustawić listy rozwijane na (brak) na kartach Aktualizuj, Wstaw i Usuń](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Ilustracja 18**. Ustaw listę rozwijaną na (brak) na kartach Update, INSERT i Delete ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))

Po skonfigurowaniu kart wybierz, Aktualizuj, Wstaw i Usuń kliknij przycisk Dalej. Ponieważ metoda `GetProductsBySupplierID(supplierID)` oczekuje parametru wejściowego, Kreator tworzenia źródła danych monituje nas o określenie źródła wartości parametru s.

W tym miejscu dostępne są kilka opcji określania źródła wartości parametru s. Możemy użyć domyślnego obiektu parametru i programowo przypisać wartość właściwości `SuppliersSelectedIndex` do właściwości `DefaultValue` parametru s w programie obsługi zdarzeń `Selecting` ObjectDataSource s. Zapoznaj się z powrotem z [programowo ustawieniem samouczek wartości parametrów elementu ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) dla odświeżacza na potrzeby programistycznego przypisywania wartości do parametrów ObjectDataSource s.

Alternatywnie możemy użyć parametrze ControlParameter i odwołać się do właściwości `Suppliers` GridView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (zobacz rysunek 19). Właściwość GridView s `SelectedValue` zwraca wartość `DataKey` odpowiadającą [właściwości`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Aby ta opcja działała, należy programowo ustawić właściwość GridView `SelectedIndex` w wybranym wierszu po kliknięciu przycisku `ListProducts`. Jako dodaną korzyść, ustawiając `SelectedIndex`, wybrany rekord zajmie `SelectedRowStyle` zdefiniowane w motywie `DataWebControls` (żółtym tle).

[![użyć parametrze ControlParameter do określenia widoku GridView s SelectedValue jako źródła parametru](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Ilustracja 19**. użycie parametrze ControlParameter do określenia widoku GridView s SelectedValue jako źródła parametru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))

Po zakończeniu działania kreatora program Visual Studio automatycznie doda pola do pól danych produktu. Usuń wszystkie oprócz `ProductName`, `CategoryName`i `UnitPrice` BoundFields, a następnie zmień właściwości `HeaderText` na Product, Category i Price. Skonfiguruj `UnitPrice` BoundField tak, aby jego wartość była sformatowana jako waluta. Po wprowadzeniu tych zmian znaczniki deklaratywne paneli, GridView i ObjectDataSource powinny wyglądać następująco:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Aby wykonać to ćwiczenie, należy ustawić właściwość GridView-`SelectedIndex` w `SelectedSuppliersIndex` i Właściwość `Visible` `ProductsBySupplierPanel`, aby `true`, gdy zostanie kliknięty przycisk `ListProducts`. Aby to osiągnąć, Utwórz procedurę obsługi zdarzeń dla `ListProducts` przycisku `Click` formant sieci Web, a następnie Dodaj następujący kod:

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Jeśli dostawca nie został wybrany z widoku GridView, zostanie wyświetlona etykieta `ChooseSupplierMsg`, a panel `ProductsBySupplierPanel` ukryty. W przeciwnym razie, jeśli wybrano dostawcę, zostanie wyświetlona `ProductsBySupplierPanel` i zostanie zaktualizowana właściwość GridView s `SelectedIndex`.

Rysunek 20 przedstawia wyniki po wybraniu dostawcy Bigfoot Breweries i kliknięciu przycisku Pokaż produkty na stronie.

[![produkty dostarczone przez Bigfoot Breweries są wyświetlane na tej samej stronie](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Ilustracja 20**. produkty dostarczone przez Bigfoot Breweries są wyświetlane na tej samej stronie ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))

## <a name="summary"></a>Podsumowanie

Jak opisano w [wzorcu/szczegółach przy użyciu wybranego wzorca GridView z szczegółowym](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) samouczkiem kontrolką detailview, rekordy można wybrać z widoku GridView przy użyciu CommandField, którego właściwość `ShowSelectButton` jest ustawiona na `true`. Ale CommandField wyświetla jego przyciski jako zwykłe przyciski wypychania, linki lub obrazy. Alternatywny interfejs użytkownika wybierający wiersz polega na udostępnieniu przycisku radiowego lub pola wyboru w każdym wierszu GridView. W tym samouczku sprawdzono, jak dodać kolumnę przycisków radiowych.

Niestety, Dodawanie kolumny przycisków radiowych jest t jako proste lub proste, ponieważ może to być oczekiwane. Nie istnieje wbudowana RadioButtonField, którą można dodać po kliknięciu przycisku, a za pomocą kontrolki sieci Web RadioButton w TemplateField wprowadza swój własny zestaw problemów. Na koniec, aby zapewnić taki interfejs, należy utworzyć niestandardową klasę `DataControlField` lub możliwość wprowadzenia odpowiedniego kodu HTML do TemplateField podczas `RowCreated` zdarzenia.

Po dodaniu, jak dodać kolumnę przycisków radiowych, daj nam zwrócić uwagę na dodawanie kolumny pól wyboru. Za pomocą kolumny pól wyboru użytkownik może wybrać jeden lub więcej wierszy GridView, a następnie wykonać kilka operacji na wszystkich wybranych wierszach (na przykład wybierając zestaw wiadomości e-mail z internetowego klienta poczty e-mail, a następnie wybrać opcję usunięcia wszystkich wybranych wiadomości e-mail). W następnym samouczku zobaczymy, jak dodać taką kolumnę.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnego klienta dla tego samouczka był David suru. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Dalej](adding-a-gridview-column-of-checkboxes-cs.md)

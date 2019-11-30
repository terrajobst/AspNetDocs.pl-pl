---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Ograniczanie funkcji modyfikacji danych na podstawie użytkownika (VB) | Microsoft Docs
author: rick-anderson
description: W aplikacji sieci Web, która umożliwia użytkownikom edytowanie danych, różne konta użytkowników mogą mieć różne uprawnienia do edycji danych. W tym samouczku sprawdzimy, jak t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: c257a930e4d27fcd42591a541e700786bf413bf0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626812"
---
# <a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Ograniczanie funkcji modyfikacji danych na podstawie użytkownika (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) lub [Pobierz plik PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> W aplikacji sieci Web, która umożliwia użytkownikom edytowanie danych, różne konta użytkowników mogą mieć różne uprawnienia do edycji danych. W tym samouczku sprawdzimy, jak dynamicznie dostosować możliwości modyfikacji danych na podstawie odwiedzanego użytkownika.

## <a name="introduction"></a>Wprowadzenie

Wiele aplikacji sieci Web obsługuje konta użytkowników i udostępnia różne opcje, raporty i funkcje w oparciu o zalogowanego użytkownika. Na przykład dzięki naszym samouczkom firma Microsoft może chcieć zezwolić użytkownikom z firm dostawców na logowanie do witryny i aktualizowanie ogólnych informacji o ich produktach — ich nazwy i ilości na jednostkę, prawdopodobnie wraz z informacjami o dostawcach, takimi jak nazwa firmy, adres, informacje o osobach kontaktowych i tak dalej. Ponadto firma Microsoft może chcieć uwzględnić niektóre konta użytkowników dla osób z naszej firmy, aby mogli logować się i aktualizować informacje o produkcie, takie jak jednostki na giełdie, poziom zmiany kolejności itd. Nasza aplikacja sieci Web może również zezwalać anonimowym użytkownikom na odwiedzenie (osoby, które nie zalogował się), ale ograniczą je do wyświetlania tylko danych. Dzięki takiemu systemowi kont użytkowników będziemy chcieć, aby formanty sieci Web na naszych stronach ASP.NET oferowały funkcje wstawiania, edytowania i usuwania odpowiednie dla aktualnie zalogowanego użytkownika.

W tym samouczku sprawdzimy, jak dynamicznie dostosować możliwości modyfikacji danych na podstawie odwiedzanego użytkownika. W szczególności utworzymy stronę wyświetlającą informacje o dostawcach w edytowalnej widoku DetailsView wraz z elementem GridView zawierającym listę produktów dostarczonych przez dostawcę. Jeśli użytkownik odwiedze stronę od firmy, może: wyświetlić informacje o dostawcy s; Edytuj swój adres; i Edytuj informacje dla każdego produktu dostarczonego przez dostawcę. Jeśli jednak użytkownik należy do konkretnej firmy, może wyświetlać i edytować własne informacje o adresie, a także edytować swoje produkty, które nie zostały oznaczone jako nieobsługiwane.

[![użytkownik z naszej firmy może edytować wszystkie informacje o dostawcy](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Rysunek 1**: użytkownik z naszej firmy może edytować wszystkie informacje o dostawcy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))

[![użytkownika z określonego dostawcy mogą tylko wyświetlać i edytować swoje informacje](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Rysunek 2**. użytkownik od określonego dostawcy może tylko wyświetlać i edytować swoje informacje ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))

Zacznij korzystać z aplikacji.

> [!NOTE]
> System członkostwa w systemie ASP.NET 2,0 s oferuje ustandaryzowaną, rozszerzalną platformę do tworzenia i weryfikowania kont użytkowników oraz zarządzania nimi. Ze względu na to, że badanie systemu członkostwa wykracza poza zakres tych samouczków, ten samouczek zamiast tego ma członkostwo "sztuczne", dzięki czemu anonimowi użytkownicy mogą zdecydować, czy pochodzą od określonego dostawcy, czy z naszej firmy. Aby uzyskać więcej informacji na temat członkostwa, zapoznaj się z artykułem moje [ASP.NET 2,0 s członkostwo, role i profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) seria artykułów.

## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Krok 1. umożliwienie użytkownikowi określenia praw dostępu

W rzeczywistej aplikacji sieci Web informacje o koncie użytkownika są uwzględniane przez firmę Microsoft lub dla określonego dostawcy, a informacje te byłyby programistycznie dostępne z naszych stron ASP.NET po zalogowaniu się użytkownika do witryny. Te informacje mogą być przechwytywane za pośrednictwem systemu ról ASP.NET 2,0 s, jako informacje o koncie użytkownika za pośrednictwem systemu profilu lub przez niektóre niestandardowe środki.

Ponieważ celem tego samouczka jest przedstawienie sposobu dostosowywania możliwości modyfikacji danych na podstawie zalogowanego użytkownika i nie ma potrzeby zaprezentowania ASP.NET 2,0 s członkostwa, ról i systemów profilów, użyjemy bardzo prostego mechanizmu, aby określić możliwości użytkownika odwiedzają stronę — DropDownList, z której użytkownik może wskazać, czy powinien być w stanie wyświetlać i edytować informacje o dostawcach, czy też informacje o określonych dostawcach, które mogą być wyświetlane i edytowane. Jeśli użytkownik wskaże, że może wyświetlać i edytować wszystkie informacje o dostawcach (domyślnie), może uzyskać dostęp do wszystkich dostawców, edytować wszystkie informacje o adresie dostawcy i zmienić nazwę i ilość na jednostkę dla każdego produktu dostarczonego przez wybranego dostawcę. Jeśli użytkownik wskaże, że może tylko wyświetlać i edytować określonego dostawcę, może jedynie wyświetlić szczegóły i produkty dla tego samego dostawcy i zaktualizować tylko nazwę i ilość na informacje o jednostce dla tych produktów, które *nie* zostały wycofane.

Pierwszym krokiem w tym samouczku jest utworzenie tej DropDownList i wypełnienie jej dostawcami w systemie. Otwórz stronę `UserLevelAccess.aspx` w folderze `EditInsertDelete`, Dodaj element DropDownList, którego właściwość `ID` jest ustawiona na wartość `Suppliers`i Powiąż ten DropDownList z nowym elementem ObjectDataSource o nazwie `AllSuppliersDataSource`.

[![utworzyć nowego elementu ObjectDataSource o nazwie AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Rysunek 3**. Tworzenie nowego elementu ObjectDataSource o nazwie `AllSuppliersDataSource` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))

Ponieważ chcemy, aby ta DropDownList obejmowała wszystkich dostawców, skonfiguruj element ObjectDataSource, aby wywoływał metodę `GetSuppliers()` klasy `SuppliersBLL`. Upewnij się również, że metoda `Update()` ObjectDataSource s jest zamapowana na metodę `UpdateSupplierAddress` klasy `SuppliersBLL`, ponieważ ten element ObjectDataSource będzie również używany przez element DetailsView, który zostanie dodany w kroku 2.

Po zakończeniu pracy z kreatorem elementu ObjectDataSource wykonaj kroki, konfigurując `Suppliers` DropDownList w taki sposób, aby pokazywały pole danych `CompanyName` i używało pola dane `SupplierID` jako wartości dla każdego `ListItem`u.

[![skonfigurować dostawców DropDownList do używania pól danych NazwaFirmy i IDDostawcy](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Rysunek 4**. Konfigurowanie `Suppliers` DropDownList do używania pól danych `CompanyName` i `SupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))

W tym momencie DropDownList wyświetla nazwy firm dostawców w bazie danych. Należy jednak również uwzględnić opcję "Pokaż/Edytuj wszystkie dostawcy" w DropDownList. Aby to osiągnąć, należy ustawić właściwość `Suppliers` DropDownList s `AppendDataBoundItems` na `true`, a następnie dodać `ListItem`, której Właściwość `Text` to "Pokaż/Edytuj wszystkich dostawców" i której wartość jest `-1`. Można to dodać bezpośrednio za pośrednictwem znaczników deklaratywnych lub przez projektanta, przechodząc do okno Właściwości i klikając wielokropek we właściwości `Items` DropDownList.

> [!NOTE]
> Zapoznaj się z powrotem do [*filtrowania wzorzec/szczegóły za pomocą*](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) samouczka DropDownList, aby uzyskać bardziej szczegółową dyskusję na temat dodawania elementu Select All do DataBound DropDownList.

Po ustawieniu właściwości `AppendDataBoundItems` i dodaniu `ListItem`, znaczniki deklaratywne DropDownList s powinny wyglądać następująco:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Rysunek 5 przedstawia zrzut ekranu bieżącego postępu, który jest wyświetlany za pomocą przeglądarki.

[![dostawcy DropDownList zawiera wszystkie listy, a także jeden dla każdego dostawcy](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Rysunek 5**: DropDownList `Suppliers` zawiera wszystkie `ListItem`i jeden dla każdego dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))

Ponieważ chcemy zaktualizować interfejs użytkownika natychmiast po zmianie zaznaczenia przez użytkownika, ustaw właściwość `Suppliers` DropDownList s `AutoPostBack` na `true`. W kroku 2 utworzysz formant DetailsView, który będzie przedstawiał informacje dla dostawców w oparciu o wybór DropDownList. Następnie w kroku 3 utworzymy procedurę obsługi zdarzeń dla tego zdarzenia DropDownList `SelectedIndexChanged`, w którym dodamy kod, który wiąże odpowiednie informacje o dostawcach w widoku DetailsView na podstawie wybranego dostawcy.

## <a name="step-2-adding-a-detailsview-control"></a>Krok 2. Dodawanie kontrolki DetailsView

Zezwól usłudze s na wyświetlanie informacji o dostawcach. W przypadku użytkownika, który może wyświetlać i edytować wszystkich dostawców, widok DetailsView będzie obsługiwał stronicowanie, co umożliwia użytkownikowi przechodzenie przez jeden rekord informacji o dostawcach w danym momencie. Jeśli użytkownik pracuje z konkretnym dostawcą, w widoku DetailsView będą wyświetlane tylko informacje o określonych dostawcach i nie będzie on zawierał interfejsu stronicowania. W obu przypadkach DetailsView musi zezwolić użytkownikowi na edytowanie pól adres dostawcy, miasto i kraj.

Dodaj element DetailsView do strony pod `Suppliers` DropDownList, ustaw jej Właściwość `ID` na `SupplierDetails`i powiąż ją z `AllSuppliersDataSource`m elementem ObjectDataSource utworzonym w poprzednim kroku. Następnie zaznacz pola wyboru Włącz stronicowanie i Włącz edycję ze znacznika inteligentnego DetailsView s.

> [!NOTE]
> Jeśli nie widzisz opcji Włącz edytowanie w tagu inteligentnym mapy inteligentnej, ponieważ metoda `Update()` ObjectDataSource s nie została zamapowana na metodę `SuppliersBLL` klasy s `UpdateSupplierAddress`. Poczekaj chwilę na odwrócić i wprowadź tę zmianę konfiguracji, po której opcja Włącz edycję powinna pojawić się w tagu inteligentnym DetailsView s.

Ponieważ metoda `UpdateSupplierAddress` `SuppliersBLL` Class akceptuje tylko cztery parametry-`supplierID`, `address`, `city`i `country` — zmodyfikuj s BoundFields, tak aby `CompanyName` i `Phone` BoundFields są tylko do odczytu. Dodatkowo Usuń `SupplierID` BoundField. Na koniec `AllSuppliersDataSource` element ObjectDataSource ma obecnie Właściwość `OldValuesParameterFormatString` ustawioną na `original_{0}`. Poświęć chwilę na całkowite usunięcie tego ustawienia właściwości z składni deklaratywnej lub ustaw ją na wartość domyślną, `{0}`.

Po skonfigurowaniu `SupplierDetails` DetailsView i `AllSuppliersDataSource` elementu ObjectDataSource będziemy mieć następujące deklaratywne znaczniki:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

W tym momencie można stronicować dane w widoku DetailsView oraz zaktualizować wybrane informacje o adresie dostawcy, niezależnie od wyboru dokonanego w `Suppliers` DropDownList (zobacz rysunek 6).

[![można przeglądać informacji o dostawcach i zaktualizować ich adresy](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Ilustracja 6**. wszystkie informacje o dostawcach mogą być wyświetlane, a jego adres został zaktualizowany ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))

## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Krok 3. Wyświetlanie tylko wybranych informacji o dostawcy

Na naszej stronie są obecnie wyświetlane informacje dla wszystkich dostawców, niezależnie od tego, czy dany dostawca został wybrany z `Suppliers` DropDownList. Aby wyświetlić tylko informacje o dostawcy dla wybranego dostawcy, należy dodać kolejny element ObjectDataSource do naszej strony, który pobiera informacje o konkretnym dostawcy.

Dodaj nowy element ObjectDataSource do strony, nadając mu nazwę `SingleSupplierDataSource`. W tagu inteligentnym kliknij link Konfiguruj źródło danych i użyj metody `GetSupplierBySupplierID(supplierID)` `SuppliersBLL` Class s. Podobnie jak w przypadku elementu `AllSuppliersDataSource` ObjectDataSource, należy mieć metodę `SingleSupplierDataSource` ObjectDataSource s `Update()` zamapowana do metody `SuppliersBLL` klasy s `UpdateSupplierAddress`.

[![skonfigurować element ObjectDataSource SingleSupplierDataSource do używania metody GetSupplierBySupplierID (IDDostawcy)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Rysunek 7**. Konfigurowanie `SingleSupplierDataSource` elementu ObjectDataSource do używania metody `GetSupplierBySupplierID(supplierID)` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))

Następnie ponownie zostanie wyświetlony monit o określenie źródła parametrów dla `GetSupplierBySupplierID(supplierID)` Metoda s `supplierID` parametr wejściowy. Ponieważ chcemy wyświetlić informacje dla dostawcy wybranego z DropDownList, użyj właściwości `Suppliers` DropDownList s `SelectedValue` jako źródła parametru.

[![używać dostawców DropDownList jako źródła parametrów IDDostawcy](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Ilustracja 8**. Użyj `Suppliers` DropDownList jako źródła parametrów `supplierID` (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))

Nawet po dodaniu drugiego elementu ObjectDataSource formant DetailsView jest obecnie skonfigurowany tak, aby zawsze używał `AllSuppliersDataSource` ObjectDataSource. Musimy dodać logikę, aby dostosować źródło danych używane w widoku DetailsView w zależności od wybranego `Suppliers` elementu DropDownList. Aby to osiągnąć, Utwórz procedurę obsługi zdarzeń `SelectedIndexChanged` dla dostawców DropDownList. Można to łatwo utworzyć przez dwukrotne kliknięcie DropDownList w projektancie. Ten program obsługi zdarzeń musi określić źródło danych, które ma być używane, i musi ponownie powiązać dane z tym DetailsView. Jest to realizowane przy użyciu następującego kodu:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Procedura obsługi zdarzeń rozpoczyna się od określenia, czy wybrano opcję "Pokaż/Edytuj wszystkie dostawcy". Jeśli tak, ustawia `SupplierDetails` widoku DetailsView `DataSourceID`, aby `AllSuppliersDataSource` i zwraca użytkownika do pierwszego rekordu w zestawie dostawców przez ustawienie właściwości `PageIndex` na 0. Jeśli jednak użytkownik wybrał określonego dostawcę z DropDownList, `DataSourceID` DetailsView s zostanie przypisany do `SingleSuppliersDataSource`. Bez względu na to, jakie źródło danych jest używane, tryb `SuppliersDetails` jest przywracany do trybu tylko do odczytu, a dane są ponownie powiązane z DetailsView przez wywołanie metody `SuppliersDetails` Control s `DataBind()`.

Po zastosowaniu tego programu obsługi zdarzeń kontrolka DetailsView wyświetla teraz wybranego dostawcę, chyba że wybrano opcję "Pokaż/Edytuj wszystkie dostawcy", w tym przypadku wszyscy dostawcy mogą być wyświetlani za pomocą interfejsu stronicowania. Na rysunku nr 9 zostanie wyświetlona strona z wybraną opcją "Pokaż/Edytuj wszystkie dostawcy"; Należy zauważyć, że interfejs stronicowania jest obecny, umożliwiając użytkownikowi odwiedzenie i zaktualizowanie dowolnego dostawcy. Na rysunku nr 10 przedstawiono stronę z wybranym dostawcą ma Maison. W tym przypadku tylko ma informacje o Maison s.

[![wszystkie informacje o dostawcach mogą być wyświetlane i edytowane.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Ilustracja 9**. Wyświetlanie i edytowanie wszystkich informacji o dostawcach ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))

[![można wyświetlać i edytować tylko wybrane informacje dostawcy.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Rysunek 10**. Wyświetlanie i edytowanie tylko wybranych informacji o dostawcy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))

> [!NOTE]
> Dla tego samouczka `EnableViewState` kontrolki DropDownList i DetailsView muszą mieć ustawioną wartość `true` (domyślnie), ponieważ zmiany właściwości DropDownList s `SelectedIndex` i DetailsView s `DataSourceID` muszą być zapamiętywane na stronach ogłaszania zwrotnego.

## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Krok 4. Tworzenie listy produktów dostawców w edytowalnym widoku GridView

W przypadku ukończenia widoku DetailsView następnym krokiem jest uwzględnienie edytowalnego widoku GridView, który wyświetla listę produktów dostarczonych przez wybranego dostawcę. Ten element GridView powinien zezwalać na edycję tylko `ProductName` i `QuantityPerUnit` pól. Ponadto, jeśli użytkownik odwiedzający stronę pochodzi od określonego dostawcy, powinien zezwalać tylko na aktualizacje tych produktów, które *nie* zostały wycofane. Aby to osiągnąć, należy najpierw dodać Przeciążenie metody `ProductsBLL` klasy s `UpdateProducts`, która przyjmuje jako dane wejściowe tylko `ProductID`, `ProductName`i `QuantityPerUnit`. W tej chwili omawiamy ten proces wcześniej w wielu samouczkach, dlatego poinformuj o tym tutaj kod, który należy dodać do `ProductsBLL`:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Po utworzeniu tego przeciążenia będziemy ponownie gotowy dodać formant GridView i skojarzony z nim element ObjectDataSource. Dodaj nowy element GridView do strony, ustaw jej Właściwość `ID` na `ProductsBySupplier`i skonfiguruj ją tak, aby korzystała z nowego elementu ObjectDataSource o nazwie `ProductsBySupplierDataSource`. Ponieważ ten widok GridView ma wyświetlać te produkty według wybranego dostawcy, użyj metody `GetProductsBySupplierID(supplierID)` `ProductsBLL` Class. Zamapuj również metodę `Update()` na nowe Przeciążenie `UpdateProduct`, który właśnie został utworzony.

[![skonfigurować element ObjectDataSource do używania właśnie utworzonego przeciążenia UpdateProduct](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Ilustracja 11**. Konfigurowanie elementu ObjectDataSource do używania właśnie utworzonego przeciążenia `UpdateProduct` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))

Zostanie wyświetlony monit o wybranie źródła parametrów dla `GetProductsBySupplierID(supplierID)` metody s `supplierID` parametr wejściowy. Ze względu na to, że chcemy wyświetlić produkty dla dostawcy wybranego w widoku DetailsView, użyj właściwości `SuppliersDetails` DetailsView formantu s `SelectedValue` jako źródła parametru.

[![użyć właściwości SuppliersDetails widoku DetailsView s SelectedValue jako źródła parametru](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Ilustracja 12**. użyj właściwości `SuppliersDetails` DetailsView s `SelectedValue` jako źródła parametru ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))

Wróć do widoku GridView, Usuń wszystkie pola GridView z wyjątkiem `ProductName`, `QuantityPerUnit`i `Discontinued`, zaznaczając `Discontinued` CheckBoxField jako tylko do odczytu. Sprawdź również opcję Włącz edytowanie w tagu inteligentnym GridView. Po wprowadzeniu tych zmian znaczniki deklaratywne dla elementu GridView i ObjectDataSource powinny wyglądać podobnie do następujących:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Tak jak w przypadku poprzednich ObjectDataSourceów, ta właściwość s `OldValuesParameterFormatString` jest ustawiona na `original_{0}`, co spowoduje problemy podczas próby zaktualizowania nazwy produktu lub ilości na jednostkę. Całkowicie Usuń tę właściwość ze składni deklaratywnej lub ustaw ją jako domyślną, `{0}`.

Po ukończeniu tej konfiguracji nasza strona zawiera teraz produkty dostarczone przez dostawcę wybranego w widoku GridView (patrz rysunek 13). Obecnie można aktualizować *wszystkie* nazwy produktów lub ilości na jednostkę. Należy jednak zaktualizować naszą logikę strony, aby zapewnić, że funkcje te będą zabronione dla nieprawidłowych produktów dla użytkowników skojarzonych z określonym dostawcą. Zajmiemy się tym ostatnim elementem w kroku 5.

[![są wyświetlane produkty dostarczone przez wybranego dostawcę](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Ilustracja 13**. wyświetlane są produkty dostarczone przez wybranego dostawcę ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))

> [!NOTE]
> Dodanie tego edytowalnego widoku GridView `Suppliers` DropDownList s `SelectedIndexChanged` obsługi zdarzeń należy zaktualizować, aby przywrócić widok GridView do stanu tylko do odczytu. W przeciwnym razie, jeśli w trakcie edytowania informacji o produkcie zostanie wybrany inny dostawca, będzie również można edytować odpowiedni indeks w widoku GridView dla nowego dostawcy. Aby tego uniknąć, po prostu ustaw właściwość GridView s `EditIndex` na `-1` w programie obsługi zdarzeń `SelectedIndexChanged`.

Ponadto należy odwołać się, że stan widoku GridView jest włączony (domyślne zachowanie). Jeśli ustawisz właściwość GridView s `EnableViewState` na `false`, zostanie uruchomione ryzyko przypadkowego usunięcia lub edycji rekordów przez współbieżnych użytkowników. Zobacz [Ostrzeżenie: problem współbieżności z elementem ASP.NET 2,0 gridviews/DetailsView/FormView, który obsługuje edytowanie i/lub usuwanie stanu widoku,](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Krok 5. nie Zezwalaj na edytowanie nieprawidłowych produktów, gdy nie wybrano Pokaż/Edytuj wszystkich dostawców

`ProductsBySupplier` GridView jest w pełni funkcjonalny, obecnie przyznaje zbyt dużo dostępu do tych użytkowników, którzy pochodzą z danego dostawcy. Zgodnie z naszymi regułami biznesowymi użytkownicy nie powinni być w stanie aktualizować wycofanych produktów. Aby to wymusić, można ukryć (lub wyłączyć) przycisk Edytuj w tych wierszach GridView z nieprawidłowymi produktami, gdy strona jest odwiedzana przez użytkownika od dostawcy.

Utwórz procedurę obsługi zdarzeń dla zdarzenia `RowDataBound` GridView. W ramach tego programu obsługi zdarzeń musimy określić, czy użytkownik jest skojarzony z konkretnym dostawcą, który w tym samouczku może zostać określony przez sprawdzenie właściwości dostawcy DropDownList s `SelectedValue` — jeśli jest to wartość inna niż-1, wówczas użytkownik jest skojarzony z określonym dostawcą. W przypadku takich użytkowników należy określić, czy produkt jest nieobsługiwany. Możemy pobrać odwołanie do rzeczywistego wystąpienia `ProductRow` powiązanego z wierszem GridView za pośrednictwem właściwości `e.Row.DataItem`, zgodnie z opisem w temacie [*Wyświetlanie podsumowania informacji zawartych w samouczku w stopce widoku GridView*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) . Jeśli produkt zostanie wycofany, możemy pobrać informacje programistyczne do przycisku Edytuj w widoku GridView s CommandField przy użyciu technik omówionych w poprzednim samouczku, co powoduje [*dodanie potwierdzenia po stronie klienta podczas usuwania*](adding-client-side-confirmation-when-deleting-vb.md). Gdy mamy odwołanie, możemy go ukryć lub wyłączyć.

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

W przypadku korzystania z tej procedury obsługi zdarzeń podczas odwiedzania tej strony jako użytkownik z określonego dostawcy nie można edytować produktów, które nie są dostępne, ponieważ przycisk Edytuj jest ukryty dla tych produktów. Na przykład Chef Anton s Gumbo mix to wycofany produkt dla nowego dostawcy Orleans Cajun. Podczas odwiedzania strony dla tego konkretnego dostawcy przycisk Edytuj dla tego produktu jest ukryty przed wzrokiem (zobacz rysunek 14). Jednak podczas odwiedzania przy użyciu opcji "Pokaż/Edytuj wszyscy dostawcy" przycisk Edytuj jest dostępny (zobacz rysunek 15).

[![dla użytkowników specyficznych dla dostawcy przycisk Edytuj dla Chef Anton s Gumbo mix jest ukryty](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Ilustracja 14**. dla użytkowników specyficznych dla dostawcy przycisk Edytuj dla Chef Anton s Gumbo mix jest ukryty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))

[![do wyświetlania/edycji wszystkich użytkowników dostawcy, wyświetlany jest przycisk Edytuj dla Chef Anton s Gumbo mix](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Ilustracja 15**. Aby wyświetlić/edytować wszystkich użytkowników dostawcy, wyświetlany jest przycisk Edytuj dla Chef Anton s Gumbo mix ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))

## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Sprawdzanie praw dostępu w warstwie logiki biznesowej

W tym samouczku strona ASP.NET obsługuje całą logikę w odniesieniu do informacji, które użytkownik może zobaczyć i jakie produkty mogą zostać zaktualizowane. W idealnym przypadku ta logika również będzie obecna w warstwie logiki biznesowej. Na przykład Metoda `SuppliersBLL` klasy `GetSuppliers()` s (zwracająca wszystkich dostawców) może obejmować sprawdzenie, czy aktualnie zalogowany użytkownik *nie* jest skojarzony z określonym dostawcą. Podobnie Metoda `UpdateSupplierAddress` może obejmować kontrolę, aby upewnić się, że aktualnie zalogowany użytkownik pracował dla naszej firmy (i w związku z tym może zaktualizować wszystkie informacje o adresie dostawcy) lub jest skojarzony z dostawcą, którego dane są aktualizowane.

W tym miejscu nie zostały uwzględnione takie LOGIKI BIZNESOWEJe, ponieważ w naszym samouczku prawa użytkownika są określane przez DropDownList na stronie, której klasy LOGIKI biznesowej nie mogą uzyskać dostępu. W przypadku korzystania z systemu członkostwa lub jednego z gotowych do użycia schematów uwierzytelniania udostępnianych przez ASP.NET (takich jak uwierzytelnianie systemu Windows) aktualnie zalogowany użytkownik i informacje o rolach są dostępne z LOGIKI biznesowej, co sprawia, że taki dostęp możliwe jest sprawdzenie praw zarówno w przypadku prezentacji, jak i warstw LOGIKI biznesowej.

## <a name="summary"></a>Podsumowanie

Większość lokacji, które zapewniają konta użytkowników, musi dostosować interfejs modyfikacji danych na podstawie zalogowanego użytkownika. Użytkownicy administracyjni mogą mieć możliwość usuwania i edytowania dowolnego rekordu, podczas gdy użytkownicy niebędący administratorami mogą być ograniczeni wyłącznie do aktualizowania lub usuwania utworzonych przez siebie rekordów. Niezależnie od tego, co może być scenariuszem, klasy kontrolki sieci Web, elementy ObjectDataSource i warstwy logiki biznesowej można rozszerzyć w celu dodania lub odmowy pewnych funkcji na podstawie zalogowanego użytkownika. W tym samouczku przedstawiono sposób ograniczania dostępnych i edytowalnych danych w zależności od tego, czy użytkownik został skojarzony z konkretnym dostawcą, czy też pracował nad firmą.

W tym samouczku zakończymy badanie wstawiania, aktualizowania i usuwania danych za pomocą kontrolek GridView, DetailsView i FormView. Począwszy od następnego samouczka, będziemy zachęcamy do dodawania obsługi stronicowania i sortowania.

Szczęśliwe programowanie!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Ubiegł](adding-client-side-confirmation-when-deleting-vb.md)

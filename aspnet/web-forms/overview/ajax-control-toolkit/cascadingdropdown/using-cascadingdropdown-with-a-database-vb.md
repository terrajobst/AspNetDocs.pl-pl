---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Używanie kontrolki CascadingDropDown z bazą danych (VB) | Microsoft Docs
author: wenz
description: Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4482aa18c4446ec8f5f160c423008398ea2e1d0d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535918"
---
# <a name="using-cascadingdropdown-with-a-database-vb"></a>Używanie kontrolki CascadingDropDown z bazą danych (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. Aby ta funkcja działała, należy utworzyć specjalną usługę sieci Web.

## <a name="overview"></a>Omówienie

Kontrolka kontrolki CascadingDropDown w zestawie narzędzi AJAX Control rozszerza kontrolkę DropDownList, tak aby zmiany w jednej DropDownList ładowały skojarzone wartości w innym DropDownList. (Na przykład jedna lista zawiera listę stanów USA, a następna lista jest wypełniana głównymi miastami tego stanu). Aby ta funkcja działała, należy utworzyć specjalną usługę sieci Web.

## <a name="steps"></a>Kroki

Najpierw wymagane jest źródło danych. Ten przykład używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition. Baza danych jest opcjonalną częścią instalacji programu Visual Studio (w tym Express Edition) i jest również dostępna jako oddzielne pobieranie w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Baza danych AdventureWorks jest częścią przykładów SQL Server 2005 i przykładowych baz danych (Pobierz w [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Najprostszym sposobem ustawienia bazy danych jest użycie Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = EN](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie pliku bazy danych `AdventureWorks.mdf`.

W tym przykładzie przyjęto założenie, że wystąpienie SQL Server 2005 Express Edition jest nazywane `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci Web; jest to również konfiguracja domyślna. Jeśli konfiguracja jest inna, należy dostosować informacje o połączeniu dla bazy danych.

Aby uaktywnić funkcje ASP.NET AJAX i zestaw narzędzi sterowania, formant `ScriptManager` musi być umieszczony w dowolnym miejscu na stronie (ale w &lt;`form`&gt; elementu):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

W następnym kroku wymagane są dwa formanty DropDownList. W tym przykładzie używamy informacji o dostawcy i kontakcie firmy AdventureWorks, dlatego tworzymy jedną listę dla dostępnych dostawców i jeden dla dostępnych kontaktów:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Następnie do strony należy dodać dwa rozszerzalne kontrolki CascadingDropDown. Jeden wypełnia pierwszą listę (dostawcy), a druga druga lista (kontakty). Następujące atrybuty muszą być ustawione:

- `ServicePath`: adres URL usługi sieci Web dostarczającej wpisy listy
- `ServiceMethod`: Metoda sieci Web dostarczająca wpisy listy
- `TargetControlID`: Identyfikator listy rozwijanej
- `Category`: informacje o kategorii, które są przesyłane do metody sieci Web w przypadku wywołania
- `PromptText`: tekst wyświetlany podczas asynchronicznego ładowania danych listy z serwera
- `ParentControlID`: (opcjonalnie) lista rozwijana nadrzędna, która wyzwala ładowanie bieżącej listy

W zależności od używanego języka programowania, nazwa usługi sieci Web, w której wprowadzono zmiany, ale wszystkie inne wartości atrybutów są takie same. Oto element kontrolki CascadingDropDown dla pierwszej listy rozwijanej:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Rozszerzalne kontrolki dla drugiej listy muszą ustawić atrybut `ParentControlID` tak, aby wybranie pozycji na liście dostawców wyzwala załadowanie skojarzonych elementów na liście kontaktów.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Rzeczywista czynność jest wykonywana w usłudze sieci Web, która została skonfigurowana w następujący sposób. Należy zauważyć, że atrybut `[ScriptService]` jest używany, w przeciwnym razie ASP.NET AJAX nie może utworzyć serwera proxy języka JavaScript w celu uzyskania dostępu do metod sieci Web z kodu skryptu po stronie klienta.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Sygnatura metod sieci Web wywoływanych przez kontrolki CascadingDropDown jest następująca:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Dlatego wartość zwracana musi być tablicą typu `CascadingDropDownNameValue`, która jest definiowana przez zestaw narzędzi sterowania. Metoda `GetVendors()` jest bardzo łatwa do zaimplementowania: kod nawiązuje połączenie z bazą danych AdventureWorks i wysyła zapytanie do pierwszych 25 dostawców. Pierwszy parametr w konstruktorze `CascadingDropDownNameValue` jest podpisem wpisu listy, a druga wartość (atrybut wartości w &lt;HTML `option`elementu &gt;). Oto kod:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Pobieranie skojarzonych kontaktów dla dostawcy (nazwa metody: `GetContactsForVendor()`) to bit trickier. Najpierw należy określić dostawcę, który został wybrany na pierwszej liście rozwijanej. Zestaw narzędzi formantu definiuje metodę pomocnika dla tego zadania: Metoda `ParseKnownCategoryValuesString()` zwraca element `StringDictionary` z danymi listy rozwijanej:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Ze względów bezpieczeństwa te dane muszą zostać sprawdzone jako pierwsze. Jeśli więc istnieje wpis dostawcy (ponieważ właściwość `Category` pierwszego elementu kontrolki CascadingDropDown jest ustawiona na `"Vendor"`), można pobrać identyfikator wybranego dostawcy:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Pozostała część metody jest stosunkowo prosta, a następnie. Identyfikator dostawcy jest używany jako parametr zapytania SQL, które pobiera wszystkie skojarzone kontakty dla tego dostawcy. Po ponownym uruchomieniu Metoda zwraca tablicę typu `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Załaduj stronę ASP.NET i po krótkim czasie, gdy lista dostawców jest wypełniona 25 wpisami. Wybierz jedną pozycję i zwróć uwagę na to, jak druga lista rozwijana jest wypełniana danymi.

[![Pierwsza lista jest wypełniana automatycznie](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Pierwsza lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image3.png))

[![druga lista jest wypełniana zgodnie z wyborem na pierwszej liście](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Druga lista jest wypełniana zgodnie z wyborem na pierwszej liście ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](filling-a-list-using-cascadingdropdown-vb.md)
> [dalej](presetting-list-entries-with-cascadingdropdown-vb.md)

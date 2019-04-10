---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Używanie kontrolki CascadingDropDown z bazą danych (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList, tak aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: d0b6f8651e327cf9ad2a3051edd323efba4f64fc
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418732"
---
# <a name="using-cascadingdropdown-with-a-database-vb"></a><span data-ttu-id="43111-103">Używanie kontrolki CascadingDropDown z bazą danych (VB)</span><span class="sxs-lookup"><span data-stu-id="43111-103">Using CascadingDropDown with a Database (VB)</span></span>

<span data-ttu-id="43111-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="43111-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="43111-105">[Pobierz program Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="43111-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span></span>

> <span data-ttu-id="43111-106">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="43111-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="43111-107">Aby to zrobić można utworzyć usługi sieci web specjalne.</span><span class="sxs-lookup"><span data-stu-id="43111-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="43111-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="43111-108">Overview</span></span>

<span data-ttu-id="43111-109">Kontrolki CascadingDropDown w zestawu narzędzi AJAX Control Toolkit rozszerza formant DropDownList tak, aby zmiany w jednej metody DropDownList ładowania skojarzone wartości w innej metody DropDownList.</span><span class="sxs-lookup"><span data-stu-id="43111-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="43111-110">(Na przykład jedna lista zawiera listę stanów USA oraz następnej liście następnie jest wypełniany głównych miast, w tym stanie.) Aby to zrobić można utworzyć usługi sieci web specjalne.</span><span class="sxs-lookup"><span data-stu-id="43111-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="43111-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="43111-111">Steps</span></span>

<span data-ttu-id="43111-112">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="43111-112">First of all, a data source is required.</span></span> <span data-ttu-id="43111-113">Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="43111-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="43111-114">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="43111-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="43111-115">Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="43111-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="43111-116">Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43111-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="43111-117">W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="43111-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="43111-118">Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="43111-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="43111-119">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu &lt; `form` &gt; elementu):</span><span class="sxs-lookup"><span data-stu-id="43111-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

<span data-ttu-id="43111-120">W następnym kroku dwóch kontrolek DropDownList są wymagane.</span><span class="sxs-lookup"><span data-stu-id="43111-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="43111-121">W tym przykładzie używamy dostawcy i skontaktuj się z informacji z AdventureWorks, dlatego utworzymy jedną listę dla dostępnych dostawców i jeden dla dostępnych kontaktów:</span><span class="sxs-lookup"><span data-stu-id="43111-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

<span data-ttu-id="43111-122">Następnie należy dodać dwa rozszerzeń kontrolki CascadingDropDown do strony.</span><span class="sxs-lookup"><span data-stu-id="43111-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="43111-123">Wstawia jedną z pierwszej listy (dostawcami), a druga wypełnia na drugiej liście (kontakty).</span><span class="sxs-lookup"><span data-stu-id="43111-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="43111-124">Należy określić następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="43111-124">The following attributes must be set:</span></span>

- `ServicePath`<span data-ttu-id="43111-125">: Adres URL usługi sieci web, zapewniając pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="43111-125">: URL of a web service delivering the list entries</span></span>
- `ServiceMethod`<span data-ttu-id="43111-126">: Metoda sieci Web, zapewniając pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="43111-126">: Web method delivering the list entries</span></span>
- `TargetControlID`<span data-ttu-id="43111-127">: Identyfikator listy rozwijanej</span><span class="sxs-lookup"><span data-stu-id="43111-127">: ID of the dropdown list</span></span>
- `Category`<span data-ttu-id="43111-128">: Informacje o kategorii, przesłane do metody sieci web, gdy zostanie wywołana</span><span class="sxs-lookup"><span data-stu-id="43111-128">: Category information that is submitted to the web method when called</span></span>
- `PromptText`<span data-ttu-id="43111-129">: Tekst wyświetlany, gdy asynchronicznie ładowania listy danych z serwera</span><span class="sxs-lookup"><span data-stu-id="43111-129">: Text displayed when asynchronously loading list data from the server</span></span>
- `ParentControlID`<span data-ttu-id="43111-130">: (opcjonalnie) tego ładowanie wyzwalaczy bieżącą listę liście rozwijanej nadrzędnego</span><span class="sxs-lookup"><span data-stu-id="43111-130">: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="43111-131">W zależności od języka programowania, używany zmienia nazwę danej usługi sieci web, ale inne wartości atrybutów są takie same.</span><span class="sxs-lookup"><span data-stu-id="43111-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="43111-132">Oto elementu kontrolki CascadingDropDown z pierwszej listy rozwijanej:</span><span class="sxs-lookup"><span data-stu-id="43111-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

<span data-ttu-id="43111-133">Kontrolek na drugiej liście, należy ustawić `ParentControlID` atrybutu, tak aby zaznaczając daną pozycję w wyzwala listy dostawców załadowanie skojarzonych elementów na liście kontaktów.</span><span class="sxs-lookup"><span data-stu-id="43111-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

<span data-ttu-id="43111-134">Następnie rzeczywista praca odbywa się w usłudze sieci web jest skonfigurowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="43111-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="43111-135">Należy pamiętać, że `[ScriptService]` atrybut jest używany, w przeciwnym razie ASP.NET AJAX, nie można utworzyć serwera proxy JavaScript dostęp do metody sieci web z poziomu kodu skryptu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="43111-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

<span data-ttu-id="43111-136">Podpis metody sieci web o nazwie przez kontrolki CascadingDropDown jest następująca:</span><span class="sxs-lookup"><span data-stu-id="43111-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

<span data-ttu-id="43111-137">Dlatego zwracana wartość musi być tablicą typu `CascadingDropDownNameValue` który jest definiowany przez zestaw narzędzi kontroli.</span><span class="sxs-lookup"><span data-stu-id="43111-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="43111-138">`GetVendors()` Metoda jest dość łatwe do wdrożenia: Kod nawiązanie połączenia z bazy danych AdventureWorks i zapytania pierwsze 25 dostawców.</span><span class="sxs-lookup"><span data-stu-id="43111-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="43111-139">Pierwszy parametr w `CascadingDropDownNameValue` Konstruktor jest podpis wpisu listy, a druga wartość (wartość atrybutu w formacie HTML w &lt; `option` &gt; elementu).</span><span class="sxs-lookup"><span data-stu-id="43111-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="43111-140">Oto kod:</span><span class="sxs-lookup"><span data-stu-id="43111-140">Here is the code:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

<span data-ttu-id="43111-141">Wprowadzenie skojarzone kontakty dla dostawcy (Nazwa metody: `GetContactsForVendor()`) jest nieco trudniejszy.</span><span class="sxs-lookup"><span data-stu-id="43111-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="43111-142">Po pierwsze można określić dostawcę, który został wybrany z pierwszej listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="43111-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="43111-143">Zestaw narzędzi kontroli definiuje metodę pomocniczą dla tego zadania: `ParseKnownCategoryValuesString()` Metoda zwraca `StringDictionary` element z listy rozwijanej danych:</span><span class="sxs-lookup"><span data-stu-id="43111-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

<span data-ttu-id="43111-144">Ze względów bezpieczeństwa te dane muszą najpierw zweryfikowana.</span><span class="sxs-lookup"><span data-stu-id="43111-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="43111-145">Zatem jeśli zapis dostawcy (ponieważ `Category` pierwszego elementu kontrolki CascadingDropDown zostaje ustalona `"Vendor"`), można pobrać Identyfikatora wybranego dostawcy:</span><span class="sxs-lookup"><span data-stu-id="43111-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

<span data-ttu-id="43111-146">Pozostała część metody jest dość proste, następnie.</span><span class="sxs-lookup"><span data-stu-id="43111-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="43111-147">Identyfikator dostawcy jest używany jako parametr dla zapytania SQL, który pobiera wszystkie skojarzone kontakty dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="43111-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="43111-148">Jeszcze raz, metoda zwraca tablicę typu `CascadingDropDownNameValue`.</span><span class="sxs-lookup"><span data-stu-id="43111-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

<span data-ttu-id="43111-149">Ładowanie strony ASP.NET, a po krótkiej chwili lista dostawców jest wypełniany 25 wpisów.</span><span class="sxs-lookup"><span data-stu-id="43111-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="43111-150">Wybierz jeden wpis i zwróć uwagę, jak na drugiej liście rozwijanej jest wypełniany danymi.</span><span class="sxs-lookup"><span data-stu-id="43111-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


[![T<span data-ttu-id="43111-151">Pierwsza lista he jest wypełniane automatycznie]</span><span class="sxs-lookup"><span data-stu-id="43111-151">he first list is filled automatically]</span></span>(using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

<span data-ttu-id="43111-152">Pierwsza lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="43111-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span></span>


[![T<span data-ttu-id="43111-153">druga lista he jest wypełniana zgodnie z wyborem na pierwszej liście]</span><span class="sxs-lookup"><span data-stu-id="43111-153">he second list is filled according to the selection in the first list]</span></span>(using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

<span data-ttu-id="43111-154">Druga lista jest wypełniana zgodnie z wyborem na pierwszej liście ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="43111-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="43111-155">[Poprzednie](filling-a-list-using-cascadingdropdown-vb.md)
> [dalej](presetting-list-entries-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="43111-155">[Previous](filling-a-list-using-cascadingdropdown-vb.md)
[Next](presetting-list-entries-with-cascadingdropdown-vb.md)</span></span>

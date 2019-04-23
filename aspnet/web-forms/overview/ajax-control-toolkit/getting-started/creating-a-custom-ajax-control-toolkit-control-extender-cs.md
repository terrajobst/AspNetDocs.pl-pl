---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Tworzenie niestandardowego interfejsu AJAX formantu rozszerzenia kontrolki Toolkit (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Niestandardowe rozszerzenia umożliwiają dostosowywanie i rozszerzanie możliwości kontrolek ASP.NET bez konieczności tworzenia nowych klas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 4428ef0a6cec4c348bc48d069b990798508c21d4
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59391666"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="912b7-103">Tworzenie niestandardowego rozszerzenia kontrolki zestawu narzędzi AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="912b7-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>

<span data-ttu-id="912b7-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="912b7-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="912b7-105">Niestandardowe rozszerzenia umożliwiają dostosowywanie i rozszerzanie możliwości kontrolek ASP.NET bez konieczności tworzenia nowych klas.</span><span class="sxs-lookup"><span data-stu-id="912b7-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="912b7-106">W tym samouczku dowiesz się, jak utworzyć niestandardowego rozszerzenia kontrolki zestawu narzędzi AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="912b7-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="912b7-107">Utworzymy prostą, ale przydatne, nowych rozszerzeń, który zmienia stan przycisku z wyłączonego na włączony podczas wpisywania tekstu w polu tekstowym.</span><span class="sxs-lookup"><span data-stu-id="912b7-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="912b7-108">Po przeczytaniu tego samouczka, można rozszerzyć z zestawu narzędzi AJAX ASP.NET przy użyciu własnych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="912b7-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="912b7-109">Można tworzyć niestandardowych kontrolek przy użyciu programu Visual Studio lub Visual Web Developer (Upewnij się, że masz najnowszą wersję programu Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="912b7-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="912b7-110">Omówienie rozszerzeń DisabledButton</span><span class="sxs-lookup"><span data-stu-id="912b7-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="912b7-111">Nasze nowe rozszerzenie kontrolki nosi nazwę rozszerzenia DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="912b7-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="912b7-112">Tego rozszerzenia ma trzy właściwości:</span><span class="sxs-lookup"><span data-stu-id="912b7-112">This extender will have three properties:</span></span>

- <span data-ttu-id="912b7-113">TargetControlID — pole tekstowe, które rozszerza formant.</span><span class="sxs-lookup"><span data-stu-id="912b7-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="912b7-114">TargetButtonIID — przycisk, który zostało wyłączone lub włączone.</span><span class="sxs-lookup"><span data-stu-id="912b7-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="912b7-115">DisabledText — tekst, który początkowo jest wyświetlana na przycisku.</span><span class="sxs-lookup"><span data-stu-id="912b7-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="912b7-116">Gdy zaczniesz pisać, przycisk powoduje wyświetlenie wartości właściwości tekst przycisku.</span><span class="sxs-lookup"><span data-stu-id="912b7-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="912b7-117">Utworzenie punktu zaczepienia extender DisabledButton do kontrolki pola tekstowego i przycisku.</span><span class="sxs-lookup"><span data-stu-id="912b7-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="912b7-118">Przed wpisaniem tekstu przycisk jest wyłączony i pole tekstowe i przycisk wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="912b7-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="912b7-119">([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="912b7-120">Po rozpoczęciu wpisywania tekstu, ten przycisk jest włączony, i pole tekstowe i przycisk wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="912b7-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="912b7-121">([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="912b7-122">Aby utworzyć naszego rozszerzenia kontrolki zestawu narzędzi, musimy utworzyć trzy następujące pliki:</span><span class="sxs-lookup"><span data-stu-id="912b7-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="912b7-123">DisabledButtonExtender.cs — ten plik jest klasa sterowania po stronie serwera, który będzie zarządzanie, tworzenie urządzenia extender i umożliwiają ustawianie właściwości w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="912b7-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="912b7-124">Definiuje właściwości, które można ustawić przy użyciu urządzenia extender.</span><span class="sxs-lookup"><span data-stu-id="912b7-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="912b7-125">Te właściwości są dostępne za pośrednictwem kodu i w czasie projektowania i dopasowania właściwości zdefiniowane w pliku DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="912b7-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="912b7-126">DisabledButtonBehavior.js — Ten plik jest, gdy dodasz wszystkie logiki skryptu klienta.</span><span class="sxs-lookup"><span data-stu-id="912b7-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="912b7-127">DisabledButtonDesigner.cs — ta klasa umożliwia funkcjonalność czasu projektowania.</span><span class="sxs-lookup"><span data-stu-id="912b7-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="912b7-128">Ta klasa jest konieczne, jeśli chcesz, aby rozszerzenie kontrolki w celu poprawnego działania z Visual Studio/Visual Web Developer Designer.</span><span class="sxs-lookup"><span data-stu-id="912b7-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="912b7-129">Dlatego rozszerzenie kontrolki składa się z kontroli po stronie serwera, zachowanie po stronie klienta i po stronie serwera klasy projektanta.</span><span class="sxs-lookup"><span data-stu-id="912b7-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="912b7-130">Dowiesz się, jak utworzyć wszystkie trzy tych plików w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="912b7-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="912b7-131">Tworzenie rozszerzeń niestandardową witrynę sieci Web i projektu</span><span class="sxs-lookup"><span data-stu-id="912b7-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="912b7-132">Pierwszym krokiem jest, aby utworzyć projekt biblioteki klas i witryny sieci Web w programie Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="912b7-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="912b7-133">Firma Microsoft ll Tworzenie niestandardowych rozszerzeń w projekcie biblioteki klas i testowania niestandardowych rozszerzeń w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="912b7-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="912b7-134">Pozwól s rozpoczynać witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="912b7-134">Let�s start with the website.</span></span> <span data-ttu-id="912b7-135">Wykonaj następujące kroki, aby utworzyć witrynę sieci Web:</span><span class="sxs-lookup"><span data-stu-id="912b7-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="912b7-136">Wybierz opcję menu **plik, nową witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="912b7-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="912b7-137">Wybierz **witryny sieci Web platformy ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="912b7-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="912b7-138">Nadaj nazwę nowej witryny sieci Web *witryna "website1"*.</span><span class="sxs-lookup"><span data-stu-id="912b7-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="912b7-139">Kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="912b7-139">Click the **OK** button.</span></span>

<span data-ttu-id="912b7-140">Następnie należy utworzyć projekt biblioteki klas, który będzie zawierał kod rozszerzenia kontrolki zestawu narzędzi:</span><span class="sxs-lookup"><span data-stu-id="912b7-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="912b7-141">Wybierz opcję menu **plik i Dodaj nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="912b7-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="912b7-142">Wybierz **biblioteki klas** szablonu.</span><span class="sxs-lookup"><span data-stu-id="912b7-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="912b7-143">Nazwij nową bibliotekę klas o nazwie **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="912b7-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="912b7-144">Kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="912b7-144">Click the **OK** button.</span></span>

<span data-ttu-id="912b7-145">Po wykonaniu tych kroków, z okna Eksploratora rozwiązań powinien wyglądać jak rysunek 1.</span><span class="sxs-lookup"><span data-stu-id="912b7-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="912b7-146">[![Rozwiązanie z witryny sieci Web i klasy projektu biblioteki](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="912b7-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="912b7-147">**Rysunek 01**: Rozwiązanie z witryny sieci Web i klasy projektu biblioteki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="912b7-148">Następnie należy dodać wszystkich niezbędnych odwołań do zestawu do projektu biblioteki klas:</span><span class="sxs-lookup"><span data-stu-id="912b7-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="912b7-149">Kliknij prawym przyciskiem myszy projekt CustomExtenders i wybierz opcję menu **Dodaj odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="912b7-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="912b7-150">Wybierz kartę .NET.</span><span class="sxs-lookup"><span data-stu-id="912b7-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="912b7-151">Dodaj odwołania do następujących zestawów:</span><span class="sxs-lookup"><span data-stu-id="912b7-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="912b7-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="912b7-152">System.Web.dll</span></span>
    2. <span data-ttu-id="912b7-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="912b7-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="912b7-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="912b7-154">System.Design.dll</span></span>
    4. <span data-ttu-id="912b7-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="912b7-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="912b7-156">Wybierz kartę przeglądania.</span><span class="sxs-lookup"><span data-stu-id="912b7-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="912b7-157">Dodaj odwołanie do zestawu AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="912b7-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="912b7-158">Ten zestaw znajduje się w folderze, w której pobrano zestawu narzędzi AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="912b7-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="912b7-159">Po wykonaniu tych kroków, folder odwołania do projektu biblioteki klas powinien wyglądać jak rysunek 2.</span><span class="sxs-lookup"><span data-stu-id="912b7-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="912b7-160">[![Folder odwołania z odwołaniami wymagane](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="912b7-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="912b7-161">**Rysunek 02**: Folder odwołania z odwołaniami wymagane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="912b7-162">Tworzenie rozszerzeń kontrolki niestandardowej</span><span class="sxs-lookup"><span data-stu-id="912b7-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="912b7-163">Teraz, gdy mamy już naszej bibliotece klasy, Zaczniemy budowanie nasze rozszerzenie formantu.</span><span class="sxs-lookup"><span data-stu-id="912b7-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="912b7-164">Pozwól s rozpoczynać kości bare klasy niestandardowego rozszerzenia kontrolki (patrz lista 1).</span><span class="sxs-lookup"><span data-stu-id="912b7-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="912b7-165">**Wyświetlanie listy 1 - MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="912b7-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="912b7-166">Istnieje kilka rzeczy, które można zauważyć, że informacje o klasie rozszerzenia kontrolki w ofercie 1.</span><span class="sxs-lookup"><span data-stu-id="912b7-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="912b7-167">Najpierw zwróć uwagę, że klasa dziedziczy z klasy bazowej ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="912b7-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="912b7-168">Wszystkich kontrolek rozszerzeń typu AJAX Control Toolkit pochodzić z tej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="912b7-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="912b7-169">Na przykład klasa bazowa zawiera właściwość TargetID, która jest wymagana właściwość każdego rozszerzenia kontrolki zestawu narzędzi.</span><span class="sxs-lookup"><span data-stu-id="912b7-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="912b7-170">Następnie zwróć uwagę, że klasa zawiera następujące atrybuty dotyczą skrypt po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="912b7-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="912b7-171">Widok — powoduje, że plik do uwzględnienia jako zasobu osadzonego w zestawie.</span><span class="sxs-lookup"><span data-stu-id="912b7-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="912b7-172">ClientScriptResource - powoduje, że zasób skryptu do pobrania z zestawu.</span><span class="sxs-lookup"><span data-stu-id="912b7-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="912b7-173">Atrybut widok jest używany do osadzania plik MyControlBehavior.js JavaScript do zestawu podczas kompilowania rozszerzeń niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="912b7-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="912b7-174">Atrybut ClientScriptResource jest używany do pobierania skryptu MyControlBehavior.js z zestawu, stosowania niestandardowego rozszerzenia na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="912b7-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="912b7-175">Aby widok i ClientScriptResource atrybuty do pracy należy skompilować pliku JavaScript jako zasobu osadzonego.</span><span class="sxs-lookup"><span data-stu-id="912b7-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="912b7-176">Zaznacz ten plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisz wartość *zasób osadzony* do **Build Action** właściwości.</span><span class="sxs-lookup"><span data-stu-id="912b7-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="912b7-177">Zwróć uwagę, że rozszerzenie kontrolki także atrybutu element TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="912b7-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="912b7-178">Ten atrybut służy do określania typu formantu, który został rozszerzony przez rozszerzenie kontrolki.</span><span class="sxs-lookup"><span data-stu-id="912b7-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="912b7-179">W przypadku wyświetlania listy 1 rozszerzenia kontrolki zestawu narzędzi umożliwia rozszerzanie pole tekstowe.</span><span class="sxs-lookup"><span data-stu-id="912b7-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="912b7-180">Na koniec Zauważ, że niestandardowego rozszerzenia zawiera właściwość o nazwie MyProperty.</span><span class="sxs-lookup"><span data-stu-id="912b7-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="912b7-181">Właściwość jest oznaczona atrybutem ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="912b7-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="912b7-182">Metody GetPropertyValue() i SetPropertyValue() są używane do przekazywania wartości właściwości z rozszerzenia po stronie serwera kontrolki do zachowania po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="912b7-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="912b7-183">Pozwól s Przejdź dalej i zaimplementować kod dla naszego rozszerzenia DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="912b7-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="912b7-184">Kod dla tego rozszerzenia można znaleźć w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="912b7-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="912b7-185">**Wyświetlanie listy 2 - DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="912b7-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="912b7-186">Rozszerzenie DisabledButton w ofercie 2 ma dwie właściwości o nazwie TargetButtonID i DisabledText.</span><span class="sxs-lookup"><span data-stu-id="912b7-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="912b7-187">IDReferenceProperty stosowany do właściwości TargetButtonID zapobiega przypisywanie coś innego niż identyfikator kontrolki przycisku do tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="912b7-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="912b7-188">Atrybuty widok i ClientScriptResource kojarzenie zachowania klienta znajduje się w pliku o nazwie DisabledButtonBehavior.js z tego rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="912b7-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="912b7-189">Omówimy ten plik JavaScript w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="912b7-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="912b7-190">Tworzenie urządzenia Extender niestandardowe zachowanie</span><span class="sxs-lookup"><span data-stu-id="912b7-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="912b7-191">Składnik po stronie klienta rozszerzenia kontrolki zestawu narzędzi nazywa się to zachowanie.</span><span class="sxs-lookup"><span data-stu-id="912b7-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="912b7-192">Rzeczywiste logikę wyłączenie i włączenie przycisku znajduje się w zachowaniu DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="912b7-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="912b7-193">Kod JavaScript, zachowanie jest uwzględnione w ofercie 3.</span><span class="sxs-lookup"><span data-stu-id="912b7-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="912b7-194">**Wyświetlanie listy 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="912b7-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="912b7-195">Plik JavaScript w ofercie 3 zawiera klasę klienta o nazwie DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="912b7-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="912b7-196">Ta klasa, takie jak jego twin po stronie serwera zawiera dwie właściwości o nazwie TargetButtonID i Uzyskaj DisabledText, którego można uzyskiwać dostęp za pomocą\_TargetButtonID/set\_TargetButtonID i Uzyskaj\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="912b7-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="912b7-197">Metodę initialize() kojarzy keyup obsługi zdarzenia z elementem docelowym zachowania.</span><span class="sxs-lookup"><span data-stu-id="912b7-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="912b7-198">Wykonuje procedurę obsługi keyup każdorazowo wpisz literę w polu tekstowym skojarzony z tym działaniem.</span><span class="sxs-lookup"><span data-stu-id="912b7-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="912b7-199">Program obsługi keyup Włącza lub wyłącza przycisk w zależności od tego, czy zawiera dowolny tekst w TextBox skojarzonych z zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="912b7-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="912b7-200">Należy pamiętać, że należy skompilować pliku JavaScript w ofercie 3 jako zasobu osadzonego.</span><span class="sxs-lookup"><span data-stu-id="912b7-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="912b7-201">Zaznacz ten plik w oknie Eksploratora rozwiązań, otwórz arkusz właściwości i przypisz wartość *zasób osadzony* do **Build Action** właściwości (zobacz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="912b7-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="912b7-202">Ta opcja jest dostępna w programie Visual Studio i Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="912b7-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="912b7-203">[![Dodawanie pliku JavaScript jako zasobu osadzonego](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="912b7-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="912b7-204">**Rysunek 03**: Dodawanie pliku JavaScript jako zasobu osadzonego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="912b7-205">Tworzenie niestandardowego rozszerzenia projektanta</span><span class="sxs-lookup"><span data-stu-id="912b7-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="912b7-206">Istnieje jedna klasa ostatniego, która musimy utworzyć do ukończenia naszego rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="912b7-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="912b7-207">Musimy utworzyć klasy projektanta w ofercie 4.</span><span class="sxs-lookup"><span data-stu-id="912b7-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="912b7-208">Ta klasa jest wymagana do udostępnienia extender działają prawidłowo z Visual Studio/Visual Web Developer Designer.</span><span class="sxs-lookup"><span data-stu-id="912b7-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="912b7-209">**Wyświetlanie listy 4 - DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="912b7-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="912b7-210">Projektant w ofercie 4 należy skojarzyć z rozszerzeń DisabledButton atrybutem projektanta. Należy zastosować atrybut Projektant klasy DisabledButtonExtender następująco:</span><span class="sxs-lookup"><span data-stu-id="912b7-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="912b7-211">Za pomocą niestandardowego rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="912b7-211">Using the Custom Extender</span></span>

<span data-ttu-id="912b7-212">Teraz, gdy firma Microsoft została zakończona, Tworzenie rozszerzeń kontrolki DisabledButton, nadszedł czas na ten jest używany w naszej witryny sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="912b7-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="912b7-213">Najpierw musimy dodać niestandardowe rozszerzenie do przybornika.</span><span class="sxs-lookup"><span data-stu-id="912b7-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="912b7-214">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="912b7-214">Follow these steps:</span></span>

1. <span data-ttu-id="912b7-215">Kliknij dwukrotnie strony w oknie Eksploratora rozwiązań, aby otworzyć stronę ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="912b7-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="912b7-216">Kliknij prawym przyciskiem myszy przybornika, a następnie wybierz opcję menu **wybierz elementy**.</span><span class="sxs-lookup"><span data-stu-id="912b7-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="912b7-217">W oknie dialogowym Wybierz elementy paska narzędzi przejdź do zestawu CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="912b7-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="912b7-218">Kliknij przycisk **OK** przycisk, aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="912b7-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="912b7-219">Po wykonaniu tych kroków rozszerzenie kontrolki DisabledButton powinna zostać wyświetlona w przyborniku (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="912b7-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="912b7-220">[![DisabledButton w przyborniku](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="912b7-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="912b7-221">**Rysunek 04**: DisabledButton w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="912b7-222">Następnie należy utworzyć nową stronę programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="912b7-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="912b7-223">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="912b7-223">Follow these steps:</span></span>

1. <span data-ttu-id="912b7-224">Tworzenie nowej strony programu ASP.NET o nazwie ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="912b7-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="912b7-225">Przeciągnij element ScriptManager na stronie.</span><span class="sxs-lookup"><span data-stu-id="912b7-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="912b7-226">Przeciągnij formant TextBox na stronie.</span><span class="sxs-lookup"><span data-stu-id="912b7-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="912b7-227">Przeciągnij formant przycisku na stronie.</span><span class="sxs-lookup"><span data-stu-id="912b7-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="912b7-228">W oknie Właściwości zmień wartość właściwości identyfikator przycisku na wartość <em>btnSave</em> i właściwość tekst na wartość *Zapisz\**.</span><span class="sxs-lookup"><span data-stu-id="912b7-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="912b7-229">Utworzyliśmy strony z formantu standardowego ASP.NET pole tekstowe i przycisk.</span><span class="sxs-lookup"><span data-stu-id="912b7-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="912b7-230">Następnie należy rozszerzyć formant pola tekstowego przy użyciu rozszerzeń DisabledButton:</span><span class="sxs-lookup"><span data-stu-id="912b7-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="912b7-231">Wybierz **Dodaj Extender** zadań opcję, aby otworzyć okno dialogowe Kreator rozszerzeń (zobacz rysunek 5).</span><span class="sxs-lookup"><span data-stu-id="912b7-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="912b7-232">Należy zauważyć, że okno dialogowe zawiera nasz niestandardowego rozszerzenia DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="912b7-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="912b7-233">Wybierz rozszerzenie DisabledButton, a następnie kliknij przycisk **OK** przycisku.</span><span class="sxs-lookup"><span data-stu-id="912b7-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="912b7-234">[![Okno dialogowe Kreator rozszerzeń](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="912b7-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="912b7-235">**Rysunek 05**: Okno dialogowe Kreator rozszerzeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="912b7-236">Ponadto firma Microsoft można ustawić właściwości rozszerzenia, które ma DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="912b7-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="912b7-237">Można zmodyfikować właściwości rozszerzenia, które ma DisabledButton, modyfikując właściwości formant pola tekstowego:</span><span class="sxs-lookup"><span data-stu-id="912b7-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="912b7-238">Wybierz pole tekstowe w projektancie.</span><span class="sxs-lookup"><span data-stu-id="912b7-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="912b7-239">W oknie właściwości rozwiń węzeł rozszerzeń (patrz rysunek 6).</span><span class="sxs-lookup"><span data-stu-id="912b7-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="912b7-240">Przypisz wartość *Zapisz* DisabledText właściwości i wartość *btnSave* właściwości TargetButtonID.</span><span class="sxs-lookup"><span data-stu-id="912b7-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="912b7-241">[![Ustawianie właściwości rozszerzenia](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="912b7-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="912b7-242">**Rysunek 06**: Ustawianie właściwości rozszerzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="912b7-243">Po uruchomieniu strony (przez naciskać klawisz F5), formant przycisku jest początkowo wyłączone.</span><span class="sxs-lookup"><span data-stu-id="912b7-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="912b7-244">Zaraz po jego uruchomieniu, wprowadzając tekst w polu tekstowym, przycisk kontrolka jest włączona (zobacz rysunek 7).</span><span class="sxs-lookup"><span data-stu-id="912b7-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="912b7-245">[![Rozszerzenie DisabledButton w działaniu](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="912b7-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="912b7-246">**Rysunek 07**: Rozszerzenie DisabledButton w akcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="912b7-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="912b7-247">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="912b7-247">Summary</span></span>

<span data-ttu-id="912b7-248">Celem tego samouczka było wyjaśniają, jak można je rozszerzyć AJAX Control Toolkit za pomocą niestandardowego rozszerzenia kontrolki.</span><span class="sxs-lookup"><span data-stu-id="912b7-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="912b7-249">W tym samouczku utworzyliśmy proste rozszerzenie kontrolki DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="912b7-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="912b7-250">Wprowadziliśmy tego rozszerzenia, tworząc klasę DisabledButtonExtender, zachowanie DisabledButtonBehavior JavaScript i klasa DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="912b7-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="912b7-251">Podobny zestaw kroków należy wykonać przy każdym utworzeniu rozszerzeń kontrolek niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="912b7-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="912b7-252">[Poprzednie](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [dalej](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="912b7-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>

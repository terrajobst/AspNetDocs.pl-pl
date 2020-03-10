---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Wskazówki dotyczące laboratorium: Visual Studio 2013 narzędzia sieci Web | Microsoft Docs'
author: rick-anderson
description: Program Visual Studio to doskonałe środowisko programistyczne dla programu. Oparte na sieci .NET i projekty sieci Web. Zawiera on zaawansowany edytor tekstu, który może być łatwo używany do...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622676"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="1ad05-104">Ćwiczenia praktyczne: narzędzia Web Tools dla programu Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1ad05-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>

<span data-ttu-id="1ad05-105">przez [zespół Camp sieci Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1ad05-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="1ad05-106">Pobierz zestaw szkoleniowy dla sieci Web Camp</span><span class="sxs-lookup"><span data-stu-id="1ad05-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="1ad05-107">Program Visual Studio to doskonałe środowisko programistyczne dla programu. Oparte na sieci .NET i projekty sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1ad05-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="1ad05-108">Zawiera on zaawansowany edytor tekstu, który może być łatwo używany do edytowania plików autonomicznych bez projektu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="1ad05-109">Program Visual Studio utrzymuje w pełni funkcjonalne drzewo analizy podczas edycji każdego pliku.</span><span class="sxs-lookup"><span data-stu-id="1ad05-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="1ad05-110">Dzięki temu program Visual Studio może zapewnić niezrównane akcje autouzupełniania i oparte na dokumentach, a jednocześnie sprawiać, że programowanie znacznie szybsze i bardziej przyjemne.</span><span class="sxs-lookup"><span data-stu-id="1ad05-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="1ad05-111">Te funkcje są szczególnie zaawansowane w dokumentach HTML i CSS.</span><span class="sxs-lookup"><span data-stu-id="1ad05-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="1ad05-112">Wszystkie te możliwości są również dostępne dla rozszerzeń, ułatwiając rozszerzanie edytorów o zaawansowane nowe funkcje zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="1ad05-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="1ad05-113">Web Essentials to zbiór (głównie) ulepszeń związanych z witryną sieci Web w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="1ad05-114">Obejmuje to wiele nowych zaawansowania IntelliSense (szczególnie w przypadku CSS), nowych funkcji linków przeglądarki, automatycznych JSHint dla plików JavaScript, nowych ostrzeżeń dotyczących języka HTML i CSS oraz wielu innych funkcji, które są niezbędne do nowoczesnego programowania w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1ad05-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="1ad05-115">Wszystkie przykładowe kod i fragmenty kodu są zawarte w zestawie szkoleń w sieci Web Camp, które są dostępne w [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="1ad05-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="1ad05-116">Omówienie</span><span class="sxs-lookup"><span data-stu-id="1ad05-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1ad05-117">Cele</span><span class="sxs-lookup"><span data-stu-id="1ad05-117">Objectives</span></span>

<span data-ttu-id="1ad05-118">W tym ćwiczeniu dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="1ad05-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="1ad05-119">Korzystaj z nowych funkcji edytora HTML zawartych w programie Web Essentials, takich jak bogate fragmenty kodu HTML5 i kodowanie Zen</span><span class="sxs-lookup"><span data-stu-id="1ad05-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="1ad05-120">Korzystanie z nowych funkcji edytora CSS zawartych w programie Web Essentials, takich jak selektor kolorów i etykietka narzędzia macierzy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1ad05-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="1ad05-121">Korzystanie z nowych funkcji edytora JavaScript zawartych w programie Web Essentials, takich jak Wyodrębnianie do plików i IntelliSense dla wszystkich elementów HTML</span><span class="sxs-lookup"><span data-stu-id="1ad05-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="1ad05-122">Wymiana danych między przeglądarką i programem Visual Studio przy użyciu linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1ad05-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1ad05-123">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1ad05-123">Prerequisites</span></span>

<span data-ttu-id="1ad05-124">Następujące czynności są wymagane do wykonania tego laboratorium praktycznego:</span><span class="sxs-lookup"><span data-stu-id="1ad05-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="1ad05-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) lub więcej</span><span class="sxs-lookup"><span data-stu-id="1ad05-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="1ad05-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="1ad05-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="1ad05-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="1ad05-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="1ad05-128">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="1ad05-128">Setup</span></span>

<span data-ttu-id="1ad05-129">Aby można było uruchomić ćwiczenia w tym ćwiczeniu, należy najpierw skonfigurować środowisko.</span><span class="sxs-lookup"><span data-stu-id="1ad05-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="1ad05-130">Otwórz okno Eksploratora Windows i przejdź do folderu **źródłowego** laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1ad05-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="1ad05-131">Kliknij prawym przyciskiem myszy **Setup. cmd** i wybierz polecenie **Uruchom jako administrator** , aby uruchomić proces instalacji, który skonfiguruje środowisko i zainstaluje fragmenty kodu programu Visual Studio dla tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1ad05-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="1ad05-132">Jeśli zostanie wyświetlone okno dialogowe Kontrola konta użytkownika, potwierdź akcję, aby wykonać operację.</span><span class="sxs-lookup"><span data-stu-id="1ad05-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="1ad05-133">Upewnij się, że wszystkie zależności dla tego laboratorium zostały sprawdzone przed uruchomieniem Instalatora.</span><span class="sxs-lookup"><span data-stu-id="1ad05-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="1ad05-134">Używanie fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="1ad05-134">Using the Code Snippets</span></span>

<span data-ttu-id="1ad05-135">W całym dokumencie laboratoryjnym pojawi się monit o wstawienie bloków kodu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="1ad05-136">Dla wygody większość tego kodu jest udostępniana jako fragmenty Visual Studio Code, do których można uzyskać dostęp z poziomu Visual Studio 2013, aby uniknąć konieczności ręcznego dodawania go.</span><span class="sxs-lookup"><span data-stu-id="1ad05-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="1ad05-137">Każdemu z nich towarzyszy rozpoczęcie rozwiązanie znajdujące się w folderze **BEGIN** w ćwiczeniu, który umożliwia wykonywanie poszczególnych czynności niezależnie od innych.</span><span class="sxs-lookup"><span data-stu-id="1ad05-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="1ad05-138">Należy pamiętać, że fragmenty kodu dodawane podczas wykonywania nie są dostępne w tych rozwiązaniach i mogą nie działać do czasu ukończenia ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="1ad05-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="1ad05-139">Wewnątrz kodu źródłowego dla ćwiczenia znajdziesz również folder **końcowy** zawierający rozwiązanie programu Visual Studio z kodem, który wynika z wykonania czynności w odpowiednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="1ad05-140">Te rozwiązania można wykorzystać jako wskazówkę, jeśli potrzebujesz dodatkowej pomocy podczas pracy w ramach tego praktycznego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="1ad05-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1ad05-141">Symulacyjn</span><span class="sxs-lookup"><span data-stu-id="1ad05-141">Exercises</span></span>

<span data-ttu-id="1ad05-142">To laboratorium praktyczne obejmuje następujące ćwiczenia:</span><span class="sxs-lookup"><span data-stu-id="1ad05-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="1ad05-143">Praca z linkiem przeglądarki i programem Web Essentials</span><span class="sxs-lookup"><span data-stu-id="1ad05-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="1ad05-144">Korzystanie z fragmentów kodu i technologii IntelliSense</span><span class="sxs-lookup"><span data-stu-id="1ad05-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="1ad05-145">Po pierwszym uruchomieniu programu Visual Studio należy wybrać jedną z wstępnie zdefiniowanych kolekcji ustawień.</span><span class="sxs-lookup"><span data-stu-id="1ad05-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="1ad05-146">Każda wstępnie zdefiniowana kolekcja jest zaprojektowana tak, aby była zgodna z konkretnym stylem deweloperskim i określa układy okien, zachowanie edytora, fragmenty kodu IntelliSense i opcje okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="1ad05-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="1ad05-147">Procedury przedstawione w tym laboratorium opisują akcje niezbędne do wykonania danego zadania w programie Visual Studio, gdy jest używana **Ogólna kolekcja ustawień deweloperskich** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="1ad05-148">W przypadku wybrania innej kolekcji ustawień dla środowiska programistycznego mogą wystąpić różnice w czynnościach, które należy wziąć pod uwagę.</span><span class="sxs-lookup"><span data-stu-id="1ad05-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="1ad05-149">Ćwiczenie 1: Praca z linkiem przeglądarki i programem Web Essentials</span><span class="sxs-lookup"><span data-stu-id="1ad05-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="1ad05-150">**Web Essentials** to rozszerzenie programu Visual Studio, które dodaje różne użyteczne funkcje do nowoczesnego programowania w sieci Web, głównie koncentrując się na znacznie szybszym i bardziej przyjemnym środowisku programistycznym dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1ad05-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="1ad05-151">Program Web Essentials można zainstalować z galerii rozszerzeń w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="1ad05-152">**Link do przeglądarki** to nowa funkcja dostępna w Visual Studio 2013, która udostępnia kanał między ŚRODOWISKiem IDE programu Visual Studio i dowolną otwartą przeglądarką do wymiany danych między aplikacją sieci Web a programem Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="1ad05-153">Web Essentials rozszerza link przeglądarki za pomocą narzędzi do manipulowania modelem obiektów DOM i stylami CSS stron sieci Web bezpośrednio z przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1ad05-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="1ad05-154">W tym ćwiczeniu zapoznajesz niektóre funkcje obsługiwane przez program **Web Essentials** i **link przeglądarki** , aby ulepszyć prostą stronę quizu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="1ad05-155">Zadanie 1 — uruchamianie projektu w wielu przeglądarkach</span><span class="sxs-lookup"><span data-stu-id="1ad05-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="1ad05-156">W tym zadaniu skonfigurujesz aplikację sieci Web tak, aby była uruchamiana w wielu przeglądarkach jednocześnie, co jest przydatne w przypadku testowania między przeglądarkami.</span><span class="sxs-lookup"><span data-stu-id="1ad05-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="1ad05-157">Otwórz **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="1ad05-158">W menu **plik** wybierz polecenie **Otwórz | Projekt/rozwiązanie..** . i przejdź do **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** w folderze **źródłowym** laboratorium (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="1ad05-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="1ad05-159">Wybierz pozycję **Rozpocznij. sln** , a następnie kliknij przycisk **Otwórz**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="1ad05-160">Na pasku narzędzi programu Visual Studio rozwiń menu przeglądarki i wybierz polecenie **Przeglądaj z...** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="1ad05-161">![Opcja menu Przeglądaj z](visual-studio-2013-web-tools/_static/image1.png "Przeglądaj za pomocą... w menu przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="1ad05-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="1ad05-162">*Opcja menu Przeglądaj z*</span><span class="sxs-lookup"><span data-stu-id="1ad05-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="1ad05-163">W oknie dialogowym **przeglądanie za pomocą** wybierz opcję **Google Chrome** i **Internet Explorer** , przytrzymując wciśnięty klawisz **Ctrl** , a następnie kliknij pozycję **Ustaw jako domyślną**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="1ad05-164">![Przeglądanie przy użyciu okna dialogowego](visual-studio-2013-web-tools/_static/image2.png "Przeglądanie przy użyciu okna dialogowego")</span><span class="sxs-lookup"><span data-stu-id="1ad05-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="1ad05-165">*Wybieranie wielu przeglądarek domyślnych*</span><span class="sxs-lookup"><span data-stu-id="1ad05-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="1ad05-166">Programy Google Chrome i Internet Explorer powinny teraz być wyświetlane jako domyślne przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1ad05-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="1ad05-167">Kliknij przycisk **Anuluj** , aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="1ad05-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="1ad05-168">![Google Chrome i Internet Explorer jako przeglądarki domyślne](visual-studio-2013-web-tools/_static/image3.png "Przeglądarki Google Chrome i przeglądarka Internet Explorer domyślne")</span><span class="sxs-lookup"><span data-stu-id="1ad05-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="1ad05-169">*Google Chrome i Internet Explorer jako przeglądarki domyślne*</span><span class="sxs-lookup"><span data-stu-id="1ad05-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-170">Po skonfigurowaniu domyślnej przeglądarki w menu przeglądarki zostanie wybrana opcja **wiele przeglądarek** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="1ad05-171">![Wiele przeglądarek](visual-studio-2013-web-tools/_static/image4.png "Wiele przeglądarek")</span><span class="sxs-lookup"><span data-stu-id="1ad05-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="1ad05-172">Naciśnij klawisz **CTRL** + **F5** , aby uruchomić aplikację bez debugowania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="1ad05-173">Gdy otwierane są obie okna przeglądarki, umieść je powyżej drugiego, aby wyświetlić aktualizacje obu przeglądarek jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="1ad05-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="1ad05-174">W przeglądarkach powinna zostać wyświetlona strona sieci Web z prostokątem jasnoniebieskim.</span><span class="sxs-lookup"><span data-stu-id="1ad05-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="1ad05-175">![Umieszczenie jednej przeglądarki powyżej drugiej](visual-studio-2013-web-tools/_static/image5.png "Umieszczenie jednej przeglądarki powyżej drugiej")</span><span class="sxs-lookup"><span data-stu-id="1ad05-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="1ad05-176">*Umieszczenie jednej przeglądarki powyżej drugiej*</span><span class="sxs-lookup"><span data-stu-id="1ad05-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="1ad05-177">Nie zamykaj przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="1ad05-177">Do not close the browsers.</span></span> <span data-ttu-id="1ad05-178">Będziesz ich używać w następnym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="1ad05-179">Zadanie 2 — używanie kodowania Zen do tworzenia elementów HTML</span><span class="sxs-lookup"><span data-stu-id="1ad05-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="1ad05-180">**Kodowanie Zen** jest wtyczką edytora dla wysokiej szybkości kodu HTML, XML, XSL (lub dowolnego innego formatu kodu strukturalnego) do kodowania i edycji.</span><span class="sxs-lookup"><span data-stu-id="1ad05-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="1ad05-181">Rdzeń tej wtyczki to zaawansowany aparat skrótu, który umożliwia rozszerzanie wyrażeń — podobnie jak w przypadku selektorów CSS — do kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="1ad05-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="1ad05-182">Kodowanie Zen to szybki sposób pisania kodu HTML przy użyciu składni selektora stylów CSS.</span><span class="sxs-lookup"><span data-stu-id="1ad05-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="1ad05-183">W tym ćwiczeniu użyjesz funkcji kodowania Zen dostarczonej przez program Web Essentials, aby wygenerować przyciski HTML reprezentujące opcje pytania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="1ad05-184">Przełącz się z powrotem do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="1ad05-185">Otwórz plik **index. cshtml** znajdujący się w **widokach** | folderze **głównym** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="1ad05-186">Zastąp **&lt;!--do zrobienia: Dodaj tutaj opcje--&gt;** komentarz z poniższym kodem, a następnie naciśnij klawisz **Tab**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="1ad05-187">Kod powinien zostać rozszerzony do formatu HTML.</span><span class="sxs-lookup"><span data-stu-id="1ad05-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="1ad05-188">![Rozwinięty kod HTML](visual-studio-2013-web-tools/_static/image6.png "Rozwinięty kod HTML")</span><span class="sxs-lookup"><span data-stu-id="1ad05-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="1ad05-189">*Rozwinięty kod HTML*</span><span class="sxs-lookup"><span data-stu-id="1ad05-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-190">Aby dowiedzieć się więcej o składni kodowania Zen, zapoznaj się z następującym [artykułem](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="1ad05-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="1ad05-191">Kliknij przycisk **Odśwież połączone przeglądarki** , aby zaktualizować obie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1ad05-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="1ad05-192">![Odśwież połączone przeglądarki](visual-studio-2013-web-tools/_static/image7.png "Odśwież połączone przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="1ad05-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="1ad05-193">*Odśwież połączone przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="1ad05-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="1ad05-194">![Internet Explorer — Zaktualizowano na stronie cztery przyciski](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer — Zaktualizowano na stronie cztery przyciski")</span><span class="sxs-lookup"><span data-stu-id="1ad05-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="1ad05-195">*Internet Explorer — Zaktualizowano na stronie cztery przyciski*</span><span class="sxs-lookup"><span data-stu-id="1ad05-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="1ad05-196">![Google Chrome — Zaktualizowano za pomocą czterech przycisków](visual-studio-2013-web-tools/_static/image9.png "Google Chrome — Zaktualizowano za pomocą czterech przycisków")</span><span class="sxs-lookup"><span data-stu-id="1ad05-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="1ad05-197">*Google Chrome — Zaktualizowano za pomocą czterech przycisków*</span><span class="sxs-lookup"><span data-stu-id="1ad05-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="1ad05-198">Przełącz się z powrotem do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="1ad05-199">Dodano przyciski do strony, ale nadal trzeba dodać pytanie dotyczące szablonu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="1ad05-200">W tym celu będziesz używać nowej funkcji w programie Web Essentials o nazwie **Lorem Ipsum Generator**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="1ad05-201">Znajdź element **DIV** z **przednim**atrybutem **klasy** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="1ad05-202">Dodaj następujący kod jako pierwszy element podrzędny elementu **DIV**, a następnie naciśnij klawisz **Tab**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="1ad05-203">Kod powinien zostać rozszerzony do formatu HTML.</span><span class="sxs-lookup"><span data-stu-id="1ad05-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="1ad05-204">![Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum")</span><span class="sxs-lookup"><span data-stu-id="1ad05-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="1ad05-205">*Lorem Ipsum*</span><span class="sxs-lookup"><span data-stu-id="1ad05-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-206">W ramach kodowania Zen można teraz generować kod Lorem Ipsum bezpośrednio w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="1ad05-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="1ad05-207">Wystarczy wstawić **Lorem** i **kartę** trafień oraz 30 Word Lorem Ipsum tekstu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="1ad05-208">Na przykład</span><span class="sxs-lookup"><span data-stu-id="1ad05-208">E.g.</span></span> <span data-ttu-id="1ad05-209">*lorem10* wstawia 10 Lorem Ipsum wyrazów.</span><span class="sxs-lookup"><span data-stu-id="1ad05-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="1ad05-210">W górnej części pytania dodasz logo przy użyciu innej nowej funkcji w programie Web Essentials o nazwie **Generator pikseli Lorem**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="1ad05-211">Dodaj następujący kod jako pierwszy element podrzędny elementu **DIV** z **kontenerem** jako wartość **klasy** , a następnie naciśnij klawisz **Tab**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="1ad05-212">Kod powinien zostać rozszerzony do formatu HTML.</span><span class="sxs-lookup"><span data-stu-id="1ad05-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="1ad05-213">![Wygenerowane piksele Lorem](visual-studio-2013-web-tools/_static/image11.png "Wygenerowane piksele Lorem")</span><span class="sxs-lookup"><span data-stu-id="1ad05-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="1ad05-214">*Wygenerowane piksele Lorem*</span><span class="sxs-lookup"><span data-stu-id="1ad05-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-215">W ramach kodowania Zen można również generować kod pikseli Lorem bezpośrednio w edytorze HTML.</span><span class="sxs-lookup"><span data-stu-id="1ad05-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="1ad05-216">Po prostu wpisz **PIX-200x200-zwierzęta** i **kartę** trafień oraz tag **IMG** z obrazem 200x200 zwierzęcia.</span><span class="sxs-lookup"><span data-stu-id="1ad05-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="1ad05-217">Aby uzyskać więcej informacji, zobacz [Lorem pikseli](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="1ad05-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="1ad05-218">Kliknij przycisk **Odśwież połączone przeglądarki** , aby zaktualizować obie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1ad05-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="1ad05-219">![Internet Explorer — generowany automatycznie obraz i tekst](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer — generowany automatycznie obraz i tekst")</span><span class="sxs-lookup"><span data-stu-id="1ad05-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="1ad05-220">*Internet Explorer — generowany automatycznie obraz i tekst*</span><span class="sxs-lookup"><span data-stu-id="1ad05-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="1ad05-221">![Google Chrome — automatycznie wygenerowany obraz i tekst](visual-studio-2013-web-tools/_static/image13.png "Google Chrome — automatycznie wygenerowany obraz i tekst")</span><span class="sxs-lookup"><span data-stu-id="1ad05-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="1ad05-222">*Google Chrome — automatycznie wygenerowany obraz i tekst*</span><span class="sxs-lookup"><span data-stu-id="1ad05-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-223">Ponieważ obraz jest wybierany losowo podczas dodawania fragmentu kodu, obraz wyświetlany w przeglądarkach może się różnić.</span><span class="sxs-lookup"><span data-stu-id="1ad05-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="1ad05-224">Nie zamykaj przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="1ad05-224">Do not close the browsers.</span></span> <span data-ttu-id="1ad05-225">Będziesz ich używać w następnym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="1ad05-226">Zadanie 3 — Aktualizowanie właściwości stylu</span><span class="sxs-lookup"><span data-stu-id="1ad05-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="1ad05-227">W tym zadaniu zostanie użyta funkcja **Tryb inspekcji** linku przeglądarki w celu wykrycia dokładnej lokalizacji, w której jest generowany konkretny element dom, a następnie zaktualizowanie właściwości Color tego elementu przy użyciu selektora kolorów dostarczonego przez program Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="1ad05-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="1ad05-228">W przeglądarce Internet Explorer naciśnij klawisz **CTRL** + **Alt** + **I** Włącz tryb inspekcji.</span><span class="sxs-lookup"><span data-stu-id="1ad05-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="1ad05-229">Przesuń wskaźnik myszy nad jasnoniebieskim obramowaniem, a następnie kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="1ad05-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="1ad05-230">![Przesuwanie wskaźnika nad jasnoniebieskim obramowaniem](visual-studio-2013-web-tools/_static/image14.png "Przesuwanie wskaźnika nad jasnoniebieskim obramowaniem")</span><span class="sxs-lookup"><span data-stu-id="1ad05-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="1ad05-231">*Przesuwanie wskaźnika nad jasnoniebieskim obramowaniem*</span><span class="sxs-lookup"><span data-stu-id="1ad05-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="1ad05-232">Przełącz się z powrotem do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="1ad05-233">Zwróć uwagę, jak element HTML wybrany w przeglądarce również jest wybierany w edytorze HTML programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="1ad05-234">![Wybrany element HTML w edytorze HTML programu Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Wybrany element HTML w edytorze HTML programu Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="1ad05-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="1ad05-235">*Wybrany element HTML w edytorze HTML programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="1ad05-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="1ad05-236">Teraz zaktualizujemy klasy CSS z **przodu** , aby zmienić styl wybranego elementu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="1ad05-237">Aby to zrobić, naciśnij klawisz **CTRL** +  **,** aby otworzyć pole wyszukiwania **Przejdź do** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="1ad05-238">Wpisz **site. css** i naciśnij klawisz **Enter** , aby otworzyć plik.</span><span class="sxs-lookup"><span data-stu-id="1ad05-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="1ad05-239">![Otwieranie pliku site. CSS](visual-studio-2013-web-tools/_static/image16.png "Otwieranie pliku site. CSS")</span><span class="sxs-lookup"><span data-stu-id="1ad05-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="1ad05-240">*Otwieranie pliku site. CSS*</span><span class="sxs-lookup"><span data-stu-id="1ad05-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="1ad05-241">Naciśnij **kombinację klawiszy CTRL** + **F** i Type **. Przerzuć-Container. Front** , aby znaleźć Selektor CSS.</span><span class="sxs-lookup"><span data-stu-id="1ad05-241">Press **CTRL** + **F** and type **.flip-container .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="1ad05-242">Kliknij jasnoniebieski kwadrat we właściwości Border klasy, aby otworzyć selektora kolorów.</span><span class="sxs-lookup"><span data-stu-id="1ad05-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="1ad05-243">![Otwieranie selektora kolorów](visual-studio-2013-web-tools/_static/image17.png "Otwieranie selektora kolorów")</span><span class="sxs-lookup"><span data-stu-id="1ad05-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="1ad05-244">*Otwieranie selektora kolorów*</span><span class="sxs-lookup"><span data-stu-id="1ad05-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="1ad05-245">Rozwiń selektor kolorów, klikając przycisk Pagon i wybierz nowy kolor.</span><span class="sxs-lookup"><span data-stu-id="1ad05-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="1ad05-246">![Rozszerzanie selektora kolorów](visual-studio-2013-web-tools/_static/image18.png "Rozszerzanie selektora kolorów")</span><span class="sxs-lookup"><span data-stu-id="1ad05-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="1ad05-247">*Rozszerzanie selektora kolorów*</span><span class="sxs-lookup"><span data-stu-id="1ad05-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="1ad05-248">Naciśnij klawisz **CTRL** + **Alt** + **Enter** , aby odświeżyć połączone przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1ad05-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="1ad05-249">Przejdź do programu Internet Explorer i zwróć uwagę na sposób zmiany koloru obramowania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="1ad05-250">![Internet Explorer — kolor obramowania zaktualizowany](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer — kolor obramowania zaktualizowany")</span><span class="sxs-lookup"><span data-stu-id="1ad05-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="1ad05-251">*Internet Explorer — kolor obramowania zaktualizowany*</span><span class="sxs-lookup"><span data-stu-id="1ad05-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="1ad05-252">Przejdź do przeglądarki Google Chrome i Zauważ, jak zmieniono kolor obramowania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="1ad05-253">![Google Chrome — Zaktualizowano kolor obramowania](visual-studio-2013-web-tools/_static/image20.png "Google Chrome — Zaktualizowano kolor obramowania")</span><span class="sxs-lookup"><span data-stu-id="1ad05-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="1ad05-254">*Google Chrome — Zaktualizowano kolor obramowania*</span><span class="sxs-lookup"><span data-stu-id="1ad05-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="1ad05-255">Przełącz się z powrotem do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="1ad05-256">Przejdź na koniec pliku **site. css** i naciśnij klawisz **Ctrl** + **F** , aby zlokalizować selektor **. BTN** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="1ad05-257">Zauważ, że właściwość **-webkit-border-radius** jest podkreślona w kolorze zielonym.</span><span class="sxs-lookup"><span data-stu-id="1ad05-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="1ad05-258">![-webkit-border-właściwość usługi RADIUS selektora BTN](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-właściwość usługi RADIUS selektora BTN")</span><span class="sxs-lookup"><span data-stu-id="1ad05-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="1ad05-259">*-webkit-border-właściwość usługi RADIUS selektora BTN*</span><span class="sxs-lookup"><span data-stu-id="1ad05-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="1ad05-260">Umieść karetkę we właściwości **-webkit-border-radius** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="1ad05-261">Niebieska linia powinna pojawić się poniżej pierwszej litery pierwszego wyrazu właściwości.</span><span class="sxs-lookup"><span data-stu-id="1ad05-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="1ad05-262">Jest to **tag inteligentny**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="1ad05-263">Naciśnij klawisz **CTRL** +  **.**</span><span class="sxs-lookup"><span data-stu-id="1ad05-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="1ad05-264">Aby otworzyć menu sugestie i kliknąć przycisk **Dodaj brakującą Właściwość standardową (border-radius)** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="1ad05-265">![Dodaj brakującą sugestię właściwości standardowych](visual-studio-2013-web-tools/_static/image22.png "Dodaj brakującą sugestię właściwości standardowych")</span><span class="sxs-lookup"><span data-stu-id="1ad05-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="1ad05-266">*Dodaj brakującą sugestię właściwości standardowych*</span><span class="sxs-lookup"><span data-stu-id="1ad05-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="1ad05-267">Właściwość **border-radius** jest automatycznie dodawana do reguły CSS.</span><span class="sxs-lookup"><span data-stu-id="1ad05-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="1ad05-268">![Brak dodanej standardowej właściwości](visual-studio-2013-web-tools/_static/image23.png "Brak dodanej standardowej właściwości")</span><span class="sxs-lookup"><span data-stu-id="1ad05-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="1ad05-269">*Brak dodanej standardowej właściwości*</span><span class="sxs-lookup"><span data-stu-id="1ad05-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="1ad05-270">Przesuń wskaźnik myszy na Właściwość **border-radius** , aby wyświetlić **etykietkę narzędzia macierzy przeglądarki**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="1ad05-271">**Etykietka narzędzia macierzy przeglądarki** pokazuje dostępność właściwości w każdej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="1ad05-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="1ad05-272">![Etykietka narzędzia macierzy przeglądarki](visual-studio-2013-web-tools/_static/image24.png "Etykietka narzędzia macierzy przeglądarki")</span><span class="sxs-lookup"><span data-stu-id="1ad05-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="1ad05-273">*Etykietka narzędzia macierzy przeglądarki*</span><span class="sxs-lookup"><span data-stu-id="1ad05-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="1ad05-274">Zauważ, że wartość właściwości **border-radius** jest nadal podkreślona.</span><span class="sxs-lookup"><span data-stu-id="1ad05-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="1ad05-275">Przesuń wskaźnik myszy nad wartość, aby wyświetlić komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="1ad05-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="1ad05-276">![Border — ostrzeżenie wartości właściwości usługi RADIUS](visual-studio-2013-web-tools/_static/image25.png "Border — ostrzeżenie wartości właściwości usługi RADIUS")</span><span class="sxs-lookup"><span data-stu-id="1ad05-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="1ad05-277">*Border — ostrzeżenie wartości właściwości usługi RADIUS*</span><span class="sxs-lookup"><span data-stu-id="1ad05-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="1ad05-278">Usuń jednostkę wartości właściwości **border-radius** zgodnie z sugerowaną przez etykietkę narzędzia.</span><span class="sxs-lookup"><span data-stu-id="1ad05-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="1ad05-279">Jako **Border-promień** to standardowa właściwość służąca do definiowania sposobu, w jaki zaokrąglenie rogów obramowania jest, można usunąć Właściwość **-webkit-border-radius** i wartość z reguły CSS.</span><span class="sxs-lookup"><span data-stu-id="1ad05-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="1ad05-280">Umieść karetkę we właściwości **zawijania słów** i Zauważ, że tag inteligentny pojawia się również poniżej.</span><span class="sxs-lookup"><span data-stu-id="1ad05-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="1ad05-281">Otwórz menu, a następnie kliknij pozycję **Dodaj brakujące specyficzne dla dostawcy**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="1ad05-282">![Dodaj sugestie dotyczące brakujących specyficznych dla dostawców](visual-studio-2013-web-tools/_static/image26.png "Dodaj sugestie dotyczące brakujących specyficznych dla dostawców")</span><span class="sxs-lookup"><span data-stu-id="1ad05-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="1ad05-283">*Dodaj sugestie dotyczące brakujących specyficznych dla dostawców*</span><span class="sxs-lookup"><span data-stu-id="1ad05-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="1ad05-284">Właściwość **-MS-Word-Otocz** jest automatycznie dodawana do reguły CSS.</span><span class="sxs-lookup"><span data-stu-id="1ad05-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="1ad05-285">![Dodana właściwość określona przez dostawcę](visual-studio-2013-web-tools/_static/image27.png "Dodana właściwość określona przez dostawcę")</span><span class="sxs-lookup"><span data-stu-id="1ad05-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="1ad05-286">*Dodana właściwość określona przez dostawcę*</span><span class="sxs-lookup"><span data-stu-id="1ad05-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="1ad05-287">Zadanie 4 — Aktualizowanie kodu HTML z przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1ad05-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="1ad05-288">W tym zadaniu zostanie użyta funkcja **tryb projektowania** linku przeglądarki do edycji obiektu Dom z przeglądarki i przetransferowania zmian do pliku źródłowego HTML w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="1ad05-289">W przeglądarce Google Chrome naciśnij **klawisze CTRL** + **Alt** + **D** , aby włączyć tryb projektowania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="1ad05-290">Przesuń wskaźnik myszy na **Lorem ipsum dolor sit amet** , a następnie kliknij przycisk.</span><span class="sxs-lookup"><span data-stu-id="1ad05-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="1ad05-291">![Edytowanie pytania](visual-studio-2013-web-tools/_static/image28.png "Edytowanie pytania")</span><span class="sxs-lookup"><span data-stu-id="1ad05-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="1ad05-292">*Edytowanie pytania*</span><span class="sxs-lookup"><span data-stu-id="1ad05-292">*Editing the question*</span></span>
3. <span data-ttu-id="1ad05-293">Powinien pojawić się kursor.</span><span class="sxs-lookup"><span data-stu-id="1ad05-293">A cursor should appear.</span></span> <span data-ttu-id="1ad05-294">Zamień oryginalny tekst na *to, co będzie wyglądać, gdy zapisuję dłuższe pytanie?* , a następnie naciśnij klawisz **ESC** , aby wyjść z trybu projektowania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="1ad05-295">![Edytowane pytanie](visual-studio-2013-web-tools/_static/image29.png "Edytowane pytanie")</span><span class="sxs-lookup"><span data-stu-id="1ad05-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="1ad05-296">*Edytowane pytanie*</span><span class="sxs-lookup"><span data-stu-id="1ad05-296">*Question edited*</span></span>
4. <span data-ttu-id="1ad05-297">Przełącz się z powrotem do programu Visual Studio i Otwórz **index. cshtml**, jeśli nie został jeszcze otwarty.</span><span class="sxs-lookup"><span data-stu-id="1ad05-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="1ad05-298">Należy zauważyć, że tekst wewnętrzny elementu **&lt;p&gt;** został zaktualizowany.</span><span class="sxs-lookup"><span data-stu-id="1ad05-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="1ad05-299">![Zaktualizowane pytanie na stronie HTML](visual-studio-2013-web-tools/_static/image30.png "Zaktualizowane pytanie na stronie HTML")</span><span class="sxs-lookup"><span data-stu-id="1ad05-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="1ad05-300">*Zaktualizowane pytanie na stronie HTML*</span><span class="sxs-lookup"><span data-stu-id="1ad05-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="1ad05-301">Zadanie 5 — przeglądanie ostrzeżeń związanych z programem optymalizacji</span><span class="sxs-lookup"><span data-stu-id="1ad05-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="1ad05-302">**Optymalizacja aparatu wyszukiwania** (wyszukiwarki) to proces zwiększania rangi witryny sieci Web na liście wyników aparatu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="1ad05-303">Im większa ranga witryny i im większa jest lista, tym więcej odwiedzanych przez tę lokację będzie można znaleźć w tym aparacie wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="1ad05-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="1ad05-304">W programie Web Essentials znajduje się narzędzie analityczne, które bada kod HTML, raportuje znalezione problemy i zapewnia pomoc w rozwiązaniu tych problemów.</span><span class="sxs-lookup"><span data-stu-id="1ad05-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="1ad05-305">Przejdź do menu **Widok** , a następnie kliknij przycisk **Lista błędów** , aby otworzyć okno **Lista błędów** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="1ad05-306">![Lista błędów w menu Widok](visual-studio-2013-web-tools/_static/image31.png "Lista błędów w menu Widok")</span><span class="sxs-lookup"><span data-stu-id="1ad05-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="1ad05-307">*Lista błędów w menu Widok*</span><span class="sxs-lookup"><span data-stu-id="1ad05-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="1ad05-308">Zwróć uwagę, że istnieje ostrzeżenie dotyczące funkcji optymalizacji informującej o braku **&lt;tagu meta&gt;** dla opisu strony.</span><span class="sxs-lookup"><span data-stu-id="1ad05-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="1ad05-309">Kliknij dwukrotnie wpis ostrzeżenia funkcji optymalizacji, aby go usunąć.</span><span class="sxs-lookup"><span data-stu-id="1ad05-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="1ad05-310">![Okno Lista błędów](visual-studio-2013-web-tools/_static/image32.png "okno listy błędów")</span><span class="sxs-lookup"><span data-stu-id="1ad05-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="1ad05-311">*Okno Lista błędów*</span><span class="sxs-lookup"><span data-stu-id="1ad05-311">*Error List window*</span></span>
3. <span data-ttu-id="1ad05-312">W oknie dialogowym **Web Essentials** kliknij przycisk **tak** , aby wstawić opis &lt;tagu Meta&gt;.</span><span class="sxs-lookup"><span data-stu-id="1ad05-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="1ad05-313">![Web Essentials — okno dialogowe](visual-studio-2013-web-tools/_static/image33.png "Web Essentials — okno dialogowe")</span><span class="sxs-lookup"><span data-stu-id="1ad05-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="1ad05-314">*Web Essentials — okno dialogowe*</span><span class="sxs-lookup"><span data-stu-id="1ad05-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="1ad05-315">Edytor dla **\_Layout. cshtml** zostanie otwarty, a tag **&lt;meta&gt;** zostanie automatycznie dodany do sekcji **nagłówkowej** pliku HTML.</span><span class="sxs-lookup"><span data-stu-id="1ad05-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="1ad05-316">![Tag meta został automatycznie dodany na stronie _Layout](visual-studio-2013-web-tools/_static/image34.png "Tag meta został automatycznie dodany na stronie _Layout")</span><span class="sxs-lookup"><span data-stu-id="1ad05-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="1ad05-317">*Meta tag automatycznie dodawany do strony układu \_*</span><span class="sxs-lookup"><span data-stu-id="1ad05-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="1ad05-318">Zmień wartość atrybutu **Content** na *GeekQuiz* i Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="1ad05-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="1ad05-319">Ćwiczenie 2: korzystanie z fragmentów kodu i technologii IntelliSense</span><span class="sxs-lookup"><span data-stu-id="1ad05-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="1ad05-320">W programie Web Essentials Edytor HTML został rozszerzony o dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="1ad05-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="1ad05-321">W tym ćwiczeniu zostaną wyświetlone nowe funkcje, które są przydatne podczas opracowywania aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1ad05-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="1ad05-322">Zadanie 1 — Używanie funkcji IntelliSense w dokumentach HTML</span><span class="sxs-lookup"><span data-stu-id="1ad05-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="1ad05-323">Pierwsza Nowa funkcja, która będzie widoczna w tym zadaniu, nazywa się **dynamiczną**technologią IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="1ad05-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="1ad05-324">Dynamiczne IntelliSense odczytuje inne Tagi i atrybuty w celu wywnioskowania możliwych identyfikatorów, które będą używane.</span><span class="sxs-lookup"><span data-stu-id="1ad05-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="1ad05-325">W tym zadaniu utworzysz nowy element formularza HTML, który zawiera etykietę i pole wejściowe.</span><span class="sxs-lookup"><span data-stu-id="1ad05-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="1ad05-326">Następnie dodasz atrybut **for** do etykiety, aby powiązać ją z danymi wejściowymi. zobaczysz sugestie IntelliSense na podstawie identyfikatorów wejść w zakres.</span><span class="sxs-lookup"><span data-stu-id="1ad05-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="1ad05-327">Otwórz **Visual Studio Express 2013 dla sieci Web** i rozwiązania **BEGIN. sln** znajdującego się w folderze **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/BEGIN** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="1ad05-328">Alternatywnie możesz kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="1ad05-329">W **Eksplorator rozwiązań**Otwórz plik **index. cshtml** znajdujący się w folderze **widoki** | **Home** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="1ad05-330">Dodaj następujący formularz wewnątrz **sekcji&lt;&gt;** elementu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="1ad05-331">(Fragment kodu — *VisualStudio2013WebTooling* - *Ex2* - *form*)</span><span class="sxs-lookup"><span data-stu-id="1ad05-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="1ad05-332">Tag wejściowy powinien być poprzedzony etykietą z nieprawidłowym opisem pola.</span><span class="sxs-lookup"><span data-stu-id="1ad05-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="1ad05-333">Dodaj następującą etykietę przed tagiem wejściowym.</span><span class="sxs-lookup"><span data-stu-id="1ad05-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="1ad05-334">Atrybut **for** **&lt;etykiety&gt;** określa, który element formularza jest powiązany z etykietą.</span><span class="sxs-lookup"><span data-stu-id="1ad05-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="1ad05-335">Wartość atrybutu powinna być taka sama jak identyfikator powiązanego elementu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="1ad05-336">Dodaj atrybut **for** do **&lt;&gt;etykieta** elementu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="1ad05-337">Jak pokazano na poniższej ilustracji, nazwa &quot;&quot; wartość pojawia się w polu IntelliSense na podstawie identyfikatora elementów w ramach tego samego zakresu ( **&lt;&gt;formularz** ).</span><span class="sxs-lookup"><span data-stu-id="1ad05-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="1ad05-338">![Wyświetlanie identyfikatora w IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Wyświetlanie identyfikatora w IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="1ad05-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="1ad05-339">*Wyświetlanie identyfikatora w IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="1ad05-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="1ad05-340">Usuń ostatnio dodany element **&lt;formularz&gt;** i jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="1ad05-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="1ad05-341">Zadanie 2 — używanie fragmentów kodu HTML</span><span class="sxs-lookup"><span data-stu-id="1ad05-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="1ad05-342">W języku HTML5 wprowadzono więcej niż 25 nowych tagów semantycznych.</span><span class="sxs-lookup"><span data-stu-id="1ad05-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="1ad05-343">Program Visual Studio ma już obsługę technologii IntelliSense dla tych tagów, ale Visual Studio 2013 przyspiesza i ułatwia pisanie znaczników przez dodawanie nowych fragmentów kodu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="1ad05-344">Chociaż te Tagi nie są skomplikowane, składają się z kilku niewielkich subtleties, takich jak dodanie poprawnych rezerw koderów dla znacznika *audio* .</span><span class="sxs-lookup"><span data-stu-id="1ad05-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="1ad05-345">W tym zadaniu zobaczysz fragmenty kodu HTML dla tagu audio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="1ad05-346">W pliku **index. cshtml** wpisz **&lt;aud** wewnątrz **sekcji&lt;&gt;** elementu, jak pokazano na poniższym rysunku.</span><span class="sxs-lookup"><span data-stu-id="1ad05-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="1ad05-347">![Wstawianie elementu audio](visual-studio-2013-web-tools/_static/image36.png "Wstawianie elementu audio")</span><span class="sxs-lookup"><span data-stu-id="1ad05-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="1ad05-348">*Wstawianie elementu audio*</span><span class="sxs-lookup"><span data-stu-id="1ad05-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="1ad05-349">Naciśnij dwukrotnie klawisz **Tab** i zwróć uwagę na to, jak poniższy kod został dodany na stronie, a kursor zostanie umieszczony na atrybucie **src** pierwszego źródła.</span><span class="sxs-lookup"><span data-stu-id="1ad05-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="1ad05-350">Naciskając klawisz **Tab** dwa razy, fragment kodu zostanie wstawiony.</span><span class="sxs-lookup"><span data-stu-id="1ad05-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="1ad05-351">Fragment kodu audio pokazuje standardowe użycie znacznika *audio* z dwoma plikami źródłowymi w celu zapewnienia lepszej obsługi.</span><span class="sxs-lookup"><span data-stu-id="1ad05-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="1ad05-352">Usuń drugi wiersz i zaktualizuj Źródło pierwszego wiersza, korzystając z następującego linku do WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="1ad05-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="1ad05-353">Kod wynikowy jest przedstawiony poniżej.</span><span class="sxs-lookup"><span data-stu-id="1ad05-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="1ad05-354">Plik źródłowy *KatanaProject. mp3* jest używany jako przykład.</span><span class="sxs-lookup"><span data-stu-id="1ad05-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="1ad05-355">Jeśli wolisz, możesz użyć innego źródła.</span><span class="sxs-lookup"><span data-stu-id="1ad05-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="1ad05-356">Naciśnij klawisz **CTRL** + **S** , aby zapisać plik.</span><span class="sxs-lookup"><span data-stu-id="1ad05-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="1ad05-357">Naciśnij klawisz **CTRL** + **F5** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="1ad05-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="1ad05-358">Zwróć uwagę, że odtwarzacz audio został dodany do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ad05-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="1ad05-359">![Odtwarzacz audio w programie Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Odtwarzacz audio w programie Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="1ad05-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="1ad05-360">*Odtwarzacz audio w programie Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="1ad05-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="1ad05-361">![Odtwarzacz audio w przeglądarce Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Odtwarzacz audio w przeglądarce Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="1ad05-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="1ad05-362">*Odtwarzacz audio w przeglądarce Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="1ad05-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="1ad05-363">Nie zamykaj przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="1ad05-363">Do not close the browsers.</span></span> <span data-ttu-id="1ad05-364">Będziesz ich używać w następnym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="1ad05-365">Zadanie 3 — Używanie funkcji IntelliSense w dokumentach JavaScript</span><span class="sxs-lookup"><span data-stu-id="1ad05-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="1ad05-366">W przypadku programu Web Essentials 2013 arkusze stylów i strony HTML tworzą listę identyfikatorów i nazw klas.</span><span class="sxs-lookup"><span data-stu-id="1ad05-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="1ad05-367">W tym zadaniu dowiesz się, jak te listy poprawiają obsługę funkcji IntelliSense języka JavaScript w programie Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="1ad05-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="1ad05-368">W pliku **index. cshtml** Dodaj następujący kod, aby zdefiniować tag **skryptu** dla kodu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1ad05-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="1ad05-369">Dodaj następujący kod wewnątrz tagu **skryptu** , aby zdefiniować przygotowaną funkcję wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="1ad05-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="1ad05-370">(Fragment kodu- *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="1ad05-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="1ad05-371">Umieść karetkę w tagu **skryptu** i naciśnij klawisz **Ctrl** +  **.**</span><span class="sxs-lookup"><span data-stu-id="1ad05-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="1ad05-372">, aby otworzyć menu sugestii.</span><span class="sxs-lookup"><span data-stu-id="1ad05-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="1ad05-373">Kliknij pozycję **Wyodrębnij do pliku**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="1ad05-374">![Plik JavaScript Wyodrębnij do sugestii plików](visual-studio-2013-web-tools/_static/image39.png "Plik JavaScript Wyodrębnij do sugestii plików")</span><span class="sxs-lookup"><span data-stu-id="1ad05-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="1ad05-375">*Plik JavaScript Wyodrębnij do sugestii plików*</span><span class="sxs-lookup"><span data-stu-id="1ad05-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="1ad05-376">W oknie **Zapisz jako** wybierz folder **skrypty** , Nazwij plik **init. js** , a następnie kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="1ad05-377">![Zapisz jako okno](visual-studio-2013-web-tools/_static/image40.png "Zapisz jako okno")</span><span class="sxs-lookup"><span data-stu-id="1ad05-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="1ad05-378">*Zapisz jako okno*</span><span class="sxs-lookup"><span data-stu-id="1ad05-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-379">Zostanie utworzony plik **init. js** , a zawartość skryptu zostanie przeniesiona do pliku.</span><span class="sxs-lookup"><span data-stu-id="1ad05-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="1ad05-380">![Plik init. js utworzony za pomocą dołączonej zawartości](visual-studio-2013-web-tools/_static/image41.png "Plik init. js utworzony za pomocą dołączonej zawartości")</span><span class="sxs-lookup"><span data-stu-id="1ad05-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="1ad05-381">*Plik init. js utworzony za pomocą dołączonej zawartości*</span><span class="sxs-lookup"><span data-stu-id="1ad05-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="1ad05-382">Otwórz plik **index. cshtml** i sprawdź, czy tag skryptu został zastąpiony odwołaniem do pliku **init. js** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="1ad05-383">![Dokumentacja języka HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Dokumentacja języka HTML init. js")</span><span class="sxs-lookup"><span data-stu-id="1ad05-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="1ad05-384">*Dokumentacja języka HTML init. js*</span><span class="sxs-lookup"><span data-stu-id="1ad05-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="1ad05-385">Przejdź do **Eksplorator rozwiązań** i Zauważ, że plik **init. js** został uwzględniony automatycznie w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="1ad05-386">![Plik init. js uwzględniony w rozwiązaniu](visual-studio-2013-web-tools/_static/image43.png "Plik init. js uwzględniony w rozwiązaniu")</span><span class="sxs-lookup"><span data-stu-id="1ad05-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="1ad05-387">*Plik init. js uwzględniony w rozwiązaniu*</span><span class="sxs-lookup"><span data-stu-id="1ad05-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="1ad05-388">Przełącz się z powrotem do pliku **init. js** , aby zaktualizować wywołanie zwrotne **funkcji.**</span><span class="sxs-lookup"><span data-stu-id="1ad05-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="1ad05-389">Wewnątrz definicji wywołania zwrotnego funkcji, która jest przenoszona do *gotowe*, Dodaj następujący kod, aby uzyskać wszystkie elementy według określonego atrybutu klasy.</span><span class="sxs-lookup"><span data-stu-id="1ad05-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="1ad05-390">Naciśnij klawisz **CTRL** + **miejsce** między cudzysłowami w wywołaniu funkcji **getElementsByClassName** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="1ad05-391">![Wyświetlanie technologii IntelliSense dla funkcji getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Wyświetlanie technologii IntelliSense dla funkcji getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="1ad05-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="1ad05-392">*Wyświetlanie technologii IntelliSense dla funkcji getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="1ad05-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-393">Zauważ, że technologia IntelliSense wyświetla klasy zdefiniowane w arkuszach stylów projektu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="1ad05-394">Zastąp wiersz utworzony przy użyciu następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="1ad05-395">Umieść kursor po elemencie " **au** " w cudzysłowie w funkcji **GetElementsByTagName** i naciśnij klawisz **Ctrl** + **miejsce**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="1ad05-396">![Wyświetlanie funkcji IntelliSense dla metody getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Wyświetlanie funkcji IntelliSense dla metody getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="1ad05-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="1ad05-397">*Wyświetlanie funkcji IntelliSense dla metody getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="1ad05-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="1ad05-398">Wybierz pozycję **&quot;audio&quot;** z listy i naciśnij klawisz **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="1ad05-399">Wyniki są pokazane na poniższym rysunku.</span><span class="sxs-lookup"><span data-stu-id="1ad05-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="1ad05-400">![Pobieranie elementów audio](visual-studio-2013-web-tools/_static/image46.png "Pobieranie elementów audio")</span><span class="sxs-lookup"><span data-stu-id="1ad05-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="1ad05-401">*Pobieranie elementów audio*</span><span class="sxs-lookup"><span data-stu-id="1ad05-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="1ad05-402">W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy plik **init. js** w folderze **skrypty** i wybierz pozycję **Zminifikować pliki JavaScript** z menu programu **Web Essentials** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="1ad05-403">![Zminifikować pliki JavaScript](visual-studio-2013-web-tools/_static/image47.png "Zminifikować pliki JavaScript")</span><span class="sxs-lookup"><span data-stu-id="1ad05-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="1ad05-404">*Zminifikować pliki JavaScript*</span><span class="sxs-lookup"><span data-stu-id="1ad05-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="1ad05-405">Po wyświetleniu monitu o włączenie automatycznej minifikacja po zmianie pliku źródłowego kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="1ad05-406">![Włączanie automatycznego ostrzeżenia minifikacja](visual-studio-2013-web-tools/_static/image48.png "Włączanie automatycznego ostrzeżenia minifikacja")</span><span class="sxs-lookup"><span data-stu-id="1ad05-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="1ad05-407">*Włączanie automatycznego ostrzeżenia minifikacja*</span><span class="sxs-lookup"><span data-stu-id="1ad05-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ad05-408">Element **init. min. js** jest tworzony i dodawany jako zależność pliku **init. js** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="1ad05-409">![Plik init. min. js został utworzony](visual-studio-2013-web-tools/_static/image49.png "Plik init. min. js został utworzony")</span><span class="sxs-lookup"><span data-stu-id="1ad05-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="1ad05-410">*Plik init. min. js został utworzony*</span><span class="sxs-lookup"><span data-stu-id="1ad05-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="1ad05-411">Otwórz plik **init. min. js** i zwróć uwagę na to, że plik jest zminimalizowanego.</span><span class="sxs-lookup"><span data-stu-id="1ad05-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="1ad05-412">![Zawartość pliku init. min. js](visual-studio-2013-web-tools/_static/image50.png "Zawartość pliku init. min. js")</span><span class="sxs-lookup"><span data-stu-id="1ad05-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="1ad05-413">*Zawartość pliku init. min. js*</span><span class="sxs-lookup"><span data-stu-id="1ad05-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="1ad05-414">W pliku **init. js** Dodaj następujący kod poniżej wywołania funkcji **GetElementsByTagName** , aby odtworzyć wszystkie elementy audio.</span><span class="sxs-lookup"><span data-stu-id="1ad05-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="1ad05-415">(Fragment kodu- *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="1ad05-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="1ad05-416">Kliknij przycisk **CTRL** + **S** , aby zapisać plik.</span><span class="sxs-lookup"><span data-stu-id="1ad05-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="1ad05-417">Ponieważ plik zminimalizowanego jest już otwarty, zostanie wyświetlone okno dialogowe z informacją o tym, że plik został zmodyfikowany poza edytorem źródła.</span><span class="sxs-lookup"><span data-stu-id="1ad05-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="1ad05-418">Kliknij przycisk **Tak**.</span><span class="sxs-lookup"><span data-stu-id="1ad05-418">Click **Yes**.</span></span>

    <span data-ttu-id="1ad05-419">![Ostrzeżenie Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "Ostrzeżenie Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="1ad05-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="1ad05-420">*Ostrzeżenie Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="1ad05-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="1ad05-421">Przełącz się z powrotem do pliku **init. min. js** , aby sprawdzić, czy plik został zaktualizowany przy użyciu nowego kodu.</span><span class="sxs-lookup"><span data-stu-id="1ad05-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="1ad05-422">![Zaktualizowano plik init. min. js](visual-studio-2013-web-tools/_static/image52.png "Zaktualizowano plik init. min. js")</span><span class="sxs-lookup"><span data-stu-id="1ad05-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="1ad05-423">*Zaktualizowano plik init. min. js*</span><span class="sxs-lookup"><span data-stu-id="1ad05-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="1ad05-424">Kliknij przycisk **Odśwież link do przeglądarki** .</span><span class="sxs-lookup"><span data-stu-id="1ad05-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="1ad05-425">Po odświeżeniu obu przeglądarek odtwarzacze audio, które zostały opisane w poprzednim zadaniu, rozpoczną odtwarzanie automatycznie.</span><span class="sxs-lookup"><span data-stu-id="1ad05-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="1ad05-426">![Odtwarzacz audio uwzględniony w widoku](visual-studio-2013-web-tools/_static/image53.png "Odtwarzacz audio uwzględniony w widoku")</span><span class="sxs-lookup"><span data-stu-id="1ad05-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="1ad05-427">*Odtwarzacz audio uwzględniony w widoku*</span><span class="sxs-lookup"><span data-stu-id="1ad05-427">*Audio player included in view*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1ad05-428">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="1ad05-428">Summary</span></span>

<span data-ttu-id="1ad05-429">Dzięki zakończeniu tego praktycznego laboratorium wiesz, jak:</span><span class="sxs-lookup"><span data-stu-id="1ad05-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="1ad05-430">Korzystaj z nowych funkcji edytora HTML zawartych w programie Web Essentials, takich jak bogate fragmenty kodu HTML5 i kodowanie Zen</span><span class="sxs-lookup"><span data-stu-id="1ad05-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="1ad05-431">Korzystanie z nowych funkcji edytora CSS zawartych w programie Web Essentials, takich jak selektor kolorów i etykietka narzędzia macierzy przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1ad05-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="1ad05-432">Korzystanie z nowych funkcji edytora JavaScript zawartych w programie Web Essentials, takich jak Wyodrębnianie do plików i IntelliSense dla wszystkich elementów HTML</span><span class="sxs-lookup"><span data-stu-id="1ad05-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="1ad05-433">Wymiana danych między przeglądarką i programem Visual Studio przy użyciu linku przeglądarki</span><span class="sxs-lookup"><span data-stu-id="1ad05-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>

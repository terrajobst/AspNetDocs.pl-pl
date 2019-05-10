---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Uaktualnianie aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2 | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano sposób uaktualniania ręcznie i za pomocą Kreatora aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2. Ten dokument jest również dostępna dla d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65125698"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="460cc-104">Uaktualnianie aplikacji ASP.NET MVC 1.0 do wzorca ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="460cc-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>

> <span data-ttu-id="460cc-105">W tym dokumencie opisano sposób uaktualniania ręcznie i za pomocą Kreatora aplikacji ASP.NET MVC 1.0 do ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="460cc-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="460cc-106">Ten dokument jest również dostępna dla [pobierania](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="460cc-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>

## <a name="introduction"></a><span data-ttu-id="460cc-107">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="460cc-107">Introduction</span></span>

<span data-ttu-id="460cc-108">ASP.NET MVC 2 można zainstalować równolegle z programem ASP.NET MVC 1.0 na tym samym serwerze.</span><span class="sxs-lookup"><span data-stu-id="460cc-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="460cc-109">Zapewnia to elastyczność deweloperom aplikacji w wyborze kiedy aktualizować aplikację ASP.NET MVC 1.0 do ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="460cc-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="460cc-110">Program Visual Studio 2010 zawiera Kreatora tego uaktualnienia istniejących projektów programu ASP.NET MVC 1.0 utworzonych za pomocą programu Visual Studio 2008 do wzorca ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="460cc-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="460cc-111">Kreator uaktualniania jest inicjowane przez otwarcie projektu programu ASP.NET MVC 1.0 w programie Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="460cc-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="460cc-112">Uaktualnij kreatora dla platformy ASP.NET MVC 1.0 w programie Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="460cc-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="460cc-113">Aby uaktualnić aplikację ASP.NET MVC 1.0 do ASP.NET MVC 2 w programie Visual Studio 2008 z dodatkiem SP1, należy użyć aplikacji (nieobsługiwane) MvcAppConverter.</span><span class="sxs-lookup"><span data-stu-id="460cc-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="460cc-114">Możesz pobrać tę aplikację z następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="460cc-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="460cc-115">Ręczne uaktualnianie projektu ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="460cc-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="460cc-116">Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 1.0 do wersji 2, wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="460cc-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="460cc-117">Tworzenie kopii zapasowych istniejący projekt.</span><span class="sxs-lookup"><span data-stu-id="460cc-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="460cc-118">W edytorze tekstu Otwórz plik projektu (plik z rozszerzeniem pliku .csproj lub .vbproj) i Znajdź ProjectTypeGuid element.</span><span class="sxs-lookup"><span data-stu-id="460cc-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="460cc-119">Jako wartość tego elementu należy zastąpić identyfikatorem GUID {603c0e0b-db56-11dc-be95-000d561079b0} z {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="460cc-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="460cc-120">Gdy wszystko będzie gotowe, wartość tego elementu powinna być następująca:</span><span class="sxs-lookup"><span data-stu-id="460cc-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="460cc-121">W folderze głównym aplikacji sieci Web należy edytować plik Web.config.</span><span class="sxs-lookup"><span data-stu-id="460cc-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="460cc-122">Wyszukaj System.Web.Mvc, wersja = 1.0.0.0 i Zamień wszystkie wystąpienia System.Web.Mvc, wersja = 2.0.0.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="460cc-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="460cc-123">Powtórz poprzedni krok dla pliku Web.config znajduje się w folderze widoków.</span><span class="sxs-lookup"><span data-stu-id="460cc-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="460cc-124">Otwórz projekt za pomocą programu Visual Studio, a następnie w **Eksploratora rozwiązań**, rozwiń węzeł **odwołania** węzła.</span><span class="sxs-lookup"><span data-stu-id="460cc-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="460cc-125">Usuń odwołanie do System.Web.Mvc (wskazujący zestaw w wersji 1.0).</span><span class="sxs-lookup"><span data-stu-id="460cc-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="460cc-126">Dodaj odwołanie do System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="460cc-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="460cc-127">Dodaj następujący element bindingRedirect do pliku Web.config w katalogu głównym aplikacji, w sekcji configuraton:</span><span class="sxs-lookup"><span data-stu-id="460cc-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="460cc-128">Utwórz nową aplikację ASP.NET MVC 2 puste.</span><span class="sxs-lookup"><span data-stu-id="460cc-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="460cc-129">Skopiuj pliki z folderu skryptów w nowej aplikacji do folderu skryptów w istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="460cc-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="460cc-130">Aktualizowanie istniejących € applicationâ™ s pliku CSS przy użyciu definicji stylów CSS w pliku Site.css.</span><span class="sxs-lookup"><span data-stu-id="460cc-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="460cc-131">Kompilowanie aplikacji i uruchom go.</span><span class="sxs-lookup"><span data-stu-id="460cc-131">Compile the application and run it.</span></span> <span data-ttu-id="460cc-132">Jeśli wystąpią błędy, zapoznaj się z sekcją fundamentalne zmiany [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) strony.</span><span class="sxs-lookup"><span data-stu-id="460cc-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>

---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Kompilowanie plików rozdziałów dla samouczków Dr 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 10237af40e3914b65e5181f17555697e86adea4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599464"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="599f1-103">Kompilowanie pobierania rozdziałów dla samouczków Dr 5 MVC 4</span><span class="sxs-lookup"><span data-stu-id="599f1-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="599f1-104">Autor [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="599f1-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="599f1-105">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="599f1-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="599f1-106">Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="599f1-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="599f1-107">Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="599f1-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

## <a name="building-the-chapter-downloads"></a><span data-ttu-id="599f1-108">Tworzenie materiałów do pobrania rozdziału</span><span class="sxs-lookup"><span data-stu-id="599f1-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="599f1-109">Pobierz i rozpakuj przykładowy plik zip projektu.</span><span class="sxs-lookup"><span data-stu-id="599f1-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="599f1-110">W niespakowanym pakiecie pobierania znajdziesz dodatkowe pliki zip, po jednym dla każdej z nich.</span><span class="sxs-lookup"><span data-stu-id="599f1-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="599f1-111">Kliknij prawym przyciskiem myszy żądany plik zip, kliknij polecenie **Właściwości**, a następnie kliknij przycisk **Odblokuj** .</span><span class="sxs-lookup"><span data-stu-id="599f1-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="599f1-112">Rozpakuj plik.</span><span class="sxs-lookup"><span data-stu-id="599f1-112">Unzip the file.</span></span>
4. <span data-ttu-id="599f1-113">Kliknij dwukrotnie plik *CUx. sln* , aby uruchomić program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="599f1-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="599f1-114">W menu **Narzędzia** kliknij kolejno pozycje **Menedżer pakietów NuGet**, **konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="599f1-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="599f1-115">W konsoli Menedżera pakietów (PMC) kliknij przycisk **Przywróć**.</span><span class="sxs-lookup"><span data-stu-id="599f1-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="599f1-116">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="599f1-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="599f1-117">Uruchom ponownie program Visual Studio, otwierając plik rozwiązania, który został zamknięty w powyższym kroku.</span><span class="sxs-lookup"><span data-stu-id="599f1-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="599f1-118">W konsoli Menedżera pakietów (PMC) wprowadź `Update-Database` polecenie:</span><span class="sxs-lookup"><span data-stu-id="599f1-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="599f1-119">Jeśli zostanie wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="599f1-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="599f1-120">*Termin "Update-Database" nie jest rozpoznawany jako nazwa polecenia cmdlet, funkcji, pliku skryptu ani programu interoperacyjnego. Sprawdź pisownię nazwy lub, jeśli ścieżka została uwzględniona, sprawdź, czy ścieżka jest poprawna, i spróbuj ponownie.*</span><span class="sxs-lookup"><span data-stu-id="599f1-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="599f1-121">Zamknij i uruchom ponownie program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="599f1-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="599f1-122">Każda migracja zostanie uruchomiona, a następnie zostanie uruchomiona Metoda inicjatora.</span><span class="sxs-lookup"><span data-stu-id="599f1-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="599f1-123">Teraz możesz uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="599f1-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="599f1-124">Wstecz</span><span class="sxs-lookup"><span data-stu-id="599f1-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)

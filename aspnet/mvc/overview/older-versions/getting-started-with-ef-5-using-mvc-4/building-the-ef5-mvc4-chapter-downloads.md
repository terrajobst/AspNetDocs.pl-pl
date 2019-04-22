---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Tworzenie w rozdziale pliki do pobrania dla platformy MVC EF 5 4 samouczki | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: e90eebaba3645802f318dbf449c3ec734265a092
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59381955"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="d8a1a-103">Tworzenie w rozdziale pliki do pobrania dla platformy MVC EF 5 4 samouczki</span><span class="sxs-lookup"><span data-stu-id="d8a1a-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="d8a1a-104">Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="d8a1a-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="d8a1a-105">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="d8a1a-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="d8a1a-106">Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="d8a1a-107">Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="d8a1a-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="d8a1a-108">Tworzenie materiałów do pobrania rozdziału</span><span class="sxs-lookup"><span data-stu-id="d8a1a-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="d8a1a-109">Pobierz i rozpakuj plik zip przykładowy projekt.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="d8a1a-110">W pakiecie do pobrania rozpakowany znajdziesz zip dodatkowe pliki — po jednym na zakończenie każdego działu.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="d8a1a-111">Kliknij prawym przyciskiem myszy plik zip odpowiednią pozycję **właściwości**i kliknij przycisk **odblokowanie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="d8a1a-112">Rozpakuj plik.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-112">Unzip the file.</span></span>
4. <span data-ttu-id="d8a1a-113">Kliknij dwukrotnie *CUx.sln* plik, aby uruchomić program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="d8a1a-114">Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet**, następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="d8a1a-115">Kliknij w konsoli Menedżera pakietów (PMC) **przywrócić**.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="d8a1a-116">Zamknij program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="d8a1a-117">Uruchom ponownie program Visual Studio, otwierając plik rozwiązania zamknięte w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="d8a1a-118">W konsoli Menedżera pakietów (PMC) wprowadź `Update-Database` polecenia:</span><span class="sxs-lookup"><span data-stu-id="d8a1a-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="d8a1a-119">Jeśli zostanie wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="d8a1a-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="d8a1a-120">*Termin "Update-Database" nie został rozpoznany jako nazwa polecenia cmdlet, funkcji, pliku skryptu lub program wykonywalny. Sprawdź pisownię nazwy lub jeśli ścieżka został uwzględniony, sprawdź, czy ścieżka jest poprawna i spróbuj ponownie.*</span><span class="sxs-lookup"><span data-stu-id="d8a1a-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="d8a1a-121">Zamknij i ponownie uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="d8a1a-122">Każdą migrację zostanie uruchomiony, a następnie uruchomi seed — metoda</span><span class="sxs-lookup"><span data-stu-id="d8a1a-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="d8a1a-123">Teraz możesz uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="d8a1a-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="d8a1a-124">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="d8a1a-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)

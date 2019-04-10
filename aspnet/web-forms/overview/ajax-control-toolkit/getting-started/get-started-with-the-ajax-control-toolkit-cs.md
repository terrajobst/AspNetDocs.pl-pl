---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Rozpoczynanie pracy z usługą AJAX Control Toolkit (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, wszystko, czego potrzebujesz, aby rozpocząć korzystanie z zestawu narzędzi AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 3bbbe0c8240a2a77edee8d39a76bf865c95f345e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381097"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="59e0c-103">Wprowadzenie do zestawu narzędzi AJAX Control Toolkit (C#)</span><span class="sxs-lookup"><span data-stu-id="59e0c-103">Get Started with the AJAX Control Toolkit (C#)</span></span>

<span data-ttu-id="59e0c-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="59e0c-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="59e0c-105">Dowiedz się, wszystko, czego potrzebujesz, aby rozpocząć korzystanie z zestawu narzędzi AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="59e0c-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="59e0c-106">Zestawu narzędzi AJAX Control Toolkit zawiera ponad 30 bezpłatnych formantów, które można używać w aplikacjach ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59e0c-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="59e0c-107">W tym samouczku dowiesz się, jak pobrać zestawu narzędzi AJAX Control Toolkit i dodawanie kontrolek zestawu narzędzi do usługi Visual Studio/Visual Web Developer Express przybornika.</span><span class="sxs-lookup"><span data-stu-id="59e0c-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="59e0c-108">Pobieranie zestawu narzędzi AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="59e0c-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="59e0c-109">[Zestawu narzędzi AJAX Control Toolkit](http://devexpress.com/act) jest to projekt open source opracowany przez członków społeczności platformy ASP.NET i zespół programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59e0c-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 


[![D<span data-ttu-id="59e0c-110">ownloading zestawu narzędzi AJAX Control Toolkit]</span><span class="sxs-lookup"><span data-stu-id="59e0c-110">ownloading the AJAX Control Toolkit]</span></span>(get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

<span data-ttu-id="59e0c-111">**Rysunek 01**: Pobieranie zestawu narzędzi AJAX Control Toolkit ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="59e0c-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>


<span data-ttu-id="59e0c-112">Po pobraniu pliku, musisz odblokować plik.</span><span class="sxs-lookup"><span data-stu-id="59e0c-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="59e0c-113">Kliknij prawym przyciskiem myszy plik, wybierz polecenie Właściwości, a następnie kliknij przycisk **odblokowanie** przycisku (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="59e0c-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


[![U<span data-ttu-id="59e0c-114">nblocking AJAX kontrolki zestawu narzędzi ZIP plików]</span><span class="sxs-lookup"><span data-stu-id="59e0c-114">nblocking the AJAX Control Toolkit ZIP file]</span></span>(get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

<span data-ttu-id="59e0c-115">**Rysunek 02**: Odblokowanie pliku AJAX kontrolki zestawu narzędzi ZIP ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="59e0c-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>


<span data-ttu-id="59e0c-116">Po odblokowaniu pliku możesz Rozpakuj plik: Kliknij prawym przyciskiem myszy plik i wybierz **Wyodrębnij wszystkie** opcji menu.</span><span class="sxs-lookup"><span data-stu-id="59e0c-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="59e0c-117">Teraz jesteśmy gotowi dodać zestawu narzędzi do przybornika Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="59e0c-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="59e0c-118">Dodawanie zestawu narzędzi AJAX Control Toolkit do przybornika</span><span class="sxs-lookup"><span data-stu-id="59e0c-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="59e0c-119">Aby użyć zestawu narzędzi AJAX Control Toolkit najłatwiej można dodać zestawu narzędzi do zestawu swoich Visual Studio/Visual Web Developer (zobacz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="59e0c-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="59e0c-120">W ten sposób można po prostu przeciągnąć kontrolki zestawu narzędzi na stronie go użyć.</span><span class="sxs-lookup"><span data-stu-id="59e0c-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


[![A<span data-ttu-id="59e0c-121">Zestaw narzędzi do sterowania JAX pojawia się w przyborniku]</span><span class="sxs-lookup"><span data-stu-id="59e0c-121">JAX Control Toolkit appears in toolbox]</span></span>(get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

<span data-ttu-id="59e0c-122">**Rysunek 03**: W przyborniku pojawi się zestawu narzędzi AJAX Control Toolkit ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="59e0c-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>


<span data-ttu-id="59e0c-123">Najpierw należy dodać kartę zestawu narzędzi AJAX Control Toolkit do przybornika.</span><span class="sxs-lookup"><span data-stu-id="59e0c-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="59e0c-124">Wykonaj następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="59e0c-124">Follow these steps.</span></span>

1. <span data-ttu-id="59e0c-125">Utwórz nową witrynę sieci Web platformy ASP.NET, wybierając opcję menu Plik, Nowa witryna.</span><span class="sxs-lookup"><span data-stu-id="59e0c-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="59e0c-126">Kliknij dwukrotnie Default.aspx w oknie Eksploratora rozwiązań, aby otworzyć go w edytorze.</span><span class="sxs-lookup"><span data-stu-id="59e0c-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="59e0c-127">Kliknij prawym przyciskiem myszy przybornika poniżej na karcie Ogólne, a następnie wybierz opcję menu **Dodaj zakładkę** (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="59e0c-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="59e0c-128">Wprowadź nową kartę o nazwie zestawu narzędzi AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="59e0c-128">Enter a new tab named AJAX Control Toolkit.</span></span>


[![A<span data-ttu-id="59e0c-129">Nowa karta dding]</span><span class="sxs-lookup"><span data-stu-id="59e0c-129">dding a new tab]</span></span>(get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

<span data-ttu-id="59e0c-130">**Rysunek 04**: Dodawanie nowej karty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="59e0c-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>


<span data-ttu-id="59e0c-131">Następnie należy dodać kontrolki zestawu narzędzi AJAX Control Toolkit na nowej karcie. Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="59e0c-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="59e0c-132">Kliknij prawym przyciskiem myszy, poniżej na karcie zestawu narzędzi AJAX Control Toolkit, a następnie wybierz opcję menu **wybierz elementy (zobacz rysunek 5)**.</span><span class="sxs-lookup"><span data-stu-id="59e0c-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="59e0c-133">Przejdź do lokalizacji, w którym rozpakowano zestawu narzędzi AJAX Control Toolkit i wybierz zestaw AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="59e0c-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


[![C<span data-ttu-id="59e0c-134">Wybierz elementy do dodania do przybornika]</span><span class="sxs-lookup"><span data-stu-id="59e0c-134">hoose items to add to the toolbox]</span></span>(get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

<span data-ttu-id="59e0c-135">**Rysunek 05**: Wybierz elementy do dodania do przybornika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="59e0c-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>


<span data-ttu-id="59e0c-136">Po wykonaniu tych kroków, zostaną wyświetlone wszystkie kontrolki zestawu narzędzi z przybornika.</span><span class="sxs-lookup"><span data-stu-id="59e0c-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="59e0c-137">Uaktualnianie do nowej wersji zestawu narzędzi</span><span class="sxs-lookup"><span data-stu-id="59e0c-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="59e0c-138">Jeśli była używana starsza wersja zestawu narzędzi, a teraz konieczne przeniesienie do nowszej wersji są zalecane kroki:</span><span class="sxs-lookup"><span data-stu-id="59e0c-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="59e0c-139">Pliki binarne — usuń starą wersję zestawu AjaxControlToolkit.dll z folderu Bin witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="59e0c-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="59e0c-140">Elementy przybornika — Usuń kartę zestawu narzędzi AJAX Control Toolkit i postępuj zgodnie z instrukcjami powyżej, aby ponownie utworzyć kartę w nowej wersji zestawu AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="59e0c-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="59e0c-141">Next</span><span class="sxs-lookup"><span data-stu-id="59e0c-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)

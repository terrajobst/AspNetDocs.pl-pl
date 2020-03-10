---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Część 1: omówienie i tworzenie projektu | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556078"
---
# <a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="151fc-102">Część 1: przegląd i tworzenie projektu</span><span class="sxs-lookup"><span data-stu-id="151fc-102">Part 1: Overview and Creating the Project</span></span>

<span data-ttu-id="151fc-103">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="151fc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="151fc-104">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="151fc-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="151fc-105">Entity Framework to struktura mapowania obiektów/obiektu.</span><span class="sxs-lookup"><span data-stu-id="151fc-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="151fc-106">Mapuje obiekty domeny w kodzie na jednostki w relacyjnej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="151fc-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="151fc-107">W największej części nie trzeba martwić się o warstwę bazy danych, ponieważ Entity Framework ją zajmie.</span><span class="sxs-lookup"><span data-stu-id="151fc-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="151fc-108">Kod operuje na obiektach, a zmiany są utrwalane w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="151fc-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="151fc-109">Informacje o samouczku</span><span class="sxs-lookup"><span data-stu-id="151fc-109">About the Tutorial</span></span>

<span data-ttu-id="151fc-110">W tym samouczku utworzysz prostą aplikację ze sklepu.</span><span class="sxs-lookup"><span data-stu-id="151fc-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="151fc-111">Istnieją dwa główne elementy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="151fc-111">There are two main parts to the application.</span></span> <span data-ttu-id="151fc-112">Normalni użytkownicy mogą wyświetlać produkty i tworzyć zamówienia:</span><span class="sxs-lookup"><span data-stu-id="151fc-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="151fc-113">Administratorzy mogą tworzyć, usuwać lub edytować produkty:</span><span class="sxs-lookup"><span data-stu-id="151fc-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="151fc-114">Posiadane umiejętności</span><span class="sxs-lookup"><span data-stu-id="151fc-114">Skills You'll Learn</span></span>

<span data-ttu-id="151fc-115">Oto, co uzyskasz:</span><span class="sxs-lookup"><span data-stu-id="151fc-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="151fc-116">Jak używać Entity Framework z interfejsem API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="151fc-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="151fc-117">Jak używać narzędzia separowania. js do tworzenia interfejsu użytkownika klienta dynamicznego.</span><span class="sxs-lookup"><span data-stu-id="151fc-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="151fc-118">Jak uwierzytelniać użytkowników przy użyciu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="151fc-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="151fc-119">Mimo że ten samouczek jest samodzielny, warto najpierw przeczytać następujące samouczki:</span><span class="sxs-lookup"><span data-stu-id="151fc-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="151fc-120">Twój pierwszy ASP.NET internetowy interfejs API</span><span class="sxs-lookup"><span data-stu-id="151fc-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="151fc-121">Tworzenie internetowego interfejsu API obsługującego operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="151fc-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="151fc-122">Znajomość [ASP.NET MVC](../../../../mvc/index.md) jest również przydatna.</span><span class="sxs-lookup"><span data-stu-id="151fc-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="151fc-123">Omówienie</span><span class="sxs-lookup"><span data-stu-id="151fc-123">Overview</span></span>

<span data-ttu-id="151fc-124">Na wysokim poziomie poniżej przedstawiono architekturę aplikacji:</span><span class="sxs-lookup"><span data-stu-id="151fc-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="151fc-125">ASP.NET MVC generuje strony HTML dla klienta.</span><span class="sxs-lookup"><span data-stu-id="151fc-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="151fc-126">Interfejs API sieci Web ASP.NET uwidacznia operacje CRUD na danych (produkty i zamówienia).</span><span class="sxs-lookup"><span data-stu-id="151fc-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="151fc-127">Entity Framework tłumaczy C# modele używane przez internetowy interfejs API do jednostek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="151fc-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="151fc-128">Na poniższym diagramie przedstawiono sposób reprezentowania obiektów domeny w różnych warstwach aplikacji: warstwy bazy danych, modelu obiektów i na końcu formatu sieci, który jest używany do przesyłania danych do klienta za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="151fc-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="151fc-129">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="151fc-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="151fc-130">Możesz utworzyć projekt samouczka przy użyciu programu Visual Web Developer Express lub pełnej wersji programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="151fc-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="151fc-131">Na stronie **startowej** kliknij pozycję **Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="151fc-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="151fc-132">W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  .</span><span class="sxs-lookup"><span data-stu-id="151fc-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="151fc-133">W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="151fc-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="151fc-134">Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="151fc-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="151fc-135">Nadaj projektowi nazwę "ProductStore" i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="151fc-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="151fc-136">W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **aplikacja internetowa** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="151fc-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="151fc-137">Szablon "aplikacja internetowa" tworzy aplikację ASP.NET MVC, która obsługuje uwierzytelnianie formularzy.</span><span class="sxs-lookup"><span data-stu-id="151fc-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="151fc-138">Jeśli aplikacja zostanie uruchomiona teraz, ma już pewne funkcje:</span><span class="sxs-lookup"><span data-stu-id="151fc-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="151fc-139">Nowi użytkownicy mogą zarejestrować się, klikając link "Register" w prawym górnym rogu.</span><span class="sxs-lookup"><span data-stu-id="151fc-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="151fc-140">Zarejestrowani użytkownicy mogą się zalogować, klikając łącze "Zaloguj się".</span><span class="sxs-lookup"><span data-stu-id="151fc-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="151fc-141">Informacje o członkostwie są utrwalane w bazie danych, która jest tworzona automatycznie.</span><span class="sxs-lookup"><span data-stu-id="151fc-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="151fc-142">Aby uzyskać więcej informacji na temat uwierzytelniania formularzy w ASP.NET MVC, zobacz [Przewodnik: używanie uwierzytelniania formularzy w ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="151fc-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="151fc-143">Aktualizowanie pliku CSS</span><span class="sxs-lookup"><span data-stu-id="151fc-143">Update the CSS File</span></span>

<span data-ttu-id="151fc-144">Ten krok to element kosmetyczny, ale strony będą renderowane jak wcześniejsze zrzuty ekranu.</span><span class="sxs-lookup"><span data-stu-id="151fc-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="151fc-145">W Eksplorator rozwiązań rozwiń folder Content (zawartość) i Otwórz plik o nazwie Site. css.</span><span class="sxs-lookup"><span data-stu-id="151fc-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="151fc-146">Dodaj następujące style CSS:</span><span class="sxs-lookup"><span data-stu-id="151fc-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="151fc-147">Dalej</span><span class="sxs-lookup"><span data-stu-id="151fc-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)

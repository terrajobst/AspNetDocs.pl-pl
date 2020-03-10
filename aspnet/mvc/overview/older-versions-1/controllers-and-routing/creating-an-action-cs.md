---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Tworzenie akcji (C#) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metody jako akcji.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: ebba935383819935ad85c95245666f4eaf6a0dca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582055"
---
# <a name="creating-an-action-c"></a><span data-ttu-id="adfde-104">Tworzenie akcji (C#)</span><span class="sxs-lookup"><span data-stu-id="adfde-104">Creating an Action (C#)</span></span>

<span data-ttu-id="adfde-105">przez [firmę Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="adfde-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="adfde-106">Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="adfde-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="adfde-107">Dowiedz się więcej o wymaganiach dotyczących metody jako akcji.</span><span class="sxs-lookup"><span data-stu-id="adfde-107">Learn about the requirements for a method to be an action.</span></span>

<span data-ttu-id="adfde-108">Celem tego samouczka jest wyjaśnienie, jak można utworzyć nową akcję kontrolera.</span><span class="sxs-lookup"><span data-stu-id="adfde-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="adfde-109">Dowiesz się więcej na temat wymagań metody akcji.</span><span class="sxs-lookup"><span data-stu-id="adfde-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="adfde-110">Dowiesz się również, jak zapobiec ujawnieniu metody jako akcji.</span><span class="sxs-lookup"><span data-stu-id="adfde-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="adfde-111">Dodawanie akcji do kontrolera</span><span class="sxs-lookup"><span data-stu-id="adfde-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="adfde-112">Dodaj nową akcję do kontrolera, dodając nową metodę do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="adfde-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="adfde-113">Na przykład kontroler z listą 1 zawiera akcję o nazwie index () i akcję o nazwie SayHello ().</span><span class="sxs-lookup"><span data-stu-id="adfde-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="adfde-114">Obie metody są udostępniane jako akcje.</span><span class="sxs-lookup"><span data-stu-id="adfde-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="adfde-115">**Lista 1 — Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="adfde-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="adfde-116">Aby można było uwidocznić w programie Universe jako czynność, metoda musi spełniać pewne wymagania:</span><span class="sxs-lookup"><span data-stu-id="adfde-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="adfde-117">Metoda musi być publiczna.</span><span class="sxs-lookup"><span data-stu-id="adfde-117">The method must be public.</span></span>
- <span data-ttu-id="adfde-118">Metoda nie może być metodą statyczną.</span><span class="sxs-lookup"><span data-stu-id="adfde-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="adfde-119">Metoda nie może być metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="adfde-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="adfde-120">Metoda nie może być konstruktora, metody pobierającej ani metody ustawiającej.</span><span class="sxs-lookup"><span data-stu-id="adfde-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="adfde-121">Metoda nie może mieć otwartych typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="adfde-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="adfde-122">Metoda nie jest metodą klasy bazowej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="adfde-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="adfde-123">Metoda nie może zawierać parametrów **ref** ani **out** .</span><span class="sxs-lookup"><span data-stu-id="adfde-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="adfde-124">Zwróć uwagę, że nie ma żadnych ograniczeń dla zwracanego typu akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="adfde-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="adfde-125">Akcja kontrolera może zwracać ciąg, typ DateTime, wystąpienie klasy losowej lub typ void.</span><span class="sxs-lookup"><span data-stu-id="adfde-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="adfde-126">Platforma ASP.NET MVC przekonwertuje wszystkie typy zwracane, które nie są wynikiem akcji na ciąg i renderuje ciąg do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="adfde-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="adfde-127">Po dodaniu dowolnej metody, która nie narusza tych wymagań dla kontrolera, metoda jest uwidaczniana jako akcja kontrolera.</span><span class="sxs-lookup"><span data-stu-id="adfde-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="adfde-128">Tutaj należy zachować ostrożność.</span><span class="sxs-lookup"><span data-stu-id="adfde-128">Be careful here.</span></span> <span data-ttu-id="adfde-129">Akcja kontrolera może być wywoływana przez dowolną osobę, która jest połączona z Internetem.</span><span class="sxs-lookup"><span data-stu-id="adfde-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="adfde-130">Nie można na przykład utworzyć akcji kontrolera DeleteMyWebsite ().</span><span class="sxs-lookup"><span data-stu-id="adfde-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="adfde-131">Uniemożliwianie wywołania metody publicznej</span><span class="sxs-lookup"><span data-stu-id="adfde-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="adfde-132">Jeśli musisz utworzyć metodę publiczną w klasie kontrolera i nie chcesz ujawniać metody jako akcji kontrolera, możesz uniemożliwić wywoływanie metody przy użyciu atrybutu [unaction].</span><span class="sxs-lookup"><span data-stu-id="adfde-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="adfde-133">Na przykład kontroler na liście 2 zawiera metodę publiczną o nazwie CompanySecrets (), która jest uzupełniona o atrybut [unaction].</span><span class="sxs-lookup"><span data-stu-id="adfde-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="adfde-134">**Lista 2 — Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="adfde-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="adfde-135">Jeśli spróbujesz wywołać akcję kontrolera CompanySecrets (), wpisując/Work/CompanySecrets na pasku adresu przeglądarki, zostanie wyświetlony komunikat o błędzie na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="adfde-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>

<span data-ttu-id="adfde-136">[![wywoływania metody niefunkcjonalnej](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="adfde-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="adfde-137">**Ilustracja 01**: wywoływanie metody niefunkcjonalnej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="adfde-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="adfde-138">[Poprzednie](creating-a-controller-cs.md)
> [dalej](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="adfde-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>

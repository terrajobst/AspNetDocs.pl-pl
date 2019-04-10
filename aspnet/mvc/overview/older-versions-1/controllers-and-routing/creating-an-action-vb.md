---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Tworzenie akcji (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać nową akcję do Kontroler składnika ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę jako akcję.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 5f8eeeaa9bd77c0259f680198e57ade8d49cd06b
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59382624"
---
# <a name="creating-an-action-vb"></a><span data-ttu-id="76614-104">Tworzenie akcji (VB)</span><span class="sxs-lookup"><span data-stu-id="76614-104">Creating an Action (VB)</span></span>

<span data-ttu-id="76614-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="76614-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="76614-106">Dowiedz się, jak dodać nową akcję do Kontroler składnika ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="76614-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="76614-107">Dowiedz się więcej o wymaganiach dotyczących metodę jako akcję.</span><span class="sxs-lookup"><span data-stu-id="76614-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="76614-108">Celem tego samouczka jest wyjaśniają, jak można utworzyć nowej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="76614-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="76614-109">Dowiesz się o wymagania dotyczące metody akcji.</span><span class="sxs-lookup"><span data-stu-id="76614-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="76614-110">Poznasz również sposób zapobiec metody przed przypadkowym jako akcję.</span><span class="sxs-lookup"><span data-stu-id="76614-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="76614-111">Dodawanie akcji do kontrolera</span><span class="sxs-lookup"><span data-stu-id="76614-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="76614-112">Możesz dodać nową akcję do kontrolera, dodając nową metodę do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="76614-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="76614-113">Na przykład kontroler w ofercie 1 zawiera akcję o nazwie indeks() i akcji, o nazwie SayHello().</span><span class="sxs-lookup"><span data-stu-id="76614-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="76614-114">Obie metody są widoczne jako akcje.</span><span class="sxs-lookup"><span data-stu-id="76614-114">Both methods are exposed as actions.</span></span>

**<span data-ttu-id="76614-115">Wyświetlanie listy 1 - Controllers\HomeController.vb</span><span class="sxs-lookup"><span data-stu-id="76614-115">Listing 1 - Controllers\HomeController.vb</span></span>**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="76614-116">Aby można ujawnić modelowi wszechświat jako akcję, metody muszą spełniać określone wymagania:</span><span class="sxs-lookup"><span data-stu-id="76614-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="76614-117">Metoda muszą być publiczne.</span><span class="sxs-lookup"><span data-stu-id="76614-117">The method must be public.</span></span>
- <span data-ttu-id="76614-118">Metoda nie może być metodą statyczną.</span><span class="sxs-lookup"><span data-stu-id="76614-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="76614-119">Metoda nie może być metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="76614-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="76614-120">Metoda nie może być Konstruktor, metody pobierającej lub ustawiającej.</span><span class="sxs-lookup"><span data-stu-id="76614-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="76614-121">Metoda nie może mieć otwartych typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="76614-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="76614-122">Metoda nie jest metodą klasy bazowej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="76614-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="76614-123">Metoda nie może zawierać **ref** lub **się** parametrów.</span><span class="sxs-lookup"><span data-stu-id="76614-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="76614-124">Należy zauważyć, że nie ma żadnych ograniczeń w zwracanym typie akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="76614-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="76614-125">Akcja kontrolera może zwracać ciąg, DateTime, wystąpienie klasy Random lub void.</span><span class="sxs-lookup"><span data-stu-id="76614-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="76614-126">Platforma ASP.NET MVC spowoduje Konwertuj zwracany typ, który nie jest wynik akcji na ciąg i renderowania ciąg do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="76614-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="76614-127">Po dodaniu dowolnej metody, które naruszają tych wymagań z kontrolerem, metoda jest ujawniona jako akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="76614-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="76614-128">Należy zachować ostrożność, w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="76614-128">Be careful here.</span></span> <span data-ttu-id="76614-129">Akcja kontrolera może być wywoływany przez każdy komputer połączony z Internetem.</span><span class="sxs-lookup"><span data-stu-id="76614-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="76614-130">Nie, na przykład utworzyć DeleteMyWebsite() akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="76614-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="76614-131">Uniemożliwienie wywoływana przez publiczną metodę</span><span class="sxs-lookup"><span data-stu-id="76614-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="76614-132">Jeśli musisz utworzyć publiczną metodę w klasie kontrolera i nie chcesz ujawniać metody akcji kontrolera, a następnie można zapobiec metodę wywoływaną za pomocą &lt;NonAction&gt; atrybutu.</span><span class="sxs-lookup"><span data-stu-id="76614-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="76614-133">Na przykład kontroler w ofercie 2 zawiera publiczną metodę o nazwie CompanySecrets(), który zostanie nadany &lt;NonAction&gt; atrybutu.</span><span class="sxs-lookup"><span data-stu-id="76614-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

**<span data-ttu-id="76614-134">Wyświetlanie listy 2 - Controllers\WorkController.vb</span><span class="sxs-lookup"><span data-stu-id="76614-134">Listing 2 - Controllers\WorkController.vb</span></span>**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="76614-135">Jeśli użytkownik podejmie próbę wywołania akcji kontrolera CompanySecrets(), wpisując /Work/CompanySecrets na pasku adresu przeglądarki następnie otrzymasz komunikat o błędzie na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="76614-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


[![I<span data-ttu-id="76614-136">Metoda NonAction nvoking]</span><span class="sxs-lookup"><span data-stu-id="76614-136">nvoking a NonAction method]</span></span>(creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

<span data-ttu-id="76614-137">**Rysunek 01**: Wywoływanie metody NonAction ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="76614-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="76614-138">[Poprzednie](creating-a-controller-vb.md)
> [dalej](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="76614-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>

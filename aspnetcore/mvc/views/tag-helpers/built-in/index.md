---
title: Pomocnicy tagów wbudowanych w platformy ASP.NET Core
author: pkellner
description: 'Dowiedz się, jak pomocnicy tagów wbudowanych w platformy ASP.NET Core zwiększyć produktywność.'
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/Index
---

# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="7f881-103">Pomocnicy tagów wbudowanych w platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f881-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="7f881-104">Przez [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="7f881-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="7f881-105">Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="7f881-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

> [!NOTE]
> <span data-ttu-id="7f881-106">Istnieją wbudowane pomocnicy tagów, które nie są opisane w dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="7f881-106">There are built-in Tag Helpers which aren't described in the documentation.</span></span> <span data-ttu-id="7f881-107">Pomocnicy tagów, te są używane wewnętrznie przez [Razor](xref:mvc/views/razor) aparatu widoku.</span><span class="sxs-lookup"><span data-stu-id="7f881-107">These Tag Helpers are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="7f881-108">Obejmuje to pomocnika tagów dla `~` litery (tylda), która rozszerza się, aby ścieżka katalogu głównego witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7f881-108">This includes a Tag Helper for the `~` (tilde) character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="7f881-109">Pomocnicy tagów wbudowanych platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f881-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="7f881-110">**[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="7f881-111">**[Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="7f881-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="7f881-113">**[Pomocnik tagu środowiska](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="7f881-114">**[Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="7f881-115">**[Pomocnik tagu obrazu](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="7f881-116">**[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="7f881-117">**[Pomocnik tagu etykiet](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="7f881-118">**[Pomocnik tagu częściowego](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="7f881-119">**[Pomocnik tagu SELECT](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="7f881-120">**[Pomocnik tagu TextArea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="7f881-121">**[Pomocnik tagu komunikat sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="7f881-122">**[Pomocnik tagu podsumowania sprawdzania poprawności](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="7f881-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f881-123">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7f881-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>

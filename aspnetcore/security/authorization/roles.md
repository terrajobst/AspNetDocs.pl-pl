---
title: Autoryzacja oparta na rolach w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ograniczyć dostęp do akcji i kontrolerów platformy ASP.NET Core, przekazując ról z atrybutem autoryzacji.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: c38e7144166ce7741eee6e3acb4d1c952ad4f024
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074969"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="b8fda-103">Autoryzacja oparta na rolach w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8fda-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="b8fda-104">Po utworzeniu tożsamości może on należeć do co najmniej jedną rolę.</span><span class="sxs-lookup"><span data-stu-id="b8fda-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="b8fda-105">Na przykład Tracy mogą należeć do ról administratora i użytkownika, o ile Scott może wyłącznie należeć do roli użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b8fda-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="b8fda-106">Jak te role są tworzone i zarządzane zależy od magazyn zapasowy procesu autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="b8fda-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="b8fda-107">Role są widoczne dla deweloperów za pośrednictwem [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) metody [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) klasy.</span><span class="sxs-lookup"><span data-stu-id="b8fda-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="b8fda-108">Dodawanie kontroli roli</span><span class="sxs-lookup"><span data-stu-id="b8fda-108">Adding role checks</span></span>

<span data-ttu-id="b8fda-109">Sprawdzanie autoryzacji opartej na rolach są deklaratywne&mdash;Deweloper umieszcza je w ramach ich kodować, kontrolera lub akcji w kontrolerze, określając role, które bieżący użytkownik musi należeć do dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="b8fda-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="b8fda-110">Na przykład, poniższy kod ogranicza dostęp do wszystkich działań na `AdministrationController` dla użytkowników, którzy są członkami `Administrator` roli:</span><span class="sxs-lookup"><span data-stu-id="b8fda-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="b8fda-111">Można określić wiele ról jako listę rozdzielonych przecinkami:</span><span class="sxs-lookup"><span data-stu-id="b8fda-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="b8fda-112">Ten kontroler będą tylko dostępne przez użytkowników, którzy są członkami z `HRManager` roli lub `Finance` roli.</span><span class="sxs-lookup"><span data-stu-id="b8fda-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="b8fda-113">Jeśli zastosujesz wiele atrybutów, a następnie uzyskiwanie dostępu do użytkownika musi być członkiem wszystkich ról, które są określone; Poniższy przykład wymaga, że użytkownik musi być członkiem zarówno `PowerUser` i `ControlPanelUser` roli.</span><span class="sxs-lookup"><span data-stu-id="b8fda-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="b8fda-114">Można zawęzić dostęp przez zastosowanie dodatkowych ról autoryzacji atrybuty na poziomie akcji:</span><span class="sxs-lookup"><span data-stu-id="b8fda-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

<span data-ttu-id="b8fda-115">W poprzednich składowych fragment kodu z `Administrator` roli lub `PowerUser` rola umożliwia dostęp do kontrolera i `SetTime` akcji, ale tylko członkowie `Administrator` rola umożliwia dostęp do `ShutDown` akcji.</span><span class="sxs-lookup"><span data-stu-id="b8fda-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="b8fda-116">Można również zablokować kontrolerem, ale zezwalaj na anonimowe, nieuwierzytelnione dostęp do poszczególnych działań.</span><span class="sxs-lookup"><span data-stu-id="b8fda-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b8fda-117">W przypadku stron Razor `AuthorizeAttribute` mogą być stosowane przez:</span><span class="sxs-lookup"><span data-stu-id="b8fda-117">For Razor Pages, the `AuthorizeAttribute` can be applied by either:</span></span>

* <span data-ttu-id="b8fda-118">Za pomocą [Konwencji](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), lub</span><span class="sxs-lookup"><span data-stu-id="b8fda-118">Using a [convention](xref:razor-pages/razor-pages-conventions#page-model-action-conventions), or</span></span>
* <span data-ttu-id="b8fda-119">Stosowanie `AuthorizeAttribute` do `PageModel` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="b8fda-119">Applying the `AuthorizeAttribute` to the `PageModel` instance:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public class UpdateModel : PageModel
{
    public ActionResult OnPost()
    {
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="b8fda-120">Filtrowanie atrybutów, w tym `AuthorizeAttribute`, może być stosowany tylko do klasy PageModel i nie można zastosować do metody obsługi określonej strony.</span><span class="sxs-lookup"><span data-stu-id="b8fda-120">Filter attributes, including `AuthorizeAttribute`, can only be applied to PageModel and cannot be applied to specific page handler methods.</span></span>
::: moniker-end


<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="b8fda-121">Uprawnienia ról oparte na zasadach</span><span class="sxs-lookup"><span data-stu-id="b8fda-121">Policy based role checks</span></span>

<span data-ttu-id="b8fda-122">Wymagania roli można również wyrazić za pomocą nowej składni zasad, których Deweloper rejestruje zasady przy uruchomieniu jako część konfiguracji usługi autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="b8fda-122">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="b8fda-123">Zwykle dzieje się to `ConfigureServices()` w swojej *Startup.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="b8fda-123">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole",
             policy => policy.RequireRole("Administrator"));
    });
}
```

<span data-ttu-id="b8fda-124">Zasady są stosowane przy użyciu `Policy` właściwość `AuthorizeAttribute` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="b8fda-124">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="b8fda-125">Jeśli chcesz określić wiele ról dozwolonych w zapotrzebowania, a następnie określić je jako parametry `RequireRole` metody:</span><span class="sxs-lookup"><span data-stu-id="b8fda-125">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="b8fda-126">W tym przykładzie autoryzowania użytkowników, którzy należą do `Administrator`, `PowerUser` lub `BackupAdministrator` ról.</span><span class="sxs-lookup"><span data-stu-id="b8fda-126">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>

### <a name="add-role-services-to-identity"></a><span data-ttu-id="b8fda-127">Dodaj usługi ról do tożsamości</span><span class="sxs-lookup"><span data-stu-id="b8fda-127">Add Role services to Identity</span></span>

<span data-ttu-id="b8fda-128">Dołącz [opcji Dodawanie ról](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) można dodać usługi ról:</span><span class="sxs-lookup"><span data-stu-id="b8fda-128">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](roles/samples/Startup.cs?name=snippet&highlight=7)]

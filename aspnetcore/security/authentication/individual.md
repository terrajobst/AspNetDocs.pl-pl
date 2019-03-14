---
title: Artykułów opartych na projektach programu ASP.NET Core utworzona za pomocą indywidualnych kont użytkowników
author: rick-anderson
description: Dowiedz się, artykułów opartych na projektach programu ASP.NET Core utworzona za pomocą indywidualnych kont użytkowników.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: c73365eafaf2c38ef02c3c83ccf5ced4264f7dc0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57068771"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="12416-103">Artykułów opartych na projektach programu ASP.NET Core utworzona za pomocą indywidualnych kont użytkowników</span><span class="sxs-lookup"><span data-stu-id="12416-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="12416-104">Tożsamość platformy ASP.NET Core znajduje się w szablonach projektu w programie Visual Studio przy użyciu opcji "Pojedyncze konta użytkowników".</span><span class="sxs-lookup"><span data-stu-id="12416-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="12416-105">Szablony uwierzytelniania są dostępne w interfejsie wiersza polecenia platformy .NET Core za pomocą `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="12416-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="12416-106">Zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/5833) uwierzytelniania interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="12416-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="12416-107">Bez uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="12416-107">No Authentication</span></span>

<span data-ttu-id="12416-108">Uwierzytelnianie jest określona w .NET Core interfejsu wiersza polecenia przy użyciu `-au` opcji.</span><span class="sxs-lookup"><span data-stu-id="12416-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="12416-109">W programie Visual Studio **Zmień uwierzytelnianie** okno dialogowe jest dostępna dla nowych aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="12416-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="12416-110">Wartość domyślna w przypadku nowych aplikacji sieci web w programie Visual Studio to **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="12416-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="12416-111">Projekty utworzone za pomocą bez uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="12416-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="12416-112">Nie zawierają stron sieci web i interfejs użytkownika do logowania i Wyloguj.</span><span class="sxs-lookup"><span data-stu-id="12416-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="12416-113">Nie zawierają kod uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="12416-113">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="12416-114">Uwierzytelnianie systemu Windows</span><span class="sxs-lookup"><span data-stu-id="12416-114">Windows Authentication</span></span>

<span data-ttu-id="12416-115">Uwierzytelnianie Windows jest określona dla nowej aplikacji sieci web w .NET Core interfejsu wiersza polecenia przy użyciu `-au Windows` opcji.</span><span class="sxs-lookup"><span data-stu-id="12416-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="12416-116">W programie Visual Studio **Zmień uwierzytelnianie** okno dialogowe udostępnia **uwierzytelniania Windows** opcje.</span><span class="sxs-lookup"><span data-stu-id="12416-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="12416-117">Jeśli wybrano opcję uwierzytelniania Windows, aplikacja jest skonfigurowana do używania [Moduł IIS uwierzytelniania Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="12416-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="12416-118">Uwierzytelnianie Windows jest przeznaczony dla witryn intranetowych.</span><span class="sxs-lookup"><span data-stu-id="12416-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12416-119">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="12416-119">Additional resources</span></span>

<span data-ttu-id="12416-120">Następujące artykuły pokazują, jak używać kod wygenerowany w szablony ASP.NET Core, korzystających z indywidualnych kont użytkowników:</span><span class="sxs-lookup"><span data-stu-id="12416-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="12416-121">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS</span><span class="sxs-lookup"><span data-stu-id="12416-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="12416-122">Potwierdzenie konta i odzyskiwanie hasła w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12416-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="12416-123">Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację</span><span class="sxs-lookup"><span data-stu-id="12416-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)

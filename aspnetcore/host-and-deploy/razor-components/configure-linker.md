---
title: Konfigurowanie konsolidatora dla Blazor
author: guardrex
description: Dowiedz się, jak kontrolować konsolidatora języka pośredniego (IL), podczas kompilowania aplikacji Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: 7c53e7912ec3b0ae471ea38777f874f55a32487d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57078029"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="09103-103">Konfigurowanie konsolidatora dla Blazor</span><span class="sxs-lookup"><span data-stu-id="09103-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="09103-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="09103-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="09103-105">Wykonuje Blazor [języka pośredniego (IL)](/dotnet/standard/managed-code#intermediate-language--execution) łączenia podczas każdej kompilacji tryb wersji, można usunąć niepotrzebne IL z zestawów danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="09103-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the output assemblies.</span></span>

<span data-ttu-id="09103-106">Można kontrolować zestawu konsolidowanie za pomocą jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="09103-106">You can control assembly linking with either of the following approaches:</span></span>

* <span data-ttu-id="09103-107">Wyłącz konsolidowanie globalnie za pomocą właściwości programu MSBuild.</span><span class="sxs-lookup"><span data-stu-id="09103-107">Disable linking globally with an MSBuild property.</span></span>
* <span data-ttu-id="09103-108">Kontrolka konsolidacji na podstawie poszczególnych zestawów przy użyciu pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="09103-108">Control linking on a per-assembly basis with a configuration file.</span></span>

## <a name="disable-linking-with-an-msbuild-property"></a><span data-ttu-id="09103-109">Wyłącz konsolidowanie za pomocą właściwości programu MSBuild</span><span class="sxs-lookup"><span data-stu-id="09103-109">Disable linking with an MSBuild property</span></span>

<span data-ttu-id="09103-110">Łączenie jest domyślnie włączanych w trybie wydania kompilowana jest aplikacja, która obejmuje publikowania.</span><span class="sxs-lookup"><span data-stu-id="09103-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="09103-111">Aby wyłączyć łączenie dla wszystkich zestawów, należy ustawić `<BlazorLinkOnBuild>` właściwości programu MSBuild `false` w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="09103-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="09103-112">Łączenie się z plikiem konfiguracyjnym kontroli</span><span class="sxs-lookup"><span data-stu-id="09103-112">Control linking with a configuration file</span></span>

<span data-ttu-id="09103-113">Łączenie mogą być kontrolowane na podstawie poszczególnych zestawów, zapewniając plik konfiguracyjny XML i określenie pliku jako element programu MSBuild w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="09103-113">Linking can be controlled on a per-assembly basis by providing an XML configuration file and specifying the file as an MSBuild item in the project file.</span></span>

<span data-ttu-id="09103-114">Poniżej przedstawiono przykładowy plik konfiguracji (*Linker.xml*):</span><span class="sxs-lookup"><span data-stu-id="09103-114">The following is an example configuration file (*Linker.xml*):</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/aspnet/Blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="09103-115">Aby dowiedzieć się więcej o formacie pliku dla pliku konfiguracji, zobacz [IL konsolidatora: Składnia xml deskryptora](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="09103-115">To learn more about the file format for the configuration file, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

<span data-ttu-id="09103-116">Określ plik konfiguracji w pliku projektu za pomocą `BlazorLinkerDescriptor` elementu:</span><span class="sxs-lookup"><span data-stu-id="09103-116">Specify the configuration file in the project file with the `BlazorLinkerDescriptor` item:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

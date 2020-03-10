---
uid: webhooks/diagnostics/debugging
title: Debugowanie elementów webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Jak debugować elementy webhook ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640869"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="5e43f-103">Debugowanie elementów webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5e43f-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="5e43f-104">Debugowanie na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="5e43f-104">Debugging in Azure</span></span>

<span data-ttu-id="5e43f-105">Aby debugować aplikację sieci Web podczas działania na platformie Azure, zobacz samouczek [Rozwiązywanie problemów z aplikacją sieci Web w Azure App Service przy użyciu programu Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="5e43f-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="5e43f-106">Debugowanie ze źródłem i symbolami</span><span class="sxs-lookup"><span data-stu-id="5e43f-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="5e43f-107">Oprócz debugowania własnego kodu możliwe jest debugowanie bezpośrednio do Microsoft ASP.NET elementów webhook, a w rzeczywistości wszystkie platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="5e43f-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="5e43f-108">Działa to niezależnie od tego, czy debugowanie odbywa się lokalnie, czy zdalnie.</span><span class="sxs-lookup"><span data-stu-id="5e43f-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="5e43f-109">Najpierw należy skonfigurować program Visual Studio do znajdowania źródła i symboli, przechodząc do **debugowania** , a następnie **Opcje i ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="5e43f-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="5e43f-110">Ustaw opcje w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5e43f-110">Set the options like this:</span></span>

![Opcje i ustawienia](_static/SourceSymbols.png)

<span data-ttu-id="5e43f-112">Następnie Dodaj link do [symbolsource.org](http://symbolsource.org) w celu pobrania źródła i symboli.</span><span class="sxs-lookup"><span data-stu-id="5e43f-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="5e43f-113">Przejdź do karty **symbole** w powyższym menu i Dodaj następujący element jako lokalizację symboli:</span><span class="sxs-lookup"><span data-stu-id="5e43f-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="5e43f-114">Ponadto upewnij się, że Katalog pamięci podręcznej ma krótką nazwę; w przeciwnym razie nazwy plików mogą być zbyt długie, co spowoduje, że symbole nie zostaną załadowane.</span><span class="sxs-lookup"><span data-stu-id="5e43f-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="5e43f-115">Przykładowa ścieżka:</span><span class="sxs-lookup"><span data-stu-id="5e43f-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="5e43f-116">Ustawienia powinny wyglądać podobnie do tego:</span><span class="sxs-lookup"><span data-stu-id="5e43f-116">The settings should look similar to this:</span></span>

![Przykład lokalizacji pliku symboli opcji](_static/SymSource.png)

---
title: Testowanie obciążenia/obciążeniowe platformy ASP.NET Core
author: Jeremy-Meng
description: W tym artykule opisano kilka istotnych narzędzi i podejścia do testowania obciążenia i aplikacje platformy ASP.NET Core testowanie obciążeniowe.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: d989bc841a372bed7ebf2c84c6abe1a57762ad04
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077084"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="14764-103">Obciążeniowe testowania platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14764-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="14764-104">Testowanie obciążenia i testy obciążenia są ważne, aby upewnić się, że aplikacja sieci web jest wydajne i skalowalne.</span><span class="sxs-lookup"><span data-stu-id="14764-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="14764-105">Swoje cele są inne nawet często mają podobne testów.</span><span class="sxs-lookup"><span data-stu-id="14764-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="14764-106">**Testy obciążenia**: Sprawdza, czy aplikacja może obsługiwać określonego obciążenia użytkowników dla niektórych scenariuszy spełniając nadal docelowy odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="14764-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="14764-107">Aplikacja jest uruchamiana w normalnych warunkach.</span><span class="sxs-lookup"><span data-stu-id="14764-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="14764-108">**Testy obciążeniowe**: Testy stabilność aplikacji podczas uruchamiania w ekstremalnych warunków i często dłuższy czas:</span><span class="sxs-lookup"><span data-stu-id="14764-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="14764-109">Duże obciążenie użytkownikami — wzrostów lub stopniowo zwiększając.</span><span class="sxs-lookup"><span data-stu-id="14764-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="14764-110">Ograniczone zasoby obliczeniowe.</span><span class="sxs-lookup"><span data-stu-id="14764-110">Limited computing resources.</span></span>  

<span data-ttu-id="14764-111">Przy dużym obciążeniu można ją odzyskiwanie po awarii i bez problemu zmieniała powrócić do oczekiwane zachowanie?</span><span class="sxs-lookup"><span data-stu-id="14764-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="14764-112">Przy dużym obciążeniu, aplikacja jest *nie* Uruchom w normalnych warunkach.</span><span class="sxs-lookup"><span data-stu-id="14764-112">Under stress, the app is *not* run under normal conditions.</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="14764-113">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="14764-113">Visual Studio Tools</span></span>

<span data-ttu-id="14764-114">Program Visual Studio pozwala użytkownikom na tworzenie, opracowywanie i debugowanie testów wydajności i obciążenia sieci web.</span><span class="sxs-lookup"><span data-stu-id="14764-114">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="14764-115">Opcja jest dostępna do utworzenia testów poprzez nagrywanie akcji w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="14764-115">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="14764-116">[Szybki start: Tworzenie projektu testu obciążeniowego](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) pokazuje, jak tworzenie, konfigurowanie i uruchamianie testu obciążenia projektów przy użyciu programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="14764-116">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="14764-117">Zobacz [dodatkowe zasoby](#add) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="14764-117">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="14764-118">Testy obciążenia można skonfigurować do uruchamiania w lokalnych lub działają w chmurze przy użyciu DevOps platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="14764-118">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="14764-119">Usługa Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="14764-119">Azure DevOps</span></span>

<span data-ttu-id="14764-120">Można uruchomić przebiegów testów obciążeniowych przy użyciu [plany testów Azure DevOps](/azure/devops/test/load-test/index?view=vsts) usługi.</span><span class="sxs-lookup"><span data-stu-id="14764-120">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="14764-121">Usługa obsługuje następujące typy format testu:</span><span class="sxs-lookup"><span data-stu-id="14764-121">The service supports the following types of test format:</span></span>

- <span data-ttu-id="14764-122">Test programu Visual Studio — testu sieci web w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14764-122">Visual Studio test – web test created in Visual Studio.</span></span>
- <span data-ttu-id="14764-123">Archiwum HTTP testami — przechwyconych ruch HTTP w ramach archiwum jest odtwarzany podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="14764-123">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
- <span data-ttu-id="14764-124">[Test oparty na adresach URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) — umożliwia określenie adresów URL można załadować testu, typy żądań, nagłówki i ciągi zapytań.</span><span class="sxs-lookup"><span data-stu-id="14764-124">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="14764-125">Uruchamianie, ustawianie parametrów, np. czas trwania, wzorca obciążenia, liczbę użytkowników itp., można skonfigurować.</span><span class="sxs-lookup"><span data-stu-id="14764-125">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
- <span data-ttu-id="14764-126">[Apache JMeter](https://jmeter.apache.org/) testu.</span><span class="sxs-lookup"><span data-stu-id="14764-126">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="14764-127">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="14764-127">Azure portal</span></span>

<span data-ttu-id="14764-128">[Portal systemu Azure pozwala konfigurowania i uruchamiania, testowanie obciążeniowe aplikacji sieci Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) bezpośrednio z karty wydajność usługi App Service w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="14764-128">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="14764-129">Test może być testu ręcznego z określonego adresu URL lub plik sieci Web Test programu Visual Studio, który można przetestować wiele adresów URL.</span><span class="sxs-lookup"><span data-stu-id="14764-129">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="14764-130">Na końcu testu raporty są generowane, aby pokazać charakterystyki wydajności aplikacji.</span><span class="sxs-lookup"><span data-stu-id="14764-130">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="14764-131">Statystyka przykład zawiera:</span><span class="sxs-lookup"><span data-stu-id="14764-131">Example statistics include:</span></span>

- <span data-ttu-id="14764-132">Średni czas odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="14764-132">Average response time</span></span>
- <span data-ttu-id="14764-133">Maksymalna przepływność: żądań na sekundę</span><span class="sxs-lookup"><span data-stu-id="14764-133">Max throughput: requests per second</span></span>
- <span data-ttu-id="14764-134">Procent niepowodzeń</span><span class="sxs-lookup"><span data-stu-id="14764-134">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="14764-135">Narzędzia innych producentów</span><span class="sxs-lookup"><span data-stu-id="14764-135">Third-party Tools</span></span>

<span data-ttu-id="14764-136">Poniższa lista zawiera narzędzia wydajności sieci web innych firm, z różnymi zestawami funkcji:</span><span class="sxs-lookup"><span data-stu-id="14764-136">The following list contains third-party web performance tools with various feature sets:</span></span>

- <span data-ttu-id="14764-137">[Apache JMeter](https://jmeter.apache.org/) : Pełny zestaw polecanych narzędziom do testowania obciążenia.</span><span class="sxs-lookup"><span data-stu-id="14764-137">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="14764-138">Powiązane z wątku: muszą jeden wątek na użytkownika.</span><span class="sxs-lookup"><span data-stu-id="14764-138">Thread-bound: need one thread per user.</span></span>
- [<span data-ttu-id="14764-139">AB — Apache HTTP server, narzędzia do testów porównawczych</span><span class="sxs-lookup"><span data-stu-id="14764-139">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
- <span data-ttu-id="14764-140">[Gatling](https://gatling.io/) : Narzędzia pulpitu za pomocą urządzenia graficznego interfejsu użytkownika i testowania.</span><span class="sxs-lookup"><span data-stu-id="14764-140">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="14764-141">Wydajniej niż JMeter.</span><span class="sxs-lookup"><span data-stu-id="14764-141">More performant than JMeter.</span></span>
- <span data-ttu-id="14764-142">[Locust.IO](https://locust.io/) : Nie są ograniczone przez wątków.</span><span class="sxs-lookup"><span data-stu-id="14764-142">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>
## <a name="additional-resources"></a><span data-ttu-id="14764-143">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="14764-143">Additional Resources</span></span>

<span data-ttu-id="14764-144">[Ładowanie serię wpisów w blogu testu](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) Autor: Charles Sterling.</span><span class="sxs-lookup"><span data-stu-id="14764-144">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="14764-145">Z dnia, ale nadal dotyczą większości tematów.</span><span class="sxs-lookup"><span data-stu-id="14764-145">Dated but most of the topics are still relevant.</span></span>

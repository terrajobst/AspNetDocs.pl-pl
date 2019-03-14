---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Wykrywanie klasy początkowej OWIN | Dokumentacja firmy Microsoft
author: Praburaj
description: W tym samouczku pokazano, jak skonfigurować klasy początkowej OWIN, które są ładowane. Aby uzyskać więcej informacji na temat OWIN Zobacz Omówienie projektu Katana. W tym samouczku został...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0b34cca8b48383dbb028106651758dff889ed614
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57070571"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="9e805-105">Wykrywanie klasy początkowej interfejsu OWIN</span><span class="sxs-lookup"><span data-stu-id="9e805-105">OWIN Startup Class Detection</span></span>
====================

> <span data-ttu-id="9e805-106">W tym samouczku pokazano, jak skonfigurować klasy początkowej OWIN, które są ładowane.</span><span class="sxs-lookup"><span data-stu-id="9e805-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="9e805-107">Aby uzyskać więcej informacji na temat OWIN, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="9e805-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="9e805-108">Ten samouczek został napisany przez Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj projektu i Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="9e805-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="9e805-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="9e805-109">Prerequisites</span></span>
>
> [<span data-ttu-id="9e805-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="9e805-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="9e805-111">Wykrywanie klasy początkowej interfejsu OWIN</span><span class="sxs-lookup"><span data-stu-id="9e805-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="9e805-112">Każda aplikacja OWIN ma klasę uruchamiania, w których określane są składniki do potoku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e805-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="9e805-113">Istnieją różne sposoby uruchamiania klasy można połączyć ze środowiskiem uruchomieniowym, w zależności od modelu hostingu wybierzesz (OwinHost, IIS i usług IIS Express).</span><span class="sxs-lookup"><span data-stu-id="9e805-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="9e805-114">Klasa początkowa przedstawiona w tym samouczku można w każdej aplikacji macierzystej.</span><span class="sxs-lookup"><span data-stu-id="9e805-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="9e805-115">Klasa startowa. Połącz się z hostingu środowiska uruchomieniowego przy użyciu których zbliża się do jednej z tych:</span><span class="sxs-lookup"><span data-stu-id="9e805-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="9e805-116">**Konwencje nazewnictwa**: Katana szuka klasę o nazwie `Startup` w przestrzeni nazw, nazwa zestawu lub globalnej przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="9e805-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="9e805-117">**Atrybut OwinStartup**: To podejście, zajmie większość deweloperów w celu określenia klasy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9e805-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="9e805-118">Następujący atrybut ustawi klasa startowa `TestStartup` klasy w `StartupDemo` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="9e805-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="9e805-119">`OwinStartup` Konwencji nazewnictwa przesłonięcia atrybutów.</span><span class="sxs-lookup"><span data-stu-id="9e805-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="9e805-120">Można również określić przyjazną nazwę, z tego atrybutu, jednak przy użyciu przyjaznej nazwy, musisz również użyć `appSetting` elementu w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9e805-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="9e805-121">**Element appSetting w pliku konfiguracyjnym**: `appSetting` Zastępuje element `OwinStartup` atrybut i konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="9e805-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="9e805-122">Możliwość posiadania wielu klas uruchamiania (przy każdym użyciu `OwinStartup` atrybutu) i skonfigurować, która klasa uruchamiania zostanie załadowany w pliku konfiguracji, za pomocą znaczników podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="9e805-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="9e805-123">Można także następujący klucz, który jawnie określa klasę uruchamiania i zestawu:</span><span class="sxs-lookup"><span data-stu-id="9e805-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="9e805-124">Następujący kod XML w pliku konfiguracyjnym Określa nazwę klasy przyjazne uruchamiania `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="9e805-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="9e805-125">Powyższe znaczników musi być używany z następujących `OwinStartup` atrybut, który określa przyjazną nazwę i powoduje, że `ProductionStartup2` klasy do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="9e805-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="9e805-126">Aby wyłączyć Dodaj OWIN uruchamiania odnajdywania `appSetting owin:AutomaticAppStartup` o wartości `"false"` w pliku web.config.</span><span class="sxs-lookup"><span data-stu-id="9e805-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="9e805-127">Tworzenie aplikacji internetowej ASP.NET przy użyciu początkowa OWIN</span><span class="sxs-lookup"><span data-stu-id="9e805-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="9e805-128">Utwórz pustą aplikację sieci web platformy Asp.Net i nadaj mu **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="9e805-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="9e805-129">-Zainstaluj `Microsoft.Owin.Host.SystemWeb` przy użyciu Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="9e805-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="9e805-130">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="9e805-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="9e805-131">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9e805-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="9e805-132">Dodaj klasę początkową OWIN.</span><span class="sxs-lookup"><span data-stu-id="9e805-132">Add an OWIN startup class.</span></span> <span data-ttu-id="9e805-133">W programie Visual Studio 2017 kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**. — w **Dodaj nowy element** okna dialogowego wprowadź *OWIN* w polu wyszukiwania, a następnie zmień nazwę pliku Startup.cs, a następnie wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="9e805-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="9e805-134">Następnym razem, aby dodać *Klasa początkowa Owin*, będzie on w dostępnym **Dodaj** menu.</span><span class="sxs-lookup"><span data-stu-id="9e805-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="9e805-135">Alternatywnie, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz **nowy element**, a następnie wybierz pozycję **Klasa początkowa Owin**.</span><span class="sxs-lookup"><span data-stu-id="9e805-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="9e805-136">Zastąp kod wygenerowany w *Startup.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9e805-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="9e805-137">`app.Use` Wyrażenie lambda jest używane do rejestrowania składnika określonego oprogramowania pośredniczącego do potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="9e805-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="9e805-138">W tym przypadku konfigurujemy rejestrowania żądań przychodzących przed udzieleniem odpowiedzi na żądanie przychodzące.</span><span class="sxs-lookup"><span data-stu-id="9e805-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="9e805-139">`next` Parametrem jest delegat ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [zadań](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) na następny składnik w potoku.</span><span class="sxs-lookup"><span data-stu-id="9e805-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="9e805-140">`app.Run` Wyrażenie lambda przechwytuje się Potok żądań przychodzących i udostępnia mechanizm odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="9e805-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="9e805-141">W powyższym kodzie możemy zostały oznaczone jako komentarz `OwinStartup` atrybutu, a my one polegania na Konwencji uruchamiania klasę o nazwie `Startup` .-Naciśnij ***F5*** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e805-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="9e805-142">Następnie kliknij przycisk Odśwież kilka razy.</span><span class="sxs-lookup"><span data-stu-id="9e805-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="9e805-143">![](owin-startup-class-detection/_static/image4.png) Uwaga: Liczby wyświetlanej na ilustracjach w tym samouczku nie będą zgodne, zostanie wyświetlona liczba.</span><span class="sxs-lookup"><span data-stu-id="9e805-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="9e805-144">Ciąg milisekund jest używany do wyświetlenia nowej odpowiedzi po odświeżeniu strony.</span><span class="sxs-lookup"><span data-stu-id="9e805-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="9e805-145">Można wyświetlić informacje o śledzeniu w **dane wyjściowe** okna.</span><span class="sxs-lookup"><span data-stu-id="9e805-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="9e805-146">Dodaj więcej klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="9e805-146">Add More Startup Classes</span></span>

<span data-ttu-id="9e805-147">W tej sekcji dodamy innej klasy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9e805-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="9e805-148">Możesz dodać wiele klasy początkowej OWIN do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9e805-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="9e805-149">Na przykład możesz tworzyć klasy startowy do tworzenia, testowania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="9e805-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="9e805-150">Utwórz nową klasę początkowa OWIN i nadaj mu nazwę `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="9e805-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="9e805-151">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="9e805-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="9e805-152">Naciśnij klawisz F5 sterowania, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="9e805-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="9e805-153">`OwinStartup` Atrybut określa, klasa początkowa produkcji jest uruchamiany.</span><span class="sxs-lookup"><span data-stu-id="9e805-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="9e805-154">Utwórz inny Klasa początkowa OWIN i nadaj mu `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="9e805-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="9e805-155">Zastąp wygenerowany kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="9e805-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="9e805-156">`OwinStartup` Określa atrybut przeciążenia powyżej `TestingConfiguration` jako *przyjazna* Nazwa klasy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9e805-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="9e805-157">Otwórz *web.config* pliku i Dodaj klucz uruchomienia aplikacji OWIN, który określa przyjazną nazwę klasy uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="9e805-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="9e805-158">Naciśnij klawisz F5 sterowania, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="9e805-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="9e805-159">Element ustawień aplikacji zajmuje poprzedzających i test jest uruchamiany w konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="9e805-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="9e805-160">Usuń *przyjazna* nazwę z `OwinStartup` atrybutu w `TestStartup` klasy.</span><span class="sxs-lookup"><span data-stu-id="9e805-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="9e805-161">Zamień na klucz uruchomienia aplikacji OWIN w *web.config* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9e805-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="9e805-162">Przywróć `OwinStartup` atrybutu w każdej klasie kodu atrybut domyślny wygenerowany przez program Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9e805-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="9e805-163">Poniższych kluczy uruchamiania aplikacji OWIN spowoduje, że klasa produkcji do uruchomienia.</span><span class="sxs-lookup"><span data-stu-id="9e805-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="9e805-164">Klucz uruchomienia ostatniego określa metody konfiguracji uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="9e805-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="9e805-165">Następujący klucz uruchomienia aplikacji OWIN pozwala zmienić nazwę klasy konfiguracji do `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="9e805-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="9e805-166">Za pomocą Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="9e805-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="9e805-167">Zastąp plik Web.config następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9e805-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="9e805-168">Klucz ostatnio wygrywa, więc w tym przypadku `TestStartup` jest określony.</span><span class="sxs-lookup"><span data-stu-id="9e805-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="9e805-169">Zainstaluj Owinhost z konsoli zarządzania Pakietami:</span><span class="sxs-lookup"><span data-stu-id="9e805-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="9e805-170">Przejdź do folderu aplikacji (folder zawierający *Web.config* plików) i w wierszu polecenia i wpisz:</span><span class="sxs-lookup"><span data-stu-id="9e805-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="9e805-171">W oknie polecenia będą wyświetlane:</span><span class="sxs-lookup"><span data-stu-id="9e805-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="9e805-172">Uruchom przeglądarkę z adresem URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="9e805-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="9e805-173">OwinHost honorowane konwencje uruchamiania wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="9e805-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="9e805-174">W oknie wiersza polecenia naciśnij klawisz Enter, aby zakończyć OwinHost.</span><span class="sxs-lookup"><span data-stu-id="9e805-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="9e805-175">W `ProductionStartup` klasy, Dodaj następujący atrybut OwinStartup, który określa przyjazną nazwę *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="9e805-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="9e805-176">W wierszu polecenia i wpisz:</span><span class="sxs-lookup"><span data-stu-id="9e805-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="9e805-177">Klasa początkowa produkcji jest ładowany.</span><span class="sxs-lookup"><span data-stu-id="9e805-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="9e805-178">Nasza aplikacja ma wiele klas uruchamiania, a w tym przykładzie firma Microsoft mają odłożone której klasy uruchamiania do obciążenia aż do czasu.</span><span class="sxs-lookup"><span data-stu-id="9e805-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="9e805-179">Przetestuj następujące opcje uruchamiania środowiska uruchomieniowego:</span><span class="sxs-lookup"><span data-stu-id="9e805-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]

---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Wykrywanie klasy startowej OWIN | Microsoft Docs
author: Praburaj
description: W tym samouczku pokazano, jak skonfigurować klasę uruchomieniową OWIN. Aby uzyskać więcej informacji na temat OWIN, zobacz Omówienie projektu Katana. Ten samouczek został...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617048"
---
# <a name="owin-startup-class-detection"></a><span data-ttu-id="b707a-105">Wykrywanie klasy początkowej interfejsu OWIN</span><span class="sxs-lookup"><span data-stu-id="b707a-105">OWIN Startup Class Detection</span></span>

> <span data-ttu-id="b707a-106">W tym samouczku pokazano, jak skonfigurować klasę uruchomieniową OWIN.</span><span class="sxs-lookup"><span data-stu-id="b707a-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="b707a-107">Aby uzyskać więcej informacji na temat OWIN, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="b707a-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="b707a-108">Ten samouczek został utworzony przez Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan i Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="b707a-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="b707a-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b707a-109">Prerequisites</span></span>
>
> [<span data-ttu-id="b707a-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b707a-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a><span data-ttu-id="b707a-111">Wykrywanie klasy początkowej interfejsu OWIN</span><span class="sxs-lookup"><span data-stu-id="b707a-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="b707a-112">Każda aplikacja OWIN ma klasę uruchomieniową, w której można określić składniki dla potoku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b707a-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="b707a-113">Istnieją różne sposoby łączenia klasy uruchamiania z środowiskiem uruchomieniowym w zależności od wybranego modelu hostingu (OwinHost, IIS i IIS-Express).</span><span class="sxs-lookup"><span data-stu-id="b707a-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="b707a-114">Klasa startowa wyświetlana w tym samouczku może być używana w każdej aplikacji hostingowej.</span><span class="sxs-lookup"><span data-stu-id="b707a-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="b707a-115">Klasę uruchomieniową można połączyć z środowiskiem uruchomieniowym hostingu przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="b707a-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="b707a-116">**Konwencja nazewnictwa**: Katana szuka klasy o nazwie `Startup` w przestrzeni nazw zgodnej z nazwą zestawu lub globalną przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="b707a-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="b707a-117">**OwinStartup — atrybut**: jest to podejście większości deweloperów do określenia klasy uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="b707a-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="b707a-118">Następujący atrybut ustawi klasę uruchomieniową na klasę `TestStartup` w przestrzeni nazw `StartupDemo`.</span><span class="sxs-lookup"><span data-stu-id="b707a-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="b707a-119">Atrybut `OwinStartup` zastępuje konwencję nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="b707a-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="b707a-120">Można również określić przyjazną nazwę z tym atrybutem, jednak użycie przyjaznej nazwy wymaga również użycia elementu `appSetting` w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b707a-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="b707a-121">**Element element appSetting w pliku konfiguracji**: element `appSetting` zastępuje atrybut `OwinStartup` i konwencję nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="b707a-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="b707a-122">Można mieć wiele klas uruchamiania (z których każdy korzysta z `OwinStartup` atrybutu) i skonfigurować klasę uruchomieniową, która zostanie załadowana w pliku konfiguracji przy użyciu znacznika podobnego do poniższego:</span><span class="sxs-lookup"><span data-stu-id="b707a-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="b707a-123">Następujący klucz, który jawnie określa klasę startową i zestaw, można również użyć:</span><span class="sxs-lookup"><span data-stu-id="b707a-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="b707a-124">Poniższy kod XML w pliku konfiguracji określa przyjazną nazwę klasy startowej `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="b707a-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="b707a-125">Powyższe oznakowanie musi być używane z poniższym atrybutem `OwinStartup`, który określa przyjazną nazwę i powoduje uruchomienie klasy `ProductionStartup2`.</span><span class="sxs-lookup"><span data-stu-id="b707a-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="b707a-126">Aby wyłączyć odnajdywanie uruchamiania OWIN Dodaj `appSetting owin:AutomaticAppStartup` z wartością `"false"` w pliku Web. config.</span><span class="sxs-lookup"><span data-stu-id="b707a-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="b707a-127">Tworzenie aplikacji sieci Web ASP.NET przy użyciu uruchamiania programu OWIN</span><span class="sxs-lookup"><span data-stu-id="b707a-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="b707a-128">Utwórz pustą aplikację sieci Web Asp.Net i nadaj jej nazwę **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="b707a-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="b707a-129">-Zainstaluj `Microsoft.Owin.Host.SystemWeb` przy użyciu Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="b707a-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="b707a-130">W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie **konsolę Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="b707a-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="b707a-131">Wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="b707a-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="b707a-132">Dodaj klasę uruchomieniową OWIN.</span><span class="sxs-lookup"><span data-stu-id="b707a-132">Add an OWIN startup class.</span></span> <span data-ttu-id="b707a-133">W programie Visual Studio 2017 kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj klasę**. w oknie dialogowym **Dodaj nowy element** wprowadź *Owin* w polu wyszukiwania, a następnie zmień nazwę na Startup.cs, a następnie wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b707a-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="b707a-134">Następnym razem, gdy chcesz dodać *klasę uruchomieniową Owin*, będzie ona dostępna w menu **Dodaj** .</span><span class="sxs-lookup"><span data-stu-id="b707a-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="b707a-135">Alternatywnie możesz kliknąć prawym przyciskiem myszy projekt i wybrać polecenie **Dodaj**, a następnie wybrać pozycję **nowy element**, a następnie wybrać **klasę uruchomieniową Owin**.</span><span class="sxs-lookup"><span data-stu-id="b707a-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="b707a-136">Zastąp wygenerowany kod w pliku *Startup.cs* następującym:</span><span class="sxs-lookup"><span data-stu-id="b707a-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="b707a-137">`app.Use` wyrażenie lambda służy do rejestrowania określonego składnika pośredniczącego w potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="b707a-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="b707a-138">W takim przypadku konfigurujemy Rejestrowanie żądań przychodzących przed odpowiedzią na żądanie przychodzące.</span><span class="sxs-lookup"><span data-stu-id="b707a-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="b707a-139">Parametr `next` jest delegatem ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [zadanie](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) do następnego składnika w potoku.</span><span class="sxs-lookup"><span data-stu-id="b707a-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="b707a-140">`app.Run` wyrażenie lambda przechwytuje potok do żądań przychodzących i udostępnia mechanizm odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b707a-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="b707a-141">W powyższym kodzie odnosimy się do atrybutu `OwinStartup` i opieramy się na Konwencji uruchamiania klasy o nazwie `Startup`.-Naciśnij klawisz ***F5*** , aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b707a-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="b707a-142">Wielokrotnie Odświeżaj.</span><span class="sxs-lookup"><span data-stu-id="b707a-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="b707a-143">![](owin-startup-class-detection/_static/image4.png) Uwaga: Liczba wyświetlana w obrazach w tym samouczku nie jest zgodna z liczbą wyświetlaną.</span><span class="sxs-lookup"><span data-stu-id="b707a-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="b707a-144">Ciąg milisekundy jest używany do wyświetlania nowej odpowiedzi po odświeżeniu strony.</span><span class="sxs-lookup"><span data-stu-id="b707a-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="b707a-145">Informacje o śledzeniu można wyświetlić w oknie **danych wyjściowych** .</span><span class="sxs-lookup"><span data-stu-id="b707a-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="b707a-146">Dodaj więcej klas uruchamiania</span><span class="sxs-lookup"><span data-stu-id="b707a-146">Add More Startup Classes</span></span>

<span data-ttu-id="b707a-147">W tej sekcji dodamy kolejną klasę uruchomieniową.</span><span class="sxs-lookup"><span data-stu-id="b707a-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="b707a-148">Do aplikacji można dodać wiele klas uruchomieniowych OWIN.</span><span class="sxs-lookup"><span data-stu-id="b707a-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="b707a-149">Można na przykład utworzyć klasy uruchomieniowe do projektowania, testowania i produkcji.</span><span class="sxs-lookup"><span data-stu-id="b707a-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="b707a-150">Utwórz nową klasę uruchomieniową OWIN i nadaj jej nazwę `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="b707a-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="b707a-151">Zastąp wygenerowany kod następującym:</span><span class="sxs-lookup"><span data-stu-id="b707a-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="b707a-152">Naciśnij klawisz CONTROL F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b707a-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="b707a-153">Atrybut `OwinStartup` określa, że produkcja jest uruchamiana.</span><span class="sxs-lookup"><span data-stu-id="b707a-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="b707a-154">Utwórz kolejną klasę uruchomieniową OWIN i nadaj jej nazwę `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="b707a-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="b707a-155">Zastąp wygenerowany kod następującym:</span><span class="sxs-lookup"><span data-stu-id="b707a-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="b707a-156">Przeciążanie atrybutu `OwinStartup` powyżej określa `TestingConfiguration` jako *przyjazną* nazwę klasy uruchomieniowej.</span><span class="sxs-lookup"><span data-stu-id="b707a-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="b707a-157">Otwórz plik *Web. config* i Dodaj klucz uruchomienia aplikacji Owin, który określa przyjazną nazwę klasy startowej:</span><span class="sxs-lookup"><span data-stu-id="b707a-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="b707a-158">Naciśnij klawisz CONTROL F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b707a-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="b707a-159">Element ustawień aplikacji przyjmuje poprzednią wartość, a Konfiguracja testu jest uruchamiana.</span><span class="sxs-lookup"><span data-stu-id="b707a-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="b707a-160">Usuń *przyjazną* nazwę z atrybutu `OwinStartup` w klasie `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="b707a-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="b707a-161">Zastąp klucz uruchomienia aplikacji OWIN w pliku *Web. config* następującym:</span><span class="sxs-lookup"><span data-stu-id="b707a-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="b707a-162">Przywróć atrybut `OwinStartup` w każdej klasie do domyślnego kodu atrybutu wygenerowanego przez program Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b707a-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="b707a-163">Każdy z kluczy uruchamiania aplikacji OWIN poniżej spowoduje uruchomienie klasy produkcyjnej.</span><span class="sxs-lookup"><span data-stu-id="b707a-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="b707a-164">Ostatni klucz uruchomienia określa metodę konfiguracji uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="b707a-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="b707a-165">Następujący klucz uruchomienia aplikacji OWIN umożliwia zmianę nazwy klasy konfiguracji na `MyConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="b707a-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="b707a-166">Korzystanie z Owinhost. exe</span><span class="sxs-lookup"><span data-stu-id="b707a-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="b707a-167">Zastąp plik Web. config następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="b707a-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="b707a-168">Ostatni klucz usługi WINS, więc w tym przypadku `TestStartup` jest określony.</span><span class="sxs-lookup"><span data-stu-id="b707a-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="b707a-169">Zainstaluj Owinhost z PMC:</span><span class="sxs-lookup"><span data-stu-id="b707a-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="b707a-170">Przejdź do folderu aplikacji (folderu zawierającego plik *Web. config* ) i w wierszu polecenia wpisz:</span><span class="sxs-lookup"><span data-stu-id="b707a-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="b707a-171">Zostanie wyświetlone okno polecenia:</span><span class="sxs-lookup"><span data-stu-id="b707a-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="b707a-172">Uruchom przeglądarkę z adresem URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="b707a-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="b707a-173">OwinHost zostały uznane za wymienione powyżej konwencje uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="b707a-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="b707a-174">W oknie wiersza polecenia naciśnij klawisz ENTER, aby zakończyć OwinHost.</span><span class="sxs-lookup"><span data-stu-id="b707a-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="b707a-175">W klasie `ProductionStartup` Dodaj następujący atrybut OwinStartup, który określa przyjazną nazwę *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="b707a-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="b707a-176">W wierszu polecenia wpisz:</span><span class="sxs-lookup"><span data-stu-id="b707a-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="b707a-177">Załadowana jest Klasa startowa produkcji.</span><span class="sxs-lookup"><span data-stu-id="b707a-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="b707a-178">Nasza aplikacja ma wiele klas startowych, a w tym przykładzie odroczono klasę uruchomieniową do załadowania do czasu wykonania.</span><span class="sxs-lookup"><span data-stu-id="b707a-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="b707a-179">Przetestuj następujące opcje uruchamiania środowiska uruchomieniowego:</span><span class="sxs-lookup"><span data-stu-id="b707a-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]

---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Część 8: ostateczne strony, obsługa wyjątków i wnioski | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 8 dodaje stronę kontaktu, stronę i wyjątek...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586885"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="c74d4-104">Część 8. Końcowe strony, obsługa wyjątków i podsumowanie</span><span class="sxs-lookup"><span data-stu-id="c74d4-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>

<span data-ttu-id="c74d4-105">Jan [Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="c74d4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="c74d4-106">Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="c74d4-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="c74d4-107">W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.</span><span class="sxs-lookup"><span data-stu-id="c74d4-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="c74d4-108">Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="c74d4-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="c74d4-109">Część 8 dodaje stronę kontaktu, stronę i obsługę wyjątków.</span><span class="sxs-lookup"><span data-stu-id="c74d4-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="c74d4-110">Jest to wniosek dotyczący serii.</span><span class="sxs-lookup"><span data-stu-id="c74d4-110">This is the conclusion of the series.</span></span>

## <a id="_Toc260221680"></a><span data-ttu-id="c74d4-111">Strona kontaktu (wysyłanie wiadomości e-mail z ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="c74d4-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="c74d4-112">Utwórz nową stronę o nazwie ContactName. aspx</span><span class="sxs-lookup"><span data-stu-id="c74d4-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="c74d4-113">Korzystając z projektanta, Utwórz następującą postać specjalnej uwagi, aby uwzględnić ToolkitScriptManager i kontrolkę Edytor z AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="c74d4-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxControlToolkit.</span></span> <span data-ttu-id="c74d4-114">.</span><span class="sxs-lookup"><span data-stu-id="c74d4-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="c74d4-115">Kliknij dwukrotnie przycisk "Prześlij", aby wygenerować procedurę obsługi zdarzeń kliknięcia w pliku związanym z kodem i zaimplementować metodę wysyłania informacji kontaktowych jako wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c74d4-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="c74d4-116">Ten kod wymaga, aby plik Web. config zawierał wpis w sekcji konfiguracji, który określa serwer SMTP, który ma być używany do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="c74d4-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="c74d4-117">Informacje o stronie</span><span class="sxs-lookup"><span data-stu-id="c74d4-117">About Page</span></span>

<span data-ttu-id="c74d4-118">Utwórz stronę o nazwie about. aspx i Dodaj dowolną zawartość, którą lubisz.</span><span class="sxs-lookup"><span data-stu-id="c74d4-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="c74d4-119">Globalna procedura obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="c74d4-119">Global Exception Handler</span></span>

<span data-ttu-id="c74d4-120">Wreszcie w całej aplikacji wystąpiły wyjątki i istnieją nieprzewidziane sytuacje, w których zimnie powodowało Nieobsłużone wyjątki w aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c74d4-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="c74d4-121">Nigdy nie chcemy wyświetlać nieobsłużonego wyjątku dla Gości witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c74d4-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="c74d4-122">Oprócz tego Nieobsłużone wyjątki w środowisku użytkownika jaszczurów mogą być również problemem z zabezpieczeniami.</span><span class="sxs-lookup"><span data-stu-id="c74d4-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="c74d4-123">Aby rozwiązać ten problem, zaimplementujmy globalny program obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="c74d4-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="c74d4-124">Aby to zrobić, Otwórz plik Global. asax i zanotuj następujący wstępnie wygenerowany program obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c74d4-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="c74d4-125">Dodaj kod, aby zaimplementować aplikację\_procedurę obsługi błędów w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="c74d4-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="c74d4-126">Następnie Dodaj stronę o nazwie Error. aspx do rozwiązania i Dodaj ten fragment kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="c74d4-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="c74d4-127">Teraz na stronie,\_obsłudze zdarzeń ładowania, Wyodrębnij komunikaty o błędach z obiektu request.</span><span class="sxs-lookup"><span data-stu-id="c74d4-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="c74d4-128">Stwierdzeni</span><span class="sxs-lookup"><span data-stu-id="c74d4-128">Conclusion</span></span>

<span data-ttu-id="c74d4-129">Zaobserwowano, że ASP.NET WebForms ułatwiają tworzenie zaawansowanej witryny sieci Web z dostępem do baz danych, członkostwem, AJAX itp.</span><span class="sxs-lookup"><span data-stu-id="c74d4-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="c74d4-130">szybko.</span><span class="sxs-lookup"><span data-stu-id="c74d4-130">pretty quickly.</span></span>

<span data-ttu-id="c74d4-131">Miejmy nadzieję ten samouczek zawiera narzędzia, które są potrzebne, aby rozpocząć tworzenie własnych aplikacji ASP.NET WebForms!</span><span class="sxs-lookup"><span data-stu-id="c74d4-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c74d4-132">Wstecz</span><span class="sxs-lookup"><span data-stu-id="c74d4-132">Previous</span></span>](tailspin-spyworks-part-7.md)

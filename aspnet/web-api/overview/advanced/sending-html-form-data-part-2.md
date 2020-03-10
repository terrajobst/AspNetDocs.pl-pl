---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Wysyłanie danych formularza HTML w interfejsie Web API ASP.NET: przekazywanie plików i wieloczęściowe MIME-ASP.NET 4. x'
author: MikeWasson
description: W tym samouczku pokazano, jak przekazywać pliki do internetowego interfejsu API. Opisano w nim również, jak przetwarzać wieloczęściowe dane MIME.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557569"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="7908e-104">Wysyłanie danych formularza HTML w interfejsie Web API ASP.NET: przekazywanie plików i wieloczęściowe MIME</span><span class="sxs-lookup"><span data-stu-id="7908e-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="7908e-105">według [Jan Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7908e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="7908e-106">Część 2: przekazywanie plików i wieloczęściowe MIME</span><span class="sxs-lookup"><span data-stu-id="7908e-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="7908e-107">W tym samouczku pokazano, jak przekazywać pliki do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7908e-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="7908e-108">Opisano w nim również, jak przetwarzać wieloczęściowe dane MIME.</span><span class="sxs-lookup"><span data-stu-id="7908e-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="7908e-109">[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="7908e-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>

<span data-ttu-id="7908e-110">Oto przykład formularza HTML służącego do przekazywania pliku:</span><span class="sxs-lookup"><span data-stu-id="7908e-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="7908e-111">Ten formularz zawiera kontrolkę wprowadzanie tekstu i kontrolkę wprowadzania plików.</span><span class="sxs-lookup"><span data-stu-id="7908e-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="7908e-112">Gdy formularz zawiera kontrolkę wprowadzania plików, atrybut **Enctype** powinien zawsze być &quot;częściową/formą&quot;danych, która określa, że formularz będzie wysyłany jako wieloczęściowy komunikat MIME.</span><span class="sxs-lookup"><span data-stu-id="7908e-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="7908e-113">Format wieloczęściowego komunikatu MIME jest najłatwiej zrozumieć, przeglądając przykładowe żądanie:</span><span class="sxs-lookup"><span data-stu-id="7908e-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="7908e-114">Ten komunikat jest podzielony na dwie *części*, po jednym dla każdej kontrolki formularza.</span><span class="sxs-lookup"><span data-stu-id="7908e-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="7908e-115">Granice części są wskazywane przez wiersze rozpoczynające się od kresek.</span><span class="sxs-lookup"><span data-stu-id="7908e-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="7908e-116">Granica części obejmuje losowy składnik (&quot;41184676334&quot;), aby upewnić się, że ciąg graniczny nie pojawia się przypadkowo wewnątrz części komunikatu.</span><span class="sxs-lookup"><span data-stu-id="7908e-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>

<span data-ttu-id="7908e-117">Każda część komunikatu zawiera jeden lub więcej nagłówków, po których następuje zawartość części.</span><span class="sxs-lookup"><span data-stu-id="7908e-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="7908e-118">Nagłówek Content-Dyspozycja zawiera nazwę formantu.</span><span class="sxs-lookup"><span data-stu-id="7908e-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="7908e-119">W przypadku plików zawiera również nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="7908e-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="7908e-120">Nagłówek Content-Type opisuje dane w części.</span><span class="sxs-lookup"><span data-stu-id="7908e-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="7908e-121">W przypadku pominięcia tego nagłówka wartość domyślna to Text/zwykły.</span><span class="sxs-lookup"><span data-stu-id="7908e-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="7908e-122">W poprzednim przykładzie użytkownik przesłał plik o nazwie GrandCanyon. jpg z typem zawartości Image/JPEG; a wartość danych wejściowych tekstu była &quot;urlopu lato&quot;.</span><span class="sxs-lookup"><span data-stu-id="7908e-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="7908e-123">Przekazywanie plików</span><span class="sxs-lookup"><span data-stu-id="7908e-123">File Upload</span></span>

<span data-ttu-id="7908e-124">Teraz przyjrzyjmy się kontrolerowi interfejsu API sieci Web, który odczytuje pliki z wieloczęściowego komunikatu MIME.</span><span class="sxs-lookup"><span data-stu-id="7908e-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="7908e-125">Kontroler odczyta pliki asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7908e-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="7908e-126">Interfejs API sieci Web obsługuje asynchroniczne akcje przy użyciu [modelu programowania opartego na zadaniach](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="7908e-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="7908e-127">Najpierw poniżej przedstawiono kod, jeśli jest przeznaczony .NET Framework 4,5, który obsługuje słowa kluczowe **Async** i **await** .</span><span class="sxs-lookup"><span data-stu-id="7908e-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="7908e-128">Zwróć uwagę, że akcja kontrolera nie przyjmuje żadnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="7908e-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="7908e-129">Dzieje się tak, ponieważ przetwarzamy treść żądania wewnątrz akcji bez wywoływania programu formatującego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="7908e-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="7908e-130">Metoda **IsMultipartContent** sprawdza, czy żądanie zawiera wieloczęściowy komunikat MIME.</span><span class="sxs-lookup"><span data-stu-id="7908e-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="7908e-131">Jeśli nie, kontroler zwraca kod stanu HTTP 415 (nieobsługiwany typ nośnika).</span><span class="sxs-lookup"><span data-stu-id="7908e-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="7908e-132">Klasa **MultipartFormDataStreamProvider** jest obiektem pomocnika, który przydziela strumienie plików dla przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="7908e-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="7908e-133">Aby odczytać wieloczęściowy komunikat MIME, wywołaj metodę **ReadAsMultipartAsync** .</span><span class="sxs-lookup"><span data-stu-id="7908e-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="7908e-134">Ta metoda wyodrębnia wszystkie części komunikatów i zapisuje je w strumieniach dostarczonych przez **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="7908e-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="7908e-135">Gdy metoda zostanie ukończona, można uzyskać informacje o plikach z właściwości **Filedata** , która jest kolekcją obiektów **MultipartFileData** .</span><span class="sxs-lookup"><span data-stu-id="7908e-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="7908e-136">**MultipartFileData. filename** to nazwa lokalnego pliku na serwerze, na którym zapisano plik.</span><span class="sxs-lookup"><span data-stu-id="7908e-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="7908e-137">**MultipartFileData. Headers** zawiera nagłówek części (*nie* nagłówek żądania).</span><span class="sxs-lookup"><span data-stu-id="7908e-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="7908e-138">Możesz użyć tej metody, aby uzyskać dostęp do zawartości\_dyspozycji i nagłówków typu zawartości.</span><span class="sxs-lookup"><span data-stu-id="7908e-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="7908e-139">Jak sugeruje nazwa, **ReadAsMultipartAsync** jest metodą asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="7908e-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="7908e-140">Aby wykonać pracę po zakończeniu metody, użyj [zadania kontynuacji](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4,0) lub słowa kluczowego **await** (.NET 4,5).</span><span class="sxs-lookup"><span data-stu-id="7908e-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="7908e-141">Oto wersja .NET Framework 4,0 poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="7908e-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="7908e-142">Odczytywanie danych kontrolki formularza</span><span class="sxs-lookup"><span data-stu-id="7908e-142">Reading Form Control Data</span></span>

<span data-ttu-id="7908e-143">Formularz HTML, który wykazał wcześniej, miał kontrolkę wprowadzanie tekstu.</span><span class="sxs-lookup"><span data-stu-id="7908e-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="7908e-144">Możesz uzyskać wartość kontrolki z właściwości **formData** **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="7908e-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="7908e-145">**FormData** to **NameValueCollection** , który zawiera pary nazwa/wartość dla kontrolek formularza.</span><span class="sxs-lookup"><span data-stu-id="7908e-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="7908e-146">Kolekcja może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="7908e-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="7908e-147">Rozważmy następujący formularz:</span><span class="sxs-lookup"><span data-stu-id="7908e-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="7908e-148">Treść żądania może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="7908e-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="7908e-149">W takim przypadku kolekcja **formData** będzie zawierać następujące pary klucz/wartość:</span><span class="sxs-lookup"><span data-stu-id="7908e-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="7908e-150">podróż: rundy</span><span class="sxs-lookup"><span data-stu-id="7908e-150">trip: round-trip</span></span>
- <span data-ttu-id="7908e-151">Opcje: niezatrzymane</span><span class="sxs-lookup"><span data-stu-id="7908e-151">options: nonstop</span></span>
- <span data-ttu-id="7908e-152">Opcje: daty</span><span class="sxs-lookup"><span data-stu-id="7908e-152">options: dates</span></span>
- <span data-ttu-id="7908e-153">stanowisko: okno</span><span class="sxs-lookup"><span data-stu-id="7908e-153">seat: window</span></span>

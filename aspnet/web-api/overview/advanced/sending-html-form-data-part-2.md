---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: Przekazywanie pliku i wieloczęściowej wiadomości MIME — ASP.NET 4.x'
author: MikeWasson
description: W tym samouczku pokazano, jak przekazać pliki do internetowego interfejsu API. Opisuje ona również, jak do przetwarzania danych wieloczęściowej wiadomości MIME.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 70e150a32f208cf75086f959d484d86e8501c6bd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59419928"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="28e53-104">Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: przekazywanie plików i wieloczęściowa wiadomość MIME</span><span class="sxs-lookup"><span data-stu-id="28e53-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="28e53-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="28e53-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="28e53-106">Część 2. przekazywanie plików i wieloczęściowa wiadomość MIME</span><span class="sxs-lookup"><span data-stu-id="28e53-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="28e53-107">W tym samouczku pokazano, jak przekazać pliki do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="28e53-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="28e53-108">Opisuje ona również, jak do przetwarzania danych wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="28e53-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="28e53-109">[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="28e53-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="28e53-110">Oto przykład z formularza HTML służącego do przekazywania pliku:</span><span class="sxs-lookup"><span data-stu-id="28e53-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="28e53-111">Ten formularz zawiera kontrolkę wprowadzania tekstu formanty i pliku wejściowego.</span><span class="sxs-lookup"><span data-stu-id="28e53-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="28e53-112">Gdy formularza zawiera kontrolki wprowadzania w pliku **typ kodowania** atrybut powinien mieć zawsze &quot;multipart/formularza data&quot;, która określa, że formularz zostanie wysłana jako wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="28e53-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="28e53-113">Format wieloczęściowej wiadomości MIME jest łatwiej zrozumieć, analizując przykładowe żądanie:</span><span class="sxs-lookup"><span data-stu-id="28e53-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="28e53-114">Ten komunikat jest podzielona na dwie *części*, jeden dla każdego formantu formularza.</span><span class="sxs-lookup"><span data-stu-id="28e53-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="28e53-115">Część granice są oznaczone wiersze rozpoczynające się z kreskami.</span><span class="sxs-lookup"><span data-stu-id="28e53-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="28e53-116">Granic część zawiera składnik losowy (&quot;41184676334&quot;) aby upewnić się, że ciąg granic nie przypadkowo pojawia się w części komunikatu.</span><span class="sxs-lookup"><span data-stu-id="28e53-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="28e53-117">Każda część zawiera jeden lub więcej nagłówków, następuje zawartości części.</span><span class="sxs-lookup"><span data-stu-id="28e53-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="28e53-118">Nagłówek Content-Disposition zawiera nazwę formantu.</span><span class="sxs-lookup"><span data-stu-id="28e53-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="28e53-119">Dla plików także zawiera nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="28e53-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="28e53-120">Nagłówek Content-Type opisano je w części.</span><span class="sxs-lookup"><span data-stu-id="28e53-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="28e53-121">W przypadku pominięcia tego pliku nagłówkowego, wartość domyślna to text/plain.</span><span class="sxs-lookup"><span data-stu-id="28e53-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="28e53-122">W poprzednim przykładzie użytkownik przekazał plik o nazwie GrandCanyon.jpg, typ zawartości image/jpeg; wartość wprowadzania tekstu &quot;wakacje&quot;.</span><span class="sxs-lookup"><span data-stu-id="28e53-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="28e53-123">Przekazywanie pliku</span><span class="sxs-lookup"><span data-stu-id="28e53-123">File Upload</span></span>

<span data-ttu-id="28e53-124">Teraz Przyjrzyjmy się kontrolera interfejsu API sieci Web, która odczytuje pliki z wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="28e53-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="28e53-125">Kontroler odczyta pliki asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="28e53-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="28e53-126">Internetowy interfejs API obsługuje akcje asynchroniczne przy użyciu [modelu programowania opartego na zadaniach](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="28e53-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="28e53-127">Po pierwsze, Oto kod, jeśli są przeznaczone dla .NET Framework 4.5, która obsługuje **async** i **await** słów kluczowych.</span><span class="sxs-lookup"><span data-stu-id="28e53-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="28e53-128">Należy zauważyć, że akcji kontrolera nie przyjmuje żadnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="28e53-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="28e53-129">Wynika to z treści żądania w akcji, możemy przetworzyć bez wywoływania elementu formatującego typu nośnika.</span><span class="sxs-lookup"><span data-stu-id="28e53-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="28e53-130">**IsMultipartContent** metoda sprawdza, czy żądanie zawiera wieloczęściowej wiadomości MIME.</span><span class="sxs-lookup"><span data-stu-id="28e53-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="28e53-131">W przeciwnym razie ten kontroler zwraca kod stanu HTTP 415 (nieobsługiwany typ nośnika).</span><span class="sxs-lookup"><span data-stu-id="28e53-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="28e53-132">**MultipartFormDataStreamProvider** klasa jest obiektem pomocnika, który przydziela strumieni plików przekazywanych plików.</span><span class="sxs-lookup"><span data-stu-id="28e53-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="28e53-133">Aby odczytać wiadomości wieloczęściowej MIME, należy wywołać **ReadAsMultipartAsync** metody.</span><span class="sxs-lookup"><span data-stu-id="28e53-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="28e53-134">Ta metoda wyodrębnia wszystkie części komunikatu i zapisuje je do strumieni, dostarczone przez **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="28e53-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="28e53-135">Po zakończeniu działania metody, można uzyskać informacji o plikach z **FileData** właściwość, która jest kolekcją z **MultipartFileData** obiektów.</span><span class="sxs-lookup"><span data-stu-id="28e53-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="28e53-136">**MultipartFileData.FileName** jest nazwą pliku lokalnego na serwerze, w którym plik został zapisany.</span><span class="sxs-lookup"><span data-stu-id="28e53-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="28e53-137">**MultipartFileData.Headers** zawiera nagłówek części (*nie* nagłówek żądania).</span><span class="sxs-lookup"><span data-stu-id="28e53-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="28e53-138">Umożliwia to dostęp do zawartości\_nagłówki dyspozycji i Content-Type.</span><span class="sxs-lookup"><span data-stu-id="28e53-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="28e53-139">Jak sugeruje nazwa, **ReadAsMultipartAsync** jest metodą asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="28e53-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="28e53-140">Do wykonywania pracy po zakończeniu metody, należy użyć [zadanie kontynuacji](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) lub **await** — słowo kluczowe (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="28e53-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="28e53-141">Oto wersja programu .NET Framework 4.0 poprzedni kod:</span><span class="sxs-lookup"><span data-stu-id="28e53-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="28e53-142">Odczytywania danych kontrolki formularza</span><span class="sxs-lookup"><span data-stu-id="28e53-142">Reading Form Control Data</span></span>

<span data-ttu-id="28e53-143">Formularza HTML, wcześniej pokazujący miał kontrolki wprowadzania tekstu.</span><span class="sxs-lookup"><span data-stu-id="28e53-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="28e53-144">Można uzyskać wartości kontrolki z **pobranie** właściwość **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="28e53-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="28e53-145">**Pobranie** jest **elementu NameValueCollection** zawierający pary nazwa/wartość dla kontrolek formularza.</span><span class="sxs-lookup"><span data-stu-id="28e53-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="28e53-146">Kolekcja może zawierać zduplikowane klucze.</span><span class="sxs-lookup"><span data-stu-id="28e53-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="28e53-147">Należy wziąć pod uwagę tego formularza:</span><span class="sxs-lookup"><span data-stu-id="28e53-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="28e53-148">Treść żądania może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="28e53-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="28e53-149">W takim przypadku **pobranie** kolekcja będzie zawierać następujące pary klucz/wartość:</span><span class="sxs-lookup"><span data-stu-id="28e53-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="28e53-150">podróży: dwustronnej konwersji</span><span class="sxs-lookup"><span data-stu-id="28e53-150">trip: round-trip</span></span>
- <span data-ttu-id="28e53-151">opcje: nonstop</span><span class="sxs-lookup"><span data-stu-id="28e53-151">options: nonstop</span></span>
- <span data-ttu-id="28e53-152">opcje: dat</span><span class="sxs-lookup"><span data-stu-id="28e53-152">options: dates</span></span>
- <span data-ttu-id="28e53-153">stanowisko: okna</span><span class="sxs-lookup"><span data-stu-id="28e53-153">seat: window</span></span>

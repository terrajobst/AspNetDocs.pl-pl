---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57076553"
---
# <a name="custom-model-binding-demo"></a><span data-ttu-id="7c5e4-101">Pokaz powiązanie modelu niestandardowego</span><span class="sxs-lookup"><span data-stu-id="7c5e4-101">Custom Model Binding Demo</span></span>

<span data-ttu-id="7c5e4-102">Test `ByteArrayModelBinder` , uruchamiając aplikację i publikując ciągu zakodowanego algorytmem base64, aby `ImageController` punktu końcowego (`/api/image/`).</span><span class="sxs-lookup"><span data-stu-id="7c5e4-102">Test `ByteArrayModelBinder` by running the app and POSTing a base64-encoded string to the `ImageController` endpoint (`/api/image/`).</span></span> <span data-ttu-id="7c5e4-103">Określ właściwości pliku i nazwa pliku w treści żądania jako dane formularza (przy użyciu [Postman](https://www.getpostman.com/) lub podobnego narzędzia).</span><span class="sxs-lookup"><span data-stu-id="7c5e4-103">Specify the file and filename properties in the request body as form-data (using [Postman](https://www.getpostman.com/) or a similar tool).</span></span> <span data-ttu-id="7c5e4-104">Możesz użyć [to przykładowy ciąg](Base64String.txt).</span><span class="sxs-lookup"><span data-stu-id="7c5e4-104">You can use [this sample string](Base64String.txt).</span></span> <span data-ttu-id="7c5e4-105">Wynik zostanie zapisany w *wwwroot/obrazy/przekazywania* folder przy użyciu nazwy pliku określonej.</span><span class="sxs-lookup"><span data-stu-id="7c5e4-105">The result is saved in the *wwwroot/images/upload* folder with the filename specified.</span></span>

<span data-ttu-id="7c5e4-106">Aby przetestować przykład niestandardowego powiązania, wypróbuj następujące punkty końcowe:</span><span class="sxs-lookup"><span data-stu-id="7c5e4-106">To test the custom binding example, try the following endpoints:</span></span>

* <span data-ttu-id="7c5e4-107">/API/authors/1</span><span class="sxs-lookup"><span data-stu-id="7c5e4-107">/api/authors/1</span></span>
* <span data-ttu-id="7c5e4-108">/API/authors/2 (nie znaleziono)</span><span class="sxs-lookup"><span data-stu-id="7c5e4-108">/api/authors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="7c5e4-109">/API/boundauthors/1</span><span class="sxs-lookup"><span data-stu-id="7c5e4-109">/api/boundauthors/1</span></span>
* <span data-ttu-id="7c5e4-110">/API/boundauthors/2 (nie znaleziono)</span><span class="sxs-lookup"><span data-stu-id="7c5e4-110">/api/boundauthors/2 (NOT FOUND)</span></span>
* <span data-ttu-id="7c5e4-111">/api/boundauthors/get/1</span><span class="sxs-lookup"><span data-stu-id="7c5e4-111">/api/boundauthors/get/1</span></span>
* <span data-ttu-id="7c5e4-112">(Brak zawartości) /API/boundauthors/Get/2 &ndash; tej akcji nie sprawdza w przypadku wartości null i zwraca *404 Nie znaleziono*.</span><span class="sxs-lookup"><span data-stu-id="7c5e4-112">/api/boundauthors/get/2 (NO CONTENT) &ndash; This action doesn't check for null and returns a *404 Not Found*.</span></span>

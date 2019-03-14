---
ms.openlocfilehash: e5565381f44480e2531925717ee7815da92d1ff5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067625"
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="da016-101">Aktualizuj wygenerowanych stron</span><span class="sxs-lookup"><span data-stu-id="da016-101">Update the generated pages</span></span>

<span data-ttu-id="da016-102">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="da016-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="da016-103">Mamy dobry początek aplikacji filmu, ale prezentacji nie jest najlepszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="da016-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="da016-104">Nie chcemy wyświetlić czas (12:00:00 AM na ilustracji poniżej) i **ReleaseDate** powinien być **Data wydania** (dwa słowa).</span><span class="sxs-lookup"><span data-stu-id="da016-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="da016-106">Aktualizacja wygenerowanego kodu</span><span class="sxs-lookup"><span data-stu-id="da016-106">Update the generated code</span></span>

<span data-ttu-id="da016-107">Otwórz *Models/Movie.cs* pliku i Dodaj wyróżnione wiersze pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="da016-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]

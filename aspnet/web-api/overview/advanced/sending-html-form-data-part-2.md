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
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Wysyłanie danych formularza HTML w interfejsie Web API ASP.NET: przekazywanie plików i wieloczęściowe MIME

według [Jan Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Część 2: przekazywanie plików i wieloczęściowe MIME

W tym samouczku pokazano, jak przekazywać pliki do internetowego interfejsu API. Opisano w nim również, jak przetwarzać wieloczęściowe dane MIME.

> [!NOTE]
> [Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).

Oto przykład formularza HTML służącego do przekazywania pliku:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Ten formularz zawiera kontrolkę wprowadzanie tekstu i kontrolkę wprowadzania plików. Gdy formularz zawiera kontrolkę wprowadzania plików, atrybut **Enctype** powinien zawsze być &quot;częściową/formą&quot;danych, która określa, że formularz będzie wysyłany jako wieloczęściowy komunikat MIME.

Format wieloczęściowego komunikatu MIME jest najłatwiej zrozumieć, przeglądając przykładowe żądanie:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Ten komunikat jest podzielony na dwie *części*, po jednym dla każdej kontrolki formularza. Granice części są wskazywane przez wiersze rozpoczynające się od kresek.

> [!NOTE]
> Granica części obejmuje losowy składnik (&quot;41184676334&quot;), aby upewnić się, że ciąg graniczny nie pojawia się przypadkowo wewnątrz części komunikatu.

Każda część komunikatu zawiera jeden lub więcej nagłówków, po których następuje zawartość części.

- Nagłówek Content-Dyspozycja zawiera nazwę formantu. W przypadku plików zawiera również nazwę pliku.
- Nagłówek Content-Type opisuje dane w części. W przypadku pominięcia tego nagłówka wartość domyślna to Text/zwykły.

W poprzednim przykładzie użytkownik przesłał plik o nazwie GrandCanyon. jpg z typem zawartości Image/JPEG; a wartość danych wejściowych tekstu była &quot;urlopu lato&quot;.

## <a name="file-upload"></a>Przekazywanie plików

Teraz przyjrzyjmy się kontrolerowi interfejsu API sieci Web, który odczytuje pliki z wieloczęściowego komunikatu MIME. Kontroler odczyta pliki asynchronicznie. Interfejs API sieci Web obsługuje asynchroniczne akcje przy użyciu [modelu programowania opartego na zadaniach](https://msdn.microsoft.com/library/dd460693.aspx). Najpierw poniżej przedstawiono kod, jeśli jest przeznaczony .NET Framework 4,5, który obsługuje słowa kluczowe **Async** i **await** .

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Zwróć uwagę, że akcja kontrolera nie przyjmuje żadnych parametrów. Dzieje się tak, ponieważ przetwarzamy treść żądania wewnątrz akcji bez wywoływania programu formatującego typu nośnika.

Metoda **IsMultipartContent** sprawdza, czy żądanie zawiera wieloczęściowy komunikat MIME. Jeśli nie, kontroler zwraca kod stanu HTTP 415 (nieobsługiwany typ nośnika).

Klasa **MultipartFormDataStreamProvider** jest obiektem pomocnika, który przydziela strumienie plików dla przekazanych plików. Aby odczytać wieloczęściowy komunikat MIME, wywołaj metodę **ReadAsMultipartAsync** . Ta metoda wyodrębnia wszystkie części komunikatów i zapisuje je w strumieniach dostarczonych przez **MultipartFormDataStreamProvider**.

Gdy metoda zostanie ukończona, można uzyskać informacje o plikach z właściwości **Filedata** , która jest kolekcją obiektów **MultipartFileData** .

- **MultipartFileData. filename** to nazwa lokalnego pliku na serwerze, na którym zapisano plik.
- **MultipartFileData. Headers** zawiera nagłówek części (*nie* nagłówek żądania). Możesz użyć tej metody, aby uzyskać dostęp do zawartości\_dyspozycji i nagłówków typu zawartości.

Jak sugeruje nazwa, **ReadAsMultipartAsync** jest metodą asynchroniczną. Aby wykonać pracę po zakończeniu metody, użyj [zadania kontynuacji](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4,0) lub słowa kluczowego **await** (.NET 4,5).

Oto wersja .NET Framework 4,0 poprzedniego kodu:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Odczytywanie danych kontrolki formularza

Formularz HTML, który wykazał wcześniej, miał kontrolkę wprowadzanie tekstu.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Możesz uzyskać wartość kontrolki z właściwości **formData** **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** to **NameValueCollection** , który zawiera pary nazwa/wartość dla kontrolek formularza. Kolekcja może zawierać zduplikowane klucze. Rozważmy następujący formularz:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Treść żądania może wyglądać następująco:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

W takim przypadku kolekcja **formData** będzie zawierać następujące pary klucz/wartość:

- podróż: rundy
- Opcje: niezatrzymane
- Opcje: daty
- stanowisko: okno

---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Walka botów (VB) | Microsoft Docs
author: wenz
description: Automatyczne botów tynków Weblogs i innych witryn internetowych z spamem, przesyłanie formularzy komentarzy bez interakcji z użytkownikiem. Kontrolka NoBot w ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627394"
---
# <a name="fighting-bots-vb"></a>Zwalczanie botów (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Automatyczne botów tynków Weblogs i innych witryn internetowych z spamem, przesyłanie formularzy komentarzy bez interakcji z użytkownikiem. Formant NoBot w narzędziu ASP.NET AJAX Control Toolkit może pomóc w walce z tymi botówami.

## <a name="overview"></a>Omówienie

Automatyczne botów tynków Weblogs i innych witryn internetowych z spamem, przesyłanie formularzy komentarzy bez interakcji z użytkownikiem. Formant NoBot w narzędziu ASP.NET AJAX Control Toolkit może pomóc w walce z tymi botówami.

## <a name="steps"></a>Kroki

Typowym podejściem do pokonania botów jest korzystanie z CAPTCHAs całkowicie zautomatyzowanego, publicznego testu Turing do informowania komputerów i ludzi. Test Turing był pierwotnie testem, w którym ktoś musiał zdecydować, czy partner komunikacji jest człowiekiem, czy komputerem. W sieci Web CAPTCHA zwykle składa się z obrazu z niezakłóconymi literami. Pomysłem jest, że tylko człowiek może odczytać litery z obrazu, a algorytmy OCR nie powiodą się.

Istnieje kilka zalet i wad tego podejścia, ale dyskusje na ten temat wykraczają poza zakres tego samouczka. Istnieje jednak kontrola w narzędziu ASP.NET AJAX Control Toolkit, która zapewnia podobne podejście: `NoBot`. Jest to prostsze w porównaniu z CAPTCHA, ale jest bardzo łatwe w użyciu i jest bardzo proste w odniesieniu do witryn sieci Web, takich jak Blogi, które są uważane za sukces w przypadku, gdy większość prób spamu zostanie poddana kontroli `NoBot`.

`NoBot` przechwytuje ogłaszanie zwrotne bieżącego formularza sieci Web ASP.NET w przypadku spełnienia co najmniej jednego z następujących warunków:

- Przeglądarka nie może usunąć układanki JavaScript (na przykład w przypadku dezaktywowania kodu JavaScript)
- Użytkownik przesłał formularz do programu Fast
- Adres IP klienta przesłał formularz zbyt często w określonym czasie.

Aby można było sprawdzić te warunki, formant `NoBot` wymaga tych atrybutów (wszystkie opcjonalne):

- `ResponseMinimumDelaySeconds` minimalną liczbę sekund między ogłaszaniem zwrotnym
- `CutoffWindowSeconds` długość przedziału czasu, w którym zwroty od jednego adresu IP są mierzone
- `CutoffMaximumInstances` maksymalną liczbę sekund na przedział czasu

Poniższe znaczniki zależą od tego, że co najmniej dwa sekundy upływa między ogłaszaniem zwrotnym i że w interwale 30-sekundowym występuje tylko pięć zwrotów.

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Następnie w zwykły sposób Pamiętaj o uwzględnieniu `ScriptManager` na stronie, tak aby Biblioteka ASP.NET AJAX została załadowana i można było użyć zestawu narzędzi do sterowania:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Ponieważ większość kontroli `NoBot` odbywa się po stronie serwera, należy sprawdzić wynik tych walidacji. Można to zrobić, wywołując metodę `IsValid()` `NoBot`. Ma jeden argument (jako `out` parametr/`ByRef`, który jest typu `NoBotState`. Reprezentacja ciągu zawiera przyczynę niepowodzenia sprawdzania i `Valid` w przeciwnym razie. Poniższy kod generuje komunikat zgodnie z wynikami `NoBot`:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Na koniec potrzebna jest forma przesyłania i elementu etykiety, który będzie wyprowadzać komunikat, a wszystko jest gotowe!

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Po uruchomieniu tego skryptu i zdezaktywowaniu kodu JavaScript lub przesłaniu formularza w ciągu pierwszych dwóch sekund lub przesłaniu formularza siedem razy w ciągu 30 sekund zostanie wyświetlony komunikat o błędzie. Należy jednak użyć tej kontrolki, ponieważ tylko od 90-95% użytkowników ma aktywowany kod JavaScript, w związku z czym 5-10% użytkowników zakończy się niepowodzeniem `NoBot`.

[![ten komunikat o błędzie mógł zostać spowodowany przez bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Ten komunikat o błędzie może być spowodowany przez bot ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Wstecz](fighting-bots-cs.md)

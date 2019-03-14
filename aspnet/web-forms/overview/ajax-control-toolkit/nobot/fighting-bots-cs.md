---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Zwalczanie Botów (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Zautomatyzowanych robotów Sztukateria dzienników w sieci Web i innych witryn sieci Web ze spamem, przesyłać formularze komentarz bez żadnej interakcji użytkownika. Kontrolka NoBot w Con AJAX programu ASP.NET...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 52ed34e7640cd125a3b4c3b50ab760a7c1d713f1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57069239"
---
<a name="fighting-bots-c"></a>Zwalczanie botów (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Zautomatyzowanych robotów Sztukateria dzienników w sieci Web i innych witryn sieci Web ze spamem, przesyłać formularze komentarz bez żadnej interakcji użytkownika. Kontrolka NoBot w ASP.NET AJAX Control Toolkit może pomóc walczą o tych botów.


## <a name="overview"></a>Omówienie

Zautomatyzowanych robotów Sztukateria dzienników w sieci Web i innych witryn sieci Web ze spamem, przesyłać formularze komentarz bez żadnej interakcji użytkownika. Kontrolka NoBot w ASP.NET AJAX Control Toolkit może pomóc walczą o tych botów.

## <a name="steps"></a>Kroki

Typową metodą pokonania Boty jest Użyj CAPTCHAs całkowicie zautomatyzowanego publicznych Turing testu, aby stwierdzić, komputerów i ludzie od siebie. Turing test była pierwotnie testu tam, gdzie ktoś konieczne podjęcie decyzji partnera komunikacji jest maszynę lub ludzi. W sieci web CAPTCHA składa się zwykle obrazu niektóre litery zniekształcony na nim. Chodzi o to, że tylko człowiek może odczytać litery na obrazie, natomiast algorytmy optyczne rozpoznawanie znaków zakończy się niepowodzeniem.

Istnieje kilka zalet i wad tego podejścia, ale dyskusję na temat to wykracza poza zakres tego samouczka. Istnieje jednak kontrolki w ASP.NET AJAX Control Toolkit, który zapewnia podejście podobne: `NoBot`. Jest to łatwiejsze do pokonania niż z mechanizmu CAPTCHA, ale jest bardzo łatwa w użyciu i taryf bardzo dobrze nadaje się do witryny sieci Web, takich jak blogi, gdzie uważa się sukcesem, jeśli większość spamu prób jest bezcelowe, który `NoBot` kontroli można zrobić.

`NoBot` Przechwytuje zwrotu bieżącego formularza sieci web platformy ASP.NET, jeśli co najmniej jeden z tych warunków jest spełniony:

- Przeglądarka nie może rozwiązać układanki JavaScript (na przykład gdy JavaScript jest dezaktywowany)
- Formularz, aby szybko zostały przesłane przez użytkownika
- Adres IP klienta przesłanego formularza zbyt często w pewien czas.

Aby sprawdzić, czy te warunki `NoBot` formant wymaga tych atrybutów (wszystkie z nich, które są opcjonalne):

- `ResponseMinimumDelaySeconds` Minimalny czas w sekundach między ogłoszeniami wstecznymi
- `CutoffWindowSeconds` długość interwału czasu, w którym ogłaszania zwrotnego w jeden adres IP jest miar
- `CutoffMaximumInstances` Maksymalna ilość sekundy na przedział czasu

Następujące wymagania znaczników tego co najmniej dwie sekundy upłynąć między ogłoszeniami wstecznymi oraz czy tylko pięć ogłaszania zwrotnego lub szybciej w interwale 30-sekundowym:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Następnie w zwykły sposób upewnij się uwzględnić `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Ponieważ większość kontroli `NoBot` robi wystąpić po stronie serwera, musisz sprawdzić wynik tych operacji sprawdzania poprawności. Można to zrobić, wywołując `NoBot`firmy `IsValid()` metody. Ma jeden argument (jako `out` parametr /`ByRef` parametru) jest typu `NoBotState`. Ciąg reprezentujący zawiera przyczynę niepowodzenia sprawdzania i `Valid` inaczej. Poniższy kod wyświetla komunikat, zgodnie z opisem w `NoBot`w wyniku:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Na koniec należy formularza w celu przesyłania i elementu label, aby wyprowadzić komunikatu i wszystko jest gotowe!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Uruchom ten skrypt, a dezaktywować JavaScript lub Prześlij formularz w ciągu pierwszych dwóch sekund lub Prześlij formularz siedem razy w ciągu 30 sekund, zostanie wyświetlony komunikat o błędzie. Jednak należy uważnie użyć tej kontrolki, ponieważ tylko około 90-95% użytkownicy mają JavaScript aktywowany, w związku z tym 5 – 10% użytkowników zakończy się niepowodzeniem `NoBot`użytkownika testu.


[![Ten komunikat o błędzie może być spowodowane przez robota](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Ten komunikat o błędzie może być spowodowane przez robota ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](fighting-bots-vb.md)

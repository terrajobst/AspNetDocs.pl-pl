---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Używanie używanie dynamicpopulate z kontrolką użytkownika i JavaScript (VB) | Microsoft Docs
author: wenz
description: Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: ee5923ad6d8b101f689a0564aef8b1e0e00a7639
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599142"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Używanie kontrolki DynamicPopulate z kontrolką użytkownika i kodem JavaScript (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> Kontrolka używanie dynamicpopulate w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony. Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta. Należy jednak zwrócić szczególną uwagę, gdy rozszerzenie znajduje się w kontrolce użytkownika.

## <a name="overview"></a>Omówienie

Formant `DynamicPopulate` w zestawie narzędzi ASP.NET AJAX Control wywołuje usługę sieci Web (lub metodę strony) i wypełnia wartość wyniki do kontrolki docelowej na stronie bez odświeżania strony. Możliwe jest również wyzwolenie populacji przy użyciu niestandardowego kodu JavaScript po stronie klienta. Należy jednak zwrócić szczególną uwagę, gdy rozszerzenie znajduje się w kontrolce użytkownika.

## <a name="steps"></a>Kroki

Najpierw potrzebna jest usługa sieci Web ASP.NET, która implementuje metodę, która ma zostać wywołana przez formant `DynamicPopulateExtender`. Usługa sieci Web implementuje metodę `getDate()`, która oczekuje jednego argumentu typu ciąg o nazwie `contextKey`, ponieważ kontrolka `DynamicPopulate` wysyła jedną część informacji o kontekście do każdego wywołania usługi sieci Web. Oto kod (pliki `DynamicPopulate.vb.asmx`), który pobiera bieżącą datę w jednym z trzech formatów:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

W następnym kroku Utwórz nową kontrolkę użytkownika (plik`.ascx`), która jest oznaczona przez następującą deklarację w pierwszym wierszu:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

Element &lt;`label`&gt; będzie używany do wyświetlania danych pochodzących z serwera.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Ponadto w pliku kontrolnym użytkownika będziemy używać trzech przycisków radiowych, z których każdy reprezentuje jeden z trzech możliwych formatów daty obsługiwanych przez usługę sieci Web. Gdy użytkownik kliknie jeden z przycisków radiowych, przeglądarka wykona kod JavaScript, który wygląda następująco:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Ten kod uzyskuje dostęp do `DynamicPopulateExtender` (nie martw się o nietypowego identyfikatora, zostanie on uwzględniony w dalszej części) i wyzwala dynamiczną populację z danymi. W kontekście bieżącego przycisku radiowego `this.value` odnosi się do jego wartości `format1`, `format2` lub `format3` dokładnie tak, jak oczekuje Metoda sieci Web.

Jedyną kwestią w kontrolce użytkownika jest jeszcze formant `DynamicPopulateExtender`, który łączy przyciski radiowe z usługą sieci Web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Ponownie można zauważyć, że jest to dziwny identyfikator używany w kontrolce: `mcd1$myDate`, a nie `myDate`. Wcześniej kod JavaScript używany `mcd1_dpe1`, aby uzyskać dostęp do `DynamicPopulateExtender` zamiast `dpe1`. Ta strategia nazewnictwa jest specjalnym wymaganiem w przypadku używania `DynamicPopulateExtender` w ramach kontrolki użytkownika. Ponadto należy osadzić kontrolkę użytkownika w określony sposób, aby wszystko działało. Utwórz nową stronę ASP.NET i zarejestruj Prefiks tagu dla właśnie zaimplementowanej kontrolki użytkownika:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Następnie na nowej stronie Uwzględnij formant `ScriptManager` AJAX ASP.NET:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Na koniec Dodaj kontrolkę użytkownika do strony. Musisz tylko ustawić jego atrybut `ID` (i `runat="server"`, oczywiście), ale trzeba również ustawić go na określoną nazwę: `mcd1` ponieważ jest to prefiks używany w kontrolce użytkownika do uzyskiwania dostępu do niego przy użyciu języka JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

I to wszystko! Strona działa zgodnie z oczekiwaniami: użytkownik klika jeden z przycisków radiowych, formant w zestawie narzędzi wywołuje usługę sieci Web i wyświetla bieżącą datę w żądanym formacie.

[![przyciski radiowe znajdują się w kontrolce użytkownika](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

Przyciski radiowe znajdują się w kontrolce użytkownika ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Ubiegł](dynamically-populating-a-control-using-javascript-code-vb.md)

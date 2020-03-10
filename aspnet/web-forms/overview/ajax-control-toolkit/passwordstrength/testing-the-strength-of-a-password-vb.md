---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testowanie siły hasła (VB) | Microsoft Docs
author: wenz
description: Hasła są wymagane niemal wszędzie, dzięki czemu użytkownicy z opóźnieniem mogą wybrać proste hasła, które są łatwe do przerwania. Kontrolka PasswordStrength w ASP. N...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: b614e1788eeafc175dd792ec6d3e4619f9ea2b7a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627303"
---
# <a name="testing-the-strength-of-a-password-vb"></a>Testowanie siły hasła (VB)

Autor [Christian Wenz](https://github.com/wenz)

[Pobierz kod](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Hasła są wymagane niemal wszędzie, dzięki czemu użytkownicy z opóźnieniem mogą wybrać proste hasła, które są łatwe do przerwania. Formant PasswordStrength w narzędziu ASP.NET AJAX Control Toolkit może sprawdzić, jak dobre jest hasło.

## <a name="overview"></a>Omówienie

Hasła są wymagane niemal wszędzie, dzięki czemu użytkownicy z opóźnieniem mogą wybrać proste hasła, które są łatwe do przerwania. Formant `PasswordStrength` w narzędziu ASP.NET AJAX Control Toolkit może sprawdzić, jak dobre jest hasło.

## <a name="steps"></a>Kroki

Formant `PasswordStrength` rozszerza pole tekstowe i sprawdza, czy hasło jest wystarczające. Oferuje wiele opcji za pośrednictwem atrybutów; Oto kilka z nich:

- `MinimumNumericCharacters` minimalna liczba znaków numerycznych wymaganych w haśle
- `MinimumSymbolCharacters` minimalna liczba znaków symbolicznych (nie liter i cyfr) wymaganych w haśle
- `PreferredPasswordLength` minimalnej długości hasła
- `RequiresUpperAndLowerCaseCharacters`, czy hasło musi zawierać wielkie i małe litery

`StrengthIndicatorType` zawiera informacje na temat sposobu prezentowania siły hasła jako tekstu (`"Text"`wartości) lub jako rodzaju paska postępu (wartości `"BarIndicator"`). W atrybucie `DisplayPosition` można skonfigurować miejsce wyświetlania informacji. Oto kompletny przykład, w tym kontrolka ASP.NET AJAX `ScriptManager`, kontrolka `PasswordStrength` i kurs pola tekstowego, w którym użytkownik może wprowadzić hasło. Na potrzeby demonstracji pole to jest zwykłym polem tekstowym, a nie polem hasła, dzięki czemu możesz zobaczyć, co wpisujesz.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Uruchom stronę i wpisz: tylko po wprowadzeniu małych liter, wielkich liter, cyfr i symboli, hasło jest uznawane za niemożliwe do oddzielenia.

[![teraz hasło jest (całkiem) dobre](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

Teraz hasło jest (całkiem) dobre ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Wstecz](testing-the-strength-of-a-password-cs.md)

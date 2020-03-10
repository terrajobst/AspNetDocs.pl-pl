---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Część 7: członkostwo i autoryzacja | Microsoft Docs'
author: jongalloway
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 7 obejmuje członkostwo i autoryzację.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539201"
---
# <a name="part-7-membership-and-authorization"></a>Część 7. Członkostwo i autoryzacja

przez [Jan Galloway](https://github.com/jongalloway)

> Sklep MVC Music jest aplikacją samouczka, która wprowadza i objaśnia krok po kroku, jak używać ASP.NET MVC i Visual Studio do programowania w sieci Web.  
>   
> Sklep MVC Music jest lekkim przykładowym wdrożeniem magazynu, który sprzedaje Albumy muzyczne w trybie online i implementuje podstawowe funkcje administracyjne, logowania użytkownika i koszyka.  
>   
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji do sklepu ASP.NET MVC Music. Część 7 obejmuje członkostwo i autoryzację.

Nasz kontroler Menedżera sklepu jest obecnie dostępny dla wszystkich osób odwiedzających naszą witrynę. Zmieńmy ten sposób, aby ograniczyć uprawnienia do administratorów lokacji.

## <a name="adding-the-accountcontroller-and-views"></a>Dodawanie elementu AccountController i widoków

Jedną z różnic między pełnym szablonem aplikacji sieci Web ASP.NET MVC 3 a szablonem pustej aplikacji sieci Web ASP.NET MVC 3 jest to, że pusty szablon nie zawiera kontrolera konta. Dodamy kontroler konta przez skopiowanie kilku plików z nowej aplikacji ASP.NET MVC utworzonej na podstawie szablonu aplikacji sieci Web ASP.NET MVC 3.

Utwórz nową aplikację ASP.NET MVC przy użyciu pełnego szablonu aplikacji sieci Web ASP.NET MVC 3 i skopiuj następujące pliki do tych samych katalogów w naszym projekcie:

1. Kopiuj AccountController.cs w katalogu controllers
2. Kopiuj AccountModels w katalogu models
3. Utwórz katalog konta w katalogu widoki i skopiuj wszystkie cztery widoki w

Zmień przestrzeń nazw dla kontrolera i klasy modelu, aby zaczynać się od MvcMusicStore. Klasa elementu AccountController powinna używać przestrzeni nazw MvcMusicStore. controllers, a Klasa AccountModels powinna używać przestrzeni nazw MvcMusicStore. models.

*Uwaga: te pliki są również dostępne na stronie pobierania MvcMusicStore-Assets. zip, z której zostały skopiowane pliki projektu witryny na początku samouczka. Pliki członkostwa znajdują się w katalogu kodu.*

Zaktualizowane rozwiązanie powinno wyglądać następująco:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Dodawanie użytkownika administracyjnego przy użyciu lokacji konfiguracji ASP.NET

Przed wymaganiem autoryzacji w naszej witrynie sieci Web należy utworzyć użytkownika z dostępem. Najprostszym sposobem utworzenia użytkownika jest użycie wbudowanej witryny sieci Web ASP.NET Configuration.

Uruchom witrynę konfiguracji ASP.NET, klikając ikonę w Eksplorator rozwiązań.

![](mvc-music-store-part-7/_static/image2.png)

Spowoduje to uruchomienie witryny sieci Web konfiguracji. Kliknij kartę Zabezpieczenia na ekranie głównym, a następnie kliknij link "Włącz role" w centrum ekranu.

![](mvc-music-store-part-7/_static/image3.png)

Kliknij link "Utwórz role lub zarządzaj nimi".

![](mvc-music-store-part-7/_static/image4.png)

Wprowadź wartość "Administrator" jako nazwę roli i naciśnij przycisk Dodaj rolę.

![](mvc-music-store-part-7/_static/image5.png)

Kliknij przycisk Wstecz, a następnie kliknij link Utwórz użytkownika po lewej stronie.

![](mvc-music-store-part-7/_static/image6.png)

Wypełnij pola informacje o użytkowniku po lewej stronie, używając następujących informacji:

| **Pole** | **Wartość** |
| --- | --- |
| **Nazwa użytkownika** | Administrator |
| **Hasło** | password123! |
| **Potwierdź hasło** | password123! |
| **E-mail** | (dowolny adres e-mail będzie działał) |
| **Pytanie zabezpieczeń** | (dowolne z nich) |
| **Odpowiedź zabezpieczeń** | (dowolne z nich) |

*Uwaga: możesz użyć dowolnego hasła, które chcesz. Domyślne ustawienia zabezpieczeń hasła wymagają hasła o długości 7 znaków i zawierają jeden znak inny niż alfanumeryczny.*

Wybierz rolę administratora dla tego użytkownika, a następnie kliknij przycisk Utwórz użytkownika.

![](mvc-music-store-part-7/_static/image7.png)

W tym momencie powinien zostać wyświetlony komunikat informujący o tym, że użytkownik został utworzony pomyślnie.

![](mvc-music-store-part-7/_static/image8.png)

Teraz możesz zamknąć okno przeglądarki.

## <a name="role-based-authorization"></a>Autoryzacja oparta na rolach

Teraz możemy ograniczyć dostęp do StoreManagerController przy użyciu atrybutu [autoryzuje], określając, że użytkownik musi należeć do roli administratora, aby uzyskać dostęp do dowolnej akcji kontrolera w klasie.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Uwaga: atrybut [autoryzuje] może być umieszczony w określonych metodach akcji, a także na poziomie klasy kontrolera.*

Teraz przechodzenie do/StoreManager powoduje wyświetlenie okna dialogowego logowania:

![](mvc-music-store-part-7/_static/image9.png)

Po zalogowaniu się przy użyciu nowego konta administratora można przejść do ekranu edycji albumu jako poprzednio.

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-6.md)
> [dalej](mvc-music-store-part-8.md)

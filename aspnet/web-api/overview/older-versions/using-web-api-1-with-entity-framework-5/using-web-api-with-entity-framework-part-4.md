---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Część 4: Dodawanie widoku administratora | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600015"
---
# <a name="part-4-adding-an-admin-view"></a>Część 4: Dodawanie widoku administratora

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Dodawanie widoku administratora

Teraz zmienimy się po stronie klienta i dodasz stronę, która będzie mogła korzystać z danych z kontrolera administratora. Na stronie użytkownicy będą mogli tworzyć, edytować lub usuwać produkty, wysyłając żądania AJAX do kontrolera.

W Eksplorator rozwiązań rozwiń folder kontrolery i Otwórz plik o nazwie HomeController.cs. Ten plik zawiera kontroler MVC. Dodaj metodę o nazwie `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

Metoda **HttpRouteUrl** tworzy identyfikator URI dla internetowego interfejsu API i zapisuje go w zbiorze widoku dla późniejszej.

Następnie umieść kursor tekstu w metodzie `Admin` akcji, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie **Dodaj widok**. Spowoduje to wyświetlenie okna dialogowego **Dodawanie widoku** .

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

W oknie dialogowym **Dodawanie widoku** Nazwij widok "Administrator". Zaznacz pole wyboru z etykietą **Utwórz widok o jednoznacznie określonym typie**. W obszarze **Klasa modelu**wybierz pozycję "produkt (ProductStore. models)". Pozostaw wszystkie inne opcje jako wartości domyślne.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Kliknięcie przycisku **Dodaj** dodaje plik o nazwie admin. cshtml w widokach/na stronie głównej. Otwórz ten plik i Dodaj następujący kod HTML. Ten kod HTML definiuje strukturę strony, ale żadne funkcje nie są jeszcze przewodne.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Tworzenie linku do strony administratora

W Eksplorator rozwiązań rozwiń folder widoki, a następnie rozwiń folder udostępniony. Otwórz plik o nazwie \_Layout. cshtml. Znajdź element **ul** z identyfikatorem "menu" i link akcji dla widoku administratora:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> W przykładowym projekcie zostały wykonane pewne inne zmiany, takie jak zastąpienie ciągu "logo" w tym miejscu. Nie wpływają one na działanie aplikacji. Możesz pobrać projekt i porównać pliki.

Uruchom aplikację i kliknij link "Administrator", który pojawia się w górnej części strony głównej. Strona administrator powinna wyglądać następująco:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Teraz Strona nie wykonuje żadnych czynności. W następnej sekcji użyjemy narzędzia separowania. js do utworzenia dynamicznego interfejsu użytkownika.

## <a name="add-authorization"></a>Dodaj autoryzację

Strona administratora jest obecnie dostępna dla wszystkich osób odwiedzających witrynę. Zmieńmy ten sposób, aby ograniczyć uprawnienia do administratorów.

Zacznij od dodania roli "Administrator" i administratora. W Eksplorator rozwiązań rozwiń folder filtry i Otwórz plik o nazwie InitializeSimpleMembershipAttribute.cs. Znajdź konstruktora `SimpleMembershipInitializer`. Po wywołaniu metody **WebSecurity. InitializeDatabaseConnection**Dodaj następujący kod:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Jest to szybka i zanieczyszczona metoda dodawania roli "Administrator" i tworzenia użytkownika dla roli.

W Eksplorator rozwiązań rozwiń folder kontrolery i Otwórz plik HomeController.cs. Dodaj atrybut **Autoryzuj** do metody `Admin`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Otwórz plik AdminController.cs i Dodaj atrybut **Autoryzuj** do całej klasy `AdminController`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> Zarówno MVC, jak i interfejs API sieci Web definiują atrybuty **autoryzacji** w różnych przestrzeniach nazw. MVC używa **System. Web. MVC. AuthorizeAttribute**, a interfejs API sieci Web używa **System. Web. http. AuthorizeAttribute**.

Teraz tylko Administratorzy mogą wyświetlać stronę administratora. Ponadto w przypadku wysłania żądania HTTP do kontrolera administratora żądanie musi zawierać plik cookie uwierzytelniania. W przeciwnym razie serwer wysyła odpowiedź HTTP 401 (bez autoryzacji). Ten element można zobaczyć w programu Fiddler, wysyłając żądanie GET do `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Poprzednie](using-web-api-with-entity-framework-part-3.md)
> [dalej](using-web-api-with-entity-framework-part-5.md)

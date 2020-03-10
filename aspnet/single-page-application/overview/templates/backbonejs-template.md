---
uid: single-page-application/overview/templates/backbonejs-template
title: Szablon szkieletu | Microsoft Docs
author: madskristensen
description: Szablon SPA. js
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 7297db7d5b35a53b40f9d9162960e529a167bd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558227"
---
# <a name="backbone-template"></a>Szablon Backbone

Autor [produktywność Madsa Kristensena](https://github.com/madskristensen)

> Szablon SPA sieci szkieletowej został zapisany przez Kazi Manzur Rashid
> 
> [Pobieranie szablonu SPA. js](https://go.microsoft.com/fwlink/?LinkId=293631)

Szablon SPA. js jest zaprojektowana tak, aby można było szybko tworzyć interaktywne aplikacje sieci Web po stronie klienta za pomocą [szkieletu. js.](http://backbonejs.org/)

Szablon zawiera początkowy szkielet tworzenia aplikacji sieci szkieletowej w języku ASP.NET MVC. Poza tym pole zapewnia podstawowe funkcje logowania użytkowników, w tym rejestrowanie użytkowników, logowanie, resetowanie haseł i potwierdzenie użytkownika przy użyciu podstawowych szablonów wiadomości e-mail.

Wymagania:

- [Aktualizacja ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Tworzenie projektu szablonu szkieletowego

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Szablon jest spakowany jako plik rozszerzenia programu Visual Studio (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**. Nazwij projekt, a następnie kliknij przycisk **OK**.

![](backbonejs-template/_static/image1.png)

W kreatorze **nowego projektu** wybierz projekt szkielet. js spa.

![](backbonejs-template/_static/image2.png)

Naciśnij klawisze CTRL + F5, aby skompilować i uruchomić aplikację bez debugowania, lub naciśnij klawisz F5, aby uruchomić polecenie z debugowaniem.

![](backbonejs-template/_static/image3.png)

Kliknięcie pozycji "Moje konto" spowoduje wyświetlenie strony logowania:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Przewodnik: kod klienta

Zacznijmy od po stronie klienta. Skrypty aplikacji klienta znajdują się w folderze ~/scripts/Application. Aplikacja jest zapisywana w języku [TypeScript](http://www.typescriptlang.org/) (pliki TS), które są kompilowane do języka JavaScript (pliki. js).

**Aplikacja**

`Application` jest zdefiniowany w aplikacji. TS. Ten obiekt inicjuje aplikację i działa jako główna przestrzeń nazw. Przechowuje informacje o konfiguracji i stanie, które są współużytkowane przez aplikację, na przykład o tym, czy użytkownik jest zalogowany.

Metoda `application.start` tworzy widoki modalne i dołącza obsługę zdarzeń dla zdarzeń na poziomie aplikacji, takich jak logowanie użytkownika. Następnie tworzy domyślny router i sprawdza, czy jest określony adres URL po stronie klienta. W przeciwnym razie przekierowuje do domyślnego adresu URL (#!/).

**Zdarzenia**

Zdarzenia są zawsze ważne podczas tworzenia luźno sprzężonych składników. Aplikacje często wykonują wiele operacji w odpowiedzi na akcję użytkownika. Szkielet udostępnia wbudowane zdarzenia ze składnikami, takimi jak model, kolekcja i widok. Zamiast tworzyć zależności między tymi składnikami, szablon używa modelu "pub/Sub": obiekt `events`, zdefiniowany w Events. TS, działa jako centrum zdarzeń do publikowania i subskrybowania zdarzeń aplikacji. Obiekt `events` jest klasą pojedynczą. Poniższy kod przedstawia sposób subskrybowania zdarzenia, a następnie wyzwalania zdarzenia:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Ruter**

W sieci szkieletowej. js Router udostępnia metody routingu stron po stronie klienta i łączenia ich z akcjami i zdarzeniami. Szablon definiuje pojedynczy router w routerze. TS. Router tworzy widoki activable i utrzymuje stan podczas przełączania widoków. (Widoki Activable są opisane w następnej sekcji). Początkowo projekt ma dwa fikcyjne widoki, Dom i informacje. Ma także widok NotFound, który jest wyświetlany, jeśli trasa nie jest znana.

**Widoki**

Widoki są zdefiniowane w ~/scripts/Application/views. Istnieją dwa rodzaje widoków, widoki activable i modalne widoki okien dialogowych. Activable widoki są wywoływane przez router. Gdy widok activable jest wyświetlany, wszystkie inne widoki activable stają się nieaktywne. Aby utworzyć widok activable, rozwiń widok przy użyciu obiektu `Activable`:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Rozszerzenie przy użyciu `Activable` dodaje dwie nowe metody do widoku, `activate` i `deactivate`. Router wywołuje te metody, aby uaktywnić i dezaktywować widok.

Widoki modalne są implementowane jako modalne okna dialogowe [uruchamiania usługi Twitter](https://twitter.github.com/bootstrap/) . Widoki `Membership` i `Profile` są widokami modalnymi. Widoki modelu mogą być wywoływane przez dowolne zdarzenia aplikacji. Na przykład w widoku `Navigation` kliknięcie linku "Moje konto" pokazuje widok `Membership` lub widok `Profile` w zależności od tego, czy użytkownik jest zalogowany. `Navigation` dołącza procedury obsługi zdarzeń do wszystkich elementów podrzędnych, które mają atrybut `data-command`. Oto znacznik HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Oto kod w obszarze nawigacji. TS, aby podłączyć zdarzenia:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Przykładów**

Modele są zdefiniowane w ~/scripts/Application/models. Wszystkie modele mają trzy podstawowe rzeczy: atrybuty domyślne, reguły walidacji i punkt końcowy po stronie serwera. Oto typowy przykład:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Dodatki plug-in**

Folder ~/scripts/Application/lib zawiera kilka przydatnych wtyczek jQuery. Plik w formacie TS definiuje wtyczkę do pracy z danymi formularza. Często trzeba serializować lub deserializować dane formularzy i wyświetlić wszelkie błędy walidacji modelu. W formularzu. wtyczka TS ma takie metody jak `serializeFields`, `deserializeFields`i `showFieldErrors`. Poniższy przykład pokazuje, jak serializować formularz do modelu.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Wtyczka flashbar. TS udostępnia różne rodzaje komunikatów z opiniami dla użytkownika. Metody są `$.showSuccessbar`, `$.showErrorbar` i `$.showInfobar`. W tle używa alertów ładowania początkowego usługi Twitter do wyświetlania animowanych komunikatów dobrze.

Wtyczka Potwierdź. TS zastępuje okno dialogowe potwierdzenia przeglądarki, chociaż interfejs API jest nieco inny:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Przewodnik: kod serwera

Teraz przyjrzyjmy się po stronie serwera.

**Kontrolery**

W aplikacji jednostronicowej serwer odgrywa tylko niewielką rolę w interfejsie użytkownika. Zazwyczaj serwer renderuje stronę początkową, a następnie wysyła i odbiera dane JSON.

Szablon ma dwa kontrolery MVC: `HomeController` renderuje stronę początkową, a `SupportsController` służy do potwierdzania nowych kont użytkowników i resetowania haseł. Wszystkie inne kontrolery w szablonie to ASP.NET Web API controllers, które wysyłają i odbierają dane JSON. Domyślnie kontrolery używają nowej klasy `WebSecurity` do wykonywania zadań związanych z użytkownikiem. Jednak mają także opcjonalne konstruktory umożliwiające przekazywanie delegatów dla tych zadań. Ułatwia to testowanie i umożliwia zastępowanie `WebSecurity` innymi przy użyciu kontenera IoC. Oto przykład:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Widoki

Widoki są przeznaczone do modularnia: Każda sekcja strony ma swój własny widok dedykowany. W przypadku aplikacji jednostronicowej często są uwzględniane widoki, które nie mają odpowiedniego kontrolera. Możesz dołączyć widok, wywołując `@Html.Partial('myView')`, ale żmudnym. Aby to ułatwić, szablon definiuje metodę pomocnika, `IncludeClientViews`, która renderuje wszystkie widoki w określonym folderze:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Jeśli nazwa folderu nie jest określona, domyślną nazwą folderu jest "ClientViews". Jeśli widok klienta używa również widoków częściowych, nazwij widok częściowy przy użyciu znaku podkreślenia (na przykład `_SignUp`). Metoda `IncludeClientViews` wyklucza wszystkie widoki, których nazwy zaczynają się od znaku podkreślenia. Aby w widoku klienta uwzględnić widok częściowy, wywołaj `Html.ClientView('SignUp')` zamiast `Html.Partial('_SignUp')`.

**Wysyłanie wiadomości E-mail**

Aby wysłać wiadomość e-mail, szablon używa [poczty pocztowej](http://aboutcode.net/postal). Jednak kod pocztowy jest abstrakcyjny od pozostałej części kodu z interfejsem `IMailer`, dzięki czemu można łatwo zastąpić go innym implementacją. Szablony wiadomości e-mail znajdują się w folderze widoki/wiadomości E-mail. Adres e-mail nadawcy jest określony w pliku Web. config w kluczu `sender.email` w sekcji **AppSettings** . Ponadto w przypadku `debug="true"` w pliku Web. config aplikacja nie wymaga potwierdzenia adresu e-mail użytkownika, aby przyspieszyć programowanie.

## <a name="github"></a>GitHub

Można również znaleźć szablon "sieci szkieletowej. js SPA" w serwisie [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).

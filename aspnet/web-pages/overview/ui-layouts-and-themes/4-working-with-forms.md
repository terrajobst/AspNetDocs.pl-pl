---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Praca z formularzami HTML w witrynach ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Formularz to sekcja dokumentu HTML, w której umieszczasz kontrolki wprowadzania danych przez użytkownika, takie jak pola tekstowe, pola wyboru, przyciski radiowe i listy rozwijane. Używasz formularzy wh...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: c7d4802063c8610a246afe67bd15eea429f7304a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639707"
---
# <a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Praca z formularzami HTML w witrynach ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano, jak przetwarzać formularz HTML (z polami tekstowymi i przyciskami) podczas pracy w witrynie internetowej ASP.NET Web Pages (Razor).
> 
> **Dowiesz się:** 
> 
> - Jak utworzyć formularz HTML.
> - Jak odczytać dane wprowadzane przez użytkownika z formularza.
> - Sprawdzanie poprawności danych wejściowych użytkownika.
> - Jak przywrócić wartości formularza po przesłaniu strony.
> 
> Są to koncepcje programowania ASP.NET wprowadzone w artykule:
> 
> - Obiekt `Request`.
> - Walidacja danych wejściowych.
> - Kodowanie HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

## <a name="creating-a-simple-html-form"></a>Tworzenie prostego formularza HTML

1. Utwórz nową witrynę sieci Web.
2. W folderze głównym Utwórz stronę sieci Web o nazwie *form. cshtml* i wprowadź następujące znaczniki:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Uruchom stronę w przeglądarce. (W programie WebMatrix w obszarze roboczym **pliki** kliknij plik prawym przyciskiem myszy, a następnie wybierz polecenie **Uruchom w przeglądarce**). Zostanie wyświetlony prosty formularz z trzema polami wejściowymi i przyciskiem **przesyłania** .

    ![Zrzut ekranu formularza z trzema polami tekstowymi.](4-working-with-forms/_static/image1.png)

    W tym momencie klikniesz przycisk **Prześlij** , nic się nie dzieje. Aby formularz był użyteczny, musisz dodać kod, który zostanie uruchomiony na serwerze.

## <a name="reading-user-input-from-the-form"></a>Odczytywanie danych wprowadzonych przez użytkownika z formularza

Aby przetworzyć formularz, należy dodać kod, który odczytuje przesłane wartości pól i wykonuje coś z nimi. Ta procedura pokazuje, jak odczytać pola i wyświetlić dane wejściowe użytkownika na stronie. (W aplikacji produkcyjnej zwykle bardziej interesujące są inne funkcje wprowadzane przez użytkownika. Należy to zrobić w artykule dotyczącym pracy z bazami danych.

1. W górnej części pliku *form. cshtml* wprowadź następujący kod:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Gdy użytkownik najpierw żąda strony, zostanie wyświetlony tylko pusty formularz. Użytkownik (który zostanie) wypełnia formularz, a następnie klika polecenie **Prześlij**. Spowoduje to przesłanie (wpisów) danych wejściowych użytkownika do serwera. Domyślnie żądanie przechodzi do tej samej strony (tj *. form. cshtml*).

    Po przesłaniu strony w tym czasie wprowadzone wartości są wyświetlane tuż nad formularzem:

    ![Zrzut ekranu pokazujący wprowadzone wartości wyświetlane na stronie.](4-working-with-forms/_static/image2.png)

    Przyjrzyj się kodowi strony. Najpierw użyj metody `IsPost`, aby określić, czy strona jest publikowana &#8212; , czy użytkownik kliknął przycisk **Prześlij** . Jeśli jest to wpis, `IsPost` zwraca wartość true. Jest to standardowy sposób w ASP.NET stronach sieci Web, aby określić, czy pracujesz z początkowym żądaniem (żądanie GET) czy zwrotem (żądanie POST). (Aby uzyskać więcej informacji na temat funkcji GET i POST, zobacz pasek boczny "HTTP GET i POST i Właściwość ispost" w artykule [wprowadzenie do programowania ASP.NET stron sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost)).

    Następnie uzyskasz wartości, które użytkownik wypełnił z obiektu `Request.Form` i umieścić je w zmiennych w celu późniejszego użycia. Obiekt `Request.Form` zawiera wszystkie wartości, które zostały przesłane ze stroną, z których każdy jest identyfikowany przez klucz. Klucz jest odpowiednikiem `name` atrybutu pola formularza, który ma zostać odczytany. Na przykład, aby odczytać pole `companyname` (pole tekstowe), należy użyć `Request.Form["companyname"]`.

    Wartości formularza są przechowywane w obiekcie `Request.Form` jako ciągi. W związku z tym, gdy trzeba będzie korzystać z wartości jako liczby lub innego typu, należy przekonwertować ją z ciągu na ten typ. W przykładzie metoda `AsInt` `Request.Form` służy do konwersji wartości pola Employees (zawierające liczbę pracowników) na liczbę całkowitą.
2. Uruchom stronę w przeglądarce, wypełnij pola formularza, a następnie kliknij przycisk **Prześlij**. Na stronie zostaną wyświetlone wprowadzone wartości.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Kodowanie HTML na potrzeby wyglądu i zabezpieczeń
> 
> Język HTML ma specjalne zastosowania do znaków takich jak `<`, `>`i `&`. Jeśli te znaki specjalne pojawiają się, gdy nie są oczekiwane, mogą ruin wygląd i funkcjonalność strony sieci Web. Na przykład przeglądarka interpretuje znak `<` (chyba że następuje spacja) jako początek elementu HTML, takich jak `<b>` lub `<input ...>`. Jeśli przeglądarka nie rozpoznaje elementu, po prostu odrzuca ciąg, który rozpoczyna się od `<`, dopóki nie osiągnie elementu, który ponownie rozpoznaje. Oczywiście może to spowodować, że na stronie zostanie wyrenderowana brzmienia.
> 
> Kodowanie HTML zastępuje te znaki zarezerwowane kodem, który przeglądarki interpretuje jako poprawny symbol. Na przykład znak `<` jest zastępowany `&lt;`, a znak `>` jest zastępowany `&gt;`. Przeglądarka renderuje te ciągi zamienne jako znaki, które mają być wyświetlane.
> 
> Dobrym pomysłem jest używanie kodowania HTML w dowolnym momencie wyświetlania ciągów (danych wejściowych) uzyskanych od użytkownika. Jeśli tego nie zrobisz, użytkownik może spróbować uzyskać złośliwy skrypt na stronie sieci Web lub wykonać inną czynność, która narusza zabezpieczenia witryny lub że nie jest to planowane. (Jest to szczególnie ważne w przypadku wprowadzenia danych przez użytkownika, zapisania go w zwrócono komunikatu, a następnie wyświetlenie go później &#8212; na przykład jako komentarz do blogu, przeglądanie przez użytkownika lub coś innego).
> 
> Aby uniknąć tych problemów, ASP.NET strony sieci Web automatycznie kodu HTML — koduje zawartość tekstową, która jest wyprowadzana z kodu. Na przykład podczas wyświetlania zawartości zmiennej lub wyrażenia przy użyciu kodu, takiego jak `@MyVar`, strony sieci Web ASP.NET automatycznie kodują dane wyjściowe.

## <a name="validating-user-input"></a>Sprawdzanie poprawności danych wejściowych użytkownika

Użytkownicy wyczynią błędy. Poproś ich o wypełnienie pola i zapomnienie lub poproszenie o wprowadzenie liczby pracowników, a zamiast nich wpisz nazwę. Aby upewnić się, że formularz został prawidłowo wypełniony przed jego przetworzeniem, sprawdź poprawność danych wejściowych użytkownika.

Ta procedura pokazuje, jak sprawdzić wszystkie trzy pola formularza, aby upewnić się, że użytkownik nie pozostawił ich puste. Sprawdź również, czy wartość Liczba pracowników jest liczbą. Jeśli wystąpią błędy, zostanie wyświetlony komunikat o błędzie informujący użytkownika, jakie wartości nie przeszły walidacji.

1. W pliku *form. cshtml* Zastąp pierwszy blok kodu następującym kodem: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Aby sprawdzić poprawność danych wejściowych użytkownika, użyj pomocnika `Validation`. Wymagane pola można zarejestrować, wywołując `Validation.RequireField`. Możesz zarejestrować inne typy walidacji, wywołując `Validation.Add` i określając pole do zweryfikowania oraz typ walidacji do wykonania.

    Po uruchomieniu strony ASP.NET wykonuje wszystkie walidacje. Wyniki można sprawdzić, wywołując `Validation.IsValid`, co zwraca wartość PRAWDA, jeśli wszystko zostało zakończone powodzeniem, a wartość FAŁSZ, Jeśli dowolne pole nie przeszło walidacji. Zazwyczaj należy wywołać `Validation.IsValid` przed wykonaniem dowolnego przetwarzania w danych wejściowych użytkownika.
2. Zaktualizuj element `<body>`, dodając trzy wywołania metody `Html.ValidationMessage`, takie jak:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Aby wyświetlić komunikaty o błędach walidacji, można wywołać kod HTML.`ValidationMessage` i przekaż mu nazwę pola, dla którego chcesz uzyskać wiadomość.
3. Uruchom stronę. Pozostaw puste pola i kliknij przycisk **Prześlij**. Zobaczysz komunikaty o błędach.

    ![Zrzut ekranu pokazujący komunikaty o błędach wyświetlane, gdy dane wejściowe użytkownika nie przechodzą walidacji.](4-working-with-forms/_static/image3.jpg)
4. Dodaj ciąg (na przykład "ABC") do pola **Liczba pracowników** , a następnie kliknij przycisk **Prześlij** ponownie. Tym razem pojawia się błąd informujący o tym, że ciąg nie ma w prawidłowym formacie, czyli w postaci liczby całkowitej.

    ![Zrzut ekranu pokazujący komunikaty o błędach wyświetlane, gdy użytkownicy wprowadzają ciąg dla pola Employees.](4-working-with-forms/_static/image4.jpg)

ASP.NET strony sieci Web udostępniają więcej opcji sprawdzania poprawności danych wejściowych użytkownika, w tym możliwość automatycznego przeprowadzania walidacji przy użyciu skryptu klienta, dzięki czemu użytkownicy otrzymują natychmiastową opinię w przeglądarce. Więcej informacji znajduje się w sekcji [dodatkowe zasoby](#Additional_Resources) .

## <a name="restoring-form-values-after-postbacks"></a>Przywracanie wartości formularza po ogłaszaniu zwrotnym

Po przetestowaniu strony w poprzedniej sekcji można zauważyć, że jeśli wystąpił błąd sprawdzania poprawności, wszystko, co zostało wprowadzone (nie tylko nieprawidłowe dane), zostało usunięte, a użytkownik musiał ponownie wprowadzić wartości dla wszystkich pól. Ilustruje to ważny punkt: w przypadku przesłania strony, przetwórz ją, a następnie ponownie Renderuj stronę, Strona zostanie ponownie utworzona od podstaw. Jak pokazano, oznacza to, że wszystkie wartości, które znajdowały się na stronie podczas przesyłania, zostaną utracone.

Można to jednak łatwo naprawić. Masz dostęp do przekazanych wartości (w obiekcie `Request.Form`, dzięki czemu można wypełniać te wartości z powrotem do pól formularza, gdy strona jest renderowana.

1. W pliku *form. cshtml* Zastąp atrybuty `value` elementów `<input>` przy użyciu atrybutu `value`. 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    Atrybut `value` elementów `<input>` został ustawiony na dynamiczne Odczytywanie wartości pola z obiektu `Request.Form`. Po pierwszym zażądaniu strony wartości w obiekcie `Request.Form` są puste. Jest to odpowiednie rozwiązanie, ponieważ w ten sposób formularz jest pusty.
2. Uruchom stronę w przeglądarce, wypełnij pola formularza lub pozostaw je puste, a następnie kliknij przycisk **Prześlij**. Zostanie wyświetlona strona, która wyświetla przesłane wartości.

    ![Formularze-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

- [1 001 sposobów uzyskiwania danych wejściowych od użytkowników sieci Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Korzystanie z formularzy i przetwarzanie danych wejściowych użytkownika](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Weryfikacja danych wejściowych użytkownika w witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Korzystanie z funkcji Autouzupełnianie w formularzach HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)

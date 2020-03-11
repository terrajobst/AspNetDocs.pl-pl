---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: Jak mogę użyć kontrolki ComboBox? (C#) | Microsoft Docs
author: microsoft
description: Pole kombi jest ASP.NETą kontrolką AJAX, która łączy elastyczność pola tekstowego z listą opcji, z których mogą wybierać użytkownicy.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 1c5fc61300441303b39e348d3eee83b6ee6847b4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554454"
---
# <a name="how-do-i-use-the-combobox-control-c"></a>Jak mogę użyć kontrolki ComboBox? (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> Pole kombi jest ASP.NETą kontrolką AJAX, która łączy elastyczność pola tekstowego z listą opcji, z których mogą wybierać użytkownicy.

Celem tego samouczka jest wyjaśnienie kontrolki ComboBox zestawu narzędzi AJAX Control Toolkit. Pole kombi działa jak kombinacja między standardową kontrolką ASP.NET DropDownList i kontrolką TextBox. Można wybrać jedną z istniejących list elementów lub wprowadzić nowy element.

Pole kombi jest podobne do rozszerzenia kontrolki autouzupełniania, ale kontrolki są używane w różnych scenariuszach. Rozszerzenie Autouzupełnianie wysyła zapytanie do usługi sieci Web, aby uzyskać pasujące wpisy. Kontrolka ComboBox, w przeciwieństwie, zostanie zainicjowana z zestawem elementów. Przy użyciu rozszerzenia Autouzupełnianie ma sens, gdy pracujesz z dużym zestawem danych (miliony części samochodu), podczas gdy przy użyciu kontrolki ComboBox ma sens, że pracujesz z niewielkim zestawem danych (dziesiątki części samochodu).

## <a name="selecting-from-a-static-list-of-items"></a>Wybieranie z statycznej listy elementów

Niech s zaczyna od prostej próbki przy użyciu kontrolki ComboBox. Załóżmy, że chcesz wyświetlić statyczną listę elementów na liście rozwijanej. Należy jednak pozostawić otwarte możliwości, aby lista nie została ukończona. Chcesz zezwolić użytkownikowi na wprowadzanie niestandardowej wartości na listę.

Utworzymy nową stronę formularzy sieci Web ASP.NET i używasz kontrolki ComboBox na stronie. Dodaj nową stronę ASP.NET do projektu i Przełącz się do widok Projekt.

Jeśli chcesz użyć kontrolki ComboBox na stronie, musisz dodać kontrolkę ScriptManager do strony. Przeciągnij formant ScriptManager z karty rozszerzenia AJAX na powierzchnię projektanta. W górnej części strony należy dodać kontrolkę ScriptManager. Możesz dodać je bezpośrednio poniżej otwierającego&gt; &lt;formularza po stronie serwera.

Następnie przeciągnij kontrolkę ComboBox na stronę. Kontrolkę ComboBox można znaleźć w przyborniku przy użyciu innych kontrolek zestawu narzędzi AJAX Control i rozszerzeń formantów (zobacz rysunek 1).

[![prosty formularz do tworzenia karty biznesowej](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Ilustracja 01**: wybranie kontrolki ComboBox z przybornika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image2.png))

Używamy kontrolki ComboBox do wyświetlania statycznej listy opcji. Użytkownik może wybrać określony poziom spiciness dla swojej żywności z listy trzech opcji: łagodny, średni i gorąca (patrz rysunek 2).

[![wybór ze statycznej listy elementów](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Ilustracja 02**. Zaznaczanie statycznej listy elementów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image4.png))

Istnieją dwa sposoby dodawania tych opcji do kontrolki ComboBox. Najpierw należy wybrać opcję Edytuj opcje zadania po umieszczeniu wskaźnika myszy nad kontrolką w widok Projekt i otworzyć edytora elementów (patrz rysunek 3).

[![edycji elementów ComboBox](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Ilustracja 03**: edytowanie elementów ComboBox ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image6.png))

Druga opcja polega na dodaniu listy elementów między tagiem otwierającym i zamykającym &lt;ASP: ComboBox&gt; w widoku źródła. Strona z listą 1 zawiera zaktualizowane pole kombi, które ma listę elementów.

**Lista 1-statyczna. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Po otwarciu strony na liście 1 można wybrać jedną z istniejących opcji z pola kombi. Innymi słowy, ComboBox działa podobnie jak kontrolka DropDownList.

Istnieje również możliwość wprowadzenia nowego wyboru (na przykład Spicy), które nie znajduje się na istniejącej liście. Tak więc ComboBox działa również podobnie jak kontrolka TextBox.

Bez względu na to, czy wybierasz wstępnie istniejący element, czy wprowadzasz element niestandardowy, podczas przesyłania formularza wybór zostanie wyświetlony w kontrolce etykieta. Po przesłaniu formularza btnSubmit\_kliknij pozycję obsługa i aktualizuje etykietę (zobacz rysunek 4).

[![wyświetlania wybranego elementu](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Rysunek 04**: wyświetlanie wybranego elementu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image8.png))

Pole kombi obsługuje te same właściwości, co formant DropDownList do pobierania wybranego elementu po przesłaniu formularza:

- SelectedItem. Text — wyświetla wartość właściwości text wybranego elementu.
- SelectedItem. Value — wyświetla wartość właściwości wartość wybranego elementu lub wyświetla tekst w polu kombi.
- SelectedValue — taka sama jak SelectedItem. Value, z wyjątkiem tego, że ta właściwość umożliwia określenie domyślnego (początkowego) wybranego elementu.

W przypadku wpisania niestandardowego wyboru do pola kombi, wybór niestandardowy jest przypisany do właściwości SelectedItem. Text i SelectedItem. Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Wybieranie listy elementów z bazy danych

Możesz pobrać listę elementów wyświetlanych przez element ComboBox z bazy danych. Na przykład można powiązać ComboBox z kontrolką kontrolki SqlDataSource, kontrolką ObjectDataSource, formantem LinqDataSource lub obiektem EntityDataSource.

Załóżmy, że chcesz wyświetlić listę filmów w kontrolce ComboBox. Chcesz pobrać listę filmów z tabeli bazy danych filmów. Wykonaj następujące kroki:

1. Utwórz stronę o nazwie filmy. aspx
2. Dodaj kontrolkę ScriptManager do strony, przeciągając element ScriptManager z karty rozszerzenia AJAX w przyborniku na stronę.
3. Dodaj kontrolkę ComboBox do strony, przeciągając pole ComboBox na stronę.
4. W widok Projekt przesuń wskaźnik myszy nad kontrolkę ComboBox i wybierz opcję zadanie **Wybierz źródło danych** (patrz rysunek 5). Zostanie uruchomiony Kreator konfiguracji źródła danych.
5. W kroku **Wybierz źródło danych** wybierz opcję &lt;nowe źródło danych&gt;.
6. W kroku **Wybierz typ źródła danych** wybierz pozycję baza danych.
7. W kroku **Wybierz połączenie danych** wybierz swoją bazę danych (na przykład MoviesDB. mdf).
8. W kroku **Zapisz parametry połączenia do pliku konfiguracji aplikacji** wybierz opcję zapisania parametrów połączenia.
9. W kroku **Konfigurowanie instrukcji SELECT** wybierz tabelę bazy danych filmów i zaznacz wszystkie kolumny.
10. W kroku **zapytania testowego** kliknij przycisk Zakończ.
11. Wróć do kroku **Wybierz źródło danych** , wybierz kolumnę tytuł dla pola, które ma być wyświetlane, oraz kolumnę identyfikator dla pola dane (zobacz rysunek).
12. Kliknij przycisk OK, aby zamknąć kreatora.

[![Wybieranie źródła danych](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Ilustracja 05**. Wybieranie źródła danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image10.png))

[![Wybieranie pól tekstu i wartości danych](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Ilustracja 06**. Wybieranie pól tekstu i wartości danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image12.png))

Po wykonaniu powyższych czynności pole kombi jest powiązane z kontrolką kontrolki SqlDataSource, która reprezentuje filmy z tabeli bazy danych filmów. Źródło strony wygląda jak lista 2 (po wyczyszczeniu formatowania przez chwilę).

**Lista 2 — filmy. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Zauważ, że formant ComboBox ma właściwość DataSourceID, która wskazuje na kontrolkę kontrolki SqlDataSource. Po otwarciu strony w przeglądarce zostanie wyświetlona lista filmów z bazy danych (patrz rysunek 7). Możesz wybrać film z listy lub wprowadzić nowy film, wpisując go w polu kombi.

[![wyświetlania listy filmów](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Ilustracja 07**. Wyświetlanie listy filmów ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image14.png))

## <a name="setting-the-dropdownstyle"></a>Ustawianie listy rozwijanej

Aby zmienić zachowanie kontrolki ComboBox, można użyć właściwości menu rozwijanego ComboBox. Ta właściwość akceptuje możliwe wartości:

- Lista rozwijana — (wartość domyślna) pole kombi wyświetla listę rozwijaną po kliknięciu strzałki i można wprowadzić wartość niestandardową.
- Prosty — ComboBox Wyświetla listę rozwijaną automatycznie i można wprowadzić wartość niestandardową.
- DropDownList — pole kombi działa podobnie jak w przypadku kontrolki DropDownList.

Różne listy rozwijane i proste są wyświetlane, gdy zostanie wyświetlona lista elementów. W przypadku prostej lista jest wyświetlana natychmiast po przeniesieniu fokusu do pola kombi. W przypadku listy rozwijanej, należy kliknąć strzałkę, aby wyświetlić listę elementów.

Wartość DropDownList powoduje, że kontrolka ComboBox będzie działać tak samo jak standardowa kontrolka DropDownList. Istnieje jednak ważna różnica w tym miejscu. W starszych wersjach programu Internet Explorer jest wyświetlana kontrolka DropDownList z nieograniczoną indeksem z. dzięki temu kontrolka będzie wyświetlana przed każdą kontrolką umieszczoną przed nią. Ponieważ ComboBox renderuje tag HTML &lt;DIV&gt; zamiast HTML &lt;Wybierz tag&gt;, pole kombi prawidłowo przestrzega porządku osi z.

## <a name="setting-the-autocompletemode"></a>Ustawianie AutoCompleteMode

Aby określić, co się dzieje, gdy ktoś wpisze tekst do pola kombi, użyj właściwości ComboBox AutoCompleteMode. Ta właściwość akceptuje następujące możliwe wartości:

- Brak — (wartość domyślna) pole kombi nie zapewnia żadnego zachowania autouzupełniania.
- Sugeruj — pole ComboBox Wyświetla listę i wyróżnia pasujący element na liście (patrz rysunek 8).
- Append-ComboBox nie wyświetla listy i dołącza pasujący element z listy do wpisanych informacji (zobacz rysunek 9).
- SuggestAppend — ComboBox Wyświetla listę i dołącza pasujący element z listy do wpisanych informacji (Zobacz Rysunek 10).

[![ComboBox wykonuje sugestię](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Ilustracja 08**: element ComboBox wykonuje sugestię ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image16.png))

[Pole kombi ![dołącza pasujący tekst](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Ilustracja 09**. pole kombi dołącza pasujący tekst ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image18.png))

[![pole kombi sugeruje i dołącza](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Ilustracja 10**. pole kombi sugeruje i dołącza ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image20.png))

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób używania kontrolki ComboBox do wyświetlania stałego zestawu elementów. Formant ComboBox jest związany ze statycznym zestawem elementów i tabelą bazy danych. Na koniec dowiesz się, jak zmodyfikować zachowanie kontrolki ComboBox, ustawiając jej listę rozwijaną i właściwości AutoCompleteMode.

> [!div class="step-by-step"]
> [Dalej](how-do-i-use-the-combobox-control-vb.md)

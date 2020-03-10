---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Tworzenie spójnego układu w witrynach ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Aby zwiększyć efektywność tworzenia stron sieci Web dla swojej witryny, możesz utworzyć bloki zawartości (takie jak nagłówki i stopki) w witrynie sieci Web, a ty c...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 3f63ce68ae4c13970ac0df196167ace0b22b592c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627450"
---
# <a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Tworzenie spójnego układu w witrynach ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak można użyć stron układu w witrynie internetowej ASP.NET Web Pages (Razor) do tworzenia bloków zawartości (takich jak nagłówki i stopki) oraz do tworzenia spójnego wyglądu wszystkich stron w witrynie.
> 
> **Dowiesz się:** 
> 
> - Jak tworzyć bloki wielokrotnego użytku zawartości, takie jak nagłówki i stopki.
> - Jak utworzyć spójny wygląd wszystkich stron w witrynie przy użyciu układu.
> - Jak przekazać dane w czasie wykonywania do strony układu.
> 
> Są to funkcje ASP.NET wprowadzone w artykule:
> 
> - Bloki zawartości, które są plikami zawierającymi zawartość sformatowaną w formacie HTML do wstawienia na wielu stronach.
> - Strony układu, które są stronami zawierającymi zawartość sformatowaną w formacie HTML, które mogą być współużytkowane przez strony w witrynie sieci Web.
> - Metody `RenderPage`, `RenderBody`i `RenderSection`, które informują ASP.NET, gdzie należy wstawiać elementy strony.
> - Słownik `PageData`, który umożliwia udostępnianie danych między blokami zawartości i stronami układu.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

## <a name="about-layout-pages"></a>Strony układu — informacje

Wiele witryn sieci Web zawiera zawartość, która jest wyświetlana na każdej stronie, na przykład nagłówek i stopka, lub pole informujące użytkowników, że są zalogowani. ASP.NET umożliwia utworzenie oddzielnego pliku z blokiem zawartości, który może zawierać tekst, znaczniki i kod, podobnie jak zwykła Strona sieci Web. Następnie można wstawić blok zawartości na innych stronach w witrynie, w których mają być wyświetlane informacje. Dzięki temu nie trzeba kopiować i wklejać tej samej zawartości na każdej stronie. Tworzenie wspólnej zawartości, takiej jak to również ułatwia aktualizowanie lokacji. Jeśli trzeba zmienić zawartość, można po prostu zaktualizować pojedynczy plik, a zmiany zostaną odzwierciedlone wszędzie tam, gdzie zawartość została wstawiona.

Na poniższym diagramie przedstawiono sposób działania bloków zawartości. Gdy przeglądarka żąda strony z serwera sieci Web, ASP.NET wstawia bloki zawartości w punkcie, w którym Metoda `RenderPage` jest wywoływana na stronie głównej. Strona ukończona (scalona) jest następnie wysyłana do przeglądarki.

![Diagram koncepcyjny pokazujący, jak Metoda RenderPage wstawia przywoływaną stronę do bieżącej strony.](3-creating-a-consistent-look/_static/image1.jpg)

W tej procedurze utworzysz stronę odwołującą się do dwóch bloków zawartości (nagłówka i stopki), które znajdują się w oddzielnych plikach. Tych samych bloków zawartości można używać na dowolnej stronie w witrynie. Gdy skończysz, zobaczysz stronę podobną do tej:

![Zrzut ekranu przedstawiający stronę w przeglądarce, która wynika z uruchamiania strony zawierającej wywołania metody RenderPage.](3-creating-a-consistent-look/_static/image2.png)

1. W folderze głównym witryny sieci Web Utwórz plik o nazwie *index. cshtml*.
2. Zastąp istniejący znacznik następującym:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. W folderze głównym Utwórz folder o nazwie *Shared*.

    > [!NOTE]
    > Typowym sposobem przechowywania plików udostępnianych między stronami sieci Web w folderze o nazwie *Shared*.
4. W folderze *udostępnionym* Utwórz plik o nazwie *\_header. cshtml*.
5. Zastąp jakąkolwiek istniejącą zawartość następującym:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Należy zauważyć, że nazwa pliku jest *\_header. cshtml*z podkreśleniem (\_) jako prefiks. ASP.NET nie wyśle strony do przeglądarki, jeśli jej nazwa zaczyna się od znaku podkreślenia. Dzięki temu osoby nie żądają (przypadkowo lub w inny sposób) tych stron bezpośrednio. Dobrym pomysłem jest użycie podkreślenia do nazwy stron, w których znajdują się w nich bloki zawartości, ponieważ nie chcesz, aby użytkownicy mogli zażądać tych stron &#8212; , które mogą być wstawiane do innych stron.
6. W folderze *udostępnionym* Utwórz plik o nazwie *\_footer. cshtml* i Zastąp zawartość następującym:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Na stronie *index. cshtml* Dodaj dwa wywołania metody `RenderPage`, jak pokazano poniżej:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Pokazuje, jak wstawić blok zawartości do strony sieci Web. Należy wywołać metodę `RenderPage` i przekazać ją do nazwy pliku, którego zawartość ma zostać wstawiona w tym momencie. W tym miejscu wstawiasz zawartość pliku *nagłówkowego\_. cshtml* i *\_plików stopki. cshtml* , który jest plikiem *index. cshtml* .
8. Uruchom stronę *index. cshtml* w przeglądarce. (W programie WebMatrix w obszarze roboczym **pliki** kliknij plik prawym przyciskiem myszy, a następnie wybierz polecenie **Uruchom w przeglądarce**).
9. W przeglądarce Wyświetl źródło strony. (Na przykład w programie Internet Explorer kliknij prawym przyciskiem myszy stronę, a następnie kliknij pozycję **Wyświetl źródło**).

    Dzięki temu można zobaczyć adiustację strony sieci Web, która jest wysyłana do przeglądarki, która łączy znaczniki strony indeksu z blokami zawartości. Poniższy przykład pokazuje Źródło strony, które jest renderowane dla *index. cshtml*. Wywołania `RenderPage`, które zostały wstawione do pliku *index. cshtml* , zostały zastąpione rzeczywistą zawartością plików nagłówkowych i stopek.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Tworzenie spójnego wyglądu przy użyciu stron układu

Do tej pory widzisz, że można łatwo dołączać tę samą zawartość na wielu stronach. Bardziej strukturalną metodą tworzenia spójnego wyglądu witryny jest użycie stron układu. Strona układu definiuje strukturę strony sieci Web, ale nie zawiera żadnej rzeczywistej zawartości. Po utworzeniu strony układu można utworzyć strony sieci Web zawierające zawartość, a następnie połączyć je ze stroną układu. Po wyświetleniu tych stron zostaną one sformatowane zgodnie ze stroną układu. (W tym sensie Strona układu działa jako rodzaj szablonu dla zawartości zdefiniowanej na innych stronach).

Strona układu jest tak samo jak każda strona HTML, z tą różnicą, że zawiera wywołanie metody `RenderBody`. Pozycja metody `RenderBody` na stronie układ określa, gdzie znajdują się informacje ze strony zawartości.

Na poniższym diagramie przedstawiono sposób łączenia stron zawartości i stron układu w czasie wykonywania w celu utworzenia gotowej strony sieci Web. Przeglądarka żąda strony zawartości. Strona zawartość zawiera kod, który określa stronę układu do użycia dla struktury strony. Na stronie układ zawartość jest wstawiana w punkcie, w którym wywoływana jest metoda `RenderBody`. Bloki zawartości można także wstawiać do strony układ, wywołując metodę `RenderPage`, tak jak w poprzedniej sekcji. Gdy strona sieci Web zostanie ukończona, jest wysyłana do przeglądarki.

![Zrzut ekranu przedstawiający stronę w przeglądarce, która wynika z uruchamiania strony zawierającej wywołania metody RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

Poniższa procedura pokazuje, jak utworzyć stronę układu i połączyć strony z zawartością.

1. W folderze *udostępnionym* witryny sieci Web Utwórz plik o nazwie *\_Layout1. cshtml*.
2. Zastąp jakąkolwiek istniejącą zawartość następującym:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Aby wstawić bloki zawartości, użyj metody `RenderPage` na stronie układu. Strona układu może zawierać tylko jedno wywołanie metody `RenderBody`.
3. W folderze *udostępnionym* Utwórz plik o nazwie *\_Header2. cshtml* i Zastąp każdą istniejącą zawartość następującym:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. W folderze głównym Utwórz nowy folder i nadaj mu nazwę *Style*.
5. W folderze *Style* Utwórz plik o nazwie *site. css* i Dodaj następujące definicje stylów:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Te definicje stylów są tutaj tylko pokazujące, w jaki sposób arkusze stylów mogą być używane ze stronami układu. Jeśli chcesz, możesz zdefiniować własne style dla tych elementów.
6. W folderze głównym Utwórz plik o nazwie *Content1. cshtml* i Zastąp każdą istniejącą zawartość następującym:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    To jest strona, która będzie używać strony układu. Blok kodu w górnej części strony wskazuje stronę układu, która ma być używana do formatowania tej zawartości.
7. Uruchom *Content1. cshtml* w przeglądarce. Renderowane strony używa formatu i arkusza stylów zdefiniowanego w *\_Layout1. cshtml* i tekstu (zawartości) zdefiniowanego w *Content1. cshtml*.

    ![Image](3-creating-a-consistent-look/_static/image4.png)

    Możesz powtórzyć krok 6, aby utworzyć dodatkowe strony zawartości, które następnie mogą współużytkować tę samą stronę układu.

    > [!NOTE]
    > Możesz skonfigurować witrynę tak, aby można było automatycznie używać tej samej strony układu dla wszystkich stron zawartości w folderze. Aby uzyskać szczegółowe informacje, zobacz [Dostosowywanie zachowania całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Projektowanie stron układu z wieloma sekcjami zawartości

Strona zawartości może zawierać wiele sekcji, co jest przydatne, jeśli chcesz użyć układów mających wiele obszarów z wymienną zawartością. Na stronie zawartość nadaj każdej sekcji unikatową nazwę. (Sekcja domyślna pozostaje nienazwana). Na stronie układ Dodaj metodę `RenderBody`, aby określić, gdzie powinna zostać wyświetlona sekcja nienazwana (domyślna). Następnie należy dodać oddzielne metody `RenderSection`, aby wyrenderować nazwane sekcje pojedynczo.

Na poniższym diagramie przedstawiono sposób, w jaki ASP.NET obsługuje zawartość, która jest dzielona na wiele sekcji. Każda nazwana sekcja jest zawarta w bloku sekcji na stronie zawartości. (Są one nazwane `Header` i `List` w tym przykładzie). Struktura wstawia sekcję zawartości na stronę układ w punkcie, w którym wywoływana jest metoda `RenderSection`. Sekcja bez nazwy (domyślnie) jest wstawiana w punkcie, w którym wywoływana jest metoda `RenderBody`, jak pokazano wcześniej.

![Diagram koncepcyjny przedstawiający sposób wstawiania przez metodę RenderSection odwołań do bieżącej strony.](3-creating-a-consistent-look/_static/image5.jpg)

Ta procedura pokazuje, jak utworzyć stronę zawartości z wieloma sekcjami zawartości i jak renderować ją przy użyciu strony układu, która obsługuje wiele sekcji zawartości.

1. W folderze *udostępnionym* Utwórz plik o nazwie *\_Layout2. cshtml*.
2. Zastąp jakąkolwiek istniejącą zawartość następującym:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Użyj metody `RenderSection`, aby renderować zarówno nagłówek, jak i listę.
3. W folderze głównym Utwórz plik o nazwie *Content2. cshtml* i Zastąp każdą istniejącą zawartość następującym:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Ta strona zawartości zawiera blok kodu w górnej części strony. Każda nazwana sekcja jest zawarta w bloku sekcji. Pozostała część strony zawiera domyślną sekcję zawartości (bez nazwy).
4. Uruchom *Content2. cshtml* w przeglądarce.

    ![Zrzut ekranu przedstawiający stronę w przeglądarce, która wynika z uruchamiania strony zawierającej wywołania metody RenderSection.](3-creating-a-consistent-look/_static/image6.png)

## <a name="making-content-sections-optional"></a>Wprowadzanie opcjonalnych sekcji zawartości

Zwykle sekcje, które tworzysz na stronie zawartości, muszą pasować do sekcji, które są zdefiniowane na stronie układu. Błędy można uzyskać w następujących przypadku:

- Strona zawartość zawiera sekcję, która nie ma odpowiedniej sekcji na stronie układ.
- Strona układu zawiera sekcję, dla której nie ma zawartości.
- Strona układu zawiera wywołania metod, które próbują renderować tę samą sekcję więcej niż raz.

Można jednak zastąpić to zachowanie dla nazwanej sekcji, deklarując sekcję jako opcjonalną na stronie układ. Pozwala to definiować wiele stron zawartości, które mogą udostępniać stronę układu, ale mogą lub nie mieć zawartości dla określonej sekcji.

1. Otwórz *Content2. cshtml* i Usuń następującą sekcję:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Zapisz stronę, a następnie uruchom ją w przeglądarce. Zostanie wyświetlony komunikat o błędzie, ponieważ strona zawartości nie udostępnia zawartości dla sekcji zdefiniowanej na stronie układ, a mianowicie sekcji nagłówek.

    ![Zrzut ekranu pokazujący błąd występujący w przypadku uruchomienia strony wywołującej metodę RenderSection, ale nie podano odpowiedniej sekcji.](3-creating-a-consistent-look/_static/image7.png)
3. W folderze *udostępnionym* otwórz stronę *\_Layout2. cshtml* i Zastąp następujący wiersz:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    przy użyciu następującego kodu:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Alternatywnie, można zastąpić poprzedni wiersz kodu następującym blokiem kodu, który tworzy te same wyniki:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Uruchom ponownie stronę *Content2. cshtml* w przeglądarce. (Jeśli nadal masz otwartą Tę stronę w przeglądarce, możesz ją po prostu odświeżyć). Ta strona jest wyświetlana bez błędów, mimo że nie ma nagłówka.

## <a name="passing-data-to-layout-pages"></a>Przekazywanie danych do stron układu

Na stronie zawartości należy określić dane, które należy odwołać na stronie układu. Jeśli tak, musisz przekazać dane ze strony zawartości do strony układu. Na przykład może być konieczne wyświetlenie stanu logowania użytkownika lub wyświetlenie lub ukrycie obszarów zawartości na podstawie danych wejściowych użytkownika.

Aby przekazać dane ze strony zawartości do strony układu, można umieścić wartości we właściwości `PageData` na stronie zawartość. Właściwość `PageData` jest kolekcją par nazwa/wartość, które zawierają dane, które mają być przekazywane między stronami. Na stronie układ można odczytać wartości z właściwości `PageData`.

Oto inny diagram. Ten element pokazuje, w jaki sposób ASP.NET może używać właściwości `PageData` do przekazywania wartości ze strony zawartości do strony układu. Po rozpoczęciu tworzenia strony sieci Web ASP.NET tworzy kolekcję `PageData`. Na stronie zawartość można napisać kod, aby umieścić dane w kolekcji `PageData`. Do wartości w kolekcji `PageData` mogą być również dostępne inne sekcje na stronie zawartość lub przez dodatkowe bloki zawartości.

![Diagram koncepcyjny pokazujący, jak strona zawartości może wypełnić słownik PageData i przekazać te informacje do strony układu.](3-creating-a-consistent-look/_static/image8.jpg)

Poniższa procedura pokazuje, jak przekazać dane ze strony zawartości do strony układu. Po uruchomieniu strony wyświetla przycisk, który umożliwia użytkownikowi ukrycie lub pokazanie listy zdefiniowanej na stronie układ. Gdy użytkownicy kliknieją przycisk, ustawia wartość PRAWDA/FAŁSZ (wartość logiczna) we właściwości `PageData`. Strona układu odczytuje tę wartość, a jeśli jest fałszywa, ukrywa listę. Ta wartość jest również używana na stronie zawartość, aby określić, czy ma być wyświetlany przycisk **Ukryj listę** lub przycisk **Pokaż listę** .

![Image](3-creating-a-consistent-look/_static/image9.jpg)

1. W folderze głównym Utwórz plik o nazwie *Content3. cshtml* i Zastąp każdą istniejącą zawartość następującym:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Kod przechowuje dwie fragmenty danych we właściwości &#8212; `PageData` tytuł strony sieci Web i wartość PRAWDA lub FAŁSZ, aby określić, czy ma zostać wyświetlona lista.

    Zwróć uwagę, że ASP.NET umożliwia umieszczenie znacznika HTML na stronie warunkowo przy użyciu bloku kodu. Na przykład blok `if/else` w treści strony określa, który formularz ma być wyświetlany w zależności od tego, czy `PageData["ShowList"]` jest ustawiona na wartość true.
2. W folderze *udostępnionym* Utwórz plik o nazwie *\_Layout3. cshtml* i Zastąp każdą istniejącą zawartość następującym:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Strona układu zawiera wyrażenie w `<title>` elementu, który pobiera wartość tytułu z właściwości `PageData`. Używa również wartości `ShowList` właściwości `PageData`, aby określić, czy ma być wyświetlany blok zawartości listy.
3. W folderze *udostępnionym* Utwórz plik o nazwie *\_list. cshtml* i Zastąp każdą istniejącą zawartość następującym:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Uruchom stronę *Content3. cshtml* w przeglądarce. Zostanie wyświetlona strona z listą widoczną po lewej stronie i przycisk **Ukryj listę** u dołu.

    ![Zrzut ekranu przedstawiający stronę zawierającą listę i przycisk "Ukryj listę".](3-creating-a-consistent-look/_static/image10.png)
5. Kliknij przycisk **Ukryj listę**. Lista znika i przycisk zmieni się, aby **wyświetlić listę**.

    ![Zrzut ekranu przedstawiający stronę, która nie zawiera listy i przycisku "Pokaż listę".](3-creating-a-consistent-look/_static/image11.png)
6. Kliknij przycisk **Pokaż listę** i ponownie Wyświetl listę.

## <a name="additional-resources"></a>Dodatkowe materiały

[Dostosowywanie zachowania całej witryny dla stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)

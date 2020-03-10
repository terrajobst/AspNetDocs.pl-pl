---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Tworzenie aplikacji MVC 3 z użyciem Razor i niewygodnego języka JavaScript | Microsoft Docs
author: microsoft
description: Przykładowa aplikacja internetowa z listą użytkowników pokazuje, jak prosta służy do tworzenia aplikacji ASP.NET MVC 3 przy użyciu aparatu widoku Razor. Przykładowa aplikacja s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540986"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Tworzenie aplikacji MVC 3 ze składnią Razor i dyskretnym kodem JavaScript

przez [firmę Microsoft](https://github.com/microsoft)

> Przykładowa aplikacja internetowa z listą użytkowników pokazuje, jak prosta służy do tworzenia aplikacji ASP.NET MVC 3 przy użyciu aparatu widoku Razor. Przykładowa aplikacja pokazuje, jak używać nowego aparatu widoku Razor z ASP.NET MVC w wersji 3 i Visual Studio 2010 w celu utworzenia fikcyjnej witryny sieci Web z listą użytkowników, która obejmuje funkcje, takie jak tworzenie, wyświetlanie, edytowanie i usuwanie użytkowników.
> 
> W tym samouczku opisano kroki, które zostały wykonane w celu skompilowania przykładowej aplikacji ASP.NET MVC 3. Projekt programu Visual Studio z C# kodem źródłowym i VB można dołączyć do tego tematu: [Pobierz](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Jeśli masz pytania dotyczące tego samouczka, Opublikuj je na [forum MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Omówienie

Aplikacja, którą tworzysz, jest prostą witryną z listą użytkowników. Użytkownicy mogą wprowadzać, wyświetlać i aktualizować informacje o użytkowniku.

![Witryna Przykładowa](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

W [tym miejscu](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)możesz pobrać projekt C# VB i zakończony.

## <a name="creating-the-web-application"></a>Tworzenie aplikacji sieci Web

Aby rozpocząć pracę z samouczkiem, Otwórz program Visual Studio 2010 i Utwórz nowy projekt za pomocą szablonu *aplikacji sieci Web ASP.NET MVC 3* . Nadaj aplikacji nazwę &quot;Mvc3Razor&quot;.

[![nowy projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

W oknie dialogowym **Nowy projekt ASP.NET MVC 3** wybierz pozycję **aplikacja internetowa**, wybierz aparat widoku Razor, a następnie kliknij przycisk **OK**.

![Nowe okno dialogowe projektu ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

W tym samouczku nie będziesz używać dostawcy członkostwa ASP.NET, więc możesz usunąć wszystkie pliki skojarzone z logowaniem i członkostwem. W **Eksplorator rozwiązań**Usuń następujące pliki i katalogi:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (i wszystkie pliki w tym katalogu)

![Soln](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Edytuj plik <em>\_Layout. cshtml</em> i Zastąp znacznik wewnątrz elementu `<div>` o nazwie `logindisplay` komunikatem <em>&quot;</em>logowanie wyłączone&quot;. W poniższym przykładzie przedstawiono nowe oznakowanie:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Dodawanie modelu

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *modele* , wybierz polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.

![Nowa Klasa MDL użytkownika](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nadaj klasie nazwę `UserModel`. Zastąp zawartość pliku *UserModel* następującym kodem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

Klasa `UserModel` reprezentuje użytkowników. Każdy element członkowski klasy ma adnotację z [wymaganym](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybutem z przestrzeni nazw [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) . Atrybuty w przestrzeni nazw [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) zapewniają automatyczne sprawdzanie poprawności klienta i serwera dla aplikacji sieci Web.

Otwórz klasę `HomeController` i Dodaj dyrektywę `using`, aby można było uzyskać dostęp do klas `UserModel` i `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Zaraz po deklaracji `HomeController` Dodaj następujący komentarz i odwołanie do klasy `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

Klasa `Users` to uproszczony magazyn danych znajdujący się w pamięci, który będzie używany w tym samouczku. W prawdziwej aplikacji należy użyć bazy danych do przechowywania informacji o użytkownikach. W poniższym przykładzie przedstawiono kilka pierwszych wierszy pliku `HomeController`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Skompiluj aplikację, aby model użytkownika był dostępny dla Kreatora tworzenia szkieletów w następnym kroku.

## <a name="creating-the-default-view"></a>Tworzenie widoku domyślnego

Następnym krokiem jest dodanie metody akcji i widoku w celu wyświetlenia użytkowników.

Usuń istniejący plik *Views\Home\Index* . Zostanie utworzony nowy plik *indeksu* w celu wyświetlenia użytkowników.

W klasie `HomeController` Zastąp zawartość metody `Index` następującym kodem:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Kliknij prawym przyciskiem myszy wewnątrz metody `Index`, a następnie kliknij polecenie **Dodaj widok**.

![Dodaj widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Wybierz opcję **Utwórz widok z silną typem** . W obszarze **Klasa danych widoku**wybierz pozycję **Mvc3Razor. models. UserModel**. (Jeśli nie widzisz **Mvc3Razor. models. UserModel** w polu **klasy widoku** , należy skompilować projekt). Upewnij się, że aparat widoku jest ustawiony na **Razor**. Ustaw wartość **Wyświetl zawartość** na **listę** , a następnie kliknij przycisk **Dodaj**.

![Dodaj widok indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

Nowy widok automatycznie szkieletuje dane użytkownika, które są przesyłane do widoku `Index`. Przejrzyj nowo wygenerowany plik *Views\Home\Index* . Linki **Utwórz nowe**, **Edytuj**, **szczegóły**i **Usuń** nie działają, ale pozostała część strony jest funkcjonalna. Uruchom stronę. Zostanie wyświetlona lista użytkowników.

![Strona indeksu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Otwórz plik *index. cshtml* i zastąp `ActionLink` znacznikami do **edycji**, **szczegółów**i **usuwania** przy użyciu następującego kodu:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Nazwa użytkownika jest używana jako identyfikator, aby znaleźć wybrany rekord w łączach **Edytuj**, **szczegóły**i **Usuń** .

## <a name="creating-the-details-view"></a>Tworzenie widoku szczegółów

Następnym krokiem jest dodanie `Details` akcji i widoku w celu wyświetlenia szczegółów użytkownika.

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Dodaj następującą metodę `Details` do kontrolera macierzystego:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Kliknij prawym przyciskiem myszy wewnątrz metody `Details` a następnie wybierz polecenie <strong>Dodaj widok</strong>. Sprawdź, czy pole <strong>Klasa widoku</strong> zawiera <strong>Mvc3Razor. models. UserModel</strong><em>.</em> Ustaw wartość <strong>Wyświetl zawartość</strong> na <strong>szczegóły</strong> , a następnie kliknij przycisk <strong>Dodaj</strong>.

![Dodaj widok szczegółów](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Uruchom aplikację i wybierz łącze Szczegóły. Funkcja automatycznego tworzenia szkieletów pokazuje każdą właściwość w modelu.

![Szczegóły](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Tworzenie widoku edycji

Dodaj następującą metodę `Edit` do kontrolera macierzystego.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Dodaj widok jak w poprzednich krokach, ale ustaw **zawartość widoku** do **edycji**.

![Dodaj widok edycji](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Uruchom aplikację i edytuj pierwszą i nazwisko jednego z użytkowników. W przypadku naruszenia wszelkich `DataAnnotation` ograniczeń, które zostały zastosowane do klasy `UserModel`, podczas przesyłania formularza zostaną wyświetlone błędy sprawdzania poprawności, które są generowane przez kod serwera. Na przykład jeśli zmienisz imię &quot;Ann&quot; na &quot;&quot;, podczas przesyłania formularza zostanie wyświetlony następujący błąd:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

W tym samouczku potraktowano nazwę użytkownika jako klucz podstawowy. W związku z tym nie można zmienić właściwości Nazwa użytkownika. W pliku *Edit. cshtml* , tuż po instrukcji `Html.BeginForm`, ustaw wartość pola Nazwa użytkownika na ukryte. Powoduje to, że właściwość zostanie przekazana w modelu. Poniższy fragment kodu przedstawia położenie instrukcji `Hidden`:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Zastąp `TextBoxFor` i `ValidationMessageFor` znaczników dla nazwy użytkownika z wywołaniem `DisplayFor`. Metoda `DisplayFor` wyświetla właściwość jako element tylko do odczytu. W poniższym przykładzie pokazano ukończony znacznik. Pierwotne wywołania `TextBoxFor` i `ValidationMessageFor` są oznaczone jako komentarze BEGIN-Comment i End-comment (`@* *@`).

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Włączanie walidacji po stronie klienta

Aby włączyć weryfikację po stronie klienta w ASP.NET MVC 3, należy ustawić dwie flagi i należy dołączyć trzy pliki JavaScript.

Otwórz plik *Web. config* aplikacji. Sprawdź, czy `that ClientValidationEnabled` i `UnobtrusiveJavaScriptEnabled` są ustawione na wartość true w ustawieniach aplikacji. Następujący fragment z głównego pliku *Web. config* zawiera poprawne ustawienia:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Ustawienie `UnobtrusiveJavaScriptEnabled` na true włącza niezauważalną technologię AJAX i niewygodną weryfikację klienta. W przypadku korzystania z niedyskretnej weryfikacji reguły sprawdzania poprawności są włączone do atrybutów HTML5. Nazwy atrybutów HTML5 mogą zawierać tylko małe litery, cyfry i łączniki.

Ustawienie `ClientValidationEnabled` na true powoduje włączenie walidacji po stronie klienta. Ustawiając te klucze w pliku *Web. config* aplikacji, można włączyć sprawdzanie poprawności klienta i niewygodne środowisko JavaScript dla całej aplikacji. Możesz również włączyć lub wyłączyć te ustawienia w poszczególnych widokach lub w metodach kontrolera przy użyciu następującego kodu:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Musisz również dołączyć kilka plików JavaScript w renderowanym widoku. Łatwy sposób dołączania kodu JavaScript we wszystkich widokach polega na dodaniu ich do pliku *Views\Shared\\_Layout. cshtml* . Zastąp element `<head>` pliku *\_Layout. cshtml* następującym kodem:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Pierwsze dwa skrypty jQuery są obsługiwane przez Microsoft Ajax Content Delivery Network (CDN). Korzystając z zalet usługi Microsoft Ajax CDN, możesz znacząco poprawić wydajność aplikacji.

Uruchom aplikację i kliknij link edycji. Wyświetl źródło strony w przeglądarce. Źródło przeglądarki pokazuje wiele atrybutów formularza `data-val` (na potrzeby sprawdzania poprawności danych). Gdy jest włączone sprawdzanie poprawności klienta i niezauważalny kod JavaScript, pola wejściowe z regułą walidacji klienta zawierają atrybut `data-val="true"`, aby wyzwolić niezauważalną weryfikację klienta. Na przykład pole `City` w modelu zostało dekoracyjne przy użyciu [wymaganego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atrybutu, co spowoduje, że w kodzie HTML pokazano w następującym przykładzie:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Dla każdej reguły sprawdzania poprawności klienta dodawany jest atrybut, który ma `data-val-rulename="message"`formularza. Korzystając z przykładowego pola `City` pokazanego wcześniej, wymagana reguła sprawdzania poprawności klienta generuje atrybut `data-val-required` i komunikat &quot;pole Miasto jest wymagane&quot;. Uruchom aplikację, Edytuj jednego z użytkowników i usuń zaznaczenie pola `City`. Po wybraniu karty z pola zobaczysz komunikat o błędzie weryfikacji po stronie klienta.

![Miasto jest wymagane](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Podobnie dla każdego parametru w regule sprawdzania poprawności klienta dodawany jest atrybut, który ma `data-val-rulename-paramname=paramvalue`. Na przykład właściwość `FirstName` ma adnotację z atrybutem [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) i określa minimalną długość równą 3 i maksymalną 8. Reguła walidacji danych o nazwie `length` ma nazwę parametru `max` i wartość parametru 8. Poniżej przedstawiono kod HTML wygenerowany dla pola `FirstName` podczas edytowania jednego z użytkowników:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Aby uzyskać więcej informacji o niezauważalnej weryfikacji klienta, zobacz wpis [niezauważalna Weryfikacja klienta w ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) w blogu Brada Wilson.

> [!NOTE]
> W programie ASP.NET MVC 3 beta czasami trzeba przesłać formularz w celu uruchomienia walidacji po stronie klienta. Można to zmienić w przypadku ostatecznej wersji.

## <a name="creating-the-create-view"></a>Tworzenie widoku

Następnym krokiem jest dodanie `Create` akcji i widoku w celu umożliwienia użytkownikowi utworzenia nowego użytkownika. Dodaj następującą metodę `Create` do kontrolera macierzystego:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Dodaj widok jak w poprzednich krokach, ale ustaw **zawartość widoku** do **utworzenia**.

![Utwórz widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Uruchom aplikację, wybierz link **Utwórz** i Dodaj nowego użytkownika. Metoda `Create` automatycznie korzysta z funkcji weryfikacji po stronie klienta i po stronie serwera. Spróbuj wprowadzić nazwę użytkownika, która zawiera białe znaki, takie jak &quot;Ben X&quot;. Po wybraniu karty z pola Nazwa użytkownika zostanie wyświetlony komunikat o błędzie weryfikacji po stronie klienta (`White space is not allowed`).

## <a name="add-the-delete-method"></a>Dodaj metodę Delete

Aby ukończyć ten samouczek, Dodaj następującą metodę `Delete` do kontrolera macierzystego:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Dodaj widok `Delete` jak w poprzednich krokach ustawienia **Wyświetl zawartość** do **usunięcia**.

![Usuń widok](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Masz teraz prostą, ale w pełni funkcjonalną ASP.NET aplikację MVC 3 z walidacją.

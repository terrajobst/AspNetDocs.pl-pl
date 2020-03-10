---
uid: single-page-application/overview/templates/breezeknockout-template
title: Szablon Breeze/odcinania | Microsoft Docs
author: madskristensen
description: Szablon aplikacji Breeze/odcinania jednostronicowego
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 5bb9ee8f758a25afa6baf3ccbaf7d5864754c7df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558192"
---
# <a name="breezeknockout-template"></a>Szablon Breeze/Knockout

Autor [produktywność Madsa Kristensena](https://github.com/madskristensen)

> Breeze/odcinanie — szablon MVC został zapisany przez dzwonek
> 
> [Pobierz szablon MVC Breeze/odcinania](https://go.microsoft.com/fwlink/?LinkId=282649)

Wysłuchano "aplikacja jednostronicowa" (SPA) i zastanawiasz się, co to jest. Podczas gdy możesz się z nią zapoznać, zamiast tego zapoznaj się z Tobą. Ale kto ma czas na pobranie przykładu? Jeśli masz program Visual Studio, będziesz mieć przykład SPA i działać w ciągu mniej niż 60 sekund dzięki szablonowi ASP.NET MVC 4 "Breeze/odcinania aplikacji jednostronicowej".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Co to jest szablon SPA Breeze/odcinania?

Większość szablonów projektu generuje szkielet aplikacji. Należy umieścić miąższ na tych kości przez dodanie kodu i ostatecznie dostarczenie działającej aplikacji. Szablon SPA Breeze/odcinania jest inny. Generuje przykładową aplikację do badania. Przedstawiono w nim projekt aplikacji SPA i wiele technik tworzenia SPA.

Szablon Breeze/odcinania jest odmianą [szablonu KNOCKOUTJS Spa](../introduction/knockoutjs-template.md) zawartym w aktualizacji ASP.NET and Web Tools 2012,2. Szablon Breeze SPA generuje aplikację z tym samym interfejsem użytkownika, ale ma inną implementację, za pomocą Breeze do zarządzania danymi.

Szablon SPA KnockoutJS wykonuje żądania obsługi z niesformatowanym systemem jQuery AJAX, który jest odpowiedni dla prostej aplikacji. Jednak bardziej zaawansowane aplikacje mają bardziej wymagające wymagania dotyczące zarządzania danymi. Na przykład większość aplikacji:

- Zapytanie i ponowne wykonywanie zapytań na serwerze podczas rozszerzonej sesji użytkownika.
- Dodaj filtry zapytań, sortowanie i stronicowanie.
- Udostępnianie tych samych danych na wielu ekranach.
- Gromadz zmiany w wielu obiektach, a następnie Zapisz je jako pojedynczą transakcję.
- Sprawdź poprawność zmian na kliencie, aby użytkownik mógł poprawić błędy przed zatwierdzeniem zmian w bazie danych.

Biblioteka BreezeJS obsługuje te działania, zwalniając Cię do tworzenia logiki aplikacji i środowiska użytkownika.

[**Breeze**](http://www.breezejs.com/?utm_source=ms-spa) to Biblioteka open source służąca do tworzenia aplikacji w językach JavaScript i HTML, a także typy aplikacji, które zostały w pełni dostarczone jako autonomiczne aplikacje pulpitu.

Szablon Breeze/odcinania pomaga wykonać ten pierwszy kluczowy krok w kierunku bardziej niezawodnej infrastruktury zarządzania danymi. Generuje przykładową aplikację do zrobienia, która jest identyczna z szablonem SPA KnockoutJS. W tym miejscu zastępuje warstwę danych AJAX z Breeze, dzięki czemu można porównać dwa podejścia obok siebie. Oczywiście stanowią jedynie ułamek to potencjał aplikacji Breeze. Jednak zobaczysz, jak działa Breeze i ile jest niezbędny do dokonania tego przejścia.

Zacznijmy.

## <a name="create-a-breezeknockout-template-project"></a>Utwórz projekt szablonu Breeze/odcinania

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Szablon jest spakowany jako plik rozszerzenia programu Visual Studio (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**. Nazwij projekt, a następnie kliknij przycisk **OK**.

W kreatorze **nowego projektu** wybierz pozycję **Breeze separowania Spa**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Naciśnij klawisze CTRL + F5, aby skompilować i uruchomić aplikację bez debugowania, lub naciśnij klawisz F5, aby uruchomić polecenie z debugowaniem.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Po pierwszym uruchomieniu aplikacji zostanie wyświetlony ekran logowania. Kliknij link "Zarejestruj się" i przejdź do widoku nowej strony glides, gdzie można wprowadzić nazwę użytkownika i hasło. (Strony logowania i rejestracji są kompilowane przy użyciu ASP.NET MVC). Po przesłaniu formularza rejestracji serwer generuje TodoList z dwoma elementami dla Twojego konta. Następnie prezentuje je na żółtej notatce.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Teraz jesteś w trakcie SPA. Wszystko, co widzisz i w trakcie manipulowania zadaniami, jest renderowane i zarządzane na kliencie przy użyciu funkcji odcinania i Breeze. Eksploruj aplikację jako użytkownika... ale z oczami dla deweloperów. Użyj narzędzi deweloperskich w przeglądarce, aby przechwycić ruch sieciowy. (W programie Internet Explorer: naciśnij klawisz F12, wybierz kartę **Sieć** , a następnie kliknij przycisk **Rozpocznij przechwytywanie**). Teraz spróbuj wykonać następujące czynności:

- Dodaj nowy element do wykonania.
- Kliknij etykietę i edytuj tytuł elementu do zrobienia
- Zaznacz pole wyboru, aby oznaczyć element jako gotowy. Zauważ, że pole tekstowe jest wyłączone, więc tytuł nie jest już edytowalny.
- Kliknij znak "x" na prawo od etykiety. Element zniknie i zostanie usunięty z bazy danych.
- Wybierz inny element i wyczyść jego tytuł. Zostanie wyświetlony komunikat o błędzie weryfikacji, że tytuł jest wymagany. Po krótkim wstrzymaniu poprzedni tytuł zostanie przywrócony.
- Wpisz przesadnie długi tytuł. Zostanie wyświetlony inny błąd walidacji, że tytuł jest zbyt długi.
- Kliknij przycisk "Dodaj listę zadań do zrobienia". Zostanie wyświetlona nowa lista z lewej strony poprzedniej listy.
- Zagraj z TodoList tytułem, wyzwalając jego wymagane i prawidłowe ważności.
- Kliknij pole tekstowe tytuł, aby wyczyścić komunikat o błędzie.
- Kliknij "x" w okręgu w prawym górnym rogu, aby usunąć TodoList i jego wykonania.

Logika walidacji jest wykonywana po stronie klienta przez Breeze. Atrybuty walidacji klas modelu serwera są propagowane do klienta i wykonywane automatycznie przed skontaktowaniem się z serwerem.

Przejrzyj ruch sieciowy. Zwróć uwagę na to, że nie wykryto żadnych wywołań do serwera, gdy Breeze błąd. Każda prawidłowa zmiana spowodowała żądanie POST do "/api/Todo/SaveChanges". Breeze wiąże się ze zmianami i wysyła je razem jako pojedyncze żądanie do metody `SaveChanges` kontrolera interfejsu API sieci Web. To różni się od szablonu SPA KnockoutJS, który sprawia, że żądania PUT, POST i DELETE dla każdego elementu są indywidualnie.

## <a name="peek-inside"></a>Wgląd w wewnątrz

Ta aplikacja ma po stronie klienta i po stronie serwera. Stos po stronie klienta składa się z małego kodu HTML i kombinacji modułów JavaScript aplikacji (w folderze "aplikacja") oraz bibliotek języka JavaScript innych firm (w folderze "skrypty").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Jeśli zbadano szablon SPA KnockoutJS, powinien on wyglądać bardzo dobrze. Skup się na niebieskich polach. Architektura interfejsu użytkownika to model-View-ViewModel (MVVM), w którym widżety HTML widoku są czyste oddzielone od kodu prezentacji pomocniczej w modelu widoku. System powiązań danych (odcinanie w tym przypadku) koordynuje widok i model widoku, dzięki czemu każdy może wykonywać swoje zadania bez Intimate wiedzy o innych.

Model hermetyzuje dane do zrobienia. Jednostki w modelu są zbudowane przez Breeze z właściwościami odcinania, aby mogły być powiązane bezpośrednio z widżetami w widoku. Model widoku prosi kontekst danych o pozyskiwanie i zapisanie jednostek modelu. Kontekst danych deleguje większość pracy z Breeze.

Stos po stronie serwera składa się z kodu dewelopera i trzech zasad biblioteki .NET: Web API, Entity Framework i Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Podstawowa architektura jest taka sama jak szablon SPA KnockoutJS. Implementacja jest jednak znacznie prostsze: DTO zostały usunięte i większość Entity Framework szczegóły zostały delegowane do Breeze.NET.

## <a name="next-steps"></a>Następne kroki

Zalecamy zapoznanie się z kodem, zgodnie z [obszerną omówieniem](http://www.breezejs.com/spa-template?utm_source=ms-spa) stosów klienta i serwera w witrynie sieci Web Breeze.

Możesz spróbować odtworzyć przy użyciu zapytania po stronie klienta Breeze; Dodaj filtry i sortuje. Możesz dodać więcej właściwości modelu i więcej jednostek, aby lepiej zainteresować się kompleksowym programowaniem SPA. Jeśli masz pewność, że projektujesz, możesz oddzielić funkcje do wykonania i zastąpić je własnymi.

Wkrótce wszystko będzie gotowe do następnego dużego kroku: Dodawanie ekranów po stronie klienta i Nawigowanie między nimi. Ten szablon SPA zostanie pozostawiony i przejdziesz do bardziej kompletnego stosu SPA, na przykład [gorąca ręczników John Papa](https://github.com/johnpapa/HotTowel#readme "Gorąca ręczników"), który dodaje Durandal do kombinacji Breeze i odcinania.

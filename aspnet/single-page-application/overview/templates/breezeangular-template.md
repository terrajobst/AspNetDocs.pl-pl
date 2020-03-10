---
uid: single-page-application/overview/templates/breezeangular-template
title: Breeze/szablon kątowy | Microsoft Docs
author: madskristensen
description: Szablon aplikacji Breeze/jednostronicowej pojedynczej strony
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3e4e63d385a56d51d3d08696782b43d6228f6201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578541"
---
# <a name="breezeangular-template"></a>Szablon Breeze/Angular

Autor [produktywność Madsa Kristensena](https://github.com/madskristensen)

> Szablon MVC Breeze/kątowy został zapisany przez dzwonek
> 
> [Pobierz szablon Breeze/kątowy MVC](https://go.microsoft.com/fwlink/?LinkId=286437)

[AngularJS](http://angularjs.org) to Biblioteka open source z usługi Google do tworzenia aplikacji jednostronicowych (aplikacji jednostronicowych). Oferuje powiązanie danych, iniekcję zależności i zarządzanie ekranem. Połącz je z [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), inną biblioteką Open Source na potrzeby modelowania i zarządzania danymi oraz masz podstawowe składniki dla doskonałej aplikacji klienckiej HTML/JavaScript.

Szablon SPA Breeze/kątowy jest odmianą [szablonu KNOCKOUTJS Spa](../introduction/knockoutjs-template.md) zawartą w aktualizacji ASP.NET and Web Tools 2012,2. Jeśli masz program Visual Studio, będziesz mieć przykład SPA i działać w mniej niż 60 sekund.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Na zewnątrz aplikacja wygląda podobnie do szablonu SPA KnockoutJS. Ale jest zupełnie inny pod okapem. Szablon KnockoutJS używa odcinania dla powiązania danych i surowego AJAX do uzyskiwania dostępu do danych. Szablon Breeze/kątowy używa kątowych dla powiązań danych i Breeze na potrzeby dostępu do danych. Te biblioteki zapewniają dodatkowe możliwości, w tym Nawigacja i historię stron.

Oto strona informacje o aplikacji:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Na tej stronie jest wyświetlany uruchomiony dziennik zdarzeń podczas bieżącej sesji użytkownika, w tym:

- Stronicowania. Zanotuj Tworzenie kontrolera do wykonania w #2 i #7.
- Zapytania zdalne (#3) i zapytania lokalnej pamięci podręcznej (#7).
- Zapisywanie nowych (#5, #6) i zmodyfikowanych jednostek (#4).
- Zmiany zweryfikowane na kliencie (#9), dzięki czemu użytkownik może poprawić błędy przed zatwierdzeniem zmian w bazie danych.

W tym szablonie nie ma więcej informacji, w tym:

- Dynamiczne ładowanie szablonów widoków HTML.
- Niestandardowe powiązanie danych poprzez dyrektywy kątowe ".
- Modularność i wstrzyknięcie zależności.
- Filtry zapytań, sortowania, stronicowania, projekcje i dołączanie powiązanych jednostek.
- Udostępnianie danych na wielu ekranach.
- Zapisywanie wielu zmian jako pojedynczej transakcji.
- Reguły walidacji są automatycznie propagowane z serwera do klienta JavaScript.

Zacznijmy.

## <a name="create-a-breezeangular-template-project"></a>Utwórz projekt szablonu Breeze/kątowy

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Szablon jest spakowany jako plik rozszerzenia programu Visual Studio (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**. Nazwij projekt, a następnie kliknij przycisk **OK**.

W kreatorze **nowego projektu** wybierz pozycję **Breeze skośne Spa**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Naciśnij klawisze CTRL + F5, aby skompilować i uruchomić aplikację bez debugowania, lub naciśnij klawisz F5, aby uruchomić polecenie z debugowaniem.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Kliknij link "informacje" w prawym górnym rogu, aby wyświetlić dziennik tych działań.

Logika walidacji jest wykonywana po stronie klienta przez Breeze. Atrybuty walidacji klas modelu serwera są propagowane do klienta i wykonywane automatycznie przed skontaktowaniem się z serwerem.

Przejrzyj ruch sieciowy. Zwróć uwagę na to, że nie wykryto żadnych wywołań do serwera, gdy Breeze błąd. Każda prawidłowa zmiana spowodowała żądanie POST do "/api/Todo/SaveChanges". Breeze wiąże się ze zmianami i wysyła je razem jako pojedyncze żądanie do metody `SaveChanges` kontrolera interfejsu API sieci Web. To różni się od szablonu SPA KnockoutJS, który sprawia, że żądania PUT, POST i DELETE dla każdego elementu są indywidualnie.

Ponadto należy zauważyć, że podczas przełączania między TodoList i stronami nie ma ruchu sieciowego. Wynika to z faktu, że zapytanie zostało ograniczone do lokalnej pamięci podręcznej Breeze.

## <a name="peek-inside"></a>Wgląd w wewnątrz

Ta aplikacja ma po stronie klienta i po stronie serwera. Stos po stronie klienta składa się z małego kodu HTML i kombinacji modułów JavaScript aplikacji (w folderze "aplikacja") oraz bibliotek języka JavaScript innych firm (w folderze "skrypty").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Architektura interfejsu użytkownika oddziela widżety HTML widoków od kodu prezentacji pomocniczej na kontrolerach. System powiązań danych kątowych koordynuje widoki i kontrolery, dzięki czemu każdy może wykonywać swoje zadania bez Intimate wiedzy o innych.

Kontroler żąda kontekstu danych w celu uzyskania i zapisania jednostek modelu. Kontekst danych deleguje większość pracy z Breeze, która konstruuje obiekty modelu samośledzenia z wyników zapytania JSON.

Stos po stronie serwera składa się z kodu dewelopera i trzech zasad biblioteki .NET: Web API, Entity Framework i Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Podstawowa architektura jest taka sama jak szablon SPA KnockoutJS. Implementacja jest jednak znacznie prostsze: DTO zostały usunięte i większość Entity Framework szczegóły zostały delegowane do Breeze.NET.

## <a name="next-steps"></a>Następne kroki

Zalecamy zapoznanie się z kodem, zgodnie z [obszerną omówieniem](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) stosów klienta i serwera w witrynie sieci Web Breeze.

Możesz spróbować odtworzyć przy użyciu zapytania po stronie klienta Breeze; Dodaj filtry i sortuje. Możesz dodać więcej właściwości modelu i więcej jednostek, aby lepiej zainteresować się kompleksowym programowaniem SPA. Jeśli masz pewność, że projektujesz, możesz oddzielić funkcje do wykonania i zastąpić je własnymi.

Radosne kodowanie!
